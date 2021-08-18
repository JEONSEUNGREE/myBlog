---
title: JS많이 쓰는 문법위주로 정리
date: 2021-08-01 20:34:08
tags:
  - javascript
  - vue
categories:
  - javascript
---

문법대해서 필요한 부분만 빠르게 정리해봤다.

Vanilla JS서 주 사용 (쓸일은 없겠지만)
'use strict' : 최상단에 추가해 정의되지않은 변수, 상수의 오류를 보여줌

<h2>블럭:</h2>
블럭으로 지역변수 느낌으로 선언 가능: {}
블럭 밖에 선언은 global 선언 말그대로 메모리에 위치해있음 필요할때만 사용변수
var VS let의 차이에서 var가 블럭안에 있음에도 블럭밖에서 잡힌다.
<hr>

(ES6 이후 let 지원)
<h2>var VS let</h2>
호이스팅 여부 (선언하기전에 사용) let의 경우는 오류가 나오지만 var의 경우는 undefined로 나옴 
또한 {}을 무시하기때문에 외부에서 사용 가능 global 선언이 되버린다.
그래서 let을 사용해야한다.

<hr>
<h2>JS는 data type 없음 </h2>
number로 메모리 할당여부를 고려하지않는다. (자바나 C와 다르게 고려할 필요가 없음)

<hr>
<h2>first-class function</h2>
이말은 function 데이터 타입처럼 변수에 할당이 가능 즉 function(function) 이런식으로 자바에서 매개변수로 메서드를 받지못했었는데
JS는 가능하다. 또한 return 타입으로 function도 가능하다. 
<hr>

<h2>`(백틱):</h2>
String + value
우리가 흔히 String + data = stringdata 이런식으로 사용을많이하는데
JS에서는 template literals라고 백틱을 통해 `string ${data}`= stringdata 이런식으로 사용가능하다. (space도 다 자동 인식)

<hr>
<h2>type of : </h2>
데이터 타입 알림
<hr>

<h2>Symbol: </h2>
고유 식별자로 사용가능 같은 Symbol 만들기 Symbol.for('') Symbol출력 Symbol.description을 사용해서 출력
(참고로 인스턴스 주소값을 가지고있어서 symbol의 이름이 XX라고해서 String XX와는 절대 같을수 없다.)
<hr>


<h2>String에서 ' 표시</h2>
consolelog("somting's stuff") 이러면 somthing 이렇게만 나온다
consolelog("somting\'s stuff") 이런식으로 해야지 정상출력 또한 자바처럼 \n로 줄바꿈 가능

<hr>
<h2>equality</h2>
stirng 5 === number 5 = > false <br>
string 5 == number 5 => true

<hr>
<h2>매개변수 디폴트값 설정</h2>
여태 이런식으로 코드를 구성했는데
function showMessage(message, from) { <br>
	if(from === undefined) { <br>
	    from = "unknown"; <br>
   }<br>
}<br>
아래 문법을 사용하면 default 값을 설정할수있다. <br>
function showMessage(message, from = "unknown") { 이렇게 하면 = 뒤에 값이 디폴트 값이됨

<hr>

<h2>...:</h2>
...args 배열형태로 전달
<hr>

<h2>Arrow functioin</h2>
자바의 람다와같이 익명 function (reuturn 생략 function, name 생략가능)<br>
(자바에서는 매개변수있으면 return 생략안됐음)<br>

ex) cont somthing = () => console.log('todo');
const add = (a,b) => a+b

블록({})을 사용할경우에는 return을 사용한다.
const somthing = (a,b) => {
	return somtihngtodo
}

	
출처는 드림코딩 by 엘리 강의를 보고 정리했다.
