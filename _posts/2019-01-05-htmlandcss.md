---
layout: post
---

# HTML5 & Css

HTML5에서 문자 인코딩을 지시하는 방식에는 세가지가 있다  
1. 전송 단계에서 HTTP헤더에 Content_Type을 표시하여 전송하는 방법
2. 문서 앞부분에 특수 유니코드 BOM(Byte Order Mark) 문자를 표시하여 인코딩에 대한 정보를 제공하는 방법
3. 문서 파일의 초기 512바이트 크기 내에 meta 태그의 charset 속성으로 인코딩을 표시하는 방법

```
즉 기존의
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">을 
<meta charset="UTF-8">로 대체 표기하는 방법
참고로 html5는 문자 인코딩 방법으로 utf-8 사용을 권장하고 있다.
```


#### html5 새로운 태그
- section : 일반적인 문서 및 애플리케이션 영역을 표시할 때 사용
- article : 뉴스 기사나 블로그 글과 같은 콘텐츠 단위를 표시할 때 사용
- aside : 문서의 주요 부분 또는 남은 부분의 콘텐츠를 표시할 때 사용
- hgroup : 섹션의 머리말을 표시할 때 사용
- header : 문서를 소개하거나 내비게이션 메뉴 모음을 표시할 때 사용
- footer : 문서의 꼬릿말 부분을 표시할 때 사용
- nav : 문서 내 내비게이션 요소의 영역을 표시할 때 사용
- figure : 그림이나 비디오 같은 임베딩 콘텐츠와 함께 캡션을 표시할 때 사용
- audio, video : 멀티미디어 콘텐츠를 표시할 때 사용
- embed : 플러그인 콘텐츠를 표시할 때 사용
- mark : 별도로 표시한 콘텐츠를 표시하고자 할 때 사용
- progress : 다운로드 중이거나 시간이 걸리는 작업이 진행 중임을 표시할 때 사용
- meter : 디스크 사용량과 같은 측정 값을 표시할 때 사용
- time : 날짜나 시간을 표시할 때 사용
- ruby, rt, rp : 루비 언어를 표현할 때 사용
- canvas : 그래프, 게임과 같은 동적인 비트맵 그래픽을 표시할 때 사용
- command : 사용자가 실행할 수 있는 명령어를 표시할 때 사용
- details : 사용자의 요청에 따라 얻은 컨트롤이나 부가정보를 표시할 때 사용
- datalist : input 태그의 새 속성인 list와 함께 콤보 박스를 만들 때 사용
- keygen : 비밀 키와 공개 키 생성을 제어할 때 사용
- output : 스크립트에 의한 계산 결과 등을 출력할 때 사용



#### input 태그에 새로 정의된 type 속성  
- tel : 전화번호를 입력할 때 사용
- search : text 값과 동일하지만 검색이 수반되는 검색어를 입력할 때 사용
- url : url을 입력할 때 사용
- email : 이메일 주소를 입력할 때 사용
- datetime : UTC 일자 및 시간을 입력할 때 사용
- date : 일자를 입력할 때 사용
- month : 연월을 입력할 때 사용
- week : 연도와 주를 입력할 때 사용
- time : 시간을 입력할 때 사용
- datetime-local : 지역화 일자 및 시간을 입력할 때 사용
- number : 한 줄의 숫자를 입력할 때 사용
- range : 슬라이더 형태로 숫자를 입력할 때 사용
- color : 색상 선택 창을 통해 색상을 선정할 때 사용


#### HTML5에 새로 도입된 태그
{{< bootstrap-table table_class="table table-dark table-striped table-bordered" >}}
| 태그명  | 속성     |  설명  |
| :-----: | :-----: | :----- |
| a, area  | media | link 태그의 속성과 동일하게 만들기 위해 적용할 미디어 타입을 지정할때 사용 |
| a, area  | ping | area 태그를 선택했을 때 ping 속성에 의해 주어진 주소를 호출하여 카운트와 같은 일을 진행하고자 할때 사용   |
| area  | hreflang | link 태그와 동일하게 만들기 위해 사용, 링크할 리소스의 언어 코드 |
| area  | rel | link 태그와 동일하게 만들기 위해 사용, 링크할 속성의 타입 지정 |
| base  | target | a 태그와 속성을 동일하게 만들기 위해 링크된 문서가 표시될 문서를 지정하고자 할 때 사용 |
| ol  | reverse | li 태그를 일련 번호가 큰 순서대로 표시 |
| meta | charset | 문자 인코딩을 지정할 때 사용 |
| input, select, textarea, button | autofocus | type 속성값이 hidden이 아닌 경우 지정가능, 페이지 로드 후 포커싱 |
| input, textarea | placeholder | 입력 형식에 대한 힌트 제공 |
| input, output, select, textarea, button fieldset | form | form에 id값을 적용, id 값을 갖는 폼과 함께 전송 |
| input, textarea 등 폼 컨트롤 태그 | required | 폼을 전송할 때 반드시 입력해야 함을 지정가능 |
| fieldset | disabled | fieldset 태그에 속한 모든 콘텐츠를 사용할 수 없도록 처리 |
| input | autocomplete | 자동 완성 여부 지정 |
| input | min | 최소값 지정 |
| input | max | 최대값 지정 |
| input | multiple | 다수 값의 입력 여부 지정 |
| input | pattern | 값을 체크하는 정규 표현식 지정 |
| input | step | 컨트롤의 증가 단위 값 지정 |
| form | novaildate | 폼 컨트롤의 값을 체크하지 않고 전송 |
| input, button | formaction, formenctype, formmethod, formnovaildate, formtarget| form 태그의 action, enctype, method, novaildate, target 속성들을 재정의 할 때 사용 |
| menu | type, label | type 속성은 메뉴 형태를 지정할 때 사용, label 속성은 메뉴 명칭을 지정할 때 사용 |
| style | scoped | style 태그에 의해 기술된 스타일을 style 태그의 부모 영역에 속한 자식 태그에 모두 적용시키고자 할 때 사용 |
| script | async | 스크립트가 실행할 수 있는 상태에서 비동기적으로 실행하도록 할 때 사용 |
| html | menifest | 오프라인 웹의 애플리케이션을 제작하거나 캐시 명세를 정의할 때 사용 |
| link | sizes | 아이콘의 크기를 정의할 때 사용(rel 속성으로 아이콘을 정의) |
| iframe | sandbox, seamless, srcdoc | sandbox 속성은 allow-same-origin, allow-forms, allow-scripts 값 중 하나를 가지며, iframe의 콘텐츠 동작의 제약을 지정할 때 사용 seamless 속성은 iframe 영역이 도큐먼트 영역과 분할되어 표시되지 않도록 할 때 사용 |
| 모든 태그 | class, dir, id, lang, title | 전역 속성으로 모든 태그에 적용 가능한 속성 |
| 모든 태그 | contenteditable | 편집 가능한 영역임을 표시하는 속성, 내용을 변경할 수 있도록 할 때 사용 |
| 모든 태그 | contextmenu | 문맥 메뉴를 제공 |
| 모든 태그 | data- | 데이터를 사용자 정의로 부여할 때 사용 |
| 모든 태그 | draggable | 드래그 사용 여부를 지정할 때 사용 |
| 모든 태그 | hidden | 도큐먼트 반영 여부를 지정할 때 사용 |
| 모든 태그 | spellcheck | 맞춤법 검사 여부를 지정 |
| 모든 태그 | accesskey | 단축키로 사용할 값을 지정 |
| 모든 태그 | tabindex | 탭 키의 이동 순서를 지정할 때 사용 |
{{< /bootstrap-table >}}



#### HTML5에서 재정의한 태그
{{< bootstrap-table table_class="table table-dark table-striped table-bordered" >}}
| 태그명  | 설명  |
| :-----: | :----- |
| a  | href 속성 없이 사용 |
| address | 가장 인접한 article 또는 body 태그와 관련된 연락처 정보 제공 |
| b | 제품 소개 내 제품명, 문서 초록의 키워드, 인쇄 강조 표현을 가진 텍스트 표현 |
| hr | 단락 단위의 주제 바꿈을 할때 사용 |
| i | 인쇄상 기울임을 표현할 때 사용 |
| label | 폼 컨트롤의 캡션을 지정할 때 사용 |
| menu | 도큐먼트의 메뉴 항목을 제공할 때 사용 |
| small | 세부 주석 및 법적 인쇄 문서에서 작은 인쇄 정보를 지정할 때 사용 |
| strong | 콘텐츠의 중요성을 강조할 때 사용 |
{{< /bootstrap-table >}}



##### 권장하지 않는 태그
{{< bootstrap-table table_class="table table-dark table-striped table-bordered" >}}
| 태그명  | 속성     |  설명  |
| :-----: | :-----: | :----- |
| img | border | 값이 0일 때만 사용하고 가급적 css 사용을 권장 |
| script | language | 생략가능, 작성시 'JavaScript' 사용을 권장 |
| a | name | id 속성으로 사용하길 권장 |
| table | summary | 테이블 요약 정보를 사용자가 볼 수 있도록 기술하기를 권장 |
| basefont, big, center, font, s, strike, tt, u | | 기능 자체를 사용하지 못하는 것이 아니라 css로 대체하여 사용하기를 권장 |
| frame, frameset, noframes | | 부정적인 사용성과 접근성 때문에 사용하지 않기를 권장 |
| acronym | | 매우 큰 혼란을 주고 있어 배제 |
| applet | | object 태그로 대체되어 배제 |
| isindex | | 폼 컨트롤을 통해 대체되어 배제 |
| dir | | ul 태그로 대체되어 배제 |
| noscript | | HTML 문법에서만 사용하고 XML 문법에서는 더이상 하용하지 않으므로 배제 |
{{< /bootstrap-table >}}



#### HTML5에서 더 이상 사용할 수 없거나 웹 브라우저가 호환성을 위해 처리하는 태그
{{< bootstrap-table table_class="table table-dark table-striped table-bordered" >}}
| 태그명  | 속성  |
| :-----: | :----- |
| a, link | nev, charset |
| a | shape, coords |
| img, iframe | longdesc |
| link | target |
| area | nohref |
| head | profile |
| html | version |
| img | name |
| meta | scheme |
| object | archive, classid, codebase, codetype, declare, standby |
| param | valuetype, type |
{{< /bootstrap-table >}}



#### HTML5에서 css로 대체된 태그
{{< bootstrap-table table_class="table table-dark table-striped table-bordered" >}}
| 태그명  | 속성  |
|:------:|:------------|
| caption, iframe, img, input, onject, legend, table, hr, div, h1, h2, h3, h4, h6, p, col, colgroup, tbody, td, tfoot, th, thead, tr | align |
| body | alink, link, text, vlink, background |
| table, tr, td, th, body | bgcolor |
| table, object | border |
| table | cellpadding, cellspacing, iframe, rules |
| col, colgroup, tbody, td, tfoot, th, thead, tr | char, charoff, valign |
| br | br |
| dl, menu, ol, ul | compact |
| iframe | frameborder |
| td, th | height, nowrap |
| img, object | hspace, vspace |
| iframe | marginheight, marginwidth, scrolling |
| hr | noshade, size |
| li, ol, ul | type |
| hr, table, td, tr, col, colgroup, pre | width |
{{< /bootstrap-table >}}



#### API
{{< bootstrap-table table_class="table table-dark table-striped table-bordered" >}}
| API  | 설명  |
| :-----: | :----- |
| 그래픽 | canvas 태그를 사용하며 지정된 영역에 2차원 그래픽 처리 지원 |
| 비디오, 오디오 | video, audio 태그 이용, 비디오 및 오디오의 재생 지원 |
| 오프라인 | 네트워크가 차단된 오프라인 환경 지원 |
| 웹 메시징 | 도메인 또는 다른 도메인 간의 데이터 통신 지원 |
| 웹 스토리지 | 클라이언트에 키/값 형태로 데이터를 저장하는 것을 지원 |
| 웹 데이터베이스 | 클라이언트에 SQL 기반으로 데이터베이스를 처리하는 것을 지원 |
| 웹 워커 | 백그라운드로 특정 작업을 수행하는 것을 지원 |
| 위치 정보 | 디바이스 위치적 정보를 이용하는 것을 지원 |
| 드래그 앤 드롭 | draggable 속성을 이용, 드래그 앤 드롭 기능을 지원 |
{{< /bootstrap-table >}}



#### 웹 스토리지
웹 브라우저의 로컬 디스크 저장 공간  
쿠키는 4KB 크기의 제한 있음  
localStorage,sessionStorage객체가 있다  

#### localStorage
웹 사이트 도메인 단위로 별도 운영  
저장된 데이터의 유효 시간이 없는 구조  

#### sessionStorage
로컬 디스크를 사용하여 데이터를 도메인 단위로 별도 저장  
웹 브라우저 종료시 데이터 소멸  
웹 브라우저와 동일한 생존 기간  

#### 웹 데이터베이스
웹 스토리지와 같이 로컬 저장 장치에 저장되고,  
HTML5의 자바스크립트 코드를 사용하여 데이터베이스를 생성, 데이터 CRUD 가능.  
'웹 SQL 데이터베이스', '인덱스드 데이터베이스'가 있다.  


웹 SQL 데이터베이스 생성(SF, CR, OP)  
openDatabase(사용할 데이터베이스 이름, version, 사용자를 위해 표시되는 이름, 데이터베이스 크기 바이트단위)
반환값  
- version: 현재 버전
- changeVersion: 버전 변경하는 메서드
- transaction(callback, error_callback, success_callback): 트랜잭션을 여는 메서드, executeSQL(sql, args, success_callback, error_callback) 메서드는 SQL 문장을 실행하기 위해 사용
- readTransaction: 읽기 전용의 트랜잭션을 여는 메서드

웹 SQL 데이터베이스 레코드 처리(SF, CR, OP)  
executeSQL(sql, args, success_callback, error_callback)  

웹 SQL 데이터베이스 버전 관리(SF, CR, OP)  
changeVersion(이전버전, 새로운버전, 버전 변경 시 처리할 콜백 함수, error_callback, success_callback)  

동기식 웹 SQL 데이터베이스 사용(SF, CR, OP)  
워커(Worker)를 사용, 사용자 인터페이스(UI)에 접근하지 못하는 자바스크립트 스레드(Thread)  
```
var worker = new Worker("test.js");
worker.postMessage("insert") // 워커에게 "insert" 메시지 전송
worker.onmessage = function(event) { // 워커로부터 메시지를 수신했을 경우 }
```

인덱스드 데이터베이스 사용(FF, CR)  
SQL 문장을 사용하지 않고 키와 값의 쌍으로 이루어진 구조로 대용량 자료 저장  
FF(파이어폭스): window.mozIndexedDB  
CR(크롬): window.webkitIndexedDB  

생성  
createObjectStore(name, optional)  
- name: 오브젝트 스토어의 이름
- optional: keyPath와 autoIncrement에 대해 기술

삭제  
deleteObjectStore(name)  

데이터 추가  
add({ key1: value1, key2: value2, key3: value3, ... })  

데이터 삭제  
delete(value)  

SQLite 데이터베이스  
별도의 환경 설정이 필요 없고, 서버가 없으며 파일로 처리하는 데이터베이스  


웹 메시징  
한 페이지에 의해 활성화된 다른 웹 페이지 간의 비동기식 메시지 교환  
postMessage 메서드와, message 이벤트 핸들러를 사용하여 서로 다른 도메인 간 메시지 교환  

도큐먼트 간 메시지 교환(IE, FF, SF, OP, CR)  
```
window.postMessage(data, [ports], 메시지를 수신하는 도메인을 지정하는 문자열)  
```

message 이벤트 핸들러 등록  
- IE: window.attachEvent("onmessage", event_handler);  
- FF, SF, OP, CR: window.addEventListener("message", event_handler); 또는 window.onmessge = event_handler;  
 
message 이벤트에 의해 전달되는 MessageEvent 객체  
- data: 수신된 메시지 내용
- origin: 메시지 송신 측의 도메인
- source: 메시지 송신 측의 window 객체
- ports: 전달된 포트
- lastEventId: 마지막 이벤트 아이디

메시지 채널의 사용(SF, OP, CR)  
MessageChannel 객체로 다대다 메시지 교환 가능  

MessagePort 객체 속성  
- postMessage: 메시지 송신을처리하는 메서드
- start: 포트에 메시지가 도착할 경우 message 이벤트 활성화 시키는 메서드
- close: start 메서드에 의해 시작된 message 이벤트 비활성화
- onmessage: 포트에 메시지가 도착하면 message 이벤트 발생, message 이벤트 핸들러 설정을 위해 사용되는 속성
- addEventListener: 이벤트 핸들러 설정을 위해 사용되는 메서드
- removeEventListener: 이벤트 핸들러 설정을 제거하기 위해 사용되는 메서드

메시지 포트의 전달(SF, OP, CR)  
MessageChannel 객체의 MessagePort를 서로 다른 도메인에서 사용하려면 하나의 포트를 다른 쪽의 window 객체에 통지해야 한다.  


서버 전송 이벤트(Server-Send Event)  
서버 프로그램이 지정한 일정한 주기 마다 서버의 처리 결과를 웹 브라우저에 통지  
```
var source = new EventSource("서버소스")
source.addEventListener("message", function(event){ ... }, false);
```
EventSource 객체의 속성과 메서드  
- URL: EventSource 생성자 메서드에서 지정된 URL 값
- readyState: 상태 값, CONNECTING(서버에 접속 요청을 수행 중인 단계), OPEN(서버에 접속 요청이 완료된 단계), CLOSED(서버와의 접속 상태가 종료된 단계)
- onopen: 서버와의 접속이 완료되었을 경우 호출되는 이벤트 핸들러
- onmessage: 서버로부터 메시지가 수신되었을 경우 발생되는 이벤트 핸들러
- onerror: 오류가 발생되었을 경우 호출되는 이벤트 핸들러
- close: 서버와의 접속을 해제하기 위한 메서드


서버 푸시(Server Push)  
서버로부터의 메시지가 클라이언트의 요청에 의해서가 아니라 서버의 판단에 의해 능동적으로 클라이언트에게 데이터를 전달하는 방식  


폴링(Polling)  
클라이언트가 주기적으로 서버에게 데이터를 요청하는 구조  


웹 워커(Web Worker)  
자바스크립트를 이용하여 백그라운드로 처리되는 일종의 스레드  
```
var worker - new Worker("worker_prime.js");
worker.onmessage = function(evt) { ... }
```
- postMessage: 메시지를 워커에게 전송한다
- terminate: 워커를 강제로 종료한다
- onmessage: 워커가 보낸 메시지를 수신하는 이벤트 핸들러
- onerror: 에러 발생을 통지받기 위한 이벤트 핸들러
- close: 워커의 메시지 수신을 중지하는 메서드
- importScript: 지정한 주소의 자바스크립트 파일을 읽어 실행하는 메서드
- self: 워커 자신을 나타내는 변수
- location: 워커 생성 시 갖는 location에 관한 정보
- navigator: 워커 생성 시 갖는 location에 관한 정보
- setTimeout, setInterval, clearInterval: 타이머 관련 작업을 수행할 수 있는 메서드
- openDatabase, openDatabaseSync, indexedDB: 웹 데이터베이스를 처리하기 위한 객체
- XMLHttpRequest: Ajax를 처리하기 위한 객체
- Worker, SharedWorker: 워커 사용을 위한 객체
- WebSocket: 웹 소켓을 위한 객체
- applicationCache: 공용 워커의 소스 파일 이름이 menifest 파일에 수록되어 오프라인으로 처리될 경우의 applicationCache 객체이다.


전용 워커(dedicated worker)  
워커가 페이지별로 생성된다.  

공용 워커(shared worker)  
여러 창에대해 공용 워커의 이름만 같을 경우 하나만 생성되는 워커  
메시지 교환을 위해 MessagePort 객체가 같이 사용된다.  
공용으로 사용할 데이터를 한곳에 모아 처리하고 그 결과를 함께 공유할 경우 유용  


웹 소켓  
웹 브라우저와 서버 간의 완전한 전이중(full-duplex) 통신을 지원  
- url: 웹 소켓 프로토콜을 사용해 연결할 서버의 url 주소
- readyState: 상태 값, 0(CONNECTING-연결 안된 상태), 1(OPEN-연결된 상태), 2(CLOSING-연결 해제 절차를 수행중인 상태), 3(CLOED-연결 해제 상태)
- bufferedAmount: send 메서드이 의해 전송 버퍼에 놓여 전송 대기 준인 데이터 바이트 수
- protocol: 서버의 요청에 의해 실행되는서브 프로토콜의 이름
- send: 데이터를 서버에 전송하기 위한 메서드
- close: 연결 상태를 해제하기 위한 메서드
- onopen: 서버와의 웹 소켓 연결을 알리는 이벤트 핸들러
- onmessage: 서버로부터 전달된 데이터의 수신을 알리는 이벤트 핸들러
- onerror: 에러 발생 시 실행되는 이벤트 핸들러
- onclose: 연결 해제 시 실행되는 이벤트 핸들러


위치정보(Geolocation) API  
자바스크립트의 window.navigator 객체에 정의된 디바이스 위치 정보  
GPS 정보 기반 혹은 네트쿼크 주소 정보를 기반으로 제공  


사용자 위치 정보의 출력(FF, CR, OP)  
getCurrentPosition(success_callback, error_callback, options)  

success_callback  
- coords.latitude: 위도
- coords.longitude: 경도
- coords.altitude: 고도 값
- coords.accuracy: 위도 경도의 오차 (미터 단위)
- coords.altitudeAccuracy: 고도의 오차 (미터 단위)
- coords.heading: 디바이스의 진행 방향으로 북쪽을 기준으로 시계 방향의 각도
- coords.speed: 디바이스의 진행 속도 (초당 미터 단위)
- timeout: 위치 정보를 얻은 시각으로 DOM 타입 스탬프 값

error_callback  
- code: 0(UNKNOWN_ERROR-알 수 없는 에러), 1(PERMISSION_DENIED-권한 없음), 2(OPSITION=UNVALILABLE-위치 정보를 얻을 수 없음), 3(TIMEOUT-시간 초과)
- message: 에러 메세지

options  
- enableHighAccuracy: boolean 값으로 정확도가 높은 위치 정보를 요청하는 경우, 기본값 false
- timeout: 위치 정보를 구하기 위해 허용된 밀리세컨드 단위의 시간 제한 값
- maximumAge: 위치 정보의 유효 기간을 밀리세컨드 단위로 설정한 것


사용자 위치 추적(FF, CR, OP)
watchPosition(success_callback, error_callback, options)  
현재 위치 정보 보고, getCurrentPosition과 차이점은 디바이스가 현재의 위치 정보가 변경되었다고 판단할 때마다 success_callback 호출  

clearPosition(watchPositionId)  
watchPosition에 의해 활성화 된 위치 정보의 보고 정지  


파일 처리(FF, CR)  
사용자에 의해 드래그앤 드롭된 파일이나 input 태그를 통해 지정된 파일에만 접근 가능  

File 객체  
- name: 파일의 이름을 알려주는 속성
- size: 파일 크기
- type: 파일 MIME 타입
- slice(start.length): 시작 위치와 바이트 단위의 길이를 지정하면 그 영역에 해당하는 파일의 내용을 잘라 새로운 Blob 객체를 생성

FileReader 객체  
- readAsText(filename, encoding): 텍스트 파일을 주어진 인코딩 방식으로 읽는다
- readAsBinaryString(filename): 이진 파일로 파일을 읽는다
- readAsDataURL(filename): 파일 내용을 DataURL 형식으로 읽는다
- result: 읽어들인 파일의 내용
- error: 에러가 발생한 경우 에러 값
- onload: 읽기가 성공적으로 완료되었을 경우 호출되는 이벤트 핸들러
- onprogress: 읽기의 진행 상황을 보고하기 위해 호출되는 이벤트 핸들러
- onerror: 읽기 에러가 발생했을 경우 호출되는 이벤트 핸들러


XMLHttpRequest Level2를 이용한 크로스 도메인 요청(CR)  
기존의 AJAX(Asynchronous JavaScript & XML)에서 사용하는 XMLHttpRequest 객체는 같은 도메인 내에서만 문서를 요청하고 결과 수신 가능,  
XMLHttpRequest Level2는 다른 도메인에 대해 요청하고 응답할 수 있음  
FormData 객체를 사용하여 단순한 형태로 전달 가능  


통지(Notification)(CR)  
자바스크립트 window객체의 webkitNotifications 객체를 통해 메시지 알림 표시  
- requestPermission: 통지 기능의 사용자 동의를 구하는 절차 수행
- checkPermission: 사용자 동의가 이루어졌는지 반환
- createNotification: 일반 텍스트 형식으로 오른쪽 하단에 통지 메시지가 나타남
- createHTMLNotification: 인자로 URL을 제공하여 오른쪽 하단에 HTML 문서가 나타남


WebGL  
웹 기반의 그래픽 라이브러리, 자바스크립트를 이용해 3D 그래픽을 웹 브라우저에서 처리할 수 있도록 하는 기술  
