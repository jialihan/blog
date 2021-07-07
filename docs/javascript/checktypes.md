## BFE 20. Detect data type in JavaScript

#### I. [Problem - bfe 20.](#question1)

#### II. [ Solution 1: `Object.prototype.toString()` on object](#question2)

#### III. [ Solution 2: use "constructor.name" property ](#question3)

<div id="question1" />

### I. [Problem - bfe 20.](https://bigfrontend.dev/problem/detect-data-type-in-JavaScript)

This is an easy problem.

For [all the basic data types](https://javascript.info/types) in JavaScript, how could you write a function to detect the type of arbitrary data?

Besides basic types, you need to also handle also commonly used complex data type including `Array`, `ArrayBuffer`, `Map`, `Set`, `Date` and `Function`

The goal is not to list up all the data types but to show us how to solve the problem when we need to.

The type should be lowercase

```js
detectType(1); // 'number'
detectType(new Map()); // 'map'
detectType([]); // 'array'
detectType(null); // 'null'

// more in judging step
```

<div id="question2" />

### II. Solution 1: `Object.prototype.toString()` on object

**Docs:** [Using toString() to detect object class](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString#using_tostring_to_detect_object_class "Permalink to Using toString() to detect object class
")

`toString()` can be used with every object and (by default) allows you to get its class.

To use the `Object.prototype.toString()` with every object, you need to call [`Function.prototype.call()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) or [`Function.prototype.apply()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) on it, passing the object you want to inspect as the first parameter (called `thisArg`).

Code Example:

```js
const toString = Object.prototype.toString;

toString.call(new Date()); // [object Date]
toString.call(new String()); // [object String]
toString.call(Math); // [object Math]

// Since JavaScript 1.8.5
toString.call(undefined); // [object Undefined]
toString.call(null); // [object Null]
```

**Solution:**

```js
/**
 * @param {any} data
 * @return {string}
 */
function detectType(data) {
  if (data instanceof FileReader) {
    return "object";
  }
  if (typeof data === "object") {
    return Object.prototype.toString
      .call(data)
      .slice(1, -1)
      .split(" ")[1]
      .toLowerCase();
  }
  return typeof data;
}
```

<div id="question3" />

### III. Solution 2: use "constructor.name" property to get the class name

**Reference:**

- https://stackoverflow.com/questions/3905144/how-to-retrieve-the-constructors-name-in-javascript
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor

#### Note

- `null` is a type of "object"
- `FileReader` is a type of "filereader", but in this problem, it need to show in "object"
- need to call `toLowerCase()` for examle: `String`, `Number`.....

#### Code

```javascript
/**
 * @param {any} data
 * @return {string}
 */
function detectType(data) {
  if (typeof data === "object") {
    if (data === null) {
      return "null";
    }
    if (data instanceof FileReader) {
      return "object";
    }
    return data.constructor.name.toLowerCase();
  }
  return typeof data;
}
```
