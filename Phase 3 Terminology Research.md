# Phase 3: JUnit Testing and JPA Best Practices

## 1. What Is JUnit?

### Definition

JUnit is a testing framework for Java that allows developers to write and run automated tests. JUnit 5 is a widely used version of JUnit.

JUnit helps developers verify that individual methods and components behave as expected.

### What Does JUnit Provide?

| Feature | Purpose | Examples |
|---|---|---|
| Annotations | Identify tests and configure their execution. | `@Test`, `@BeforeEach`, `@AfterEach`, `@ParameterizedTest`, `@Disabled` |
| Assertions | Check whether the actual result matches the expected result. | `assertEquals()`, `assertTrue()`, `assertThrows()`, `assertAll()`, `assertLinesMatch()` |
| Lifecycle management | Run setup and cleanup methods before or after tests. | `@BeforeEach`, `@AfterEach`, `@BeforeAll`, `@AfterAll` |
| Extensions | Add functionality to the testing process. | Mockito integration, Spring testing support, custom extensions |
| Reporting | Produce test results for developers and build tools. | Maven Surefire reports, Gradle test reports |

### Common JUnit Annotations

| Annotation | Purpose |
|---|---|
| `@Test` | Marks a method as a test. |
| `@BeforeEach` | Runs before each test method. |
| `@AfterEach` | Runs after each test method. |
| `@BeforeAll` | Runs once before all test methods in a test class, subject to the test lifecycle configuration. |
| `@AfterAll` | Runs once after all test methods in a test class. |
| `@ParameterizedTest` | Runs the same test with different input values. |
| `@Disabled` | Disables a test or test class. |

### Example JUnit Test

Suppose you have a calculator method that adds two numbers.

```java
public class Calculator {

    public int add(int firstNumber, int secondNumber) {
        return firstNumber + secondNumber;
    }
}
```

You can test it using JUnit 5:

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class CalculatorTest {

    @Test
    void shouldAddTwoNumbers() {
        Calculator calculator = new Calculator();

        int result = calculator.add(5, 3);

        assertEquals(8, result);
    }
}
```

The test passes when the result is `8`.

### Why Is JUnit Important?

- **Early error detection:** Find problems before deploying the application.
- **Regression testing:** Verify that existing functionality still works after changes.
- **Confidence:** Confirm that methods produce the expected results.
- **Automation:** Run tests repeatedly without checking everything manually.
- **Maintainability:** Make it safer to refactor and improve code.

---

## 2. Types of Tests in Spring Boot

Spring Boot applications can be tested at different levels. The main difference is how much of the application is loaded and which components are being tested.

| Test type | Annotation or approach | Typical scope | Relative speed |
|---|---|---|---|
| Unit test | JUnit `@Test`, often with Mockito | One class with dependencies isolated or mocked | Fast |
| Repository slice test | `@DataJpaTest` | JPA repositories and related persistence configuration | Usually fast |
| MVC slice test | `@WebMvcTest` | Spring MVC controllers and selected web components | Usually fast |
| Full integration test | `@SpringBootTest` | The full Spring application context and selected real components | Usually slower |

Actual execution times depend on the application, test configuration, database, and environment.

### 2.1 Unit Tests

Unit tests check a small piece of functionality in isolation.

For example, you could test a utility method that removes spaces from a document title.

```java
@Test
void shouldRemoveSpacesFromTitle() {
    String result =
            DocumentMetadataUtils.normaliseTitle(
                    " Project Report ");

    assertEquals("Project Report", result);
}
```

Unit tests generally avoid loading the entire Spring application context.

### 2.2 Repository Slice Tests

`@DataJpaTest` loads a focused Spring test context for JPA-related components.

It is useful for testing:

- Repository queries.
- Entity mappings.
- Relationships between entities.
- Database persistence behaviour.

Example:

```java
@DataJpaTest
class DocumentMetadataRepositoryTest {

    @Autowired
    private DocumentMetadataRepository repository;

    @Test
    void shouldSaveDocumentMetadata() {
        DocumentMetadata metadata = new DocumentMetadata();

        metadata.setTitle("Project Report");

        DocumentMetadata saved = repository.save(metadata);

        assertNotNull(saved.getId());
        assertEquals("Project Report", saved.getTitle());
    }
}
```

By default, `@DataJpaTest` commonly configures an embedded database when one is available and wraps each test in a transaction that is rolled back afterward.

### 2.3 MVC Slice Tests

`@WebMvcTest` focuses on the web layer, including Spring MVC controller behaviour.

It is useful for checking:

- HTTP endpoints.
- Request validation.
- Response status codes.
- JSON responses.
- Controller behaviour with mocked service dependencies.

### 2.4 Full Integration Tests

`@SpringBootTest` loads the application context for broader integration testing.

It can be used to verify that different components work together, such as controllers, services, repositories, and database configuration.

A full integration test does not automatically require a real external database. The database depends on the test configuration; Testcontainers can be used to run a real database in a container.

### When Should You Use Each Test Type?

- Use **unit tests** for individual methods and business rules.
- Use **`@DataJpaTest`** for repository queries and persistence behaviour.
- Use **`@WebMvcTest`** for controller and HTTP behaviour.
- Use **`@SpringBootTest`** when you need to verify how several application components work together.

---

## 3. Best Practices for JPA Calls

JPA stands for **Java Persistence API**. It provides a standard for mapping Java objects to relational database records. Hibernate is a common JPA implementation.

Good JPA practices help improve performance, prevent unnecessary database queries, and make data-access code easier to maintain.

### 3.1 Fetch Strategies: Avoid the N+1 Query Problem

The N+1 query problem occurs when an application executes one query to retrieve a group of records and then executes additional queries to retrieve related data for each record.

For example:

1. One query retrieves 100 documents.
2. Each document triggers another query to retrieve its author.
3. The application may execute 101 queries instead of a smaller number.

This can significantly affect performance.

#### How to Avoid It

- Use fetch joins when related data is needed.
- Consider entity graphs for specifying the relationships to fetch.
- Use DTO projections when only selected fields are required.
- Inspect generated SQL and measure query performance.
- Avoid switching every relationship to `EAGER` fetching as a blanket solution.

**Important:** `LAZY` and `EAGER` describe fetch strategies. They do not guarantee a particular number of SQL queries in every situation.

### 3.2 Transactions: Define Clear Boundaries

A transaction groups database operations into a unit of work.

Spring supports transaction management using `@Transactional`.

```java
@Transactional
public DocumentMetadata createMetadata(
        DocumentMetadata metadata) {
    return repository.save(metadata);
}
```

A service method is often a suitable place to define the transaction boundary because it represents a business operation.

#### Read-Only Transactions

```java
@Transactional(readOnly = true)
public List<DocumentMetadata> findAllMetadata() {
    return repository.findAll();
}
```

The `readOnly = true` attribute indicates that the transaction is intended for reading.

Depending on the transaction manager and JPA provider, this hint may enable optimisations. It does **not** universally guarantee that dirty checking is disabled or that flushing is impossible.

#### Transaction Boundary Guidelines

- Put transaction boundaries around appropriate business operations.
- Use service methods to coordinate multiple repository calls.
- Use read-only transactions for read operations where appropriate.
- Understand when transactions commit or roll back.
- Avoid putting transaction boundaries in controllers without a clear reason.

### 3.3 Prefer Derived Repository Queries When Appropriate

Spring Data JPA can generate queries from repository method names.

For straightforward queries, derived methods are often easier to read than manually written JPQL.

For example:

```java
public interface DocumentMetadataRepository
        extends JpaRepository<DocumentMetadata, Long> {

    List<DocumentMetadata> findByAuthorAndStatus(
            String author,
            DocumentStatus status);

    boolean existsByTitleIgnoreCase(String title);
}
```

These methods allow Spring Data JPA to derive the query from the method name.

#### Derived Queries vs. Custom Queries

| Prefer derived queries when... | Consider custom queries when... |
|---|---|
| The query is simple and easy to express through a method name. | The query contains complex joins or conditions. |
| You need straightforward filtering. | You need aggregation or a specialised projection. |
| You need an existence check. | A derived method would become excessively long or difficult to understand. |

For example, `existsByTitleIgnoreCase()` is a suitable existence check when it matches the required business rule.

**Important:** Custom JPQL is not inherently bad. Choose the approach that makes the query correct, understandable, and maintainable.

### 3.4 Use Pagination for Large Datasets

Retrieving every record can consume excessive memory and increase response times.

Spring Data JPA supports pagination through `Pageable`.

Example repository:

```java
Page<DocumentMetadata> findAll(Pageable pageable);
```

Example service:

```java
@Transactional(readOnly = true)
public Page<DocumentMetadata> findAllMetadata(
        Pageable pageable) {
    return repository.findAll(pageable);
}
```

Example usage:

```java
Pageable pageable = PageRequest.of(0, 20);

Page<DocumentMetadata> result =
        service.findAllMetadata(pageable);
```

This requests the first page containing up to 20 records.

A `Page` can provide:

- `getContent()` — records on the current page.
- `getTotalElements()` — total number of matching records.
- `getTotalPages()` — number of pages.
- `getNumber()` — current page index.

For very large datasets, counting all matching rows can itself be expensive. Depending on the use case, `Slice` or keyset pagination may be more appropriate.

### 3.5 Use DTO Projections Instead of Loading Unnecessary Entities

DTO stands for **Data Transfer Object**.

A projection allows the application to retrieve only the fields needed for a particular operation.

For example, an API that only displays a document's title and status may not need to retrieve every entity field or related entity.

Example DTO:

```java
public record DocumentMetadataSummary(
        Long id,
        String title,
        DocumentStatus status) {
}
```

A repository can define a projection query:

```java
@Query("""
    SELECT new com.example.documentmetadata.DocumentMetadataSummary(
        d.id, d.title, d.status
    )
    FROM DocumentMetadata d
    """)
List<DocumentMetadataSummary> findAllSummaries();
```

Replace the package name with the actual package containing your DTO.

#### Benefits

- Retrieves only the required fields.
- Can reduce unnecessary data loading.
- Helps prevent accidental exposure of internal entity fields.
- Makes API responses easier to design and maintain.

Entities and DTOs serve different purposes. Returning DTOs from APIs is often preferable to exposing JPA entities directly.

### 3.6 Implement `equals()` and `hashCode()` Carefully

These methods determine object equality and hash-based collection behaviour.

JPA entities require particular care because:

- Entity IDs may not exist until after persistence.
- Hibernate may use proxies.
- Entity fields can change over time.
- Relationships can introduce recursion or unexpected equality behaviour.

Avoid automatically including every entity field in `equals()` and `hashCode()`. Doing so can cause problems when mutable fields change or relationships are loaded.

Choose an equality strategy that matches the entity's lifecycle and identity requirements. A stable, unique business key can be appropriate when one exists. Using only a generated ID also requires care when comparing two transient entities whose IDs are both `null`.

### 3.7 Understand Dirty Checking

Dirty checking is a JPA mechanism that detects changes to managed entities and synchronises those changes with the database during flushing.

Example:

```java
@Transactional
public void updateTitle(Long id, String newTitle) {
    DocumentMetadata metadata =
            repository.findById(id)
                    .orElseThrow();

    metadata.setTitle(newTitle);
}
```

The entity is managed within the transaction. When the transaction is flushed or committed, Hibernate can detect the changed title and issue an update.

You generally do not need to call `repository.save()` again for an already managed entity just to persist its changes.

Use `save()` when creating new entities or when its merge semantics are appropriate for the entity's state.

### 3.8 Use Bulk Updates and Deletes Carefully

Bulk JPQL operations can update or delete many database records using a single SQL statement.

For example:

```java
@Modifying
@Query("""
    UPDATE DocumentMetadata d
    SET d.status = :status
    WHERE d.status = :oldStatus
    """)
int updateStatus(
        @Param("oldStatus") DocumentStatus oldStatus,
        @Param("status") DocumentStatus status);
```

The method can update multiple rows without loading every entity individually.

However, bulk operations bypass normal per-entity dirty checking and lifecycle callbacks. Entities already loaded into the persistence context can become stale.

When appropriate, consider clearing or refreshing the persistence context after the operation. Spring Data JPA also supports options such as `clearAutomatically = true` on `@Modifying`.

Use bulk operations only when their behaviour matches the application's requirements.

### 3.9 Configure the Database Connection Pool

A connection pool reuses database connections rather than creating a new connection for every database operation.

Spring Boot commonly uses HikariCP by default when it is available.

Connection-pool settings may include:

- Maximum pool size.
- Connection timeout.
- Idle timeout.
- Maximum connection lifetime.

Example configuration:

```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 10
      connection-timeout: 30000
```

These values are examples, not universal recommendations. Production settings should be based on workload, database capacity, and deployment resources.

### 3.10 Consider Disabling Open Session in View

Open Session in View (OSIV) allows the persistence context to remain available through web request processing, depending on the framework configuration.

In Spring Boot web applications, this behaviour is commonly enabled by default for JPA.

When enabled, lazy-loaded relationships may be accessed after the service transaction has completed, potentially causing additional database queries during response processing.

To disable it:

```yaml
spring:
  jpa:
    open-in-view: false
```

#### Why Disable It?

- Encourages explicit control over which data is loaded.
- Helps keep database access inside the service or repository layer.
- Makes it easier to identify lazy-loading problems.
- Reduces the risk of accidental database queries during response serialisation.

**Important:** Disabling OSIV does not automatically eliminate N+1 queries. Fetch plans and queries must still be designed correctly. Required relationships should be loaded within the appropriate transaction.

---

## 4. Quick Reference: JPA Best Practices

| Prefer | Avoid |
|---|---|
| Explicit transaction boundaries | Unclear transaction ownership |
| Derived queries for simple operations | Overly complex repository method names |
| Pagination for large result sets | Loading every record unnecessarily |
| DTO projections for selected fields | Exposing entities directly without considering API needs |
| Carefully designed fetch plans | Ignoring N+1 queries |
| Appropriate entity equality rules | Including every mutable field in `equals()` and `hashCode()` |
| Letting dirty checking persist managed changes | Calling `save()` unnecessarily on every managed entity |
| Bulk operations when appropriate | Ignoring persistence-context consistency |
| Measured connection-pool configuration | Arbitrary production settings |
| Explicit data loading and considered OSIV settings | Unplanned lazy loading during response processing |

---

## 5. Phase 3 Summary

By completing this phase, you should understand:

- **JUnit:** How to write automated tests for Java applications.
- **Unit tests:** How to test individual units of code in isolation.
- **Spring slice tests:** How to test focused parts of a Spring application.
- **Integration tests:** How to verify that application components work together.
- **Fetch strategies:** How to control the loading of related entities and investigate N+1 queries.
- **Transactions:** How to define boundaries for database operations.
- **Repository queries:** When to use derived query methods and when custom queries are appropriate.
- **Pagination:** How to retrieve large datasets in manageable pages.
- **DTO projections:** How to retrieve only the data required by an operation.
- **Entity equality:** Why JPA entity equality needs careful design.
- **Dirty checking:** How Hibernate detects changes to managed entities.
- **Bulk operations:** How to update or delete multiple records efficiently.
- **Connection pooling:** How database connections can be reused.
- **Open Session in View:** How persistence-context scope affects lazy loading.

These practices help make Spring Boot applications more reliable, testable, and efficient as they grow.
