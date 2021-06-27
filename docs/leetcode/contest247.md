## Leetcode Contest 247 - JavaScript

#### I. [1913. Maximum Product Difference Between Two Pairs](#question-1)

#### II. [1914. Cyclically Rotating a Grid](#question-2)

#### III. [1915. Number of Wonderful Substrings](#question-3)

#### IV. [1916. Count Ways to Build Rooms in an Ant Colony](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1913. Maximum Product Difference Between Two Pairs](https://leetcode.com/problems/maximum-product-difference-between-two-pairs/)

The **product difference** between two pairs `(a, b)` and `(c, d)` is defined as `(a * b) - (c * d)`.

- For example, the product difference between `(5, 6)` and `(2, 7)` is `(5 * 6) - (2 * 7) = 16`.

Given an integer array `nums`, choose four **distinct** indices `w`, `x`, `y`, and `z` such that the **product difference** between pairs `(nums[w], nums[x])` and `(nums[y], nums[z])` is **maximized**.

Return _the **maximum** such product difference_.

**My JavaScript Solution:**

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxProductDifference = function (nums) {
  nums.sort((a, b) => a - b);
  var n = nums.length;
  return nums[n - 1] * nums[n - 2] - nums[0] * nums[1];
};
```

<div id="question-2"/>

### [1914. Cyclically Rotating a Grid](https://leetcode.com/problems/cyclically-rotating-a-grid/)

**Steps:**

- step1: loop for each layer, `layer = min(m/2, n/2);`
- step2: flat the 2D array into **"one-dimension"** array,
- step3: rotation `(k % len)` on that one-dimension array
- step4: Refill the updated one-dimention array into 2D array

**Reference:**
My discussion article: [JavaScript: BruteForce O(M\*N) array rotation](<https://leetcode.com/problems/cyclically-rotating-a-grid/discuss/1299714/JavaScript%3A-BruteForce-O(M*N)-array-rotation>)

<div id="question-3"/>

### [1915. Number of Wonderful Substrings](https://leetcode.com/problems/number-of-wonderful-substrings/)

A **wonderful** string is a string where **at most one** letter appears an **odd** number of times.

- For example, `"ccjjc"` and `"abab"` are wonderful, but `"ab"` is not.

Given a string `word` that consists of the first ten lowercase English letters (`'a'` through `'j'`), return _the **number of wonderful non-empty substrings** in_ `word`_. If the same substring appears multiple times in_ `word`_, then count **each occurrence** separately._

A **substring** is a contiguous sequence of characters in a string.

**Example 1:**
**Input:** word = "aba"
**Output:** 4
**Explanation:** The four wonderful substrings are underlined below:

- "**a**ba" -> "a"
- "a**b**a" -> "b"
- "ab**a**" -> "a"
- "**aba**" -> "aba"
- **Similar Question:**
- [1371. Find the Longest Substring Containing Vowels in Even Counts](https://leetcode.com/problems/find-the-longest-substring-containing-vowels-in-even-counts/)
- [1371 JS Solution - bitmask](https://leetcode.com/problems/find-the-longest-substring-containing-vowels-in-even-counts/discuss/1299935/JavaScript-with-explanation-bitmask)

**Referenced Solution LC. 1915:**

- [discussion article 1](https://leetcode.com/problems/number-of-wonderful-substrings/discuss/1299525/Count-bitmasks-with-picture)
- [discussion article 2](https://leetcode.com/problems/number-of-wonderful-substrings/discuss/1299523/C++-Bit-Vector-+-Prefix-Parities-%28Similar-to-Prefix-Sums%29)

**My JS Solution:**

```js
var wonderfulSubstrings = function (word) {
  // bitmask: 10 bits => 2^10 = 1024 masks
  var n = word.length;
  var count = new Array(1024).fill(0);
  count[0] = 1; // base count for the first index to allow at least one odd count
  var mask = 0;
  var ans = 0;
  for (var w of word) {
    var bit = getCharCode(w);
    mask = mask ^ (1 << bit);

    // 1. count at most 1 odd character from a to j
    for (var i = 0; i < 10; i++) {
      ans += count[mask ^ (1 << i)];
    }

    // 2. count all even chars in substring, the same mask state
    ans += count[mask];

    // 3. update current state
    count[mask]++;
  }
  return ans;
};
function getCharCode(c) {
  return c.charCodeAt(0) - 97;
}
```

<div id="question-4" />

### [1916. Count Ways to Build Rooms in an Ant Colony](https://leetcode.com/problems/count-ways-to-build-rooms-in-an-ant-colony/)

You are an ant tasked with adding `n` new rooms numbered `0` to `n-1` to your colony. You are given the expansion plan as a **0-indexed** integer array of length `n`, `prevRoom`, where `prevRoom[i]` indicates that you must build room `prevRoom[i]` before building room `i`, and these two rooms must be connected **directly**. Room `0` is already built, so `prevRoom[0] = -1`. The expansion plan is given such that once all the rooms are built, every room will be reachable from room `0`.

You can only build **one room** at a time, and you can travel freely between rooms you have **already built** only if they are **connected**. You can choose to build **any room** as long as its **previous room** is already built.

Return _the **number of different orders** you can build all the rooms in_. Since the answer may be large, return it **modulo** `109 + 7`.

**Reference:**

- [Bottom up DFS with return value](https://leetcode.com/problems/count-ways-to-build-rooms-in-an-ant-colony/discuss/1299540/PythonC%2B%2B-clean-DFS-solution-with-explanation)
- [How to solve TLE in comb(n,k in JavaScript - myself](<https://leetcode.com/problems/count-ways-to-build-rooms-in-an-ant-colony/discuss/1300223/JavaScript%3A-how-to-avoid-TLE-in-%22comb(nk)%22>)

**My JS Solution:**

```js
var waysToBuildRooms = function (prevRoom) {
  var mod = 1000000007n;
  // build adj
  var adj = new Map();
  for (var i = 1; i < prevRoom.length; i++) {
    if (!adj.has(prevRoom[i])) {
      adj.set(prevRoom[i], []);
    }
    adj.get(prevRoom[i]).push(i);
  }
  function dfs(node) {
    if (!adj.has(node)) {
      return [1n, 1]; // [ways, # of nodes]
    }
    var ans = 1n; // base result of number of ways
    var num = 0; // total # of nodes on current branch
    for (var next of adj.get(node)) {
      var [tmp, subNum] = dfs(next);

      ans = (ans * tmp * comb(subNum + num, subNum)) % mod;
      num += subNum;
    }
    return [ans, num + 1];
  }
  function comb(n, k) {
    // C(n,k) = n! / k!*(n-k)!
    console.log(n, k);
    if (n === k) {
      return 1n;
    }
    k = Math.min(k, n - k);
    var res = 1n;
    for (var i = 0; i < k; i++) {
      res *= BigInt(n - i);
      res /= BigInt(i + 1);
    }
    return res;
  }
  return Number(dfs(0)[0]);
};
```

<div id="question-5"/>

### Summary

- 2744 / 12636
- **Q3: didn't find the correct BitMask solution**, for each substring will always TLE of `O(n^2)`, but here, don't need to check for each substring, each letter only have 2 state of "even or odd".
- **Q4: remember this comb(n,k) function in JS**, write it in a proper and **safe** way, especially the edge cases and reduce loop time.
- keep moving!
