# MSA

마이크로 서비스 아키텍쳐

- USE: Spring Cloud, Config Server, Ntflix Eureka, JWT, Doker, Kafka, Prometheus, Gragana

# MSA 흐름도

- 참조 : https://velog.io/@tedigom/MSA-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-1-MSA%EC%9D%98-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90-3sk28yrv0e

<img width="650" height="400" alt="스크린샷 2023-01-14 오후 6 48 26" src="https://user-images.githubusercontent.com/77275513/212466144-d6bc31ab-5ab1-4171-be20-13399f39cd19.png">


API Gateway Service Discovery가 중추 

# Kafka

Kafka 홈페이지
- http://kafka.apache.org

Kafka와 데이터를 주고받기 위해 사용하는 Java Library
- https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients

Zookeeper 및 Kafka 서버 기동
$KAFKA_HOME/bin/zookeeper-server-start.sh  $KAFKA_HOME/config/zookeeper.properties

$KAFKA_HOME/bin/kafka-server-start.sh  $KAFKA_HOME/config/server.properties

Topic 생성
$KAFKA_HOME/bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092 \

--partitions 1

Topic 목록 확인
$KAFKA_HOME/bin/kafka-topics.sh --bootstrap-server localhost:9092 --list

Topic 정보 확인
$KAFKA_HOME/bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092

Windows에서 기동
- 모든 명령어는 $KAFKA_HOME\bin\windows 폴더에 저장

- .\bin\windows\zookeeper-server-start.bat  .\config\zookeeper.properties
