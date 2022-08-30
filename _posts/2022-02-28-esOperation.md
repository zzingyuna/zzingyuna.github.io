---
layout: post
---

# Elasticsearch 운영 관리



search_type
내부적으로 질의를 처리하는 방식 선택
색인의 크기가 커져도 괜찮다면 퍼지/와일드카드/구문 쿼리 대신 엔그램과 싱글을 사용하면 검색이 빨라짐
스크립트 사용 보다 색인 시 새로운 필드를 추가하여 검색 속도 향상
스크립트가 자주 변경되지 않으면 elasticsearch 플러그인 형태로 네이티브 스크립트 작성
샤드별로 문서 빈도가 불균형하면 dfs_query_then_fetch 사용
많은 문서를 탐색해야 한다면 스캔 검색 사용
search_type 요청 매개변수를 추가하고 아래 값 중 하나를 선택
- query_then_fetch: 기본값, 다큐먼트를 정렬하고 순위를 매기고자 할때, 전체 샤드의 검색이 모두 수행된 후 결과 출력, 전체 취합된 결과를 size 매개변수에서 지정한 수만큼 출력
- query_and_fetch: 모든 샤드를 대상으로 병렬로 질의 수행, 샤드별로 검색되는대로 결과 출력, size * 샤드 갯수만큼 출력
- dfs_query_then_fetch: query_then_fetch와 같으나 정확한 스코어링을 위해 검색어들을 사전 처리, 분산된 키워드 빈도를 계산하는 작업 추가 단계 존재
- dfs_query_and_fetch: query_and_fetch와 같으나 정확한 스코어링을 위해 검색어들을 사전 처리, 분산된 키워드 빈도를 계산하는 작업 추가 단계 존재
- count: 질의와 일치하는 다큐먼트 수만 반환
- scan: 질의가 엄청난 결과를 반환하리라 기대할 경우에 사용



검색 수행 우선순위
질의를 수행할 샤드 제어, preference 요청 매개변수에 설정
_primary 주 샤드에서만 실행, 색인에서 최신 정보를 사용하길 원하지만 자료가 바로 복제되지 않을 경우 유용
_primary_first 주 샤드에서 실행
_local 요청을 전송받은 노드의 사용 가능한 샤드에서 실행
_only_node:node_id 노드 식별자로 지정한 노드에서 실행
_preter_node:node_id 노드 식별자로 지정한 노드에서 실행, 해당 노드가 사용 불가할 경우 다른 노드에서 실행
_shards:1,2 샤드 식별자로 지정한 샤드에서 검색 연산 수행





엘라스틱서치가 제공하는 가능성
Script: 실제 스크립트 코드 포함
Lang: 언어에 대한 정보, 생략시 MVEL 가정
Params: 매개변수와 값 포함

스크립트 수행 과정에서 사용 가능한 객체
_doc: 계산된 점수와 필드값을 사용해 찾은 현재 다큐먼트에 접근하게 만든다, 예) _doc.title.value
_source: 현재 다큐먼트의 원본과 여기에 정의된 값에 접근하게 만든다, 예) _source.title
_fields: 다큐먼트 필드값에 접근할 목적으로 사용한다, 예) _fields.title.value

필드 하나는 다음과 같이 색인에 저장

_source 다큐먼트의 일부
해석되지 않은 상태에서 저장된 값
토큰으로 해석되어 색인된 값



아파치 티카 Apache Tika
랭귀지 디텍션 Language detaction
분석기가 lang 필드에 기반해 선택되기를 원하면 엘라스틱서치에 존재하는 분석기 이름 중 하나를 lang필드 값에 추가해야한다



스크롤 API
스크롤 관련 정보와 결과에 대한 정보를 엘라스틱서치라 얼마나 오랫동안 보존해야 할지 제안하는 값

질의 전송 예
curl -XGET 'localhost:9200/library/_search?pretty&scroll=5m' -d '{ "query": { "match_all": {} } }'

엘라스틱서치는 종단점 _search/scroll 제공
scroll_id 를 사용해 다음 이어지는 페이지를 얻을 수 있다
해당 핸들은 일정 시간동안만 유효하므로 지정 시간이 지나면 무효화된 값이되어 오류 응답을 반환한다



스케일 아웃

클러스터에 노드 추가
노드 업그레이드
스케일링 전략

오버 샤딩: 미리 감안하여 색인에 대해 의도적으로 다수의 샤드 생성
데이터를 색인과 샤드에 분할하기
처리량 극대화 하기(예: 색인이 진행되는 동안 레플리카 수를 줄이고 색인 작업이 종료되면 레플리카 수를 늘린다)



할당 인식 Allocation Awareness
데이터의 복제본이 어디에 위치할지에 대한 인지 상태, 클러스터의 위상 topology 설계.

샤드 기반 할당: "cluster.routing.allocation.awareness.attributes:rack"로 설정, "node.rack:1" 메타데이터 키가 할당 인식 파라미터
강제 할당 인식: "cluster.routing.allocation.awareness.attributes:zone", "cluster.routing.allocation.force.zone.values:us-east,us-west"로 설정



스레드 풀 종류

bulk: 기본값은 가용 프로세서 수
index: 기본값은 가용 프로세서 수
search: 기본값은 가용 프로세서 수 * 3



I/O가 사용할 노드 수준 제한
indices.store.throttle.type: none(노드전체)/merge(머지할때제한)/all(모든 노드 모든 샤드에 대한 작업 제한 한계점 적용)
index.store.throttle.type: node(노드전체)/merge(머지할때제한)/all(모든 노드 모든 샤드에 대한 색인 작업 제한 한계점 적용)
노드 수준 설정의 경우 "indices.store.throttle.max_bytes_per_sec: 50mb" 사용(초당 메가바이트),
색인 수준 설정의 경우 index.store.throttle.max_bytes_per_sec 사용

저장소 플러그인: 아마존 S3, 하둡 HDFS



Merge
시간이 지나면서 작은 세그먼트를 모아 새로운 단편(fragment)에 쓰고 삭제한다, 단편화가 많으면 색인 성능은 떨어진다.
index.merge.scheduler.max_thread_count 로 머지 스케줄링 설정 가능

색인 축소
색인의 샤드 개수 축소
index.blocks.write: true
인덱스명/_shrink/reduceindex
index.blocks.write: false



_rollover
검사할 조건을 정의하고 elasticsearch에 남겨두면 새 색인을 롤링, alias를 이용해서 가상 색인만 참조하게 할 수 있다

suggestion 추천 기능
제안 기능은 제안자를 사용하여 제공된 텍스트를 기반으로 유사하게 보이는 용어를 제안합니다.

검색 GET Preference
검색을 실행할 샤드 복사본을 선택
https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-request-preference.html#search-request-preference

ctx.op 값 설정
ctx.op = "delete" 스크립트 실행 후 도큐먼트 삭제
ctx.op = "none" 도큐먼트는 색인 과정을 건너뛴다, 스크립트가 도큐먼트를 변경하지 않았다면 색인 재생성 오버헤드를 막기 위해 해당 값으로 설정하는 것이 좋다.
ctx._timestamp 레코드의 타임스탬프

_update
원본 도큐먼트가 없으면 doc_as_upsert로 upsert 제공
필드 삭제: "script": {"inline": "ctx._source.remove("필드명")"}
필드 추가: "script": {"inline": "ctx._source.필드명=필드내용"}

_mget
다중 get 호출

search_after
스크롤 결과를 빠르게 건너뛸 수 있는 기능

_delete_by_query
쿼리로 삭제

_update_by_query

span 쿼리

노드 오버헤드: 일부 노드는 너무 많은 샤드가 할당돼 전체 클러스터의 병목이 될 수 있다
노드 셧다운: 가득 찬 디스크, 하드웨어 장애 및 전원 문제와 같은 여러 가지 이유로 발생할 수 있다
샤드 재배치 문제나 변형: 일부 샤드는 온라인 상태를 얻을 수 없다
너무 큰 샤드: 샤드가 너무 큰 경우 대량의 루씬 세그먼트 병합으로 인해 색인 성능이 저하된다
빈 색인과 빈 샤드: 메모리와 자원을 낭비하지만 모든 샤드에는 많은 활성 스레드가 있기 때문에 사용되지 않는 색인과 샤드가 많은 경우 일반 클러스터 성능이 저하된다.



압축 해제 rpm, deb 설치
config/ /etc/elasticsearch/
logs/ /var/log/elasticsearch/



배열 필드
단일 필드를 다중 필드 설정으로 데이터 재색인 없이 업그레이드 할 수 있다
다중 필드를 단일로 변경할 수 없다
한 번 지정하면 매핑에서 서브 필드를 삭제할 수 없다

동시성 제어 방법

낙관적 잠금(Optimistic locking) : 병렬 작업 허용, 드물게 나타나는 충돌 추정
비관적 잠금(Pessimistic locking) : 충돌을 야기하는 작업을 막으면서 방지


retry_on_conflict 파라미터
어플리케이션 도움 없이 자동으로 일래스틱서치가 재시도

범위 검색을 사용할때 바이너리 매치("이 도큐먼트는 범위 내에 있다 또는 이 도큐먼트는 범위 내에 없다")를 가지는 범위 쿼리로 들어간 도큐먼트는
범위 쿼리가 되는데 필요하지 않기 때문에 쿼리로 만들지 필터로 만들지 확실치 않다면 필터로 만들자.


필드 데이터 캐시
elasticsearch가 색인 문서들에서 필드의 값을 저장하는 인 메모리 캐시이고 정렬, 스크립팅 혹은 필드들 안의 값들을 집계할 때 사용
데이터를 압축하는 방식으로 적재
필드 정렬이나 집합에 수행시 fielddata 매핑에 loading을 eager로 설정

필드 데이터 관리

메모리 제한: indices.fielddata.cache.size, indices.fielddata.cache.expire 설정
서킷 브레이커 circuit breaker에 필드 데이터 사용
doc values로 메모리값 무시: 색인 데이터와 함께 디스크에 저장하고 그 값을 사용, 매핑 속성에 "doc_values:true" 설정
TF-IDF(TF=term frequency / 단어빈도, IDF=inverse document frequency /역 문서 빈도)
하나의 문서에서 단어가 자주 반복된다면 관련성은 높아진다, 전체 문서에서 단어가 자주 반복된다면 관련성은 낮아진다

기본 점수 계산 함수
조합 인자 coordination factor: 얼마나 많은 문서에서 찾았는지, 얼마나 많은 단어를 찾았는지
질의 표준화 query normalization: 질의 결과 비교


부스팅(boosting)
문서의 관련성을 수정하는 절차. 문서를 색인할때 부스팅, 문서를 질의할때 부스팅이 있다
부스팅하는 값은 절대값이 아닌 상대값이다.




중요도 정의
입력 자료 필드의 중요도 정의
{
    "title": "The Complete Sherlock Holmes",
    "author": {
        "_value": "Arthur Conan Doyle",
        "_boost": 10.0
    },
    "year": 1936
}
매핑에서 중요도 정의
{
    "mappings": {
        "book": {
            "properties": {
                "title": { "type": "string" },
                "author": { "type": "string", "boost": 10.0 }
            }
        }
    }
}



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


샤드와 레플리카 수동으로 옮기기  
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




##### 샤드 파편화 조각모음
```
POST /hug_confluence/_forcemerge?max_num_segments=1
```

##### health 체크 timeout 옵션
```
curl --connect-timeout 1 -XGET "http://elastic:elasticpwd@localhost:9200/_cluster/health?pretty"
```




```
Use bulk requestsedit
Use multiple workers/threads to send data to Elasticsearch
Increase the refresh interval
Disable refresh and replicas for initial loads
Disable swapping
Give memory to the filesystem cache
Use auto-generated ids
Use faster hardware
Indexing buffer size
Disable_field_names
Additional optimizations
대량 요청 사용편집
여러 작업자/스레드를 사용하여 Elasticsearch에 데이터 보내기
새로 고침 간격 늘리기
초기 로드에 대해 새로 고침 및 복제본 비활성화
스와핑 비활성화
파일 시스템 캐시에 메모리 제공
자동 생성 ID 사용
더 빠른 하드웨어 사용
인덱싱 버퍼 크기
비활성화_필드_이름
추가 최적화


coordinate 노드 별도 구축하는 방법 확인
(연산에 대한 통제가 불가능한 상황이라면 시스템 전체의 장애를 방지하기 위해 coordinate 노드 별도 구축)
-> 로드 밸런스 역할, 클러스터 크기가 더 클 때 필요.
-> 서울반도체의 경우 현재 3개의 노드 사용중, coordinate 노드 추가를 위해 추가 서버 제공 필수
   data 노드는 coordinate 노드 역할을 수행하는데 coordinate노드로 지정(고정)을 하는 경우
   master 노드 1개, coordinate 노드 1개, data 노드가 1개로 1개의 data 노드에 과부화 가능성 높음
-> 설정값
node.master: false
node.data: false
node.ingest: false
node.ml: false
cluster.remote.connect: false
```



```
GET _cat/health?v
GET _cat/nodes?v

GET _cat/indices?v&s=index
GET _cat/shards?v&s=index&s=node


GET /_cat/thread_pool/write,flush,refresh?v=true&h=id,name,active,queue,rejected,completed&pretty&s=id


GET _cat/tasks?v&h=action,task_id,timestamp,running_time&s=id
GET _cat/tasks?v&detailed&h=action,task_id,running_time,description


POST /*/_forcemerge?max_num_segments=1
POST /*/_cache/clear



GET */_settings
PUT */_settings
{
  "index.translog.flush_threshold_size": "1024MB",
  "index.translog.flush_threshold_size" : null,
  "index.blocks.read_only": false,
  "index.blocks.write": null,
  "index.refresh_interval": "1s",
  "number_of_replicas": 0
}


PUT */_settings
{
	"index.search.slowlog.threshold.query.warn": "10s",
	"index.search.slowlog.threshold.query.info": "5s",
	"index.search.slowlog.threshold.query.debug": "2s",
	"index.search.slowlog.threshold.fetch.warn": "1s",
	"index.search.slowlog.threshold.fetch.info": "800ms",
	"index.search.slowlog.threshold.fetch.debug": "500ms"
}
```

