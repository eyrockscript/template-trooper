Microservice Template
=====================

Table of Contents

-   [Features](#features)
-   [Requirements](#requirements)
-   [Dependencies](#dependencies)
-   [Configuration](#configuration)
-   [How to use this template](#how-to-use-this-template)
-   [Contribution](#contribution)
-   [License](#license)


This project serves as a template for creating Spring-based microservices. It is designed to streamline the setup process and provide a starting point with recommended configurations and standards. The project integrates Spring Cloud Config Client for external configuration management in a distributed system.


Features
--------

-   Use of Gradle for build and dependency management.
-   Global Exception Handler: A global exception handler is in place to handle all exceptions at a global level, log errors and respond appropriately.
-   Logging Configuration: Configured to use the Logback logging framework, the service logs are output to both the console and a Graylog server. The Graylog server is provided as a separate service in the Docker Compose file.
-   Spring Cloud Config Client: The project integrates Spring Cloud Config Client to provide externalized configuration in a distributed environment. This allows applications to be packaged once and deployed anywhere.
-   Dockerfile & Docker Compose: The Dockerfile and Docker Compose included in the project ensure easy containerization of the microservice and effortless deployment of the entire application stack, including the Graylog service.

Requirements
------------

-   Java 17
-   Gradle
-   Docker

Dependencies
------------

-   Spring Boot Starter Web (Excludes Spring Boot Starter Logging)
-   Logback GELF - version 2.1.2
-   Spring Cloud Starter Bootstrap
-   Spring Cloud Starter Config
-   Spring Cloud Config Client
-   Spring Boot Starter Test
-   Spring Security Test

Configuration
-------------

### Exception Handling

The project uses a `GlobalExceptionHandler` class to handle all exceptions occurring in the application. Any exception thrown within the application will be handled in this class, and the exception information will be sent to the logs and to graylog.

```

@Slf4j
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(value = Exception.class)
    public ResponseEntity<Object> handleException(Exception ex) {
        log.error("internal server error", ex);
        return new ResponseEntity<>("An error occurred: " + ex.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

### Logback

The `logback-spring.xml` file provides configuration for logging. This configuration includes sending logs to the console and to graylog via the `GELF` appender.

### Gradle

The `build.gradle` file defines how the application is built and what dependencies are needed to run it. This file is set up to use Spring Boot, Lombok, and several Spring Cloud dependencies.

### Docker

The project includes a `Dockerfile` that defines how the application is packaged into a Docker container. This file copies the JAR produced by Gradle into the image and sets the entry command to run the application.

### Spring Cloud Config

This project uses Spring Cloud Config Client for managing external configuration. You need to set up a `bootstrap.yml` file with the necessary configuration to connect to your Spring Cloud Config Server.

How to use this template
------------------------

1.  Clone the repository to your local machine.

2.  Navigate to the project directory in your terminal.

3.  Run the project using Gradle:


```
./gradlew bootRun
```


1.  Package the project into a JAR using Gradle:


```

./gradlew build
```


1.  Build the Docker image:

```
docker build -t microservice-template .
```


1.  Run the Docker image:

```
docker run -p 8080:8080 microservice-template
```


This will start the microservice on port 8080.

Contribution
------------

If you want to contribute to this project, please fork the repository and create a pull request. All contributions are welcome.

License
-------

This project is licensed under the terms of the MIT license. See the `LICENSE` file for more details.
