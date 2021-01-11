##  LIS - Longest Increasing Sequence

#### I.  [LC 300. Longest Increasing Sequence](#question-1)
- [brute force - DP solution - O(N^2)](#q1-1)
- [Optimized Solution: Binary Search - O( NlogN )](#q1-2)

#### II. [LC 1713. Minimum Operations to Make a Subsequence ](#question-2)
- [Analysis and break down problem](#q2-1)
- [Javascript solution code](#q2-2)

<div id="question-1"/>

### I. [300.  Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

Given an integer array  `nums`, return the length of the longest strictly increasing subsequence.

A  **subsequence**  is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example,  `[3,6,2,7]`  is a subsequence of the array  `[0,3,1,6,2,2,7]`.

**Example 1:**
**Input:** nums = [10,9,2,5,3,7,101,18]
**Output:** 4
**Explanation:** The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

**Example 2:**
**Input:** nums = [0,1,0,3,2,3]
**Output:** 4

**Example 3:**
**Input:** nums = [7,7,7,7,7,7,7]
**Output:** 1

<div id="q1-1" />

#### 1.1 brute force solution: DP - O(N^2)
dp[i] : the longest increasing sequence until index `i`

```js
var lengthOfLIS = function(nums) {
    /* O(n^2) not the optimized solution*/
    var n = nums.length;
    const dp = [...Array(n+1)].fill(1);
   
    let max = 0;
    for (let i = 0; i < nums.length; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        max = Math.max(dp[i], max);
    }
    return max;
};
```

<div id="q1-2" />

#### 1.2 Optimized Solution: Binary Search - O( NlogN )
Keep a local array to maintain the increasing sequence:
- initial is empty array: []
- binary search to find the first index to insert current **num**
- Be careful the edge case: [7,7,7,7], find the **lower_bound**
- if the index === array.length, means it's the largest number, just **append to the tail**.
- if the index is valid in array, **replace** current **num** at the index

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var lengthOfLIS = function(nums) {
    var n = nums.length;
    var arr = [nums[0]];
   
    for (let i = 1; i < nums.length; i++) {
        // find upper bound number to replace
        var l = 0;
        var r = arr.length;
        while(l < r)
        {
            var mid = Math.floor( (r+l)/2);
            if(arr[mid] < nums[i])
            {
                l = mid +1;
            }
            else
            {
                r = mid;
             }
        }
        if(l === arr.length)
        {
            arr.push(nums[i]);
        }
        else
        {
            arr[l] = nums[i];
        }      
    }
    return arr.length;
};
```

<div id="question-2"/>

### II. [1713.  Minimum Operations to Make a Subsequence](https://leetcode.com/problems/minimum-operations-to-make-a-subsequence)

You are given an array  `target`  that consists of  **distinct**  integers and another integer array  `arr`  that  **can**  have duplicates.

In one operation, you can insert any integer at any position in  `arr`. For example, if  `arr = [1,4,1,2]`, you can add  `3`  in the middle and make it  `[1,4,3,1,2]`. Note that you can insert the integer at the very beginning or end of the array.

Return  _the  **minimum**  number of operations needed to make_ `target` _a  **subsequence**  of_ `arr`_._

A  **subsequence**  of an array is a new array generated from the original array by deleting some elements (possibly none) without changing the remaining elements' relative order. For example,  `[2,7,4]`  is a subsequence of  `[4,2,3,7,2,1,4]`  (the underlined elements), while  `[2,4,2]`  is not.

**Example 1:**
**Input:** target = [5,1,3], `arr` = [9,4,2,3,4]
**Output:** 2
**Explanation:** You can add 5 and 1 in such a way that makes `arr` = [5,9,4,1,2,3,4], then target will be a subsequence of `arr`.

<div id="q2-1" />

#### 2.1 Analysis and Break down problem

 - don't need to care about the numbers not found in target array;
- replace the current array with the index of the target array ( because it's **distinct number in target array** )
- transform the problem to be:
	**minimum operation = total_length - longest_increase_sequence**
	- reference to the above LC 300 problem

For example:
 -  target: [6,4,8,1,3,2] ->   0, 1, 2, 3, 4, 5 ( index )
  - array: [4,7,6,2,3,8,6,1] -> 1,-1, 0, 5, 4, 2, 0, 3 (index)
  - if not found in target, use `-1` to replace, but we can omit the `-1`
   -  we got: increasing seq: [0,2,3], length is 3.
   - min_result = total_length ( 6 ) - LIS_length ( 3 ) = 3
<div id="q2-2" />

#### 2.2 javascript solution code

**Example:**
```
  // 5 1 3 -> 0,1,2
  // 9 4 2 3 4 -> -1, -1, -1, 2, -1 
  // => increasing sequence: [2] 
    
  // [6,4,8,1,3,2] ->     0, 1, 2, 3, 4, 5
  // [4,7,6,2,3,8,6,1] -> 1,-1, 0, 5, 4, 2, 0, 3
  // => increasing seq: [0,2,3]
```
**Code:**
```js
var minOperations = function(target, arr) {
    const n = target.length;
    const indexMap = new Map();
    target.forEach((el,i)=>{
        indexMap.set(el, i);
    });
    const array = arr.map((el,i) =>{
        if(indexMap.has(el))
        {
            return indexMap.get(el);
        }
        else
            return -1;
    }).filter(el=>el !== -1);
    
    // find the longest increasing sequence (LIS)
    let list = [];
    for(let i = 0; i<array.length; i++)
    {
        let l = 0;
        let r = list.length;
        while(l<r)
        {
            let mid = Math.floor((l+r)/2);
            if(list[mid]<array[i])
                {
                    l = mid+ 1;
                }
            else
                {
                    r = mid;
                }
        }
        if(l === list.length)
            {
                list.push(array[i]);
            }
        else
           { list[l] = array[i];}
    }
    
    return n - list.length;
    
};
```
