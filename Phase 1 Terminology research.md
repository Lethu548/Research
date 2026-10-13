# Phase 1: Java and Maven Fundamentals

## 1. What Are Dependencies?

Dependencies are pieces of code that a project relies on to function. Instead of writing everything from scratch, developers use pre-built libraries that handle common tasks, allowing them to focus on their own application logic.

In Java, libraries are often distributed as **JAR (Java Archive) files**. These files contain compiled Java classes and other resources that can be reused in other projects.

Maven simplifies dependency management by downloading the required libraries and making them available to the project automatically. This saves time because developers do not have to manually download JAR files and add them to the project's libraries.

### Why Are Dependencies Important?

- **Save time:** Reuse existing code instead of implementing everything from scratch.
- **Improve productivity:** Focus on the application's specific functionality.
- **Simplify management:** Maven downloads and manages the required libraries.
- **Support consistency:** Dependencies can be declared in the `pom.xml` file so Maven knows which libraries the project needs.

---

## 2. What Are Artifacts?

An artifact is an output produced by the build process. It is something that can be distributed, installed, or deployed.

For a Java Maven project, the artifact is often a **JAR file**. A JAR can contain compiled `.class` files, application resources, and a manifest containing information about the archive.

When you run:

```bash
mvn clean install
```

Maven cleans the previous build output, compiles the source code, runs the configured tests, packages the application, and installs the resulting artifact into your local Maven repository if the build succeeds.

For example, the generated artifact might be named:

```text
intern-0.0.1-SNAPSHOT.jar
```

### Running the Artifact

If the JAR is an executable JAR with the necessary runtime configuration, you can run it using:

```bash
java -jar target/intern-0.0.1-SNAPSHOT.jar
```

The exact filename depends on the project's Maven configuration.

### Why Do We Deploy Artifacts to a Server?

Running an application on your local machine usually makes it available only in your local environment or network, depending on its configuration.

Deploying an application to a server allows other users or systems to access it over a network, provided that the server, network, and application are configured correctly.

**In simple terms:** The artifact is the packaged application that can be distributed or deployed.

---

## 3. What Are Imports, and Why Are They Used?

Imports tell the Java compiler which classes or interfaces your source code refers to by their simple names.

Without imports, you would often have to write the fully qualified name of each class, including its package.

### Example

Without an import:

```java
public class Main {
    public static void main(String[] args) {
        java.util.Scanner scanner =
                new java.util.Scanner(System.in);
    }
}
```

With an import:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
    }
}
```

Both examples refer to the same `Scanner` class.

### Why Are Imports Used?

- **Readability:** They make code shorter and easier to understand.
- **Identification:** They make it clearer which classes are being used.
- **Package distinction:** They help identify the intended class when different packages contain classes with the same name.

### Exact Imports vs. Wildcard Imports

An exact import specifies a particular class:

```java
import java.util.Scanner;
```

A wildcard import makes types in a package available by their simple names:

```java
import java.util.*;
```

Explicit imports can make dependencies easier to identify when reading a source file. Wildcard imports are valid Java, but some teams prefer explicit imports for readability and maintainability.

**Important:** Imports do not download dependencies. Maven manages external libraries, while Java imports refer to types available on the project's classpath or module path.

---

## 4. Package Naming and Folder Structure

Packages organise related Java classes and interfaces. They help prevent naming conflicts and make projects easier to maintain.

### Base Package Naming

A common convention is to use a reverse-domain-style name for the base package.

For example:

```text
com.company.project
```

A typical Maven project might have this structure:

```text
intern/
├── pom.xml
└── src/
    ├── main/
    │   ├── java/
    │   │   └── com/
    │   │       └── company/
    │   │           └── project/
    │   │               └── Main.java
    │   └── resources/
    └── test/
        ├── java/
        └── resources/
```

The `pom.xml` file defines the Maven project configuration. Java source files belong under `src/main/java`, while test source files belong under `src/test/java`.

### Java Naming Conventions

| Element | Naming convention | Example |
|---|---|---|
| Packages | Lowercase, no underscores | `com.company.project` |
| Classes | PascalCase | `DocumentMetadata` |
| Interfaces | PascalCase | `DocumentRepository` |
| Methods | camelCase | `createDocument()` |
| Variables | camelCase | `documentTitle` |
| Constants | UPPER_SNAKE_CASE | `MAX_FILE_SIZE` |
| Enums | PascalCase | `DocumentStatus` |

### Relationship Between Package Names and Maven Coordinates

Maven uses project coordinates to identify an artifact:

- **Group ID:** Identifies the organisation or project group, such as `com.company`.
- **Artifact ID:** Identifies the specific project, such as `intern`.
- **Version:** Identifies the release or development version, such as `0.0.1-SNAPSHOT`.

For example:

```xml
<groupId>com.company</groupId>
<artifactId>intern</artifactId>
<version>0.0.1-SNAPSHOT</version>
```

These coordinates do not have to match the Java package name exactly, although using a consistent naming structure can make projects easier to organise.

---

## 5. Using YAML Files

YAML stands for **YAML Ain't Markup Language**. It is a human-readable data format commonly used for configuration files.

In Spring Boot applications, YAML files are often used to configure application settings in `application.yml`.

### Example

```yaml
spring:
  application:
    name: intern

server:
  port: 8080

app:
  document:
    max-file-size: 10
    enabled: true
```

In this example:

- `spring.application.name` sets the application name.
- `server.port` configures the server port.
- `app.document.max-file-size` is a custom configuration value.
- `app.document.enabled` is a Boolean setting.

### Why Use YAML?

#### 5.1 Hierarchy

YAML uses indentation to represent nested structures, making relationships between configuration values clear.

```yaml
database:
  connection:
    host: localhost
    port: 5432
```

#### 5.2 Readability

YAML has a clean structure that can be easier to read than a long list of configuration properties.

#### 5.3 Lists and Maps

YAML supports lists and key-value mappings.

Example of a list:

```yaml
app:
  supported-formats:
    - pdf
    - docx
    - txt
```

Example of a map:

```yaml
app:
  document:
    title: Project Report
    version: 1.0
```

#### 5.4 Multiple Profiles

Spring Boot supports multiple YAML documents in one file, separated by `---`. These can be activated for different environments.

```yaml
spring:
  application:
    name: intern

---
spring:
  config:
    activate:
      on-profile: dev

server:
  port: 8080

---
spring:
  config:
    activate:
      on-profile: test

server:
  port: 8081
```

The `dev` and `test` sections apply when their corresponding profiles are active.

#### 5.5 Data Types

YAML supports values such as strings, numbers, Booleans, lists, and maps. These can be interpreted as their corresponding data types when parsed.

```yaml
app:
  document:
    enabled: true
    max-retries: 3
    description: Document metadata service
```

Correct indentation and careful handling of values are important because YAML uses indentation to define structure.

#### 5.6 Maintainability

YAML can make larger configuration files easier to organise by grouping related settings together. However, whether YAML is preferable to Java properties depends on the project's needs and team conventions.

---

## 6. Phase 1 Summary

By completing this phase, I understand the following concepts:

- **Dependencies:** Reusable libraries that a project needs to function.
- **Artifacts:** Build outputs, such as JAR files, that can be distributed or deployed.
- **Imports:** Declarations that allow Java source code to refer to classes and interfaces by their simple names.
- **Packages:** A way to organise Java classes and interfaces using a consistent naming structure.
- **Maven coordinates:** The group ID, artifact ID, and version used to identify a Maven artifact.
- **YAML:** A human-readable format used to organise configuration data, including Spring Boot application settings.

These fundamentals help me understand how Java projects are structured, how Maven builds and manages them, and how configuration files support application development.
