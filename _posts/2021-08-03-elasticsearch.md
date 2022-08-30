---
layout: post
---

# Elasticsearch



query

q 매개변수에 필드 지시자가 없을 경우 df 매개변수로 기본 검색 필드를 지정가능
analyzer 프로퍼티는 질의 분석에 사용될 분석기 이름 정의
default_operator 프로퍼티는 질의에 사용될 기본 부울 연산자를 지정 explain 매개변수를 true로 지정하면 결과의 다큐먼트마다 질의 해설 정보를 추가
다큐먼트 마다 색인이름, 타입이름, 다큐먼트 식별자, _source필드 반환


독자적인 정렬 방식을 사용하여 다큐먼트에 대한 점수를 추적할때 track_scores=true 옵션 추가

검색 타임아웃 지정 timeout=5s

결과 윈도우 크기 지정 size=10&from=2 (2번째 결과부터 5개 반환)

검색 타입 search_type 매개변수를 이용해 지정가능 (dfs_query_then_fetch, dfs_query_and_fetch, query_then_fetch, query_and_fetch, count, scan)

확장된 키워드 소문자로 만들기 lowercase_expanded_term=true

와일드카드와 접두어 분석 사용 analyze_wildcard=true

연산자와 증가값을 이용한 중요도 설정 title:book^4

numeric_detection:true 문자열을 숫자로 인지하여 검색 가능

dynamic_date_formats 새 문자열 필드를 검사하여 내용이 에 지정된 날짜 패턴과 일치하는지 확인합니다

dynamic:false 자동 필드 추가 기능 off

타입 속성 문자열, 숫자, 날짜, 부울, 바이너리 설정 가능 index_name: 색인에 저장될 필드 이름, 정의하지 않으면 해당 필드를 정의한 객체 이름으로 설정된다

index: analyzed와 no 값으로 설정 가능, 문자열 필드일 경우 not_analyzed라고 설정 가능, analyzed 필드는 색인되므로 검색 가능한 필드, no로 설정하면 검색 불가능한 필드, not_analyed인 경우 필드는 색인되지만 분석되지는 않아 검색어가 완벽하게 일치해야 결과에 포함된다. index 프로퍼티를 no로 설정하면 해당 필드의 include_in_all 프로퍼티를 비활성화 한다

store: yes로 설정하면 필드의 원본 값을 색인에 기록, no는 기본값으로 원본 내용을 결과에 담아 반환하지 않는다. 색인은 되어 있으므로 이 필드를 대상으로 자료를 검색가능하다

boost: 기본값은 1, 속성값이 높을수록 해당 필드의 값이 중요하다

null_value: 필드가 색인된 다큐먼트의 일부로 포함되지않은 경우 색인에 기록될 값을 명세, 필드 생략이 기본 방식

copy_to: 모든 필드값을 복사할 필드를 명세

include_in_all: 대항 필드가 _all 필드에 반드시 포함되어야 하는지 명세

문자열 속성 term_vector: no(기본값), yes, with_offsets, with_positions, with_positions_offsets 라는 값 설정 가능, 루씬 키워드 백터 계산 방식 정의

omit_norms: 분석될 문자열 필드 false(기본값), 색인되지만 분석되지 않을 문자열 true

analyzer: 색인과 검색을 위해 사용될 분석기 이름 정의, 전역으로 정의된 분석기 이름 사용

index_analyzer: 색인을 위해 사용할 분석기 이름

search_analyzer: 특정 필드로 전송한 질의문자열의 일부를 처리하기 위해 사용할 분석기 이름 정의

norms.enabled: 특정 필드를 위해 norms를 메모리에 올릴지 여부, 기본 true=메모리에 올린다

norms.loading: eager는 해당 필드에 대한 norms가 항상 메모리에 올려져 있음, lazy는 필요할 때만 메모리에 올림

position_offset_gap: 기본값 0, 색인에서 이름이 동일한 필드의 인스턴스 사이에 떨어진 차이 명세

index_options: 키워드를 담은 구조체인 출현 목록을 위한 색인 옵션 정의, docs: 다큐먼트번호만 색인, freqs: 다큐먼트 번호와 키워드빈도 색인, positions: 다큐먼트 번호와 키워드빈도와 위치 색인(기본값)

ignore_above: 필드의 최대 크기를 글자 수로 정의, 크기가 더 큰 값이 들어올 경우 무시된다.

숫자 속성 byte, short, integer, long, float, double precision_step: 필드값에 대해 생성될 키워드 수 명세, 값이 작을 수록 생성되는 키워드 수가 늘어난다, 값 당 키워드 수가 많을수록 색인 비용이 크지만 범위 질의는 빨라진다. 기본값은 4

ignore_malformed: 기본값 false, 형식이 엉망이인 값을 제외하려면 true로 설정

날짜 속성 기본적으로 UTC로 저장 format: 날짜 형식 명세, 기본값은 dateOptionalTime

precision_step: 필드값에 대해 생성될 키워드 수 명세, 값이 작을 수록 생성되는 키워드 수가 늘어난다, 값 당 키워드 수가 많을수록 색인 비용이 크지만 범위 질의는 빨라진다. 기본값은 4

복수 필드 설정

"name" : {
    "type": "string",
    "fields": {
        "subname": { "type": "string", "index": "not_analyed" }
    }
}

첫번째 필드는 name으로 두번째 필드는 name.subname으로 참조 가능.
IP 주소 타입 필드 설정

"address": { "type": "ip", "store": "yes"}
token_count 타입 필드에 제공된 텍스트를 저장하고 색인하는 대신 필드에 얼마나 많은 단어가 존재하는지에 대한 색인 정보를 저장하게 만든다.

분석기 standard: 유럽 언어에 맞춘 표준 분석기 simple: 문자를 기준으로 제공된 값을 분리, 소문자로 변환 whitespace: 여백 문자를 기준으로 제공된 값을 분리 stop: 제공된 불용어 집합에 기반해 자료를 필터링 keyword: 제공된 값을 그대로 전달하는 분석기, not_analyzed로 특정 필드를 명세하는 경우와 동일한 효과 pattern: 정규 표현식을 사용해 유연하게 텍스트를 분리 lanuage: 특정 언어를 위해 동작하게 설계된 분석기 snowball: standard와 유사하지만 어간 추출 알고리즘을 추가로 제공

"similarity": "BM25" 점수 지수를 계산하기위해 BM25를 사용하여 필드 정의

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


질의 프로퍼티
from: 다큐먼트 시작 위치, 기본값은 0
size: 질의 결과로 얻을 최대 다큐먼트수, 기본값은 10

최소 점수값 설정 후 점수보다 높은 다큐먼트만 결과 반환

{
    "min_score": 0.75,
    "query" : {
        ...
    }
}
검색 결과에 포함할 필드 선택
fields 배열을 정의하지 않으면 _source 필드를 사용,
저장된 모든 필드를 반환하길 원한다면 *을 전달

{
    "fields": ["title", "content"],
    "query" : {
        ...
    }
}
_source 필드에서 필드 선택하는 방법

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
스크립트가 평가하는 값 사용
year 필드에서 1800을 뺀 값을 correctYear라는 필드로 반환

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
스크립트 필드에 매개변수 전달

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
_primary 주 샤드에서만 실행, 색인에서 최신 정보를 사용하길 원하지만 자료가 바로 복제되지 않을 경우 유용
_primary_first 주 샤드에서 실행
_local 요청을 전송받은 노드의 사용 가능한 샤드에서 실행
_only_node:node_id 노드 식별자로 지정한 노드에서 실행
_preter_node:node_id 노드 식별자로 지정한 노드에서 실행, 해당 노드가 사용 불가할 경우 다른 노드에서 실행
_shards:1,2 샤드 식별자로 지정한 샤드에서 검색 연산 수행
사용자 정의값

검색 샤드 API

GET /*/_search_shards
{
    "query": {
        "match_all": {}
    }
}




필드 일치 요구
질의와 일치하는 필드만 보여주기를 원하는 경우
require_field_match: true로 설정

PostingsHighlighter
필드 정의에서 index_options 프로퍼티를 offsets 로 설정
매핑에 필드 두개 contents, contents.ps 정의

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

질의 검증
curl -XGET 'localhost:9200/test/_validate/query?pretty' -d '{쿼리내용}'
curl -XGET 'localhost:9200/test/_validate/query?pretty&explain' --data-binary '{쿼리내용}'

기본 정렬
분석되지 않는 필드를 선택해서 정렬하는 것이 바람직
{
    "sort": {
        "_score": "desc"
    }
}


누락된 필드를 위한 정렬
"missing": "_last" 설정으로 해당 필드가 없는 다큐먼트를 결과 목록의 가장 아래로 보낸다
missing 매개변수를 숫자로 설정하면 정의된 필드가 없는 다큐먼트는 필드값이 숫자인 다큐먼트로 취급된다
{
    "sort": {
        "section": {
            "order": "asc",
            "missing": "_last"
        }
    }
}

동적 기준으로 정렬
tags 필드에 색인된 첫 값으로 정렬
{
    "sort": {
        "_script": {
            "script": "doc['tags'].values.length > 0 ? doc['tags'].values[0] : '\u19999'",
            "type": "string",
            "order": "asc"
        }
    }
}



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


식별된 언어를 사용한 질의
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




MVEL (MVFLEX Expression Language)
스크립트 작성을 위한 언어,
자바 객체를 사용하게 만들어주며 getter/setter 호출에 대한 프로퍼티를 자동으로 매핑, 단순한 타입 변환, 컬렉션을 사상(map)하면 배열과 연관 배열로 사상한다
다른 언어 사용시 명령어

bin/plugin -install elasticsearch/elasticsearch-lang-javascript/2.0.0.RCI
직접 만든 스크립트 라이브러리 사용
만든 스크립트는 엘라스틱서치 디렉터리의 config/scripts에 위치해야 한다
파일 확장자는 사용된 언어를 지정해야 한다
사용 예 (파일명: test_sort.js)
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


엘리어스
하나 이상의 실제 색인을 지칭하기 위해 사용할 수 있는 포인터 혹은 이름.
클러스터 상태 정보 내에 존재하며 마스터 노드에 의해서 관리된다.
클러스터 상태 맵 자료구조에 존재하는 색인에 매핑시키기 위한 추가적인 키가 오버헤드 된다.
재색인 관점에서 유연성 제공

멀티서치
"_msearch" 종단점 사용

멀티겟
다수의 문서 조회, "mget" 종단점 사용



search_type

query_then_fetch: 전체 샤드의 검색이 모두 수행된 후 결과 출력, 전체 취합된 결과를 size 매개변수에서 지정한 수만큼 출력
query_and_fetch: 샤드별로 검색되는대로 결과 출력, size * 샤드 갯수만큼 출력
dfs_query_then_fetch: query_then_fetch와 같으나 정확한 스코어링을 위해 검색어들을 사전 처리
dfs_query_and_fetch: query_and_fetch와 같으나 정확한 스코어링을 위해 검색어들을 사전 처리
색인의 크기가 커져도 괜찮다면 퍼지/와일드카드/구문 쿼리 대신 엔그램과 싱글을 사용하면 검색이 빨라짐
스크립트 사용 보다 색인 시 새로운 필드를 추가하여 검색 속도 향상
스크립트가 자주 변경되지 않으면 elasticsearch 플러그인 형태로 네이티브 스크립트 작성
샤드별로 문서 빈도가 불균형하면 dfs_query_then_fetch 사용
많은 문서를 탐색해야 한다면 스캔 검색 사용



할당 인식 Allocation Awareness
데이터의 복제본이 어디에 위치할지에 대한 인지 상태, 클러스터의 위상 topology 설계.

샤드 기반 할당: "cluster.routing.allocation.awareness.attributes:rack"로 설정, "node.rack:1" 메타데이터 키가 할당 인식 파라미터
강제 할당 인식: "cluster.routing.allocation.awareness.attributes:zone", "cluster.routing.allocation.force.zone.values:us-east,us-west"로 설정


핫 스레드 HOT_THREADS API
특정 프로세스가 블락되어 문제를 일으키고 있는것을 확인가능
curl localhost:9200/_nodes/hot_threads

type 파라미터: cpu,wait,block 중 하나 스냅샷의 대상이 되는 스레드 상태
interval 파라미터: 기본값 500ms, 첫 번째와 두번째 사이 대기 시간
threads 파라미터: 몇 개의 상위 핫스레드를 보여줄 것인가


스레드 풀 종류

bulk: 기본값은 가용 프로세서 수
index: 기본값은 가용 프로세서 수
search: 기본값은 가용 프로세서 수 * 3



가비지 컬렉터
메모리 부족시 실행, 레퍼런스가 끊긴 객체들 삭제 JVM 어플리케이션이 사용할 수 있도록 메모리 확보


스왑 방지

bootstrap.mlockall:true 설정
설정확인 curl localhost:9200/_nodes/process
개별 노드의 설정을 사용하여 특정 태그값을 가진 노드에만 위치할 색인을 생성하여 더 많은 힙 이외의 메모리를 가진 노드에 위치, 색인에 대한 응답시간 향상
"index.routing.allocation.include.tag: 태그명1,태그명2..."

I/O가 사용할 노드 수준 제한
indices.store.throttle.type: none(노드전체)/merge(머지할때제한)/all(모든 노드 모든 샤드에 대한 작업 제한 한계점 적용)
index.store.throttle.type: node(노드전체)/merge(머지할때제한)/all(모든 노드 모든 샤드에 대한 색인 작업 제한 한계점 적용)
노드 수준 설정의 경우 "indices.store.throttle.max_bytes_per_sec: 50mb" 사용(초당 메가바이트),
색인 수준 설정의 경우 index.store.throttle.max_bytes_per_sec 사용

저장소 플러그인: 아마존 S3, 하둡 HDFS


# distance_type 파라미터 추가
sloppy_arc: 두 점 사이의 거리를 원호의 빠른 근사치를 구해서 계산
arc: 원의 호를 계산, sloppy_arc보다 정확
plane: 가장 빠르지만 부정확한 구현

# optimize_bbox 파라미터
none
memory
indexed

거리범위 필터 (특정 위치에서 50~100km 사이 검색)
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

거리범위 집계
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

바운딩 박스 필터
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

거리 순 정렬
$ curl localhost:9200/인덱스명/_search -d '{
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

점수 계산할 때 거리 고려하기 (거리가 멀어질수록 점수 낮춤)
$ curl localhost:9200/인덱스명/_search? -d '{
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


특정 위치로부터 범위 안에 있는 결과만 반환
$ curl localhost:9200/인덱스명/_search -d '{
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

지오해시
지오 포인트에서 지오해시를 색인하도록 하려면 매핑에서 geohash:true 로 설정,
그 해시의 부모도 색인하려면 geohash_prefix:true 로 설정
지오해시 셀의 크기는 precision 옵션을 통해 설정

모양 색인하기
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


겹치는 모양 걸러내기
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







전처리 필터

Html Strip Char 필터
토크나이저 필터

Standard
WHITESPACE TOKENIZER
Ngram
Edge Ngram
Keyword
토큰 필터

Ascii Folding
Lowercase
Uppercase
Stop
Stemmer
Synonym
Trim
글로벌 타임아웃 설정
모든 검색 쿼리에 동일하게 적용되도록 정책으로 설정, 기본값 -1(무제한)

PUT _cluster/settings
{
    "transient": {
        "search.default_search_timeout": "1s"
    }
}
질의 결과에대한 스코어 계산확인 -> _explain 사용
질의를 실행하는 과정에서 각 샤드별로 얼마나 많은 시간이 소요되는지 확인 -> "profile": true 사용


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

아파치 스파크
https://ko.wikipedia.org/wiki/%EC%95%84%ED%8C%8C%EC%B9%98_%EC%8A%A4%ED%8C%8C%ED%81%AC

kopf
ElasticSearch API의 모든 잠재력을 탐색할 수 있는 REST 클라이언트를 제공
Kopf는 더 이상 유지되지 않습니다. 대체품(cerebro)이 개발되었으며 현재 https://github.com/lmenezes/cerebro 에서 유지 관리되고 있습니다 .

색인 샘플
https://github.com/dakrone/elasticsearch-in-action

벤치마크 툴
Luceneutil
https://github.com/mikemccand/luceneutil

Rally
https://www.elastic.co/kr/blog/announcing-rally-benchmarking-for-elasticsearch





클러스터 Split Brain 문제 방지
마스터 노드 후보 투표 직전에 네트워크 단절 등으로 클러스터 안에 마스터 후보 노드들이 자신이 마스터 노드가 되어야 한다는 잘못된 판단으로 하나 이상의 노드가 마스터 노드로 전환되면서 데이터 노드도 마스터 노드 개수만큼 분리되고 샤드 조각이 존재하지 않는 등 데이터들이 마구 복구되고 클러스터 전체가 뒤죽박죽 될 수 있다
discovery.zen.minimum_master_nodes 속성으로 대기 중인 마스터 노드 후보들이 투표로 마스터를 선정


핫 스레드
https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-nodes-hot-threads.html



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

_termvector 종단점
색인된 텀에 대한 정보, 도큐먼트와 색인에 얼마나 있는지, 도큐먼트 내 어디에 있는지 확인

소문자화, 스태밍(stemming), 언어 특화(language-specific), 동의어

내장 분석기
표준 분석기, 단순 분석기, 화이트스페이스, 불용어, 키워드, 패턴, 언어 및 다국어, 스노우볼

토큰화
표준 토크나이저, 키워드, 문자, 소문자화, 화이트스페이스, 패턴, UAX URL EMAIL, 경로 계층


토큰 필터
표준, 소문자화, 길이, 불용어, TRUNCATE, TRIM, LIMIT TOKEN COUNT, 반전, 유일성, ASCII FOLDING, 동의어

Ngram
토큰의 각 단어 부분을 다중 서브 토큰으로 분해하는 방식
1-gram, Bigram, Trigram, min_gram, max_gram

edge ngram
ngram 분해의 변종, 앞부분 부터 만든다

shingle
문자 수준이 아닌 토큰 수준에서 ngram

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

explain API
점수 설명하기, "explain=true" 설정을 url이나 전송 내용에 지정



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





동의어 파일 시스템 설정

"filter": {
    "synonym": {
        "type": "synonym",
        "synonyms_path": "synonym.txt"
    }
}
동의어 확장

"filter": {
    "synonym": {
        "type": "synonym",
        "expand": true,
        "synonym": ["one", "two", three"]
    }
}
expand 설정을 false로 했을때 one, two three => one 이었다면,
expand 설정을 true로 했을때 one, two three => one, two three 이 된다.

WordNet 동의어 사용
https://wordnet.princeton.edu/


