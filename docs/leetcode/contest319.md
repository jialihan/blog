## Leetcode Contest 319 - JavaScript

#### I. [2469. Convert the Temperature](#question-1)

#### II. [2470. Number of Subarrays With LCM Equal to K](#question-2)

#### III. [2471. Minimum Number of Operations to Sort a Binary Tree by Level](#question-3)

#### IV. [2472. Maximum Number of Non-overlapping Palindrome Substrings](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [2469. Convert the Temperature](https://leetcode.com/problems/convert-the-temperature/description/)

[Solution](https://leetcode.com/problems/convert-the-temperature/solutions/2815133/js-use-tofixed-for-decimals/):

```
var  convertTemperature = function(celsius) {
	const  k = celsius + 273.15;
	const  f = celsius * 1.8 + 32;
	return [k.toFixed(5), f.toFixed (5)];
};
```

<div  id="question-2"/>

### [2470. Number of Subarrays With LCM Equal to K](https://leetcode.com/problems/count-number-of-distinct-integers-after-reverse-operations/)

Solution: [link](https://leetcode.com/problems/number-of-subarrays-with-lcm-equal-to-k/solutions/2814921/js-o-n-2-array-indexof-solution/)

<div  id="question-3"/>

### [2471. Minimum Number of Operations to Sort a Binary Tree by Level](https://leetcode.com/problems/minimum-number-of-operations-to-sort-a-binary-tree-by-level/description/)

Solution: [link](https://leetcode.com/problems/minimum-number-of-operations-to-sort-a-binary-tree-by-level/solutions/2809035/javascript-bfs-array-swaps-at-each-index/)

Find the min swaps in an array:

```
function  findMinSwaps (arr) {
	let  cnt = 0;
	arr.sort((a,b)=>a[0]-b[0]);
	for(let  i = 0; i<arr.length; i++) {
		while(arr[i][1] !== i) {
			const  j = arr[i][1];
			[arr[i], arr[j]] = [arr[j], arr[i]];
			cnt++;
		}
	}
	return  cnt;
}
```

<div  id="question-4"  />

### [2472. Maximum Number of Non-overlapping Palindrome Substrings](https://leetcode.com/problems/maximum-number-of-non-overlapping-palindrome-substrings/description/)

Solution:
https://leetcode.com/problems/maximum-number-of-non-overlapping-palindrome-substrings/solutions/2815116/javascript-o-k-n-array-without-dp/

- solution 1: Array check k length of palindrome
- solution 2: DP of length of k

<div  id="question-5"/>

### Summary

- keep moving!!!
