## Data Structure in JavaScript

I. [How Computer Store Data](#how-computer-store-data)

II. [Several Algorithms](#several-algorithms)

III. [Reverse A String](#reverse-a-string)

IV. [Merge Sort In Javascript](#merge-sort-in-javascript)

### How Computer Store Data

- CPU
- RAM: Random Access Memory
- Storage: Persistent Memory

Want to share a so cute and nice video to bring my memory back to the **circuits and logic gates**......., and it's Chinese caption made me really understand the content, hahahha......
[Registers and RAM: Crash Course Computer Science #6](https://www.youtube.com/watch?v=fpnE6UAfbtU)

For example: when a large bit number exceeds the RAM storage, Javascript will get `Infinity` :

```
Math.pow(2,100000000)
Infinity
```

### Several Algorithms

Try to think about them in JavaScript:

- Sorting
- DP
- BFS + DFS ( Searching )
- Recursion

In javascript, we can only use **Arrays and Objects** ?
-- Not Really, we can build our own data structure in JS.

**Dynamic Array Operations:**

- lookup: O(1)
- push: O(1)
- insert: O(n): use `slice()`
- delete: O(n)

### Reverse A String

1. Common Solution

   ```
   function  reverse(str) {
   	// check input
   	if (!str || str.length < 2) {
   	return  str;
   	}

   	let  res = [];
   	for (let  i = str.length - 1; i >= 0; i--) {
   		res.push(str[i]);
   	}
   	return  res.join("");
   }
   ```

2. A cleaner solution with Array's built in method: `Array.prototype.reverse()`: [MDN: Array/reverse](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)
   only one line:
   ```
   const  reverse2 = (str) =>  str.split("").reverse().join("");
   const  reverse3 = (str) => [...str].reverse().join("");
   ```

### Merge Sort In Javascript

1. The common solution

```
function  mergeSortedArrays(arr1, arr2) {
	let  res = [];
	let  i = 0;
	let  j = 0;
	let  item1 = arr1[i];
	let  item2 = arr2[j];

	// check input
	if (!arr2 || arr2.length === 0) {
		return  arr1;
	}
	if (!arr1 || arr1.length === 0) {
		return  arr2;
	}

	// merge sort
	while (item1 || item2) {
		if (!item2 || item1 < item2) {
			res.push(item1);
			item1 = arr1[++i];
		} else  if (!item1 || item1 >= item2) {
			res.push(item2);
			item2 = arr2[++j];
		}
	}
	console.log(mergedArray);
	return  res;
}
```

2. A more cleaner way:

- use [Array/slice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)
- use [Array/concat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)

```
const  merge2 = (arr1, arr2) => {
let  sorted = [];
while (arr1.length && arr2.length) {
	if (arr1[0] < arr2[0])
		sorted.push(arr1.shift());
	else
		sorted.push(arr2.shift());
}
return  sorted.concat(arr1.slice().concat(arr2.slice()));
};
console.log(merge2([2, 5, 10, 57], [9, 12, 13]));
```
