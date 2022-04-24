## Chapter 4 - Algorithms and Flow Control

The performance of how the code has been organized in JavaScript.

### 1. Loops

#### 1.1 types of loops

- for loop
  > Note: define a variable inside for-loop, which is equal to define it outside of for-loop, since JS only has a function-based scope.
- while loop
- do-while loop
- for-in loop: important! - never use this to loop an array, this is for object to use looping properties.

#### 1.2 loop performance

There are actually just two factors to contribute to loop perforamance:

- Work done per iteration
- Number of iterations

**Ways to improve performance:**
1.2.1 Decreasing the work per iteration
For example: original code

```js
for (var i = 0; i < items.length; i++) {
  process(items[i]);
}
```

Things to improve:

- One property lookup (items.length) in the control condition
- One comparison (i < items.length) in the control condition

Optimized code 1: use an variable to store the length

```js
//minimizing property lookups
for (var i = 0, len = items.length; i < len; i++) {
  process(items[i]);
}
```

Optimized code 2: loop in reverse way to get the length only at initial time

```js
//minimizing property lookups and reversing
for (var i = items.length; i--; ) {
  process(items[i]);
}
```

> Note: Decreasing the work done per iteration is most effective when the loop has a complexity of O(n). When the loop is more complex than O(n), it is advisable to focus your attention on decreasing the number of iterations.

1.2.2 Decreasing the number of iterations
Slow

```js
// loop every item directly
for (var count = 0; count < len; count++) {
  name = document.getElementsByTagName("div")[count].nodeName;
}
```

Faster

```js
// loop a copy of collection
const copy = document.getElementsByTagName("div");
for (var count = 0; count < len; count++) {
  name = copy[count].nodeName;
}
```

**Fastest**: use local variable when access collection elements

```js
const copy = document.getElementsByTagName("div");
for (var count = 0; count < len; count++) {
  var el = copy[count];
  name = el.nodeName;
}
```

#### 1.3 Function based iteration

The forEach() method is implemented natively in Firefox, Chrome, and Safari. Additionally, most JavaScript libraries have the logical equivalent:

```
//jQuery
jQuery.each(items, function(index, value){
    process(value);
});

//Dojo
dojo.forEach(items, function(value, index, array){
    process(value);
});

//Prototype
items.each(function(value, index){
    process(value);
});
```

### 2. Conditionals

#### 2.1 if-else vs. switch

Result:
As it turns out, the switch statement is faster in most cases when compared to if-else, but significantly faster only when the number of conditions is large.

Generally speaking, if-else is best used when there are two discrete values or a few different ranges of values for which to test. When there are more than two discrete values for which to test, the switch statement is the most optimal choice.

#### 2.2 optimize if-else

This is achieved by applying a binary-search-like approach, splitting the possible values into a series of ranges to check and then drilling down further in that section.

#### 2.3 Lookup Tables

Advantages:

- very fast compared to if-else and switch
- they also help to make code more readable when there are a large number of discrete values for which to test.

The amount of space that this switch statement occupies in code is probably not proportional to its importance. The entire structure can be replaced by using an array as a lookup table:

```js
//define the array of results
var results = [
  result0,
  result1,
  result2,
  result3,
  result4,
  result5,
  result6,
  result7,
  result8,
  result9,
  result10
];

//return the correct result
return results[value];
```

Lookup Table vs. Switch use-case:

- Lookup tables are most useful when there is logical **mapping between a single key and a single value** (as in the previous example).
- A switch statement is more appropriate when each key r**equires a unique action or set of actions** to take place.

### 3 Recursion

Complex algorithms are typically made easier by using recursion.
Eg: a factorial function

```
function factorial(n) {
    if(n===0)
    {
        return 1;
    }
    else {
        return n*factorial(n-1);
    }
}
```

**Cons:**

- The problem with recursive functions is that an **ill-defined or missing terminal condition** can lead to long execution times that freeze the user interface.
- Further, recursive functions are more likely to run into browser **call stack size limits**.

#### 3.1 Call stack limit

When you exceed the maximum call stack size by introducing too much recursion, the browser will error out with one of the following messages:

- Internet Explorer: “Stack overflow at line x”
- Firefox: “Too much recursion”
- Safari: “Maximum call stack size exceeded”
- Opera: “Abort (control stack overflow)”

Chrome is the only browser that doesn’t display a message to the user when the call stack size has been exceeded.

#### 3.2 Recursion patterns

Bad example to remind infinite loop patterns:
Bad Example 1: call itself

```js
function recurse() {
  recurse();
}
recurse();
```

Bad Example 2: call each other

```
function first(){
    second();
}

function second(){
    first();
}

first();
```

#### 3.3 Iteration

example of Merge sort.
The code for this merge sort is fairly simple and straightforward, but the mergeSort() function itself ends up getting called very frequently. Recursive functions are a very common cause of stack overflow errors in the browser.

#### 3.4 Memoization

To make memoizing a function easier, you can define a memoize() function that encapsulates the basic functionality. For example:

```js
function memoize(fundamental, cache) {
  cache = cache || {}; // object to memo

  var shell = function (arg) {
    if (!cache.hasOwnProperty(arg)) {
      cache[arg] = fundamental(arg);
    }
    return cache[arg];
  };

  return shell;
}
```

Usage:

```js
//memoize the factorial function
var memfactorial = memoize(factorial, { 0: 1, 1: 1 });

//call the new function
var fact6 = memfactorial(6);
var fact5 = memfactorial(5);
var fact4 = memfactorial(4);
```

### 4. Summary

Some optimization techniques are important:

- The `for`, `while`, and `do-while` loops all have similar performance characteristics, and so **NO** one loop type is significantly faster or slower than the others.
- Avoid the `for-in` loop unless you need to iterate over a number of unknown object properties.
- The best ways to improve loop performance are to decrease the amount of work done per iteration and decrease the number of loop iterations.
- Generally speaking, `switch` is always faster than `if-else`, but isn’t always the best solution.
- **Lookup tables** are a faster alternative to multiple condition evaluation using `if-else` or `switch`.
- Browser call stack size limits the amount of recursion that JavaScript is allowed to perform; stack overflow errors prevent the rest of the code from executing.
- If you run into a stack overflow error, change the method to an iterative algorithm or make use of memoization to avoid work repetition.
