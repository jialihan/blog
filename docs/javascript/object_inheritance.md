## Object and Inheritance in Javascript

### 1. prototype and object inheritance
The prototype property of an object is where we  put methods and properties that we want **other objects to inherit, but not itself**.
For example:
* Person Object -> Prototype
* Jennie inherits  Person: then Jennie can access Person object' **prototype** in Jennie's **prototype property**.
```
Person.prototype.calculateAge = function(){......};
var jennie = new Person();
jennie.calculateAge();
```
**prototype chain**
When a certain method or peroperty is called, it starts searching in itself, if it's not found, the search moves on to object's prototype. This continues until the method is found.


### 2. create object use function constructor
#### 2.1 use JavaScript object literal
```
var john =  {
	name:  'Jelly',
	yearOfBirth:  1994,
	job:  'dancer'
};
```
#### 2.2 usual convention: use function constructor
```
var  Person  =  function(name,  yearOfBirth,  job)  {
	this.name = name;
	this.yearOfBirth = yearOfBirth;
	this.job = job;
};
```
create an instance: use `new` key word, a brand new empty object is created, and the constructor  function `Person()` is called, it will create a new execution context, and that it points the `this` key word to our newly-created object by using the `new` keyword.
```
var jennie = new Person('Jennie',1994, 'dancer');
```

#### 2.3 add a function to the object
* use function constructor
	```
	var Person = function(name, yearOfBirth, job)  {
	this.calculateAge = function() {
		console.log(2018 - this.yearOfBirth);
	};
	```
* use prototype
	```
	Person.prototype.calAge =  this.calAge  =  function()  {
		console.log(2018  -  this.yearOfBirth);
	};
	```
	we can also attach property to the prototype for other instance to access:
	```
	Person.prototype.lastName = 'Han';
	```
* use-case in instances
	```
	jennie.calculateAge();
	var name = jennie.lastName;
	```
### 3. create object that inherits from prototype: Object.create
* First define a prototype ( this is also an object) object:
	```
	var personProto =  {
		calulateAge: function()  {
		console.log(2018  -  this.yearOfBirth);
		}
	};
	```
* create instance object:
	```
	var jennie = Object.create(personProto);
  ```
 
 * use second argument: an object that contains data we want to put into the object.
	 ```
	 var jennie =  Object.create(personProto,  {
		name:  { value:  'jane'  },
		yearOfBirth:  { value:  1980  },
		job:  { value:  'designer'  }
	});
	```
* Reference:
[Object/create](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

### 4. primitives vs. objects
* Primitives: `string, number, boolean, undefined, null`
   Primitives are passing by value, for example:
   ```
   var a = 1;
   var b = a;
   a = 2;
   // but b is still 1
  ```
	 When we pass a primitive into a function, it will never affect the varaible outside, while we can make any change inside the function. For example:
	 ```
	 var a = 1;
	 function change(a)
	 {
		 a = 2;
	 }
	 change(a);
	 console.log(a); // still 1, not changed
	 ```
 * object: pass by reference
   ```
   var obj2 = obj1;
   obj1.age = 18;
   // then obje2.age = 18, it will also change
   ```

### Method shorthand(ES6)
There exists a shorter syntax for methods in an object literal:
Original(ES6):
```
const obj = {
	name: 'jelly',
	getName: function {
		return this.name;
	}
}
```
It's the same with the shorthand written syntax:

```
const obj = {
	name: 'jelly',
	getName() {
		return this.name;
	}
}
// usage
obj.getName();
```
Note: attention: since **arrow function has no "this" keyword**, 
if we want to easliy access and make use of **"return this.name;"**,
then we just use `function(){}` syntax.
**Wrong** code:
```
const obj = {
	name: 'jelly',
	getName: ()=> {
		return this.name; // wrong, cannot access this.name
	}
}
```

### 6. Working with functions
* store functions in a variable
* pass a function as an argument to another function
* return a function from a function