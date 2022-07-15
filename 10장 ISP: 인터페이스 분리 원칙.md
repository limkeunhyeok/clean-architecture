# Clean Architecture

## 03 설계 원칙

### 10장 ISP: 인터페이스 분리 원칙

![ISP](https://velog.velcdn.com/images/hellojihyoung/post/106355d6-3b0a-46b5-b868-14ac79bbd236/image.png)

> 위의 그림에서 User1: op1, User2: op2, User3: op3만 사용한다고 하면, User1은 op2와 op3을 사용하지 않아도 해당 메서드에 의존하게 된다.

- 이러한 문제는 아래 그림처럼 오퍼레이션을 인터페이스 단위로 분리하여 해결한다.

![분리](https://velog.velcdn.com/images/hellojihyoung/post/d61b1591-83ab-4625-822a-6f5b8338bd41/image.png)

#### ISP와 언어

- 언어 타입(정적, 동적 등)에 따라 소스 코드 의존성 여부가 다르다.
  - 따라서, ISP를 아키텍처가 아닌, 언어와 관련된 문제라고 결론내릴 여지가 있다.

#### ISP와 아키텍처

- 필요 이상으로 많은 걸 포함하는 모듈에 의존하는 것은 불필요한 재컴파일과 재배포를 강제한다.

#### 결론

> 불필요한 짐을 실은 무언가에 의존하면 예상치도 못한 문제에 빠진다.
