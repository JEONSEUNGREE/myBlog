---
title: 0_2_JPA_Yml
tags:
  - JPA
categories:
  - JPA
date: 2021-09-06 00:12:12
---

H2 DB 사용시 yml 세팅
윈도우 기준
1. h2 공홈 설치
2. 실행

![h2실행화면](/review_img/JPA/2.PNG)

위와같이 실행하고 bash로 홈의 파일들을 확인하면 아래와같이 testdsl.mv.db가 생성된다.

![testdsl.mv.db](/review_img/JPA/3.PNG)

그 다음부터는 jdbc:h2:tcp://localhost/~/testdsl 다음주소로 접속해야한다.

yml 설정 (설정파일이라고 보면된다. xml, json등 과 같이 특정 구조를 가지고있는 파일)

![yml설정](/review_img/JPA/4.PNG)

가벼운 프로젝트용이기 때문에 ddl-auto: create (서버실행시 db 테이블을 드랍하고 새로 만든다)
테스트코드용 yml도 따로 만들수있다. 테스트 폴더 내에 resource 생성후 yml 생성후 설정

jpa yml 옵션에서 show_sql: true는 하지않는게 좋다. 쿼리가 system.out.print로 출력되기때문에
만약에 쿼리에 물음표(?) 보고싶다면 debug 옵션도 있지만 p6spy를 통해서도 확인가능하다.
https://github.com/gavlyukovskiy/spring-boot-data-source-decorator 의 p6spy 라이브러리를 이용해서 볼수있다.



