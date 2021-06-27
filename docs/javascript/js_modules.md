## JavaScript Modules - CommonJS vs AMD vs RequireJS vs ES6 Modules

#### I. [ES5 old modular pattern](#question1)

#### II. [ CommonJS](#question2)

#### III. [Asynchronous Module Definition (AMD)](#question3)

#### IV. [RequireJS](#question4)

#### V. [ES6 Modules](#question5)

#### VI. [Reference Links](#question6)

<div id="question1" />

### I. ES5 old modular pattern

use IIFE to have data privacy in side the modular block.
**Code Example:**

```js
var Module = (function () {
  var _privateMethod1 = function () {
    console.log("private 1");
  };
  var _privateMethod2 = function () {
    console.log("private 2");
  };
  return {
    publicMethod1: function () {
      console.log("public 1");
      _privateMethod1();
    },
    publicMethod2: function () {
      console.log("public 2");
    }
  };
})();
// Usage
Module.publicMethod1();
Module.publicMethod2();
```

**Reference docs:** [ES5 Module pattern](https://gist.github.com/0bie/aa52302f5e810c02f404ae73fc443c11)

<div id="question2" />

### II. CommonJS

CommonJS is used on **server side**. It interacts with the module system using the keywords _require_ and _exports_.

**Code Example:**

```js
//------ payments.js ------
var customerStore = require('store/customer'); // import module//------ store/customer.js ------
exports = function(){
    return customers.get('store);
}
```

These modules are designer for server development and these are synchronous.ie., the files are loaded one by one in order inside the file.

**NodeJS implementation**

They are heavily influenced by **CommonJS** specification. The major difference arises in the exports object. NodeJS modules use _module.exports_ as the object to get exported while CommonJS uses just the _exports_ variable.

Code Example:

```js
//payments.js
var customerStore = require('store/customer'); // import module//store/customer.js
function customerStore(){
    return customers.get('store);
}
modules.exports = customerStore;
```

<div id="question3" />

### III. Asynchronous Module Definition (AMD)

AMD was born as CommonJS wasn’t suited for the browsers early on. it supports **asynchronous** module loading.

**Docs:** [AMD - github](https://github.com/amdjs/amdjs-api/wiki/AMD)

<div id="question4" />

### IV. RequireJS

**[RequireJS](https://requirejs.org/)** implements the AMD API. It loads the plain JavaScript files as well as modules by using plain script tags. It includes an **"optimizing tool"** which can be run while deploying our code for **better performance.**

```html
<script data-main="scripts/main" src="scripts/require.js"></script>
```

This is the only code required to include files in RequireJS. The _data-main_ attribute defines the initialization and it looks for scripts and dependencies.

<div id="question5" />

### V. ES6 Modules

ECMAScript 6 a.k.a., ES6 a.ka., ES2015 offers possibilities for importing and exporting modules compatible with **both synchronous and asynchronous** modes of operation.

Code Example:

```js
export const Foo = () => {};
export const a = 1;
export default Boo = () => {};
```

**Docs:** [es6-modules-in-depth](https://hacks.mozilla.org/2015/08/es6-in-depth-modules/)

<div id="question6" />

### VI. Reference Links

- JS modules: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules
- commonJS & AMD & vs RequireJS vs ES6_Modules: https://medium.com/computed-comparisons/commonjs-vs-amd-vs-requirejs-vs-es6-modules-2e814b114a0b
