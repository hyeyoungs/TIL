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
<br>`오직 작업을 조정`하고 아래에 위치한 계층에 `도메인 객체의 협력자에게 작업을 위임`함

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

<br><br>

# 어디까지 추상화 해야 하는가?
## 추상화
> 추상화 : 복잡한 자료, 모듈, 시스템 등으로부터 핵심적인 개념 또는 기능은 간추려 내는 것 <br>`모듈을 격리하고 인터페이스로 만드는 과정`
<br>

### 시스템 외부 연동(DB, WebClient ➡️ `Repository`)은 `추상화`
#### 시스템 외부 연동 추상화하지 않은 경우
Business Layer가 JPA Repository에 강결합 되어있기 때문에 인프라가 변경되면, Business Layer도 변경되어야 함
<br>
#### 시스템 외부 연동 추상화한 경우
- JPA에서 Mongo로 바꿔도 Service Layer(=Business Layer)에 변경 없이 수정이 가능<br>Repository를 선택적으로 변경할 수 있음
- Service 로직 테스트하고 싶을 때, 테스트 하기 쉬워짐<br>(h2나 mock 라이브러리 없어도 자연스러운 테스트를 할 수 있음)

### `서비스`는 `구체`로 

### `도메인 영역`(= 핵심 로직)은 `풍부하게 만들기`, 필요에 따라 추상화

<br>

## 설계
### 설계에 정답은 없음
### 확실한 건 테스트하기 좋은 코드는 좋은 설계일 확률이 높음
