# Build Stage: Use a Maven image with OpenJDK 17 for building the application
FROM maven:3.9.5-eclipse-temurin-17 AS build

WORKDIR /app

# Copy the pom.xml and fetch dependencies
COPY pom.xml ./ 
RUN mvn dependency:resolve -DskipTests

# Copy the source code and build the application
COPY src ./src
RUN mvn clean package -DskipTests

# Runtime Stage: Use a lightweight OpenJDK image for running the application
FROM eclipse-temurin:17-jdk-alpine

WORKDIR /app

# Copy the JAR file from the build stage
COPY --from=build /app/target/spring-boot-data-jpa-0.0.1-SNAPSHOT.jar app.jar

# Expose the application's port
EXPOSE 8080

# Run the application with Dockerize
ENTRYPOINT ["dockerize", "-wait", "tcp://testdb:3306", "-timeout", "300s", "java", "-jar", "app.jar"]
