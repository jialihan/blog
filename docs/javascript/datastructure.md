## Data Structure in JavaScript

I. [How Computer Store Data](#how-computer-store-data)

II. [Several Algorithms](#several-algorithms)

III. [Reverse A String](#reverse-a-string)

IV. [Merge Sort In Javascript](#merge-sort-in-javascript)

V. [Hash Tables](#hash-tables)

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

pros and cons about array:

| pros          | cons                       |
| ------------- | -------------------------- |
| fast lookups  | slow inserts               |
| fast push/pop | slow deletes               |
| ordered       | fixed size if static array |

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

### Hash Tables

- insert: O(1)
- lookup: O(1)
- delete: O(1)
- search: O(1)

But sometimes the collision slow down the algorithm:
Ways to solve the collisions: https://en.wikipedia.org/wiki/Hash_table

Write a Hash Table class by yourself:

```
class  HashTable {
	constructor(size) {
		this.data = new  Array(size);
	}

	_hash(key) {
		// todo
	}

	set(key, val) {
		const  index = this._hash(key);
		if (this.data[index]) {
			this.data[index] = [];
		}
		// collision: just add on that array
		this.data[index].push([key, value]);
	}

	get(key) {
		const  index = this._hash(key);
		const  bucket = this.data[index];
		if (bucket) {
			for (const [k, v] of  bucket) {
			if (key === k) {
				return  v;
			}
			}
		} // avg: O(1)
		return  undefined;
	}

	keys() {
		// loop all keys in hashtable
		const  keysArray = [];
		for (let  i = 0; i < this.data.length; i++) {
			if (this.data[i]) {
				keysArray.push(this.data[i][0][0]);
			}
		}
	}
}

const  myHashTable = new  HashTable(5);
myHashTable._hash("foo"); // still have access
myHashTable.set("foo", 100);
myHashTable.get("foo");
```
