## Implementation of Native Array Methods

#### I. [Array.prototype.reduce](#question1)

- [Basic usage](#q1-1)
- [Advanced Usage ](#q1-2)
- [Implement your own Reducer function](#q1-3)

#### II. [Array.prototype.filter()](#question2)

- [Basic usage](#q2-1)
- [Advanced Usage ](#q2-2)
- [Implement your own Filter function](#q2-3)

#### III. [Array.prototype.map()](#question3)

- [Basic usage](#q3-1)
- [Advanced Usage ](#q3-2)
- [Implement your own Map() function](#q3-3)

#### IV. [Array.prototype.push()](#question4)

<div  id="question1"  />

### I. Array.prototype.reduce

<div  id="q1-1"  />

#### 1.1 Basic Usage

**Docs:** [reduce()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

For example:

```js
// eg: 1 + 2 + 3 + 4
array1.reduce(callbackFn(acc, curValue, index, array), initialValue);
```

<div  id="q1-2"  />

#### 1.2. Advanced Usage

The **callbackFn()** function takes four arguments:

1. `Accumulator` - returned value at each step
2. `Current Value`
3. `[Current Index`]` - optional
4. `[Source Array]` - optional

**Edge case:**
When `initialValue` not provided:

- accumulator will be the first element in array: `acc = arry[0]`
- `reduce()` will execute from `index 1` instead of index `0`
- If the array is empty and no `initialValue` is provided, [`TypeError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError) will be thrown.

**How reduce works?**

For example: when **No initial value provided**

```js
[0, 1, 2, 3, 4].reduce(function (
  accumulator,
  currentValue,
  currentIndex,
  array,
) {
  return accumulator + currentValue;
});
```

Callback invoked like the following:

| `callback` iteration | `accumulator` | `currentValue` | `currentIndex` | `array` | return value |

|--|--|--|--|--|--|--|--|

| first call | `0` | `1` |`1` | `[0, 1, 2, 3, 4]` | `1` |

| second call | `1` | `2` |`2` | `[0, 1, 2, 3, 4]` | `3` |

| third call | `3` | `3` |`3` | `[0, 1, 2, 3, 4]` | `6` |

| fourth call | `6` | `4` |`4` | `[0, 1, 2, 3, 4]` | `10` |

<div  id="q1-3"  />

#### 1.3 Implement your own Reducer function

Question: implement your own reducer function in `Array.prototype.myReduce()` with the same functionality.

For example:

```js
[1, 2, 3].myReduce((a, b) => a + b, 0);
```

**Solution:**

- attention to edge case when initial value is not provided

- not use arrow function, because there is **no** `this` keyword you can access.

```js
Array.prototype.myReduce = function (fn, initialValue) {
  if (!initialValue && this.length === 0) {
    throw "error input";
  }
  // 1. setup initial value
  var acc = arguments.length === 1 ? this[0] : initialValue;
  for (var i = 0; i < this.length; i++) {
    if (i === 0 && arguments.length === 1) {
      continue;
    }
    // 2. callbackFn with accumulator and curValue
    acc = fn.call(this, acc, this[i], i, this);
  }
  return acc;
};

// usage
console.log([1, 2, 3].myReduce((a, b) => a + b));
```

**Edge Case:**

```js
// Edge case:
// initial value could be undefined spec  , expects "undefined1" but gets 1
// initial value could be null spec  , expects "null1" but gets 1
```

<div  id="question2"  />

### II. Array.prototype.filter()

<div  id="q2-1"  />

#### 2.1 Basic Usage

**Docs:** [filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

For example:

```js
const words = ["a", "ab", "abc", "abcde"];
const result = words.filter((word) => word.length >= 3);
console.log(result);
// expected output: Array ["abc", "abcde"]
```

<div  id="q2-2"  />

#### 2.2. Advanced Usage

**Syntax:**

```js
let newArray = arr.filter(callback(currentValue[, index[, array]]) {
  // return element for newArray, if true
}[, thisArg]);
```

**Parameters:**

- `callback`
  the current element of the array. Return a value that coerces to `true` to keep the element, or to `false` otherwise.
  It accepts three arguments:
  - `currentValue`
  - `index - `Optional
  - `array - `Optional
- `thisArg - `Optional
  Value to use as `this` when executing `callback`. If a `thisArg` parameter is provided to `filter`, it will be used as the callback's `this` value. Otherwise, the value `undefined` will be used as its `this` value.

**Advanced usage example:**
With more parameters:

```js
// Appending new words
words = ["a", "ab", "abc", "abcd", "abcde"];
const myFunc = words.filter((el, index, arr) => {
  arr[index + 1] = "extra";
  return word.length < 6;
});
```

<div  id="q2-3"  />

#### 2.3 Implement your own `filter()` function

**Attention:** there is an optional param `thisArg` that needs to consider to invoke the function, so I use `call(this, param1, param2, ...)` there.

```js
Array.prototype.myFilter = function (fn, that) {
  var res = [];
  for (var i = 0; i < this.length; i++) {
    if (fn.call(that, this[i], i, this)) {
      res.push(this[i]);
    }
  }
  return res;
};
// usage
console.log([1, 2, 3].myFilter((el) => el !== 1)); // [2,3]
```

<div  id="question3"  />
  
### III. Array.prototype.map()

<div  id="q3-1"  />

#### 3.1 Basic Usage

**Docs:** [map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

For example:

```js
const array1 = [1, 2, 3, 4];
// pass a function to map
const map1 = array1.map((x) => x * 2);
// expected output: Array [1,4,9,16]
```

<div  id="q3-2"  />

#### 3.2 Advanced Usage

**Syntax:**

```js
let newArray = arr.map(callback(currentValue[, index[, array]]) {
  // return element for newArray, after executing something
}[, thisArg]);
```

**Parameters:**

- `callback`
  Function that is called for every element of `arr`. Each time `callback` executes, the returned value is added to `newArray`.
  The `callback` function accepts the following **arguments**:
  - `currentValue`
  - `index - `Optional
  - `array - `Optional
- `thisArg - `Optional
  Value to use as `this` when executing `callback`.

**Edge cases:**

- when `undefined` or nothing returned, there is a `undefined` value added in new-returned-array.

**Advanced usage example:**
With more parameters:

```js
// Appending new words
arr = ['a', 'ab', 'abc']
const myarray = arr.map( (el, index, arr) => {
	 ...
})
```

<div  id="q3-3"  />

#### 3.3 Implement your own `map()` function

**Attention:** there is an optional param `thisArg` that needs to consider to invoke the function, so I use `call(this, param1, param2, param3)` there.

```js
Array.prototype.myMap = function (fn, that) {
  var res = [];
  for (var i = 0; i < this.length; i++) {
    res.push(fn.call(that, this[i], i, this));
  }
  return res;
};
// usage
var newArray = [1, 2, 3].myMap((el, i, arr) => {
  return el + i * 2;
});
console.log(newArray); // (3) [1, 4, 7]
```

<div  id="question4"  />

### 4. Array.prototype.push

native implementation of `array.push()`, with the help of native method ["array.splice()"](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice).

Here, the usage is: `splice(n, 0, ...args)`, which means insert/add extra items from arguments at the end of original array.

**Code:**

```js
Array.prototype.myPush = function (...args) {
  var params = [...args];
  this.splice.apply(this, [this.length, 0].concat(params));
  return this.length;
};
```

**Usage:**

```js
var arr = [];
arr.myPush("a", "b", "c");
```

<div  id="question5"  />

### 5. Array.prototype.shift

```js

```
