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
FROM eclipse-temurin:11-jre

WORKDIR /opt/shipping

EXPOSE 8080

# Environment variables
ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql
ENV DB_USER=shipping
ENV DB_PASS=roboshop@1
ENV DB_NAME=shipping

# Copy built JAR from previous stage
COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar

# Start the app
CMD ["java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar"]
