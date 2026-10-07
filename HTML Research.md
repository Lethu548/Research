# HTML Fundamentals, Forms, and URL Encoding

## Introduction

HyperText Markup Language (HTML) is the standard markup language used to create and structure web pages. It provides the foundation for web content by defining elements such as headings, paragraphs, images, links, tables, and forms. HTML works together with CSS and JavaScript to create modern web applications.

This research focuses on HTML fundamentals, semantic HTML, forms, form submission methods, URL encoding, and accessibility considerations.

---

# What is HTML?

HTML (HyperText Markup Language) is used to structure content on the web. It consists of elements and tags that tell a browser how content should be displayed.

HTML enables developers to create:

* Headings
* Paragraphs
* Hyperlinks
* Images
* Tables
* Lists
* Forms
* Navigation menus

HTML serves as the foundation of every website and web application.

---

# Semantic HTML

## What is Semantic HTML?

Semantic HTML refers to the use of meaningful HTML elements that clearly describe their purpose within a web page.

Instead of using generic elements for everything, semantic elements provide meaning to both developers and browsers.

### Common Semantic Elements

```html
<header>
<nav>
<main>
<section>
<article>
<footer>
```

### Benefits of Semantic HTML

* Improves website accessibility.
* Enhances search engine optimization (SEO).
* Makes code easier to read and maintain.
* Helps screen readers understand page content.
* Provides better document structure.

---

# HTML Forms

## What is an HTML Form?

An HTML form is used to collect information from users and send it to a server for processing.

Forms are commonly used for:

* User registration
* Login systems
* Contact forms
* Search functionality
* Online purchases
* Feedback collection

### Basic Form Components

```html
<form>
<label>
<input>
<button>
```

### Example Form

```html
<form>
    <label>Name:</label>
    <input type="text">

    <button type="submit">
        Submit
    </button>
</form>
```

When a user submits a form, the browser sends the collected information to a server.

---

# Form Attributes

The `<form>` element contains important attributes that control how data is submitted.

## Action Attribute

The `action` attribute specifies where the form data will be sent.

Example:

```html
<form action="/submit">
```

---

## Method Attribute

The `method` attribute specifies how data will be transmitted.

### GET Method

```html
<form method="GET">
```

Characteristics:

* Data is appended to the URL.
* Suitable for search operations.
* Data is visible in the browser address bar.
* Not recommended for sensitive information.

Example:

```text
https://example.com/search?name=John
```

---

### POST Method

```html
<form method="POST">
```

Characteristics:

* Data is sent in the request body.
* More secure than GET.
* Suitable for passwords and sensitive information.
* Commonly used in web applications.

---

# URL Encoding

## What is URL Encoding?

URL encoding is the process of converting special characters into a format that can be safely transmitted within a URL.

Some characters have special meanings in URLs and must be encoded before transmission.

### Examples

| Character | Encoded Value |
| --------- | ------------- |
| Space     | `%20` or `+`  |
| @         | `%40`         |
| #         | `%23`         |
| &         | `%26`         |

Example:

```text
John Doe
```

Becomes:

```text
John%20Doe
```

---

## Why URL Encoding is Important

URL encoding ensures that:

* Special characters do not break URLs.
* Data is transmitted correctly.
* Browsers can interpret user input properly.
* Form submissions are processed accurately.

URL encoding is commonly used when forms are submitted using the GET method.

---

# Form Encoding Types

HTML forms support different encoding methods through the `enctype` attribute.

## application/x-www-form-urlencoded

```html
<form enctype="application/x-www-form-urlencoded">
```

Characteristics:

* Default encoding type.
* Suitable for most form submissions.
* Encodes special characters before transmission.

---

## multipart/form-data

```html
<form enctype="multipart/form-data">
```

Characteristics:

* Used for file uploads.
* Allows binary data to be transmitted.
* Required when uploading images, documents, or videos.

---

## text/plain

```html
<form enctype="text/plain">
```

Characteristics:

* Sends data as plain text.
* Rarely used in production environments.
* Primarily used for testing purposes.

---

# Accessibility in HTML

## Importance of Accessibility

Accessibility ensures that websites can be used by all users, including those with disabilities.

Proper HTML structure improves usability for:

* Screen reader users
* Keyboard-only users
* Users with visual impairments
* Users with motor impairments

---

## Accessibility Best Practices

### Use Labels with Inputs

Example:

```html
<label for="username">
    Username
</label>

<input id="username" type="text">
```

### Use Semantic HTML

Use elements such as:

```html
<header>
<nav>
<main>
<footer>
```

instead of relying solely on generic `<div>` elements.

### Provide Meaningful Content Structure

A clear document structure helps assistive technologies understand the page.

---

# Key Findings

Through this research, the following findings were identified:

* HTML provides the structure for web pages.
* Semantic HTML improves accessibility, maintainability, and SEO.
* Forms allow users to interact with web applications.
* GET and POST methods are used to submit form data.
* URL encoding ensures special characters are transmitted safely.
* Different form encoding types support different use cases.
* Accessibility should be considered when designing web pages and forms.

---

# What I Learned

During this research, I learned that HTML is more than just displaying content on a webpage. Semantic HTML provides meaningful structure that improves accessibility and search engine optimization. I also learned how HTML forms collect and transmit user data using GET and POST methods. Additionally, I explored URL encoding and how it allows special characters to be safely included in URLs. Finally, I learned the importance of accessibility and proper form design when building web applications.

---

# Conclusion

HTML is the foundation of web development and plays a critical role in structuring and presenting content on the internet. Understanding semantic HTML, forms, URL encoding, and accessibility principles enables developers to create more effective, maintainable, and user-friendly web applications. These concepts form an essential foundation for further learning in web development and backend technologies such as Spring Boot.
