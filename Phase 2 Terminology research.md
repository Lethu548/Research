# Phase 2: Spring Boot and Java Fundamentals

## 1. Why Use Lombok?

### Definition

Lombok is a Java library that reduces boilerplate code by generating common methods through annotations during compilation.

Boilerplate code is repetitive code that developers often have to write in multiple classes, such as getters, setters, constructors, and methods like `toString()`, `equals()`, and `hashCode()`.

### Why Is Lombok Used?

Lombok reduces repetitive code, making classes shorter and easier to read.

For example, instead of manually writing getters and setters, you can use Lombok annotations.

```java
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class DocumentMetadata {

    private String title;
    private String description;
}
```

Lombok generates the getter and setter methods during compilation.

### Why Do Teams Use Lombok?

- **Less code:** Developers can focus on the data and application logic instead of repetitive methods.
- **Fewer manual errors:** Generated methods such as `equals()` and `hashCode()` reduce the need to implement these methods manually, although developers must still choose the appropriate equality behaviour.
- **Immutability support:** Annotations such as `@Value` encourage immutable classes, while `@Builder` provides a convenient way to construct objects.
- **JPA compatibility:** Annotations such as `@NoArgsConstructor`, `@Getter`, and `@Setter` can be useful when working with JPA entities. However, the entity must still follow JPA requirements.
- **IDE support:** With the appropriate Lombok plugin or extension, IDEs can recognise generated methods and improve code navigation and assistance.

### Common Lombok Annotations

| Annotation | Purpose |
|---|---|
| `@Getter` | Generates getter methods. |
| `@Setter` | Generates setter methods. |
| `@NoArgsConstructor` | Generates a constructor without parameters. |
| `@AllArgsConstructor` | Generates a constructor with parameters for all fields. |
| `@ToString` | Generates a `toString()` method. |
| `@EqualsAndHashCode` | Generates `equals()` and `hashCode()` methods. |
| `@Data` | Combines getters, setters, `toString()`, `equals()`, and `hashCode()`, plus a required-arguments constructor. |
| `@Builder` | Provides a builder API for creating objects. |
| `@Value` | Creates an immutable-style class with final fields by default. |

**Important:** Lombok must be configured correctly in Maven and supported by your IDE for the generated code to work smoothly.

---

## 2. What Is Spring Data JPA?

### Definition

Spring Data JPA is a Spring module that simplifies database access by allowing developers to define repository interfaces. Spring Data generates implementations of those interfaces at runtime.

Instead of writing repetitive database-access code for every operation, developers can use repository methods to save, retrieve, update, and delete entities.

### Example

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface DocumentMetadataRepository
        extends JpaRepository<DocumentMetadata, Long> {
}
```

In this example:

- `DocumentMetadata` is the entity representing the document metadata.
- `Long` is the type of the entity's primary key.
- `JpaRepository` provides common database operations.

For example:

```java
repository.save(metadata);
repository.findById(1L);
repository.findAll();
repository.deleteById(1L);
```

Spring Data JPA provides the repository implementation, so you do not need to write each of these basic operations yourself.

### What Does Spring Data JPA Help With?

#### 2.1 Repository Implementations

Spring Data JPA creates repository implementations at runtime and provides common CRUD operations.

CRUD stands for:

- **Create:** Insert new records.
- **Read:** Retrieve records.
- **Update:** Modify existing records.
- **Delete:** Remove records.

#### 2.2 Persistence Context and Entity Management

JPA uses an `EntityManager` to manage entity persistence. Spring integrates JPA with its transaction management and persistence infrastructure.

The persistence context tracks managed entities and helps synchronise changes with the database.

#### 2.3 Transactions

Spring supports declarative transaction management, commonly through `@Transactional`.

Transactions help group database operations into a single unit of work. If a transaction fails and the failure triggers a rollback, the changes made within that transaction can be rolled back.

#### 2.4 Database Connections

Connection pooling is generally provided by the configured connection pool, such as HikariCP in many Spring Boot applications. It reuses database connections instead of opening a new connection for every request.

#### 2.5 Schema Generation

JPA providers, such as Hibernate, can generate or update database schemas from entity definitions when configured to do so.

For example:

```yaml
spring:
  jpa:
    hibernate:
      ddl-auto: update
```

The `update` setting asks Hibernate to update the schema based on the entity mappings. It is convenient for development but should be used cautiously in production because it is not a substitute for controlled database migrations.

#### 2.6 Pagination and Sorting

Spring Data JPA supports pagination and sorting through `Pageable` and `Sort`.

This allows an application to retrieve a subset of records instead of loading every record at once.

#### 2.7 Auditing

Spring Data JPA supports auditing fields such as:

- `@CreatedDate`
- `@LastModifiedDate`
- `@CreatedBy`
- `@LastModifiedBy`

These annotations can track when a record was created or modified and, when configured, who performed the operation.

#### 2.8 Dynamic Queries

Spring Data JPA supports specifications and the JPA Criteria API for building queries dynamically based on different conditions.

**In simple terms:** Spring Data JPA reduces the amount of database-access code developers have to write.

---

## 3. What Is H2 Database?

### Definition

H2 is a lightweight relational SQL database written in Java. It is commonly used for development, testing, and prototyping.

It can run in embedded mode inside the application's Java Virtual Machine (JVM), or it can run in server mode.

### Why Is H2 Used?

- **Lightweight:** It is relatively small and easy to include in a Java project.
- **Easy setup:** Embedded mode does not require a separately installed database server.
- **Testing:** It is useful for running database-related tests.
- **SQL support:** It supports SQL operations for working with relational data.
- **Web console:** It provides a browser-based console that can be enabled for inspecting the database.

### In-Memory Database

An in-memory H2 database stores its data in memory rather than in a persistent database file.

When the database instance is closed, its in-memory data is lost.

This makes it useful for tests that need a fresh database each time.

### Example Maven Dependency

Add the following dependency inside the `<dependencies>` section of your `pom.xml` file:

```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

If you use the Spring Boot parent or dependency management, you can usually omit the version because Spring Boot manages a compatible version.

### Example Spring Boot Configuration

In `src/main/resources/application.yml`:

```yaml
spring:
  datasource:
    url: jdbc:h2:mem:documentdb
    driver-class-name: org.h2.Driver
    username: sa
    password:

  jpa:
    hibernate:
      ddl-auto: create-drop

  h2:
    console:
      enabled: true
      path: /h2-console
```

This configuration uses an in-memory database called `documentdb`.

The `create-drop` setting creates the schema when the application starts and drops it when the persistence context is closed. It is intended for development and testing, not for production data.

When the console is enabled, it is generally available at:

```text
http://localhost:8080/h2-console
```

### Is H2 Always Faster Than MySQL or PostgreSQL?

Not necessarily. H2 can be very fast for lightweight tests because it runs locally and can operate in memory. However, performance depends on the workload, configuration, database features, and test environment.

H2 also does not behave identically to every production database. Tests against H2 alone may not detect database-specific SQL, migration, or behaviour issues.

---

## 4. What Is the Service Layer?

### Definition

The service layer is the business-logic layer between controllers and repositories in a typical Spring Boot application.

It is responsible for applying business rules and coordinating operations before data is saved or retrieved.

A common application structure looks like this:

```text
Client
  |
  v
Controller
  |
  v
Service
  |
  v
Repository
  |
  v
Database
```

### Responsibilities of the Service Layer

#### 4.1 Transaction Management

The service layer often defines transaction boundaries using `@Transactional`.

```java
@Transactional
public void createMetadata(DocumentMetadata metadata) {
    repository.save(metadata);
}
```

This tells Spring to manage a transaction around the method when the application is configured appropriately.

#### 4.2 Business Rules

The service layer enforces application-specific rules.

For example:

> Document metadata cannot be archived while documents still reference it.

The service can check for those references before allowing the operation.

#### 4.3 Orchestration

A service can coordinate multiple repositories or external services to complete one business operation.

For example, creating document metadata might involve saving metadata, recording an audit entry, and publishing an event.

#### 4.4 Validation

The service layer can enforce business rules involving multiple fields or entities.

For example, it might verify that a document's expiry date is later than its creation date.

Basic request validation can also be handled at the controller boundary using Bean Validation annotations.

#### 4.5 DTO Mapping

DTO stands for **Data Transfer Object**.

The service layer or a dedicated mapper can convert between entities and DTOs.

- **Entity:** Represents data persisted through JPA.
- **DTO:** Represents data transferred between parts of an application, such as an API request or response.

Separating DTOs from entities helps control which fields the API exposes.

#### 4.6 Events

A service can publish an event after a successful business operation.

For example, after saving document metadata, it might publish a `MetadataCreatedEvent` for another component to process.

#### 4.7 Security and Permissions

The service layer can enforce business-level authorisation checks, such as verifying whether a user is allowed to archive a document.

Security can also be enforced at the HTTP or method level, depending on the application's design.

### Without a Service Layer vs. With a Service Layer

| Without a service layer | With a service layer |
|---|---|
| Business logic can become scattered across controllers. | Business rules can be centralised. |
| Controllers may become difficult to maintain. | Controllers can focus on handling HTTP requests and responses. |
| Logic can be harder to test in isolation. | Business logic can be unit-tested with mocked dependencies. |
| Reusing logic in scheduled jobs or other components can be difficult. | Multiple components can call the same service. |
| Transaction boundaries may be unclear. | Transactions can be defined around business operations. |

**In simple terms:** Controllers handle HTTP requests, services handle business logic, and repositories handle database access.

---

## 5. What Are Enums?

### Definition

An enum, short for *enumeration*, is a Java type that represents a fixed set of named constants.

Enums are useful when a variable should only have one of a predefined set of values.

### Example

```java
public enum DocumentStatus {
    DRAFT,
    ACTIVE,
    ARCHIVED
}
```

You can use this enum in a class:

```java
public class DocumentMetadata {

    private DocumentStatus status;

    public DocumentMetadata(DocumentStatus status) {
        this.status = status;
    }
}
```

Now the `status` field can hold one of the declared `DocumentStatus` values.

### Why Use Enums Instead of Strings or Integers?

| Approach | Problem or benefit |
|---|---|
| String status | Values can contain spelling mistakes, such as `"ACTVI"` or `"Actve"`. |
| Integer status | Values such as `0`, `1`, and `2` can be unclear without additional documentation. |
| Enum status | Provides named, type-safe values with IDE support and safer refactoring. |

### Benefits of Enums

- **Type safety:** Java checks that the value belongs to the declared enum type.
- **Readability:** `DocumentStatus.ARCHIVED` is clearer than `2`.
- **IDE support:** IDEs can suggest available enum constants.
- **Refactoring:** Renaming constants can be managed through IDE refactoring tools.
- **Consistency:** A predefined set of values reduces accidental variations.

**Important:** When enums are persisted to a database, JPA can store them as ordinals or strings. Using `EnumType.STRING` is often preferable because it stores the enum name rather than its numeric position.

Example:

```java
@Enumerated(EnumType.STRING)
private DocumentStatus status;
```

This requires importing the relevant JPA annotations.

---

## 6. Why Use Utility Classes?

### Definition

A utility class contains reusable helper methods that perform general-purpose tasks without representing a particular business entity.

Utility classes are useful for operations involving strings, dates, numbers, formatting, or other common transformations.

### Example

```java
public final class DocumentUtils {

    private DocumentUtils() {
        // Prevents this utility class from being instantiated.
    }

    public static String normaliseTitle(String title) {
        return title.trim();
    }
}
```

You can call the method without creating an object:

```java
String title = DocumentUtils.normaliseTitle(" Project Report ");
```

The result is:

```text
Project Report
```

### Common Characteristics of Utility Classes

| Characteristic | Description |
|---|---|
| Static methods | Methods can be called using the class name without creating an instance. |
| No instance state | The class does not maintain per-object state. |
| General-purpose logic | Methods perform reusable operations rather than implementing a specific business workflow. |
| Final class | Prevents the class from being subclassed. |
| Private constructor | Prevents normal external instantiation. |

### What Does Stateless Mean?

A stateless method does not depend on changing instance state to perform its work.

For example, a method that trims a string is generally stateless because it processes the supplied input without needing stored object data.

Pure functions are an even stricter concept: a pure function produces the same output for the same input and has no observable side effects.

Not every static utility method is necessarily pure.

### When Should You Avoid a Utility Class?

Avoid placing business logic that depends on repositories, database transactions, Spring services, or external APIs inside a general-purpose utility class.

For example, a method that checks whether metadata can be archived based on database records belongs more naturally in a service because it depends on application business rules and data access.

---

## 7. Phase 2 Summary

By completing this phase, I understand that:

- **Lombok:** Reduces repetitive Java code through annotations.
- **Spring Data JPA:** Simplifies database access using repository interfaces.
- **H2:** Provides a lightweight SQL database useful for development and testing.
- **Service layer:** Holds business logic and coordinates application operations.
- **Enums:** Represent a fixed set of named, type-safe constants.
- **Utility classes:** Provide reusable helper methods for general-purpose operations.

These concepts work together in a Spring Boot application. Lombok reduces boilerplate, Spring Data JPA handles repository access, H2 provides a convenient database for tests, the service layer enforces business rules, enums restrict values to a known set, and utility classes support reusable helper operations.
