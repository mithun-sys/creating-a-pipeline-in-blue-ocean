# Use an official OpenJDK image as base
FROM eclipse-temurin:17-jdk-jammy

# Set working directory
WORKDIR /app

# Copy built JAR from Jenkins build context
COPY target/*.jar app.jar

# Expose port
EXPOSE 8080

# Run the JAR
ENTRYPOINT ["java", "-jar", "app.jar"]
