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


