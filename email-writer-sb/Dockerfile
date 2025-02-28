# FROM maven:3.9.6-eclipse-temurin-21 AS build
# WORKDIR /app
# COPY pom.xml .
# RUN mvn dependency:go-offline
#
# #build
# COPY src ./src
# RUN mvn clean package -DskipTests || cat /app/target/*.log
#
# #run
# FROM eclipse-temurin:21-jre AS runtime
#
# # First, check if the JAR is in /app/target/ or /app/
# RUN ls -lh /app/target || true
# RUN ls -lh /app || true
#
# WORKDIR /app
# COPY --from=build /app/email-writer-sb-0.0.1-SNAPSHOT.jar .
#
# EXPOSE 8080
# ENTRYPOINT ["java", "-jar", "/app/email-writer-sb-0.0.1-SNAPSHOT.jar"]

# Build stage
FROM maven:3.9.6-eclipse-temurin-21 AS build

WORKDIR /app

# Copy pom.xml and download dependencies
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy source code and build application
COPY src ./src
RUN mvn clean package -DskipTests

# Runtime stage
FROM eclipse-temurin:21-jre AS runtime

WORKDIR /app

# Copy the JAR file from the build stage
COPY --from=build /app/target/email-writer-sb-0.0.1-SNAPSHOT.jar email-writer.jar

# Expose application port
EXPOSE 8080

# Run the application
ENTRYPOINT ["java", "-jar", "/app/email-writer.jar"]