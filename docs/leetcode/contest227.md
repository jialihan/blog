## Leetcode Contest 227- JavaScript

#### I. [1752.  Check if Array Is Sorted and Rotated](#question-1)

#### II. [1753. Maximum Score From Removing Stones](#question-2)
- [Priority Queue Solution](#q2-1)
- [ Smart Clean Math Solution](#q2-2)

#### III. [1754.  Largest Merge Of Two Strings](#question-3)

#### IV. [1755.  Closest Subsequence Sum](#question-4)

#### V. [Summary](#summary)


<div id="question-1"/>

###  [1752.  Check if Array Is Sorted and Rotated](https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/)

Given an array  `nums`, return  `true` _if the array was originally sorted in non-decreasing order, then rotated  **some**  number of positions (including zero)_. Otherwise, return  `false`.

There may be  **duplicates**  in the original array.

**Note:**  An array  `A`  rotated by  `x`  positions results in an array  `B`  of the same length such that  `A[i] == B[(i+x) % A.length]`, where  `%`  is the modulo operation.

**Example 1:**
**Input:** nums = [3,4,5,1,2]
**Output:** true
**Explanation:** [1,2,3,4,5] is the original sorted array.
You can rotate the array by x = 3 positions to begin on the the element of value 3: [3,4,5,1,2].

**Example 2:**
**Input:** nums = [2,1,3,4]
**Output:** false
**Explanation:** There is no sorted array once rotated that can make nums.

**Analysis:**
- understand the problem a little bit
	-	try to find the pivot point: x, failed
- just brute force to try every rotate x: `[ 0 .. n-1 ]`

**JavaScript solution:**
```js
var check = function(nums) {
    var a = [...nums].sort((i,j)=>i-j);
    for(var x =0; x < nums.length; x++)
    {
        var flag = true;
        for(var i = 0; i<nums.length; i++)
        {
            if(a[i] !== nums[(i+x)% nums.length])
            {
                flag = false;
                break;
            }
        }
        if(flag)
        {
            return true;
        }
        
    }
    return false;
};
```

<div id="question-2"/>

### [1753. Maximum Score From Removing Stones](https://leetcode.com/problems/maximum-score-from-removing-stones/)

You are playing a solitaire game with  **three piles**  of stones of sizes  `a`​​​​​​,  `b`,​​​​​​ and  `c`​​​​​​ respectively. Each turn you choose two  **different non-empty** piles, take one stone from each, and add  `1`  point to your score. The game stops when there are  **fewer than two non-empty**  piles (meaning there are no more available moves).

Given three integers  `a`​​​​​,  `b`,​​​​​ and  `c`​​​​​, return  _the_  **_maximum_** _**score**  you can get._

**Example 1:**
**Input:** a = 2, b = 4, c = 6
**Output:** 6
**Explanation:** The starting state is (2, 4, 6). One optimal set of moves is:
- Take from 1st and 3rd piles, state is now (1, 4, 5)
- Take from 1st and 3rd piles, state is now (0, 4, 4)
- Take from 2nd and 3rd piles, state is now (0, 3, 3)
- Take from 2nd and 3rd piles, state is now (0, 2, 2)
- Take from 2nd and 3rd piles, state is now (0, 1, 1)
- Take from 2nd and 3rd piles, state is now (0, 0, 0)
There are fewer than two non-empty piles, so the game ends. Total: 6 points.

<div id="q2-1"/>

#### 2.1 Priority Queue Solution

use this data structure: [MaxPriorityQueue()](https://github.com/datastructures-js/priority-queue), but i am not familiar with this API, then just use array sort to mock the PQ.
```js
var maximumScore = function(a, b, c) {
    var arr = [a,b,c];
    var res = 0;
    while(true)
    {
        arr.sort((x,y)=>y-x);
        // check stop condition
        if(arr[0] === 0 || arr[1] === 0)
        {
            break;
        }
        arr[0]--;
        arr[1]--;
        res++;
    }
    return res;
};
```

<div id="q2-2"/>

#### 2.2 Smart Clean Math Solution
see discussion solution here:  [simple math solution](https://leetcode.com/problems/maximum-score-from-removing-stones/discuss/1053491/One-line-Python-O%281%29)
- when a > (b+c)
	Then we can pair every (b+c) in a, return (b+c).
- if not, then a <= (b+c)
	- Then we can divide a into two parts that `a = a1+a2`
	- pair  `a1` in b, pair `a2` in c
	- remaining:  `(b-a1) === (c-a2)` 
	- total pair is: `a1` + `a2` + `(b-a1)` or `c-a2`
	- times 2 then divide 2 is the same: `
	```
	total = [2*a1 + 2*a2 + (b-a1)+ (c-a2)] / 2
		  = [ a1 + a2 + b +  c ] / 2 
		  = [ a + b + c ] / 2
	```
**JS solution code:**
```js
var maximumScore = function(a, b, c) {
    var arr = [a,b,c];
    arr.sort((x,y)=>y-x);
    if(arr[0] >= arr[1]+arr[2])
        return arr[1]+arr[2];
    return Math.floor((arr[0]+arr[1]+arr[2]) / 2);
};
```
<div id="question-3"/>

### [1754.  Largest Merge Of Two Strings](https://leetcode.com/problems/largest-merge-of-two-strings/)

You are given two strings  `word1`  and  `word2`. You want to construct a string  `merge`  in the following way: while either  `word1`  or  `word2`  are non-empty, choose  **one**  of the following options:

-   If  `word1`  is non-empty, append the  **first**  character in  `word1`  to  `merge`  and delete it from  `word1`.
    -   For example, if  `word1 = "abc"` and  `merge = "dv"`, then after choosing this operation,  `word1 = "bc"`  and  `merge = "dva"`.
-   If  `word2`  is non-empty, append the  **first**  character in  `word2`  to  `merge`  and delete it from  `word2`.
    -   For example, if  `word2 = "abc"` and  `merge = ""`, then after choosing this operation,  `word2 = "bc"`  and  `merge = "a"`.

Return  _the lexicographically  **largest**_ `merge` _you can construct_.

A string  `a`  is lexicographically larger than a string  `b`  (of the same length) if in the first position where  `a`  and  `b`  differ,  `a`  has a character strictly larger than the corresponding character in  `b`. For example,  `"abcd"`  is lexicographically larger than  `"abcc"`  because the first position they differ is at the fourth character, and  `d`  is greater than  `c`.

**Example 1:**
**Input:** word1 = "cabaa", word2 = "bcaaa"
**Output:** "cbcabaaaaa"
**Explanation:** One way to get the lexicographically largest merge is:
- Take from word1: merge = "c", word1 = "abaa", word2 = "bcaaa"
- Take from word2: merge = "cb", word1 = "abaa", word2 = "caaa"
- Take from word2: merge = "cbc", word1 = "abaa", word2 = "aaa"
- Take from word1: merge = "cbca", word1 = "baa", word2 = "aaa"
- Take from word1: merge = "cbcab", word1 = "aa", word2 = "aaa"
- Append the remaining 5 a's from word1 and word2 at the end of merge.

**Example 2:**
**Input:** word1 = "abcabc", word2 = "abdcaba"
**Output:** "abdcabcabcaba"

**JavaScript Solution:**

Difficulty:
- when char is the same, how to decide which way to merge?

see discussion here: [Javascript greedy solution](https://leetcode.com/problems/largest-merge-of-two-strings/discuss/1053907/JavaScript-greedy)

<div id="question-4"/>

### [1755.  Closest Subsequence Sum](https://leetcode.com/problems/closest-subsequence-sum/)

You are given an integer array  `nums`  and an integer  `goal`.

You want to choose a subsequence of  `nums`  such that the sum of its elements is the closest possible to  `goal`. That is, if the sum of the subsequence's elements is  `sum`, then you want to  **minimize the absolute difference**  `abs(sum - goal)`.

Return  _the  **minimum**  possible value of_  `abs(sum - goal)`.

Note that a subsequence of an array is an array formed by removing some elements  **(possibly all or none)**  of the original array.

**Example 1:**
**Input:** nums = [5,-7,3,5], goal = 6
**Output:** 0
**Explanation:** Choose the whole array as a subsequence, with a sum of 6.
This is equal to the goal, so the absolute difference is 0.

**JavaScript Solution:**
- list all possible sums in whole length array: **~~Heap out of memory~~**
- cut the array into two sub-arrays, then find the sum, **correct**

See discussion here: [cut in half + Binarysearch solution](https://leetcode.com/problems/closest-subsequence-sum/discuss/1053891/JavaScript-Cut-in-half-then-Binary-Search)

### Summary
- Math problem vs bruteforce solution
- js - priority queue
- all possible sum: out of memory
- binary search in sorted array
- 3688 / 11076
- keep moving !!! 
