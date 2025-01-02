# MSA
마이크로 서비스 아키텍쳐 (노트내용 정리 작업 진행 중)

- USE: Spring Cloud, Config Server, Ntflix Eureka, JWT, Docker, Kafka, Prometheus, grafana

# MSA 흐름도

- 참조 : https://velog.io/@tedigom/MSA-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-1-MSA%EC%9D%98-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90-3sk28yrv0e

<img width="650" height="400" alt="스크린샷 2023-01-14 오후 6 48 26" src="https://user-images.githubusercontent.com/77275513/212466144-d6bc31ab-5ab1-4171-be20-13399f39cd19.png">

### 기본 설명
MicroService Architecture는 크게 Inner Architecture와 Outer Architecture로 구분할 수 있습니다. 위 그림에서 남색 부분은 Inner Architecture의 영역이고, 회색 부분은 Outer Architecture 부분입니다.
- Inner architecture는 내부 서비스와 관련된 architecture입니다. 쉽게 말해, 내부의 서비스를 어떻게 잘 쪼개는지에 대한 설계입니다.
  - 서비스를 어떻게 정의할 것인가?
   - 주문 플랫폼에서 주문하기, 찜하기 등 같은 서비스로 넣을지, 다른 서비스로 분리할 것인지는 비즈니스나 시스템의 특성에 따라 정의되어야 합니다. 
   - 서비스를 정의하기 위해 고려해야할 사항은 비즈니스 뿐만 아니라, 서비스 간의 종ㅅ곡성, 배포 용이성, 장애 대응 등 굉장히 많습니다.
  - DB 접근 구조를 어떻게 설계할 것인가?
   - 일반적으로 MicroService는 API를 통해서 접근합니다. 또한 각 MicroService에는 자체의 데이터베이스도 가질 수 있는데, 일부 비즈니스 트랙잭션은 여러 MicroService를 걸쳐 있기 때문에, 각 서비스에 연결된 데이터베이스의 정합성을 보장할 방안이 필요합니다.
- Outer architecture
  - 참조 그림에서와 같이 MSA의 Outer architecture을 총 6개의 영역으로 분류하고 있습니다.

### External Gateway
External Gateway는 전체 서비스 외부로부터 들어오는 접근을 내부 구조를 드러내지 않고 처리하기 위한 요소입니다. **사용자 인증(Consumer Identity Provider)과 권한 정책관리(policy management)**를 수행하며, `API Gateway`가 여기서 가장 핵심적인 역할을 담당합니다.

<br/>

# 장애 처리와 Microservice 분산 추적 외 기타<br/>
Resilience4J, Zipkin, Prometheus, Grafana


