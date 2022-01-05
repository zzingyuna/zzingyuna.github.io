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
query_then_fetch: 기본값, 다큐먼트를 정렬하고 순위를 매기고자 할때  
query_and_fetch: 모든 샤드를 대상으로 병렬로 질의 수행  
dfs_query_and_fetch: query_and_fetc 와 비슷, 분산된 키워드 빈도를 계산하는 작업 추가 단계 존재  
dfs_query_then_fetch: query_then_fetch와 비슷, 분산된 키워드 빈도를 계산하는 작업 추가 단계 존재  
count: 질의와 일치하는 다큐먼트 수만 반환  
scan: 질의가 엄청난 결과를 반환하리라 기대할 경우에 사용  


검색 수행 우선순위  
질의를 수행할 샤드 제어, preference 요청 매개변수에 설정  
_primary  주 샤드에서만 실행, 색인에서 최신 정보를 사용하길 원하지만 자료가 바로 복제되지 않을 경우 유용  
_primary_first  주 샤드에서 실행  
_local  요청을 전송받은 노드의 사용 가능한 샤드에서 실행  
_only_node:node_id  노드 식별자로 지정한 노드에서 실행  
_preter_node:node_id  노드 식별자로 지정한 노드에서 실행, 해당 노드가 사용 불가할 경우 다른 노드에서 실행  
_shards:1,2  샤드 식별자로 지정한 샤드에서 검색 연산 수행  
사용자 정의값  


검색 샤드 API  
```
GET /*/_search_shards
{
    "query": {
        "match_all": {}
    }
}
```


term 질의  
대상 필드에 키워드가 존재하는 다큐먼트와 일치하는지만 판단, 정확하지만 분석되지 않은 키워드  
색인된 다큐먼트에 포함된 키워드와 정확히 일치, boost 속성 포함 가능  


terms 질의  
내용에 특정 키워드가 여러 개 들어 있는 다큐먼트와 일치하는지 판단  
분석되지 않은 여러 키워드와 일치하는 결과 제공  
minimum_match 프로퍼티로 일치하는 개수 설정 가능  


match_all 질의  
색인에 존재하는 모든 다큐먼트와 일치하는 결과 제공, boost 속성 포함 가능  


common 키워드 질의  
불용어를 사용하지 않을 때 흔히 사용하는 단어에 대한 질의 관련성와 정확성 개선을 위해 사용  
질의를 두 그룹으로 나눠 첫번째 그룹은 중요한 키워드로 구성, 수행 후 두번 째 그룹에 대해 질의 수행  
사용 매개변수  
query: 실재 질의내용 정의  
cutoff_frequency: 높은 빈도 그룹과 낮은 빈도 그룹의 기준인 퍼센트 값 지정  
low_freq_operator: or나 and로 설정 가능, 낮은 빈도 키워드 그룹에서 질의 구성하기 위해 부울 연산자 명세, 모든 키워드가 존재하기를 원한다면 and로 설정  
high_freq_operator: or나 and로 설정 가능, 높은 빈도 키워드 그룹에서 질의 구성하기 위해 부울 연산자 명세, 모든 키워드가 존재하기를 원한다면 and로 설정  
minimum_should_match: 일치하기 위해 다큐먼트에 존재해야 할 최소 키워드 수 명세  
boost: 다큐먼트 점수에 반영할 중요도 정의  
analyzer: 질의 텍스트를 분석하기 위해 사용할 분석기 이름과 기본 분석기 정의  
disable_coord: 기본값 false, 다큐먼트가 포함하는 모든 질의 키워드의 비율에 기반한 점수 지수 계산 활성화 여부  



match 질의  
질의 매개변수로 넘긴 값을 받아 분석한 다음 적절한 질의 생성  
루씬 질의 구문을 지원하지않는다  


부울 match 질의  
제공된 텍스트를 분석하고 bool 질의를 생성  
operator: or, and로 설정, 기본값 or  
analyzer: 질의 텍스트를 분석하기 위해 사용할 분석기명 지정  
fuzziness: 퍼지fuzzy 질의 생성  
prefix_length: 퍼지 질의 동작 방식 제어  
max_expansions: 퍼지 질의 동작 방식 제어
zero_terms_query: 분석기가 모든 키워드를 제거할 때 질의 동작 방식 명세, none 기본값  
cutoff_frequency: 질의를 높은 빈도 키워드와 낮은 빈도 키워드라는 두 그룹으로 나눈다  
```
{
    "query": {
        "match": {
            "title" : {
                "query": "crime and punishment",
                "operator": "and"
            }
        }
    }
}
```



match_phrase 질의  
분석된 텍스트에서 구phrase 질의를 생성  
slop: 구를 판별하기 위해 키워드 사이에 알려져 있지 않은 단어가 얼마나 많이 들어갈 수 있는지를 정의하는 정수값  
analyzer: 질의 텍스트를 분석하기 위해 사용할 분석기 이름과 기본 분석기를 정의  
```
{
    "query": {
        "match_phrase": {
            "title" : {
                "query": "crime and punishment",
                "slop": 1
            }
        }
    }
}
```


multi_match 질의  
fields 매개변수에 담긴 여러 필드를 대상으로 수행  
```
{
    "query": {
        "multi_match": {
            "query": "crime and punishment",
            "fields": ["title", "content"]
        }
    }
}
```
use_dis_max: 부울값 정의, 기본값 true, dismax 질의나 bool 질의를 설정  
tie_breaker: use_dis_max 매개변수를 true로 설정한 경우에만 사용, 최저 점수 질의 항목과 최대 점수 질의 항목 사이의 균형을 명세  


query_string 질의  
아파치 루씬 질의 구문 지원  
```
{
    "query": {
        "query_string": {
            "query": "title:crime^10 +title:punishment -otitle:cat +author:(+Fyodor +dostoevsky)",
            "default_field": "title"
        }
    }
}
```
query: 질의 텍스트 명세  
default_field: 질의 실행을 위한 기본 필드 명세  
default_operator: 기본값 or, 기본 논리 연산자 명세  
analyzer: 질의를 분석하기 위해 사용할 분석기의 이름 명세  
allow_leading_wildcard: 와일드카드 문자를 키워드의 첫 글자로 허용할지 말지 명세, 기본값 true  
lowercase_expand_terms: 질의 재작성 결과로 만들어져야 하는 키워드가 소문자인지 여부 명세, 기본값 true  
enable_position_increments: 결과 질의에서 위치 증가 기능을 켤지 여부 명세, 기본값 true  
fuzzy_max_expansions: fuzzy 질의를 확장하는 최대 키워드 수 명세, 기본값 50  
fuzzy_prefix_length: 생성된 fuzzy 질의 접두어 길이를 명세, 기본값 0  
fuzzy_min_sim: fuzzy 질의에 대한 최소 유사성 명세, 기본값 0.5  
phrase_slop: 구에 대한 slop값 명세, 기본값 0  
boost: 중요도 값, 기본값 1.0  
analyze_wildcard: wildcard 질의가 생성한 키워드를 분석할지 여부, 기본값 false  
auto_generate_phrase_queries: 질의에서 자동으로 구 질의를 생성할지 여부, 기본값 false  
minimum_should_match: 다큐먼트와 일치한다고 가정하기 위해 충족해야 하는 절의 최소 부울 조건, 퍼센트가 정수값 사용  
lenient: 형식이 잘못된 오류 무시여부  

DisMax는 Disjunction Max 의 줄임말  
분리 Disjunction는 여러 필드에 대해 검색이 실행되며 필드마다 중요도 가중치를 다르게 줄 수 있다  
최대는 해당 키워드에 대한 최고 점수만 마지막 다큐먼트 점수에 포함되며 일치하는 키워드를 담은 모든 필드의 점수를 합치지 않는다  



여러 필드에 대한 query_string 질의 수행  
bool 질의를 사용해 질의를 생성하는 기본방법, dismax 질의를 사용하는 방법 존재  
dismax 질의를 사용하려면 질의 내용에 use_dis_max 프로퍼티를 true로 설정해야 함  
```
{
    "query": {
        "query_string": {
            "query": "crime punishment",
            "fields": ["title", "content"],
            "use_dis_max": true
        }
    }
}
```


simple_query_string 질의  
오류가 발생해도 예외를 던지지 않는다  
```
{
    "query": {
        "simple_query_string": {
            "query": "title:crime^10 +title:punishment -otitle:cat +author:(+Fyodor +dostoevsky)",
            "default_operator": "and"
        }
    }
}
```


ids 질의  
식별자를 기준으로 반환된 다큐먼트를 필터링  
"type": "book" 과 같이 특정 타입에 대해서만 검색 가능  
```
{
    "query": {
        "ids": {
            "type": "book",
            "values": ["10", "15", "20"]
        }
    }
}
```


prefix 질의  
해당 접두어로 시작하는 값이 특정 필드에 포함한 다큐먼트와 일치하는지 판단  
boost 속성 포함 가능  
```
{
    "query": {
        "prefix": {
            "title": "cri"
        }
    }
}
```



fuzzy_like_this 질의  
more_like_this 질의와 유사, 퍼지 문자열을 사용, 키워드를 생성한 다음 최고로 차별화된 키워드를 고른다  
```
{
    "query": {
        "fuzzy_like_this": {
            "fields": ["title", "content"],
            "like_text": "crime punishment"
        }
    }
}
```
fields: 질의를 수행할 대상 필드  
like_text: 타큐먼트와 비교할 텍스트  
ignore_tf: 유사성 계산에서 키워드 빈도를 무시할지 여부, 기본값 false  
max_query_terms: 생성된 질의에 포함될 최대 질의 키워드 수 명세, 기본값 25  
min_similarity: 차별화된 키워드의 최소 유사성 명세, 기본값 0.5  
prefix_length: 차별화된 키워드의 공통 접두어 길이 명세, 기본값 0  
boost: 질의 중요도, 기본값 1.0  
analyzer: 제공된 질의를 분석하기 위해 사용할 분석기 이름  


fuzzy_like_this_field 질의  
fuzzy_like_this와 비슷, 단일 필드를 대상으로 동작
```
{
    "query": {
        "fuzzy_like_this_field": {
            "title": {
                "like_text": "crime punishment"
            }
        }
    }
}
```


fuzzy 질의  
에디트 거리edit distance 알고리즘 기반, 검색된 다큐먼트를 대상으로 질의에 넘긴 키워드를 기준으로 삼아 에디트 거리를 계산  
철자 오류에도 불구하고 관심이 있는 다큐먼트를 찾는데 성공  
value: 실제 질의  
boost: 중요도 값, 기본값 1.0  
min_similarity: 일치한다고 판단하기위한 키워드의 최소 유사성  
prefix_length: 공통 접두어 길이, 기본값 0  
max_expansions: 질의에서 확장될 최대 키워드 수 명세, 기본값 unbounded  



wildcard 질의  
*나 ? 같은 와일드 카드 사용 허용  
boost 속성 포함  



more_like_this 질의  
제공된 텍스트와 유사한 다큐먼트 조회  
fields: 질의 대상 필드  
like_text: 다큐먼트와 비교할 텍스트  
percent_terms_to_match: 질의에 속한 키워드의 퍼센트, 기본값 0.3  
min_term_freq: 이 값보다 작을 경우 키워드를 무시할 기준이 되는 빈도, 기본값 2  
max_query_terms: 최대 질의 키워드 수 명세, 기본값 25  
stop_words: 무시할 단어의 배열  
min_doc_freq: 최소한 다큐먼트에 존재해야할 키워드 수, 기본값 5  
max_doc_freq: 최대한 다큐먼트에 존재 가능한 키워드 수, 기본값 unbounded  
min_word_len: 무시하지 않기 위한 단일 단어의 최소 길이, 기본값 0  
max_word_len: 무시하지 않기 위한 단일 단어의 최대 길이, 기본값 unbounted  
boost_terms: 개별 항목의 중요도 값, 기본값 1  
boost: 중요도 값, 기본값 1.0  
analyzer: 질의 분석기 이름  


more_like_this_field 질의  
more_like_this 질의와 유사, 단일 필드에 대해서만 동작  
```
{
    "query": {
        "more_like_this_field" : {
            "title": {
                "like_text": "crime and punishement",
                "min_term_freq": 1,
                "min_doc_"freq": 1
            }
        }
    }
}
```


range 질의  
특정 범위에 속한 필드값을 포함한 다큐먼트를 찾는다  
단일 필드에서만 사용 가능  
gte: 지정한 값보다 크거나 같은 값인지 판단  
gt: 지정한 값보다 큰 값인지 판단  
lte: 지정한 값보다 작거나 같은 값인지 판단  
lt: 지정한 값보다 작은 값인지 판단  
```
{
    "query": {
        "range" : {
            "year": {
                "gte": 1700,
                "lte": 1900
            }
        }
    }
}
```


dismax 질의  
모든 하위 질의가 반환한 다큐먼트를 결합해결과를 이를 반환할 경우 유용  
점수가 낮은 하위 질의가 다큐먼트의 최종 점수에 영향을 미치는 방법을 통제할 수 있다  


regexp 질의  
정규표현식을 사용  


---------------------------------------------------------
복합 질의  


bool 질의  
should: 조건에 따라 일치할 수도 일치하지 않을 수도 있다  
must: 반드시 일치해야 한다  
must_not: 반드시 일치하지 않아야 한다  
boost: 중요도 값, 기본값 1.0  
minimum_should_match: 충족해야 하는 should 절의 최소 부울 조건 제어, 퍼센트나 정수값 사용  
disable_coord: 기본값 false, 점수 지수 계산을 활성화/비활성화, 정확도는 떨어지나 조금 빠른 질의를 위해서는 true로 설정  


boosting 질의  
질의 중 하나가 반환하는 다큐먼트 점수를 낮춘다  
positive 절은 다큐먼트 점수가 변경되지 않고 남을 질의  
negative 절은 다큐먼트 점수를 낮춰야 할 질의  
negative_boost 절은 negative 절의 질의 점수를 낮추기 위한 중요도 값  


constant_score 질의  
다른 질의를 감싸며 감싼 질의가 반환하는 다큐먼트마다 상수 점수를 반환  


indices 질의  
여러 색인에 다른 질의를 수행할 때 사용  
색인 배열과 질의 두가지 값 필요  


결과 필터링  
post_filter대신 filtered 질의를 사용하는 것이 훨씬 더 빠르다  


필터 사용  
```
{
    "query": {
        "match" : {
            "title": "test"
        }
    },
    "post_filter" :{
        "term": { "year": 1961 }
    }
}
```


필터 타입  
range 필터  
```
{
    "post_filter" :{
        "range": {
            "year": {
                "gte": 1930,
                "lte": 1990
            }
        }
    }
}
```

exists 필터  
지정 필드의 값이 없는 다큐먼트 제외  
```
{
    "post_filter" :{
        "exists": {
            "field": "year"
        }
    }
}
```

missing 필터  
지정 필드의 값이 있는 다큐먼트 제외  
```
{
    "post_filter" :{
        "missing": {
            "field": "year",
            "null_value": 0,
            "existence": true
        }
    }
}
```

script 필터  
계산된 값으로 필터링 할 경우  
```
{
    "post_filter" :{
        "script": {
            "script": "now - doc['year'].value > 100",
            "params": {
                "now": 2012
            }
        }
    }
}
```

type 필터  
```
{
    "post_filter" :{
        "type": {
            "value": "book",
        }
    }
}
```

limit 필터  
단일 샤드에서 반환된 다큐먼트 수를 제한  
```
{
    "post_filter" :{
        "limit": {
            "value": 1,
        }
    }
}
```

ids 필터  
```
{
    "post_filter" :{
        "ids": {
            "type": ["book"],
            "values": [1]
        }
    }
}
```

multi_match 필터  
```
{
    "post_filter" :{
        "query": {
            "multi_match": {
                "query": "novel erich",
                "fields": ["tags", "authoer"]
            }
        }
    }
}
```


필터 캐시  
결과를 캐시하는 필터: exists, missing, range, term, terms  


강조  
필드의 원본 내용이 존재하여야 한다  
FastVectorHighlighter, PostingsHighlighter 있다  
term_vector프로퍼티가 with_positions_offsets로 설정된 필드라면 FastVectorHighlighter 사용할 것이다  
```
{
    "query": {
        "term": { "title": "crime" }
    },
    "highlight": {
        "fields": { "title": {} }
    }
}
```


강조 태그 변경  
```
{
    "query": {
        "term": { "title": "crime" }
    },
    "highlight": {
        "fields": { "title": {} },
        "pre_tags": ["<b>"],
        "post_tags": ["<b>"]
    }
}
```


강조되는 영역 제어  
number_of_fragments: 기본값 5, 반환하는 영역 수 정의, 0으로 설정하면 전체 필드  
fragment_size: 강조된 영역의 최대 길이를 글자 단위로 명세, 기본값 100  


전역 강조  
```
{
    "query": {
        "term": { "title": "crime" }
    },
    "highlight": {
        "fields": { "title": {} },
        "pre_tags": ["<b>"],
        "post_tags": ["<b>"]
    }
}
```

지역 강조  
```
{
    "query": {
        "term": { "title": "crime" }
    },
    "highlight": {
        "fields": { 
            "title": {
                "pre_tags": ["<b>"],
                "post_tags": ["<b>"]
            }
         }
    }
}
```


필드 일치 요구  
질의와 일치하는 필드만 보여주기를 원하는 경우  
require_field_match: true로 설정  


PostingsHighlighter  
필드 정의에서 index_options 프로퍼티를 offsets 로 설정  
매핑에 필드 두개 contents, contents.ps 정의
```
curl -XPOST 'localhost:9200/test/doc/_mapping' -d '
{
    "doc": {
        "properties": {
            "contents": {
                "type": "string",
                "fields": {
                    "ps": { "type": "string", "index_options": "offsets" }
                }
            }
        }
    }
}'
```


질의 검증  
```
curl -XGET 'localhost:9200/test/_validate/query?pretty' -d '{쿼리내용}'

curl -XGET 'localhost:9200/test/_validate/query?pretty&explain' --data-binary '{쿼리내용}'
```


기본 정렬  
분석되지 않는 필드를 선택해서 정렬하는 것이 바람직  
```
{
    "sort": {
        "_score": "desc"
    }
}
```


누락된 필드를 위한 정렬  
"missing": "_last" 설정으로 해당 필드가 없는 다큐먼트를 결과 목록의 가장 아래로 보낸다  
missing 매개변수를 숫자로 설정하면 정의된 필드가 없는 다큐먼트는 필드값이 숫자인 다큐먼트로 취급된다  
```
{
    "sort": {
        "section": {
            "order": "asc",
            "missing": "_last"
        }
    }
}
```


동적 기준으로 정렬  
tags 필드에 색인된 첫 값으로 정렬  
```
{
    "sort": {
        "_script": {
            "script": "doc['tags'].values.length > 0 ? doc['tags'].values[0] : '\u19999'",
            "type": "string",
            "order": "asc"
        }
    }
}
```


질의 재작성  
prefix,  wildcard 질의와 같은 복수 키워드 질의는 성능적인 이유로 인해 재작성 기능 사용  
질의 재작성 방식을 제어하기 위해 복수 키워드 질의에서 rewriter 매개변수 사용 가능  
```
{
    "query": {
        "prefix": {
            "title": "s",
            "rewrite": "constant_score_boolean"
        }
    }
}
```
scoring_boolean: 생성된 각각의 키워드를 부울 should 절로 변환  
constant_score_boolean: scoring_boolean와 비슷 점수 계산이 없어 CPU 덜 소비  
constant_score_filter: 키워드에 대한 모든 다큐먼트를 표시하게 동작하는 전용 필터 생성  
top_terms_N: 생성된 각각의 키워드를 부울 should 절로 변환, 부울 절의 최대 제약을 넘기지 않게 상위 점수 N개만 유지  
top_terms_boost_N: top_terms_N과 유사, 점수는 질의가 아닌 중요도로 계산  


엘라스틱서치 동적인 동작 방식을 따르지 않게 색인 일부를 끄고 싶은 상황에  
```
"author": {
    "type": "object",
    "dynamic": false,
    "properties: {
        "name": {
            "type": "object",
            "dynamic": false,
            "properties": {
                "firstName": { "type": "string", "index": "analyzed" },
                "lastName": { "type": "string", "index": "analyzed" }
            }
        }
    }
}
```
새로운 필드를 추가하려면 매핑을 갱신해야 한다  
elasticsearch.yml 파일에 index.mapper.dynamic 프로퍼티를 추가하고 false로 설정하면 동적 매핑 기능 끌 수 있음  


중첩된 객체를 기준으로 자료를 필터링 하고 싶으면 nested 필터를 사용할 수 있다.  

중첩된 다큐먼트를 처리할 때 path 프로퍼티 외에 score_mode 프로퍼티를 사용할 수 있으며,  
중첩된 질의에서 점수를 계산하는 방법 정의  
avg: 기본값, 중첩된 개별 질의 점수를 계산해 평균을 주 질의 점수에 포함  
total: 중첩된 개별 점수를 계산해 총합을 주 질의 점수에 포함  
max: 중첩된 개별 점수를 계산해 최대값을 주 질의 점수에 포함  
none: 점수를 취하지 않음  



부모-자식 관계  
부모 다큐먼트에 필요한 필드는 name  
자식 매핑을 생성하려면 _parent 프로퍼티 추가, parent 타입 이름 설정  
부모 다큐먼트 생성  
```
curl -XPOST 'localhost:9200/shop/cloth/1 -d { "name": "Test shirt" }
```
자식 다큐먼트 생성  
```
curl -XPOST 'localhost:9200/shop/variation/1000?parent=1' -d { "color": "red", "size": "XL" }
```
자식 다큐먼트에 질의를 내리고 부모의 존재 유무 점검 가능  
부모에 대해 질의를 내릴때 자식 다큐먼트 반환 안됨, 반대도 마찬가지  

has_child 타입은 자식 다큐먼트 검색을 원한다고 알려준다  
```
curl -XPOST 'localhost:9200/shop/_search?pretty' -d '
{
    "query": {
        "has_child": {
            "type": "variation",
            "query": {
                "bool": {
                    "must": [
                        { "term": { "size": "XXL" }},
                        { "term": { "color": "red" }}
                    ]
                }
            }
        }
    }
}
```

top_children 질의  
부모 다큐먼트를 반환, score 매개변수를 사용한 점수 계산 방법을 명세  


has_parent 질의  
부모 다큐먼트에서 일치하는 자식 다큐먼트를 반환 받을때 사용  
```
curl -XPOST 'localhost:9200/shop/_search?pretty' -d '
{
    "query": {
        "has_parent": {
            "parent_type": "cloth",
            "query": {
                        "term": { "name": "test" }
                    ]
                }
            }
        }
    }
}
```


부모-자식관계 사용시 고려 사항  
부모와 자식 다큐먼트를 동일 샤드에 저장해야 한다  
has_child와 같은 질의 수행시 다큐먼트 식별자를 미리 메모리에 올려 캐시할 필요가 있다  



색인 구조 수동변경  
새로운 필드 추가  
추가 후 모든 다큐먼트 재 색인 필요  
```
curl -XPUT 'localhost:9200/users/user/_mapping' -d '
{
    "user": {
        "properties": {
            "phone": {
                "type": "string",
                "store": "yes",
                "index": "not_analyzed"
                
            }
        }
    }
}
```

새로운 타입 정의 추가, 새로운 필드 추가, 새로운 분석기 추가 가능  
필드 타입 변경, 필드 저장기능 변경, 색인된 프로퍼티값 변경, 이미 색인된 다큐먼트의 분석기 변경 불가!!  



다큐먼트의 점수 계산 작업 (TF/IDF)  
- 다큐먼트 중요도 Document boost: 타큐먼트에 주어진 중요도값  
- 필드 중요도 Field boost: 필드에 주어진 중요도  
- 조정 지수 Coord: 더 많은 검색 키워드를 포함하는 다큐먼트에 점수를 더 준다  
- IDF Inverse Document Frequency: 키워드가 얼마나 드문지 점수 계산  
- 길이 기준 Length norm: 필드가 길수록 지수가 주는 중요도가 작아진다.  필드가 포함된 키워드 수에 기반  
- 키워드 빈도 Term frequency: 다큐먼트에 얼마나 많은 키워드가 출현하는지 기술  
- 질의 기준 Query norm: 질의 키워드의 중요도를 제곱한 합으로 계산  

일치되는 키워드가 드물수록 다큐먼트 점수가 높다  
다큐먼트 필드가 작을수록 다큐먼트 점수가 높다  
필드에 대한중요도가 높을수록 다큐먼트 점수가 높다  


엘라스틱서치가 제공하는 가능성  
Script: 실제 스크립트 코드 포함  
Lang: 언어에 대한 정보, 생략시 MVEL 가정  
Params: 매개변수와 값 포함  


스크립트 수행 과정에서 사용 가능한 객체  
_doc: 계산된 점수와 필드값을 사용해 찾은 현재 다큐먼트에 접근하게 만든다, 예) _doc.title.value  
_source: 현재 다큐먼트의 원본과 여기에 정의된 값에 접근하게 만든다, 예) _source.title   
_fields: 다큐먼트 필드값에 접근할 목적으로 사용한다, 예) _fields.title.value  

필드 하나는 다음과 같이 색인에 저장  
- _source 다큐먼트의 일부  
- 해석되지 않은 상태에서 저장된 값  
- 토큰으로 해석되어 색인된 값  


MVEL (MVFLEX Expression Language)  
스크립트 작성을 위한 언어,  
자바 객체를 사용하게 만들어주며 getter/setter 호출에 대한 프로퍼티를 자동으로 매핑,
단순한 타입 변환, 컬렉션을 사상(map)하면 배열과 연관 배열로 사상한다  
다른 언어 사용시 명령어  
```
bin/plugin -install elasticsearch/elasticsearch-lang-javascript/2.0.0.RCI
```


직접 만든 스크립트 라이브러리 사용  
만든 스크립트는 엘라스틱서치 디렉터리의 config/scripts에 위치해야 한다  
파일 확장자는 사용된 언어를 지정해야 한다  
사용 예 (파일명: test_sort.js)  
```
{
    "query": {
        ...
    },
    "sort": {
        "_script": {
            "script": "test_sort",
            "type": "string",
            "order": "asc"
        }
    }
}
```


컴파일된 코드 사용  
- 팩터리 구현: org.elasticsearch.script.NAtiveScriptFactory 클래스 구현  
- 컴파일된 스크립트 구현: org.elasticsearch.script.AbstractSearchScript 클래스를 상속받아 run() 메소드 구현  
- 스크립트 설치: 컴파일된 클래스를 jar 아카이브로 묶은 다음 엘라스틱서치의 lib 디렉터리에 복사, elasticsearch.yml 파일에 추가  
- 스크립트 수행: 엘라스틱서치 재시작  


엘라스틱서치에서 여러 언어 처리하기  
- 언어별로 나눠 다른 타입에 다큐먼트 저장  
- 언어별로 분리된 색인에 다큐먼트 저장  
- 단일 다큐먼트의 여러 필드에 언어별로 저장  


다큐먼트의 언어 감지 라이브러리  
- 아파치 티카 Apache Tika  
- 랭귀지 디텍션 Language detaction  


분석기가 lang 필드에 기반해 선택되기를 원하면 엘라스틱서치에 존재하는 분석기 이름 중 하나를 lang필드 값에 추가해야한다  

식별된 언어를 사용한 질의  
```
curl -XGET 'localhost:9200/docs/_search?pretty=true' -d
{
    "query":{
        "match": {
            "content": {
                "query": "찾을 키워드",
                "analyzer": "분석기 이름"
            }
        }
    }
}
```

중요도를 질의에 추가  
가장 중요한 필드 to, 덜 중요한 필드 from  
```
{
    "query': {
        "query_string": {
            "fields": ["from^5", "to^10", "subject],
            "query": "john",
            "use_dis_max": false
        }
    }
}
```


constant_score 질의  
필터나 질의를 받아 점수로 사용하는 값을 명시적으로 설정, boost 매개변수를 이용해 일치하는 다큐먼트마다 점수 부여  
```
{
    "query": {
        "constant_score': {
            "query": {
                "query_string": {
                    "query": "available:false author:heller"
                }
            }
        }
    }
}
```


boosting 질의  
일치하는 모든 다큐먼트의 점수를 줄이고 싶을 때 질의의 일부로 사용  
```
{
    "query": {
        "boosting': {
            "positive": {
                "term": {
                    "available": true
                }
            },
            "negative": {
                "match": {
                    "author": "remarque"
                }
            },
            "negative_boost": 0.1
        }
    }
}
```


function_score 질의  
점수 계산에 비용이 들어갈 경우 유용, 필터링된 다큐먼트의 점수를 계산  
```
{
    "query": {
        "function_scroe": {
            "query": { ... },
            "filter": { ... },
            "functions": [
                {
                    "filter": { ... },
                    "FUNCTION": { ... }
                }
            ],
            "boost_mode": "...",
            "score_mode": "...",
            "max_boost": "...",
            "boost": ..."
        }
    }
}
```
boost_mode: 질의 점수와 결합될 함수 질의 점수 계산 방법 정의  
- multiply: 기본 동작 방식, 함수에서 계산된 점수를 질의 점수에 곱한다  
- replace: 질의 점수를 완전히 무시하고 함수가 계산한 점수 반환  
- sum: 질의 점수와 함수 점수를 합한다  
- avg: 질의 점수와 함수 점수의 평균값  
- max: 함수 점수와 질의 점수 중 최대값  
- min: 함수 점수와 질의 점수 중 최소값  

score_mode: 함수가 계산한 모든 점수를 결합하는 방법 정의  
- multiply: 기본 동작 방식, 점수의 곱 반환  
- sum: 점수의 합  
- avg: 점수의 평균  
- first: 다큐먼트와 일치하는 필터가 존재하는 첫 함수의 점수 반환  
- max: 최대 점수
- min: 최소 점수  


functions 절에 포함할 수 있는 함수  
- boost_factor: 다큐먼트의 점수에 주어진 값을 곱한다  
- script_score: 반환하는 점수를 계산하기 위해 스크립트를 사용  
- random_score: 초기 seed 값을 명세해 가상 난수 점수롤 생성  
- decay: 거리가 멀어짐에 따라 decay 함수가 계산한 점수도 낮아진다, 특정 지점에서 거리를 기준으로 중요도를 높이는 경우 사용  


향후 지원이 중단될 질의  
custom_boost_factor, custom_score, custom_filters_score  


중요도 정의  
- 입력 자료 필드의 중요도 정의  
```
{
    "title": "The Complete Sherlock Holmes",
    "author": {
        "_value": "Arthur Conan Doyle",
        "_boost": 10.0
    },
    "year": 1936
}
```
- 매핑에서 중요도 정의  
```
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
```


동의어 파일 시스템 설정  
```
"filter": {
    "synonym": {
        "type": "synonym",
        "synonyms_path": "synonym.txt"
    }
}
```


동의어 확장  
```
"filter": {
    "synonym": {
        "type": "synonym",
        "expand": true,
        "synonym": ["one", "two", three"]
    }
}
```
expand 설정을 false로 했을때 one, two three => one 이었다면,  
expand 설정을 true로 했을때 one, two three => one, two three 이 된다.  


WordNet 동의어 사용  
[https://wordnet.princeton.edu/](https://wordnet.princeton.edu/)  


aggs(aggregations) 프로퍼티 = 집계 질의  
숫자 집계  
min, max, sum, avg 집계  
스크립트 사용  
value_count 집계  
stats와 extended_stats 집계  
버킷 bucketing aggrgation 집계  
terms 집계  
range 집계  
date_range 집계  
ip_range 집계  
missing 집계  
nested 집계  
children 집계
top_hits 집계
matrix_stats 집계
histogram 집계  
date_histogram 집계  
geo_distance 집계  
geo_bounds 집계
geo_centroid 집계
geohash_grid 집계: [https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-geohashgrid-aggregation.html](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-geohashgrid-aggregation.html)  
필터 쿼리: 필터 결과는 점수에 의해 정렬되지 않는다. 모든 결과에 대한 점수가 1.0 이므로..  


집계 중첩 예  
```
{
    "aggs": {
        "variations": {
            "nested": {
                "path": "variation"
            },
            "aggs": {
                "sizes": {
                    "terms": {
                        "field": "variation.size"
                    }
                }
            }
        }
    }
}
```


중첩된 집계에서 얻은 값을 사용한 집계  
```
{
    "aggs": {
        "availability": {
            "terms": {
                "field": "copies",
                "order": { "numbers.avg": "desc" }
            },
            "aggs": {
                "numbers": { "stats": {} }
            }
        }
    }
}
```


전역과 부분 집합  
```
{
    "query": {
        "filtered": {
            "query": {
                "match_all": {}
            },
            "filter": {
                "term": { "available": "true" }
            }
        }
    }
    "aggs": {
        "with_global": {
            "global": {},
            "aggs": {
                "copies": {
                    "value_count": {
                        "field": "copies"
                    }
                }
            }
        },
        "without_global": {
            "value_count": {
                "field": "copies"
            }
        }
    }
}
```


포함과 배제 예  
```
{
    "aggs": {
        "availability": {
            "terms": {
                "field": "characters",
                "exclude": "al.*",
                "include": "a.*"
            }
        }
    }
}
```


패싯 타입 정보  
_type: 사용된 패싯 타입 정의, 패싯 타입마다 독자적으로 제공  
missing: 패싯을 계산하기에 충분한 자료가 없는 다큐먼트 수를 정의  
total: 패싯 계산 과정에서 존재하는 토큰 수 정의  
other: 반환된 수에 포함되지 않는 패싯값의 수를 정의  


패싯 결과에서 질의와 일치하는 다큐먼트 수 얻기  
```
{
    "query": { "match_all": {} },
    "facets": {
        "my_query_facet": {
            "query": {
                "term": { "tags": "personal" }
            }
        }
    }
}
```


패싯 계산에 필터 사용(질의를 감싼 필터 패싯이나 질의 패싯보다 빠름)  
```
{
    "query": { "match_all": {} },
    "facets": {
        "my_filter_facet": {
            "filter": {
                "term": { "tags": "personal" }
            }
        }
    }
}
```


키워드 패싯(빈도가 높은 키워드 반환)  
```
{
    "query': { "match_all": {}},
    "facets": {
        "tags_facet_result": {
            "terms": {
                "field": "tags"
            }
        }
    }
}
```


term 패싯 매개변수  
- size: 빈도가 높은 최상위 키워드가 몇 개인지 명세  
- shard_size: 샤드당 결과 수 명세, size 매개변수 보다 낮은 값으로 설정 불가  
- order: 패싯 순서 명세 (count:기본값 빈도 높은 수부터, term:알파벳 오름차순, reverse_count: 빈도 낮은 수 부터, reverse_term:알파벳 내림차순)  
- all_terms: true로 설정하면 결과에 모든 키워드 반환  
- exclude: 패싯 계산에서 배제되어야 할 키워드 명세  
- regex: 패싯 계산 과정에 포함될 키워드를 제어할 정규표현식 명세  
- script: 패싯 계산에서 키워드 처리 목적으로 사용될 스크립트 명세  
- fields: 패싯 계산을 위해 복수 필드를 지정하는 배열 명세  
- script_field: 계산 목적으로 실제 키워드를 제공할 스크립트 정의  


범위 기반 패싯  
```
{
    "query': { "match_all": {}},
    "facets": {
        "ranges_facet_result": {
            "range": {
                "field": "total",
                "ranges": [
                    { "to": 90 },
                    { "from": 90, "to": 180 },
                    { "from": 180 },
                ]
            }
        }
    }
}
```
from: 범위의 시작  
to: 범위의 끝  
min: 해당 범위에 속한 필드의 최소값  
max: 해당 범위에 속한 필드의 최대값  
count: 해당 범위에 속한 필드값이 존재하는 다큐먼트 수  
total_count: 해당 범위에 속한 정의된 필드값의 총 수  
total: 해당 범위에 속한 정의된 모든 필드값의 합  
mean: 해당 범위에 속한 ranges 패싯 과정에 사용된 모든 필드값의 평균값  


집계된 자료 계산 과정에서 다른 필드 선택하기  
범위를 계산하는 필드와는 다른 필드를 대상으로 집계를 할 경우 key_field, key_value 두개의 프로퍼티 사용  


histogram 패싯  
필드값의 간격에 따라 히스토그램을 만든다  
```
{
    "query': { "match_all": {}},
    "facets": {
        "total_histogram": {
            "histogram": {
                "field": "total",
                "interval": 1000
            }
        }
    }
}
```


date_histogram 패싯  
```
{
    "query': { "match_all": {}},
    "facets": {
        "date_histogram_test": {
            "date_histogram": {
                "field": "date",
                "interval": day
            }
        }
    }
}
```


statistical 패싯  
숫자 필드 타입에 대한 통계 자료 계산  
```
{
    "query': { "match_all": {}},
    "facets": {
        "statistical_test": {
            "statistical": {
                "field": "total"
            }
        }
    }
}
```
반환되는 통계 목록  
- _type: 패싯 타입  
- count: 정의된 필드에 값이 있는 다큐먼트 수  
- total: 정의된 필드를 모두 합한 값  
- min: 최소 필드값  
- max: 최대 필드값  
- mean: 명세된 필드의 평균값  
- sum_of_squares: 명세된 필드의 값을 제곱한 결과의 합  
- variance: 명세된 필드의 분산  
- std_deviation: 명세된 필드의 표준편차  


terms_stat 패싯  
statistical과 term 패싯을 결합한 형태  
```
{
    "query': { "match_all": {}},
    "facets": {
        "total_tags_terms_stats": {
            "terms_stat": {
                "key_field": "tags",
                "value_field": "total"
            }
        }
    }
}
```
key_field: 키워드를 제공하는 필드 이름을 담음  
value_field 숫자 자료를 저장하기 위한 필드  


geo_distance 패싯  
특정 위치에서 떨어진 거리에 포함되는 다큐먼트 수에 대한 정보 제공  
```
{
    "query': { "match_all": {}},
    "facets": {
        "spatial_test": {
            "geo_distance": {
                "location": {
                    "lat": 10.0,
                    "lon": 10.0
                },
                "ranges": [
                    { "to": 10 },
                    { "from": 10, "to": 100 },
                    { "from": 100 }
                ],
                "unit": "mi",
                "distance_type": "plane"  #arc: 정밀한 계산, plane 빠른 계산
            }
        }
    }
}
```


패싯 결과 필터링  
복수 값인 tags 필드에 대한 계산된 패싯에 fashion 키워드가 있는 다큐먼트롤 범위를 좁힐때  
```
{
    "query": { "match_all": {} },
    "facets": {
        "tags": {
            "terms": { "field": "tags" },
            "facet_filter": {
                "term": { "tags": "fashion" }
            }
        }
    }
}
```


제안 기능  
성능을 염두에 두고 사용자의 철자 오류 수정, 자동완성 구현  
[https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-suggesters.html](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-suggesters.html)  


제안 기능 타입  
- term: 전달된 각 단어에 대해 올바른 내용을 돌려주는 제안  
- phrase: 적절한 구를 반환하는 제안  
- completion: 자동완성 결과를 제공하게 설계된 제안  


제안 기능 예  
키워드 철자가 잘못된 serlock holnes 구에 대한 제안 결과 얻기  
```
curl -XGET 'localhost:9200/library/_search?pretty' -d '
{
    "query": {
        "match_all": {}
    },
    "suggest": {
        "first_suggestion": {
            "text": "serlock holnes",
            "term": {
                "field": "_all"
            }
        }
    }
}'
```

여러 제안을 얻고 싶을때  
```
curl -XGET 'localhost:9200/library/_search?pretty' -d '
{
    "query": {
        "match_all": {}
    },
    "suggest": {
        "first_suggestion": {
            "text": "serlock holnes",
            "first_suggestion": {
                "term": {
                    "field": "_all"
                }
            },
            "second_suggestion": {
                "term": {
                    "field": "title"
                }
            }
        }
    }
}'
```
결과 options 배결의 프로퍼티  
- text: 제안 텍스트 정의  
- score: 제안 점수 정의, 점수가 높을수록 제안 결과도 좋다  
- freq: 제안 빈도정의, 다큐먼트에 해당 단어가 얼마나 많이 등장하는지 표현  

term 제안  
문자열 에디트 거리 string edit distance 기반  
term 제안 옵션  
- text: 제안 결과를 얻고싶은 텍스트  
- field: 제안 결과를 생성할 필드  
- analyzer: 분석기 이름  
- size: 기본값 5, 제공되는 키워드를 대상으로 반환할 최대 제안 수  
- sort: 결과에 대한 정렬 방법(score, frequency)  

추가 옵션  
- lowercase_terms: true로 설정하면 분석 직후 text 필드에서 생성한 모든 키워드를 소문자로 만든다  
- max_edits: 기본값 2, 최대 에디트 거리  
- prefix_len: 기본값 1
- min_word_len: 기본값 4, 최소 제안 글자 수  
- shard_size: 기본값이 size 매개변수로 지정된 값, 각 샤드에서 읽어햐 하는 제안 최대 수  


phrase 제안  
term 제안 기능 위에 구에 대한 계산 논리 추가  
제공 옵션  
- lowercase_terms: true로 설정하면 분석 직후 text 필드에서 생성한 모든 키워드를 소문자로 만든다  
- max_edits: 기본값 2, 최대 에디트 거리  
- prefix_len: 기본값 1
- min_word_len: 기본값 4, 최소 제안 글자 수  
- shard_size: 기본값이 size 매개변수로 지정된 값, 각 샤드에서 읽어햐 하는 제안 최대 수  
- max_errors: 교정을 위해 오류가 발생할 수 있는 키워드 최대 수  
- separator: 기본값은 여백문자, 키워드를 바이그램 bigram 필드로 나누기 위해 사용될 구분자 명세  


completion 제안  
자동완성 기능 색인 생성 예  
```
curl -XPOST 'localhost:9200/authors' -d '
{
	"mappings": {
        "author": {
            "properties": {
                "name": { "type": "string" },
                "ac": {
                    "type": "completion",
                    "index_analyzer": "simple",
                    "search_analyzer": "simple",
                    "payloads": true
                }
            }
        }
    }
}'


curl -XPOST 'localhost:9200/authors/author/1' -d '
{
	"name": "Fyodor Dostoevsky",
    "ac": {
        "input": [ "fyodor", "dostoevsky" ],
        "output": "Fyodor Dostoevsky",
        "payload": { "books": [ "123", "124"] },  #payload: 추가정보 제공
        "weight": 30   #weight: 제안의 가중치, 높을수록 중요도가 높아진다
    }
}'
```


예상 검색(percolator)  
```
curl -XPOST 'localhost:9200/notifier' -d '
{
    "mappings": {
        "book": {
            "properties": {
                "abailable": { "type": "boolean" }
            }
        }
    }
}'
```
예상 검색에 등록된 질의도 도큐먼트 이므로 percolator 색인에 저장될 질의 중 예상 검색에 사용된 질의를 선택하기 위해  
엘라스틱서치에 일반적인 질의를 전송할 수 있다  
예상 검색은 has_child, top_children, has_parent, nested와 같은 질의는 사용할 수 없다  


색인된 다큐먼트 예상 검색  
```
curl -XGET 'localhost:9200/library/book/1/_precolate?precolate_index=notifier'
```
이미 색인된 library 도큐먼트와 일치하는 질의가 있는지 precolate_index 매개변수로 정의한 예상 검색 색인에 대해 점검  



파일 내용 검색  
애플리케이션에 논리 logic를 추가해 파일을 가져와 중요한 정보를 추출하고 json 객체를 생성해 엘라스틱 서치에 색인하는 방법  
```
bin/plugin -install elasticsearch/elasticsearch-mapper-attachments/2.0.0.RC1

설치 후 매핑을 따르는 색인 준비

{
    "mappings": {
        "file": {
            "properties": {
                "note": { "type": "string", "store": "yes" },              #사용할 이름
                "book": {
                    "type": "attachment",
                    "fields": {
                        "file": { "store": "yes", "index": "analyzed" },   #파일 내용
                        "date": { "store" :"yes" },                        #파일 생성날짜
                        "author": { "store": "yes" },                      #파일 작성자
                        "keywords": { "store": "yes" },                    #다큐먼트와 연결된 키워드
                        "content_type": { "store": "yes" },                #다큐먼트의 MIME 타입
                        "title": { "store": "yes" }                        #다큐먼트 제목
                    }
                }
            }
        }
    }
}
```


파일 내용을 json 구조체에 포함하기 위해 base64 알고리즘으로 파일 내용 인코딩  
```
base64 -i example.docx -o example.docx.base64
```



Geo 공간 검색(spatial search)  
거리 기반 정렬 예  
```
{
    "query": {
        "match_all": {}
    },
    "sort": [
        {
            "_geo_distance": {
                "location": "48.8567, 2.3508",
                "unit": "km"
            }
        }
    ]
}
```

bounding_box 필터링 예  
```
{
    "filter": {
        "geo_bounding_box": {
            "location": {
                "top_left": "52.4796, -1.903",
                "bottom_right: "48.8567, 2.3508"
            }
        }
    }
}
```

거리 제한 예  
```
{
    "filter": {
        "geo_distance": {
            "location": "48.8567, 2.3508",
            "distance": "500km"
        }
    }
}
```


공간검색 임의의 형태  
```
#매핑 추가
{
    "poi": {
        "properties": {
            "name": { "type": "string", "index": "not_analyzed" },
            "location": { "type": "geo_shape" }
        }
    }
}

# point
{
    "location": {
        "type": point",
        "coordinates": [-0.1275, 51.507222]
    }
}

# envelope
{
    "type": "envelope",
    "coordinates": [
        [-0.087890625, 51.50874245880332],
        [2.4169921875, 48.80686346108517]
    ]
}

# polygon
{
    "type": "polygon",
    "coordinates": [
        [-7.250977, 55.124723],
        [1.845703, 51.500194],
        [-7.250977, 55.124723]
    ]
}

# multipolygon
{
    "type": "multipolygon",
    "coordinates": [
        [
            [-7.250977, 55.124723],
            [1.845703, 51.500194],
            [-7.250977, 55.124723]
        ],
        [
            [-0.087890625, 51.50874245880332],
            [2.4169921875, 48.80686346108517],
            [3.88916015625, 51.01375465718826],
            [-0.087890625, 51.50874245880332]
        ]
    ]
}

# 용례
{
    "filter": {
        "geo_shape": {
            "location": {
                "shape": {
                    "type": "polygon",
                    "coordinates: [
                        [
                            [-0.087890625, 51.50874245880332],
                            [2.4169921875, 48.80686346108517],
                            [3.88916015625, 51.01375465718826],
                            [-0.087890625, 51.50874245880332]
                        ]
                    ]
                }
            }
        }
    }
}
```


색인에 형태 저장  
예) 한국의 경계  
```
# 매핑 정의
{
    "country": {
        "properties": {
            "name": { "type": "string", "index": "not_analyzed" },
            "area": { "type": "geo_shape" }
        }
    }
}

# 검색
{
    "filter": {
        "geo_shape": {
            "location": {
                "indexed_shape": {
                    "index": "countries",
                    "type": "country",
                    "path": "area",
                    "id": "1"
                }
            }
        }
    }
}
```


스크롤 API  
스크롤 관련 정보와 결과에 대한 정보를 엘라스틱서치라 얼마나 오랫동안 보존해야 할지 제안하는 값  
질의 전송 예  
```
curl -XGET 'localhost:9200/library/_search?pretty&scroll=5m' -d '{ "query": { "match_all": {} } }'
```
엘라스틱서치는 종단점 _search/scroll 제공  
scroll_id 를 사용해 다음 이어지는 페이지를 얻을 수 있다  
해당 핸들은 일정 시간동안만 유효하므로 지정 시간이 지나면 무효화된 값이되어 오류 응답을 반환한다  



terms 필터  
```
{
    "query": {
        "constant_score": {
            "filter": {
                "terms": {
                    "title": [ "crime", "punishment" ],
                    "execution": "plain"
                }
            }
        }
    }
}
```
execution 매개변수  
- plain: 기본값, 결과를 비트셋에 저장하고 캐시  
- fielddata: 키워드를 비교하기 위해 fielddata 캐시를 사용, 키워드를 대량으로 필터링 할때 효율적  
- bool: 각 키워드마다 term 필터 생성하고 거기에 bool 필터 생성, 캐시안됨
- and:bool: 값과 유사하지만, bool 필터 대신 and 필터 생성  
- or:bool: 값과 유사하지만, bool 필터 대신 or 필터 생성  


키워드 탐색  
```
"filter": {
    "terms": {
        "id": {
            "index": "books",              # 메모리에 올릴 색인
            "type": "book",                # 관심 있는 타입
            "id": "3",                     # 다큐먼트 식별자
            "path": "similar",             # 키워드를 메모리에 올릴 필드 이름
            "routing": "",                 # 키워드를 필터에 올릴 때 라우팅값
            "cache": true                  # 기본값 true, 캐시할지 여부
        },
        "_cache_key": "books_3_similar"
    }
}
```
execution 매개변수는 고려하지 않음  


키워드 탐색 캐시 설정  
elasticsearch.yml 파일에서 아래 프로퍼티 설정  
indices.cache.filter.terms.size: 기본값 10mb, 키워드 탐색 캐시를 위해 사용 가능한 총 메모리양  
indices.cache.filter.terms.expire_after_access: 마지막 접근 후 만료되어야 하는 시간  
indices.cache.filter.terms.expire_after_write: 캐시에 들어간 후 만료되어야 하는 최대 시간  



클러스터 형성과 노드 검색 과정:탐색 discovery  
기본 젠(zen) 탐색 모듈 사용, 멀티캐스트와 유니캐스트 지원  
멀티캐스트/유니캐스트 [http://guruble.com/elasticsearch-3-node-discovery/](http://guruble.com/elasticsearch-3-node-discovery/)  



##### elasticsearch.yml 프로퍼티  
> node.master: true  
마스터 노드로 동작  
> node.data: true  
자료를 담는 노드로 동작  
> discovery.zen.minimum_master_nodes  
서로 연결되어야 하는 마스터로 승격이 가능한 노드의 최소 수 정의  
> cluster.name  
기본 이름 elasticsearch  

> discovery.zen.ping.multicast.enabled  
멀티캐스트 활성화 여부   
> discovery.zen.ping.multicast.group  
멀티캐스트 요청을 위해 사용할 그룹 주소, 기본값 224.2.2.4  
> discovery.zen.ping.multicast.port  
멀티캐스트 요청을 위해 사용될 포트, 기본값 54328  
> discovery.zen.ping.multicast.ttl  
멀티캐스트 요청이 유효한 시간, 기본값 3초     
> discovery.zen.ping.multicast.address  
엘라스틱서치가 바인드할 주소, 기본값 null  

> indices.memory.index_buffer_size  
기본값 100%, 특정 노트에 존재하는 모든 색인의 샤드 사이에서 나눠 사용할 수 있는 전체 메모리 양 제어  
> indices.memory.min_shard_index_buffer_size  
기본값 4mb, 샤드별 최소 색인 버퍼값 설정  

> index.refresh_interval  
기본값 1s(1초), 색인 검색기 객체는 얼마나 자주 새로 고쳐야 하는지명세  

> threadpool.index.type: cache  
들어오는 요청에 대해 스레드를 생성하는 한계가 없는 스레드 풀  
> threadpool.index.type: fixed  
크기가 고정된 스레드 풀  

> threadpool.index  
색인과 삭제 연산에 쓰인다. 기본값의 타입은 fixed, 크기는 가용 프로세서 수, 큐 크기는 300  
> threadpool.search  
검색과 수를 세기 위한 요청에 쓰인다. 기본값의 타입은 fixed, 크기는 가용 프로세서 수*3, 큐 크기는 1000  
> threadpool.suggest  
제안 요청을 위한 스레드, 기본값의 타입은 fixed, 크기는 가용 프로세서 수, 큐 크기는 1000  
> threadpool.get  
실시간 get 요청에 쓰인다. 기본값의 타입은 fixed, 크기는 가용 프로세서 수, 큐 크기는 1000  
> threadpool.bulk  
대량 연산을 위한 스레드 풀,  기본값의 타입은 fixed, 크기는 가용 프로세서 수, 큐 크기는 50  
> threadpool.percolate  
예상 검색 요청을 위한 스레드 풀, 기본값 타입은 fixed, 크기는 가용 프로세서 수, 큐 크기는 1000

> discovery.zen.ping.unicast.hosts  
유니캐스트를 사용하기 위해 클러스터를 형성하는 모든 호스트 명세  
```
discovery.zen.ping.unicast.hosts: 192.168.2.1:[9300-9399], 192.168.2.2:[9300-9399]
```

> cluster.routing.allocation.allow_rebalance  
always: 필요한 즉시 균형잡기 바로 시작  
indices_primaries_active: 모든 주 샤드가 초기화되고 균형잡기 시작  
indices_all_active: 기본값, 모든 샤드와 레플리카가 초기화되고 나서 균형잡기 시작  
> cluster.routing.allocation.cluster_concurrent_rebalance  
전체 클러스터를 대상으로 한 번에 노드 사이에서 옮길 수 있는 샤드 수 지정  
> cluster.routing.allocation.node_concurrent_recoveries  
단일 노드에서 동시에 초기화하는 샤드 수 설정  
> cluster.routing.allocation.node_initial_primaries_recoveries  
노드에서 동시에 초기화하는 주 샤드 수 제어  
> cluster.routing.allocation.enable  
all: 기본값, 모든 샤드 유형을 할당할 수 있다  
primaries: 주 샤드만 할당할 수 있다  
new_primaries: 새롭게 생성된 주 샤드만 할당할 수 있다  
none: 샤드 할당을 비활성화  
> indices.recovery.concurrent_streams  
샤드 집합에서 샤드를 복구하기 위해 한번에 노드에서 열릴 수 있는 스트림 수 제어  


> node.zone  
> index.routing.allocation.include.zone  
> index.routing.allocation.exclude.zone #할당에서녿 배체  
> index.routing.allocation.require.size #노드 할당 규칙  
> index.routing.allocation.require.zone #노드 할당 규칙  
> index.routing.allocation.include._ip #IP 기반 샤드 할당 활성화  
> cluster.routing.allocation.disk.threshold_enabled #디스크 기반 샤드 할당 활성화  
> cluster.info.update.interval #디스크 기반 샤드 할당 구성 방법1  
> cluster.routing.allocation.disk.watermark.low #디스크 기반 샤드 할당 구성 방법2  
> cluster.routing.allocation.disk.watermark.high #디스크 기반 샤드 할당 구성 방법3  
> cluster.routing.allocation.include._ip #클러스터 IP기반 색인 배치  
> index.routing.allocation.total_shards_per_node #단일 노드에 위치 가능한 샤드의 최대 수 지정  


> discovery.zen.fd.ping_interval  
노드가 동작하고 응답하는지 점검하기 위해 전송되는 시그널(핑ping) 주기, 기본값 1s(1초)  
> discovery.zen.fd.ping_timeout  
기본값 30s(30초), 핑 응답을 기다릴 시간  
> discovery.zen.fd.ping_retries  
기본값 3, 재시도 수  


게이트웨이 모듈  
색인과 색인 내부에 저장된 자료, 타입 매핑이나 색인 수준의 설정 등 클러스터 자료와 메타데이터를 영속적으로 저장,  
매번 클러스터를 시작할 때마다 게이트웨이에서 읽으며 클러스터에 변경을 가할 때 게이트웨이 모듈을 사용해 영속적으로 저장  
elasticsearc.yml 파일 설정  
> gateway.type: local  
색인과 메타데이터를 지역 파일시스템에 저장  
fs, hdfs, s3 옵션이 있다  


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


템플릿  
새로 생성되는 색인 이름과 비교하기 위한 패턴을 정의  
```
curl -XPUT http://localhost:9200/_template/main_template?pretty -d '
{
    "template": "*",
    "order": 1,
    "settings": {
        "index.number_of_replicas": 0
    },
    "mappings": {
        "_default": {
            "_source": {
                "enabled": false
            }
        }
    }
}'
```
위와 같이 템플릿을 지정하면 생성되는 모든 색인은 레플리카가 없으며 원본도 저장하지 않는다  
템플릿은 파일로 저장 가능하며 config/templates 디렉터리에 위치해야 한다  


동적 템플릿  
```
{
    "mapping": {
        "article": {
            "dynamic_templates": {
                "match": "*",    # 필드 이름이 패턴과 일치할때 해당 템플릿 사용, *이라 모든 필드에 적용됨, unmatch 값도 사용가능(필드 이름이 패턴과 일치하지 않을때 해당 펨플릿 사용)
                "mapping": {
                    "type": "string",
                    "index": "analyzed",
                    "fields": {
                        "str": { "type": "{dynamic_type}", "index": "not_analyzed" }
                    }
                }
            }
        }
    }
}
```
각 필드는 복수 값 필드로 취급되며 첫번째 필드 타입은 문자열, 두번째 필드는 엘라스틱서치가 타입을 판단하는 article 타입을 위핸 매핑정의  
glob 패턴을 사용하며 match_pattern=regexp 를 사용해 패턴 변경 가능 (정규표현식 사용가능)  
중첩된 다큐먼트 이름이 일치하는지 path_match, path_unmatch를 사용하여 정의 가능  


스냅샷  
생성한 시점의 해당 클러스터 관련 자료, 색인에대한 정보 보존  
name: 저장소 이름  
type: 저장소 타입(fs, url)  
settings: 타입을 fs로 설정했을 경우 location, url로 설정했을 경우 url 추가 정보 필요  
```
# 스냅샷 저장소 생성
curl -XPUT localhost:9200/_snapshot/backup -d '
{
    "type": "fs",
    "settings": {
        "location": "/tmp/es_backup_folder/cluster1"
    }
}'

# 스냅샷 생성
curl -XPUT localhost:9200/_snapshot/backup/bckp1?wait_for_completion=true&pretty
스냅샷 생성 후 응답을 받기 위해 wait_for_completion 매개변수 사용
```
> curl -XGET localhost:9200/_snapshot/backup?pretty  
저장소 정의 확인  
> curl -XGET localhost:9200/_snapshot/_all?pretty  
모든 저장소 점검  
> curl -XDELETE localhost:9200/_snapshot/backup?pretty  
스냅샷 저장소 삭제  


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


클러스터 상태 API  
> curl 'localhost:9200/_cluster/health?pretty'  
> curl 'localhost:9200/_cluster/health/test1,test2?pretty'  
녹색 상태: 모든 샤드가 적절히 할당되었으며 오류가 없음  
황색 상태: 요청을 처리할 준비가 되있음, 주 샤드가 할당되었지만 몇몇 레플리카는 할당되지 않은 상태  
적색 상태: 주 샤드 하나가 할당되지 않았음, 클러스터가 아직 준비되지 않은 상태  


> curl 'localhost:9200/_cluster/health?level=indices'  
클러스터 정보 외에 색인별 상태 정보 확인 가능  
> curl 'localhost:9200/_cluster/health?level=shards'  
샤드별 정보 확인 가능  


> curl 'localhost:9200/_cluster/health?timeout=30s'  
해당 명령이 최대 30초 동안 기다려 응답 반환  
> curl 'localhost:9200/_cluster/health?wait_for_status=yellow'  
상태 api 호출을 클러스터 상태가 yello 상태가 될때 결과 반환  
> curl 'localhost:9200/_cluster/health?wait_for_nodes>=3'  
응답을 반환하기 위해 필요한 노드 수 설정, 노드가 3개 이상일 때  
> curl 'localhost:9200/_cluster/health?wait_for_relocating_shard=0'  
엘라스틱서치에 기다려야 하는 재배치 중인 샤드 수를 알려준다, 0으로 설정하면 재배치 중인 모든 샤드를 기다린다  

> curl localhost:9200/library,map/_stats?pretty  
응답 결과 구조  
map, library 의 색인에 대한 통계 확인,  
indices(색인에 대한 정보)  
- docs: 다큐먼트 count, deleted 정보, 세그먼트 병합과정에서 다큐먼트가 물리적으로 삭제 그전까지 deleted에 카운팅  
- store: 색인크기, 속도 조절 통계 등의 정보  
- indexing: 색인과 삭제 자료 처리 정보  
- get: 실시간 get 정보, 성공하지 못한 전체 결과 수 등  
- search: 검색 정보  
- merges: 루씬 세그먼트 병합에 대한 정보  
- refresh: 새로 고침 연산에 대한 정보  
- flush: 플러시에 대한 정보  
- warmer: 색인 미리 채우기와 수행 시간에 대한 정보  
- filter_cache: 필터 캐시 통계  
- id_cache: 식별자 캐시 통계  
- fielddata: 필드 자료 캐시 통계  
- percolate: 예상 검색에 대한 정보  
- completion: 자동완성 기능에 대한 정보  
- segments: 루씬 세그먼트에 대한 정보  
- translog: 트랜잭션 로그 수와 크기에 대한 정보  
primaries(노드에 할당된 주 샤드 정보),  
total(레플리카를 포함한 모든 샤드에 대한 정보)  


노드정보 API  
- 노드 이름: /_nodes/노드명
- 노드 식별자: /_nodes/식별자코드
- ip 주소: /_nodes/ip주소
- 설정된 프로퍼티로 조회: /_nodes/프로퍼티명:값
- 패턴: /_nodes/패턴이들어간문자열
- 노드열거: /_nodes/노드1,노드2
- 패턴과 노드열거: /_nodes/노드*
- 엘라스틱서치 구성: /_nodes/노드명/settings
- 서버 정보: /_nodes/노드명/os
- 프로세스 식별자와 사용 가능한 파일 기술자: /_nodes/노드명/process
- 자바 가상기계 정보: /_nodes/노드명/jvm
- 스레드 풀 구성: /_nodes/노드명/thread_pool
- 네트워크 인터페이스 정보: /_nodes/노드명/network
- transport 모듈과 관련 정보: /_nodes/노드명/transport
- http와 관련된 정보: /_nodes/노드명/http
- 플러그인 정보: /_nodes/노드명/plugins
- 노드 상태: /_nodes/노드명/stats
- 크기, 다큐먼트수, 색인관련 통계: /_nodes/노드명/stats/indices
- 운영체제 관련 통계 정보: /_nodes/노드명/stats/os
- 메모리, CPU 등 통계 정보: /_nodes/노드명/stats/process
- 자바 가상기계 통계 정보: /_nodes/노드명/stats/jvm
- tcp 수준의 통계 정보: /_nodes/노드명/stats/network
- transport 모듈이 송수신하는 자료 통계 정보: /_nodes/노드명/stats/transport
- http연결에 대한 통계 정보: /_nodes/노드명/stats/http
- 디스크 공간과 I/O연산 관련 통계 정보: /_nodes/노드명/stats/fs
- 스레드 풀 상태 통계 정보: /_nodes/노드명/stats/thread_pool
- 필드 자료 캐시의 회로 차단기에 대한 통계 정보: /_nodes/노드명/stats/breaker


클러스터 상태 API  
> curl 'localhost:9200/_cluster/state?pretty'  
- /_cluster/state/version: 클러스터 버전 정보
- /_cluster/state/master_node: 마스터 노드 정보
- /_cluster/state/nodes: 노드에 대한 정보
- /_cluster/state/routing_table: 라우팅에 대한 정보
- /_cluster/state/metadata: 메타데이터에 대한 정보
- /_cluster/state/blocks: 응답의 blocks 부분을 반환
- /_cluster/pending_tasks: 실행을 기다리는 과업을 점검
- /_cluster/segments: 전체 클러스터나 개별 색인 모두에 사용, 물리적인 색인과 연결된 세그먼트에 대한 정보  


cat API  
> curl -XGET 'localhost:9200/_cat/shards?v'  
- /_cat/aliases: 앨리어스 정보
- /_cat/aliases/앨리어스명: 지정한 앨리어스 정보
- /_cat/allocation: 할당된 샤드와 디스크 사용량 정보
- /_cat/count: 모든 색인이나 개별 색인에 대한 다큐먼트 수
- /_cat/count/색인명: 지정한 색인에 대한 다큐먼트 수
- /_cat/health: 클러스터 상태
- /_cat/indices: 색인에 대한 정보
- /_cat/indices/색인명: 지정한 색인에 대한 정보
- /_cat/master: 마스터 노드 정보
- /_cat/nodes: 클러스터 토플로지 cluster topology 정보
- /_cat/pending_tasks: 실행을 위해 기다리고 있는 작업 정보
- /_cat/recovery: 복구 과정에 대한 진행 상황
- /_cat/thread_pool: 스레드 풀과 관련해 클러스터 범우 통계
- /_cat/shards: 샤드에 대한 정보
- /_cat/shards/색인명: 지정한 색인의 샤드에 대한 정보


* 엘라스틱서치는 기본적으로 클러스터 상태가 바뀌어 균형잡기(rebalancing)가 필요하다 생각할때 클러스터의 특정 노드에서 다른 노드로 샤드를 이동한다.

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


색인 미리 채우기  
```
curl -XPUT 'localhost:9200/부모인색인명/색인명/_warmer/질의이름' -d '{
    "query": {
        "match_all": {}
    },
    "facets" {
        "warming_facet": {
            "terms": {
                "field": "tags"
            }
        }
    }
}'
```


미리 채운 색인 질의 인출  
```
curl -XGET 'localhost:9200/색인명/_warmer/질의이름?pretty=true'
curl -XGET 'localhost:9200/색인명/_warmer'
curl -XGET 'localhost:9200/색인명/_warmer/tags*'
```


미리 채운 색인 질의 삭제  
```
curl -XDELETE 'localhost:9200/색인명/_warmer/질의이름'
curl -XDELETE 'localhost:9200/색인명/_warmer/_all'
curl -XDELETE 'localhost:9200/색인명/_warmer/tags*'
```


미리 채운 색인 질의 비활성화  
> curl -XPUT 'localhost:9200/색인명/_settings' -d '{ "index.warmer.enabled": false }'  


실행된 질의 정보 로그로 확인  
1. elasticsearch.yml 파일에 아래 내용 입력  
index.search.slowlog.threshold.query.warn: 10s  
index.search.slowlog.threshold.query.info: 5s  
index.search.slowlog.threshold.query.debug: 2s  
index.search.slowlog.threshold.query.trace: 1s  
2. logging.yml 파일에 아래 내용 입력  
logger:  
 index.search.slowlog: TRACE, index_search_slow_log_file  


앨리어스  
```
# 생성
curl -XPOST 'localhost:9200/_aliases' -d '{
    "actions": [
        { "add": { "index": "day10", "alias": "week12" } },
        { "add": { "index": "day11", "alias": "week12" } },
        { "add": { "index": "day12", "alias": "week12" } },
    ]
}'

# 검색
curl -XGET 'localhost:9200/day10,day11,day12/_search?q=test'

# 변경
curl -XPOST 'localhost:9200/_aliases' -d '{
    "actions": [
        { "remove": { "index": "day9", "alias": "week12" } }
    ]
}'

# 명령 결합
curl -XPOST 'localhost:9200/_aliases' -d '{
    "actions": [
        { "add": { "index": "day10", "alias": "week12" } },
        { "remove": { "index": "day9", "alias": "week12" } }
    ]
}'

# 모든 앨리어스 조회
curl -XGET 'localhost:9200/day10/_aliases'
curl -XGET 'localhost:9200/_aliases'

# 삭제
curl -XDELETE 'localhost:9200/data/_alias/client'

# 필터링
curl -XPOST 'localhost:9200/_aliases' -d '{
    "actions": [
        { "add": {
            "index": "data",
            "alias": "client",
            "filter": { "term": { "clientId": "123" } }
        } }
    ]
}'

# 라우팅
curl -XPOST 'localhost:9200/_aliases' -d '{
    "actions": [
        { "add": {
            "index": "data",
            "alias": "client",
            "search_routing": "12345,12346,12347",
            "index_routing": "12345"
        } }
    ]
}'

# 설정된 라우팅값 확인을 위한 조회
curl -XGET 'localhost:9200/client/_search?q=test&routing=99999,12345'
```


플러그인  
[https://www.elastic.co/guide/en/elasticsearch/plugins/current/plugin-management.html](https://www.elastic.co/guide/en/elasticsearch/plugins/current/plugin-management.html)  
> bin/plugin -install elasticsearch/elasticsearch-lang-javascript/2.0.0.RC1  
> bin/plugin -install lang-javascript -url file:///tmp/elasticsearch-lang-javascript-2.0.0.RC1.zip  


자바 플러그인  
es-plugin.properties가 담긴 .jar파일 포함  
내장 http 서버가 사용할 사이트 부문을 포함 가능, _site 디렉터리 아래 위치해야 함  


사이트 플러그인 (/_plugin/_plugin_name)  
내장 http 서버가 제공하는 파일 집합으로 취급, 엘라스틱 동작 방식에 어떤 변경도 없음  


플러그인 제거  
> bin/plugin -remove river-mongodb  


설정 갱신  
재시동 할때까지만 유효한 설정  
```
curl -XPUT 'localost:9200/_cluster/settings' -d '{
    "transient": { #임시 프로퍼티 설정, 영구=permanent
        "PROPERTY_NAME": "PROPERTY_VALUE"
    }
}'
```

TF-IDF 유사도 알고리즘  
오타 처리: fuzzy 질의  
게이트웨이: 데이터를 디스크에 기록해서 노드가 내려가도 데이터를 잃지 않도록 하는 elasticsearch 구성 요소  

수평적 확장: 클러스터에 노드 추가  
수직적 확장: 노드에 자원 추가(CPU,메모리 등)  

ElasticSearch head  
ElasticSearch 플러그인, 어떻게 샤드가 클러스터에 분산되는지 보여줌  
Chrome 확장 프로그램이나 Elasticsearch 플러그인(버전 5부터는 불가능)으로 실행되지 않을 때 Elasticsearch 에서 CORS 를 활성화해야 합니다. 그렇지 않으면 브라우저가 동일 출처 정책 위반으로 인해 elasticsearch-head의 요청을 거부합니다.  

kopf  
ElasticSearch API의 모든 잠재력을 탐색할 수 있는 REST 클라이언트를 제공  
Kopf는 더 이상 유지되지 않습니다. 대체품(cerebro)이 개발되었으며 현재 https://github.com/lmenezes/cerebro 에서 유지 관리되고 있습니다 .  

marvel  
ElasticSearch에서 제공하는 모니터링 관련 플러그인. (유료)  

색인 샘플  
https://github.com/dakrone/elasticsearch-in-action  

윈도우  
https://cygwin.com/  
curl, git 추가  

압축 해제    rpm, deb 설치  
config/ /etc/elasticsearch/  
logs/ /var/log/elasticsearch/  

메인 로그(cluster-name.log): ElasticSearch가 동작 중일 때 무슨 일이 일어났는지에 관한 일반적인 정보를 알 수 있다  
느린 검색 로그(cluster-name_index_search_slowlog.log): 쿼리가 너무 느리게 실행될때 로그를 남긴다, 기본으로 0.5초 넘게 걸리면 로그  
느린 색인 로그(cluster-name_index_indexing_slowlog.log): 색인 작업이 0.5초 이상 걸리면 로그  

모든 로그를 남기고 싶다면 logging.yml 파일에 "rootLogger: TRACE, console, file" 입력  

ElasticSearch에 의해 사용하는 메모리는 힙이라고 부른다  
기본설정: 256MB 할당해서 1GB까지 확장, 실 서비스에서는 전체 RAM의 절반을 할당  
기본값 변경  
유닉스: export ES_HEAP_SIZE=500m; bin/elasticsearch  
윈도우: SET ES_HEAP_SIZE=500m & bin/elasticsearch.bat  
영구적으로 변경  
유닉스: bin/elasticsearch.in.sh 파일에서 #!/bin/sh 이후에 ES_HEAP_SIZE 추가  
윈도우: bin/elasticsearch.bat 파일에서 #!/bin/sh 이후에 ES_HEAP_SIZE 추가  

배열 필드  
단일 필드를 다중 필드 설정으로 데이터 재색인 없이 업그레이드 할 수 있다  
다중 필드를 단일로 변경할 수 없다  
한 번 지정하면 매핑에서 서브 필드를 삭제할 수 없다  

동시성 제어 방법  
- 낙관적 잠금(Optimistic locking) : 병렬 작업 허용, 드물게 나타나는 충돌 추정  
- 비관적 잠금(Pessimistic locking) : 충돌을 야기하는 작업을 막으면서 방지  

retry_on_conflict 파라미터  
어플리케이션 도움 없이 자동으로 일래스틱서치가 재시도  


범위 검색을 사용할때 바이너리 매치("이 도큐먼트는 범위 내에 있다 또는 이 도큐먼트는 범위 내에 없다")를 가지는 범위 쿼리로 들어간 도큐먼트는  
범위 쿼리가 되는데 필요하지 않기 때문에 쿼리로 만들지 필터로 만들지 확실치 않다면 필터로 만들자.  

_termvector 종단점  
색인된 텀에 대한 정보, 도큐먼트와 색인에 얼마나 있는지, 도큐먼트 내 어디에 있는지 확인  

소문자화, 스태밍(stemming), 언어 특화(language-specific), 동의어  

내장 분석기  
- 표준 분석기, 단순 분석기, 화이트스페이스, 불용어, 키워드, 패턴, 언어 및 다국어, 스노우볼  

토큰화  
- 표준 토크나이저, 키워드, 문자, 소문자화, 화이트스페이스, 패턴, UAX URL EMAIL, 경로 계층  

토큰 필터  
- 표준, 소문자화, 길이, 불용어, TRUNCATE, TRIM, LIMIT TOKEN COUNT, 반전, 유일성, ASCII FOLDING, 동의어  

Ngram  
토큰의 각 단어 부분을 다중 서브 토큰으로 분해하는 방식  
1-gram, Bigram, Trigram, min_gram, max_gram  

edge ngram  
ngram 분해의 변종, 앞부분 부터 만든다  

shingle  
문자 수준이 아닌 토큰 수준에서 ngram  

스태밍(Stemming)  
단어를 단어의 원형이나 어근으로 줄인다  

스태밍 알고리즘  
snowball 필터, porter_stem 필터, kstem 필터  

스태밍 사전  
Hunspell 분석기, 사전 파일은 elasticsearch.yml 파일이 있는 같은 디렉터리에 hunspell 이라는 디렉터리에 있어야 한다  
hunspell 디렉터리 내부에 개별 언어 사전은 locale 명으로 폴더를 만들어 넣어야 한다  


필드 데이터 캐시  
elasticsearch가 색인 문서들에서 필드의 값을 저장하는 인 메모리 캐시이고 정렬, 스크립팅 혹은 필드들 안의 값들을 집계할 때 사용  
데이터를 압축하는 방식으로 적재  
필드 정렬이나 집합에 수행시 fielddata 매핑에 loading을 eager로 설정  

필드 데이터 관리  
- 메모리 제한: indices.fielddata.cache.size, indices.fielddata.cache.expire 설정
- 서킷 브레이커 circuit breaker에 필드 데이터 사용
- doc values로 메모리값 무시: 색인 데이터와 함께 디스크에 저장하고 그 값을 사용, 매핑 속성에 "doc_values:true" 설정

TF-IDF(TF=term frequency / 단어빈도, IDF=inverse document frequency /역 문서 빈도)  
하나의 문서에서 단어가 자주 반복된다면 관련성은 높아진다, 전체 문서에서 단어가 자주 반복된다면 관련성은 낮아진다  

기본 점수 계산 함수  
조합 인자 coordination factor: 얼마나 많은 문서에서 찾았는지, 얼마나 많은 단어를 찾았는지  
질의 표준화 query normalization: 질의 결과 비교  

TF-DIF 외 점수방법  
- Okapi BM25
--- 사전의 각 용어에 일치하는 값의 배열에 각 문서를 대응하고, 문서의 순위 결정. k1, b, discount_overlaps 설정이 있음
--- k1은 숫자형으로 단어 빈도가 점수에 얼마나 중요한지, 기본값은 1.2
--- b는 0-1 사이의 숫자형으로 문서의 길이가 점수에 미치는 영향도, 기본값은 0.75
--- discount_overlaps는 여러 토큰이 같은 장소에서 발생할 경우 필드에 영향을 미칠지와 어떻게 길이를 정규화 할지 여부, 기본값은 true
- Divergence from randomness 혹은 DFR similatity
- Information based 혹은 IB similarity
- LM Dirichlet similarity
- LM Jelinek Mercer similarity

부스팅(boosting)  
문서의 관련성을 수정하는 절차. 문서를 색인할때 부스팅, 문서를 질의할때 부스팅이 있다  
부스팅하는 값은 절대값이 아닌 상대값이다.  

explain API  
점수 설명하기, "explain=true" 설정을 url이나 전송 내용에 지정  

질의 리스코어링(rescoring)  
초기 질의가 실행되고 응답받은 결과를 가지고 점수를 계산하는 것  

function_score 질의  
임의의 함수에 숫자로 점수를 지정하여 초기 질의에 맞는 문서의 점수를 결정하는데 세부적으로 조절할 수 있다  
- 가중치: 점수에 일정한 수를 곱한다
- 점수 결합: score_mode(개별 함수를 어떤 방법으로 결합할지), boost_mode(원본 질의 점수와 function 점수를 어떻게 결합할지)
- field_value_factor: 숫자 필드를 포함하는 필드의 이름을 받고 추가로 점수에 곱할 상수값을 받아 적용
- 스크립트: Groovy 언어로 쓰여 있고 _score 를 사용하여 문서의 원래 접수에 접근 가능
- random: 무작위 점수를 문서에 할당
- decay 함수: 특정 필드를 기준으로 점수를 점진적으로 줄여준다

지표 집계 타입  
- stats: 통계 수집, 얼마나 많은 데이터가 있는지 최소, 최대 평균 값 등을 구함
- individual stats(min, max, sum, avg, value_count)
- extended_stats: 성격 검사 결과 포함, 분산과 표준편차를 이용하여 유사한 성격을 그룹지어 통계 수집
- percentiles: 백분위 집계
- percentile_ranks
- cardinality: 유일한 사용자 수를 찾을 때

메트릭 집계(Metrics Aggregations)  
특정 필드에 대해 합이나 평균을 계산하거나 다른 집계와 중첩하여 결과에 대해 특정 필드의 _score값에 따라 정렬 등의 계산, 다양한 집계 수행  

다중 버킷 집계  
- 텀즈 집계 Terms aggergations
- 범위 집계
- 히스토그램 Histogram
- 중첩 역 중첩
- 지오 거리, 지오 해시 그리드

파이프라인 집계(Pipeline Aggregations)  
다른 집계로 생성된 버킷을 참조해서 집계 수행  

형제 집계(Sibling Aggregations)  
동일 선상의 위치에서 수행되는 새 집계  

부모 집계(Parent Aggregations)  
집계를 통해 생성된 버킷을 사용해계산을 수행하고 그 결과를 기존 집계에 반영  

top_hits 집계  
하위 집계의 결과를 묶어서 보여줌  

significant_terms 집계  
전체 색인보다 질의 결과에 의해 자주 나타나는 단어 반환  

도큐먼트 간 관계 정의 옵션  
- 개체 타입
- 중첩 도큐먼트
- 도큐먼트 간 부모 자식 관계
- 비정규화
- 애플리케이션측 조인


Nested 타입 옵션  
- include_in_parent:true
-- 인접한 부모 도큐먼트에 하나의 중첩 도큐먼트 필드를 색인해 넣는 옵션
- include_in_root:true
-- 중첩 매핑에 추가하면 내부 개체를 두 번 색인한다 (중첩 도큐먼트에서 한번, 루트 도큐먼트에서 한번)
- score_mode:avg
-- 일치하는 중첩 도큐먼트 개수의 평균 점수
- score_mode:total
-- 중첩 도큐먼트 점수를 요약
- score_mode:max
-- 중첩 도큐먼트 점수의 최댓값
- score_mode:none
-- 어떠한 점수도 총점에 대해 산출하지 않는다
- inner_hits: { from: 0, size:1 }
-- 중첩 도큐먼트 중 어느 것이 명시한 중첩 쿼리로 일치했는지 확인


멀티캐스트 디스커버리  
클러스터에 추가되는 노드들의 ip가 자주 바뀌는 경우처럼 같은 네트워크 내에 위치하고 있는 매우 유동적인 클러스터를 사용하는 경우 유용  

유니캐스트 디스커버리  
접속할 대상이 되는 일래스틱서치 호스트 목록을 사용, 노드의 ip주소가 자주 변경되지 않거나 네트워크 전체가 아닌 특정 노드들끼리만 서로 통신하여야 하는 경우 유용  

마스터 노드  
클러스터의 상태(현재 설정값, 샤드, 색인, 노드들의 상태)를 관리하는 역할  


노드 해제  
노드를 먼저 해체하여 해당 노드에 있는 모든 샤드를 클러스터의 다른 노드로 옮긴다  
```
curl -XPUT localhost:9200/_cluster/settings -d '{
    "transient": {
	    "cluster.routing.allocation.exclude._ip": "ip주소.."
	}
}'
```

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

주와 레플리카 존재하는 세그먼트 파일을 동일한 것으로 만들기 위한 방법  
1. 주와 레플리카 샤드 몯에서 하나의 큰 세그먼트 파일을 만들어내기 위한 optimize API 사용
2. 레플리카 수를 0으로 변경했다가 다시 큰 수로 변경

스케일 아웃  
- 클러스터에 노드 추가
- 노드 업그레이드

스케일링 전략  
- 오버 샤딩: 미리 감안하여 색인에 대해 의도적으로 다수의 샤드 생성
- 데이터를 색인과 샤드에 분할하기
- 처리량 극대화 하기(예: 색인이 진행되는 동안 레플리카 수를 줄이고 색인 작업이 종료되면 레플리카 수를 늘린다)

엘리어스  
하나 이상의 실제 색인을 지칭하기 위해 사용할 수 있는 포인터 혹은 이름.  
클러스터 상태 정보 내에 존재하며 마스터 노드에 의해서 관리된다.  
클러스터 상태 맵 자료구조에 존재하는 색인에 매핑시키기 위한 추가적인 키가 오버헤드 된다.  
재색인 관점에서 유연성 제공  

멀티서치  
"_msearch" 종단점 사용  

멀티겟  
다수의 문서 조회, "mget" 종단점 사용  

리프레시 주기 설정  
```
curl -XPUT localhost:9200/인덱스명/settings -d '{
  "index.refresh_interval": "5s"    // -1로 설정시 리프레시 비활성화
}'
```

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

워머  
캐시 준비, 레프레시 작업 수행시마다 elasticsearch가 쿼리를 수행하도록 도와준다, 처음 실행될때 너무 느린데 색인이 느려지는 것은 크게 문제 없고 검색 속도가 빨라야 한다면 색인 워머 사용 추천.  
- 워머목록 확인: curl localhost:9200/인덱스명/_warmer?
- 워머 추가, 삭제 가능, 워머가 많으면 리프레시 느림

* 분석 필드.  
index_options:positions 기본값, 위치 저장  
index_options:freqs 위치 색인 비활성화  
index_options:docs 빈도 색인 X

search_type  
- query_then_fetch: 전체 샤드의 검색이 모두 수행된 후 결과 출력, 전체 취합된 결과를 size 매개변수에서 지정한 수만큼 출력
- query_and_fetch: 샤드별로 검색되는대로 결과 출력, size * 샤드 갯수만큼 출력
- dfs_query_then_fetch: query_then_fetch와 같으나 정확한 스코어링을 위해 검색어들을 사전 처리
- dfs_query_and_fetch: query_and_fetch와 같으나 정확한 스코어링을 위해 검색어들을 사전 처리

* 색인의 크기가 커져도 괜찮다면 퍼지/와일드카드/구문 쿼리 대신 엔그램과 싱글을 사용하면 검색이 빨라짐
* 스크립트 사용 보다 색인 시 새로운 필드를 추가하여 검색 속도 향상
* 스크립트가 자주 변경되지 않으면 elasticsearch 플러그인 형태로 네이티브 스크립트 작성
* 샤드별로 문서 빈도가 불균형하면 dfs_query_then_fetch 사용
* 많은 문서를 탐색해야 한다면 스캔 검색 사용

색인 템플릿  
데이터 저장 형태가 반복되서 나타나는 것이 확인된 경우 사용  
- REST API, 설정 파일로 생성
- 복수 템플릿 병합 가능
- 설정시 order 속성이 적은 숫자면 먼저 적용, 높은 숫자면 오버라이딩 한다
- 템플릿 조회. curl localhost:9200/_template

템플릿 파일 시스템에서 설정하고 싶을때  
- 템플릿 설정은 json 포맷이어야 한다
- 다템플릿 정의는 elasticsearch 설정이 있는 위치의 templates 디렉토리에 있어야 한다(elasticsearch.yml path.conf 위치)
- 템플릿 정의는 마스터로 선출될 가능성이 있는 노드의 디렉토리에 있어야 한다

동적 매핑 비활성화  
elasticsearch.yml  index.mapper.dynamic:false  

할당 인식 Allocation Awareness  
데이터의 복제본이 어디에 위치할지에 대한 인지 상태, 클러스터의 위상 topology 설계.  
- 샤드 기반 할당: "cluster.routing.allocation.awareness.attributes:rack"로 설정, "node.rack:1" 메타데이터 키가 할당 인식 파라미터
- 강제 할당 인식: "cluster.routing.allocation.awareness.attributes:zone", "cluster.routing.allocation.force.zone.values:us-east,us-west"로 설정

클러스터 상태 확인  
- curl localhost:9200/_cluster/health
- relocting_shards: 샤드를 클러스터의 다른 노드로 이동시키는 수
- initializing_shards: 색인을 새로 생성하거나 노드를 재시작 하는 수
- unassigned_shards: 할당되지 않는 샤드 수
- level 파라미터 추가 시 문제를 디테일하게 확인 가능

느린작업 확인 로그  
- 슬로우 로그
-- 인덱스명/_settings
-- index.search.slowlog.threshold.query.warn: 10s
-- index.search.slowlog.threshold.query.info: 1s
-- index.search.slowlog.threshold.query.debug: 2s
-- index.search.slowlog.threshold.query.trace: 500ms
- 슬로우 색인 로그
-- 인덱스명/_settings
-- index.search.slowlog.threshold.fetch.warn: 1s
-- index.search.slowlog.threshold.fetch.info: 1s
-- index.search.slowlog.threshold.fetch.debug: 500ms
-- index.search.slowlog.threshold.fetch.trace: 200ms

핫 스레드 HOT_THREADS API  
특정 프로세스가 블락되어 문제를 일으키고 있는것을 확인가능  
curl localhost:9200/_nodes/hot_threads  
- type 파라미터: cpu,wait,block 중 하나 스냅샷의 대상이 되는 스레드 상태
- interval 파라미터: 기본값 500ms, 첫 번째와 두번째 사이 대기 시간
- threads 파라미터: 몇 개의 상위 핫스레드를 보여줄 것인가


스레드 풀 종류  
- bulk: 기본값은 가용 프로세서 수
- index: 기본값은 가용 프로세서 수
- search: 기본값은 가용 프로세서 수 * 3

elasticsearch.yml 벌크 스레드 풀 설정  
threadpool.bulk.type: fixed / cache
threadpool.bulk.size: 40
threadpool.bulk.queue_size: 200

가비지 컬렉터  
메모리 부족시 실행, 레퍼런스가 끊긴 객체들 삭제 JVM 어플리케이션이 사용할 수 있도록 메모리 확보  

필터 캐시  
쿼리 결과 메모리에 저장, "index.cache.filter.typed:node", "index.cache.filter.size: 20%"로 설정  
- 색인 수준 (거의 사용 안함)
- 노드 수준

필드 캐시  
필드 값 메모리에 저장, "indices.fielddata.cache.size: 20%"로 설정  
- 노드별 현재 상태 조회: curl localhost:9200/_nodes/stats/indices/fielddata?fields=*&pretty=1
- 색인별 현재 상태 조회: curl localhost:9200/_stats/fielddata?fields=*&pretty=1
- 노드별 색인별 현재 상태 조회: curl localhost:9200/_nodes/stats/indices/fielddata?level=indices&fields=*&pretty=1

질의의 결과 캐시  
indices.requests.cache.size: 1%(기본값)  

노드의 모든 샤드가 공유하는 캐시  
index.queries.chche.enabled: true  


서킷 브레이커  
메모리 부족 에러 방지를 위해 인위적인 임계치  
- indices.breaker.total.limit: 기본값은 힙의 70%
- indices.breaker.fielddata.limit: 기본값은 힙의 60%
- indices.breaker.request.limit: 기본값은 힙의 40%

스왑 방지  
- bootstrap.mlockall:true 설정
- 설정확인 curl localhost:9200/_nodes/process


개별 노드의 설정을 사용하여 특정 태그값을 가진 노드에만 위치할 색인을 생성하여 더 많은 힙 이외의 메모리를 가진 노드에 위치, 색인에 대한 응답시간 향상  
"index.routing.allocation.include.tag: 태그명1,태그명2..."  

I/O가 사용할 노드 수준 제한  
indices.store.throttle.type: none(노드전체)/merge(머지할때제한)/all(모든 노드 모든 샤드에 대한 작업 제한 한계점 적용)  
index.store.throttle.type: node(노드전체)/merge(머지할때제한)/all(모든 노드 모든 샤드에 대한 색인 작업 제한 한계점 적용)  
노드 수준 설정의 경우 "indices.store.throttle.max_bytes_per_sec: 50mb" 사용(초당 메가바이트),  
색인 수준 설정의 경우 index.store.throttle.max_bytes_per_sec 사용  

데이터 백업  
- 스냅샷 API: 클러스터 상태, 데이터 복사, 논블로킹 작업
- 스냅샷 API를 이용해 공유 파일 시스템으로 백업
```
# 저장소 정의
curl -XPUT localhost:9200/_snapshot/저장소이름 -d '{
  "type": "fs",
  "settings": {
    "location": "저장소 네트워크상 위치",
	"compress": true,
	"max_snapshot_bytes_per_sec": "20mb",
	"max_restore_bytes_per_sec": "20mb"
  }
}'

#첫번째 스냅샷 실행
curl -XPUT localhost:9200/_snapshot/저장소이름/first_snapshot
#첫번째 스냅샷 실행(스냅샷이 종료된 후까지 기다린 후 요청 반환)
curl -XPUT localhost:9200/_snapshot/저장소이름/first_snapshot?wait_for_completion=true
#두번째 스냅샷 실행
curl -XPUT localhost:9200/_snapshot/저장소이름/second_snapshot

#복원하기
curl -XPOST localhost:9200/_snapshot/저장소이름/first_snapshot/_restore
#복원하기 (복원하기가 종료된 후까지 기다린 후 요청 반환)
curl -XPOST localhost:9200/_snapshot/저장소이름/first_snapshot/_restore?wait_for_completion=true

#삭제하기
curl -XDELETE localhost:9200/_snapshot/저장소이름/first_snapshot
```

- 저장소 플러그인: 아마존 S3, 하둡 HDFS

거리 순 정렬
```
curl localhost:9200/인덱스명/_search -d '{
  "query": {
    "match": {
	  "title": "elasticsearch"
	}
  },
  "sort": [
    {
	  "_geo_distance": {
	    "location.geolocation": "40,-105",
		"order": "asc",
		"unit": "km",
		"mode": "avg" #min/max/avg 선택가능
	  }
	}
  ]
}'
```

점수 계산할 때 거리 고려하기 (거리가 멀어질수록 점수 낮춤)  
```
curl localhost:9200/인덱스명/_search? -d '{
  "query": {
    "function_score": {
	  "query" :{
	    "match": {
		  "title": "elasticsearch"
		}
	  }
	},
	"linear": {
	  "location.geolocation": {
	    "origin": "40, -105",
		"scale": "100km",
		"decay": 0.5
	  }
	}
  }
}'
```

특정 위치로부터 범위 안에 있는 결과만 반환  
```
curl localhost:9200/인덱스명/_search -d '{
  "query": {
    "filtered": {
	  "filter": {
	    "geo_distance": {
		  "distance": "50km",
		  "location.geolocation": "40, -105"
		}
	  }
	}
  }
}'

# distance_type 파라미터 추가
sloppy_arc: 두 점 사이의 거리를 원호의 빠른 근사치를 구해서 계산
arc: 원의 호를 계산, sloppy_arc보다 정확
plane: 가장 빠르지만 부정확한 구현

# optimize_bbox 파라미터
none
memory
indexed
```

거리범위 필터 (특정 위치에서 50~100km 사이 검색)  
```
curl localhost:9200/인덱스명/_search -d '{
  "query": {
    "filtered": {
	  "filter": {
	    "geo_distance_range": {
		  "from": "50km",
		  "to": "100km",
		  "location.geolocation": "40, -105"
		}
	  }
	}
  }
}'
```

거리범위 집계  
```
"aggs": {
  "events_ranges": {
    "geo_distance": {
	  "field": "location.geolocation",
	  "origin": "40, -105",
	  "unit": "km",
	  "ranges": [
	    { "to": 100 },
	    { "from": 100, "to": 5000 },
	    { "from": 5000 }
	  ]
	}
  }
}
```

바운딩 박스 필터  
```
curl localhost:9200/인덱스명/_search -d '{
  "query": {
    "filtered": {
	  "filter": {
	    "geo_bouding_box": {   #바운딩박스(직사각형)/다각형/지오해시 모양 선택 가능
		  "location.geolocation": {
		    "top_left": "40, -103",
			"buttom_right": "38,-103"
		  }
		}
	  }
	}
  }
}'

위도와 경도를 별도로 색인하도록 하려면 아래와 같이 설정
"geolocation": { "type": "geo_point", "lat_lon": true }
```

지오해시  
지오 포인트에서 지오해시를 색인하도록 하려면 매핑에서 geohash:true 로 설정,  
그 해시의 부모도 색인하려면 geohash_prefix:true 로 설정  
지오해시 셀의 크기는 precision 옵션을 통해 설정  

모양 색인하기  
```
curl -XPUT localhost:9200/인덱스명/인덱스번호 -d '{
  "area": {
    "type": "polygon",
	"coordinates": [
	  [
	    [45, 30],
	    [46, 30],
	    [45, 31],
	    [46, 32]
	  ]
	]
  }
}'
```

겹치는 모양 걸러내기  
```
curl localhost:9200/인덱스명/_search -d '{
  "query": {
    "filtered": {
	  "filter": {
	    "geo_shape": {
		  "area": {
		    "shape": {
			  "type": "polygon",
			  "coordinates": [
			   [
			     [45, 30.5],
			     [46, 30.5],
			     [45, 31.5],
			     [46, 32.5]
			   ]
			  ]
			}
		  }
		}
	  }
	}
  }
}'
```

사이트 플러그인  
헤드 플러그인 head plugin, 일래스틱서치 코프 kopf, 빅데스크 bigdesk, 일래스틱서치 hq, 왓슨 whatson  


코드 플러그인  
일래스틱서치가 실행하는 JVM 코드를 포함하는 플러그인, AWS 플러그인, ICU분석 플러그인  
aws, zure, elasticsearch-lang-python, elasticsearch-lang-ruby 등(elasticsearch-lang-*)  

마블 플러그인(https://www.elastic.co/kr/downloads/marvel)  

플러그인 설치  
config 폴더가 있는 위치에서 "bin/plugin -install mobz/elasticsearch-head" 명령어 실행  
[https://mobz.github.io/elasticsearch-head/](https://mobz.github.io/elasticsearch-head/)  

설치된 플러그인 확인  
$ bin/plugin --list  

사이트 플러그인 접속  
http://localhost:9200/_plugin/이름  

elasticsearch에 사용 플러그인 명세  
elasticsearch.yml > "plugin.mandatory: analysis-icu,head" 추가  
플러그인 설치 후 바로 시작  

플러그인 제거  
$ bin/plugin --remove 이름  

하이라이팅(type:plain)  
- no_match_size: 프래그먼트가 갖길 원하는 문자 수
- require_field_match:true 쿼리와 일치하는 필드만 하이라이트
- fragment_size: 0으로 설정하면 전체 필드 내용이 보인다, 필드별로 보일 문자 수 조절 가능
- number_of_fragments: 프래그먼트 수 조절, 0으로 설정하면 전체 필드를 하나의 프래그먼트로 변환, fragment_size 무시

하이라이트 리스코어링  
```
curl localhost:9200/인덱스명/_search -d '{
  "query": {
    "match": {
	  "name": "elasticsearch search"
	}
  },
  "rescore": {
    "window_size": 200,
	"query": {
	  "rescore_query": {
	    "wildcard": {
		  "tags.verbatim": "*search"
		}
	  }
	}
  },
  "highlight": {
    "highlight_query": {
	  "query_string": {
	    "query": "name:elasticsearch name:search tags.verbatim:*search"
	  }
	},
	"fields": {
	  "name": {},
	  "tags.verbatim": {}
	}
  }
}'
```

포스팅 하이라이터(type:postings)  
하이라이트할 필드에 index_options를 offsets로 설정하여 색인에 각 필드 위치(포지션과 오프셋)를 저장해야 한다  
모든 일치하는 텀 하이라이팅  
```
curl localhost:9200/_analyze -d '검색어1 검색어2...'
# 각 텀의 오프셋 추출 가능
```

패스트 백터 하이라이터(type:fvh)  
매핑에서 "term_vector:with_positions_offsets" 로 설정  
플레인 하이라이터 처럼 구에 속한 텀들만 하이라이트  
다중 필드 하이라이트 할때 다중 필드를 하나로 합쳐 모든 일치하는 것들을 하이라이트한다  
```
# 다중 필드 하이라이트
"highlight": {
  "fields": {
    "description": { "matched_fields": ["description", "descrption.suffix"]}
  }
}

# 다른 프래그먼트에 다른 태그 사용
# 첫 하이라이트는 b태그 그 다음 태그는 em 태그로 적용된다
"highlight": {
  "fields": {
    "description": {
	  "pre_tags": ["<b>", "<em>"],
	  "post_tags": ["</b>", "</em>"]
	}
  }
}

# 하이라이트 순번 매기기
"highlight": {
  "tags_schema": "styled",
  "fields": {
    "description": {}
  }
}
클래스 이름(hlt숫자)를 이용하여 표시
```

하이라이트 경계 문자 설정  
```
# _very_very_very_very_<em>long</em>_name=1 과 같이 프래그먼트 구분
"highlight": {
  "fields": {
    "description": {
	  "fragment_size": 20,
	  "boundary_chars": ".,!?\t\n_"
	}
  }
}
```

패스트 벡터 하이라이터 일치 수 제한하기  
phrase_limit 파라미터 사용  


모니터링 플러그인  
- 빅데스크 Bigdesk: 클러스터 시각화
- Elastic HQ: 관리와 모니터링을 함께
- 헤드 Head: 고급 쿼리 생성
- 코프 Kopf: 스냅샷, 워머, 퍼컬레이터
- Elasticsearch Marvel: 상세한 분석
- 세마텍스트 SPM: 클라우드와 설치형으로 성능 모니터링, 쿼리, 알람, 이상감지 기능 제공


퍼컬레이터 Percolator  
- 문서 대신 쿼리의 색인을 만든다. 메모리에 쿼리 등록 후 빠르게 실행
- 쿼리 대신 문서를 elasticsearch에 보낸다.(=퍼컬레이트하다) 작은 인 메모리 색인 생성, 등록한 쿼리는 작은 색인에서 실행해서 어떤 쿼리가 일치하는지 알아낸다
- 보통의 쿼리 같은 방식 대신 문서와 일치하는 쿼리 목록을 받는다
- 다중 퍼컬레이트 API로 한 번에 퍼컬레이트 가능

퍼컬레이터 기초
1. 등록한 쿼리가 참조하는 모든 필드에 대해 매핑이 있다는 것을 확인한다
2. 쿼리 자체를 등록한다
3. 문서에 퍼컬레이터를 실행한다

쿼리 등록 취소  
```
curl -XDELETE localhost:9200/인덱스명/.percolator/docid
curl -XPOST localhost:9200/인덱스명/_close
curl -XPOST localhost:9200/인덱스명/_open
close와 open을 해줘야 메모리에 있던 쿼리도 등록 취소된다

curl -XGET localhost:9200/인텍스명/_percolator? -d '{
  "doc": {
    "title": "test"
  }
}'
색인 이름과 id로 식별되는 일치한 쿼리 목록 리턴
```

퍼컬레이터 쿼리 분리하고 필터링  
- 퍼컬레이션을 별도의 색인에 유지, 다른 데이터와 별도로 확장 가능 (특히 별도의 elasticsearch 클러스터에 색인 저장 하면 그렇다)  
각 퍼컬레이션에서 실행하는 쿼리 수를 줄인다  
- 라우팅과 퍼컬레이터 사용: 많은 퍼컬레이션을 실행하는 사용자가 있을때 각 사용자의 쿼리를 하나의 샤드에 유지하도록 한다  
- 등록한 쿼리 필터링

퍼컬레이트한 문서 하이라이팅  
점수는 퍼컬레이터처럼 거꾸로 동작, 퍼컬레이트한 문서가 아닌 쿼리에 점수를 매긴다  
일치한 쿼리의 순서 나열: _score에만 정렬 지원  
일치하는 쿼리의 메타데이터 집계  


퍼컬레이터 성능 팁  
- 요청이나 응답 포맷의 최적화: 기존 문서를 퍼컬레이트하고 하나의 요청에 여러 개의 문서를 퍼컬레이트 하고 전체 ID 목록 대신 일치하는 쿼리의 수만 요청
- 쿼리를 정리하는 방법의 최적화: 쿼리 수를 줄이기 위해 라우팅과 필터링 사용방법 체크
- 퍼컬레이션을 별도의 색인에 유지한다, 다른 데이터와 별도로 확장 가능하다
- 각 퍼컬레이션에서 실행하는 쿼리 수를 줄인다, 라우팅과 필터링을 포함하는 전략이다


##### 자동완성과 검색어 제안
제안자 모듈 Suggesters module  
루씬의 스펠체커 SpellChecker 사용  
- 텀 제안자: 제공한 텍스트의 텀별로 색인하여 키워드 제안
- 구 제안자: 전체 텍스트에 대한 DYM 대안을 제공
- 완성 제안자: 텀의 접두사에 기반을 둬서 자동완성 기능 제공
- 문맥 제안자: 텀(카테고리)이나 지오 포인트 위치에 기반을 둬서 선택 가능한 것들을 필터링하도록 한다

텀 제안자  
- 다수의 제안 (=size) 제공
- sort를 frequency로 변경하여 인기 단어 추출 가능
- suggest_mode로 언제 제안을 보여줄지 설정 가능(missing, popular, always)
- max_edits: 제공한 텀에서 제안될 수 있는 텀까지는 편집 거리
- prefix_length: 단어의 시작을 가정, 접두사 지정
- min_doc_freq: 충분히 대중적인 텀으로 후보 제안 한정
- max_term_freq: 입력 텍스트에서 대중적인 텀이 수정되지 않도록 한다

구 제안자  
텀 제안자에 얼마나 텀들이 색인에 함께 발생하는지에 기반을 둬서 후보 구들에 가중치를 주는 새로운 로직 추가=엔그램 언어 모델 ngram-language model  
스튜피드 백오프 Stupid Backoff 알고리즘 사용  

스무딩 모델 Smoothing Model  
싱글의 빈도 뿐 아니라 싱글의 크기에 의해서도 점수를 준다  

- max_errors: 최대 텀의 수를 정정하는 제안만 허락, 낮을수록 제안 요청 속도 빠른 대응
- confidence: 점수로 필터링하기 위한 주 옵션, 구 제안자가 입력한 텍스트 뿐 아니라 가능한 제안도 점수를 메긴다
- real_word_error_likelihood: 색인 자체에서 철자가 잘못된 단어들의 비율을 기술


자동완성 제안자  
완성과 문맥 제안자는 더 빠른 자동완성을 생성하도록 돕는다. 루씬의 제안 모듈에 구성해서 데이터를 메모리에 유한 상태 변환기 FSTs Finite state Transducers로 유지한다  

색인 시 유사도 개선하기  
index_analyzer와 search_analyzer 옵션을 통해 분석 조정 가능  

검색 시 유사도 개선하기  
fuzziness로 최대 허락하는 편집 거리 명시,  
min_length에 어떤 길이의 입력 텍스트를 퍼지 쿼리하도록 할지 명시  
prefix_length로 첫 번째 문자를 정정하는걸 고려, 유연성 비용에서 성능 개선  


페이로드로 순간 검색 구현  

문맥 제안자  
카테고리(텀)나 지리 위치일 수 있는 context 필터 가능  
- 매핑에 문맥 정의 필요! (문맥은 FST 구조 위에서 동작)
- 문서와 제안 요청에 문맥 추가하기
- 문맥 정의 후 요청에 문액없이 문맥 제안자를 실행하면 에러 발생됨
- 기능 관점에서 필요할 때 문맥 제안자, 필요하지 않을 때는 완성 제안자를 가지고 있는 것처럼 동작

사용자 정의 사전 추가  
```

POST /my_index/_close

PUT /my_index/_settings
{
  "index": {
    "blocks": {
      "read_only_allow_delete": "false"
    },
    "analysis": {
      "analyzer": {
        "korean_analyzer": {
          "filter": [
            "lowercase",
            "synonym"
          ],
          "type": "custom",
          "tokenizer": "korean_tokenizer"
        }
      },
      "filter": {
        "synonym": {
          "type": "synonym",
          "synonyms_path": "userdict_ko.txt",
          "updateable":"true"
        }
      },
      "tokenizer": {
        "korean_tokenizer": {
          "type": "nori_tokenizer",
          "decompound_mode": "mixed"
        }
      }
    },
    "number_of_replicas": "1"
  }
}

PUT /my_index/_settings
{
  "index": {
    "blocks": {
      "read_only_allow_delete": "false"
    },
    "analysis": {
      "analyzer": {
        "korean_analyzer": {
          "filter": [
            "lowercase",
            "synonym"
          ],
          "type": "custom",
          "tokenizer": "korean_tokenizer"
        }
      },
      "filter": {
        "synonym": {
          "type": "synonym",
          "synonyms": ["발품"],
          "updateable":"true"
        }
      },
      "tokenizer": {
        "korean_tokenizer": {
          "type": "nori_tokenizer",
          "decompound_mode": "mixed",
          "user_dictionary_rules": [
            "발품"
          ]
        }
      }
    },
    "number_of_replicas": "1"
  }
}

POST /hug_sharepoint/_open

```



전처리 필터
- Html Strip Char 필터


토크나이저 필터  
- Standard
- WHITESPACE TOKENIZER
- Ngram
- Edge Ngram
- Keyword


토큰 필터
- Ascii Folding
- Lowercase
- Uppercase
- Stop
- Stemmer
- Synonym
- Trim


글로벌 타임아웃 설정  
모든 검색 쿼리에 동일하게 적용되도록 정책으로 설정, 기본값 -1(무제한)  
```
PUT _cluster/settings
{
    "transient": {
        "search.default_search_timeout": "1s"
    }
}
```


질의 결과에대한 스코어 계산확인 -> _explain 사용  
질의를 실행하는 과정에서 각 샤드별로 얼마나 많은 시간이 소요되는지 확인 -> "profile": true 사용   



샤드 크기가 10GB 이상은 되지 않아야 한다  
노드의 최적 수치: 노드의 개수/2 + 1  
node.master = false, node.data = false 의 경우 검색 로드 밸런스(노드에서 데이터를 읽어오고 결과를 집계하는 등등)처럼 역할  

Merge   
시간이 지나면서 작은 세그먼트를 모아 새로운 단편(fragment)에 쓰고 삭제한다, 단편화가 많으면 색인 성능은 떨어진다.  
index.merge.scheduler.max_thread_count 로 머지 스케줄링 설정 가능  

색인 축소   
색인의 샤드 개수 축소  
index.blocks.write: true   
인덱스명/_shrink/reduceindex   
index.blocks.write: false   

데이터 새로고침 주기  
index.refresh_interval  
 
_rollover  
검사할 조건을 정의하고 elasticsearch에 남겨두면 새 색인을 롤링, alias를 이용해서 가상 색인만 참조하게 할 수 있다  

sudo bin/elasticsearch-plugin install ingest-attachment  
첨부 파일 수집 플러그인을 사용하면 Elasticsearch가 Apache 텍스트 추출 라이브러리 Tika 를 사용하여 일반적인 형식(예: PPT, XLS 및 PDF)의 첨부 파일을 추출할 수 있습니다 .  

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
필드 삭제: "script": {"inline": "ctx._source.remove(\"필드명\")"}  
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


작업 목록  
https://www.elastic.co/guide/en/elasticsearch/reference/current/tasks.html  

핫 스레드  
https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-nodes-hot-threads.html  

샤드 할당의 현재 상태  
_cluster/allocation/explain  

캐시 정리  
인덱스명/_cache/clear  

cerebro  
클러스터와 데이터에 대한 개요 제공  
https://github.com/lmenezes/cerebro  

인제스트 파이프라인  
https://m.blog.naver.com/olpaemi/222005459201  

[Logstash] grok 필터, 정규표현식  
https://ksr930.tistory.com/105  

Ingest geoip Processor Plugin  
https://www.elastic.co/guide/en/elasticsearch/plugins/6.8/ingest-geoip.html  

elastic4  
스칼라 클라이언트, 빅데이터 등에 사용   
https://github.com/sksamuel/elastic4s  