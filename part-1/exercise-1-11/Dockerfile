# Start from openjdk:8 with Java 8
FROM openjdk:8

EXPOSE 8080

COPY . .

WORKDIR /usr/src/app

# Install with maven
RUN ./mvnw package
CMD ["java", "-jar", "./target/docker-example-1.1.3.jar"]
