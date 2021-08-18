---
title: AsyncVsDefer
date: 2021-08-01 20:34:31
tags:
  - javascript
  - CS
categories:
  - CS
---

Async Vs Defer

<h3>비동기가 필요한 이유</h3>
html을 브라우저 읽을때 코드를 읽다 sciprt를 만나면 서버에서 다운받고 읽게된다.
이 방식이면 스크립트가 클수록 사용자가 기다리는 시간이 발생한다.
이 비동기 적용을 하지않는경우 스크립트의 위치가 head에있다면 먼저 script를 받기때문에 html 코드가 보이지않게되고
반대로 body에있다면 html은 어느정도읽히지만 script에 의존적이라면 html이 보여도 이상하게 나올수있다.
따라서 비동기적으로 처리가 필요하다.
<hr>

<h2>Async</h2>
script 에 asyn src="" 이런식으로asyn을 넣어주면 비동기적 처리를 실행하게된다.<br>
장점 : 병렬 처리로 처리시간 감소<br>
단점 : js에서 queryselect등의 작동이 불가해질수있다. 다운받아지는 시점에따라 기능이 어느정도 될지 예상 하기 힘듬
<hr>

<h2>defer</h2>
script defer src="" 식으로 선언하고 html을 parsing 하는 과정에 병렬로 script를 다운받고 다 받아지면 실행하게 된다.<br>

또한 순서에있이서도 차이가 있는데
async의 경우 1,2,3 script 순서로 의존적이라하면 먼저 받아진 순서가 달라서 실행과정에서 꼬일수있지만
defer같은경우는 순서에따라서 받아지고 순서에따라 실행됨

오늘 vue에서도 router의 모든 컴포넌트에 적용하니까 정말 체감이 많이된다.

