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