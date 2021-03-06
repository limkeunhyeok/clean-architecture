# Clean Architecture

## 03 설계 원칙

### 7장 SRP: 단일 설계 원칙

> 단일 책임의 원칙(SRP)은 함수는 반드시 하나의 일만 해야 한다는 원칙이 아닌, 단일 모듈의 변경의 이유가 하나뿐이어야 한다는 원칙이다. 변경의 이유란 사용자와 이해관계자를 가리킨다. 즉, 하나의 모듈은 오직 하나의 사용자 또는 이해관계자에 대해서만 책임져야 한다.

- 모듈은 단순히 함수와 데이터 구조로 구성된 응집된 집합이다.
  - 단일 액터(actor)를 책임지는 코드를 함께 묶어주는 힘이 바로 응집성(cohesion)이다.

#### 징후 1: 우발적 중복

![employee](https://media.vlpt.us/images/hellojihyoung/post/29478a24-4a1f-42df-b1b7-583cba6e8568/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-04-03%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%202.26.35.png)

- 위 클래스는 SRP를 위반하는데, 이들 세 가지 메서드가 서로 다른 세 명세의 액터를 책임지기 때문이다.
  - calculatePay() 메서드는 회계팀에서 기능을 정의하며, CFO 보고를 위해 사용한다.
  - reportHours() 메서드는 인사팀에서 기능을 정의하고 사용하며, COO 보고를 위해 사용한다.
  - save() 메서드는 데이터베이스 관리자가 기능을 정의하고, CTO 보고를 위해 사용한다.
- 만약 CFO 팀에서 초과 근무를 제외한 업무 시간을 계산하는 방식을 약산 수정하려 할 때, 인사를 담당하는 COO 팀에서는 초과 근무를 제외한 업무 시간을 CFO 팀과는 다른 목적으로 사용하기 때문에, 이 같은 변경을 원하지 않는다.

> 따라서, 서로 다른 액터가 의존하는 코드를 서로 분리해야 한다.

#### 징후 2: 병합

- 하나의 소스 파일에 다양하고 많은 메서드를 포함하면 병합이 자주 발생한다.
- 만약 DBA가 속한 CTO 팀에서 데이터베이스의 Employee 테이블 스키마를 수정하기로 하고, 이와 동시에 인사 담당자가 속한 COO 팀에서 reportHours() 메서드의 보고서 포맷을 변경하기로 할 때, 이 두 개발자의 변경사항은 서로 충돌하고, 결과적으로 병합이 발생한다.

> 이 문제를 벗어나는 방법은 서로 다른 액터를 뒷받침하는 코드를 서로 분리하는 것이다.

#### 해결책

- 해결책은 데이터와 메소드를 분리하는 방식이다.
  - 메서드가 없는 클래스를 만들어 세개의 클래스를 공유하여 세 클래스가 서로의 존재를 모르게 한다.
  - 이로인해 우연한 중복을 피할 수 있다.

![employee data](https://media.vlpt.us/images/hellojihyoung/post/f7348f32-aef4-41c3-ba15-f33a7829da09/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-04-03%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%203.03.11.png)

- 위의 해결책은 개발자가 세 가지 클래스를 인스턴스화하고 추적해야 한다는 게 단점이다.

![facade pattern](https://miro.medium.com/max/1400/1*BQaFMMrIhk_89LXGRMsNMQ.jpeg)

- 퍼사드 패턴은 건물의 정면을 의미하는 단어로, 사용자가 어떤 행위를 위해 사용하는 서브 클래스들 사이의 간단한 통합 인터페이스를 제공해주는 역할을 한다.
  - Facade 객체에서 제공하는 메서드를 호출하여 서브 클래스의 사용을 이끌어낸다.
- 퍼사드 패턴을 활용하여 Employee 클래스에 중요한 메서드를 유지하고, Employee 클래스를 덜 중요한 나머지 메서드들에 대한 퍼사드로 사용하여 문제를 해결한다.

#### 결론

- SRP는 메서드와 클래스 수준의 원칙이다.
- 상위 수준에서 다른 형태로도 존재한다.
  - 컴포넌트 수준 → 공통 폐쇄 원칙(Common Closure Principle)이 된다.
  - 아키텍처 수준 → 아키텍처 경계(Architectural Boundary)의 생성을 책임지는 변경의 축이 된다.
