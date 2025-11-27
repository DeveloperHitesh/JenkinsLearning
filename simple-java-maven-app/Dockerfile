# ------------------------------
# 1️⃣ Build Stage (Maven)
# ------------------------------
FROM maven:3.9.6-eclipse-temurin-17 AS builder

WORKDIR /app

# Copy pom and download dependencies first (for caching)
COPY pom.xml .
RUN mvn -e -B dependency:go-offline

# Now copy the full project
COPY src ./src

# Build JAR (skip tests for fast CI build)
RUN mvn -e -B clean package -DskipTests



# ------------------------------
# 2️⃣ Runtime Stage (Slim Java Image)
# ------------------------------
FROM eclipse-temurin:17-jdk-alpine

# Create working directory
WORKDIR /app

# Copy JAR from builder stage
COPY --from=builder /app/target/*.jar app.jar

# Expose application port (optional – depends on your app)
EXPOSE 8080

# Startup command
ENTRYPOINT ["java", "-jar", "app.jar"]
