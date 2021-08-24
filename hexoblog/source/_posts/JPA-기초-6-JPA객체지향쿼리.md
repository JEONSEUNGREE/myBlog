---
title: JPA-기초-6-JPA객체지향쿼리
tags:
  - JPA
categories:
  - JPA
date: 2021-08-24 00:22:36
---


JPQL
가장 단순한 조회 방법

JPA를 사용하면 엔티티 객체를 중심으로 개발 (객체가 있다고 생각하고 DB 중심에서 벗어난 생각)
하지만 특정 조건에있어서는 sql을 사용할 수 밖에는 없다.

JPA는 SQL을 추상화한 JPAQL이라는 객체 지향 쿼리 언어를 제공
SQL문법과 유사(SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN)지원

JPQL은 엔티티 객체를 대상으로 쿼리
SQL은 데이터 베이스 테이블을 대상으로 쿼리

SQL을 추상화해서 특정 DB SQL에 의존 X (실제 쿼리를 보면 각 DB vendor 에 맞게 쿼리 작성후 날림)

JPQL을 한마디로 정의하면 객체지향 SQL

    예 ) 
    String jpql = "select m From Member m where m.name like '%hello%'"; - 멤버 테이블이아닌 엔티티를 select 하는것, 별칭사용필수

    List<Member> result = em.createQuery(jpql, Member.class).getResultList();

결과 조회 API

    query.getResultList(): 결과가 하나 이상, 리스트 반환
    query.getSingleResult(): 결과가 정확히 하나, 단일 객체 반환(하나 초과 인경우 예외 발생)

파라미터 바인딩
이름 기준

    SELECT m FROM Member m where m.username=:username
    query.setParameter("username", usernameParam);

위치 기준

    SELECT m FROM Member m where m.username=?1
    query.setParameter(1, usernameParam);


프로젝션

    SELECT m FROM Member m -> 엔티티 프로젝션

    //멤버를 가져올때 멤버와 연관된 팀 가져오고싶을때 (팀 엔티티를 가져옴)
    SELECT m.team FROM Member m -> 엔티티 프로젝션

    //특정 엔티티 조회가 아닌 값을 가져옴
    SELECT m.username, m.age FROM Member m -> 단순 값 프로 젝션 

    //DTO로 조회 -> new 명령어
    SELECT new jpabook.jpql.UserDTO(m.username, m.age)
    FROM Member m

페이징 API 두가지

    setFirstResult(int starPosition) : 조회 시작 위치 (0부터시작)

    setMaxResults(int maxResult) : 조회할 데이터 수

예) 10번쨰부터 20개 가져오는 코드 (스프링에서는 이것조차 interface에서 전부 구현되있음)

    String jpql = "select m from Member m order by m.name desc";
    List<member> resultList = em.createQuery(jpql, Member.class)
    .setFirstResult(10)
    .setMaxResults(20)
    .getResultList();

    select
    COUNT(m), 회원수
    SUM(m.age), 나이합
    AVG(m.age), 평균나이
    MAX(m.age), 최대 나이
    MIN(m.age) 최소 나이
    from Member m

조인

    내부 조인: SELECT m FROM Member m [INNER] JOIN m.team t

    외부 조인: SELECT m FROM Member m LEFT [OUTER] JOIN m.team t

    세타 조인: SELECT count(m) FROM Member m, Team t where m.username = t.name

페치 조인 lazy로딩 적용하고 필요할때 한번에 조회(이렇게 하면 물론 단면적으로보면 현재코드에서는 lazy로딩 적용 의미 없긴하다.)
엔티티 객체 그래프를 한번에 조회하는 방법
별칭을 사용할 수 없다.

    JPQL: select m from Member m join fetch m.team
    SQL: SELECT M.*, T.*FROM MEMBER T INNER JOIN TEAM T ON M.TEAM_ID=T.ID

case식 가능

select 
	case when m.age <= then '학생요금'
	      when m.age >= then '경로요금'
	      else '일반요금'
	end
from Member m 


사용자 정의 함수 호출 가능
Named 쿼리 - 어노테이션 가능 (로딩시점에 파싱되서 로딩시점에 오류 체크 가능)

출처: https://www.youtube.com/watch?v=PMNSeD25Qko
[토크ON세미나] JPA 프로그래밍 기본기 다지기 6강 - JPA 내부구조 | T아카데미

 




