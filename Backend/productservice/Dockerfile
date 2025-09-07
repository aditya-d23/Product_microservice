# ================================
# Stage 1: Build the JAR
# ================================
FROM eclipse-temurin:21-jdk AS build

# Set working directory
WORKDIR /workspace

# Copy Maven wrapper and pom.xml
COPY mvnw pom.xml ./
COPY .mvn .mvn

# Copy source code
COPY src src

# Build application (skip tests for faster build)
RUN ./mvnw -B -DskipTests clean package

# ================================
# Stage 2: Run the app
# ================================
FROM eclipse-temurin:21-jre

# Set working directory
WORKDIR /app

# Copy JAR from build stage
COPY --from=build /workspace/target/*.jar app.jar

# Expose Spring Boot default port
EXPOSE 8080

# Run the application
ENTRYPOINT ["java", "-jar", "/app/app.jar"]

