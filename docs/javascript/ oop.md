## OOP
Objected-Oriented Programming languages like:  **#C, python, ruby, Java**, it's about objects and relationships.

### 1. Constructor Functions
```
function  Person(name,  age)  {
	this.name = name;
	this.age = age;
}
```
How to use this constructor function to create new objects?
**Caution: wrong use**
```
const p1 =  Elf('peter',  'fire');
console.log(p1.name);  // Cannot read property 'name' of undefined
```
**Correct use** constructor function: have to use **new** keyword:
`new` calls the function constructor:
```
const p2 =  new Elf('peter',  'fire');
console.log(p2.name); // 'peter' 
```

**Rule**:  All constructor functions should start with a **capital letter**.

### 2. native new Function
The syntax for creating a function:
```
let func = new Function ([arg1, arg2, ...argN], functionBody);
let result =  new  Function('a',  'b',  'return a + b');
```

### 3.  add method to object use prototype
For example:
```
Person.prototype.getName  =  function()  {
	return  'my name is '  +  this.name;
};
```
Edge case about nested function in object's method to access **"this"** keyword.
Bug: nested function **"this"** points to **global object**.
```
Person.prototype.build  =  function()  {
	function  getName()  {
		return    'I am ' + this.name; // wrong
	}
	return  getName();
};
jelly.build();  // undefined
```
**Solution 1: use bind()**
```
Person.prototype.build  =  function()  {
	function  getName()  {
		return    'I am ' + this.name; // wrong
	}
	return  getName.bind(this)();
};
```
**Solution 2: take advantage of closure to access "this" in parent function**
```
Person.prototype.build  =  function()  {
	const self = this;
	function  getName()  {
		return  'I am ' + self.name; // because of closure
	}
	return  getName();
};
```

### 4. ES6: Classes
Syntax:
* class keyword
* constructor() function to init
* method/function property:  one place in memory that every instance can access, memory efficient
```
class  Person  {
	constructor()  {
		// create in memory for every instance
	}
	myFunc()  {
		// one function in one place memory that every instance can access
	}
}
```
### 5. Inheritance
1 ) cloned object doesn't clone the inheritence, because they didn't point to the same memory place.
```
const f1 =  new Person('a',  1);
const f2 =  {...f1};
f1.__proto__;  // Person{}
f2.__proto__;  // empty {}
```
2 ) class extend class
```
class  Designer  extends  Person  {
}
const designer = new Designer();
```
Instance relationship:
* **designer instanceof Designer;** // true
* **designer instanceof Person**; // true

Prototype relationship:
* **Designer.prototype.isPrototypeOf(designer)**;  // true
* **Person.prototype.isPrototypeOf(Designer.prototype)**;  // true

### 6. private vs public 
* private variable cannot be accessed
	- `#age`: private varaible, only inside of the object
* but current method() cannot be private

### 7. OOP - principles
* Encapsulation
	wrap code into boxes, more reusable
* Abstraction
	hiding the complexity from the usage, extend classes, inheritence
* inheritance
* polymorphism: same function in different arguments
  ```
  test(a, b);
  test();
  ```

