## Leetcode Contest 255 - JavaScript

#### I. [1979. Find Greatest Common Divisor of Array](#question-1)

#### II. [1980. Find Unique Binary String](#question-2)

#### III. [1981. Minimize the Difference Between Target and Chosen Elements](#question-3)

#### IV. [1982. Find Array Given Subset Sums - not solved](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1979. Find Greatest Common Divisor of Array](https://leetcode.com/problems/find-greatest-common-divisor-of-array/)

Given an integer array `nums`, return _the **greatest common divisor** of the smallest number and largest number in_ `nums`.

The **greatest common divisor** of two numbers is the largest positive integer that evenly divides both numbers.

**My JavaScript Solution:**

```js
var findGCD = function (nums) {
  function gcd(a, b) {
    if (b === 0) {
      return a;
    }
    return gcd(b, a % b);
  }
  var max = Math.max(...nums);
  var min = Math.min(...nums);
  return gcd(max, min);
};
```

<div id="question-2"/>

### [1980. Find Unique Binary String](https://leetcode.com/problems/find-unique-binary-string/)

Given an array of strings `nums` containing `n` **unique** binary strings each of length `n`, return _a binary string of length_ `n` _that **does not appear** in `nums`. If there are multiple answers, you may return **any** of them_.

**Example 1:**

**Input:** nums = ["01","10"]
**Output:** "11"
**Explanation:** "11" does not appear in nums. "00" would also be correct.

**Example 2:**

**Input:** nums = ["00","01"]
**Output:** "11"
**Explanation:** "11" does not appear in nums. "10" would also be correct.

**Analysis:**
`n bits` the largest number value is `2^n-1`, just find one of the NON-Existed binary string. Due to the simple JS built-in methods, it's so simple to call:

- to binary stirng: `number.toString(2)`
- `padStart()`: need to padding leading zeros if length is `< n`, ensure all string are `n bits` long.

```js
var findDifferentBinaryString = function (nums) {
  var n = nums.length;
  var set = new Set();
  nums.forEach((el) => set.add(el));
  let s;
  for (var num = 0; num < Math.pow(2, n); num++) {
    s = num.toString(2).padStart(n, "0"); // highly expressive JavaScript !!!
    if (!set.has(s)) {
      return s;
    }
  }
  return ""; // not found
};
```

<div id="question-3"/>

### [1981. Minimize the Difference Between Target and Chosen Elements](https://leetcode.com/problems/minimize-the-difference-between-target-and-chosen-elements/)

You are given an `m x n` integer matrix `mat` and an integer `target`.

Choose one integer from **each row** in the matrix such that the **absolute difference** between `target` and the **sum** of the chosen elements is **minimized**.

Return _the **minimum absolute difference**_.

The **absolute difference** between two numbers `a` and `b` is the absolute value of `a - b`.

**Constraints:**

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 70`
- `1 <= mat[i][j] <= 70`
- `1 <= target <= 800`

**Reference:**
https://leetcode.com/problems/minimize-the-difference-between-target-and-chosen-elements/discuss/1418793/C%2B%2B-Dynamic-Programming-O(M*N*5000)

**Bug:**
I have a weird test case, since `70*70>=4900` due to `0` based index, we have to ensure the `4901` length of dp array size. But when I do `4902`, the TLE test case passed!

**JS Solution:**

```js
var minimizeTheDifference = function (mat, target) {
  var m = mat.length;
  var n = mat[0].length;
  var dp = new Array(m).fill(0).map((el) => new Array(4902).fill(null));
  function helper(arr, index, sum) {
    if (index === m) {
      return Math.abs(target - sum);
    }
    if (dp[index][sum] !== null) {
      return dp[index][sum];
    }
    var res = Infinity;
    for (var j = 0; j < n; j++) {
      res = Math.min(res, helper(arr, index + 1, sum + arr[index][j]));
    }
    dp[index][sum] = res;
    return res;
  }
  return helper(mat, 0, 0);
};
```

<div id="question-4" />

### [1982. Find Array Given Subset Sums](https://leetcode.com/problems/find-array-given-subset-sums/)

TODO

<div id="question-5"/>

### Summary

- 2805 / 11837
- Q3: DFS + memo => `dp[row][sum]`
