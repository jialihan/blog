### Array Manipulation

### Splice: return a new array
1. Syntax
	```
	[].splice(index, howmany, item1, ....., itemN)
	```
	parameters:
	* `index`: required. the position to add/remove items,  `negative values` to specify the position from the end of the array.
	* `howmany`: optional, how many items to be removed. if `0`, none will be removed! 
	* `newItems`:  optional. The new item(s) to be added to the array.
2.  Use case
```
var arr = [1,2,3,4,5];
arr.splice(1); // [1], remove from index 1 to the end
arr.splice(1,1); // [1,3,4,5], only remove 1 element from index 1
arr.splice(1,1,'new1', 'new2'); // [1,'new1','new2',3,4,5], remove 2, add new two items
```
### Slice: 