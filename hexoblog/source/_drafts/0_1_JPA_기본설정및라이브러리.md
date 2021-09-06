---
title: 0_1_JPA_기본설정및라이브러리
tags:
  - JPA
categories:
  - JPA
date: 2021-09-06 00:12:01
---

![인텔리제이]](/review_img/JPA/1.PNG)

./gradlew dependencies - bash에서 프로젝트 내 폴더
의존관계확인 명령어

의존관계중에
spring-boot-starter-에 HikariCP라고 있는데 -커넥션 풀

Connect-pool :
웹 컨테이너(WAS)가 실행되면서 DB와 미리 connection(연결)을 해놓은 객체들을 pool에 저장해두었다가.
클라이언트 요청이 오면 connection을 빌려주고, 처리가 끝나면 다시 connection을 반납받아 pool에 저장하는 방식을 말합니다.

spring-boot-starter-loggin-logback:
우리가 자주쓰는 log기능 slf4j 인터페이스에서 log관련 구현체를 사용

core는 스프링부트스타터내에 있다.

테스트관련 의존관계
spring-boot-starter-test
Junit,spring-test,mockito(목객체만듬),assert-j 

참고로 junit4는 
@RunWith(SpringRunner.class) 어노테이션이 들어갔지만 junit5부터는 @SpringBootTest만 넣어주면된다.

서버 사이드 템플릿 쓰는경우 유용한 라이브러리 implementation 'org.springframework.boot:spring-boot-devtools'
서버사이드에서 혹시 정적파일 랜더링 매번하기 귀찮을시 위 라이브러리를 사용하면 수정하고 바로 확인할수있다. 
(수정하고 bulid에 recomplie만 해주면 html만 컴파일해줌)


스프링관련 언어가이드
https://spring.io/guides