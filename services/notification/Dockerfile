#Start with a base image containing Java runtime
FROM openjdk:17-jdk-slim

MAINTAINER robert_stroe.com

RUN apt-get update &&\
   apt-get install -y curl

#Add application jar to image
COPY target/notification-0.0.1-SNAPSHOT.jar notification-0.0.1-SNAPSHOT.jar

#Execute the application
ENTRYPOINT ["java","-jar","notification-0.0.1-SNAPSHOT.jar"]