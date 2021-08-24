---
title: JPA-기초-7-JPA_Spring_QueryDSL
tags:
  - JPA
categories:
  - JPA
date: 2021-08-24 00:22:41
---


스프링 데이터 JPA
우리가 현재 스프링부트에서 사용하는 JPA - 독일의 모 개발자
em.persist() - save()
em.find() - find()  이런것들이 구현된것이다. 우리가 CRUD를 했던 모든 코드가 인터페이스화 되서 적용된 산물로 보면 될 것 같다.
인터페이스 상속을 통한 구현 JPAREPOSITORY<타입, 식별자> 이런식으로 사용  

스프링에 로딩 시점에 구현체를 만들어서 각각의 레포가 적용됨   

Spring Data Jpa ---생성----> somethingRepo(구현 클래스) ------> somthing<interface>

    스프링 데이터-------------------(db 넣었다 빼는 기능)

    기본기능들(CrudRepository)
    save(S) : S
    findOne(ID) : T
    exists(ID) : boolean
    count() : long
    delete(T).....

    PaginAndSortingRepository
    findAll(Sort) : Iterable<T>
    findAll(Pagealbe) : page<T>

    다양한 API가 있지만 그중에서 2개

    정렬 기능
    public interface MemberRepository extends JpaRepository<Member, Long> {
	List<Member> findByName(String username, Sort sort);
    }

    SELECT * FROM MEMBER M WHERE M.NAME = 'hello' ORDER BY AGE DESC

    페이징기능
    controller쪽에서
    Page<somthing>
    PaageRequest pageRequest = new PageRequest(1, 10); 현재페이지1 부터 10개 
    repository.findByName("somthing", pageRequest);

    repository 
    public interface MemberRepository extends JpaRepository<Member, Long> {
	Page<Member> findByName (String username, Pagealbe pagealbe);
    }

    스프링 데이터-------------------



    스프링 데이터 JPA-------------------(기본으로 공통 JPA기능)

    JpaRepsitory (jpa공통 기능)
    findAll() : List<T>
    findAll(Sort) : List<T>
    findAll(Iteralbe<ID>) : List<T>
    save(Iterable<S>) : List<S>
    flush............

    스프링 데이터 JPA-------------------

named 쿼리 어노테이션 처럼 
우리가 스프링 부트 실행시 Jparepository의 쿼리문을 파싱하는데 이때 문법오류를 알아서 걸러준다.
- 프로젝트하면서 jpa레포에서 쿼리 실수하면 났었던 그 오류인것같다. 확실히 좋은것같다. -


QueryDSL
문자가 아닌 코드로 작성
컴파일 시점에 문법 오류 발견
코드 자동완성
코드모양이 JPQL과 거의 비슷
동적 쿼리 - 가장 큰 장점
jpql문법 전부 지원해줌

JPAQueryFactory query = new JPAQueryFactory(em);
QMember m = QMember.member;

List<Memeber> hello = query.selectFrom(m)
.where(m.age.gt(18).and(m.name.contains("hello")))
.orderBy(m.age.desc())
.fetch();

queryDSL로 한번 wrap하였다.
queryDSL이 자바코드로 jpql을 generate 해준다.

쿼리로 실수할 일이없다. 실행할필요없이 오류를 자동완성되고 오류를 잡아주는 장점이있다. 
가장큰 장점
JPQL을 문자열을 더하지만 QueryDSL은 동적쿼리를 작성해서 자바코드처럼 작성가능

String name = "member";
int age = 9;

QMember m = QMember.member;

BooleanBuilder builder = new BooleanBuilder();
if (name != null) {
	builder.and(m.name.contains(name));
}
if (age != 0) {
	builder.and(m.age.gt(age);
}

List<Member> list = 
query.selectFrom(m)
.where(builder)
.fetch();

그렇기때문에 리팩토링 (공통적인부분 메서드로) 가독성 모든 부분에서 자바처럼 코드를 짤수있다. 
쿼리지만 자바코드로 해결이가능한점.

느낀점:
강의를 듣는데 빨려들어갔다. 
양심있으면 책사야된다 이미샀지만. 책을 사서 공부하지만 역시 강의로 듣는건 다르다.
특히 마지막에 QueryDSL은 무조건 습득하겠다. 
이제 JPA 튜토리얼 끝이다.
이제 적용하면서 습득하면 될 것 같다.

출처: https://www.youtube.com/watch?v=gRqyzi9VGYc
[토크ON세미나] JPA 프로그래밍 기본기 다지기 8강 - Spring Data JPA와 QueryDSL 이해 | T아카데미


