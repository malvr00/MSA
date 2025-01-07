# MSA
마이크로 서비스 아키텍쳐 (노트내용 간단정리)

- USE: Spring Cloud, Config Server, Ntflix Eureka, JWT, Docker, Kafka, Prometheus, grafana

# MSA 흐름도

<img width="650" height="400" alt="스크린샷 2023-01-14 오후 6 48 26" src="https://user-images.githubusercontent.com/77275513/212466144-d6bc31ab-5ab1-4171-be20-13399f39cd19.png">

### 기본 설명
- 설명
  - MicroService Architecture는 크게 Inner Architecture와 Outer Architecture로 구분할 수 있습니다. 위 그림에서 남색 부분은 Inner Architecture의 영역이고, 회색 부분은 Outer Architecture 부분입니다.
- Inner architecture는 내부 서비스와 관련된 architecture입니다. 쉽게 말해, 내부의 서비스를 어떻게 잘 쪼개는지에 대한 설계입니다.
  - 서비스를 어떻게 정의할 것인가?
   - 주문 플랫폼에서 주문하기, 찜하기 등 같은 서비스로 넣을지, 다른 서비스로 분리할 것인지는 비즈니스나 시스템의 특성에 따라 정의되어야 합니다. 
   - 서비스를 정의하기 위해 고려해야할 사항은 비즈니스 뿐만 아니라, 서비스 간의 종ㅅ곡성, 배포 용이성, 장애 대응 등 굉장히 많습니다.
  - DB 접근 구조를 어떻게 설계할 것인가?
   - 일반적으로 MicroService는 API를 통해서 접근합니다. 또한 각 MicroService에는 자체의 데이터베이스도 가질 수 있는데, 일부 비즈니스 트랙잭션은 여러 MicroService를 걸쳐 있기 때문에, 각 서비스에 연결된 데이터베이스의 정합성을 보장할 방안이 필요합니다.
- Outer architecture
  - 참조 그림에서와 같이 MSA의 Outer architecture을 총 6개의 영역으로 분류하고 있습니다.

### External Gateway
- 설명
  - External Gateway는 전체 서비스 외부로부터 들어오는 접근을 내부 구조를 드러내지 않고 처리하기 위한 요소입니다. **사용자 인증(Consumer Identity Provider)과 권한 정책관리(policy management)** 를 수행하며, `API Gateway`가 여기서 가장 핵심적인 역할을 담당합니다.
- 프로젝트 
  - Spring Cloud Gateway [[이동]](https://github.com/malvr00/MSA/tree/master/lab1/apigateway-server)  

### Service Mesh
- 설명
  - Service Mesh는 마이크로서비스 구성 요소간의 네트워크를 제어하는 역할을 합니다. 서비스 간에 통신을 하기 위해서는 service discovery, service routing, 트래픽 관리 및 보안 등을 담당하는 요소가 있어야 합니다.<br/>
더 나아가 **서비스 간 통신을 보다 안전하고 효율적으로 관리하는 네트워크 계층** 입니다.
- 주요기능
  - 트래픽 관리: 라우팅, A/B 테스트, Canary 배포 가능
  - 보안: 서비스 간 TLS 암호화, 인증, 인가
  - 모니터링 및 로깅: 서비스 간 호출 추적, 메트릭 수집
  - 자동 리트라이 및 서킷 브레이커: 장애 발생 시 자동 복구
- 프로젝트
  - Reverse Proxy + Routing [[이동]](https://github.com/malvr00/MSA/tree/master/lab1/apigateway-server)
  - Service Discovery + Load Balancing [[이동]](https://github.com/malvr00/MSA/tree/master/lab1/discoveryservice)

### Container Management
- 설명
  - 컨테이너 기반 어플리케이션 운영은 유연성과 자율성을 가지며, 개발자가 손쉽게 접근 및 운영할 수 있는 인프라 관리 기술이기 때문에 MSA에 적합하다고 평가받고 있습니다.
  - 대표적인 컨테이너 관리 환경인 Kubernetes가 Container management에 많이 사용되고 있습니다.<br/>

### Backing Service
- 설명
  - Backing Service는 어플리케이션이 실행되는 가운데 네트워크를 통해서 사용할 수 있는 모든 서비스를 말하며, My SQL과 같은 데이터베이스, 캐쉬 시스템 등 포괄적인 개념입니다.
  - MSA에서의 특징적인 Backing service들 중 하나는 Message queue입니다. MSA에서는 메세지의 송신자와 수신자가 직접 통신하지 않고 Message Queue를 활용하여 비동기적으로 통신하는 것을 지향합니다.
    - 만약 MSA를 적용한 프로젝트에서 장애가 발생할 경우 마이크로서비스 오케스트레이션이 진행되면서, 새로운 마이크로 서비스를 신규 생성하거나 재생성 등의 작업을 진행하게 됩니다. 만약 Message Queue를 사용하지 않는 강한 결합 구조의 경우, 여러 서비스를 걸치는 실시간 트랜잭션을 처리할 때, 하나의 서비스가 죽어버리면 트랜잭션이 끓어지기 때문에 하나의 서비스 때문에 큰 에러가 발생하게 됩니다.
      - 위와 같은 이유 때문에 MSA에서 데이터 변경이나, 보상 트랜잭션과 관련된 처리는 Message Queue를 활용한 비동기 처리가 효율적입니다.
- 프로젝트
  - Kafka Producer [[이동]](https://github.com/malvr00/MSA/tree/master/lab1/order-service/src/main/java/com/example/orderservice/messagequeue)
  - Kafka Consumer [[이동]](https://github.com/malvr00/MSA/tree/master/lab1/catalog-service/src/main/java/com/example/catalogservice/messagequeue)

## 장애 처리와 Microservice 분산 추적 외 기타
Resilience4J, Zipkin, Prometheus, Grafana 

### Resilience4J
- 설명
  - Resilience4J는 마이크로서비스 아키텍처에서 장애를 감지하고 복구하는 여러 가지 패턴을 지원하는 라이브러리야. 주로 서킷 브레이커, 재시도, 레이트 리미터, 타임아웃 등 다양한 기능을 제공하여 시스템의 안정성을 높여준다.
- 주요 기능
  - 서킷 브레이커 (Circuit Breaker)
    - 장애가 발생한 서비스에 대한 요청을 일정 시간 차단해 더 큰 장애를 방지하고, 장애가 해결되면 다시 요청을 시작.
    - 예를 들어, 외부 서비스 호출이 실패하면 서킷 브레이커가 트리거되어 그 후 일정 시간 동안 요청을 차단.
  - 재시도 (Retry)
    - 요청 실패 시 일정 횟수까지 자동으로 재시도
  - 레이트 리미터 (Rate Limiter)
    - 서비스를 호출할 때의 요청 수를 제한하여 과부하를 방지.
  - 타임아웃 (TimeLimiter)
    - 서비스 호출에 걸리는 시간을 제한하여 너무 긴 응답 대기 시간을 방지.
  - 사용 목적
    - 위 주요 기능들은 장애 발생 시 시스템의 다운타임을 최소화하고, 서비스의 안정성을 보장해줍니다.

### Zipkin
- 설명
  - 분산 트랜잭션 및 요청 추적
    - Zipkin은 분산 시스템에서 서비스 간 호출을 추적하고, 트랜잭션 흐름을 시각화하는 도구야. 마이크로서비스 환경에서 각 서비스의 요청 처리 시간을 추적할 수 있어, 성능 문제를 파악하고, 서비스 간의 의존성을 분석할 때 유용합니다.
- 주요 기능
  - 분산 추적 (Distributed Tracing)
    - 하나의 요청이 여러 서비스 간을 거칠 때 각 서비스에서 발생하는 지연 시간 등을 추적할 수 있습니다. 예를 들어, A -> B -> C 순서로 요청이 처리될 때, 각 서비스의 처리 시간을 측정합니다.
  - 트랜잭션 흐름 시각화
    - 요청이 처리되는 전체 흐름을 시각적으로 확인할 수 있어서 병목 지점을 쉽게 찾아낼 수 있습니다.
  - 트래픽 분석
    - 서비스 간의 호출 빈도, 응답 시간 등의 메트릭을 추적할 수 있어, 성능 문제를 해결하는 데 유용합니다.
