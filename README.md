
# ILP HCL Store

A simple Java web application project using Maven.

## Overview
ILP HCL Store is a demo e-commerce web application designed to showcase a basic Java EE architecture using JSP and Servlets. The project demonstrates how to structure, build, and test a Java web application with modern development tools and best practices. It is ideal for learning, prototyping, or as a starting point for more complex solutions.

## Features
- Simple landing page with a modern look
- Modular Maven project structure
- Unit testing with JUnit
- WAR packaging for easy deployment
- Ready for CI/CD integration (Jenkins, etc.)

## Technology Stack
- **Java 8+**: Core programming language
- **Maven**: Build automation and dependency management
- **JUnit 4**: Unit testing framework
- **JSP**: JavaServer Pages for the web UI
- **Servlet API**: For backend logic (can be extended)
- **Tomcat** (or any Java EE servlet container): For deployment

## How It Works
- The application starts with a landing page (`index.jsp`) that welcomes users to the HCL Store.
- The project is structured for easy extension: you can add Servlets, business logic, and more JSPs as needed.
- Unit tests are provided to ensure code quality and demonstrate test integration.
- The Maven build process compiles the code, runs tests, and packages the application as a `.war` file for deployment.

## Project Structure
- `src/main/java` - Java source code (add your Servlets and business logic here)
- `src/main/webapp` - Web application files (JSP, HTML, CSS, etc.)
- `src/test/java` - Unit tests

## Prerequisites
- Java 8 or higher
- Maven 3.x

## Build
To build the project, run:

```
mvn clean install
```

## Run Tests
To run the tests:

```
mvn test
```

## Deployment
The build will generate a WAR file in the `target/` directory, which can be deployed to any Java EE servlet container (e.g., Tomcat).

## License
This project is for demo purposes.
