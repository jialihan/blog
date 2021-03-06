## Understand Array.prototype.reduce

#### I. [reduce function in Array](#p1)  

#### II. [Advanced Usage ](#p2)  

#### III. [Implement your own Reducer function](#p3)

<div id="p1" />

### I. reduce function in Array
docs: [reduce()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

basic usage:
```js
const reducer = (accumulator, currentValue) => accumulator + currentValue;
// eg: 1 + 2 + 3 + 4
array1.reduce(reducer,initialValue);
```

<div id="p2" />

### II. Advanced Usage

The  **reducer**  function takes four arguments:

1.  Accumulator
2.  Current Value
3.  Current Index - optional
4.  Source Array - optional

**Edge case:**
When `initialValue` not provided:
- accumulator will be the first element in array: `acc = arry[0]`
- `reduce()` will execute from `index 1` instead of index `0`
- If the array is empty and no `initialValue` is provided, [`TypeError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError) will be thrown.

**How reduce works?**
For example: when No initial value provided
```js
[0, 1, 2, 3, 4].reduce(function(accumulator, currentValue, currentIndex, array) {
  return accumulator + currentValue
})
```

Callback invoked like the following:
| `callback`  iteration | `accumulator` | `currentValue` | `currentIndex` | `array` | return value |
|--|--|--|--|--|--|--|--|
| first call | `0` | `1` |`1` | `[0, 1, 2, 3, 4]` | `1` |
| second call | `1` | `2` |`2` | `[0, 1, 2, 3, 4]` | `3` |
| third call | `3` | `3` |`3` | `[0, 1, 2, 3, 4]` | `6` |
| fourth call | `6` | `4` |`4` | `[0, 1, 2, 3, 4]` | `10` |

<div id="p3" />

### III. Implement your own Reducer function

Question: implement your own reducer function in `Array.prototype.myReduce()` with the same functionality.

For example:
```js
[1,2,3].myReduce((a,b)=>a+b, 0);
```

**Solution:**
- attention to edge case when initial value is not provided
- not use arrow function, because there is **no** `this` keyword you can access.

```js
Array.prototype.myReduce = function (fn, initialValue) {
	var  accumulator = initialValue || this[0];
	for (var  i = 0; i < this.length; i++) {
		if (!initialValue && i === 0) {
		// when initialValue is not provided
		continue;
		}
		accumulator = fn.call(null, accumulator, this[i], i, this);
	}
	return  accumulator;
};
// usage
console.log([1, 2, 3].myReduce((a, b) =>  a + b));
```








