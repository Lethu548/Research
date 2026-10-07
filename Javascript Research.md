# JavaScript Fundamentals and Advanced Concepts

## 1. Introduction

JavaScript is a powerful and flexible programming language widely used to make websites interactive and dynamic. It can modify HTML and CSS dynamically and can also run on servers using technologies such as Node.js.

JavaScript is commonly used together with:

* **HTML** – provides the structure of a webpage.
* **CSS** – controls the presentation and styling.
* **JavaScript** – provides behaviour, logic, and interactivity.

This research covers JavaScript fundamentals and several more advanced concepts, including variables, data types, operators, control flow, arrays, strings, functions, objects, object-oriented programming, asynchronous programming, promises, the event loop, and error handling.

---

# 2. JavaScript Variables

A variable is a container used to store a value in memory. JavaScript provides three keywords for declaring variables:

* `var`
* `let`
* `const`

## 2.1 `var`

`var` can be function-scoped or globally scoped.

```javascript
var number = 5;

console.log(number);

var number = 20;

console.log(number);
```

A variable declared using `var` can be reassigned and redeclared in the same scope.

---

## 2.2 `let`

`let` was introduced with ES6 and has block scope.

```javascript
let number = 10;

number = 20;

console.log(number);
```

A `let` variable can be updated, but it cannot be redeclared in the same scope.

---

## 2.3 `const`

`const` is also block-scoped.

```javascript
const number = 100;

console.log(number);
```

A variable declared with `const` cannot be reassigned.

---

# 3. JavaScript Data Types

JavaScript data types are divided into **primitive** and **non-primitive** types.

## 3.1 Primitive Data Types

Primitive values represent single values.

JavaScript has the following primitive data types:

* Number
* String
* Boolean
* Undefined
* Null
* BigInt
* Symbol

### Number

Used for integers and decimal numbers.

```javascript
let number = 42;
let pi = 3.14;
```

### String

Used to represent text.

```javascript
let message = "Hello World";
```

### Boolean

Represents either `true` or `false`.

```javascript
let loggedIn = true;
```

### Undefined

Represents a variable that has been declared but has not been assigned a value.

```javascript
let value;

console.log(value);
```

### Null

Represents an intentional absence of a value.

```javascript
let value = null;
```

### Symbol

Represents a unique value and can be used as an object key.

```javascript
let id = Symbol("unique");
```

### BigInt

Used for integers larger than the safe integer range of the Number type.

```javascript
let largeNumber = 12345678901234567890n;
```

---

## 3.2 Non-Primitive Data Types

Non-primitive values include:

* Objects
* Arrays
* Functions

Example:

```javascript
let person = {
    name: "Gugu",
    age: 25
};

let languages = ["HTML", "CSS", "JavaScript"];

function greet() {
    console.log("Hello");
}
```

---

# 4. Important JavaScript Comparisons

JavaScript has several behaviours that are important to understand when comparing values.

## `null` and `undefined`

```javascript
console.log(null === undefined);
```

Output:

```text
false
```

`null` and `undefined` are different values even though both can represent an absence of a value.

---

## Comparing Arrays

```javascript
console.log([] === []);
```

Output:

```text
false
```

Arrays are objects and comparisons between separately created arrays compare their references rather than their contents.

---

## Strict Equality

```javascript
console.log("5" === 5);
```

Output:

```text
false
```

The strict equality operator checks both value and type.

---

## Loose Equality

```javascript
console.log(true == 1);
```

Output:

```text
true
```

The loose equality operator can perform type coercion.

---

## `NaN`

```javascript
console.log(NaN === NaN);
```

Output:

```text
false
```

`NaN` represents a value that is not a valid number result.

To check for `NaN`, JavaScript provides:

```javascript
Number.isNaN(value);
```

---

# 5. JavaScript Operators

Operators are symbols or keywords used to perform operations on values.

## 5.1 Arithmetic Operators

```javascript
const sum = 5 + 3;
const difference = 10 - 2;
const product = 4 * 2;
const quotient = 8 / 2;
```

Common arithmetic operators include:

| Operator | Purpose        |
| -------- | -------------- |
| `+`      | Addition       |
| `-`      | Subtraction    |
| `*`      | Multiplication |
| `/`      | Division       |
| `%`      | Remainder      |

---

## 5.2 Assignment Operators

Assignment operators assign values to variables.

```javascript
let number = 10;

number += 5;
number *= 2;
```

Common assignment operators include:

* `=`
* `+=`
* `-=`
* `*=`
* `/=`

---

## 5.3 Comparison Operators

Comparison operators return a Boolean result.

```javascript
console.log(10 > 5);
console.log(10 === "10");
```

Common operators include:

* `>`
* `<`
* `>=`
* `<=`
* `===`
* `!==`

---

## 5.4 Logical Operators

Logical operators are used to combine conditions.

```javascript
const a = true;
const b = false;

console.log(a && b);
console.log(a || b);
console.log(!a);
```

| Operator | Purpose |   |    |
| -------- | ------- | - | -- |
| `&&`     | AND     |   |    |
| `        |         | ` | OR |
| `!`      | NOT     |   |    |

---

## 5.5 Bitwise Operators

Bitwise operators work with binary representations of numbers.

Examples include:

```text
&
|
^
~
<<
>>
>>>
```

---

## 5.6 Ternary Operator

The ternary operator provides a short form of an `if...else` expression.

```javascript
const age = 18;

const status = age >= 18 ? "Adult" : "Minor";

console.log(status);
```

Syntax:

```javascript
condition ? expression1 : expression2;
```

---

## 5.7 Unary Operators

Unary operators operate on a single value.

Examples include:

```javascript
+x
-x
++x
--x
!x
typeof x
delete obj.property
```

---

## 5.8 Relational Operators

Relational operators can determine relationships between values.

```javascript
const obj = {
    length: 10
};

console.log("length" in obj);
console.log([] instanceof Array);
```

* `in` checks whether a property exists.
* `instanceof` checks whether an object is an instance of a constructor.

---

## 5.9 Optional Chaining Operator

The optional chaining operator `?.` allows safe access to nested properties.

```javascript
const person = {
    name: "Aman",
    address: {
        city: "Delhi"
    }
};

console.log(person.address?.city);
console.log(person.contact?.phone);
```

If a property does not exist, optional chaining can return `undefined` instead of immediately causing an error.

---

# 6. JavaScript Control Flow

Control flow determines the order in which code executes.

## 6.1 `if`

```javascript
const age = 18;

if (age >= 18) {
    console.log("You are an adult.");
}
```

The block runs when the condition is true.

---

## 6.2 `if...else`

```javascript
const score = 40;

if (score >= 50) {
    console.log("You passed.");
} else {
    console.log("You failed.");
}
```

---

## 6.3 `if...else if...else`

Used when multiple conditions need to be evaluated.

```javascript
const temperature = 25;

if (temperature > 30) {
    console.log("It's hot.");
} else if (temperature >= 20) {
    console.log("It's warm.");
} else {
    console.log("It's cold.");
}
```

---

## 6.4 `switch`

A `switch` statement compares an expression with different cases.

```javascript
const day = "Monday";

switch (day) {
    case "Monday":
        console.log("Start of the week.");
        break;

    case "Friday":
        console.log("End of the workweek.");
        break;

    default:
        console.log("It's a regular day.");
}
```

The `break` statement prevents execution from continuing into the next case.

---

# 7. JavaScript Loops

Loops are used to execute code repeatedly.

## 7.1 `for` Loop

Used when the number of iterations is known.

```javascript
for (let i = 1; i <= 3; i++) {
    console.log(i);
}
```

---

## 7.2 `while` Loop

Runs while a condition remains true.

```javascript
let i = 1;

while (i <= 3) {
    console.log(i);
    i++;
}
```

---

## 7.3 `do...while` Loop

Executes the code at least once before checking the condition.

```javascript
let i = 1;

do {
    console.log(i);
    i++;
} while (i <= 3);
```

---

# 8. JavaScript Arrays

An array is an ordered collection of values.

JavaScript arrays use zero-based indexing:

```javascript
let languages = ["HTML", "CSS", "JavaScript"];

console.log(languages[0]);
```

The first element has index `0`.

---

## 8.1 Creating an Array

### Array Literal

```javascript
let numbers = [10, 20, 30];
```

### Array Constructor

```javascript
let numbers = new Array(10, 20, 30);
```

The array literal approach is generally simpler and easier to read.

---

## 8.2 Accessing Elements

```javascript
let languages = ["HTML", "CSS", "JavaScript"];

console.log(languages[0]);
console.log(languages[1]);
```

### Accessing the Last Element

```javascript
console.log(languages[languages.length - 1]);
```

---

## 8.3 Modifying Elements

```javascript
let languages = ["HTML", "CSS", "JavaScript"];

languages[1] = "Bootstrap";
```

---

## 8.4 Adding Elements

### `push()`

Adds an element to the end.

```javascript
languages.push("Node.js");
```

### `unshift()`

Adds an element to the beginning.

```javascript
languages.unshift("Web Development");
```

---

## 8.5 Removing Elements

### `pop()`

Removes the last element.

```javascript
languages.pop();
```

### `shift()`

Removes the first element.

```javascript
languages.shift();
```

### `splice()`

Can remove or replace elements.

```javascript
let languages = ["HTML", "CSS", "JavaScript"];

languages.splice(1, 1);
```

---

## 8.6 Array Length

```javascript
console.log(languages.length);
```

The `length` property returns the number of elements.

---

## 8.7 Iterating Through Arrays

### Using a `for` Loop

```javascript
for (let i = 0; i < languages.length; i++) {
    console.log(languages[i]);
}
```

### Using `forEach()`

```javascript
languages.forEach(function(item) {
    console.log(item);
});
```

---

## 8.8 Joining Arrays

The `concat()` method combines arrays.

```javascript
let frontend = ["HTML", "CSS"];
let backend = ["Node.js", "Express.js"];

let technologies = frontend.concat(backend);

console.log(technologies);
```

---

## 8.9 Converting an Array to a String

```javascript
console.log(languages.toString());
```

---

## 8.10 Checking if a Value is an Array

```javascript
console.log(Array.isArray(languages));
```

Another method is:

```javascript
console.log(languages instanceof Array);
```

---

## 8.11 Array Constructor Difference

These two statements are not equivalent:

```javascript
const a = [5];

const b = new Array(5);
```

The first creates an array containing the value `5`, while the second creates an array with a length of `5`.

---

# 9. JavaScript Strings

A string is a sequence of characters used to represent text.

JavaScript does not have a separate character type, so a single character is represented as a string.

Strings are immutable.

---

## 9.1 Creating Strings

### Single Quotes

```javascript
let text = 'Hello';
```

### Double Quotes

```javascript
let text = "Hello";
```

---

## 9.2 Template Literals

Template literals use backticks.

```javascript
let name = "Gugu";

let message = `Hello ${name}`;

console.log(message);
```

Template literals allow expressions to be embedded directly into strings.

---

## 9.3 Empty Strings

```javascript
let first = "";
let second = '';
```

---

## 9.4 Multiline Strings

Template literals can contain multiple lines.

```javascript
let message = `
    This is a
    multiline
    string
`;
```

---

## 9.5 String Length

```javascript
let text = "JavaScript";

console.log(text.length);
```

---

## 9.6 String Concatenation

```javascript
let first = "Java";
let second = "Script";

let result = first + second;

console.log(result);
```

---

## 9.7 Escape Characters

Escape characters can be used to represent special characters.

```text
\'  Single quote
\"  Double quote
\\  Backslash
```

Example:

```javascript
const text = "\"Hello\"";
```

---

## 9.8 Finding Substrings

```javascript
let text = "JavaScript Tutorial";

console.log(text.substring(0, 10));
```

---

## 9.9 Changing Letter Case

```javascript
let text = "JavaScript";

console.log(text.toUpperCase());
console.log(text.toLowerCase());
```

---

## 9.10 Searching Strings

The `indexOf()` method returns the position of the first matching substring.

```javascript
let text = "def abc abc";

console.log(text.indexOf("abc"));
```

---

## 9.11 Replacing Text

```javascript
let text = "Learn HTML at GfG and HTML is useful";

let result = text.replace(/HTML/g, "JavaScript");

console.log(result);
```

---

## 9.12 Trimming Whitespace

```javascript
let text = "    Learn JavaScript    ";

console.log(text.trim());
```

---

## 9.13 Accessing Characters

Characters can be accessed using bracket notation.

```javascript
let text = "Learn JavaScript";

console.log(text[6]);
```

The `charAt()` method can also be used:

```javascript
console.log(text.charAt(6));
```

---

## 9.14 String Comparison

JavaScript provides several ways of comparing strings.

```javascript
let s1 = "Ajay";
let s2 = new String("Ajay");

console.log(s1 == s2);
console.log(s1 === s2);
console.log(s1.localeCompare(s2));
```

---

# 10. JavaScript Functions

A function is a reusable block of code designed to perform a specific task.

Functions can:

* Accept inputs.
* Perform operations.
* Return results.
* Be reused multiple times.

---

## 10.1 Parameters and Arguments

```javascript
function greet(name) {
    console.log("Hello " + name);
}

greet("Alice");
```

In this example:

* `name` is the **parameter**.
* `"Alice"` is the **argument**.

---

## 10.2 Default Parameters

Default parameters provide a value when no argument is supplied.

```javascript
function greet(name = "Guest") {
    console.log("Hello " + name);
}

greet();
greet("Aman");
```

---

## 10.3 Return Statement

The `return` statement sends a result back from a function.

```javascript
function add(a, b) {
    return a + b;
}

let result = add(5, 10);

console.log(result);
```

---

# 11. Types of JavaScript Functions

## 11.1 Named Function

A named function has a defined name.

```javascript
function greet() {
    return "Hello!";
}

console.log(greet());
```

---

## 11.2 Anonymous Function

An anonymous function does not have an explicit name.

```javascript
const greet = function() {
    return "Hi there!";
};

console.log(greet());
```

---

## 11.3 Function Expression

A function can be assigned to a variable.

```javascript
const add = function(a, b) {
    return a + b;
};
```

---

## 11.4 Arrow Function

Arrow functions provide a shorter function syntax.

```javascript
const square = n => n * n;

console.log(square(4));
```

---

## 11.5 Immediately Invoked Function Expression

An IIFE executes immediately after it is defined.

```javascript
(function() {
    console.log("This runs immediately!");
})();
```

---

## 11.6 Callback Function

A callback is a function passed as an argument to another function.

```javascript
function calculate(a, b, operation) {
    return operation(a, b);
}

function add(x, y) {
    return x + y;
}

console.log(calculate(2, 3, add));
```

---

## 11.7 Constructor Function

Constructor functions can be used to create multiple objects.

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

const user = new Person("Neha", 22);

console.log(user.name);
```

---

## 11.8 Async Function

An `async` function is used when working with asynchronous operations.

```javascript
async function fetchData() {
    return "Data fetched!";
}

fetchData().then(console.log);
```

---

## 11.9 Generator Function

Generator functions use `*` and `yield`.

```javascript
function* numbers() {
    yield 1;
    yield 2;
    yield 3;
}

const generator = numbers();

console.log(generator.next().value);
console.log(generator.next().value);
```

---

## 11.10 Recursive Function

A recursive function calls itself until a condition stops the recursion.

```javascript
function factorial(n) {
    if (n === 0) {
        return 1;
    }

    return n * factorial(n - 1);
}

console.log(factorial(5));
```

---

## 11.11 Higher-Order Function

A higher-order function accepts another function as an argument or returns a function.

```javascript
function applyOperation(a, b, operation) {
    return operation(a, b);
}

function add(x, y) {
    return x + y;
}

console.log(applyOperation(2, 3, add));
```

---

## 11.12 Nested Functions

A function can be defined inside another function.

```javascript
function outerFun(a) {

    function innerFun(b) {
        return a + b;
    }

    return innerFun;
}

const addTen = outerFun(10);

console.log(addTen(5));
```

The inner function can access values from the surrounding function.

---

## 11.13 Pure Functions

A pure function:

* Produces the same result for the same inputs.
* Does not produce side effects.
* Does not modify state outside its scope.

Example:

```javascript
function pureAdd(a, b) {
    return a + b;
}

console.log(pureAdd(2, 3));
```

---

## 11.14 Rest Parameter Functions

Rest parameters use `...` to collect remaining arguments into an array.

```javascript
function sum(...numbers) {
    return numbers.reduce((a, b) => a + b, 0);
}

console.log(sum(1, 2, 3, 4));
```

---

# 12. Function Overloading in JavaScript

Function overloading allows multiple functions with the same name to behave differently according to their parameters.

JavaScript does not provide traditional function overloading in the same way as languages such as Java or C++.

When multiple functions with the same name are declared, the later definition replaces the earlier one.

Example:

```javascript
function foo(arg1) {
    console.log(arg1);
}

function foo(arg1, arg2) {
    console.log(arg1, arg2);
}

foo("Geeks");
```

The second function definition is used.

The first argument is provided, while the second argument becomes `undefined`.

---

## Simulating Function Overloading

JavaScript can simulate overloading by checking:

* Number of arguments.
* Argument types.
* Arrays.
* Rest parameters.
* The `arguments` object.

Example:

```javascript
function calculate(...args) {

    if (args.length === 1) {
        return args[0];
    }

    if (args.length === 2) {
        return args[0] + args[1];
    }

    return null;
}
```

Therefore, JavaScript does not support true traditional function overloading, but similar behaviour can be implemented manually.

---

# 13. JavaScript Objects

An object is a dynamic data structure that stores related information as key-value pairs.

Example:

```javascript
const person = {
    name: "Sourav",
    age: 23,
    job: "Developer"
};
```

Objects can contain:

* Primitive values.
* Other objects.
* Functions.

When functions are defined inside objects, they are commonly called methods.

---

# 14. Creating Objects

## 14.1 Object Literal

```javascript
const person = {
    name: "Sourav",
    age: 23
};
```

Object literals are the most common way to create objects.

---

## 14.2 Object Constructor

```javascript
const person = new Object();

person.name = "Sourav";
person.age = 23;
```

The object literal syntax is generally simpler and easier to read.

---

# 15. Object Operations

## 15.1 Accessing Properties

### Dot Notation

```javascript
console.log(person.name);
```

### Bracket Notation

```javascript
console.log(person["age"]);
```

---

## 15.2 Modifying Properties

```javascript
person.age = 24;
```

---

## 15.3 Adding Properties

```javascript
person.city = "Pretoria";
```

---

## 15.4 Removing Properties

The `delete` operator removes a property.

```javascript
delete person.city;
```

---

## 15.5 Checking Properties

Using `in`:

```javascript
console.log("name" in person);
```

Using `hasOwnProperty()`:

```javascript
console.log(person.hasOwnProperty("name"));
```

---

## 15.6 Iterating Through an Object

A `for...in` loop can be used.

```javascript
for (let key in person) {
    console.log(key + ": " + person[key]);
}
```

---

## 15.7 Merging Objects

Objects can be merged using the spread operator.

```javascript
const first = {
    name: "Sourav"
};

const second = {
    age: 23
};

const combined = {
    ...first,
    ...second
};
```

---

## 15.8 Object Length

Objects do not have a direct `length` property.

The number of properties can be obtained with:

```javascript
Object.keys(person).length;
```

---

# 16. Objects and Maps

Both objects and maps store key-value data, but they behave differently.

| Feature    | Map                           | Object                      |
| ---------- | ----------------------------- | --------------------------- |
| Keys       | Can be any value              | Usually strings or symbols  |
| Size       | Built-in `size` property      | Usually calculated          |
| Iteration  | `for...of`, `forEach()`       | `for...in`, `Object.keys()` |
| Common use | Dynamic key-value collections | Structured records          |

---

# 17. Object-Oriented Programming in JavaScript

Object-Oriented Programming (OOP) is a programming paradigm based on objects containing data and behaviour.

OOP can help with:

* Code reuse.
* Maintainability.
* Modularity.
* Scalability.
* Representing real-world entities.

---

# 18. Classes

A class is a blueprint used to create objects.

```javascript
class Student {

    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    displayInfo() {
        console.log("Name:", this.name);
        console.log("Age:", this.age);
    }
}
```

Objects can then be created from the class:

```javascript
const student1 = new Student("Rahul", 20);

student1.displayInfo();
```

---

# 19. Constructor

The constructor is a special method in a class that executes when an object is created.

```javascript
class Dog {

    constructor(name) {
        this.name = name;
    }
}
```

`this.name` is an instance property belonging to the individual object.

---

# 20. Class Methods

Methods define the behaviour of objects.

```javascript
class Dog {

    constructor(name, breed) {
        this.name = name;
        this.breed = breed;
    }

    bark() {
        console.log(`${this.name} says woof!`);
    }
}

const dog = new Dog("Rayne", "Husky");

dog.bark();
```

---

# 21. Getters and Setters

Getters retrieve values, while setters modify values.

```javascript
class Dog {

    constructor(name) {
        this.name = name;
    }

    get dogName() {
        return this.name;
    }

    set dogName(newName) {
        this.name = newName;
    }
}
```

Example:

```javascript
const dog = new Dog("Rayne");

console.log(dog.dogName);

dog.dogName = "Buddy";

console.log(dog.dogName);
```

---

# 22. Static Properties

A static property belongs to the class rather than to an individual object.

```javascript
class Dog {

    static sound = "bark";

    constructor(name) {
        this.name = name;
    }
}
```

Access it using:

```javascript
console.log(Dog.sound);
```

---

# 23. Nested Objects

An object can contain another object.

```javascript
const dog = {
    name: "Rayne",
    breed: "Husky",
    details: {
        height: "18 inches",
        weight: "30 pounds"
    }
};
```

Nested properties can be accessed as:

```javascript
console.log(dog.details.height);
```

---

# 24. OOP Principles

## 24.1 Abstraction

Abstraction hides unnecessary implementation details and exposes only the important functionality.

It can be achieved using:

* Classes
* Closures
* Modules
* Private fields

---

## 24.2 Encapsulation

Encapsulation combines data and behaviour while controlling access to internal data.

JavaScript supports private fields using `#`.

---

## 24.3 Inheritance

Inheritance allows one class to acquire functionality from another class.

```javascript
class Animal {

    speak() {
        console.log("Animal sound");
    }
}

class Dog extends Animal {

    speak() {
        console.log("Bark");
    }
}
```

The child class can reuse and extend functionality from the parent class.

---

## 24.4 Polymorphism

Polymorphism means that the same method can behave differently depending on the object.

```javascript
class Animal {

    speak() {
        console.log("Animal sound");
    }
}

class Dog extends Animal {

    speak() {
        console.log("Bark");
    }
}

class Cat extends Animal {

    speak() {
        console.log("Meow");
    }
}
```

The `speak()` method produces different behaviour for different objects.

---

## 24.5 Association

Association represents a relationship between objects that can exist independently.

### Aggregation

A weak association where objects can exist independently.

Example:

> A company has employees.

### Composition

A strong association where one object owns another.

Example:

> A house is composed of rooms.

---

# 25. Asynchronous JavaScript

JavaScript normally executes code one operation at a time, but it provides mechanisms for handling tasks that take time without blocking the entire application.

Examples include:

* API requests.
* File operations.
* Timers.
* User events.
* Database operations.

Common approaches include:

* Callbacks.
* Promises.
* Async/await.

---

# 26. Callbacks

A callback is a function passed as an argument to another function and executed later.

```javascript
function greet(name, callback) {

    console.log("Hello, " + name);

    callback();
}

function sayBye() {
    console.log("Goodbye!");
}

greet("Ajay", sayBye);
```

Callbacks are commonly used with:

* Event listeners.
* API operations.
* File operations.
* Database operations.

---

# 27. Callback Hell

When callbacks become deeply nested, the code can become difficult to read and maintain.

Example:

```javascript
step1(() => {

    step2(() => {

        step3(() => {

            console.log("All steps completed");

        });

    });

});
```

This pattern is often called **callback hell**.

Promises and async/await provide alternatives for structuring asynchronous operations.

---

# 28. Promises

A Promise represents the eventual result of an asynchronous operation.

A Promise has three states:

| State     | Meaning                          |
| --------- | -------------------------------- |
| Pending   | Operation has not completed      |
| Fulfilled | Operation completed successfully |
| Rejected  | Operation failed                 |

Example:

```javascript
const promise = new Promise((resolve, reject) => {

    const number = 4;

    if (number % 2 === 0) {
        resolve("The number is even!");
    } else {
        reject("The number is odd!");
    }
});

promise
    .then(result => console.log(result))
    .catch(error => console.error(error));
```

---

# 29. Promise Methods

## `Promise.all()`

Waits for all promises to complete.

```javascript
Promise.all([
    Promise.resolve("Task 1"),
    Promise.resolve("Task 2")
])
.then(results => console.log(results));
```

If one Promise is rejected, the combined operation rejects.

---

## `Promise.allSettled()`

Waits for all promises to settle, whether fulfilled or rejected.

```javascript
Promise.allSettled([
    Promise.resolve("Task 1"),
    Promise.reject("Task 2 failed")
])
.then(results => console.log(results));
```

---

## `Promise.race()`

Returns the result of the first Promise that settles.

```javascript
Promise.race([
    Promise.resolve("First"),
    Promise.resolve("Second")
])
.then(result => console.log(result));
```

---

## `Promise.any()`

Returns the first fulfilled Promise.

If all promises reject, it rejects with an `AggregateError`.

---

## `Promise.resolve()`

Creates a fulfilled Promise.

```javascript
Promise.resolve("Success")
    .then(value => console.log(value));
```

---

## `Promise.reject()`

Creates a rejected Promise.

```javascript
Promise.reject("Failure")
    .catch(error => console.error(error));
```

---

## `finally()`

Runs after a Promise settles regardless of whether it succeeded or failed.

```javascript
Promise.resolve("Complete")
    .finally(() => console.log("Cleanup completed"));
```

---

# 30. Promise Chaining

Promises can be chained to execute asynchronous tasks in sequence.

```javascript
Promise.resolve(5)
    .then(value => value * 2)
    .then(value => value + 3)
    .then(finalValue => console.log(finalValue));
```

Each `.then()` can pass its result to the next step.

---

# 31. Timeout Handling with Promises

`Promise.race()` can be used to implement a timeout.

```javascript
const fetchData = new Promise(resolve => {
    setTimeout(() => resolve("Data loaded"), 3000);
});

const timeout = new Promise((_, reject) => {
    setTimeout(() => reject("Timeout!"), 2000);
});

Promise.race([fetchData, timeout])
    .then(result => console.log(result))
    .catch(error => console.error(error));
```

---

# 32. Wrapping Callbacks in Promises

A callback-based operation can be converted into a Promise-based operation.

```javascript
function loadData(callback) {
    setTimeout(() => {
        callback("Data loaded");
    }, 1000);
}

function promisifiedLoadData() {
    return new Promise(resolve => {
        loadData(result => resolve(result));
    });
}

promisifiedLoadData()
    .then(data => console.log(data));
```

---

# 33. Event Loop in JavaScript

The event loop is an important mechanism for handling asynchronous operations.

JavaScript uses a **call stack**, queues, and the event loop to manage execution.

The main components include:

### Call Stack

Manages currently executing functions using a Last-In, First-Out (LIFO) structure.

### Web APIs / Background Operations

Handle operations such as:

* `setTimeout`
* `setInterval`
* `fetch`
* DOM events

### Task Queue

Stores callbacks waiting to be executed.

### Microtask Queue

Stores microtasks such as Promise callbacks.

### Event Loop

Continuously checks whether the call stack is available and moves queued work for execution.

---

# 34. Microtasks and Task Queue

Consider:

```javascript
console.log("Start");

setTimeout(() => {
    console.log("setTimeout Callback");
}, 0);

Promise.resolve().then(() => {
    console.log("Promise Resolved");
});

console.log("End");
```

The synchronous statements execute first.

The Promise callback is placed in the microtask queue, while the timer callback is placed in the task queue.

The microtask is processed before the timer callback.

---

# 35. Problems with the Event Loop

## 35.1 Blocking the Main Thread

Long-running or infinite synchronous work can prevent other tasks from executing.

```javascript
while (true) {
    console.log("Blocking...");
}
```

This can make an application unresponsive.

---

## 35.2 Delayed `setTimeout()`

A timer does not guarantee that its callback will execute exactly after the specified delay.

A busy call stack can delay its execution.

---

## 35.3 Excessive Microtasks

Because microtasks are processed before the next task, excessive microtask processing can delay other queued operations.

---

# 36. Async and Await

`async` and `await` provide a cleaner syntax for working with Promises.

## `async`

An `async` function always returns a Promise.

```javascript
async function getData() {
    return "Hello World";
}
```

---

## `await`

The `await` keyword waits for a Promise to settle inside an async function.

```javascript
async function getData() {

    const result = await Promise.resolve("Hello World");

    console.log(result);
}

getData();
```

---

# 37. Error Handling with Async/Await

`try...catch` can be used to handle errors in async code.

```javascript
async function fetchData() {

    try {

        const response = await fetch(
            "https://api.example.com/data"
        );

        const data = await response.json();

        console.log(data);

    } catch (error) {

        console.error(
            "Error fetching data:",
            error.message
        );
    }
}
```

This structure makes asynchronous code easier to read while providing centralized error handling.

---

# 38. JavaScript Events

Events are actions that occur in the browser.

Examples include:

* Mouse clicks.
* Keyboard input.
* Mouse movement.
* Form submission.
* Focus changes.

JavaScript can respond to these events.

Example:

```html
<button id="button">Click Me</button>

<script>
    const button = document.getElementById("button");

    button.onclick = function() {
        alert("You clicked the button");
    };
</script>
```

---

# 39. DOM Manipulation

The Document Object Model (DOM) represents the structure of an HTML document.

JavaScript can manipulate the DOM to change:

* Content.
* Attributes.
* Classes.
* Styles.
* Elements.

Example:

```javascript
document.body.classList.toggle("background");
```

DOM manipulation allows webpages to change dynamically without requiring the entire page to reload.

---

# 40. JavaScript Error Handling

Error handling allows applications to detect and manage problems during execution.

The main tools include:

* `try`
* `catch`
* `throw`
* `finally`

---

# 41. Types of JavaScript Errors

## Syntax Error

Occurs when code does not follow JavaScript syntax rules.

```javascript
console.log("Hello World"
```

The closing parenthesis is missing.

---

## Reference Error

Occurs when an undeclared variable is accessed.

```javascript
console.log(x);
```

---

## Type Error

Occurs when an operation is performed on an inappropriate type.

```javascript
let number = 5;

number.toUpperCase();
```

---

## Range Error

Occurs when a value is outside an acceptable range.

```javascript
let array = Array(-1);
```

---

## Custom Errors

Developers can create their own errors.

```javascript
throw new Error("Custom error occurred");
```

---

# 42. `try...catch`

The `try...catch` statement allows errors to be caught and handled.

```javascript
try {

    const result = 10 / 0;

    if (!isFinite(result)) {
        throw new Error("Cannot divide by zero");
    }

    console.log(result);

} catch (error) {

    console.error(
        "Error occurred:",
        error.message
    );
}
```

---

# 43. `finally`

The `finally` block runs regardless of whether an error occurs.

```javascript
try {

    console.log("Trying...");

    throw new Error("An error occurred");

} catch (error) {

    console.error(error.message);

} finally {

    console.log("Cleaning up...");
}
```

This is useful for cleanup operations.

---

# 44. Throwing Custom Errors

Custom errors can provide meaningful messages for application-specific situations.

```javascript
function check(age) {

    if (age < 18) {
        throw new Error("Age must be 18 or above");
    }

    console.log("Access granted");
}

try {

    check(16);

} catch (error) {

    console.error(error.message);
}
```

---

# 45. Error Handling with Async/Await

Errors occurring during asynchronous operations can also be handled with `try...catch`.

```javascript
async function fetchData() {

    try {

        const response =
            await fetch("https://api.example.com/data");

        const data = await response.json();

        console.log(data);

    } catch (error) {

        console.error(
            "Error fetching data:",
            error.message
        );
    }
}

fetchData();
```

---

# 46. Best Practices for JavaScript Error Handling

Good error handling should:

* Handle errors appropriately.
* Provide useful error messages.
* Avoid silently ignoring failures.
* Use `finally` when cleanup is required.
* Use custom errors when application-specific situations need clearer messages.
* Record useful information for debugging.

---

# 47. Relationship Between JavaScript Concepts

The concepts researched are connected and build on each other.

A simplified relationship is:

```text
Variables
    ↓
Data Types
    ↓
Operators
    ↓
Control Flow
    ↓
Functions
    ↓
Arrays and Objects
    ↓
Classes and OOP
    ↓
Events and DOM Manipulation
    ↓
Callbacks
    ↓
Promises
    ↓
Async/Await
    ↓
Event Loop
    ↓
Error Handling
```

Understanding the earlier concepts makes the more advanced JavaScript concepts easier to understand.

---

# 48. Key Findings

The research demonstrated that JavaScript provides functionality for both basic programming and advanced web application development.

The main findings are:

* Variables store and manage values.
* JavaScript supports primitive and non-primitive data types.
* Operators perform calculations, assignments, comparisons, and logical operations.
* Control flow allows programs to make decisions and repeat operations.
* Arrays manage ordered collections of values.
* Strings provide functionality for working with text.
* Functions allow code to be reused and organized.
* Objects group related properties and behaviour.
* Classes provide a structured approach to object-oriented programming.
* OOP concepts include abstraction, encapsulation, inheritance, and polymorphism.
* Callbacks can be used for asynchronous operations and event handling.
* Promises provide a structured way to manage asynchronous results.
* Async/await provides a cleaner syntax for Promise-based programming.
* The event loop manages asynchronous execution.
* DOM manipulation allows JavaScript to update webpages dynamically.
* Error handling helps applications recover from unexpected problems.

---

# 49. What I Learned

Through this research, I developed a broader understanding of JavaScript as both a programming language and a web-development technology.

I learned how variables and data types are used to store information, how operators and control-flow statements determine program behaviour, and how arrays, strings, objects, and functions are used to structure and manipulate data.

I also learned more advanced concepts such as object-oriented programming, classes, inheritance, encapsulation, abstraction, and polymorphism. These concepts showed how JavaScript can be used to organize larger applications.

Another important area was asynchronous JavaScript. I learned about callbacks, callback hell, Promises, Promise methods, async/await, and the event loop. These concepts explain how JavaScript can handle operations such as API requests and timers while continuing to process other work.

Finally, I learned about error and exception handling using `try...catch`, `throw`, and `finally`, as well as how errors can be handled when using asynchronous functions.

---

# 50. Conclusion

JavaScript is a flexible language that provides both fundamental programming features and advanced functionality for modern web applications. It can manipulate HTML and CSS, respond to browser events, communicate with external services, and run on the server.

The research covered the core building blocks of JavaScript as well as more advanced topics including functions, objects, OOP, asynchronous programming, Promises, the event loop, async/await, DOM manipulation, and error handling.

These concepts provide a strong foundation for continuing into more advanced JavaScript development and technologies such as Node.js, APIs, and frontend frameworks.
