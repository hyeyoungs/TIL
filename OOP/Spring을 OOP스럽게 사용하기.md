# 스프링에서 OOP와 안티 패턴 : SmartUI, Transaction script

<br>

## SmartUI
- 백엔드에서 UI란? Controller
- 스마트 컨트롤러를 피하기
- **Controller**는 어떤 서비스를 실행할지 선택하는 정도의 역할만

<br>

### Layered systen at DDD
#### Service
소프트웨어가 수행할 작업을 정의하고 표현력있는 도메인 객체가 문제를 해결하는 레이어, 다른 시스템의 응용 계층과 상호작용하는데 필요한 것들
<br>이 계층은 최대한 얇게 유지 되어야 함
<br>오직 작업을 조정하고 아래에 위치한 계층에 도메인 객체의 협력자에게 작업을 위임함

<img width="800" alt="image" src="https://github.com/hyeyoungs/TIL/assets/29566893/c0560d6a-90f3-47af-9a10-caf485253470">
