## Execution Context vs Lexical Scope 
**I.** [Execution Context](#execution-context)

**II.** [Scoping And Scope Chain](#scoping-and-scope-chain)

**III.** [Compare Scope Vs Execution Context](#compare-scope-vs-execution-context)

**IV.** [More About  Hoisting Examples](#more-about-hoisting-examples)

**V.** [More About This Keyword](#more-about-this-keyword)

**VI.** [Context And Scope For Arrow Functions](#context-and-scope-for-arrow-functions)


### Execution Context

we can associate execution context with a big **object** that owns the current executing code. every time a new function is being called, it creates a new execution context on the stack.

#### 1. Creation Phase

It does three things in creation phase:
#### 1.1  create a **variable object(VO)**: 
it contains `arguments` and `function declarations`, not ~~"function expressions"~~.
* 1 ) argument object is created

*  **2) Hoisting**: parser also scanning for **function declarations, variable declarations**

	- variable is created, but set to **undefined**

	- function declarations: reference to it created, but **inner code not parsed and assigned** until it got executed during the run time. That means **it does not compile nested function declarations**.

	**Atten**: ~~let and const in ES6 will not be hoisted~~

#### 1.2 create the  **scope chain** 

#### 1.3 create  **'this'** variable 
"this" is created to reference to the current object that the method is called on, also it means the value of current execution context.
	

#### 2. Execution Phase

### Scoping And Scope Chain

1 ) Scope is **function-based**, and it's the space/environment where the variable defined in it can be accessible or not.

2 ) **Lexical Scope**: a function that is lexically inside of another function always have access to the scope of outer/parent function.

For example:

```
function first()
{
	second();
	function second()
	{
		third();
		function third()
		{
			fourth();
			function fourth()
			{
				// todo
			}
		}
	}
}
```

So the scope chain will be:

**fourth -> third -> second -> first -> global-object**

The **order of the scope chain** is decided by which functions are **written in our code**, the written position lexically in our code.

  

### Compare Scope Vs Execution Context

#### 1. Different things!!!
* execution context that store the scope chain of each function in the variable-object(VO), but they do not have an effect on the scope chain itself.

* execution-context order is decided by which functions are called

* scope-chain order is decided by which functions are written lexcially in our code.


#### 2. Error Example: ReferenceError for not defined variable

```
first();
var a = 'a';
function first() {
	second();
	var b = 'b';
	function second(){
		var c = 'c';
		third();
	}
}
function third() {
	console.log(a); // Good!
	console.log(c); // Uncaught ReferenceError: c is not defined
}
```

#### 3. Summary things happened in the above code:

* second() can call third() because of scope-chain

* third() can access **'a' varaible** because of **lexical written** position, it can access the outer(global-object) scope.

*  **Not Defined Reference Error**: because execution context is not the scope-chain, the **order of calling the function not matter**, the **accessible of variable** is **invalid lexical scope**, then it failed!

### More About Hoisting Examples

**Example 1**: **Correct**: **function declaration** use it before declare it.
```
first(); // already available to use in our VO
// later to delcare it
function first(){
	// todo
}
```

**Example 2**: **Wrong**: **function expression** will not be hoisted

```
first(); // this function expression is not hoisted in VO
var first = function(){
	// todo
};
```

**Example 3**: **Correct**: **variable** will be set to `undefined`

```
console.log(name); // undefined, not an error
var name = 'jelly';
console.log(name); // 'jelly'
```

**Example 4**: **hoisting works the same on different execution context object !!!**

Because every function creates a new execution context, then it has its own context object, scope-chain and its own "this" keyword, which is the same workflow like global-object.

* For example 4.4.1: function declarations:
```
function first() {
	second(); // hoisted in current execution context of "first"
	function second(){
		// todo
	}
}
```
* **For example 4.4.2:  Hoisted on Variables**
```
var name = "John"; // global varaible
function first() {
	// Because it hoisted again on the same namespace var
	console.log(name); // undefined, but not an error
	var name = 'jelly';
	console.log(name); // 'jelly'
}
console.log(name); // 'John'
```
  

### More About This Keyword

* regular function call: points to the **global object** (the window object, in the browser)

* Method call: this points to the **object that calls the method**

*  `'this'` keyword is not assigned a value until the function is really being executed and being called!!!!

For example:
```
var obj = {
	name: 'jelly',
	calculate: function(){
		console.log(this); // points to "obj"
		innerFunc();
		function innerFunc(){
			// points to "global object": window object
			console.log(this);
		}
	}
};
obj.calculate();
```

the inner function is not bind or set to certain object, then it points to the window object.

After we bind this inner function, it will also points to the "obj":

```
var obj = {
	name: 'jelly',
	calculate: function(){
		innerFunc.bind(this); // we bind to "obj" object
		function innerFunc(){
			console.log(this); // points to "obj"
		}
	}
};
```

### Context And Scope For Arrow Functions

Important to arrow functions:

* arrow function does **not have "this"** keyword

* they share surrounding "this" keyword, so they have a **lexical "this" keyword**

How it shares the surrounding lexical "this" keyword? Some examples here:

**6.1 Example 1**: share "this" to local certain object

```
var obj = {
	name: 'jelly',
	calculate: function(){
		// this: points to 'obj'
		const innerFunc = ()=>{
		console.log(this.name); // 'jelly': have access
		}
		innerFunc();
	}
};
```
**6.2 Example 2**: share "this" to global object

```
var obj = {
	name: 'jelly',
	calculate:() => {
		// this: points to global object
		const innerFunc = ()=>{
		console.log(this.name); // undefined: no access
		}
		innerFunc();
	}
};
```

**6.3 Example 3**: Use case in **map() method to access "this.properties"**

if we use arrow function in map() methods, we can easily have access to **'this'** object's any properties.

```
Object.prototype.myFunction = function(array){
const result = array.map(el=>{
	return ''+this.name; // have access
	})
}

```

Translate into old function(), we need to manually **bind()** the inner function like before in ES5. The above code **equals** the following in old code:

```
Object.prototype.myFunction = function(array){
	const result = array.map(function(el) {
	return ''+this.name;
	}.bind(this));
}
```
