
# JavaScript - The Good Parts

#### [Chapter 1 Good Parts](#chapter1)
- [What is javascript?](#ch1-1)
- [ JavaScript language features](#ch1-2)
- [Why Javascript finally ?](ch1-3)

#### [Chapter 2 Grammar](#chapter2)
- [Whitespace](#ch2-1)
- [Comments](#ch2-2)
- [Names](#ch2-3)
- [Number](#ch2-4)
- [String](#ch2-5)
- [Statement](#ch2-6)
- [Function](#ch2-7)
- [Literals](#ch2-8)


<div id="chapter1" />

## I. Chapter 1: Good Parts

<div id="ch1-1" />

### 1.1 What is javascript?
- JS is a beautiful, elegant, highly expressive , dynamic programming language.
- JS has significant **differences** from **other language**
- Good parts: easy to learn for any background people

<div id="ch1-2" />

### 1.2 JavaScript language features
**Good:** 
- Loose typing
	- JS cannot detect type errors
	- don't need to cast types
- dynamic objects 
- expressive object [literal notation](https://en.wikipedia.org/wiki/Literal_%28computer_programming%29)
	- similar JSON format
	- key-value pairs to define objects & components
- Prototype inheritance

**Bad:**
- global object: together in a common namespace
- unavoidable bad parts
- JS has many errors and sharp edges


<div id="ch1-3" />

### 1.3 Why Javascript finally ?
- JS is the **only** language in all browsers.  (java failed)
- it's lightweight and expressive, a really good language.

<div id="chapter2" />

## II. Chapter 2: Grammar

<div id="ch2-1" />

### 2.1 whitespace - for formatting characters

<div id="ch2-2" />

### 2.2 Comments
- way1: `/*  */`
	A special case example, might be error, cannot be commented:
	```js
	/*
		var s = /a*/.match(t);
	*/
	```
- way2: `//` - starting line

<div id="ch2-3" />

### 2.3 Names
Some reserved words:
- abstract
- boolean break byte
- case catch char class const continue
- debugger default delete do double
- ......

Weird part: some should be reversed, **but not:**
- undefined
- NaN 
- Infinity

But actually in modern browser, I tested:
but in newer browsers (tested in Chrome & ES6), assigning a new value to  `undefined`  has no effect. So although it  _seems_  you can assign a new value, you actually  **cannot**.

![image](../assets/chapter2-3.png ':size=269x168')

<div id="ch2-4" />

### 2.4 Numbers
- single number type: 64-bit floating point, same as java's double
- `1.0` is the same value as `1`
- **Advantage:** 
	- type error avoided
	- overflow in short integers(`Integer.MAX_VALUE`) in Java avoided
		```java
		int val1 =  9898989;  
		int val2 =  6789054;  
		long sum =  (long)val1 +  (long)val2;  
		if  (sum >  Integer.MAX_VALUE)  
		{  
			throw  new  ArithmeticException("Overflow!");  
		}
		```
- number literal has an exponent part -> based on power of 10
	For example: `1e2 === 100, 1.73e2 === 173`

- negative numbers useing `-` operator
- NaN: 
	- not equal to any value, including itself: `NaN !== NaN`
	- detect NaN with the `isNaN()` function

- **[Infinity](https://www.w3schools.com/jsref/jsref_infinity.asp):** 
	is displayed when a number exceeds the upper limit of the floating point numbers, which is 1.797693134862315E+308.

<div id="ch2-5" />

### 2.5 string
- string literal: `''` - a single quote
- `\(backslash)` is the escape character
	- usage: when not permitted string eg: quotes, backslashed(`\`), control chars(`\n, \b, \t`).
- JS was built when Unicode was a `16-bit` set, each char is 16-bits wide. 
	- convert to ASCII code in JS: `ch.charCodeAt(0)`: # [String.prototype.charCodeAt()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt)
- concat string: `+ (ES5)`, and ``backtick in ` (ES6) ``
- `toUpperCase(), toLowerCase()`

<div id="ch2-6" />

### 2.6 Statements
- all `<script>`  tag compile together  and use the same **global namespace.**
- statement types:
	- expressive statement
	- disruptive statement: eg: break
	- try statement
	- if statement
		**Falsy values**:
			- false
			- null
			- undefined
			- empty string: `''`
			- number: 0
			- number: NaN
	- switch statement
	- while statement 
	- for statement
		- for ( initialization, condition, increment ): 3  parts
		- for ( key in object): `in` keyword, usually used to determine a props from prototype chain or itself:
			```js
			for( key in obj)
			{
				if(obj.hasOwnProperty(key))
				{
					...
				}
			}
			```
	- do statement: execute at least once
	- return statement: return **a function or value**, if no return statement, it's `undefined`.

<div id="ch2-7" />

### 2.7 Expressions
- literal value: eg: string or number
- invocation expression: eg: `()` with parentheses
- refinement expression
- ternary `?` operator, then by `:`

Note: [operator precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence) 

<div id="ch2-8" />

### 2.8 Literals
- Object literal: a easy way for new objects: using `{}`	
	 Similar like k-v pairs
	 - literal name: string
	 -  literal value: expressions
	
- Array literal:  `[]`
- Function literal: an **expression** rather than function statement to define a function value.
	For example: syntax
	```js
	var myFunc = function() {
		// function body
	}
	// func name is optional
	var myFunc = function fnName() {
		 ...
	}
	```


