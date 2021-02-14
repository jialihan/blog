##  Max Sub Array Sum - kadane's Algorithm

#### I. [Kadane's Algorithm](#question-1)

#### II. [1749. Maximum Absolute Sum of Any Subarray](#question-2)
- [Solution1: kadane's algorithm](#q2-1)
- [Another smart solution: PreSum](#q2-3)

<div id="question-1"/>

### [I. Introduction: Kadane's Algorithm](#question-1)
docs:  [article](https://hackernoon.com/kadanes-algorithm-explained-50316f4fd8a6)

![image](../../assets/kadaneAlgo.png ':size=445x71')

<div id="question-2"/>

### [II. 1749. Maximum Absolute Sum of Any Subarray](https://leetcode.com/problems/maximum-absolute-sum-of-any-subarray/)

You are given an integer array  `nums`. The  **absolute sum**  of a subarray  `[numsl, numsl+1, ..., numsr-1, numsr]`  is  `abs(numsl  + numsl+1  + ... + numsr-1  + numsr)`.

Return  _the  **maximum**  absolute sum of any  **(possibly empty)**  subarray of_ `nums`.

Note that  `abs(x)`  is defined as follows:

-   If  `x`  is a negative integer, then  `abs(x) = -x`.
-   If  `x`  is a non-negative integer, then  `abs(x) = x`.

**Example 1:**
**Input:** nums = [1,-3,2,3,-4]
**Output:** 5
**Explanation:** The subarray [2,3] has absolute sum = abs(2+3) = abs(5) = 5.

<div id="q2-1"/>

#### Solution1: kadane's algorithm 

A little bit modification:
- max positive sum
- min negative sum -> `Math.abs(min)`

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxAbsoluteSum = function(nums) {
    // kadane's algo
    var max = 0;
    var min = 0;
    var pre = 0;
    for(var num of nums)
    {
        pre += num;
        max = Math.max(max, pre);
        min = Math.min(min, pre);
    }
    return max-min;
};
```

<div id="q2-2"/>

#### 2.2 Another smart solution: PreSum

Analysis:
- find the sub array: `sum[i..j]` with max Sum
- `sum[i..j] = Max(sum[j] - sum[i])`
	- Max_Sum[j] - Min_Sum[i]
	- `| Max(0, sum[j]) - Min(0, sum[i])|`
	- no matter `i` or `j` is before or after, because we use `abs()` value.

**Javascript solution:**

```js
var maxAbsoluteSum = function(nums) {
    // sum[i:j] = Prefix[j] - Prefix[i-1];
    // max(prefix, 0) - min(prefix, 0) 
    var max = 0;
    var min = 0;
    var pre = 0;
    for(var num of nums)
    {
        pre += num;
        max = Math.max(max, pre);
        min = Math.min(min, pre);
    }
    return max-min;
};
```