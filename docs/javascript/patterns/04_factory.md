## JavaScript - Factory Design Pattern

#### I. [What is Factory Pattern?](#chapter1)

#### II. [Code Example ](#chapter2)

- [2.1 Creator](#ch2-1)
- [2.2 Specific Sub-Classes](#ch2-2)
- [2.3 Usage & Test](#ch2-3)

#### III. [Built-in Object Factory in JavaScript](#chapter3)

#### IV. [Reference and Useful Links](#chapter4)

<div id="chapter1" />

### I. What is Factory Pattern?

Category: creational pattern

Why use Factory Pattern?

- when setting up similar objects
- create object without knowing the specific type/class at compile time

Features:

- The objects created by the factory method (or class) are by design inheriting from the same parent object;
- they are specific subclasses implementing specialized functionality
- a common parent constructor: eg: `FactoryClass`
- a static method called: `create()`
- specialized or internal constructors, eg: `AClass`, `BClass`, `CClass`....

<div id="chapter2" />

### II. Code Example

<div id="ch2-1" />

#### 2.1 Creator

Here the creator is the "EmployeeFactory" class, which has a `create()` method.

```js
function EmployeeFactory() {
  this.create = function (name, type) {
    var employee;
    switch (type) {
      case "fulltime":
        employee = new FullTime(name);
        break;
      case "parttime":
        employee = new PartTime(name);
        break;
      case "contractor":
        employee = new Contractor(name);
        break;
      default:
        throw "invalid employee type";
    }
    employee.say = function () {
      console.log(this.name + ": I am a " + this.type + "empolyee");
    };
    return employee;
  };
}
```

<div id="ch2-2" />

#### 2.2 Specific Sub-Classes

```js
var FullTime = function (name) {
  this.name = name;
};
var PartTime = function (name) {
  this.name = name;
};
var Contractor = function (name) {
  this.name = name;
};
```

<div id="ch2-3" />

#### 2.3 Usage & Test

```js
var factory = new EmployeeFactory();
var employees = [];
employees.push(factory.create("alice", "fulltime"));
employees.push(factory.create("bob", "parttime"));
employees.forEach((el) => el.say());
```

Output:

```text
alice: I am a undefinedempolyee
bob: I am a undefinedempolyee
```

<div id="chapter3" />

### III. Built-in Object Factory in JavaScript

The fact that _Object()_ is also a factory is of little practical use, just something worth mentioning as an example that the factory pattern is all around us.

The built-in global [_Object()_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object) constructor. It also behaves as a factory, because it creates different objects, depending on the input.

Code Example:

```js
var o = new Object(),
  n = new Object(1),
  s = Object("1"),
  b = Object(true);

// test
console.log(o.constructor === Object); // true
console.log(n.constructor === Number); // true
console.log(s.constructor === String); // true
console.log(b.constructor === Boolean); // true
```

<div id="chapter4" />

### IV. Reference and Useful Links

- youtube: https://www.youtube.com/watch?v=kuirGzhGhyw&t=389s
- code example: https://www.dofactory.com/javascript/design-patterns/factory-method
- Book: https://learning.oreilly.com/library/view/javascript-patterns/9781449399115/ch07.html
