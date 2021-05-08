## This keyword in JavaScript

#### I. [this in function declaration](#p1)  

#### II. [this in Arrow Function](#p2)  

#### III. [Init Map function](#p3)

#### IV. [this in Nested functions](#p4) 

#### V. [this in Closure's returned functions](#p5)

#### VI. [method call another method inside same object/class - this](#p6)

#### VII [method creates another class/object method - this](#p7)

<div id="p1" />

### I. this in function declaration
Most common use-case, **function declaration has "this"** keyword in its "function-based" scope, can access "this" keyword inside of local   object.
```js
const obj =  {
	dev:  'bfe',
	a:  function()  {
		return  this.dev
	},
};
console.log(obj.a()); // "bfe"
``` 

[**ES shorthand**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#method_definitions) for method declared inside of the object, still HAVE access to "this" keyword, it's the same as function declaration.
```js
const obj =  {
	dev:  'bfe',
	a()  {
		return  this.dev
	},
}
console.log(obj.a()); // "bfe"
```

<div id="p2" />

### II. this in Arrow Function

Docs: [arrow functions -MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#arrow_functions_used_as_methods)

- NO "this" keyword 
- have a lexical scope, share surround's "this" keyword

```js
const obj =  {
	dev:  'bfe',
	a: () => {
		return  this.dev
	},
};
console.log(obj.a()); // undefined
```

<div id="p3" />

### III. this in IIFE block

**Docs:** [stackoverflow](https://stackoverflow.com/questions/37481022/why-iife-this-keyword-refers-to-window-object/37481344)

IIFE has its own data privacy, **"this"** points to where it's been called. the place IIFE is written decides who call it.
- function declaration IIFE
	```js
	const obj =  {
		dev:  'bfe',
		a:  function()  {
			return  (function(){
				// this -> window object
				return  this.dev
			})();
		},
	};
	console.log(obj.a()); // undefined
	```
- arrow function IFFE
	```js
	const obj =  {
		dev:  'bfe',
		a:  function()  {
		    // this -> obj
			return  (()  =>  {
				// this -> surrounding this obj
				return  this.dev
			})();
		},
	};
	console.log(obj.a()); // "bfe"
	```

<div id="p4" />

### IV. this in Nested functions

#### 4.1 nested function declaration
-   regular function call: points to the  **global object**  (the window object, in the browser)
-   Method call: this points to the  **object that calls the method**
-   `'this'`  keyword is not assigned a value until the function is really being executed and being called!!!!
```js
const obj = {
	dev: "bfe",
	a:  function() {
		  function innerFunc(){ 
			  // this -> window object
			  console.log(this.dev);
		  }
		  innerFunc(); 
	},
};
obj.a()(); // undefined
```

#### 4.2 nested Arrow Function
It depends on the surrounding lexical "this" keyword, the same that arrow function can access surrounding "this".
```js
const obj = {
	dev: "bfe",
	a:  function()  {
		 // this -> obj
		return  ()  =>  {
		    // this -> obj
			return  this.dev;
		}
	},
};
console.log(obj.a()()); // "bfe"
```

<div id="p5" />

### V. this in Closure's returned functions

#### 5.1 return a function declaration

It needs to bind "this", otherwise, it's just a **regular function call**, not an object's call.
```js
const obj = {
	dev: "bfe",
	b:  function() {
		return  this.dev;
	},
	a: function () {
		return this.b;
	}
};
console.log(obj.a()()); // undefined
```
How to fix it? -- manually bind
```js
{
	a: function () {
		// this -> obj
		return this.b.bind(this);
	}
}
console.log(obj.a()()); // "bfe"
```

#### 5.2 return an arrow function 
Under this situation, same to the returned function declaration.

Even it's an arrow function,  the **returned arrow function** is decided by the surrounding that call it. **If not bind, the window object will call it regularly.**  

```js
const obj = {
	dev: "bfe",
	b: () => {
		// no "this"
		return  this.dev;
	},
	a: function () {
		return this.b;
	}
};
console.log(obj.a()()); // undefined
```

**How to fix it?** -- CANNOT

[stackoverflow - link](https://stackoverflow.com/posts/33308151/timeline)

You can NOT  _rebind_  `this`  in an arrow function.
```js
// wrong !!!!
{
	b: () => {
		// no "this"
		return  this.dev;
	},
	a: function () {
		// Wrong !!! 
		return this.b.bind(this);
	}
}
```

<div id="p6" />

### VI. method call another method inside same object/class - this

#### 6.1 call another function declaration

```js
const obj = {
	dev: "bfe",
	b: function () {
		// this -> obj
		return  this.dev;
	},
	a: function () {
		return this.b();
	}
};
console.log(obj.a()); // "bfe"
```
This is execute & calling another method in same object, it equals to the following: 
```js
{
	a: function () {
		// this -> obj
		var result = this.b();  // object.b()
		return result; // "bfe"
	}
}
```

#### 6.2 call another arrow-function method

```js
const obj = {
	dev: "bfe",
	b: () => {
		// no this
		return  this.dev;
	},
	a: function () {
		return this.b();
	}
};
console.log(obj.a()); // undefined
```
it equals to the following: 
```js
{
	a: function () {
		// this -> obj
		var result = this.b();  // object.b()
		return result; // undefined
	}
}
```
But here since arrow function has NO "this", when `object.b()` return **"undefined"** .

<div id="p7" />

### VII. method creates another class/object method - this
create and define a new class method inside same object/class.
```js
const obj = {
	dev: "bfe",
	a: function () { 
		this.c = function()
		{
			// this -> obj
			return this.dev;
		}; 
		return this.c(); 
	}
}; 
console.log(obj.a());  // "bfe"
```
It equals that define a new method , and call another method in the same class/object.
```js
const obj = {
	dev: "bfe",
	a: function () { 
		return this.c(); 
	},
	c: function() {
		// this -> obj
		return this.dev;
	}; 
}; 
console.log(obj.a());  // "bfe"
```