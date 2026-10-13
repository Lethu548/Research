# Phase 2: Practical Exercises

These exercises reinforce Lombok, Spring Data JPA, H2, the service layer, enums, and utility classes.

**Project scenario:** Build a simple Document Metadata API that allows users to create, retrieve, update, and manage document metadata.

## Exercise 1: Practise Lombok

**Objective:** Reduce boilerplate code in a Java class.

### Tasks

1. Create a `DocumentMetadata` class.
2. Add these fields:
   - `Long id`
   - `String title`
   - `String description`
   - `String author`
3. Use Lombok annotations to generate getters, setters, and constructors.
4. Create a metadata object and print its values.

### Example

```java
import lombok.Getter;
import lombok.Setter;
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class DocumentMetadata {

    private Long id;
    private String title;
    private String description;
    private String author;
}
```

### Test yourself

- Can you access the title using `getTitle()`?
- Can you change it using `setTitle()`?
- What happens if you remove `@Getter`?
- Why must Lombok be added to your Maven dependencies?

**Expected outcome:** You can create and use a Java class without manually writing every getter and setter.

---

## Exercise 2: Create an Entity Using Spring Data JPA

**Objective:** Map a Java class to a database table.

### Tasks

1. Annotate `DocumentMetadata` with `@Entity`.
2. Mark `id` with `@Id`.
3. Use `@GeneratedValue` to generate IDs.
4. Create a repository extending `JpaRepository`.
5. Save and retrieve metadata.

### Example entity

```java
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import lombok.Getter;
import lombok.Setter;
import lombok.NoArgsConstructor;

@Entity
@Getter
@Setter
@NoArgsConstructor
public class DocumentMetadata {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;
    private String description;
    private String author;
}
```

### Repository

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface DocumentMetadataRepository
        extends JpaRepository<DocumentMetadata, Long> {
}
```

### Test yourself

- What does `@Entity` do?
- Why is `@Id` required?
- What does `JpaRepository` provide?
- Why does the repository not need you to implement `save()` manually?

**Expected outcome:** You can persist and retrieve document metadata using a repository.

**Note:** This example uses `jakarta.persistence`, which is appropriate for Spring Boot 3 and later. Spring Boot 2 projects generally use `javax.persistence` instead.

---

## Exercise 3: Configure H2 Database

**Objective:** Run your application against an in-memory database.

### Tasks

1. Add the H2 dependency to `pom.xml`.
2. Configure the database in `application.yml`.
3. Set up JPA schema generation for development.
4. Run your Spring Boot application.
5. Save metadata and retrieve it.

### Example configuration

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

### Test yourself

- What is an in-memory database?
- What happens to its data when the database instance shuts down?
- Why is H2 useful for automated tests?
- Why should you avoid relying on `create-drop` for production data?

**Expected outcome:** Your application starts with an H2 database and can store metadata while the database instance is running.

---

## Exercise 4: Create a Service Layer

**Objective:** Separate business logic from database-access logic.

### Tasks

1. Create a `DocumentMetadataService` class.
2. Inject `DocumentMetadataRepository` through the constructor.
3. Create a method to save metadata.
4. Create a method to retrieve metadata by ID.
5. Throw an appropriate exception when the requested record does not exist.

### Starter code

```java
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class DocumentMetadataService {

    private final DocumentMetadataRepository repository;

    public DocumentMetadataService(
            DocumentMetadataRepository repository) {
        this.repository = repository;
    }

    @Transactional
    public DocumentMetadata create(DocumentMetadata metadata) {
        return repository.save(metadata);
    }

    @Transactional(readOnly = true)
    public DocumentMetadata getById(Long id) {
        return repository.findById(id)
                .orElseThrow(() ->
                        new RuntimeException(
                                "Document metadata not found"));
    }
}
```

### Improve the implementation

Replace the generic `RuntimeException` with a meaningful custom exception or an appropriate exception handled by your API.

### Test yourself

- Why should business logic be in a service?
- Why is constructor injection useful?
- What does `@Transactional` do?
- Why might a scheduled job need to call the same service?

**Expected outcome:** Your service handles application operations while the repository handles database access.

---

## Exercise 5: Create and Use an Enum

**Objective:** Restrict metadata status to a predefined set of values.

### Tasks

1. Create an enum called `DocumentStatus`.
2. Add `DRAFT`, `ACTIVE`, and `ARCHIVED`.
3. Add a `status` field to `DocumentMetadata`.
4. Use `@Enumerated(EnumType.STRING)` to persist the enum as text.
5. Set the default status to `DRAFT` when creating metadata.

### Enum

```java
public enum DocumentStatus {
    DRAFT,
    ACTIVE,
    ARCHIVED
}
```

### Entity field

```java
@Enumerated(EnumType.STRING)
private DocumentStatus status = DocumentStatus.DRAFT;
```

Add the required `jakarta.persistence.Enumerated` and `jakarta.persistence.EnumType` imports.

### Test yourself

- Why is an enum safer than storing status as an unrestricted string?
- What happens if you try to assign `"ACTIVE"` directly to a `DocumentStatus` variable?
- Why might storing enum names be safer than storing ordinal numbers?

**Expected outcome:** Each metadata record has a status selected from the allowed values.

---

## Exercise 6: Create a Utility Class

**Objective:** Practise writing reusable helper methods.

### Tasks

1. Create a `DocumentMetadataUtils` class.
2. Add a method to remove leading and trailing spaces from a title.
3. Add a method to check whether a title is blank.
4. Make the class `final`.
5. Add a private constructor.

### Starter code

```java
public final class DocumentMetadataUtils {

    private DocumentMetadataUtils() {
        // Prevents creating instances of this class.
    }

    public static String normaliseTitle(String title) {
        if (title == null) {
            return null;
        }

        return title.trim();
    }

    public static boolean isBlank(String title) {
        return title == null || title.isBlank();
    }
}
```

### Test yourself

- Why are these methods static?
- Why is the constructor private?
- Why does the class not need `@Service`?
- What is the difference between a utility method and a business operation?

**Expected outcome:** You can reuse general-purpose helper methods without creating utility objects.

**Note:** `String.isBlank()` requires Java 11 or later.

---

## Exercise 7: Write Unit Tests

**Objective:** Verify that your utility methods and service logic work correctly.

### Utility test examples

Using JUnit 5:

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

class DocumentMetadataUtilsTest {

    @Test
    void shouldRemoveSpacesFromTitle() {
        String result =
                DocumentMetadataUtils.normaliseTitle(
                        " Project Report ");

        assertEquals("Project Report", result);
    }

    @Test
    void shouldIdentifyBlankTitle() {
        assertTrue(
                DocumentMetadataUtils.isBlank("   "));
    }

    @Test
    void shouldIdentifyNullTitle() {
        assertTrue(
                DocumentMetadataUtils.isBlank(null));
    }
}
```

### Additional tasks

- Test a normal title.
- Test a blank title.
- Test a `null` title.
- Write a service test using a mocked repository.
- Write a repository integration test using H2.

**Expected outcome:** You can distinguish unit tests, which test small units in isolation, from integration tests, which verify components working together.

---

## Exercise 8: Integrate Everything

**Objective:** Build a small working document metadata feature.

### Requirements

Implement the following:

- [ ] Create a `DocumentMetadata` entity.
- [ ] Use Lombok to reduce boilerplate.
- [ ] Create a `DocumentStatus` enum.
- [ ] Configure H2 for development and testing.
- [ ] Create a `DocumentMetadataRepository`.
- [ ] Create a `DocumentMetadataService`.
- [ ] Create a `DocumentMetadataController`.
- [ ] Add an endpoint to create metadata.
- [ ] Add an endpoint to retrieve metadata by ID.
- [ ] Return an appropriate HTTP status when metadata is not found.
- [ ] Write unit tests for utility and service logic.
- [ ] Write integration tests for repository and API behaviour.

### Suggested folder structure

```text
src/
├── main/
│   ├── java/
│   │   └── com/example/documentmetadata/
│   │       ├── controller/
│   │       │   └── DocumentMetadataController.java
│   │       ├── service/
│   │       │   └── DocumentMetadataService.java
│   │       ├── repository/
│   │       │   └── DocumentMetadataRepository.java
│   │       ├── entity/
│   │       │   └── DocumentMetadata.java
│   │       ├── enums/
│   │       │   └── DocumentStatus.java
│   │       ├── util/
│   │       │   └── DocumentMetadataUtils.java
│   │       └── DocumentMetadataApplication.java
│   └── resources/
│       └── application.yml
└── test/
    └── java/
        └── com/example/documentmetadata/
```

Adapt the package name to your existing Maven project rather than creating a second application unnecessarily.

---

## 9. Commands to Run Your Tests

Run these commands from the directory containing `pom.xml`.

Run the tests:

```bash
mvn test
```

Clean the previous build output and package the project:

```bash
mvn clean package
```

Build the project, run its configured tests, and install the artifact into your local Maven repository:

```bash
mvn clean install
```

Review the test results before moving to the next exercise.

---

## 10. Completion Checklist

By the end of Phase 2, you should be able to explain:

- [ ] How Lombok generates boilerplate code.
- [ ] How Spring Data JPA repositories access a database.
- [ ] How H2 can support database testing.
- [ ] Why a service layer separates business logic from HTTP and database concerns.
- [ ] How enums provide type-safe status values.
- [ ] How utility classes provide reusable helper methods.
- [ ] How to test individual methods and integrated application components.

**Final challenge:** Add an endpoint that archives a document's metadata, but only if no documents reference it. Implement the business rule in the service layer and write a test proving that archiving is rejected when references exist.
