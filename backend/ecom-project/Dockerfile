# Stage 1: Build the JAR using Maven
FROM maven:3.9.6-eclipse-temurin-21 AS build


WORKDIR /app

# Copy pom.xml separately to cache dependencies
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy source code
COPY src ./src

# Build the project and skip tests
RUN mvn clean package -DskipTests


# Stage 2: Create final lightweight runtime image
FROM openjdk:21-slim

WORKDIR /app

# Copy the JAR file from the build stage
COPY --from=build /app/target/ecom-project-0.0.1-SNAPSHOT.jar ./ecom-project.jar

# Expose application port
EXPOSE 8080

# Run the JAR
ENTRYPOINT ["java", "-jar", "ecom-project.jar"]
