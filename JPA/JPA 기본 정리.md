# JPA 기본 정리
## JPA
### JPA VS Hibernate VS Spring-data-jpa
- spring-data-jpa : JPA를 더 쉽게 사용할 수 있도록 ex) interface JpaRepository @Query, @Modifying ...
- JPA : 기술 명세 (자바 측에서 정한 인터페이스) ex) @Entity, @Table, @Column...
- Hibernate : JPA의 구현체

![image](https://github.com/hyeyoungs/TIL/assets/29566893/62da2b2c-2848-48db-bd27-d07d537a91e5)

<br>

### 연관 관계의 주인
#### 관계를 표현하는데 있어서 가장 중요한 것 = 연관 관계의 핵심 = 외래키
ex. team_id

![image](https://github.com/hyeyoungs/TIL/assets/29566893/22dacd0b-a5fc-4b01-bd48-d2c9310aa2af)

<br>

- mappedBy : 나는 연관관계의 주인이 아님 (연관 관계의 주인은 외래키에 맵핑된 객체)
- 아예 양방향 안 만드는게 좋음

![image](https://github.com/hyeyoungs/TIL/assets/29566893/a246e98d-aa4a-4962-93b8-91357ab3e94c)

<br>

### N + 1
백엔드 팀을 하나 불러오기 위해, n명의 팀원 + 1번의 팀을 불러오는 쿼리가 나가는 경우
<br>
- 지연 로딩 - 연관 데이터를 필요할 때 불러옴 (getStaffs()할 때 n번 쿼리가 나감)
- 즉시 로딩 - 처음부터 연관 데이터를 불러옴 (처음부터 n번 쿼리가 나감)

<br>n+1은 Fetch 방식의 문제는 아님
#### 해결법
- EntityGraph
    - ![image](https://github.com/hyeyoungs/TIL/assets/29566893/6457e2e5-1907-49ad-b2f8-c844bd83c271)
- FetchJoin
    - ![image](https://github.com/hyeyoungs/TIL/assets/29566893/5a5ccb68-17c0-472c-ac9f-fef72cf16a2a)
- 구조적 해결법
    - classRepositoryImpl
        - ![image](https://github.com/hyeyoungs/TIL/assets/29566893/4d3a4d7d-e7bd-44f6-b917-5029cefecf6e)
        - 팀을 가져오는 쿼리를 한번 받고, 팀에 소속된 직원을 가져오는 쿼리를 한번 더 하는 것
        - 그렇게 가져온 데이터를 조합해서 도메인으로 변환하고 반환하는 것
