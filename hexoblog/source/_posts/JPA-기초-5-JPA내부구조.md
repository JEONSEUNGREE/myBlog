---
title: JPA-기초-5-JPA내부구조
tags:
  - JPA
categories:
  - JPA
date: 2021-08-24 00:22:31
---


엔티티 매니저 요청이 올 때마다 스레드가 생성되서는 처리함.

영속성 컨텍스트 (=엔티티 매니저)
JPA를 이해하는데 중요한 용어
엔티티를 영구 저장하는 환경이라는 뜻
EntityManager.persist(entity)

엔티티매니저가 영속성 컨텍스트(눈에 보이지않는 영역) 

스프링프레임워크 같은 컨테이너환경 (엔티티 매니저를 주입 받아서 사용가능) 
엔티티매니저 N : 영속성 컨텍스트 1 - N:1관계
만약 a,b,c서비스가 같은 트랜잭션이면 같은 영속성 컨텍스트 접근

엔티티의 생명주기
비영속
영속성 컨텍스트와 전혀관계가 없는 상태
예)
생각해보면 프로젝트할때 실수로 set만 해놓고 .save() 빼먹으면 저장이되지않았다. - 이것을 비영속 상태라고한다. JPA와 관계가없다.

영속 
영속성 컨텍스트에 저장된 상태

    EntityManager em = emf.createEntityManager();
    em.getTransaction().begin();
    //미리 멤버객체를 만들고 set한 가정
    em.persist(member) - 객체를 저장환 상태 (영속)

준영속
영속석 컨텍스트에 저장되었다가 분리된 상태 - 값이 변해도 더이상 db 반영이 되지않는다.

    em.detach(member);
    em.clear()
    em.close()

삭제
삭제된 상태

    em.remove(member0;

왜 이런 비영속,준영속이있나? 영속성컨텍스트의 이점

엔티티 조회, 1차 캐시
영속컨텍스트 안에는 여러 기능이있는데 이중 1차캐시라는것이 존재한다.
영속컨텍스트가 존재할때만 잠깐 있는 것 내부 메모리공간 정도

여기서 조회하게되면 db로 가는게 아니라 1차캐시를 찾고 db로 이동하게됨 (한개의 트랜잭션에서 잠깐 발생됨 따라서 공유X)
그리고 db에서 찾게되도 1차캐시로 저장하고 반환됨 

따라서 동일성보장

    Member a = em.find(Member.class, "member1");
    member b = em.find(Memeber.class, "member1");
    a == b true
1차캐시내에서 같은 레퍼런스를 물고있는거다.

트랜잭션을 지원하는 쓰기 지연 (버퍼기능)

    transaction.begin();

    em.persist(memberA); -1차캐시저장 sql만 생성
    em.persist(memberB); -1차캐시저장 sql만 생성
    // 아직 DB insert X

    transaction.commit(); 커밋하는 순간 insert쿼리 전송 (물론 쿼리문은 두번날아감)

쓰기지연을 보내는 sql쿼리를 보내는 과정을 flush라고 함 3강에서했던 캐시 전송(비운다기보다 DB와 sync) 썻던예제를 보면 flush,clear()에서 flush
commit은 (flush+commit기능)

변경감지 (우리가 update쿼리를 설정하지않는이유)
조회후에
set수정
(update쿼리X)
transaction.commit(); 

사실 1차캐시 말고 1차캐시 생성시점에서 스냅샷이라는 것도 존재하는데 jpa를 flush하는 시점에 비교해서 바뀐부분이있으면 자동으로 update쿼리를 만든다
오해하면안되는 부분 flush는 컨텍스트를 비우는것이아닌 db와 sync 하는 목적
비우는것은 아까 clear()메서드다.

flush의 경우에는
em.flush() 직접호출
트랜잭션 커밋 -commit + flush
jpql 쿼리 실행 - 플로시 자동호출 ( 이부분이 em.setName(something1), em.setName(something2) 이 값 중간에 끼워져있다면 
자동으로 flush가 되고 jqpl쿼리가 실행되면서 flush 되버린다. 
이유: 만약에 jpql 쿼리의 flush가 연동되지않는다면 나중에 값이 조회되지않는 문제가 발생하기때문에 commit은 안됨 (실수방지 자동)
그래서 mybatis 또는 jdbcTemplate의 경우 select값이 안나올수가있었는데 영속성 컨텍스트가 flush되지않아서

지연로딩에 관해
(지연로딩을 하려면 영속성 컨텍스트가 살아있어야하는데 이는 컨트롤러로 나갈때 빠지게된다. LazyInitializationException
따라서 한 트랜잭션이 끝나고 컨트롤러에서 lazy로딩시 오류 발생 - 준영속성 상태 해결- 준영속전에 미리 로딩하는 등의 방법)
우리가 member만 조회하고싶을때 lazy 옵션을 적용했는데 이때 null Exception이 터지지않는 이유는 프록시로 조회됬기때문이다.
가짜 값을 넣어주고 실제 조회할때 값을 가져오게된다.

가급적 지연로딩을 사용해야하는 이유
연쇄적으로 작용을 방지하기위함 ( member에서 팀을 eager로 건다 근데 team에도 eager 걸려있다면 많은 sql이 작용하다 문제가 발생한다.)
필요할때 fetch를 사용하면된다.


출처: https://www.youtube.com/watch?v=wt_BEqxjaj8
[토크ON세미나] JPA 프로그래밍 기본기 다지기 7강 - JPA 객체지향쿼리 | T아카데미