## Functions in JavaScript

- I. [basic features](#ch1)
- II. [Invocation  - 3 way](#ch2)
- III. [Arguments](#ch3)
- IV. [Exceptions](#ch4)
- V. [Augment Types](#ch5)
- VI. [Recursion](#ch6)
	- [Problem 1: Towers of Hanoi puzzle](#ch6-1)
	- [Problem 2: Problem 2: Walk through the DOM](#ch6-2)
	- [Problem 3: factorial](#ch6-3)
- VII. [Scope](#ch7)
- VIII. [Closure](#ch8)
- IX. [Callbacks](#ch9)
- X. [Module](#ch10)
- XI. [Cascade](#ch11)
- XII. [Curry](#ch12)
- XIII. [Partial application](#ch13)
- XIV. [Memorization](#ch14)
- XV. [compose & pipe](#ch15)


<div id="ch1" />

### 1. basic features

When we invoke the function, automatically get 2 params in Execution Context:
- this
- arguments

When we define our functions, complier looks the code **lexically**, it determines what variables are available for us, and our **variable environment**, and it also add the **scope chain.**

**What properties in a function object?**
function object is linked to -> [Function.prototype](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function) -> linked to Object.prototype

Eg: `myFunc(){}`
- Code()
- name: optional - anonymous function
- properties from **"Function.prototype"**: `.call()`, `.apply()`, `.bind()`
- ...

**Function constructors:**
[docs-MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/Function)

**Syntax:**
```js
var myFunc = new Function('a', 'b', 'return a + b');
```
**Note:** 
 [Difference between Function constructor and function declaration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function#difference_between_function_constructor_and_function_declaration "Permalink to Difference between Function constructor and function declaration")
- only global & it's local scope
- do not create closures to their creation contexts

<div id="ch2" />

### 2. Invocation  - 3 way
- function invocation pattern - `()`: eg: `myfunc();` , binding global object (window)
- method invocation pattern: eg: `obj.myfunc()`, binding `this` to the object
- use [call](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)() / [apply](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)(): eg: `myfunc.call();`

### 3. functions are first class citizens
1 ) functions can be **assigned to variables & properties** of objects
```js
var myfunc = function () {};
var obj = { myfunc: function(){} }; 
```

2 ) a function can be **passed as parameter** to another parameter
```js
function a(myFunc)
{
	myFunc();
}
a(()=>console.log('this is my func !'));
```
3 )  a function can be **returned as a value** from another function
```js
function b() {
	return fucntion c() {console.log('function c!')};
}
```

<div id="ch3" />

### 3. Arguments
a bonus parameter inside functions is `arguments`: an array-like-object, but not the array, no array methods on it.
- arguments.length
- `arguments[i]`: to grab the arguments

<div id="ch4" />

### 4. Exceptions
throw an exception, which interrupts the exectuation of the code. It contains an object with name for the type of exception, and the desc message prop, other props if needed.
```js
throw {
	name: 'TypeError',
	message: '............'
};
```

<div id="ch5" />

### 5. Augment Types

Adding  a method to `Object.prototype` makes that method                available to all objects. This also works for functions, arrays, strings, numbers, regular expressions, and booleans.

1 ) adding method to `Function.prototype`
```js
Function.prototype.method = function(name, func){
	this.prototype[name] = func;
	return this;
}
```

2 ) adding method to `Number.prototype` using the above 
For example, get the integer part of a number in JS:
```js
Number.method('getInteger', function(){
	return Math[this < 0 ? 'ceil': 'floor'](this);
});
// test
console.log((-10/3).getInteger());
```

3 ) adding method to `String.prototype` using the above method
For example, trim() the string using regex expression:
```js
// TODO: understand the regex
String.method('trim', function(){
	 return this.replace(/^\s+|\s+$/g, '');
});
// test
console.log("    test    ".trim());
```

<div id="ch6" />

### 6. Recursion

A recursive function is a function that calls itself, either                directly or indirectly.

<div id="ch6-1" />

#### 6.1 Problem 1: Towers of Hanoi puzzle 
Reference article:  [Tower of hanoi](https://js-algorithms.tutorialhorizon.com/2016/01/11/tower-of-hanoi/)

Assumption: We move the `n disks` from src to dest, with another empty.

Steps:
-   Move a tower of  `height-1`  to the  `buffer`, using the  `destination` as an helper.
-   Move the remaining disk to the  `destination`.
-   Move the tower of  `height-1`  from the  `buffer`  to the  `destination`  using the  `source` as an helper.

**Solution:**
```js
var hanoi = function hanoi(n, src, dest, buffer) {
	if (n > 0) {
		// move n-1 from src to buffer
		hanoi(n - 1, src, buffer, dest);
		// move remaining 1 disk from src to the dest 
		console.log('Move disk', n, ' from Tower ', src, ' to Tower ', dest);
		// move n-1 disks from buffer to dest    
		hanoi(n - 1, buffer, dest, src);
	}
}
hanoi(3, 'Src', 'Dest', 'Buffer');
```
**Result:**

![image](../assets/hanoiresult.png ':size=691x168')

<div id="ch6-2" />

#### 6.2 Problem 2: Walk through the DOM
Since dom doesn't have a function to getElements by attribute, it only have a single node function [`Element.getAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/getAttribute). 

Note: [Node.nodeType](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType) = 1, means the Element node.

```js
var  walk_the_dom = function  walk(node, fn){
	fn(node);
	node = node.firstChild;
	while(node)
	{
		walk(node, fn);
		node = node.nextSibling;
	}
}

var  getElementsByAttribute = function(attr, value) {
	var  res = [];
	walk_the_dom(document.body, function(node) {
		var  curValue = node.nodeType === 1 && node.getAttribute(attr);
		if(typeof  curValue === 'string' && (curValue === value || typeofvalue !== 'string'))
		{
			res.push(node);
		}
	});
	return  res;
}
```

<div id="ch6-3" />

#### 6.3 Problem 3: factorial  
Functions that recurse very deeply can fail by exhausting the return  stack:
```js
var factorial = function factorial(n){
	if(n===1 || n===2)
	{
		return n;
	}
	return n * factorial(n-1);
}
factorial(4); // 24
```

<div id="ch7" />

### 7. Scope

Scope in a programming language controls the **visibility and                lifetimes of variables and parameters**. 

This is an important service to the programmer because it - 
- reduces naming collisions
-  and provides automatic memory management

ES5 - function-based scope
ES6 - block -based scope

<div id="ch8" />

### 8. Closure

Good thing:
Inner functions get access to the parameters and                variables of the functions they are defined within (with the exception of this and arguments).

see another closure article instead: [here](https://jialihan.github.io/blog/#/javascript/closure) 


<div id="ch9" />

### 9. Callbacks

Functions can make it easier to deal with discontinuous events.

Use-case:
 To make an asynchronous request, providing a callback                function that will be invoked when the server's response is received. 
```js
request = prepare_the_request(  );
send_request_asynchronously(request, function(response) {   
	display(response);    
});
```
The second param function, is a callback will be executed, when the response is available.

<div id="ch10" />

### 10. Module

#### 10.1 What is the Module?

A module is a function or object that presents an interface but that hides its state and implementation. 

#### 10.2 the module pattern

- defines private variables and function
- create privileged **function from closure** that can have access to private variables
- return those functions to outside to use

**For example:** create a module

here **private variables** are: `prefix`, `seq`;
returned **api functions** are: `set_prefix()`, `set_seq()`, `gensym()`
```js
var serial_maker = function (  ) {
	var prefix = '';    
	var seq = 0;    
	return {        
		set_prefix: function (p) {            
			prefix = String(p);        
		},        
		set_seq: function (s) {            
			seq = s;       
		},        
		gensym: function ( ) {            
			var result = prefix + seq;            
			seq += 1;            
			return result;        
		}    
	};
};
```

Usage:
```js
var seqer = serial_maker();
seqer.set_prefix('Q');
seqer.set_seq(1000);
var unique = seqer.gensym( ); // unique is "Q1000"
```

<div id="ch11" />

### 11. Cascade

- some methods don't return a value
- to enable cascades, return `this` instead of `undefined`
- **The Javascript cascade design pattern is a way to create a chaining pattern for methods of our own objects.**

For example 1:
```js
getElement('myBoxDiv')    
	.move(350, 150)    
	.width(100)    
	.height(100)    
	.color('red')
```

For example 2:
```js
string.split('').reverse().join();
```

Advantages of cascade:
- it can produce interfaces that are every **expressive**

<div id="ch12" />

### 12. Curry

#### 12.1 What is curry?
- Currying allows us to produce a new function by combining a                function and an argument
- It can multiple params, using curry modifying to a function that take one param at time.
	For exmaple:
	```js
	var multiply = (a,b)=>a*b;
	var curriedMutiply = (a)=>(b)=>a*b;
	/* usage */
	multiply(2, 3); // 6
	curriedMutiply(2)(3); // 6
	```

#### 12.2 How curry works? 
- works by creating a closure  that holds that original function and the arguments to curry
- it **returns** the result of **calling that original function**, **passing it  all of the arguments from the invocation of curry**  and the current invocation.
	```js
	var curriedMutiply = (a)=>(b)=>a*b;
	var curriedMutiplyBy2 = curriedMutiply(2);
	```

<div id="ch13" />

### XIII. Partial application

Ahead binding some parameters, then return the new function with some parameter already set there.

For example:
```js
var multiply = (a,b,c)=>a*b*c;
var partialMultiplyBy2 = multiply.bind(null, 2);
// usage
multiply(2,3,4); // 24
partialMultiplyBy2(3,4); // 24
```

<div id="ch14" />

### XIV. Memorization

#### 14.1 what is memorization?
Functions can use objects to remember the results of previous operations, making  it possible to avoid unnecessary work. This optimization is called  *memoization*. 

#### 14.2 how it works?

We will keep our memoized results in a memo   array that we can hide in a closure. When our function is called, it first looks to  see if it already knows the result. If it does, it can immediately return it:

#### 14.3 Memorizer for fibonacci example

Original **recursive** Fibonacci function:
```js
var fib = function fib(n) {
	return  n < 2 ? n : fib(n-1) + fib(n-2);
}
```
Write a **memorizer** function:
```js
var  memo = function(fn){
	var  cache = [];
	var  func = function(...args)
	{
		if(!cache[args[0]])
		{
			cache[args[0]]= fn.apply(null, arguments);
		}
		return  cache[args[0]];
	}
return  func;
}
```

Usage:
wrap the memorizer on the original recursive method:
```js
var memoFib = memo(fib);
memoFib(3); // 2
memoFib(4); // 3
```

<div id="ch15" />

### XV. compose & pipe

#### 15.1 compose
**Goal:**
create a little assembly line, compose different functions together. Composibility is a system design principle that deals relationship between components.
```js
fn1(fn2(fn3(50)));
compose(fn1, fn2, fn3)(50) //Right to lext
```

**Implement a compose function** by ourself:
```js
const compose = (f, g) => (data) =>  f(g(data));
// usage
const multiplyBy3AndAbsolute = compose (
	(num) => num*3, 
	Math.abs
);
console.log(multiplyBy3AndAbsolute(-50)); // 150
```

#### 15.2 pipe
Goal: 
function's order will be different, the opposite order, but the result will be same as compose way.
```js
fn1(fn2(fn3(50)));
pipe(fn3, fn2, fn1)(50)//left to right
```
**Implement a pipe function:**
```js
const  pipe = (f, g) => (a) =>  g(f(a));
const multiplyBy3AndAbsolute = pipe (
	Math.abs,
    (num) => num*3, 
);
console.log(multiplyBy3AndAbsolute(-50)); // 150
```
