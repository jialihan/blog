## Leetcode Contest 246 - JavaScript

#### I. [1903. Largest Odd Number in String](#question-1)

#### II. [1904. The Number of Full Rounds You Have Played](#question-2)

#### III. [1905. Count Sub Islands](#question-3)

#### IV. [1906. Minimum Absolute Difference Queries](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1903. Largest Odd Number in String](https://leetcode.com/problems/largest-odd-number-in-string/)

You are given a string `num`, representing a large integer. Return _the **largest-valued odd** integer (as a string) that is a **non-empty substring** of_ `num`_, or an empty string_ `""` _if no odd integer exists_.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**
**Input:** num = "52"
**Output:** "5"
**Explanation:** The only non-empty substrings are "5", "2", and "52". "5" is the only odd number.

**Example 2:**
**Input:** num = "4206"
**Output:** ""
**Explanation:** There are no odd numbers in "4206".

**My solution:**

```js
var largestOddNumber = function (num) {
  var res = "";
  for (var i = num.length - 1; i >= 0; i--) {
    if (parseInt(num[i]) % 2 === 1) {
      res = [...num].slice(0, i + 1).join("");
      return res;
    }
  }
  return res;
};
```

<div id="question-2"/>

### [1904. The Number of Full Rounds You Have Played](https://leetcode.com/problems/the-number-of-full-rounds-you-have-played/)

A new online video game has been released, and in this video game, there are **15-minute** rounds scheduled every **quarter-hour** period. This means that at `HH:00`, `HH:15`, `HH:30` and `HH:45`, a new round starts, where `HH` represents an integer number from `00` to `23`. A **24-hour clock** is used, so the earliest time in the day is `00:00` and the latest is `23:59`.

Given two strings `startTime` and `finishTime` in the format `"HH:MM"` representing the exact time you **started** and **finished** playing the game, respectively, calculate the **number of full rounds** that you played during your game session.

- For example, if `startTime = "05:20"` and `finishTime = "05:59"` this means you played only one full round from `05:30` to `05:45`. You did not play the full round from `05:15` to `05:30` because you started after the round began, and you did not play the full round from `05:45` to `06:00` because you stopped before the round ended.

If `finishTime` is **earlier** than `startTime`, this means you have played overnight (from `startTime` to the midnight and from midnight to `finishTime`).

Return _the **number of full rounds** that you have played if you had started playing at_ `startTime` _and finished at_ `finishTime`.

**Example 1:**
**Input:** startTime = "12:01", finishTime = "12:44"
**Output:** 1
**Explanation:** You played one full round from 12:15 to 12:30.
You did not play the full round from 12:00 to 12:15 because you started playing at 12:01 after it began.
You did not play the full round from 12:30 to 12:45 because you stopped playing at 12:44 before it ended.

**Example 2:**
**Input:** startTime = "20:00", finishTime = "06:00"
**Output:** 40
**Explanation:** You played 16 full rounds from 20:00 to 00:00 and 24 full rounds from 00:00 to 06:00.
16 + 24 = 40.

**Reference:**
discussion article: [Java - O(1) 100% faster](<https://leetcode.com/problems/the-number-of-full-rounds-you-have-played/discuss/1284627/Java-O(1)-100-faster>)

**JS solution:**

```js
var numberOfRounds = function (startTime, finishTime) {
  var [s1, s2] = startTime.split(":").map((el) => parseInt(el));
  var [e1, e2] = finishTime.split(":").map((el) => parseInt(el));
  var start = s1 * 60 + s2;
  var end = e1 * 60 + e2;
  if (start > end) {
    end += 24 * 60;
  }
  return Math.floor(end / 15) - Math.ceil(start / 15);
};
```

<div id="question-3"/>

### [1905. Count Sub Islands](https://leetcode.com/problems/count-sub-islands/)

You are given two `m x n` binary matrices `grid1` and `grid2` containing only `0`'s (representing water) and `1`'s (representing land). An **island** is a group of `1`'s connected **4-directionally** (horizontal or vertical). Any cells outside of the grid are considered water cells.

An island in `grid2` is considered a **sub-island** if there is an island in `grid1` that contains **all** the cells that make up **this** island in `grid2`.

Return the _**number** of islands in_ `grid2` _that are considered **sub-islands**_.

**Reference:**

- [DFS - JAVA solution - global variable](https://leetcode.com/problems/count-sub-islands/discuss/1284347/JAVA-Simple-DFS-with-explanation-and-comments.-TO%28n*m%29-and-SO%28n*m%29)
  **Only do dfs on "grid2",** a mark to `true or false` is a sub-island when comparing with **valid** `grid2[i][j]` with the position on `grid1[i][j]`
- [DFS with return boolean value](https://leetcode.com/problems/count-sub-islands/discuss/1284261/Java-DFS)

**My JS solution:**

```js
var countSubIslands = function (grid1, grid2) {
  var dir = [
    [0, -1],
    [1, 0],
    [-1, 0],
    [0, 1]
  ];
  function dfs(grid1, grid2, x, y) {
    visited[x][y] = true;
    if (grid1[x][y] !== 1) {
      isSub = false;
    }
    for (var d of dir) {
      var i = x + d[0];
      var j = y + d[1];
      if (
        i >= 0 &&
        j >= 0 &&
        i < m &&
        j < n &&
        !visited[i][j] &&
        grid2[i][j] === 1
      ) {
        dfs(grid1, grid2, i, j);
      }
    }
  }
  var m = grid1.length;
  var n = grid1[0].length;
  var cnt = 0;
  var isSub = true;
  var visited = new Array(m).fill(0).map((el) => new Array(n).fill(false));
  for (var i = 0; i < m; i++)
    for (var j = 0; j < n; j++) {
      if (grid2[i][j] === 1 && !visited[i][j]) {
        isSub = true;
        dfs(grid1, grid2, i, j);
        if (isSub) {
          cnt++;
        }
      }
    }
  return cnt;
};
```

<div id="question-4" />

### 1906. Minimum Absolute Difference Queries

The **minimum absolute difference** of an array `a` is defined as the **minimum value** of `|a[i] - a[j]|`, where `0 <= i < j < a.length` and `a[i] != a[j]`. If all elements of `a` are the **same**, the minimum absolute difference is `-1`.

- For example, the minimum absolute difference of the array `[5,2,3,7,2]` is `|2 - 3| = 1`. Note that it is not `0` because `a[i]` and `a[j]` must be different.

You are given an integer array `nums` and the array `queries` where `queries[i] = [li, ri]`. For each query `i`, compute the **minimum absolute difference** of the **subarray** `nums[li...ri]` containing the elements of `nums` between the **0-based** indices `li` and `ri` (**inclusive**).

Return _an **array**_ `ans` _where_ `ans[i]` _is the answer to the_ `ith` _query_.

A **subarray** is a contiguous sequence of elements in an array.

The value of `|x|` is defined as:

- `x` if `x >= 0`.
- `-x` if `x < 0`.

**Reference:**

- [DP - preSum with explanation - python](https://leetcode.com/problems/minimum-absolute-difference-queries/discuss/1284212/Python-Cumulative-sums-solution-%2B-2Liner-explained)
- [C++ - preSum & binary search with explanation](https://leetcode.com/problems/minimum-absolute-difference-queries/discuss/1284321/Prefix-Sums-vs.-Binary-Search)

**JS solution - count and preSum:**
explain DP and counting:

```text
dp[i][num]: Numbers before index i (1-based), which Number and how many number's count

whether a number appears in range [l, r] is decided by:
if(dp[l][num] !== dp[r+1][num]), means "num" did appear
```

```js
var minDifference = function (nums, queries) {
  var n = nums.length;
  var dp = new Array(n + 1).fill(0).map((el) => new Array(101).fill(0));
  // build dp count array
  for (var i = 0; i < n; i++)
    for (var j = 1; j <= 100; j++) {
      dp[i + 1][j] = dp[i][j] + (j === nums[i]);
    }
  // count for each query
  var ans = [];
  for (var [l, r] of queries) {
    var preNum = -1;
    var tmp = Infinity;
    r++;
    for (var j = 1; j <= 100; j++) {
      if (dp[l][j] < dp[r][j]) {
        if (preNum !== -1) {
          // calc current gap
          tmp = Math.min(tmp, j - preNum);
        }
        preNum = j;
      }
    }
    if (tmp === Infinity) {
      tmp = -1;
    }
    ans.push(tmp);
  }
  return ans;
};
```

<div id="question-5"/>

### Summary

- 6593 / 13703
- **Q2: I give up when i cannot handle the tricky timing problem, 😂😂😂**
- keep moving!
