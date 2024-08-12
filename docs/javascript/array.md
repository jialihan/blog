## Arrays in JavaScript

#### I. [Array Literals](#question1)

#### II. [Length property](#question2)

#### III. [Delete](#question3)

- [`delete` operator](#q3-1)
- [Array.prototype.splice()](#q3-2)

#### IV. [Enumeration](#question4)

#### V. [Confusion: object vs. array](#question5)

#### VI. [c on Array.prototype](#question6)

- [6.2 Slice method](#q6-2)
- [6.3 Pop, Push, Shift and Unshift](#q6-3)
- [6.4 how to "shuffle()" an Array?](#q6-4)
- [6.5 sort() default behavior](#q6-5)

#### VII. [Dimensions](#question7)

<div id="question1" />

### I. Array Literals

Array Literals provide a very convenient notation for creating new array values.

For example:

```js
var arr = [];
arr[1]; // undefined
arr.length; // 0
```

Array can have a **mixture of values**:
for example: array can have string, number, boolean.

```js
var mixArray = ["a", 0, 1, true, NaN, Infinity];
```

<div id="question2" />
  
### II. Length property
- Every array has a length property
- it's **not the upper_bound**, length can **increase** when we augment the array.

<div id="question3" />

### III. Delete

<div id="q3-1" />

#### 3.1 delete operator

the `delete` operator can be used to remove elements from the array, but it leaves a hole in the array, length is not change.

For example:

```js
var arr = [1, 2, 3]; // (3) [1, 2, 3]
delete arr[2]; // true
arr.length; // 3
arr; // (3) [1, 2, empty]
```

<div id="q3-2" />

#### 3.2 `Array.prototype.splice()`

**Docs:** [splice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

**Syntax:**
The splice() method adds/removes items to/from an array, and returns the removed item(s). Note: `This method changes the original array`.

```js
[].splice(index, howmany, item1, ....., itemN)
```

**Parameters:**

- `index`: required. the position to add/remove items, `negative values` to specify the position from the end of the array.

- `howmany`: optional, how many items to be removed. if `0`, none will be removed!

- `newItems`: optional. The new item(s) to be added to the array.

  **Use cases:**

```js
var arr = [1, 2, 3, 4, 5];
arr.splice(1); // [1], remove from index 1 to the end
arr.splice(1, 1); // [1,3,4,5], only remove 1 element from index 1
arr.splice(1, 1, "new1", "new2"); // [1,'new1','new2',3,4,5], remove 2, add new two items
```

<div id="question4" />

### IV. Enumeration

#### 4.1 `for in` loop can iterate all entries, but order not guaranteed

#### 4.2 `for loop with index`

```js
for (var i = 0; i < array.length; i++) {
  console.log(array[i]);
}
```

<div id="question5" />

### V. Confusion: object vs. array

when you do `typeof array` is `'object'`, JS doesn't have a good distinguishing between arrays and objects.

In modern browsers, we can use: [`Array.isArray()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray)

```js
Array.isArray([1, 2, 3]); // true
Array.isArray({ foo: 123 }); // false
Array.isArray("foobar"); // false
Array.isArray(undefined); // false
```

<div id="question6" />

### VI. Methods on Array.prototype

`Array.prototype` can be augmented as well.

The easiest way to have a full scope of what methods array can do, we just need to open the browser's console, and type:

```js
Array.prototype;
```

Then we can see all the methods that array instances can use.

```
[constructor: ƒ, concat: ƒ, copyWithin: ƒ, fill: ƒ, find: ƒ, …]
concat: ƒ concat()
constructor: ƒ Array()
copyWithin: ƒ copyWithin()
entries: ƒ entries()
every: ƒ every()
fill: ƒ fill()
filter: ƒ filter()
find: ƒ find()
findIndex: ƒ findIndex()
flat: ƒ flat()
flatMap: ƒ flatMap()
forEach: ƒ forEach()
includes: ƒ includes()
indexOf: ƒ indexOf()
join: ƒ join()
keys: ƒ keys()
lastIndexOf: ƒ lastIndexOf()
length: 0
map: ƒ map()
pop: ƒ pop()
push: ƒ push()
reduce: ƒ reduce()
reduceRight: ƒ reduceRight()
reverse: ƒ reverse()
shift: ƒ shift()
slice: ƒ slice()
some: ƒ some()
sort: ƒ sort()
splice: ƒ splice()
toLocaleString: ƒ toLocaleString()
toString: ƒ toString()
unshift: ƒ unshift()
values: ƒ values()
Symbol(Symbol.iterator): ƒ values()
Symbol(Symbol.unscopables): {copyWithin: true, entries: true, fill: true, find: true, findIndex: true, …}
```

<div id="q6-2" />

#### 6.2 Slice method

**Syntax:**
slice() method returns the selected elements in an `new array`. [start,end), end index is not included.

```js
[].slice(start, end);
```

- if start is omitted, default value is from index `0`
- if end is omitted, all elements from start until the end of the array

  **Use cases:**

- use `[].slice()` to clone an array, return a new array object.

- normal usage:

```js
var arr = [1, 2, 3];
arr.slice(1); // select index 1 to the end, [2,3]
arr.slice(-1); // select last element, [3]
arr.slice(0, 2); // select index from 0 to 1, [1,2]
```

<div id="q6-3" />

#### 6.3 Pop, Push, Shift and Unshift

Javascript also gives us four method to easily add/ remove items to the beginning/ end of the array. For example, original array is `var arr = [1,2,3];`.

- `push()`: <strong>Add items</strong> to the <strong>end</strong> of the array, can have multiple arguments
  ```js
  arr.push(4); // [1,2,3,4]
  arr.push(4, 5); // [1,2,3,4,5]
  ```
- `pop()`: <strong>Remove an item</strong> from the <strong>end</strong> of the array
  ```js
  arr.pop(); // [1,2]
  ```
- `unshift()`: <strong>Add items</strong> to the <strong>beginning</strong> of the array ,can have multiple arguments
  ```js
  arr.unshift(0); // [0,1,2,3]
  arr.unshift(-1, 0); // [-1,0,1,2,3]
  ```
- `shift()`: <strong>Remove an item </strong>to the <strong>end</strong> of the array
  ```js
  arr.shift(); // [2,3]
  ```

<div id="q6-4" />

#### 6.4 how to "shuffle()" an Array?

- [lodash "shuffle()"](https://github.com/lodash/lodash/blob/master/shuffle.js) method
- [bfe-08](https://bigfrontend.dev/problem/can-you-shuffle-an-array): My own "shuffle()" method:
  ```js
  function shuffle(arr) {
    // modify the arr inline to change the order randomly
    for (var i = arr.length - 1; i > 0; i--) {
      var newIndex = Math.floor(Math.random() * (i + 1));
      // swap i & newIdx
      [arr[i], arr[newIndex]] = [arr[newIndex], arr[i]];
    }
  }
  // const arr = [1, 2, 3, 4];
  // shuffle(arr);
  // console.log(arr);
  ```

#### 6.5 sort() default behavior

The [default sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) order is ascending, built upon converting the elements into **strings**, then comparing their sequences of UTF-16 code units values.

eg:

```js
const array1 = [1, 30, 4, 21, 100000];
array1.sort();
console.log(array1);
// Expected output: Array [1, 100000, 21, 30, 4]
```

### VII. Dimensions

Note: Javascript does not have more than one dimension.

How to make a 2D array in javascript?

#### 7.1 solution 1: array literal

```js
var items = [
  [1, 2],
  [3, 4],
  [5, 6],
];
```

#### 7.2 solution 2: for loop

```js
var x = new Array(n);
for (var i = 0; i < n; i++) {
  x[i] = [];
  // or
  // x[i] = new Array(3);
}
```

#### 7.3 solution 3: `map()` in ES6

For example: create a `m*n` dimension array

```js
var arr = Array(m)
  .fill(null)
  .map(() => Array(n));
```
