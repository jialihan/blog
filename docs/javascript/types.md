## Types in JavaScript

### 1. Premitives and Non-premitives
Primitives:
1.  typeof 5:  **'number'**
2. typeof 'abcdef': **'string'**
3. typeof true: **'boolean'**
4. typeof undefined: **'undefined'**: absence of definition
5. typeof null: **'object'**:  absence of value
6. ES6 new type: typeof Symbol('unique') : **'symbol'** , [Symbol- docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)

Non-premitives: (note: functions and Arrays are objects )
* typeof {}: **'object'**
* typeof []: **'object'**
* typeof function(){}: **'function'**, but it's just an **object**

More references on objects: [Reference/Global_Objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)

**edge case**:
accessing a property on a primitive:
```
true.toString(); // => 'true'
// behind the scenes: like a wrapper
Boolean(true).toString(); // =>'true'
```

### 2. Array
* `Array.isArray()` : [Array/isArray](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray)
 eg: 
	Array.isArray([1, 2, 3]);  // true
	Array.isArray({foo: 123}); // false
	Array.isArray('foobar');   // false

### 3. Pass by Value vs. Pass by Reference
* premitives are pass by value
* object(**objects, functions, arrays**): pass by refrence

If clone and create a new object, two ways:(**shallow clone**)
* const newObj = **Object.assign**({}, oldObject);
* **spread operator** in ES6: const newObj = {...oldObj};
A tricky part to **deep clone** an object:
	```
	const newObj = JSON.parse(JSON.stringify(oldObj));
	```
### 4. Type Coercion
For example: 
` 1 == '1' => true`
` 1 === '1' => false`

And also type coercion in `if` statement:
`if(1) => true`
`if(0) => false`

Some edge cases: compare using `is`, `==`, `===`
more on documents: [Equality_comparisons_and_sameness](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)
For example:
` NaN === NaN => false`
` NaN == NaN => false`
` Object.is(NaN,NaN) => true`

More examples:
```
false  ==  ""	// true
false  ==  []	// true
false  ==  {}	// false
""  ==  0	 // true
""  ==  []	 // true
""  ==  {}	 // false
0  ==  []	 // true
0  ==  {}	 // false
0  ==  null  // false  
```
