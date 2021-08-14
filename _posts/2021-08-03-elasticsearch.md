---
layout: post
---

# Elasticsearch

디렉터리 구조
bin: elasticsearch 인스턴스를 구동하고 플러그인 관리에 필요한 스크립트를 담은 디렉터리
config: 구성 파일이 위치한 디렉터리
lib: elasticsearch가 사용하는 라이브러리를 포함한 디렉터리

elasticsearch 시작 후 생성된 디렉터리 구조
data: elasticsearch가 사용할 모든 자료가 저장될 위치
logs: 이벤트와 오류에 대한 정보가 담긴 파일이 저장될 위치
plugins: 설치된 플러그인이 저장된 위치
work: elasticsearch가 사용하는 임시 파일이 저장될 위치


JVM(Java Virtual Machine) 힙 메모리 제약
ES_HEAP_SIZE 값을 1024보다 크게 설정, 전체 시스템 메모리의 50%가 넘지 않아야 한다.


클러스터 상태 점검
$ curl -XGET http://127.0.0.1:9200/_cluster/health?pretty

elasticsearch 중단
$ curl -XPOST http://127.0.0.1:9200/_cluster/nodes/_shutdown

단일 노드만 중단
$ curl -XPOST http://127.0.0.1:9200/_cluster/nodes/노드식별번호/_shutdown

노드 식별자 확인
$ curl -XPOST http://127.0.0.1:9200/_cluster/State/nodes/

신규 다큐먼트 생성
$ curl -XPUT http://127.0.0.1:9200/blog/article/1 -d {"title": "제목", "content": "내용"}

자동 식별자 생성, 다큐먼트 생성
$ curl -XPUT http://127.0.0.1:9200/blog/article/ -d {"title": "제목", "content": "내용"}

다큐먼트 인출
$ curl -XGET http://127.0.0.1:9200/blog/article/1

다큐먼트 갱신
$ curl -XPUT http://127.0.0.1:9200/blog/article/1/_update -d '{"script": "ctx._source.content = \"내용 수정\""}'

다큐먼트 갱신, 없는 필드값 추가
$ curl -XPOST http://127.0.0.1:9200/blog/article/1/_update -d '{\"creator\": \"User1\"}'

다큐먼트 삭제
$ curl -XDELETE http://127.0.0.1:9200/blog/article/1

검색 질의 기본
$ curl -XGET http://127.0.0.1:9200/blog/_search?

두가지 색인에 대한 검색 질의
$ curl -XGET http://127.0.0.1:9200/blog,sns/_search?

검색의 타입 지정에 대한 검색 질의
$ curl -XGET http://127.0.0.1:9200/blog/es/_search?

질의 텍스트가 분석되는 방법 확인 질의
$ curl -XGET 'http://127.0.0.1:9200/blog/_analyze?field=title' -d '검색단어'

여러개 매개변수로 질의
$ curl -XGET 'http://127.0.0.1:9200/blog/_search?q=pubished:2013&df=title&explain=true&default_operator=AND'
(리눅스 시스템에서는 shell이 &문자를 분석하기 때문에 '싱클 쿼테이션 필요)

q 매개변수에 필드 지시자가 없을 경우 df 매개변수로 기본 검색 필드를 지정가능  
analyzer 프로퍼티는 질의 분석에 사용될 분석기 이름 정의  
default_operator 프로퍼티는 질의에 사용될 기본 부울 연산자를 지정
explain 매개변수를 true로 지정하면 결과의 다큐먼트마다 질의 해설 정보를 추가  
다큐먼트 마다 색인이름, 타입이름, 다큐먼트 식별자, _source필드 반환

결과 정렬
$ curl -XGET 'http://127.0.0.1:9200/blog/_search?sort=published:desc'
published 필드를 내림차순으로 정렬한 결과 반환  

독자적인 정렬 방식을 사용하여 다큐먼트에 대한 점수를 추적할때 track_scores=true 옵션 추가  

검색 타임아웃 지정 timeout=5s

결과 윈도우 크기 지정 size=10&from=2 (2번째 결과부터 5개 반환)  

검색 타입 search_type 매개변수를 이용해 지정가능
(dfs_query_then_fetch, dfs_query_and_fetch, query_then_fetch, query_and_fetch, count, scan)  

확장된 키워드 소문자로 만들기
lowercase_expanded_term=true  

와일드카드와 접두어 분석 사용
analyze_wildcard=true  


루씬 질의 구문  
[https://lucene.apache.org/core/4_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html](https://lucene.apache.org/core/4_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)  

연산자와 증가값을 이용한 중요도 설정
title:book^4  


numeric_detection:true
문자열을 숫자로 인지하여 검색 가능


dynamic_date_formats
새 문자열 필드를 검사하여 내용이 에 지정된 날짜 패턴과 일치하는지 확인합니다


dynamic:false
자동 필드 추가 기능 off



타입 속성
문자열, 숫자, 날짜, 부울, 바이너리 설정 가능
index_name: 색인에 저장될 필드 이름, 정의하지 않으면 해당 필드를 정의한 객체 이름으로 설정된다

index: analyzed와 no 값으로 설정 가능, 문자열 필드일 경우 not_analyzed라고 설정 가능, analyzed 필드는 색인되므로 검색 가능한 필드, no로 설정하면 검색 불가능한 필드, not_analyed인 경우 필드는 색인되지만 분석되지는 않아 검색어가 완벽하게 일치해야 결과에 포함된다. index 프로퍼티를 no로 설정하면 해당 필드의 include_in_all 프로퍼티를 비활성화 한다

store: yes로 설정하면 필드의 원본 값을 색인에 기록, no는 기본값으로 원본 내용을 결과에 담아 반환하지 않는다. 색인은 되어 있으므로 이 필드를 대상으로 자료를 검색가능하다

boost: 기본값은 1, 속성값이 높을수록 해당 필드의 값이 중요하다

null_value: 필드가 색인된 다큐먼트의 일부로 포함되지않은 경우 색인에 기록될 값을 명세, 필드 생략이 기본 방식

copy_to: 모든 필드값을 복사할 필드를 명세

include_in_all: 대항 필드가 _all 필드에 반드시 포함되어야 하는지 명세


문자열 속성
term_vector: no(기본값), yes, with_offsets, with_positions, with_positions_offsets 라는 값 설정 가능, 루씬 키워드 백터 계산 방식 정의

omit_norms: 분석될 문자열 필드 false(기본값), 색인되지만 분석되지 않을 문자열 true

analyzer: 색인과 검색을 위해 사용될 분석기 이름 정의, 전역으로 정의된 분석기 이름 사용

index_analyzer: 색인을 위해 사용할 분석기 이름

search_analyzer: 특정 필드로 전송한 질의문자열의 일부를 처리하기 위해 사용할 분석기 이름 정의

norms.enabled: 특정 필드를 위해 norms를 메모리에 올릴지 여부, 기본 true=메모리에 올린다

norms.loading: eager는 해당 필드에 대한 norms가 항상 메모리에 올려져 있음, lazy는 필요할 때만 메모리에 올림

position_offset_gap: 기본값 0, 색인에서 이름이 동일한 필드의 인스턴스 사이에 떨어진 차이 명세

index_options: 키워드를 담은 구조체인 출현 목록을 위한 색인 옵션 정의, docs: 다큐먼트번호만 색인, freqs: 다큐먼트 번호와 키워드빈도 색인, positions: 다큐먼트 번호와 키워드빈도와 위치 색인(기본값)

ignore_above: 필드의 최대 크기를 글자 수로 정의, 크기가 더 큰 값이 들어올 경우 무시된다.



숫자 속성
byte, short, integer, long, float, double
precision_step: 필드값에 대해 생성될 키워드 수 명세, 값이 작을 수록 생성되는 키워드 수가 늘어난다, 값 당 키워드 수가 많을수록 색인 비용이 크지만 범위 질의는 빨라진다. 기본값은 4

ignore_malformed: 기본값 false, 형식이 엉망이인 값을 제외하려면 true로 설정


날짜 속성
기본적으로 UTC로 저장
format: 날짜 형식 명세, 기본값은 dateOptionalTime

precision_step: 필드값에 대해 생성될 키워드 수 명세, 값이 작을 수록 생성되는 키워드 수가 늘어난다, 값 당 키워드 수가 많을수록 색인 비용이 크지만 범위 질의는 빨라진다. 기본값은 4


복수 필드 설정
```
"name" : {
    "type": "string",
    "fields": {
        "subname": { "type": "string", "index": "not_analyed" }
    }
}

첫번째 필드는 name으로 두번째 필드는 name.subname으로 참조 가능.
```


IP 주소 타입 필드 설정
```
"address": { "type": "ip", "store": "yes"}
```

token_count 타입
필드에 제공된 텍스트를 저장하고 색인하는 대신 필드에 얼마나 많은 단어가 존재하는지에 대한 색인 정보를 저장하게 만든다.


분석기
standard: 유럽 언어에 맞춘 표준 분석기
simple: 문자를 기준으로 제공된 값을 분리, 소문자로 변환
whitespace: 여백 문자를 기준으로 제공된 값을 분리
stop: 제공된 불용어 집합에 기반해 자료를 필터링
keyword: 제공된 값을 그대로 전달하는 분석기, not_analyzed로 특정 필드를 명세하는 경우와 동일한 효과
pattern: 정규 표현식을 사용해 유연하게 텍스트를 분리
lanuage: 특정 언어를 위해 동작하게 설계된 분석기
snowball: standard와 유사하지만 어간 추출 알고리즘을 추가로 제공


"similarity": "BM25"
점수 지수를 계산하기위해 BM25를 사용하여 필드 정의


유사성 모델
Okapi BM25: 해당 질의에 대한 다큐먼트를 찾을 확률을 추정하는 확률론적 모델  
DFR(Divergence from randomness): 이름이 동일한 확률론적 모델에 기반, 자연어로 표현된 텍스트에 대해 잘 동작  
IB(Information-based): DFR과 유사  


출현 목록 형식  
색인 파일의 기록 방법을 변경  
default: 기본 형식, 동적으로 저장되는 필드와 키워드 벡터 압축을 제공  
pulsing: 고유 값 수가 큰 필드를 위해 출현 목록을 해석해 terms 배열에 넣는다, 해당 필드에 대한 질의 속도를 높일 수 있다  
direct: 읽기 연산 과정에서 키워드를 배열로 올리는 형식, 배열은 메모리에 압축 해제된 형식으로 존재, 성능은 높여주지만 메모리에 민감  
memory: FST(Finite State Transducers)라는 구조체를 사용해 키워드와 포스트 목록을 메모리로 읽는다.  
bloom_default: default 형식의 확장, 디스크에 쓰는 bloom filter 기능을 추가  
bloom_plusing: pulsing 형식의 확장, pulsing 형식의 특성에 bloom filter 기능 추가  
매핑: "postins_format": "pulsing"  

doc 값 형식 명세  
"doc_values_format": "default" 기본 doc 값, 성능과 메모리 사이의 균형을 잘 잡았다.  
"doc_values_format": "disk" 디스크에 자료를 저장, 패싯이나 정렬과 같은 연산 사용시 성능이 떨어진다.  
"doc_values_format": "memory" 메모리에 자료를 저장, 표준 역색인 필드와 비교해 정렬과 패싯 함수의 성능 비슷, 색인 새로고침 연산이 빠르고 급격히 변화하는 색인과 짧은 색인 새로고침에 대응 가능.  


자료 색인  
대량 요청 시 /_bulk 사용  
색인 이름과 함께 /index_name/_bulk로 사용  
타입과 색인 이름과 함께 /index_name/type_name/_bulk  

UDP(User Datagram Protocol) 대량 연산  
대량 연산보다 색인을 더 효율적이고 빠르게 하는 방법  

식별자 필드  
- _uid: 다큐먼트 유일한 식별자, 다큐먼트 식별자와 다큐먼트 타입 결합  
- _id: 색인 과정에서 설정된 실제 식별자 저장  

_type 필드  
색인되지만 분석되거나 저장되지 않음  
해당 필드를 저장할 경우 mappings 파일 변경 필요  
해당 필드는 색인 제외 가능하나 키워드 질의나 필터와 같은 몇 질의가 동작하지 않을 가능성 있음  

_all 필드  
검색의 편의를 위해 다른 포든 필드의 자료를 모아 단일 필드에 저장  
특정 필드가 포함되지 않게 설정하려면 include_all_property 사용  
해당 기능을 비활성화 하려면 mappings파일 수정  
프로퍼티: enabled, store, term_vector, analyzer, index_analyzer, search_analyzer  

_source 필드  
색인 과정에서 엘라스틱서치에 전송한 원본 json 다큐먼트 저장  
특정 필드를 저장하지 않은 경우 강조 기능을 위한 원본 자료로 사용  
프로퍼티: enabled, includes, excludes  

_index 필드  
다큐먼트가 저장될 색인에 대한 정보 저장  
색인을 매일 새로 만들고 alias를 사용할 경우 반환된 다큐먼트가 저장된 특정 색인에 관심이 있는 경우 유용  

_size 필드  
기본 비활성화, _source 필드의 원래 크기를 자동으로 색인하고 다큐먼트에 함께 저장하게 만든다.  

_timestamp 필드  
기본 비활성화, 다큐먼트가 색인된 시점을 저장  
자동으로 생성하는 대신 path를 이용해 다른 필드의 값을 사용 가능  

_ttl 필드  
Time To Live, 다큐먼트의 생명 주기, 생명 주기가 지나면 다큐먼트 자동 삭제  


세그먼트 병합  
루씬 라이브러리가 여러 세그먼트를 가져와 여기서 찾은 정보를 토대로 새로운 세그먼트를 생성하는 과정  
병합 과정이 끝나면 원본 세그먼트들은 디스크에서 삭제  


병합 정책  
index.merge.policy.type: tiered  
tiered: 기본 병합 정책, tier당 허용된 최대 세그먼트 수를 고려하면서 대략적으로 비슷한 크기의 세그먼트를 병합  
log_byte_size: 병합 지수값 보다 세그먼트 수가 커질 경우 하나로 합치는 과정을 반복하므로 점점 더 커진다  
log_doc: log_byte_size 와 유사하지만 색인에 존재하는 다큐먼트 수로 세그먼트 크기를 계산  

병합 스케줄러  
index.merge.scheduler.type: concurrent  
병렬 병합 스케줄러(concurrent): 기본 병합 과정, 독자적인 thread로 시작, 지정된 스레드 수만큼 동시에 병합 수행  
직렬 병합 스케줄러(serial): 호출한 스레드에서 병합 과정 수행 후 다음 스레드 진행  

병합 지수  
index.merge.policy.merge_factor: 10  
색인 과정에서 얼마나 자주 세그먼트를 병합할지 명세,  
기본값 10, 배치색인을 위해서는 더 큰 값을, 일반적인 색인 관리를 위해서는 더 낮은 값을 사용  

속도 조절  
indices.store.throttle.type: merge   
indices.store.throttle.max_byte_per_sec: 10mb #초당 처리 가능한 바이트 수    
none: 속도 조절을 켜지 않는다  
merge: 병합 과정에만 속도 조절이 영향을 미치게 정의한다  
all: 속도 조절이 모든 자료 저장 활동에 영향을 미치게 정의한다  


라우팅  
명시적인 라우팅값을 명세하기로 결정하면 색인과 검색 과정에서 지정하여 검색 동작 방식을 단순화 가능  
사용하기 위해 라우팅 필터 추가 필요 "?routing=값"  
검색시 "?routing=값1,값2&q=검색어"  


질의 프로퍼티  
from: 다큐먼트 시작 위치, 기본값은 0  
size: 질의 결과로 얻을 최대 다큐먼트수, 기본값은 10  


버전값 확인  
```
{
    "version": true,
    "query" : {
        ...
    }
}
```


최소 점수값 설정 후 점수보다 높은 다큐먼트만 결과 반환  
```
{
    "min_score": 0.75,
    "query" : {
        ...
    }
}
```


검색 결과에 포함할 필드 선택  
fields 배열을 정의하지 않으면 _source 필드를 사용,  
저장된 모든 필드를 반환하길 원한다면 *을 전달  
```
{
    "fields": ["title", "content"],
    "query" : {
        ...
    }
}
```


_source 필드에서 필드 선택하는 방법  
```
{
    "partial_fields": {
        "partiall": {
            "include": ["title"],
            "exclude": ["updator"]
        }
    },
    "query" : {
        ...
    }
}
```


스크립트가 평가하는 값 사용  
year 필드에서 1800을 뺀 값을 correctYear라는 필드로 반환  
```
{
    "script_fields": {
        "correctYear": {
            "script": "doc['year'].value - 1800"
        }
    },
    "query" : {
        ...
    }
}
```


스크립트 필드에 매개변수 전달   
```
{
    "script_fields": {
        "correctYear": {
            "script": "doc['year'].value - paramYear",
            "params": {
                "paramYear" : 1800
            }
        }
    },
    "query" : {
        ...
    }
}
```



검색 타입  
내부적으로 질의를 처리하는 방식 선택  
search_type 요청 매개변수를 추가하고 아래 값 중 하나를 선택  
query_then_fetch: 