---
layout: post
---

# Javascript And JQuery
자바스크립트 공식 명칭은 ECMAScript.  

function(){} 형태는 함수이지만 이름이 없으므로 익명함수라고 부른다.  

클로저  
지역 변수를 남겨두는 현상 혹은 내부의 변수들이 살아있는 것이므로 함수로 생성된 공간을 부르는 명칭  

타이머 함수  
setTimeout(): 특정한 시간 후에 함수를 한 번 실행  
setInterval(): 특정한 시간마다 함수를 실행  

인코딩과 디코딩 함수  
- escape(): 적절한 정도로 인코딩, 영문자 숫자 특수문자를 제외하고 모두 인코딩
- unescape(): 적절한 정도로 디코딩
- encodeURI(uri): 최소한의 문자만 인코딩, 인터넷 주소에서 사용되는 일부 특수문자는 변환하지 않는다
- decodeURI(encodedURI): 최소한의 문자만 디코딩
- encodeURIComponent(urlComponent): 대부분의 문자를 모두 인코딩
- decodeURIComponent(encodedURI): 대부분의 문자를 모두 디코딩

#### javascript
- eval(): 문자열을 자바스크립트 코드로 실행하는 함수
- isFinite(): number가 무한한 값인지 확인
- isNaN(): number가 NaN인지 확인
- parseInt(string, [진수]): string을 정수로 바꿔준다
- parseFloat(string, [진수]): string을 유리수로 바꿔준다
- Number(obj): 숫자로 바꿔준다, 숫자로 바꿀 수 없으면 NaN으로 변환한다

in 키워드  
'속성명' in 객체명  
해당 속성명이 객체 내에 포함되어있는지 확인  

with(객체) {...}  
괄호 안에서 객체를 명시할 필요없이 속성명만 사용하여 간략화 가능  

delete(객체.속성)  
속성 제거  

생성자 함수  
new 키워드로 객체를 생성할 수 있는 함수  

instanceof 키워드  
객체 instanceof 객체  -> true 결과인 경우 상속되었다고 판단  

constructor() 메서드  
typeof 연산자로 타입 비교가 어려울 때 두 대상을 같은 자료형으로 취급하고 싶을 때 사용  

Number 객체 메서드  
- toExponential(): 숫자를 지수 표시로 나타낸 문자열 리턴
- toFixed(): 숫자를 고정 소수점 표시로 나타낸 문자열 리턴, 소수점 몇 번째 자리까지 자르는 메서드
- toPrecision(): 숫자를 길이에 따라 지수 또는 고정 소수점 표시로 나타낸 문자열 리턴, 유효 숫자의 자릿수

Number 생성자 함수의 속성  
- MAX_VALUE: 자바스크립트 숫자가 나타낼 수 있는 최대의 숫자
- MIN_VALUE: 자바스크립트 숫자가 나타낼 수 있는 최소의 숫자
- NaN: 자바스크립트의 숫자로 나타낼 수 없는 숫자
- POSITIVE_INFINITY: 양의 무한대 숫자
- NEGATIVE_INFINITY: 음의 무한대 숫자

String 객체의 HTML 관련 메서드  
- anchor(): a 태그로 문자열을 감싸 리턴
- big(): big 태그로 문자열을 감싸 리턴
- blink(): blink 태그로 문자열을 감싸 리턴
- bold(): b 태그로 문자열을 감싸 리턴
- fixed(): tt 태그로 문자열을 감싸 리턴
- fontcolor('색상'): font 태그로 문자열을 감싸고 color 속성을 주어 리턴
- fontsize(숫자): font 태그로 문자열을 감싸고 size 속성을 주어 리턴
- italice(): i 태그로 문자열을 감싸 리턴
- link(): a 태그에 href 속성을 지정해 리턴
- small(): small 태그로 문자열을 감싸 리턴
- strike(): strike 태그로 문자열을 감싸 리턴
- sub(): sub 태그로 문자열을 감싸 리턴
- sup(): sup 태그로 문자열을 감싸 리턴

연산 메서드  
- reduce(): 배열의 요소가 하나가 될 때까지 요소를 왼쪽부터 두개씩 묶는 함수 실행
- reduceRight(): 배열의 요소가 하나가 될 때까지 요소를 오른쪽부터 두개씩 묶는 함수 실행

ECMAScript 5 Object 객체  
- Object.defineProperty(): 객체에 속성 추가
- Object.defineProperties(): 객체에 속성들을 추가
- Object.create([상속할 객체], {...}): 객체 생성
- Object.preventExtensions(): 객체의 속성 추가 제한
- Object.isExtensible(): 객체에 속성 추가가 가능한지 확인

location 객체  
- assign(link): 현재 위치 이동
- reload(): 새로고침
- replace(link): 현재 위치를 이동, 뒤로가기 불가


이벤트 버블링  
IE: cancelBubble 속성을 true로 변경  
그 외: stopPropagation() 메서드 사용  

IE 이벤트 연결 및 제거  
attachEvent(eventProperty, eventListener); 이벤트가 추가되는 형식  
detachEvent(eventProperty, eventListener); 함수명을 모르면 제거 불가  

#### JQuery

JQuery 속성 선택자  
- 요소[속성=값] 속성과 값이 같은 객체 선택
- 요소[속성|=값] 속성 안의 값이 특정 값과 같은 객체 선택
- 요소[속성~=값] 속성 안의 값이 특정 값을 단어로 시작하는 객체 선택
- 요소[속성^=값] 속성 안의 값이 특정 값으로 시작하는 객체 선택
- 요소[속성$=값] 속성 안의 값이 특정 값으로 끝나는 객체 선택
- 요소[속성*=값] 속성 안의 값이 특정 값을 포함하는 객체 선택
- 요소:odd 홀수 번째 위치한 객체 선택
- 요소:even 짝수 번째 위치한 객체 선택
- 요소:first 첫 번째 위치한 객체 선택
- 요소:last 마지막에 위치한 객체 선택
- 요소:contains(문자열) 특정 문자열을 포함하는 객체 선택
- 요소:eq(n) n번째 위치하는 객체 선택
- 요소:gtn(n) n번째 초과에 위치하는 객체 선택
- 요소:has(h1) h1태그가 있는 객체 선택
- 요소:lt(n) n번째 미만에 위치하는 객체 선택
- 요소:not(선택자) 선택자와 일치하지 않는 객체 선택
- 요소:nth-child(3n+1) 3n+1번째에 위치하는 객체 선택 (예: 1,4,7..)

$.noConflict()  
플러그 간 충돌 방지 메서드, jquery $를 사용할 수 없음  

end() 문서 객체 선택을 한 단계 뒤로 돌린다  


on() 이벤트 연결 / off() 이벤트 제거 / one() 이벤트 한 번만 연결  

trigger() 이벤트 강제 발생  

preventDefault() 기본 이벤트 제거  
stopPropagation() 이벤트 전달 제거  


innerfade 사진 변환 효과 innerfade 플러그인 사용  

clearQueue()  
메소드가 호출되어 실행되지 않은 대기열에 모든 함수는 큐로부터 제거  

stop()  
현재 실행 중인 애니메이션을 중지  

delay()  
큐에 있는 명령을 잠시 중지  




