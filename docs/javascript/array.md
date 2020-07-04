### Array Manipulation

### Array.prototype
The easiest way to have a full  scope of what methods array can do, we just need to open the browser's console, and type:
```
Array.prototype
```
then we can see all the methods that array instances can use.
```
[constructor: ƒ, concat: ƒ, copyWithin: ƒ, fill: ƒ, find: ƒ, …]
  concat:  ƒ concat()
  constructor:  ƒ Array()
  copyWithin:  ƒ copyWithin()
  entries:  ƒ entries()
  every:  ƒ every()
  fill:  ƒ fill()
  filter:  ƒ filter()
  find:  ƒ find()
  findIndex:  ƒ findIndex()
  flat:  ƒ flat()
  flatMap:  ƒ flatMap()
  forEach:  ƒ forEach()
  includes:  ƒ includes()
  indexOf:  ƒ indexOf()
  join:  ƒ join()
  keys:  ƒ keys()
  lastIndexOf:  ƒ lastIndexOf()
  length:  0
  map:  ƒ map()
  pop:  ƒ pop()
  push:  ƒ push()
  reduce:  ƒ reduce()
  reduceRight:  ƒ reduceRight()
  reverse:  ƒ reverse()
  shift:  ƒ shift()
  slice:  ƒ slice()
  some:  ƒ some()
  sort:  ƒ sort()
  splice:  ƒ splice()
  toLocaleString:  ƒ toLocaleString()
  toString:  ƒ toString()
  unshift:  ƒ unshift()
  values:  ƒ values()
  Symbol(Symbol.iterator):  ƒ values()
  Symbol(Symbol.unscopables):  {copyWithin:  true,  entries:  true,  fill:  true,  find:  true,  findIndex:  true, …}
```

### Splice
1. Syntax
The splice() method adds/removes items to/from an array, and returns the removed item(s). Note: `This method changes the original array`.
	```
	[].splice(index, howmany, item1, ....., itemN)
	```
	parameters:
	* `index`: required. the position to add/remove items,  `negative values` to specify the position from the end of the array.
	* `howmany`: optional, how many items to be removed. if `0`, none will be removed! 
	* `newItems`:  optional. The new item(s) to be added to the array.
3.  Use case
```
var arr = [1,2,3,4,5];
arr.splice(1); // [1], remove from index 1 to the end
arr.splice(1,1); // [1,3,4,5], only remove 1 element from index 1
arr.splice(1,1,'new1', 'new2'); // [1,'new1','new2',3,4,5], remove 2, add new two items
```
### Slice
1. Syntax:
slice() method returns the selected elements in an `new array`. [start,end), end index is not included.
	```
	[].slice(start, end)
	```
* if start is omitted,  default value is from index `0`
* if end is omitted, all elements from start until the end of the array
2. Use case
*  use `[].slice()` to clone an array, return a new array object.
 * normal usage:
	 ```
	 var arr = [1,2,3];
	 arr.slice(1); // select index 1 to the end, [2,3]
	 arr.slice(-1); // select last element, [3]
	 arr.slice(0,2); // select index from 0 to 1, [1,2]
	```

###  Pop, Push, Shift and Unshift
Javascript also gives us four method to easily add/ remove items to the beginning/ end of the array. For example, original array is `var arr = [1,2,3];`.
* `push()`:  <strong>Add items</strong> to the <strong>end</strong> of the array, can have multiple arguments
   ```
   arr.push(4); // [1,2,3,4]
   arr.push(4, 5); // [1,2,3,4,5]
  ```
* `pop()`: <strong>Remove an item</strong> from the <strong>end</strong> of the array
   ```
   arr.pop(); // [1,2]
  ```
* `unshift()`: <strong>Add items</strong> to the <strong>beginning</strong> of the array ,can have multiple arguments
   ```
   arr.unshift(0); // [0,1,2,3]
   arr.unshift(-1, 0); // [-1,0,1,2,3]
  ```
 * `shift()`:  <strong>Remove an item </strong>to the <strong>end</strong> of the array
	```
   arr.shift(); // [2,3]
   ```