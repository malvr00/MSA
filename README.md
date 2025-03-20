# MSA
마이크로 서비스 아키텍쳐

- USE: Spring Cloud, Config Server, Ntflix Eureka, JWT, Doker, Kafka, Prometheus, Gragana

# MSA 흐름도

- 참조 : https://velog.io/@tedigom/MSA-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-1-MSA%EC%9D%98-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90-3sk28yrv0e

<img width="650" height="400" alt="스크린샷 2023-01-14 오후 6 48 26" src="https://user-images.githubusercontent.com/77275513/212466144-d6bc31ab-5ab1-4171-be20-13399f39cd19.png">

<br/>
API Gateway Service Discovery가 중추

---
<br/>

# Kafka

<br/>
Kafka 홈페이지<br/>
- http://kafka.apache.org
<br/><br/>
Kafka와 데이터를 주고받기 위해 사용하는 Java Library<br/>
- https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients<br/>
<br/>
Zookeeper 및 Kafka 서버 기동<br/>
$KAFKA_HOME/bin/zookeeper-server-start.sh  $KAFKA_HOME/config/zookeeper.properties<br/>
<br/>
$KAFKA_HOME/bin/kafka-server-start.sh  $KAFKA_HOME/config/server.properties<br/>
<br/>
Topic 생성<br/>
$KAFKA_HOME/bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092 \<br/>
<br/>
--partitions 1<br/>
<br/>
Topic 목록 확인<br/>
$KAFKA_HOME/bin/kafka-topics.sh --bootstrap-server localhost:9092 --list<br/>
<br/>
Topic 정보 확인<br/>
$KAFKA_HOME/bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092<br/>
<br/>
Windows에서 기동<br/>
- 모든 명령어는 $KAFKA_HOME\bin\windows 폴더에 저장<br/>
<br/>
- .\bin\windows\zookeeper-server-start.bat  .\config\zookeeper.properties<br/>
<br/>
메시지 생산<br/>
$KAFKA_HOME/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic quickstart-events<br/>
<br/>
메시지 소비<br/>
$KAFKA_HOME/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic quickstart-events \<br/>
<br/>
 --from-beginning<br/>

# Kafka Connect<br/>
Kafka Connect 설치<br/>
curl -O http://packages.confluent.io/archive/5.5/confluent-community-5.5.2-2.12.tar.gz<br/>
<br/>
curl -O http://packages.confluent.io/archive/6.1/confluent-community-6.1.0.tar.gz<br/>
<br/>
tar xvf confluent-community-6.1.0.tar.gz<br/>
<br/>
cd  $KAFKA_CONNECT_HOME<br/>
<br/>
 <br/>
<br/>
Kafka Connect 실행<br/>
./bin/connect-distributed ./etc/kafka/connect-distributed.properties<br/>
<br/>
 <br/>
<br/>
JDBC Connector 설치<br/>
- https://docs.confluent.io/5.5.1/connect/kafka-connect-jdbc/index.html<br/>
<br/>
- confluentinc-kafka-connect-jdbc-10.0.1.zip <br/>
<br/>
 <br/>
<br/>
etc/kafka/connect-distributed.properties 파일 마지막에 아래 plugin 정보 추가<br/>
- plugin.path=[confluentinc-kafka-connect-jdbc-10.0.1 폴더]<br/>

# Kafka Connect Source<br>
<br/>
<img width="908" alt="스크린샷 2023-03-22 오후 6 44 24" src="https://user-images.githubusercontent.com/77275513/226864112-e24cac6f-9a55-4941-8d48-88ff31a723c0.png">
<br/>
Postman을 활용하여 등록하였음. <br/>
등록: localhost:8083/connectors

```json
 {
     "name": "my-source-connect",
     "config": {
         "connector.class": "io.confluent.connect.jdbc.JdbcSourceConnector",
         "connection.url": "jdbc:mariadb://localhost:3306/mydb",
         "connection.user": <User name>,
         "connection.password": <User password>,
         "mode": "incrementing",
         "incrementing.column.name":"id",
         "table.whitelist": "mydb.users",
         "topic.prefix": "my_topic_",
         "tasks.max": 1
     }
 }
```

<br/>
조회: localhost:8083/connectors/{name} 상태: localhost:8083/connectors/{name}/status
<br/>
<br/>

# Kafka Connect Sink<br/>
등록: localhost:8083/connectors

```json
{

  "name":"my-sink-connect",

  "config":{
    "connector.class":"io.confluent.connect.jdbc.JdbcSinkConnector",
    "connection.url":"jdbc:mysql://localhost:3306/mydb",
    "connection.user":<User name>,
    "connection.password":<User password>,
    "auto.create":"true",
    "auto.evolve":"true",
    "delete.enabled":"false",
    "tasks.max":"1",
    "topics":"my_topic_users"
  }
}
```

<br/>

집킨, 서킷브레이커 정리 추가예정
<br/>

# 장애 처리와 Microservice 분산 추적 외 기타<br/>
Resilience4J, Zipkin, Prometheus, Grafana


