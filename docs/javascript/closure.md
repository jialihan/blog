## Closure in JavaScript
* it's a combination of  **function and lexical scope**
* Closures allow a function to access variables from an enclosing scope or environment even after it leaves the original scope in which it was declared.

For example:
 ```
function PlusTwo() {
	  let x = 2;
	  return function(y) {
	    return x + y;
	  };
}
 ```


### I. functions are first class citizens in JS
1. functions are also objects
*  can be assigned to variables 
	```
	const myFunc = function(){...}
	```
* can be assigned as methods in object: [Method_definitions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Method_definitions)
	```
	const obj = {
		foo(){
			return 'hello world';
		}
	}
	// call the method
	obj.foo();
	```
2. function can be passed as arguments into another function
	```
	function a(fn)
	{
		fn();
	}
	```
3.  function can be returned from another function
	```
	function a()
	{
		return function fn(){...};
	}
	// usage
	const myFn = a();
	```
Some special cases:
* if function declaration inside a for loop, it will initialize the function in memory in every loop.
	```
	for(i = 0; i <n;i++)
	{
		function a(){...}
		a();
	}
	```
* pay attention to **referenceError** inside of a function, not accessible to some parameters. Better to have default parameters.

### II. Higher order function
A function that takes another function as argument or returns another function.

### III. closure example
difference from `hoisting`:
```
function  callMeMaybe()  {
	// goes into WEB API be put on the callback queue
	setTimeout(function()  {
	console.log(callMe);
	},  4000);
	// const variable not hoisting!!!
	const callMe =  'Hi! I am call back!';
}
callMeMaybe();
```
* when the event loop pushes it back onto the stack
* at that time when we already run this function, `const: callMe` has already been created and assigned.

### IV. closure: memory efficient
When we access variable from the closure, we won't directly create that variable again and again because we are using the returned function from the closure.
1.  memory cost when every function call to return the data
	```
	function first(index){
	const bigArray =  new Array(7000).fill('1'); // created and garbaged 
	return bigArray[index];
	}
	// usage
	first(677);
	first(677);
	first(677);
	```
	then the `bigArray`  have been created and garbaged 3 times.
2. instead, we use the function returned from the closure to make memory efficient.
	```
	function second(){
		const bigArray =  new Array(7000).fill('1');
		return  (index)=>{
			return bigArray[index];
		};
	}
	const getData =  second(); // bigArray created only once
	getData(677);
	getData(677);
	getData(677);
	```
	Then we use the returned function to call many times, they will have direct access to their parent function's variable, stored in that closure.

### V. closure in for loop
1. ES5: scope is function-based, use IIFE to ensure data privacy.
```
for (var i =  0; i < 5; i++) {
	(function(index)  {
		setTimeout(function()  {
		console.log('I am at index '  + index);
		},  3000);
	})(i);
}
```
2. ES6:  if we use `let`, it's **{} block-scoped**
```
for (let i =  0; i < 5; i++) {
{   
	// scope created here
	// after 3s, "i" value still kept
	setTimeout(function()  {
		console.log('I am at index '  + i);
	},  3000);
}
```

