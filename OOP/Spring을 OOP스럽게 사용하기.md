# 스프링에서 OOP와 안티 패턴 : SmartUI, Transaction script

## SmartUI
- 백엔드에서 UI란? Controller
- 스마트 컨트롤러를 피하기
- **Controller**는 `어떤 서비스를 실행할지 선택`하는 정도의 역할만

<br>

### Layered systen at DDD
#### Controller
어떤 서비스를 실행할지 선택하는 역할
<br>

#### Service
소프트웨어가 수행할 작업을 정의하고 표현력있는 도메인 객체가 문제를 해결하는 레이어, 다른 시스템의 응용 계층과 상호작용하는데 필요한 것들
<br>`이 계층은 최대한 얇게` 유지 되어야 함
<br>오직 작업을 조정하고 아래에 위치한 계층에 `도메인 객체의 협력자에게 작업을 위임`함

<img width="800" alt="image" src="https://github.com/hyeyoungs/TIL/assets/29566893/c0560d6a-90f3-47af-9a10-caf485253470">
<br>

#### Domain model
`업무 개념과 업무 상황에 관한 정보, 업무 규칙을 표현하는 일을 책임`짐
<br>상태 저장과 관련된 기술적인 세부사항은 **인프라스트럭쳐**에 위임함

<br>

## Transaction script
아마도 스프링을 처음 배운 코드

- Service
  - 두꺼운 컴포넌트, 객체가 모두 수동적으로 동작함
  - 서비스가 거의 신
  - 모든 객체에 대한 접근 권한을 갖고있고 모든 분기를 다 처리하는 코드가 되어버림
  - <img width="800" alt="image" src="https://github.com/hyeyoungs/TIL/assets/29566893/b16e06f7-f060-4ab9-b67e-b442ab733f5c">
 
- Repository
  - 추상화하지 않고 JPA Repostiory를 다이렉트로 접근해서 사용
  - <img width="800" alt="image" src="https://github.com/hyeyoungs/TIL/assets/29566893/faab75d1-8927-488d-bc39-5a6877ea32ea">


<br>🥲 테스트하기도 힘들고 전혀 객체지향스럽지도 않음
<br>😅 서비스 코드가 트랜잭션을 실행하는 스크립트의 역할일 뿐
<br>😂 비즈니스 로직을 서비스가 들고 있어서 생기는 문제

#### 서비스는 작업을 조정하고, 비즈니스 로직을 도메인에게 위임해야 함
#### 비즈니스 로직을 제발 도메인이 들고 있게 하기


- Service의 과한 책임을 객체에 위임함으로써
  - 객체가 능동적
  - Service가 얇아짐
  - Service는 Repository 결합되지 않으니, 테스트 하기 쉬워짐 <br>(객체를 테스트할 때, 불필요한 어노테이션이 필요없고 Repository에 대한 의존성이 사라짐)
 
<br><br>

# 어디까지 추상화 해야 하는가?
## 추상화
> 추상화 : 복잡한 자료, 모듈, 시스템 등으로부터 핵심적인 개념 또는 기능은 간추려 내는 것 <br>`모듈을 격리하고 인터페이스로 만드는 과정`
<br>

### 시스템 외부 연동(DB, WebClient ➡️ `Repository`)은 `추상화`
#### 시스템 외부 연동 추상화하지 않은 경우
Business Layer가 JPA Repository에 강결합 되어있기 때문에 인프라가 변경되면, Business Layer도 변경되어야 함
#### 시스템 외부 연동 추상화한 경우
- JPA에서 Mongo로 바꿔도 Service Layer(=Business Layer)에 변경 없이 수정이 가능<br>Repository를 선택적으로 변경할 수 있음
- Service 로직 테스트하고 싶을 때, 테스트 하기 쉬워짐<br>(h2나 mock 라이브러리 없어도 자연스러운 테스트를 할 수 있음)

### `서비스`는 `구현체`로 

### `도메인 영역`(= 핵심 로직)은 `풍부하게 만들기`, 필요에 따라 추상화

![image](https://github.com/hyeyoungs/TIL/assets/29566893/34ce20ee-16e6-42ad-94f6-26412b976386)


<br>

## 설계
#### 설계에 정답은 없음
#### 확실한 건 테스트하기 좋은 코드는 좋은 설계일 확률이 높음

<br><br>

# 서비스란 무엇인가?
## Service
> Service : 도메인과 도메인 서비스에게 책임을 위임하는 Facade 패턴의 일종

<br>

### DDD에서 말하는 서비스
서비스는 이름 끝에 "Manager"와 같은 것이 붙음

![image](https://github.com/hyeyoungs/TIL/assets/29566893/e91b6ee3-22f8-4b55-845d-7996532c06a6)

항상 내가 짠 코드가 절차지향적인 코드가 되지 않았는지 경계하고 객체로 분할해야 됨

- ProductService는 애플리케이션 서비스
- Repository에 접근하는 동작은 어떤 도메인도 들고 있기 애매하므로 Service가 들고 있는 것이 맞음
- PriceCalculator 같은 객체를 도메인 서비스라고 부름

![image](https://github.com/hyeyoungs/TIL/assets/29566893/fa21b953-a005-49b0-877f-5e32ac7831a6)

- 가격 계산이라는 로직을 도메인이 들고있게 할 수 있다면 그게 더 나음

<br>

### 중요
#### 중요한 건 풍부한 도메인을 만들라는 것
#### 서비스는 가능한 적게 만들고 얇게 만들라는 것

<br>

## 서비스에 관한 조언
### 오브젝트 디자인에서 말하는 서비스
한번 생성하면 특정 작업을 하는 작은 기계처럼 영원히(= 불변성) 실행할 수 있음. 이러한 객체를 서비스라고 함

<br>

### 서비스의 불변성
#### 생성자 주입만 사용함
- 순환참조를 막아줌
- 서비스를 분변이어야 함. 즉 인스턴스 생성을 마친 후에는 바꿀 수 없어야 함

> 생성자 주입을 해야하는 이유 : 한번 생성으로 영원히 일을 하는 일관된 객체를 만들어야 하기 때문

<br>

### RequiredArgsConstructor
미관상 생성자 자체를 넣는게 예쁘지 않다고 느껴진다면 `@RequiredArgsConstructor` + `private final`을 이용하자

![image](https://github.com/hyeyoungs/TIL/assets/29566893/70ee41ca-3165-4ac4-81fa-e454c3ebbb26)

<br>

### 순환 참조는 피하자

![image](https://github.com/hyeyoungs/TIL/assets/29566893/1292c18b-af08-4fa1-a591-6ea1ce430024)

<br>

### 중요
#### 서비스는 얇게 유지
#### 서비스의 멤버 변수는 모두 final로 유지
#### 서비스에 setter가 존재한다면 지우기
#### 반드시 생성자 주입으로 변경

# 기타 꿀팁
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
