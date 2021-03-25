## Leetcode Contest 233- JavaScript

#### I. [1800.  Maximum Ascending Subarray Sum](#question-1)

#### II. [1801. Number of Orders in the Backlog](#question-2)

#### III. [1802.  Maximum Value at a Given Index in a Bounded Array](#question-3)

#### IV. [1803.  Count Pairs With XOR in a Range](#question-4)
- [XOR properties](#q4-1)
- [XOR related Problems Summary](#q4-1)


<div id="question-1"/>

### [1800.  Maximum Ascending Subarray Sum](https://leetcode.com/contest/weekly-contest-233/problems/maximum-ascending-subarray-sum/)

Given an array of positive integers  `nums`, return the  _maximum possible sum of an  **ascending**  subarray in_ `nums`.

A subarray is defined as a contiguous sequence of numbers in an array.

A subarray  `[numsl, numsl+1, ..., numsr-1, numsr]`  is  **ascending**  if for all  `i`  where  `l <= i < r`,  `numsi < numsi+1`. Note that a subarray of size  `1`  is  **ascending**.

**Example 1:**
**Input:** nums = [10,20,30,5,10,50]
**Output:** 65
**Explanation:** [5,10,50] is the ascending subarray with the maximum sum of 65.

**Example 2:**
**Input:** nums = [10,20,30,40,50]
**Output:** 150
**Explanation:** [10,20,30,40,50] is the ascending subarray with the maximum sum of 150.

```js
var maxAscendingSum = function(nums) {
    var n = nums.length;
    var res = nums[0];
    var sum = nums[0];
    for(var i = 1; i<n;i++)
    {
        if(nums[i-1] < nums[i])
        {
            sum += nums[i];
           
        }
        else
        {
            sum = nums[i];
        }
        res = Math.max(sum, res);
    }
    return res;
};
```

Reference discussion article here: [Javascript running sum solution](https://leetcode.com/problems/maximum-ascending-subarray-sum/discuss/1119865/JavaScript-Linear-Running-Sum-solution)

<div id="question-2"/>

### [1801. Number of Orders in the Backlog](https://leetcode.com/problems/number-of-orders-in-the-backlog/)

Since the problem description is so long, then just see my solution at the discussion article here:
 [JavaScript: Priority Queue solution](https://leetcode.com/problems/number-of-orders-in-the-backlog/discuss/1119861/JavaScript-Priority-Queue-solution)

<div id="question-3"/>

### [1802.  Maximum Value at a Given Index in a Bounded Array](https://leetcode.com/problems/maximum-value-at-a-given-index-in-a-bounded-array/)

You are given three positive integers  `n`,  `index`  and  `maxSum`. You want to construct an array  `nums`  **(0-indexed)** that satisfies the following conditions:

-   `nums.length == n`
-   `nums[i]`  is a  **positive**  integer where  `0 <= i < n`.
-   `abs(nums[i] - nums[i+1]) <= 1`  where  `0 <= i < n-1`.
-   The sum of all the elements of  `nums`  does not exceed  `maxSum`.
-   `nums[index]`  is  **maximized**.

Return  `nums[index]`  of the constructed array.

Note that  `abs(x)`  equals  `x`  if  `x >= 0`, and  `-x`  otherwise.

**Example 1:**
**Input:** n = 4, index = 2,  maxSum = 6
**Output:** 2
**Explanation:** The arrays [1,1,**2**,1] and [1,2,**2**,1] satisfy all the conditions. There are no other valid arrays with a larger value at the given index.

**Example 2:**
**Input:** n = 6, index = 1,  maxSum = 10
**Output:** 3

**See the discussion article here:**  [Javascript: Priority Queue](https://leetcode.com/problems/maximum-average-pass-ratio/discuss/1108438/JavaScript-Priority-Queue)

Analysis: 
- Math: calculate the optimal sum with descending order from index center
	```
	[1, 1, 1, 2, 3, ...., val, val-1, val-2, ... , 2, 1, 1, 1]
	0                     index                            n-1
	```
- Binary search from 0 to highest limit value
	- upper bound, a tricky part here, use `math.ceil()` to avoid dead loop

**See the discussion article here:** 

[Javascript: Math BinarySearch solution](https://leetcode.com/problems/maximum-value-at-a-given-index-in-a-bounded-array/discuss/1119841/JavaScript-Math-%2B-BinarySearch-with-explanation)

<div id="question-4" />

### [1803.  Count Pairs With XOR in a Range](https://leetcode.com/problems/count-pairs-with-xor-in-a-range/)

Given a  **(0-indexed)**  integer array  `nums`  and two integers  `low`  and  `high`, return  _the number of  **nice pairs**_.

A  **nice pair**  is a pair  `(i, j)`  where  `0 <= i < j < nums.length`  and  `low <= (nums[i] XOR nums[j]) <= high`.

**JS - brute force solution**
```js
var countPairs = function(nums, low, high) {
    var n = nums.length;
    var cnt = 0;
    for(var i = 0; i<n; i++)
        for(var j = i+1; j<n; j++)
        {
            var tmp = nums[i] ^ nums[j];
            if(tmp>=low && tmp<=high)
            {
                cnt++;
            }
        }
    return cnt;
};
```

**Trie-solution-JavaScript:**
See the discussion article here: [JS: trie solution](https://leetcode.com/problems/count-pairs-with-xor-in-a-range/discuss/1126330/JavaScript-Trie-with-explanation)


<div id="q4-1"/>

#### 4.1 XOR Properties

**XOR and 0:**  `x ^ 0 = x`

**XOR on the same argument:**  `x ^ x = 0`

**Commutativity:**  `x ^ y = y ^ x`

**Another calculate:** `x ^ y = (x | y) - (x & y)`

<div id="q4-2"/>

#### 4.2 XOR related Problems
- [1720. Decode XORed Array](https://leetcode.com/problems/decode-xored-array/) : easy
- [421. Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/) : medium, there's an **Trie Solution**
- [1738. Find Kth Largest XOR Coordinate Value](https://leetcode.com/problems/find-kth-largest-xor-coordinate-value/) - medium, [DP solution](https://leetcode.com/problems/find-kth-largest-xor-coordinate-value/discuss/1032283/JavaScript-vs-Java-solution-DP)
- [1442. Count Triplets That Can Form Two Arrays of Equal XOR](https://leetcode.com/problems/count-triplets-that-can-form-two-arrays-of-equal-xor/) - medium, consider divide into two parts to make:
	```js
	part1 ^ part2 === 0;
	// then part1 === part2
	```


