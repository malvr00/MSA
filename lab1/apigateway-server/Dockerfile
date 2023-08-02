FROM openjdk:17-ea-11-jdk-slim

VOLUME /tmp

COPY target/apigateway-server-1.0.jar apigatewayServer.jar

ENTRYPOINT ["java", "-jar", "apigatewayServer.jar"]