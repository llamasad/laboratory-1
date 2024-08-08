FROM maven:3.9.4-eclipse-temurin-17-alpine as build
WORKDIR /app
COPY . .
RUN mvn clean package -Dmaven.test.failure.ignore=true


FROM openjdk:17-jdk-alpine
COPY --from=build /app/target/spring-boot-2-hello-world-1.0.2-SNAPSHOT.jar app.jar 
ENTRYPOINT [ "java", "-jar" , "app.jar" ]