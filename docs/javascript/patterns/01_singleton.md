## JavaScript - Singleton Design Pattern

#### [I. ES5 - Singleton implementation](#chapter1)

- [Bad: 1.1 use static variable](#ch1-2)
- [Good: 1.2 use closure with "new" keyword](#ch1-2)
- [Good: 1.3 use IIFE with "getInstance()" method](#ch1-3)

#### [II. ES6 - Singleton implementation ](#chapter2)

<div id="chapter1" />

### I. ES5 - Singleton implementation

<div id="ch1-1" />

#### 1.1 Bad: use static variable

```js
function Singleton() {
  // check if existed an instance
  if (Singleton.instance) {
    return Singleton.instance;
  }

  // normal construct process
  this.name = "jel";
  this.age = 18;

  // cache
  Singleton.instance = this;

  // implicit return
  // return this; // NOT needed
}
Singleton.prototype.getName = function () {
  return this.name;
};
```

**Tests:**

```js
var uni = new Singleton();
var uni2 = new Singleton();
console.log(uni === uni2); // true
console.log(uni.getName()); // jel
```

**Why it's a bad idea?**
Because we outside anyone can **change & access** the static variable _"instance"_, for example:

```js
Singleton.instance = { x: 1, y: 1 };
uni = new Singleton();
uni2 = new Singleton();
console.log(uni === uni2); // true
console.log(uni); // {x: 1, y: 1}
console.log(uni2); // {x: 1, y: 1}
```

<div id="ch1-2" />

#### 1.2 Good: 1.2 use closure with "new" keyword

**Good:**

- use "new" keyword to create the same instance
- NO outside ACCESS & changes on the instance object

```js
function Singleton2() {
  // cached instance
  var instance = this;

  // normal construct process
  this.name = "jel";
  this.age = 18;

  // rewrite constructor
  Singleton2 = function () {
    return instance;
  };
}
Singleton2.prototype.getName = function () {
  return this.name;
};
```

**Tests:**

```js
var uni = new Singleton2();
var uni2 = new Singleton2();
console.log(uni === uni2); // true
console.log(uni.getName()); // jel
console.log(uni); // Singleton2 {name: "jel", age: 18}
console.log(uni2); // Singleton2 {name: "jel", age: 18}
console.log(Singleton2.instance); // undefined: can NOT ACCESS
```

<div id="ch1-3" />

### 1.3 Good: 1.3 use IIFE with "getInstance()" method

**Good:**

- **IIFE** has data privacy, canNOT access the instance object directly
- use a closure and return a wrapper with the `"getInstance()"` method
- canNOT use "new" keyword to get the instance

```js
var Singleton3 = (function () {
  // internal class
  function SingletonClass() {
    this.name = "jel";
    this.age = 18;
  }
  SingletonClass.prototype.getName = function () {
    return this.name;
  };
  var instance;
  return {
    // closure
    getInstance: function () {
      if (instance == null) {
        instance = new SingletonClass();
        // Hide the constructor so the returned object can't be new'd...
        instance.constructor = null;
      }
      return instance;
    }
  };
})();
```

**Tests:**

```js
var uni = Singleton3.getInstance();
var uni2 = Singleton3.getInstance();
console.log(uni === uni2); // true
console.log(uni.getName()); // jel
console.log(uni); // SingletonClass {name: "jel", age: 18, constructor: null}
console.log(uni2); // SingletonClass {name: "jel", age: 18, constructor: null}
var uni3 = new Singleton3(); // ERROR: Uncaught TypeError: Singleton3 is not a constructor
```

<div id="chapter2" />

### II. ES6 - Singleton implementation

```js
class Singleton4 {
  static instance = null;
  constructor() {
    if (Singleton4.instance) {
      return Singleton4.instance;
    }
    // normal constructor process
    this.name = "jel";
    this.age = 18;
    Singleton4.instance = this;
  }
  getName() {
    return this.name;
  }
}
```

Tests:

```js
var uni = new Singleton4();
var uni2 = new Singleton4();
console.log(uni === uni2); // true
console.log(uni.getName()); // jel
console.log(uni); // SingletonClass {name: "jel", age: 18}
console.log(uni2); // SingletonClass {name: "jel", age: 18}
```
