## Iterations and Generators in JavaScript

#### 1. [What is a generator function?](#question1)

#### 2. [The iterator protocol](#question2)

- [2.1 Iterator Syntax](#q2-1)
- [2.2 Usage Example](#q2-2)
- [2.3 use Generator function as an Iterator](#q2-3)

#### 3. [The iterable protocol](#question3)

- [3.1 Protocal Syntax](#q3-1)
- [3.2 bfe #39. range() use iteration protocol](#q3-2)
- [3.3 iterable object from a Generator function](#q3-3)

#### 4. [Iterator Pattern in JS](#question4)

#### 5. [Reference and Links](#question5)

<div id="question1" />

### I. What is a [generator function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)?

The `function*` declaration (`function` keyword followed by an asterisk) defines a _generator function_, which returns a [`Generator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator) object.

#### 1.1 Syntax:

```js
function* generator(i) {
  yield i;
  yield i + 10;
}
```

#### 1.2 How to consume it? - [Generator Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)

method:

- [`next()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator/next) : Returns a value yielded by the [`yield`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield) expression.
- [`return()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator/return) : Returns the given value and finishes the generator.
- [`throw()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator/throw)

Usage Example:

```js
const gen = generator(10);
console.log(gen.next().value);
// expected output: 10
```

#### 1.3 BFE 39 - implement range()

[#39. link](https://bigfrontend.dev/problem/implement-range)

```js
// Solution1: function generator
function range(from, to) {
  return (function* gen() {
    while (from <= to) {
      yield from++;
    }
  })(from, to);
}
```

<div id="question2" />

### II. [The iterator protocol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterator_protocol "Permalink to The iterator protocol")

<div id="q2-1" />

#### 2.1 Iterator Syntax

An object is an iterator when it implements a `next()` method with the following semantics:

![image](../assets/iterator-protocol.png ":size=550")

**Syntax Example:**

```js
// Satisfies both the Iterator Protocol and Iterable
const myIterator = {
	// ...
    next: function() {
        return {
			done: ...,
			value: ...,
		}
    }
};
```

**Iterator Code Example:**
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#iterator_examples

<div id="q2-2" />

#### 2.2 Usage Example

```js
myIterator.next(); // {value: 1, done: false}
myIterator.next().value; // 1
```

<div id="q2-3" />

#### 2.3 use Generator function as an Iterator

**Note:**
generation function returns an [Generator object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator), which conforms to both the iterable protocol and the iterator protocol.

**Instance Method:**

- Generator.prototype.`next()`
  Returns a value yielded by the yield expression.
- Generator.prototype.`return()`
  Returns the given value and finishes the generator.
- Generator.prototype.`throw()`

**Usage:**

```js
function* gen() {
  yield* [1, 2, 3];
}
var myIterator = gen();
console.log(myIterator.next()); // {value: 1, done: false}
console.log(myIterator.next().value); // 1
```

<div id="question3" />

### III. [The iterable protocol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterable_protocol "Permalink to The iterable protocol")

**Docs:**
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterable_protocol

<div id="q3-1" />

#### 3.1 Protocal Syntax

![image](../assets/iterable-protocol.png ":size=600")

> Note:
> ONLY **The iterable protocol** allows JavaScript objects to define or customize their iteration behavior, such as what values are looped over in a [`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) construct.

So if you want to call it in a `for...of` loop, you **MUST implement the iterable protocol**, only the "iterator protocol" is **NOT** enough.

A suggestion Syntax for both protocol:

```js
// Satisfies both the Iterator Protocol and Iterable
const myIterator = {
  next: function () {
    // ...
  },
  [Symbol.iterator]: function () {
    return this;
  }
};
```

<div id="q3-2" />

#### 3.2 bfe #39. range() use iteration protocol

```js
function range(from, to) {
  var i = from;
  return {
    next: function () {
      return i <= to
        ? {
            value: i++,
            done: false
          }
        : {
            done: true
          };
    },
    [Symbol.iterator]: function () {
      return this;
    }
  };
}
```

<div id="q3-3" />

#### 3.3 iterable object from a Generator function

For example, we already have an generator function:

```js
function* gen() {
  yield* [1, 2, 3];
}
```

To make an iterable object:

**Syntax 1:**

```js
var myIterable = {
  [Symbol.iterator]: gen
};
```

**Syntax 2:**

```js
var myIterable = {
  [Symbol.iterator]: function* () {
    yield* [1, 2, 3];
  }
};
```

**Usage Example:**
Only "Iterable" object can be used in `for...of` loop and `...`(spread operator):

```js
for (var val of myIterable) {
  console.log(val);
}
console.log(...myIterable); // 1 2 3
```

<div id="question4" />

#### IV. Iterator Pattern in JS

Pretty similar to JS standard [Iterator protocal](#question2), but we can build more custom & convenient methods, eg: `hasNext(), current(), rewind()....`

**Method:**

- next()
- hasNext()
- rewind() : To reset the pointer back to the beginning
- current() : To return the current element, because you cannot do this with `next()` **without advancing the pointer**

**Code Example:**

```js
var myIterator = (function () {
  var index = 0,
    data = [1, 2, 3],
    length = data.length;
  return {
    next: function () {
      var element;
      if (!this.hasNext()) {
        return null;
      }
      element = data[index++];
      return element;
    },
    hasNext: function () {
      return index < length;
    },
    rewind: function () {
      index = 0;
    },
    current: function () {
      return data[index];
    }
  };
})();
```

<div id="question5" />

### V. Reference and Links

- **Iterators and generators** : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators
- **Iteration protocols** : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterable_protocol
- **Understanding JS Generators** : https://codeburst.io/understanding-generators-in-es6-javascript-with-examples-6728834016d5
