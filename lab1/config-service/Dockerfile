FROM openjdk:17-ea-11-jdk-slim

VOLUME /tmp

COPY apiEncryptionKey.jks apiEncryptionKey.jks

COPY target/config-service-1.0.jar configService.jar

ENTRYPOINT ["java", "-jar", "configService.jar"]