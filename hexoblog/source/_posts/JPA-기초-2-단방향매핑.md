---
title: JPA-기초-2-단방향매핑
tags:
  - JPA
categories:
  - JPA
date: 2021-08-24 00:22:05
---


아래는 각 객체간의 연관관계가 없이 두번 조회를 통해 값을 알아내는 방법이다.
구조는 다음과 같다.

    @Entity
    public class Memeber {
	@Id
	@GeneratedValue
	private Long id;

	@Column(name = "USERNAME")
	private String name;
	private int age;

	@Column(name = "TEAM_ID"
	private Long teamId;
    }

    @Entity
    public class Team {
	@Id
	@GeneratedValue
	private Long id;
	private String name;
    }

저장시 다음과같이 팩토리, 매니저 생성 
(팩토리의경우는 비용이 크지만 매니저의 경우 비용이 적다) - 자바 ORM 표준 JPA프로그래밍

    EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
    EntityManager em = emf.createEntityManager();

    EntityTransaction tx = em.getTransaction();
    tx.begin();

    팀저장
    Team team = new Team();
    team.setName("TeamA");
    em.persist(team);

    멤버저장
    Member member = new Member();
    member.setName("member1");
    member.setTeamId(team.getId()); <- team id
    em.persist(member);

팀을 조회하기 위해서는 연관관계가 없기때문에 두번 조회가 필요하다. (엔티티 매니저를 통해서 접근)
객체 지향적인 방법은 아니다. (토이프로젝트 진행시 JPA를 사용할줄 몰라서 이런식으로 사용했었다.)
왜냐면 객체를 DB중심적으로 설계됐기때문이다. 객체중심적이었다면 접근 방식이 당연히 다르다.

조회 하는 순서 2번
1번
Member findMember = em.find(Member.class, member.getId());
Long teamId = findMember.getTeamId();

2번 연관관계가없다.

    Team findTeam = em.find(Team.class, team.getId());
    em.find(Team.class, teamId);

객체를 테이블에 맞추어 데이터 중심으로 모델링하면, 협력관계를 만들수없다는 문제가 존재한다.

본격적인 연관관계 매핑

단방향 매핑 
Member -> Team
Member는 Team을 참조할수있다.

    @Entity
    public class Memeber {
	@Id
	@GeneratedValue
	private Long id;

	@Column(name = "USERNAME")
	private String name;
	private int age;

아래부분을 수정한다.

    //	@Column(name = "TEAM_ID"
    //	private Long teamId;

    다음과같이 team 객체를 넣어준다.
    하나의 팀이 여러 멤버를 갖기때문에 ManyToOne (멤버 입장에서 생각했을때 다대일관계)
	@ManyToOne
	@JoinColumn(name = "TEAM_ID")
	private Team team;
    }

위와같이 설계했을때 객체 지향적 모델링이 된다
Member의 team 객체가 team_id(FK) 와 매핑이 된다.

저장방식

    팀
    Team team = new Team();
    team.setName("TeamA:)'
    em.persist(team);

    회원
    Member member = new Member();
    member.setName("member1");
    member.setTeam(team); //그냥 객체 그대로 넣어버리면된다.
    em.persist(member);

조회시 (참조연관관계로 조회 - 객체 그래프 탐색)

    Member findMember = em.find(Member.class, member.getId());

    Team findTeam= findMember.getTeam();


    jpa는 내부에 캐싱이되서 만약 select 쿼리를 보고싶으면 
    em.flush(); db쿼리를 보내준다.(뒤에 나오지만 비우는게아니다. 쓰기지연을 한번에 보내준다.)
    em.clear(); cash를 비운다.

또한 기본값이 join이기때문에 member만 가져오고 싶다면 fetch옵션을 lazy로 두면 된다. 기본값이 EAGER

    @ManyToOne(fetch = FetchType.LAZY)

출처: https://www.youtube.com/watch?v=bEtTpCviSc4 
[토크ON세미나] JPA 프로그래밍 기본기 다지기 4강 - 연관관계 매핑 | T아카데미