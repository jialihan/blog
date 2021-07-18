## Understand JSON in JavaScript

#### I. [What is JSON?](#question1)

#### II. [JSON Syntax & Values](#question2)

#### III. [Using JSON Securely](#question3)

#### IV. [Use Special Characters in JSON](#question4)

#### V. [Implement JSON.stringify() method](#question5)

- [5.1 Rules](#q5-1)
- [5.2 Code Implementation](#q5-2)
- [5.3 Test Examples](#q5-3)

#### VI. [Implement JSON.parse() method](#question6)

#### VII. [References and Useful Links](#question7)

<div id="question1" />

### I. What is JSON?

- JavaScript Object Notation (JSON) is a lightweight data interchange format.

- It is based on JavaScript's object literal notation, one of JavaScript's best parts.

- Even though it is a subset of JavaScript, it is language independent.

- It can be used to exchange data between programs written in all modern programming languages.

<div id="question2" />

### II. JSON Syntax & Values

JSON supports the following 6 values:

- string
- number
- boolean: _true/false_
- array
- object literal: `{}`
- "null"

> Note:
>
> 1. "BigInt" is not supported in JSON
> 2. inside of `{}` object literal, it's **un-ordered** K-V pairs

Example 1: JSON array

```
[
	{
		name: 'jel',
		age: '54'
	},
	1.0,
	true,
	"hello",
	[1,2,null, 3]
]
```

Example 2: JSON object literal

```
{
	{
		id: 1,
		title: 'hello'
	},
	{
		id: 2,
		title: 'world'
	}
}
```

<div id="question3" />

### III. Use JSON Securely

DO **NOT** USE `eval(....)` to parse json string!

```
// WRONG
var myData = eval('(' + myJSONText + ')');
```

**Safe method:** `JSON.parse(...)`

`JSON.parse` will throw an exception if the text contains anything dangerous.

<div id="question4" />

### IV. Use Special Characters in JSON

The following characters are **reserved characters** and can **NOT** be used in JSON and must be properly escaped to be used in strings.

- **Backspace** to be replaced with `\b`
- **Form feed** to be replaced with `\f`
- **Newline** to be replaced with `\n`
- **Carriage return** to be replaced with `\r`
- **Tab** to be replaced with `\t`
- **Double quote** to be replaced with `\"`
- **Backslash** to be replaced with `\\`

Another **good practice**:
A JSON string must be **double-quoted**, according to the [specs](http://json.org/), so you don't need to escape `'`.

<div id="question5" />

### V. Implement JSON.stringify() method

<div id="q5-1" />

#### 5.1 Rules

**For single value:**

- **'bigint'** -> Special rule: **throw new Error()**
- string -> return itself
- number: many cases
  - Infinity -> return `"null"`
  - NaN -> return `"null"`
  - **Others** -> return `"" + number`
- boolean -> return `""+boolean_value`
- "function" -> return `undefined` **(special rule)**
- "symbol" -> return `"null"`
- undefined -> return `"null"`

**For Objects:**

- **null** : special object, just return `"null"`
- **Date**: use `toISOString()` method to return the date string
  HOW JSon stringify parse Date() to string?
  - https://stackoverflow.com/questions/11491938/issues-with-date-when-using-json-stringify-and-json-parse
  - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString
- **Array**: recursive to call `stringify() single value` on each element in array
  ```js
  if (val instanceof Array) {
    return "[" + val.map((el) => stringify(el)).join(",") + "]";
  }
  ```
- **Other objects** with `{}`
  - recursively construct with each **"key, value"** pair
  - **Special Rules:**
    - if key is undefined, skip it, eg: `{ [Symbol()]: 'a' }`
    - if value if undefined, skip it. eg: `{a: undefined}`
    - if value if function, skip it, eg: `{a: ()=>{}}`
    - Delete last comma in string before wrapped into the `{}`, if needed

<div id="q5-2" />

#### 5.2 Code Implementation

```js
/**
 * @param {any} data
 * @return {string}
 */
function stringify(data) {
  if (typeof data === "bigint") {
    throw new Error("JSON.stringify canNOT serialize a BigInt!");
  }
  if (
    typeof data !== "object" ||
    data === null ||
    Array.isArray(data) ||
    data instanceof Date
  ) {
    return stringifyValues(data);
  }
  let s = "{";
  for (var key of Object.keys(data)) {
    if (
      data[key] === undefined ||
      data[key] instanceof Function ||
      key === undefined
    ) {
      // skipped
      continue;
    }
    s += `"${key}":` + stringifyValues(data[key]);
    s += ",";
  }
  // delete last comma if existed
  if (s[s.length - 1] === ",") {
    s = s.slice(0, -1);
  }
  s += "}";
  return s;
}
function stringifyValues(val) {
  switch (typeof val) {
    case "string":
      return `"${val}"`;
    case "number":
      if (val === Infinity || isNaN(val)) {
        return "null";
      }
      return "" + val;
    case "boolean":
      return "" + val;
    case "function":
      return undefined;
    case "symbol":
    case "undefined":
      return "null";
    case "object":
      if (val instanceof Date) {
        return '"' + val.toISOString() + '"';
      }
      if (val instanceof Array) {
        return "[" + val.map((el) => stringify(el)).join(",") + "]";
      }
      if (val === null) {
        return "null";
      }
      return stringify(val);
  }
}
```

<div id="q5-3" />

#### 5.3 Test Examples

```js
123
'string'
true
Boolean(false)
Number(1)
String('12')
[1, 'string', {a:  3}]
[NaN, null, undefined, Infinity]
new  Date()
{a: undefined, b: null, c: NaN, d: Infinity}
```

Other complicated checks:

```text
// Symbol-keyed property spec , expects "{}" but gets "}"
// console.log(stringify({ [Symbol()]: "value" }));

// Symbol in array

// function

// BigInt should lead to error

// non-enumerable properties should be ignored

// circular object should lead to error

// circular array should lead to error

// new Map() with enumerable keys

// property in prototype should be ignored

// emoji {a:'✌️'}
```

<div id="question6" />

### VI. Implement JSON.parse() method

**Notes:** before when split the `":"`, only split at the **first** `indexOf()` the current string

```javascript
/**
 * @param {string} str
 * @return {object | Array | string | number | boolean | null}
 */
function parse(s) {
  if (s === "") {
    throw new Error("Use Double Quots in JSON");
  }
  if (s[0] === "'") {
    throw new Error("Use Double Quots in JSON");
  }
  if (s === "[]") {
    return [];
  } else if (s === "null") {
    return null;
  } else if (s === "true") {
    return true;
  } else if (s === "false") {
    return false;
  } else if (s === "{}") {
    return {};
  } else if (s[0] === '"') {
    return s.slice(1, -1);
  } else if (+s === +s) {
    return Number(s);
  } else if (s[0] === "[") {
    return s
      .slice(1, -1)
      .split(",")
      .map((value) => parse(value));
  } else if (s[0] === "{") {
    return parseObj(s);
  } else {
    return null;
  }
}
function parseObj(str) {
  // parse object
  var obj = {};
  var arr = str.slice(1, -1).split(",");
  for (var s of arr) {
    var idx = s.indexOf(":");
    var key = s.slice(0, idx);
    var val = s.slice(idx + 1, s.length);
    obj[parse(key)] = parse(val);
  }
  return obj;
}
```

**Test Cases:**

```js
'{}'

'{"a":3}' spec , expects {a:3} but gets {}
'true'

'123'

'"123"'

'null'

'[{"a":{"b":{"c":[1]}}},null,"str"]' spec , expects [{a:{b:{c:[1]}}},null,"str"] but gets [{},null,"str"]

'{"a":"✌️"}' spec , expects {a:"✌️"} but gets {}

'[1,2,]'

'{'a':3}' spec Expected function to throw an exception.

'{"a":}' spec Expected function to throw an exception.
```

<div id="question7" />

### VII. References and Useful Links

- https://gist.github.com/andrew8088/6f53af9579266d5c62c8#file-stringify-js-L14
- Date to string:
  - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString
  - https://stackoverflow.com/questions/11491938/issues-with-date-when-using-json-stringify-and-json-parse
- [How to escape special characters in building a JSON string?](https://stackoverflow.com/questions/19176024/how-to-escape-special-characters-in-building-a-json-string)
