## Execution Context & Scope & Hoisting

### 1. execution context
we can associate execution context with a big **object** that owns the current executing code. every time a new function is being called, it creates a new execution context on the stack.
#### 1.1 It has  three properties:
* create a **variable object(VO)**: contains `arguments` and `function declarations`, **not function expressions**.
* **scope chain** is initialized
* **'this'** variable created to reference to the current object that the method is called on, also it means the value of current execution context.
#### 1.2  it has two phase
* create phase
	- variable object
	- scope chain
	- "this" keyword
* execution phase


### 2. Phase 1: variable object
* 1 ) argument object is created
* **Hoisting**:  parser also scanning for **function declarations, variable declarations**
	- 2 ) variable is created, but set to **undefined**
	- 3 ) function declarations: reference to it created, but **inner code not parsed and assigned** until it got executed during the run time. That means **it does not compile nested function declarations**.

### 2.1 Phase 1: Hoisting Examples
**Example 1**: **Correct**: **function declaration** use it before declare it
```
first(); // already available to use in our VO 
// later to delcare it
function first(){
	...
}
```
**Example 2**: **Wrong**: **function expression** will not be hoisted
```
first(); // this function expression is not hoisted in VO
var first = function(){
	...
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
* For example: function declarations:
	```
	function first() {
	  second(); // hoisted in current execution context of "first"
	  function second(){
	    ...
	  } 
	}
	```
* For example: variables
	```
	function first() {
		console.log(name); // undefined, but not an error
		var name = 'jelly'; 
		console.log(name); // 'jelly'
	}
	```

### 3. Phase 1: Scoping and Scope chain
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
				// TODO: ...
			}  
		}  
	}  
} 
```
So the scope chain will be:
**fourth ->  third -> second -> first -> global-object**
The **order of the scope chain** is decided by which functions are **written in our code**, the written position lexically in our code.

#### 3.1 compare scope-chain and execution context:
Different things!!!
* execution context that store the scope chain of each function in the variable-object(VO), but they do not have an effect on the scope chain itself.
* execution-context order is decided by which functions are called
* scope-chain order is decided by which functions are written lexcially in our code.

#### 3.2 Error Example: ReferenceError for not defined variable
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
Summary things happened in the above code:
* second() can call third() because of scope-chain
* third() can access **'a' varaible** because of **lexical written** position, it can access the outer(global-object) scope.
* **Not Defined Reference Error**: because execution context is not the scope-chain, the **order of calling the function not matter**, the **accessible of variable** is **invalid lexical scope**, then it failed!

### 4. Phase 1: 'this' keyword
* regular function call: points to the **global object** (the window object, in the browser)
* Method call: this points to the **object that calls the method**
* `'this'` keyword is not assigned a value until the function is really being executed and being called!!!!

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

### 4. Context and Scope in Arrow Function
Important to arrow functions:
* arrow function does **not have "this"** keyword
* they share surrounding "this" keyword, so they have a **lexical "this" keyword**
How it shares the surrounding lexical "this" keyword? Some examples here:
**Example 1**: share "this" to local certain object
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

**Example 2**: share "this" to global object
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
**Example 3**: Use case in **map() method to access "this.properties"**
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