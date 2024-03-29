# JPA?

- 자바 진영의 ORM 기술 표준

### ORM?

- Object-relational mapping(객체 관계 매핑)
- 객체는 객체대로 설계
- 관계형 데이터베이스는 관계형 데이터베이스 대로 설계
- ORM 프레임 워크가 중간에서 매핑

JPA는 애플리케이션과 JDBC 사이에서 동작한다

![스크린샷 2023-06-10 오후 10.20.21.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3c077ffa-2a46-4ab2-9d5d-341bd55bd04b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.20.21.png)

![스크린샷 2023-06-10 오후 10.20.38.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/94d26898-408f-400d-95cc-18a411b8d34a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.20.38.png)

멤버 객체를 DAO에 넘기고 JPA에게 저장을 요청하면 JPA가 인서트 스크립트를 만들어주고 쿼리까지 DB에 날려준다.

![스크린샷 2023-06-10 오후 10.21.32.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e341d998-9bee-4b73-bf83-dec5a840b67f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.21.32.png)

엔티티 오브젝트를 만들어서 우리에게 반환해준다

EJB에겐 두 가지 기술이 있었다.

- 스프링에서 사용하는 컨테이너 영역
- 엔티티 빈

엔티티 빈은 사용하기가 어려웠다. 이걸 발전시켜서 오픈소스로 만든게 하이버네이트. 이걸 거의 복붙해서 만든게 자바 표준 JPA이다. JPA는 오픈소스에서 출발했기 때문에 실용적이다.

JPA는 인터페이스의 모음이다. 

- 객체 중심의 개발
- 생산성, 유지보수에 용이하다
- 패러다임의 불일치 해결
- 성능에 이점이 있다.

### 생산성

- 저장: jpa.persist(member)
- 조회: Member member = jpa.find(memnerId)
- 수정: member.setName(”변경할 이름”)
- 삭제: jpa.remove(member)

### 유지보수

필드만 추가하면 SQL은 JPA가 처리한다

### 1차 캐시와 동일성 보장

1. 같은 트랜잭션 안에서는 같은 엔티티를 반환한다 - 약간의 조회 성능 향상
2. DB Isolation Level이 Read Commit이어도 애플리케이션에서 Repeatable Read를 보장한다

![스크린샷 2023-06-10 오후 10.31.44.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9ab9b1fe-2db2-4c55-857d-6204b2179e76/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.31.44.png)

같은 트랜잭션 안에서만. 다른 트랜잭션 안에서는 안된다

### 트랜잭션을 지원하는 쓰기 지연 - INSERT

1. 트랜잭션을 커밋할 때까지 INSERT SQL을 모음
2. JDBC BATCH SQL 기능을 사용해서 한번에 SQL 전송

![스크린샷 2023-06-10 오후 10.33.20.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/786f45f6-de0c-432f-b7ee-183f875aaf38/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.33.20.png)

### 지연 로딩과 즉시 로딩

- 지연 로딩: 객체가 실제 사용될 때 로딩
- 즉시 로딩: JOIN SQL로 한번에 연관된 객체까지 미리 조회

@Entity: JPA가 관리할 객체

@Id: 데이터베이스 PK와 매핑

JPA를 사용하면 엔티티 객체를 중심으로 개발한다

문제는 검색 쿼리인데, 검색을 할 때도 테이블이 아닌 엔티티 객체를 대상으로 검색한다

모든 DB 데이터를 객체로 변환해서 검색하는 것은 불가능하다.

애플리케이션이 필요한 데이터만 DB에서 불러오려면 결국 검색 조건이 포함된 SQL이 필요하다

객체는 참조가 있는 방향으로만 조회를 할 수 있다.

Member가 Team 객체를 가지고 있으면 member.getTeam()으로 조회할 수 있다. 하지만 team.getMember()는 불가능

반면에 테이블은 외래키 하나로 MEMBER JOIN TEAM, TEAM JOIN MEMBER 가능

객체를 테이블에 맞춰 모델링하면 Member객체와 연관된 Team 객체를 참조를 통해 조회할 수 없다.

```java
// 객체를 통한 참조 불가능
class Member {
	String id; // MEMBER_ID 컬럼
	Long teamId; // TEAM_ID FK 사용
	....
}

// 객체지향 모델링
class Member{
	String id;
	Team team; // 참조로 연관관계를 맺는다
	...
}
```

객체지향 모델링을 사용하면 객체를 테이블에 저장하거나 조회하기가 쉽지 않다. 객체 모델은 외래 키가 필요없고 단지 참조만 있으면 된다. 반면 테이블은 참조가 필요 없고 외래 키만 있으면 된다

### 지연로딩

JPA는 연관된 객체를 사용하는 시점에 적절한 SELECT SQL을 실행한다. JPA를 이용하면 연관된 객체를 신뢰하고 마음껏 조회할 수 있다. 이 기능은 실제 객체를 사용하는 시점까지 데이터베이스 조회를 미룬다고 해서 지연 로딩이라고 한다. JPA는 지연 로딩을 투명하게 처리한다

### 비교

데이터베이스는 기본 키의 값으로 각 로우를 구분한다. 반면에 객체는 동일성 비교와 동등성 비교라는 두가지 비교 방법이 있다

- 동일성 비교: ==. 객체 인스턴스의 주소값 비교
- 동등성 비교: equals() 메소드 사용. 객체 내부의 값 비교

## 영속성 컨텍스트

- 엔티티를 영구 저장하는 환경
- EntityManager.persist(entity);
- 엔티티 매니저를 통해서 영속성 컨텍스트에 접근

### 영속성 컨텍스트의 이점

- 1차 캐시: 한번 조회한거 또 조회하면 DB에 쿼리가 안날라감. 동일한 트랜잭션 안에서.
- 동일성 보장
- 트랜잭션을 지원하는 쓰기 지연: 버퍼링
- 변경 감지
- 지연 로딩: 조회할때 쿼리를 나중에 날림

# 객체와 테이블 매핑

## 엔티티 매핑 소개

- 객체와 테이블 매핑: @Entity, @Table
- 필드와 컬럼 매핑: @Column
- 기본 키 매핑: @Id
- 연관관계 매핑: @ManyToOne, @JoinColumn - 일대다, 다대일 관계에서 JPA에서 어떻게 매핑해줄지

### @Entity

- 이 어노테이션이 붙은 클래스는 JPA가 관리, 엔티티라고 한다
- JPA를 사용해서 테이블과 매핑할 클래스는 @Entity 필수
- **주의**
    - 기본 생성자 필수(파라미터가 없는 public 또는 protected 생성자)
    - final 클래스, enum, interface, inner 클래스 사용 x
    - 저장할 필드에 final 사용 x

![스크린샷 2023-06-11 오후 12.35.46.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/77e2d497-78ff-4114-90eb-aebe1f32bb04/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.35.46.png)

![스크린샷 2023-06-11 오후 12.35.21.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5c92b1dc-a4d1-44a8-bff8-ae918d355f8c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.35.21.png)

### @Transient

- 필드 매핑 X
- 데이터베이스에 저장 X, 조회 X
- 주로 메모리상에서만 임시로 어떤 값을 보관하고 싶을 때 사용