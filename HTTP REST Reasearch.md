#  Hypertext Transfer Protocol (HTTP)

## Introduction

Hypertext Transfer Protocol (HTTP) is an application-layer protocol used for communication between clients and servers on the World Wide Web. It follows a request-response model where a client sends a request and the server returns a response. HTTP forms the foundation of web applications, websites, and REST APIs.

## How HTTP Works

HTTP operates using a client-server architecture:

1. A client sends an HTTP request.
2. The server processes the request.
3. The server returns an HTTP response.
4. The client receives and displays the result.

### Request Flow

```text
Client → HTTP Request → Server
Client ← HTTP Response ← Server
```

## Characteristics of HTTP

### Stateless Protocol

HTTP is stateless, meaning each request is processed independently and the server does not automatically remember previous requests from the same client. Session management is typically implemented using cookies, tokens, or other mechanisms.

### Client-Server Architecture

HTTP separates the client from the server, allowing both components to evolve independently.

### Resource-Based Communication

Resources are identified using URLs (Uniform Resource Locators). A client accesses resources by sending requests to specific URLs.

## HTTP Versions

| Version  | Year | Status   |
| -------- | ---- | -------- |
| HTTP/0.9 | 1991 | Obsolete |
| HTTP/1.0 | 1996 | Obsolete |
| HTTP/1.1 | 1997 | Standard |
| HTTP/2   | 2015 | Standard |
| HTTP/3   | 2022 | Standard |

HTTP has evolved over time to improve performance, security, and scalability. Modern applications commonly use HTTP/2 and HTTP/3.

## Common HTTP Methods

HTTP methods define the action that should be performed on a resource.

| Method  | Description               |
| ------- | ------------------------- |
| GET     | Retrieve data             |
| POST    | Create new data           |
| PUT     | Update existing data      |
| DELETE  | Remove data               |
| PATCH   | Partially update data     |
| OPTIONS | View supported operations |
| HEAD    | Retrieve headers only     |

These methods form the foundation of RESTful API development.

## HTTP Status Codes

HTTP responses contain status codes indicating the result of a request.

### 1xx – Informational

Request received and processing continues.

### 2xx – Success

The request was processed successfully.

Examples:

* 200 OK
* 201 Created

### 3xx – Redirection

Additional action is required to complete the request.

### 4xx – Client Errors

The request contains invalid information.

Examples:

* 400 Bad Request
* 401 Unauthorized
* 404 Not Found

### 5xx – Server Errors

An error occurred on the server.

Examples:

* 500 Internal Server Error
* 503 Service Unavailable

## HTTPS

HTTPS (Hypertext Transfer Protocol Secure) is the secure version of HTTP. It uses encryption through TLS (Transport Layer Security) to protect data transmitted between clients and servers. HTTPS is essential for modern web applications because it improves security and protects sensitive information.

## Importance of HTTP in Spring Boot

Spring Boot applications rely heavily on HTTP for:

* REST API communication
* Web application requests
* Client-server interactions
* Data exchange using JSON and XML

---

# Representational State Transfer (REST)

## Introduction

REST (Representational State Transfer) is an architectural style introduced by Roy Fielding in 2000. It provides a set of principles used for designing scalable, maintainable, and distributed web services.

REST is widely used when developing web APIs because it is lightweight, flexible, and easy to integrate with different platforms.

## Purpose of REST

REST was created to support the development of large-scale distributed systems such as the World Wide Web. It promotes:

* Scalability
* Modularity
* Performance
* Reliability
* Simplicity

## REST Architectural Constraints

REST is based on six architectural constraints.

### 1. Client-Server

The client and server are independent systems that communicate through requests and responses.

### 2. Stateless

Every request contains all information required to process it.

### 3. Cacheable

Responses can be cached to improve performance.

### 4. Uniform Interface

Resources are accessed through consistent and predictable URLs.

### 5. Layered System

Multiple layers can exist between clients and servers without affecting communication.

### 6. Code on Demand (Optional)

Servers may provide executable code to clients when necessary.

## REST Resources

A resource is any piece of information exposed by an API.

Examples:

```text
/employees
/products
/customers
/orders
```

Each resource is identified by a unique URI.

## REST and HTTP

REST commonly uses HTTP methods to perform operations on resources.

| HTTP Method | Operation      |
| ----------- | -------------- |
| GET         | Retrieve       |
| POST        | Create         |
| PUT         | Update         |
| PATCH       | Partial Update |
| DELETE      | Remove         |

This mapping enables REST APIs to provide standard CRUD functionality.

## Benefits of REST

### Simplicity

REST uses standard HTTP methods and URLs.

### Scalability

REST supports distributed systems and large numbers of clients.

### Flexibility

REST can exchange data using multiple formats such as:

* JSON
* XML
* HTML

### Platform Independence

Clients and servers can be implemented using different programming languages.

## REST in Spring Boot

Spring Boot provides built-in support for RESTful API development through annotations such as:

* `@RestController`
* `@RequestMapping`
* `@GetMapping`
* `@PostMapping`
* `@PutMapping`
* `@DeleteMapping`

These annotations simplify API creation and reduce development time.

## Conclusion

HTTP and REST are fundamental technologies used in modern web application development. HTTP provides the communication protocol while REST provides the architectural principles for designing scalable web services. Together they form the foundation of Spring Boot REST API development and modern microservice architectures.
