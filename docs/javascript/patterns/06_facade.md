## JavaScript - Facade Design Pattern

#### I. [What is Decorator Pattern?](#chapter1)

#### II. [Code Example 1](#chapter2)

#### III. [Code Example 2](#chapter3)

#### IV. [Reference and Useful Links](#chapter6)

<div id="chapter1" />

### I. What is Decorator Pattern?

The façade is a simple pattern; it provides only **an alternative interface** to an object.

The façade is a simple pattern; it provides only an alternative interface to an object.

<div id="chapter2" />

### II. Code Example 1

As we can see below, our instance of the Module pattern contains a number of methods that have been privately defined. A Facade is then used to supply a much simpler API to accessing these methods:

![image](../assets/facade-eg_1_code.png)

**Usage & Output:**

![image](../assets/usage_output.png)

<div id="chapter3" />

### III. Code Example 2

The intent of the Façade is to provide a high-level interface (properties and methods) that makes a subsystem or toolkit easy to use for the client.

#### 3.1 Diagram

![image](../assets/diagram-facade.png)

- **Façade** -- In example code: **Mortgage**
  - knows which subsystems are responsible for a request
  - delegates client requests to appropriate subsystem objects
- **Sub Systems** -- In example code: **Bank, Credit, Background**
  - implements and performs specialized subsystem functionality
  - have no knowledge of or reference to the façade

#### 3.2 Code Example

```js
// facade class
var Mortgage = function (name) {
  this.name = name;
};
Mortgage.prototype = {
  applyFor: function (amount) {
    // access multiple subsystems...
    var result = "approved";
    if (!new Bank().verify(this.name, amount)) {
      result = "denied";
    } else if (!new Credit().get(this.name)) {
      result = "denied";
    } else if (!new Background().check(this.name)) {
      result = "denied";
    }
    return this.name + " has been " + result + " for a " + amount + " mortgage";
  }
};
// sub systems
var Bank = function () {
  this.verify = function (name, amount) {
    // complex logic ...
    return true;
  };
};
var Credit = function () {
  this.get = function (name) {
    // complex logic ...
    return true;
  };
};
var Background = function () {
  this.check = function (name) {
    // complex logic ...
    return true;
  };
};
// Usage
function run() {
  var mortgage = new Mortgage("Joan Templeton");
  var result = mortgage.applyFor("$100,000");
  console.log(result); // Joan Templeton has been approved for a $100,000 mortgage
}
```

<div id="chapter4" />

### IV. Reference and Useful Links

- usage_output.pngusage_output.pngusage_output.png
- https://learning.oreilly.com/library/view/learning-javascript-design/9781449334840/ch09s09.html
- https://learning.oreilly.com/library/view/javascript-patterns/9781449399115/ch07.html
