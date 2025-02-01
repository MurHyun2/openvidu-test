# Stage 1: Build with Maven
FROM maven:3.8.6-openjdk-17-slim AS builder

WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline

COPY src ./src
RUN mvn package -DskipTests

# Stage 2: Run the application
FROM openjdk:17-jre-slim

WORKDIR /app
COPY --from=builder /app/target/*.jar ./app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]