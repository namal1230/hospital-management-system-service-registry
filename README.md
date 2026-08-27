# Hospital Management System - Service Registry
● Student Name: Namal Dilmith Ruwanpathirana
<br><br>
● Student Number: 2301671058
<br><br>
● Slack Handle : namaldilmith2
<br><br>
● GCP Project ID : pro-edu-476313
<br><br>
Version: 1.0.0
Last Updated: August 23, 2026

<img width="877" height="601" alt="Screenshot 2026-08-22 215929" src="https://github.com/user-attachments/assets/b43a4e95-4e45-4758-9e50-dcfc3df78f8c" />
<br>
<br>
<br>
<img width="1182" height="1412" alt="Screenshot 2026-08-23 080015" src="https://github.com/user-attachments/assets/99379665-cef6-4013-a62e-970212f8b349" />



This repository contains the Service Registry component for the Hospital Management System. The service registry enables microservices to discover and register with each other (Eureka) and include[...]

> Note: This README was generated from the repository's pom.xml. Double-check the dependency choices in the pom (e.g., `spring-cloud-starter-config` vs `spring-cloud-config-server`) to ensure they[...]

## Project overview

- ArtifactId: ServiceRegistry
- GroupId: com.example
- Spring Boot parent: 4.1.0
- Spring Cloud BOM: 2025.1.2
- Java version: 25 (configured in pom.xml)
- Key dependencies:
  - org.springframework.cloud:spring-cloud-starter-netflix-eureka-server
  - org.springframework.cloud:spring-cloud-starter-config (present in pom; typically used for config client)
  - org.springframework.boot:spring-boot-starter-test (test scope)

This module is intended to act as a Eureka service registry for the Hospital Management System microservices. It may also be configured to act as a Config Server or include a config client dependi[...]

## Prerequisites

- Java JDK 25 (ensure JAVA_HOME points to a JDK 25 installation)
- Maven 3.8+ (or compatible)
- Optional: Docker (if you prefer to run as a container)

> If you need to target a different Java version, update the `<java.version>` property in `pom.xml` and ensure your development environment matches.

## Build

From the repository root:

```bash
mvn clean package -DskipTests
```

This will produce an executable jar under `target/` on a successful build.

## Run (locally)

You can run the application using Maven or the packaged jar.

Using Maven:

```bash
mvn spring-boot:run
```

Using the jar:

```bash
java -jar target/ServiceRegistry-0.0.1-SNAPSHOT.jar
```

## Recommended application configuration

Place the following configuration in `src/main/resources/application.yml` (or `application.properties`) to run the registry as a standalone Eureka server.

```yaml
server:
  port: 9001

spring:
  application:
    name: service-registry

eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
  instance:
    hostname: localhost
```

- `register-with-eureka: false` and `fetch-registry: false` ensure the registry itself does not try to register to another Eureka instance.
- The Eureka dashboard is available at http://localhost:8761/ by default when the server is up.

## If you intend to include a Config Server

The POM currently includes `spring-cloud-starter-config` which is usually the config *client* starter. If your goal is to host a centralized configuration server, replace or add the `spring-cloud-[...]

Example (pom snippet for config server):

```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

And in your Spring Boot application main class:

```java
import org.springframework.cloud.config.server.EnableConfigServer;

@EnableConfigServer
@SpringBootApplication
public class ServiceRegistryApplication {
  public static void main(String[] args) {
    SpringApplication.run(ServiceRegistryApplication.class, args);
  }
}
```

## Useful commands

- Run tests:
  - mvn test
- Run with a specific profile:
  - mvn spring-boot:run -Dspring-boot.run.profiles=local

## Docker (optional)

A simple Dockerfile to containerize the registry:

```dockerfile
FROM eclipse-temurin:25-jre-focal
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

Build and run:

```bash
docker build -t hospital-service-registry:latest .
docker run -p 8761:8761 hospital-service-registry:latest
```

## Health checks and actuator

If Spring Boot Actuator is enabled, endpoints like `/actuator/health` can be used to monitor the registry.

Add to `pom.xml` if needed:

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

And enable endpoints in `application.yml`:

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info
```

## Notes and troubleshooting

- Ensure the Spring Cloud version is compatible with your Spring Boot parent version. The POM uses Spring Boot 4.1.0 and Spring Cloud 2025.1.2 — verify compatibility if you change versions.
- If you see class or dependency errors related to Netflix Eureka, confirm the Spring Cloud BOM is imported correctly (the POM contains `spring-cloud-dependencies` in dependencyManagement).
- If you intend to serve both Eureka and Config Server responsibilities from the same app, be sure to configure both properly and understand the implications for startup order and bootstrap confi[...]

## Contributing

Contributions are welcome. Please open issues describing bugs or feature requests, and submit pull requests for fixes.

## License

This repository does not include an explicit license file. Add a LICENSE file to clarify usage rights.

## Contact

For questions about this project, open an issue in the repository or contact the repository owner.
