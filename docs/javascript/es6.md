## New features in ES6 compared to ES5
###  Variable Declaration
* **ES5**:  `var`: mutable
	```
	var x;
	x = 'hello';
	x = 5;
	x = true;
	```
* **ES6**:  `let, const`
	```
	// constants: must have a inital value
    const x = 0;
    // mutable variables
    let x;
    x = 'hello';
    x = 5;
    x = true;
	```
###  Data  Privacy
* **ES5**:  `IIEF`
	 use braces () to wrap an function and execute it immediately. 
	 [Immediately Invoked Function Expression](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)
	```
	(function() {
	    var c = 3;
	 })();
	```
* **ES6**:  `Blocks {}`
		use curly bracket {} as a block,  outside have no access. variables exist only within the corresponding block.
	```
	{
	  const a = 1;
	  let b = 2;
	  var c = 3;
	}
	```
### String improvement
* **ES5**:  `+` sign to concat strings
	```
	var str = a + ' , ' + b;
	```
* **ES6**:  backtick
		use backtick `` ` ``, use `${}` to wrap a variable.
	```
	var a = 1;
	const str = ` ${a}, ${b}`;
	```

### Arrow Function
* **ES5**:  function keyword
	```
	// function declaration
	function func_name (args) {
	}
	// function expression
	var myFunc = function(args){};
	```
* **ES6**:  Arrow Function
		Reference docs : [MDN arrow function]([https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions))
	```
	(arg) => online
	(args) => {
	    //  multiple lines
	}
	```
### Destructuring

* **ES5**:  None

* **ES6**
	1. destructure from Array
	```
	const [ name, age ] = [ 'john', 26 ];
	```
	2. destructure from Object
	use the exact same key name to destructure and you can also declare a **new name** for this key.
	```
	const obj = { 
	    name: 'John',
	    age: 29
	};
	const { name : firstName,  age } = obj;
	```
### Arrays
####  1. Transform iterable object into Arrays
**ES5**: using prototype method to transform the array
```
var arr = Array.prototype.slice.call(obj);
```
**ES6**:  use `from()` method
The `Array.from()` static method creates a new, shallow-copied `Array` instance from an array-like or iterable object.([MDN reference link]([https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)))
```
 const arr = Array.from(obj);
```
####  2. New ways of looping in arrays
**ES5**:  
```
for (var i = 0; i < arry.length; i++) {
}
```
**ES6**:  use `of` key word in for loop
```
for (const element of array) {
}
```


### Spread Operator
**ES5**:   None

**ES6**:  spread operator: `...`
1) spread an Array
	```
	const a = [1,2];
	const b = [...a, 3];
	```
2) Spread an Object
   if the same key name in the new object, it will override the previous old value. 
	```
	const newObj = {...obj1, ...obj2};
	```

### Rest Parameters and Default Parameters
1. Rest parameters
Define arbitary numbers of arguments, using spread operators, then get them from an array. The **rest parameter** syntax allows us to represent an indefinite number of arguments as an array.
	```
	function myFunc(…args) {
	   let arg0 = args[0];
	   let arg1 = args[1];
	   ...
	}
	```
2. Default parameters
allow named parameters to be initialized with default values if no value or `undefined` is passed. [Default function parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)
	```
	function myFunc(a = 1, b)
	{
	}
	```

### Maps
1. declare a map object
	```
	const map = new Map();
	```
2. common methods:
* `set`: map.set('key', 'value');  // set  k-v pair
* `get`: const value = map.get('key); // get value
* `size`: map.size;
* `delete`: map.delete('key');  
* `has`: map.has('key'); // true or false
* `clear`: map.clear(); // clear all k-v pairs
3.  loop in a map
      [forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/forEach),  [map.entries]([https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/entries](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/entries))
	```
	map.forEach( (val, key) => { } );

	for (let [key, val] of map.entries() ) { }
	```
### Class and SubClass
1. Constructor
* **ES5**:  Function Constructor
	Example:
	```
	var Person = function(name, age) {
		 this.name = name;
		 this.age = age;
	};
	```
* **ES6**: Class Constructor
	```
	class Person {
		constructor(name, age) {
		   this.name = name;
		   this.age = age;
		}
	```
2. Add a method to class
* **ES5**:  Use prototype
	Example:
	```
	className.prototype.myFunc = function() { };
	```
* **ES6**: Class Constructor
attach to the class directly can have instance method and static method.
	```
	class Person {
	    constructor() {
	    }
	    myFunc {
	    }
	    static myFunc2 {.  
	    }
	}
	```
	More about instance method and static method:
	[MDN Javascript Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
	