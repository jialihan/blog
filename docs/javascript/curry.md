## Curry and Partials in JavaScript

#### I. [What is Curry & Partials ?](#p1)  

#### II. [Advanced Curry Implementation](#p2)  

#### III. [Implement Curry with Placeholder](#p3)

<div id="p1" />

### I. What is Curry & Partials ?

Docs:
[https://javascript.info/currying-partials](https://javascript.info/currying-partials)
[https://lodash.com/docs/4.17.15#curry](https://lodash.com/docs/4.17.15#curry)

**Currying** is a transformation of functions that translates a function from callable as `f(a, b, c)` into callable as `f(a)(b)(c)`.

**Partials:** ahead binding some parameters, then return the new function with some parameter already set there.

**Examples:**
```js
// curry
var  curriedMutiply = (a)=>(b)=>a*b;
var  curriedMutiplyBy2 = curriedMutiply(2);

// partials
var  multiply = (a,b,c)=>a*b*c;
var  partialMultiplyBy2 = multiply.bind(null, 2);
multiply(2,3,4); // 24
partialMultiplyBy2(3,4); // 24
```

<div id="p2" />

### II. Advanced Curry Implementation

**Docs:** https://javascript.info/currying-partials#advanced-curry-implementation

**Problem:**

link: [bfe 1.](https://bigfrontend.dev/problem/implement-curry)
[Currying](https://en.wikipedia.org/wiki/Currying)  is a useful technique used in JavaScript applications.
Please implement a  `curry()`  function, which accepts a function and return a curried one.

Here is an example
```js
const join = (a, b, c) => {
   return `${a}_${b}_${c}`
};
const curriedJoin = curry(join);

curriedJoin(1, 2, 3) // '1_2_3'
curriedJoin(1)(2, 3) // '1_2_3'
curriedJoin(1, 2)(3) // '1_2_3'
```

**Steps:**
- 1. when arguments length enough, just call it
- 2. when arguments is less than expected, curry it (recursive call it): [Function.length](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/length)

**Solution 1: use call()**
```js
function curry(fn)  {
	return  function curriedFn(...args){
		if(arguments.length >= fn.length)
		{
			return fn(...args);
		}
		else
		{
			return  function(...args2){
				return curriedFn.call(null,  ...args,  ...args2);
			}
		}
	}
}
```

**Solution 2: use apply()**
```js
function curry(fn)  {
	return  function curriedFn(...args){
		if(arguments.length >= fn.length)
		{
			return fn.apply(this, args);
		}
		else
		{
			return  function(...args2){
				return curried.apply(this, args.concat(args2));
			}
		}
	}
}
```

**Solution 3: use bind() - partials**
```js
function curry(fn)  {
	return  function curried(...args)  {
		if  (args.length >= fn.length)  {
			return fn(...args);
		} 
		else {
			return curried.bind(this,  ...args);
		}
	};
}
```

**Solution 4: use recursion**
```js
function curry(fn)  {
	return  function curried(...args)  {
		if  (args.length >= fn.length)  {
			return fn(...args);
		}  
		return  function(...args2)  {
			return curried(...args,  ...args2);
		}
	};
}
```
**Simplified** Recursion using **arrow function**:
```js
function curry(fn)  {
	return  function curried(...args)  {
		if  (args.length >= fn.length)  {
			return fn(...args);
		}
		return  (...args2)  => curried(...args,  ...args2);
	};
}
```

<div id="p3" />

### III. Usage Examples

**Problem:** 
link: [bfe 2.](https://bigfrontend.dev/problem/implement-curry-with-placeholder)
please implement  `curry()`  which also supports placeholder.
Here is an example
```js
const  join = (a, b, c) => {
   return `${a}_${b}_${c}`
}
const curriedJoin = curry(join)
const _ = curry.placeholder

curriedJoin(1, 2, 3) // '1_2_3'
curriedJoin(_, 2)(1, 3) // '1_2_3'
curriedJoin(_, _, _)(1)(_, 3)(2) // '1_2_3'
```

more to read
[https://javascript.info/currying-partials](https://javascript.info/currying-partials)
[https://lodash.com/docs/4.17.15#curry](https://lodash.com/docs/4.17.15#curry)
[https://github.com/planttheidea/curriable](https://github.com/planttheidea/curriable)

**Steps:**
-   next call params will take turns to  **replace previous placeholder**, eg: when you call with  `(_, 3 ,4)`  at first, then next call with  `(1,2)`, replace  **'_'**  with  **1**,
-   then  **merge remaining args**  in the next calls, eg: remaining is  **2**, then merge it, final args are  `(1,3,4,2)`
-   when longer params and NO placeholders, directly return the  `fn(...args)`

**Solution JS code:**
```js
function curry(fn) {
  return function curried (...args) {
    const newArgs = args.slice(0, fn.length);
    if (args.length >= fn.length && newArgs.indexOf(curry.placeholder) === - 1) {
      // if enough length && no placeholder, just call the function
      return fn(...args);
    }
    return (...args2) => {
	  // replace placeholder with new args2
      var updatedArgs = args.map(el=>{
        if(el === curry.placeholder && args2.length > 0)
        {
            return args2.shift();
        }
        return el;
      });
      return curried.apply(null, updatedArgs.concat(args2));
    }
  }
}
curry.placeholder = Symbol()
```