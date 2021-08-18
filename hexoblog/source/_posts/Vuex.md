---
title: Vuex
date: 2021-08-01 21:03:55
tags:
  - vue
categories:
  - vue
---

<h3>중앙관리가 필요한이유</h3>
vue를 쓰다보면 단순히 부모와 자식이 하나씩있으면 간단하지만 
같은 todo앱을 만들때도 컴포넌트를 엄청사용해서 emit,props를 쓸때와 vuex를 쓸때는 시간도 그렇고 코드를 보기도 편했다.
자식의 자식이 연달아 있다면 계속 emit을 올려주거나 props를 연달아 받게된다.
그런 문제를 해결하기 위해서 v-bind=$attr, v-on=$listener를 사용하는 방법도있지만 store를 통해 관리해주는 vuex가 있다.
<hr>

<h3>구조</h3>
vuex를 보자면 vuex의 구조는 state, mutations, actions, getters, mutationstype이있다.
여기서 state는 임계영역 데이터라고 보고, mutations는 동기화를 시키는영역 component에는 딱히 비교할만한게없다.
actions는 비동기영역으로 메서드라고 보면되고 getters는 computed같은 흐름으로 보면된다

또한 actions는 비동기적이기때문에 동기작업이 필요한경우는 꼭 mutations에 넣어두고 서버로부터 요청이오는데 걸리는 시간때문에
actions는 비동기 처리를하면서 다른 작업을 할 수 있는것이다. 요청이 만료되었을때 비로소 mutations에 데이터 commit이 되고 데이터 접근이 가능한 순서 
<p>actions => mutations => state</p>
<hr>

<h3>Actions</h3>
vuex는 처음에 actions에 context라는 object를 넣는다.<br> 
그다음으로 입력받은 payload (context, payload) 구조<br>
---> actions로 넘길때 this.$store.dispatch (action이름, value)<br>
---> mustations로 commit할때 this.$store.commit()<br>

addtodo( {commit, dispatch ....등이 있음}, value)
context 안에는 다음과같이 commit, dispatch....같은 다양한 기능이있음
actions에서 비동기가 일어나는 기능 axios , seTimeout....등 처리
일단 비동기한후에 commit처리 (mutations)

ex)
 actions: {
    addTodo( {commit}, value) {
//또는 axios.post() 서버에 요청 작업
// setTimeout으로 2초후에 commit하는 작업 등
      setTimeout(function() {
        commit('ADD_TODO', value); 
      }, 2000);
    }
  },

payload는 많이들 데이터를 통 느낌으로 보낼때 단일 value가 아닌 배열식일때 사용한다. convention같은것

<더미데이터>
https://jsonplaceholder.typicode.com/ 에서 더미데이터를 요청해서 받을수있다. 서버가 따로 없다면 이것을 이용해서 axios사용

axios promise가 리턴됨 (비동기)
그래서 만약 axios.get('https://jsonplaceholder.typicode.com/users').then(res => {}이런식  요청하면
요청을 보내고 promise를 리턴해준다.

axios요청을 보내놓고 밑에 작업을한다. 그뒤에 res 결과가 오면 res => {}에 {}가 실행됨

ex) 다음과같이 된다면
     axios.get('https://jsonplaceholder.typicode.com/users').then(res => {
            console.log(res);
        })
        const a = 1;
        console.log(a);
    }
아마 서버요청한뒤에 응답을기다리는게 아니라 바로 console.log(a)를찍고 그뒤에 res가 출력될것이다.
<hr>

<h3>사용할때 아래처럼 고려하면 편하다.</h3>

state를 가져올때는 computed 안에 작성하도록한다.

actions를 작성할때 vuex에서 어떤 context라는 object를 넣어준다.

이것만 기억하자
commit 뮤테이션방향
dispatch 액션방향

action
addtodo(context, value}
context안에 commit , dispatch 등등 여러개가있다.

MAPHELPER impot { mapstate } from 'vuex' 구조

computed: mapstate,getters
methods: mutations, mapActions가 들어간다.

관련된 로직끼리 묶어서 파일로 뺴내는 것을 modules이라한다. 
코드가 적은게 아니라면 사용하는게 맞다

-출처 코지코더를 보고 정리했다 -
