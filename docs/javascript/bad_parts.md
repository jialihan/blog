## JavaScript - The bad parts

#### I. [==](#question1)

#### II. [eval()](#question2)

#### III. [Block-less statement](#question3)

#### IV. [bitwise operator in JS](#question4)

#### V. [function statement vs. function expression](#question5)

#### VI. [Typed Wrappers](#question6)

#### VII. [void operator](#question7)

<div id="question1" />

### I. ==

Always use `===` and `!==`, the `==` is not checking the types, for example:
```js
'' == 0; // true
0 == '0'; // true
```

<div id="question2" />

### II. eval()
- Slower: because it needs the compiler to parse the string to execute a trivial assignment statement.
- Security concern: it grants too much authority to the eval's text.

<div id="question3" />

### III. Block-less statement

`if`, `while` and `do`, `for` statement can take **a block or a single line statement**. 
Advantages:
- it saves two chars `{}`
Disadvantages: 
- it obscures the structure of the code
	```js
	if(ok)
	    t = true;
	    next();
	```
   Please always use the `block {}` after the loop statement, which a good practice:
	```js
	if(ok) {
		t = true;
	}   
	next();
	```

<div id="question4" />

### IV. Bitwise operators in JS
Becareful, NO integers in Javascript, only has **double precision floating-point** numbers.

So, the bitwise operators **convert their number operands into integers**, do their business, and then **convert them back.** 

```js
&    and
|    or
^    xor
˜    not
>>   signed right shift 
>>>  unsigned right shift
<<   left shift
```

**JavaScript is rarely used for doing bit manipulation.** Here's the **signed** 32 bits representation of  numbers:
```js
0  =>  00000000 | 00000000 | 00000000 | 00000000 
-1  =>  11111111 | 11111111 | 11111111| 11111111  
2147483647  =>  01111111 | 11111111 | 11111111 | 11111111
-2147483648  =>  10000000 | 00000000 | 00000000 | 00000000
```

<div id="question5" />

### V. function statement vs. function expression
they looks the same
- function statement: hoisted, prohibited in `if()` statement.
- function expression: NOT hoisted

<div id="question6" />

### VI. Typed Wrappers

JavaScript has a set of typed wrappers. For example:
```
new Boolean(false)
```

Don't use new **Boolean or  new Number or new String**.Also avoid new Object and new Array. Use {}  and [] instead.


<div id="question7" />

### VII. [void operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/void)

It's very CONFUSING !!!

The  **`void`  operator**  evaluates the given  `_expression_`  and then returns  [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined).

**Syntax:**
```js
void expression
```

The `void` operator is often used merely to obtain the `undefined` primitive value, usually using "`void(0)`" (which is equivalent to "`void 0`"). In these cases, the global variable [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) can be used.

```js
void (0); // undefined
void (0 === '0'); // undefined
void 0 === '0'; //false: (void 0 ) === '0'
```