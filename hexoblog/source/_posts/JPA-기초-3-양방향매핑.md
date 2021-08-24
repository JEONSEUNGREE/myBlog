---
title: JPA-기초-3-양방향매핑
tags:
  - JPA
categories:
  - JPA
date: 2021-08-24 00:22:10
---


-------------------제일 중요한 부분--------------------------

Member 에서도 Team 접근하고 Team에서도 Member접근하도록하는것

보면 DB의 경우는 객체 처럼 member.team 또는 team.member가아닌 다 접근이 가능하다.

멤버의 경우 단방향 매핑이 둘다 있는 것과 같다.

	@Entity
	public class Memeber {
	@Id
	@GeneratedValue
	private Long id;

	@Column(name = "USERNAME")
	private String name;
	private int age;

	@ManyToOne
	@JoinColumn(name = "TEAM_ID")
	private Team team;
	}

	팀의 경우 매핑이 다르다. Team 엔티티는 컬렉션 추가
	@Entity
	public class Team {
	@Id
	@GenerateValue
	private Long id;
	private String name;
	
	@OneToMany(mappedBy = "team")
	List<Memeber> members = new ArrayList<Member>();
	}

	조회
	Team findTeam = em.find(Team.class, team.getId());

	int memberSize = findTeam.getMembers().size(); 역방향조회

왜 @OneTomay만 적으면 될것같은데 mappedBy라는 옵션이 추가되었는가

------------mappedBy란?---------------
만약에 현재 Member에다가 team을 set하는게 아니라 Team에다가 member를 set하는경우도 업데이트가 될것이다.
이부분에서 누가 주인인지 지정해야할 필요가있다. 둘중에 하나만이 업데이트 값을 쥘수있고 하나는 조회만 가능하게 하는 것. 
Member를 주인으로 정하면 member.setTeam하면 되고
만약 Team을 주인으로 정하면 Member.team().getmembers.add() 하면 업데이트가 된다.
둘다 동시에 넣으면 주인의 것 하나만 업데이트가 되게한다.

주인 지정 방법 mappedBy (mappedBy를 썼다는것 자체가 어딘가 매핑되었다는 소리 Joinculumn 없음 매핑이 되었을뿐)
주인의 것만 영향을 주지 mappedBy를 사용한 부분은 db업데이트의 영향을 미치지않는다 조회용으로 사용하겠다는 의미

결론: 테이블과 다르게 객체에서는 누가 연관관계 주인인지 정할필요가 있다. 주인만 업데이트가능하다.
하지만 객체지향적으로는 양쪽 team과 Member 모두 set,add 해줄 필요가있다.

그럼 누구를 주인으로 정하나?
외래키가 있는 곳을 주인으로 정하는 것. (사실 케바케지만 일반적으로 인지부조화문제때문)

Member: 현재 외래키 갖고있음
Team: PK = FK
이유:  분명 Team을 업데이트 했는데 Member가 업데이트쿼리가 날라감 (혼란)

어지간해서는 단방향 필요할때 양방향으로 설정

객체 연관관계
회원 -> 팀 (단방향)
팀 -> 회원 (단방향)

테이블
회원 <-> 팀 (양방향)

객체는 사실 양방향 관계가 아니라 서로다른 단방향이 2개다. (양방향처럼 보이는것 뿐)
테이블은 외래키 하나로 두테이블을 관리 가능
다음과같이
	SELECT * FROM MEMBER M JOIN TEAM T ON M.TEAM_ID = T.TEAM_ID
	SELECT * FROM TEAM T JOIN MEMBER M ON T.TEAM_ID = M.TEAM_ID

주인관계를 잘 모를때 실수하는 부분(현재 mappedBy가 team)

Team team = new Team();
team.setName("TeamA");

em.persist(team);

Member member = new Member();
member.setname("member1");

역방향으로 업데이트 설정 (잘못된  예)
team.getMembers().add(member);

주인에서 업데이트(올바른 예)
member.setTeam(team);
em.persist(member);

위와같이 실행한 경우 당연 업데이트 되지않음

출처: https://www.youtube.com/watch?v=bEtTpCviSc4 
[토크ON세미나] JPA 프로그래밍 기본기 다지기 4강 - 연관관계 매핑 | T아카데미

출처: https://www.youtube.com/watch?v=0zTtkIYMOIw 
[토크ON세미나] JPA 프로그래밍 기본기 다지기 5강 - 양방향 매핑 | T아카데미
