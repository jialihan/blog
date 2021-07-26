## JavaScript - Decorator Design Pattern

#### I. [What is Decorator Pattern?](#chapter1)

#### II. [Code Example ](#chapter2)

- [2.1 Creator](#ch2-1)
- [2.2 Specific Sub-Classes](#ch2-2)
- [2.3 Usage & Test](#ch2-3)

#### III. [Built-in Object Factory in JavaScript](#chapter3)

#### IV. [Decorator in JavaScript - Experimental](#chapter4)

- [4.1 What is a decorator?](#ch4-1)
- [4.2 Property Decorator](#ch4-2)
- [4.3 Class Decorator](#ch4-3)

#### V. [Decorator in TypeScript - more popular](#chapter5)

#### VI. [Reference and Useful Links](#chapter6)

<div id="chapter1" />

### I. What is Decorator Pattern?

**Category**: structural pattern

Why use Factory Pattern?

- they can be considered another viable alternative to object subclassing.
- Decorators offered the ability to add behavior to existing classes in a system dynamically

**Features:**

- The Decorator pattern isn’t heavily tied to how objects are created , but instead **focuses on the problem of extending their functionality**.
- Rather than just relying on prototypal inheritance, we work with a single base object and **progressively add decorator objects** that provide the additional capabilities.
- rather than subclassing, we **add (decorate) properties or methods to a base object** so it’s a little more streamlined.

<div id="chapter2" />

### II. Code Example - decorate methods to object

<div id="ch2-1" />

#### 2.1 target class object

```js
// The constructor to decorate
function MacBook() {
  this.cost = function () {
    return 997;
  };
  this.screenSize = function () {
    return 11.6;
  };
}
```

<div id="ch2-2" />

#### 2.2 Multiple Decorators

```js
// Decorator 1
function  Memory(macbook) {
	var  v = macbook.cost();
	macbook.cost = function () {
		return  v + 75;
	};
}

// Decorator 2
function  Engraving(macbook) {
	var  v = macbook.cost();
	macbook.cost = function () {
		return  v + 200;
	};
}

// Decorator 3
function  Insurance(macbook) {=
	var  v = macbook.cost();
	macbook.cost = function () {
		return  v + 250;
	};
}
```

<div id="ch2-3" />

#### 2.3 Usage & Test

```js
var mb = new MacBook();
// decorate this object
Memory(mb);
Engraving(mb);
Insurance(mb);
console.log(mb.cost()); // Outputs: 1522
console.log(mb.screenSize()); // Outputs: 11.6
```

<div id="chapter3" />

### III. Implement "decorate()" method using a List

In this implementation, decorate() doesn’t do anything to the object, it simply appends to a list.
Usage Example:

```js
var sale = new Sale(100); // the price is 100 dollars
sale.decorate("fedtax"); // add federal tax
sale.decorate("quebec"); // add provincial tax
sale.decorate("money"); // format like money
sale.getPrice(); // "$112.88"
```

#### 3.1 target class

The `Sale()` constructor now has a list of decorators as an own property:

```js
function Sale(price) {
  this.price = price || 100;
  this.decorators_list = [];
}
```

#### 3.2 create decorator properties

The available decorators are once again implemented as properties of `Sale.decorators`.

```js
Sale.decorators = {};
Sale.decorators.fedtax = {
  getPrice: function (price) {
    return price + (price * 5) / 100;
  }
};
Sale.decorators.quebec = {
  getPrice: function (price) {
    return price + (price * 7.5) / 100;
  }
};
Sale.decorators.money = {
  getPrice: function (price) {
    return "$" + price.toFixed(2);
  }
};
```

#### 3.3 "decorate()" method

just append the decorator property name/key into our List:

```js
Sale.prototype.decorate = function (decorator) {
  this.decorators_list.push(decorator);
};
```

#### 3.4 Usage: when use these decorators

```js
Sale.prototype.getPrice = function () {
  var price = this.price,
    i,
    max = this.decorators_list.length,
    name;
  for (i = 0; i < max; i += 1) {
    name = this.decorators_list[i];
    price = Sale.decorators[name].getPrice(price);
  }
  return price;
};
```

Weakness:

- **repeated code** needed in each method that you want to decorate

Improve:

- abstracted into a helper method that takes a method and makes it “decoratable.” In such an implementation the decorators_list property would become an object with properties named after the methods and values being arrays of decorator objects.

<div id="chapter4" />

### IV. Decorator in JavaScript - Experimental

Is Decorator supported in JavaScript? Can we use it?

- Decorators aren’t a core feature of JavaScript yet;
- ES2016 decorators are not yet part of the spec.
- they’re working their way through [ECMA TC39’s standardization process](https://github.com/tc39/proposals).
- reference: https://stackoverflow.com/questions/34461548/how-can-i-use-decorators-today

<div id="ch4-1" />

#### 4.1 What is a Decorator?

Decorator is shorthand for “decorator function” (or method). It’s a function that modifies the behavior of the function or method passed to it by returning a new function.
Two types of decorator in JS:

- property decorator
  ```js
  @readonly
  getName() {
      return this.firstName + ' ' + this.lastName;
  }
  ```
- class decorator
  ```js
  @log("Demo")
  class Example {
    constructor(name, age) {}
  }
  ```

<div id="ch4-2" />

#### 4.2 Property Decorator

JS decorator functions are passed 3 arguments:

- `target`: The `prototype` of the class that the decorated method belongs to (`Foo.prototype`)
- `name`: The name of the decorated method in the class (`bar`)
- `descriptor`: A descriptor object, which is the same descriptor object that would be passed to [`Object.defineProperty`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
  ```js
  // descriptor object example
  {
    configurable: true,
    enumerable: true,
    value: 20,
    writable: true
  }
  ```

**Syntax:**

```javascript
function readOnly(target, key, descriptor) {
  return {
    ...descriptor,
    writable: false
  };
}
```

**Usage Example:**

```js
class Person {
  @readOnly age = 20;
  // (you can also put @readOnly on the line above the property)
  constructor(name) {
    this.name = name;
  }
}
```

<div id="ch4-3" />

#### 4.3 class decorator

The decorator function is called with a **single parameter** which is the **"constructor function"** being decorated.

```js
// class Decorator
function hero(target) {
  target.isHero = true;
  target.heroName = "superman";
}

@hero
class Person {
  constructor(name) {
    this.name = name;
  }
}
console.log(Person.isHero); // true
```

<div id="chapter5" />

### V. Decorator in TypeScript - more popular

**Docs:**

- Official TS doc: https://www.typescriptlang.org/docs/handbook/decorators.html
- my article - decorator in TS: https://jialihan.github.io/blog/#/typescript/day8_decorator

<div id="chapter6" />

### VI. Reference and Useful Links

- https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841
- https://davidtang.io/2019-10-12-javascript-decorators-on-methods-in-es6-classes/
- https://www.sitepoint.com/javascript-decorators-what-they-are/
- https://www.simplethread.com/understanding-js-decorators/
