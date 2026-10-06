# Creating My First Spring Boot Project Using VS Code

## Introduction

This document outlines the steps I followed to create my first Spring Boot project using Visual Studio Code (VS Code), Maven, and Java. The goal was to understand the Spring Initializr project creation process and successfully run a basic Spring Boot application.

---

# Prerequisites

Before creating the project, ensure the following software is installed:

* Visual Studio Code (VS Code)
* Java Development Kit (JDK)
* Maven (optional because Spring Initializr can generate the Maven project structure)
* Internet connection (required to download dependencies)

---

# Required VS Code Extensions

Open VS Code and install the following extensions:

## Java Extension Pack

Provides:

* Java language support
* Debugging tools
* Maven support
* JUnit support

Extension:

* **Extension Pack for Java**

---

## Spring Boot Extension Pack

Provides:

* Spring Boot Dashboard
* Spring Initializr support
* Spring Boot project management

Extension:

* **Spring Boot Extension Pack (VMware)**

After installation, a hexagon-shaped icon appears on the left navigation panel.

When hovering over it, it displays:

```text
Spring Dashboard
```

---

# Creating the Spring Boot Project

## Step 1: Open Spring Initializr

Open:

```text
View → Command Palette
```

Or press:

```text
Ctrl + Shift + P
```

Type:

```text
Spring Initializr
```

Select:

```text
Spring Initializr: Create a Maven Project
```

I selected Maven because I am still learning Maven and wanted to understand its project structure.

---

## Step 2: Select Spring Boot Version

Choose the Spring Boot version.

Example:

```text
4.1.1
```

This was the latest version available at the time.

---

## Step 3: Select Project Language

Choose:

```text
Java
```

Other available options include:

* Kotlin
* Groovy

---

## Step 4: Enter Group ID

Enter the Group ID.

Example:

```text
com.example
```

The Group ID identifies the organization or package structure of the project.

---

## Step 5: Enter Artifact ID

Enter the Artifact ID.

Example:

```text
demo
```

I chose **demo** because it was my first Spring Boot testing project.

The Artifact ID becomes the project name.

---

## Step 6: Select Packaging Type

Choose one of the following:

### JAR

Java Archive

Used for standalone Spring Boot applications.

### WAR

Web Archive

Used when deploying to an external application server.

### EAR

Enterprise Archive

Used for large enterprise applications.

For this project, I selected:

```text
JAR
```

---

## Step 7: Select Java Version

Choose the Java version.

I selected:

```text
Java 21
```

Reason:

* Modern Long-Term Support (LTS) version
* Works well with JUnit 5
* Fully supported by Spring Boot

---

## Step 8: Select Dependencies

Choose the required dependencies.

I selected:

### Spring Web

Provides:

* REST APIs
* Controllers
* Embedded Tomcat Server

### Lombok

Provides:

* Automatic generation of getters
* Setters
* Constructors
* Other boilerplate code

### Spring Boot DevTools

Provides:

* Automatic application restart
* Faster development experience

Press:

```text
Enter
```

to continue.

---

## Step 9: Choose Project Location

VS Code opens File Explorer.

Create a new folder.

Example:

```text
mySpringBootProject
```

Select:

```text
Generate into this folder
```

VS Code downloads all required dependencies and generates the project structure.

When successful, a message similar to the following appears:

```text
Successfully generated project
```

Click:

```text
Open
```

to open the project.

---

# Generated Project Structure

A typical Spring Boot project contains:

```text
mySpringBootProject
│
├── src
│   ├── main
│   │   ├── java
│   │   └── resources
│   │
│   └── test
│
├── pom.xml
├── mvnw
├── mvnw.cmd
└── target
```

Important files:

### pom.xml

Contains:

* Project information
* Dependencies
* Maven configuration

### DemoApplication.java

Main entry point of the Spring Boot application.

---

# Installing Java 21 (If Required)

The Spring Boot Dashboard may not appear if Java 21 is not installed.

Install Temurin JDK 21 using Winget.

Open PowerShell as Administrator and run:

```powershell
winget install --id EclipseAdoptium.Temurin.21.JDK --source winget
```

After installation:

1. Close PowerShell
2. Reopen PowerShell

Verify installation:

```powershell
java -version
```

You should see Java 21 displayed.

Also verify:

```powershell
javac -version
```

---

# Running the Application

There are multiple ways to run a Spring Boot application.

## Using Spring Boot Dashboard

Open:

```text
Spring Dashboard
```

Locate your application.

Click:

```text
Run
```

or

```text
Debug
```

The application starts and the console displays startup logs.

---

## Using Maven

Open a terminal inside the project folder:

```bash
mvn spring-boot:run
```

Spring Boot starts the embedded Tomcat server.

---

# Accessing the Application

After the application starts successfully, open a browser and navigate to:

```text
http://localhost:8080
```

If no controller has been created yet, you may see:

```text
Whitelabel Error Page
```

or

```text
404 Not Found
```

This is normal because no endpoints exist yet.

---

# Creating Your First Endpoint

Open:

```text
DemoApplication.java
```

or create a new controller class.

Example:

```java
@RestController
public class HomeController {

    @GetMapping("/")
    public String home() {
        return "Hello World";
    }
}
```

Save the file.

If DevTools is installed, Spring Boot automatically restarts.

Refresh:

```text
http://localhost:8080
```

The browser should display:

```text
Hello World
```

---

# What I Learned

Through this exercise, I learned:

* How to install Spring Boot support in VS Code.
* How to use Spring Initializr.
* How to create a Maven-based Spring Boot project.
* The purpose of Group ID and Artifact ID.
* The difference between JAR, WAR, and EAR packaging.
* How to select Spring Boot dependencies.
* How to install Java 21 using Winget.
* How to run a Spring Boot application.
* How to access the application through a web browser.
* How to create and modify a simple endpoint.

---

# Conclusion

Creating my first Spring Boot project helped me understand the basic Spring Boot project setup process. I learned how to generate a project using Spring Initializr, configure dependencies, run the application, and create a simple endpoint that can be accessed through a web browser. This serves as a foundation for learning more advanced Spring Boot concepts such as REST APIs, databases, testing, and microservices.
