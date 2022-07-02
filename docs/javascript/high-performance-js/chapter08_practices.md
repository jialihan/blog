## Chapter 8 - Programming Practices

### 1. Avoid double evaluation

JS allows you to take a string containing code & exec it from within running code. There are 4 ways to do this:

- `eval()`
- `Function()`
- `setTimeout()`
- `setInterval()`

**Example 1: eval**

```
let num1 = 1, num2 = 2;
console.log(eval("num1+num2")); // output 3
```

**Example 2: Function**

```
let  sum = new  Function("a", "b", "return a+b");
console.log(sum(1, 2)); // output: 3
```

**Example 3: setTimeout**

```
//setTimeout() evaluating a string of code
setTimeout("console.log('timeout:', sum(num1, num2))", 100);
// output: timeout: 3
```

**Example 4: setInterval**

```
//setInterval() evaluating a string of code
let timer = setInterval("console.log('interval:', sum(num1, num2))", 100);
setTimeout(() => {
	clearInterval(timer);
}, 300);
// output:
// interval: 3
// interval: 3
// interval: 3
```

How to improve these evaluations?

- there is no need to use `eval() or Function()`, and it’s best to avoid them whenever possible.
- For the other two functions, `setTimeout() and setInterval()`, it’s recommended to pass in a function as the first argument instead of a string.

```
setTimeout(() => {
	console.log("timeout:", sum(num1, num2));
}, 100);

let  timer2 = setInterval(() => {
	console.log("interval:", sum(num1, num2));
}, 300);
```

### 2. Use Object/Array Literals

There are multiple ways to create objects and arrays in JavaScript, but nothing is faster than creating object and array literals.

```
let obj = new Object();
let arr = new Array();
```

Better way to do it:

```
let obj = {};
let arr = [];
```

### 3. Don't do repeat work

Better ways to do it:

- **Lazy loading**: Lazy loading means that no work is done until the information is necessary.
- **Conditional Advance Loading**:
  An alternative to lazy-loading functions is conditional advance loading, which does the detection upfront, while the script is loading, instead of waiting for the function call. The detection is still done just once, but it comes earlier in the process.

### 4. Use the Fast Parts

#### 4.1 Bitwise Operators

JavaScript numbers are all stored in IEEE-754 **64-bit format**. For bitwise operations, though, the **number is converted into a signed 32-bit** representation.

There are 4 bitwise logic operators in JS:

- Bitwise AND: `25 & 3` // 1 ("1")
- Bitwise OR: `25 | 3` // 27 ("11011")
- Bitwise XOR: `25 ^ 3` // 26 ("11010")
- Bitwise NOT: `~25` // -26 ("-11010")

#### 4.2 Native JS method

Eg: `Math` Object ([MDN doc](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math#static_properties))

### 5. Summary

- Avoid the double evaluation penalty by **avoiding** the use of `eval() and the Function()` constructor. Also, **pass functions** into `setTimeout() and setInterval()` instead of strings.
- Use object and array **literals** when creating new objects and arrays. They are created and initialized faster than nonliteral forms.
- Avoid doing the same work repeatedly. Use **lazy loading or conditional advance loading** when browser-detection logic is necessary.
- When performing mathematical operations, consider using **bitwise operators** that work directly on the underlying representation of the number.
- **Native methods** are always faster than anything you can write in JavaScript. Use native methods whenever available.
