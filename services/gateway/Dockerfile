FROM openjdk:17-jdk-slim

MAINTAINER robert_stroe.com

RUN apt-get update &&\
   apt-get install -y curl

COPY target/gateway-0.0.1-SNAPSHOT.jar gateway-0.0.1-SNAPSHOT.jar

ENTRYPOINT ["java", "-jar", "gateway-0.0.1-SNAPSHOT.jar"]