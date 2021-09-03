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
| 태그명  | 속성     |  설명  |
| :-----: | :-----: | :----- |
| a, area  | media | link 태그의 속성과 동일하게 만들기 위해 적용할 미디어 타입을 지정할때 사용 |
| a, area  | ping | area 태그를 선택했을 때 ping 속성에 의해 주어진 주소를 호출하여 카운트와 같은 일을 진행하고자 할때 사용   |
| area  | hreflang | link 태그와 동일하게 만들기 위해 사용, 링크할 리소스의 언어 코드 |
| area  | rel | link 태그와 동일하게 만들기 위해 사용, 링크할 속성의 타입 지정 |
| base  | target | a 태그와 속성을 동일하게 만들기 위해 링크된 문서가 표시될 문서를 지정하고자 할 때 사용 |
| ol  | reverse | li 태그를 일련 번호가 큰 순서대로 표시 |
|meta | charset | 문자 인코딩을 지정할 때 사용 |
|input, select, textarea, button | autofocus | type 속성값이 hidden이 아닌 경우 지정가능, 페이지 로드 후 포커싱 |
|input, textarea | placeholder | 입력 형식에 대한 힌트 제공 |
|input, output, select, textarea, button fieldset | form | form에 id값을 적용, id 값을 갖는 폼과 함께 전송 |
|input, textarea 등 폼 컨트롤 태그 | required | 폼을 전송할 때 반드시 입력해야 함을 지정가능 |
|fieldset | disabled | fieldset 태그에 속한 모든 콘텐츠를 사용할 수 없도록 처리 |
|input | autocomplete | 자동 완성 여부 지정 |
|input | min | 최소값 지정 |
|input | max | 최대값 지정 |
|input | multiple | 다수 값의 입력 여부 지정 |
|input | pattern | 값을 체크하는 정규 표현식 지정 |
|input | step | 컨트롤의 증가 단위 값 지정 |
|form | novaildate | 폼 컨트롤의 값을 체크하지 않고 전송 |
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



#### HTML5에서 재정의한 태그
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



##### 권장하지 않는 태그
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



#### HTML5에서 더 이상 사용할 수 없거나 웹 브라우저가 호환성을 위해 처리하는 태그
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



#### HTML5에서 css로 대체된 태그
| 태그명  | 속성  |
| :-----: | :----- |
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



#### API
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



