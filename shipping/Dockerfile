# ------------------------------------------------------------
# Stage 1: Build the JAR file using Maven
# ------------------------------------------------------------
FROM maven:3.8.8-eclipse-temurin-11 AS build

WORKDIR /opt/shipping

COPY pom.xml .
RUN mvn dependency:resolve

COPY src ./src
RUN mvn package -DskipTests


# ------------------------------------------------------------
# Stage 2: Run the Shipping service
# ------------------------------------------------------------
FROM openjdk:11-jre-slim

EXPOSE 8080

WORKDIR /opt/shipping

# Environment variables
ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql

# Copy jar from builder stage
COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar

# Start the service
CMD ["java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar"]
