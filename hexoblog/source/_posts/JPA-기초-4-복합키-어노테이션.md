---
title: JPA-기초-4-복합키 어노테이션
tags:
  - JPA
categories:
  - JPA
date: 2021-08-24 00:22:21
---


복합키 어노테이션 (pk 여러개설정시)
실제 비즈니스 로직에서는 pk값 사용시 꼬일수있다. (그래서 auto_increment ,sequence 같은 값을 사용)
예를 예전에 주민번호를 pk값으로 설정했다고한다.
근데 개인정보보호법으로 db에 저장불가로 둠 과 동시에 모든 pk값을 바꾸게된다.
@IdClass 
@EmbeddedId
@Embeddable
@MapsId


출처: https://www.youtube.com/watch?v=0zTtkIYMOIw
[토크ON세미나] JPA 프로그래밍 기본기 다지기 5강 - 양방향 매핑 | T아카데미
