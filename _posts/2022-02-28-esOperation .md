---
layout: post
---

# Elasticsearch 운영 관리



elasticsearch 중단  
$ curl -XPOST http://127.0.0.1:9200/_cluster/nodes/_shutdown

단일 노드만 중단  
$ curl -XPOST http://127.0.0.1:9200/_cluster/nodes/노드식별번호/_shutdown


롤링 리스타트  
노드를 업그레이드하거나 정적인 설정값을 바꾼 경우 데이터의 가용성을 해치지 않으며 클러스터를 재구동하기 위한 방법  
1. 클러스터의 allocation 비활성화
2. 업그레이드할 노드를 정지
3. 노드 업그레이드
4. 업그레이드된 노드 구동
5. 업그레이드된 노드가 클러스터에 추가되기를 기다림
6. 클러스터의 allocation 활성화
7. 클러스터가 그린 상태가 되기를 기다림

샤드 재분배 중지 설정
```
curl -XPUT 'localhost:9200/_cluster/settings' -d '{
    "transient": {
	    "cluster.routing.allocation.enable": "none"
	}
}'
```

샤드 재분배 활성화 설정
```
curl -XPUT 'localhost:9200/_cluster/settings' -d '{
    "transient": {
	    "cluster.routing.allocation.enable": "all"
	}
}'
```


* 샤드와 레플리카 수동으로 옮기기  
```
# 1.샤드 옮기기
curl -XPOST 'localhost:9200/_cluster/reroute' -d '{
    "commands": [
        {
            "move": {
                "index": "shop",
                "shard": 1,
                "from_node": "es_node_one",
                "to_node": "es_node_two"
            }
        }
    ]
}'

# 2.샤드 할당 취소
curl -XPOST 'localhost:9200/_cluster/reroute' -d '{
    "commands": [
        {
            "cancel": {
                "index": "shop",
                "shard": 0,
                "from_node": "es_node_one"
            }
        }
    ]
}'

# 3.샤드 강제 할당
curl -XPOST 'localhost:9200/_cluster/reroute' -d '{
    "commands": [
        {
            "allocate": {
                "index": "users",
                "shard": 0,
                "from_node": "es_node_two"
            }
        }
    ]
}'

# 단일 요청에 여러 명령으로
curl -XPOST 'localhost:9200/_cluster/reroute' -d '{
    "commands": [
        {
            "move": {
                "index": "shop",
                "shard": 1,
                "from_node": "es_node_one",
                "to_node": "es_node_two"
            }
        },
        {
            "cancel": {
                "index": "shop",
                "shard": 0,
                "from_node": "es_node_one"
            }
        }
    ]
}'
```



복구  
모든 샤드와 레플리카를 초기화하는 과정, 트랜잭션 로그에서 모든 자료를 읽어 샤드에 적용  
> gateway.expected_nodes: 10  
엘라스틱서치는 클러스터에 속한 노드 수가 10이 되면 즉시 복구 프로세스 시작  
> gateway.recover_after_nodes: 8  
프로퍼티에 설정된 값과 클러스테에 합류한 노드 수가 8이 된 직후 복구 시작  
> gateway.recover_after_time: 10m  
클러스터가 형성된 다음 10분 후 게이트웨이 복구  
> gateway.recover_after_master_nodes  
마스터로 승격이 가능한 자격을 갖춘 노드가 얼마나 많이 존재해야 복구 가능한지 명세  
> gateway.recover_after_data_nodes  
클러스터 자료 노드가 얼마나 많이 존재해야 복구 가능한지 명세  
> gateway.expected_master_nodes  
마스터로 승격이 가능한 자격을 갖춘 노드가 얼마나 많이 존재해야 복구 가능한지 명세  
> gateway.expected_data_nodes  
클러스터에 자료 노드가 얼마나 많이 존재해야 하는지 명세  


필터 캐시  
- 노드 필터 캐시
단일 노드에 할당된 모든 색인을 대상으로 공유  
indices.cache.filter.size로 주어진 전체 메모리 중 특정 비율이나 특정 양을 사용하게 구성  
- 색인 필터 캐시
최종 크기를 예측하기 어렵다  

indices.fielddata.cache.size  
필드 자료 캐시에 할당한 메모리양 설정, 절대값(2GB) 혹은 퍼센트(40%)로 설정가능  

indices.fielddata.cache.expire  
최대 비활성 시간 설정, 분 (10m)으로 설정 가능  

indices.fielddata.breaker.limit  
기본값 80%, 차단기 동작을 위한 프로퍼티, 필드 자료 회로 차단기 ciruit breaker로 프로세스에 할당한 힙 메모리 중 80퍼센트 이상을 요구하는 즉시 예외를 던진다  

indices.fielddata.breaker.overhead  
기본값 1.03, 차단기 동작을 위한 프로퍼티, 필드를 대상으로 원래 추정한 값에 보정을 위해 곱해야 할 상수값 정의  


플러시  
메모리 내 세그먼트를 디스크로 커밋, 트랜잭션 로그 삭제  
- 메모리 버퍼가 가득 찼을때 실행된다
- 마지막 플러시로부터 일정시간 지났을때 실행된다
- 트랜잭션 로그의 사이즈가 일정한 임계치를 넘었을때 실행된다

세그먼트 머지  
세그먼트 총 숫자 적절히 유지, 삭제된 문서 제거로 검색 성능 향상  
- index.merge.policy.segments_per_tier 하나의 계층에 가지고 있을 세그먼트 수
- index.merge.policy.max_merge_at_once 한번에 합쳐질 수 있는 세그먼트 수
- index.merge.policy.max_merged_segment 세그먼트 크기의 최대값
- index.merge.scheduler.max_thread_count 머지 작업에 사용될 수 있는 스레드 최대 개수

강제 머지 요청 -> 최적화  
갱신이 일어나지 않는 색인에서 효과적  
```
curl localhost:9200/인덱스명/_optimize?max_num_segments=1
# max_num_segments: 샤드별로 몇 개의 세그먼트로 병합되기를 원하는지
```

저장 제한 Store Throttling  
indices.store.throttle.max_bytes_per_sec 머지 작업이 사용할 수 있는 I/O 처리량 제한  

indices.store.throttle.type:none  
저장 제한 임계값 없애기  

저장 설정  
- MMapDirectory: 필요 파일을 가상 메모리에 매핑, 메모리에 파일 직접 접근, 파일 시스템 캐시 사용. 사용되지 않는 메모리를 디스크로 내리는 시스템 가상메모리 스왑과 유사, 크기가 크고 무작위로 접근되는 파일일때 유리 (index.store.type:mmapfs)
- NIOFSDirectory: 읽을 데이터를 JVM 힙 버퍼에 복제, 크기가 작고 연속적으로 접근되는 파일일때 유리 (index.store.type:niofs)

필터캐시  
필터의 결과 캐시
- indices.cache.filter.size: 캐시 크기 설정
- indices.cashe.filter.expire: 캐시 만료 시간 설정

캐시 불가 예  
- 범위 필터에서 now 적용시
- has_child, has_parent 필터는 _cache 플래스 사용불가

캐시 축출  
캐시가 가득차서 공간 확보를 위해 LRU 정책에 의거 사용된지 오래된 캐시 항목 제거시 발생  

비트셋  
크기가 작고 쉽게 만들 수 있다, 캐시를 생성하는 오버헤드가 매우 적다, 각 필터별로 저장된다, 다른 비트셋과 결합하여 사용 가능하다  
- 비트셋 사용 필터: term, terms, exists/missing, prefix
- 비트셋 미사용 필터: regexp, nested/has_child/has_parent, script, geofilters
- bool 필터에서 비트셋을 사용하는 필터들을, in/or/not 필터에서 비트셋을 사용하지 않는 필터들을 결합하여 사용하면 좋다

doc value  
색인 타입에 연산되어 색인과 함께 디스크 저장  


샤드 쿼리 캐시  
샤드가 잘 변하지 않고 많은 같은 요청을 반복적으로 수행하는 경우, 정적인 데이터에 대한 검색 요청 전체를 재사용하는 경우 캐시  
- index.cache.query.enable:true 색인 레벨에서 샤드 쿼리 캐시 활성화
- query_cache 파라미터: 쿼리에 대해 색인 레벨 설정
- indices.cache.query.size: 샤드 쿼리 캐시 크기 설정

cache.recycler.page.limit.heap 페이지 리사이클러  
집계에서 사용하는 큰 배열은 재사용할 수 있도록 여기에 배치됩니다. 기본값은 힙의 10%입니다.  



저장소 타입 설정  
> index.store.type: simplefs  
임의 접근 파일을 사용해 색인 파일에 접근하는 디스크 기반 저장소  
> index.store.type: niofs  
색인 파일에 접근하기 위해 자바 NIO 클래스를 사용하는 디스크 기반 색인 저장소  
> index.store.type: mmapfs  
mmap을 사용해 메모리에 색인 파일을 매핑하는 디스크 기반 저장소  
> index.store.type: memory  
색인을 RAM 메모리에 저장  


* 주의사항  
- 일반적으로 엘라스틱서치 운영 중인 jvm 프로세스에 전체 가용 메모리의 50~60 퍼센트를 넘어가는 메모리 할당 금지  
- jvm 힙 크기 재조정을 피하기 위해 Xmx, Xms 인수를 동일 값으로 설정하는 것이 바람직  
- 일반적으로 64비트 운영체제를 사용한다면 mmafs 선택, 유닉스 기반 시스템에서는 niofs를 윈도 기반 시스템에서는 simplefs 선택  
- 색인 새로고침 주기는 되도록 길게 설정  
- 스레드 풀 조율  
- 병합 과정 조율, 질의를 빠르게 하려면 색인에 대한 세그먼트 수를 줄여야 한다, 색인이 빨라지려면 색인에 대한 세그먼트 수를 늘려야 한다 양쪽의 절충 지점을 찾아 내는것이 중요  
- 필드 자료 캐시에 한계가 없어 메모리 부족 오류 문제 방지를 위해 필드 자료 캐시 크기를 제한하거나 회로 차단기를 사용해 예외를 던지도록 처리  
- 색인을 위한 RAM 버퍼는 10 퍼센트 정도를 기본으로 설정한다  
- 트랜잭션 로깅 조율, 트랜잭션 로그에 쌓인 정보를 처리하여 샤드 초기화가 느려지는 현상 개선  


트랜잭션 로깅 traslog  
샤드별 구조체로 로그 선행 기입 목적으로 사용된다  
최대 5,000개 연산을 200mb 크기까지 보존  
index.translog.flush_threshold_ops: 트랜잭션 로그에 저장될 최대 연산 수  
index.translog.flush_threshold_size: 트랜잭션 로그의 최대 크기  



데이터 백업  
- 스냅샷 API: 클러스터 상태, 데이터 복사, 논블로킹 작업
- 스냅샷 API를 이용해 공유 파일 시스템으로 백업

스냅샷  
생성한 시점의 해당 클러스터 관련 자료, 색인에대한 정보 보존  
name: 저장소 이름  
type: 저장소 타입(fs, url)  
settings: 타입을 fs로 설정했을 경우 location, url로 설정했을 경우 url 추가 정보 필요  

```
# 저장소 정의
curl -XPUT localhost:9200/_snapshot/저장소이름 -d '{
  "type": "fs",
  "settings": {
    "location": "저장소 네트워크상 위치", (ex: "/tmp/es_backup_folder/cluster1")
	"compress": true,
	"max_snapshot_bytes_per_sec": "20mb",
	"max_restore_bytes_per_sec": "20mb"
  }
}'

# 스냅샷 생성
curl -XPUT localhost:9200/_snapshot/저장소이름/스냅샷이름?wait_for_completion=true&pretty
스냅샷 생성 후 응답을 받기 위해 wait_for_completion 매개변수 사용

# 저장소 정의 확인
curl -XGET localhost:9200/_snapshot/저장소이름?pretty

# 모든 저장소 점검
curl -XGET localhost:9200/_snapshot/_all?pretty

#복원하기
curl -XPOST localhost:9200/_snapshot/저장소이름/스냅샷이름/_restore

#복원하기 (복원하기가 종료된 후까지 기다린 후 요청 반환)
curl -XPOST localhost:9200/_snapshot/저장소이름/스냅샷이름/_restore?wait_for_completion=true

#삭제하기
curl -XDELETE localhost:9200/_snapshot/저장소이름/스냅샷이름

#스냅샷 저장소 삭제
curl -XDELETE localhost:9200/_snapshot/저장소이름?pretty

```


스냅샷 생성시 매개변수  
- indices: 스냅샷을 찍길 원하는 색인 이름  
- ignore_unabailable: false로 설정하면 indices 매개변수가 존재하지 않는 색인을 가리키는 경우 명령 실패  
- include_global_state: 기본값 true, 클러스터 상태를 스냅샷에 기록  
- partial: true로 설정하면 사용 가능한 샤드 관련 내용만 저장 나머지는 생략  


스냅샷 복구  
> curl -XPOST 'localhost:9200/_snapshot/backup/bckp1/_restore'  -d '{ "indices": "c*" }'  
c 글자로 시작하는 색인만 복구  
다른 매개변수  
- ignore_unavailable: 스냅샷 생성과 동일  
- include_global_state: 스냅샷 생성과 동일  
- rename_pattern: 스냅샷에 저장된 색인 이름 변경, 정규표현식 값 사용  
- rename_replacement: rename_pattern과 함께 목표 색인 이름 정의  

