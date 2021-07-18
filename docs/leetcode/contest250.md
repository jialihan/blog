## Leetcode Contest 250 - JavaScript

#### I. [1935. Maximum Number of Words You Can Type](#question-1)

#### II. [1936. Add Minimum Number of Rungs](#question-2)

#### III. [1937. Maximum Number of Points with Cost](#question-3)

#### IV. [1938. Maximum Genetic Difference Query - not solved](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1935. Maximum Number of Words You Can Type](https://leetcode.com/problems/maximum-number-of-words-you-can-type/)

There is a malfunctioning keyboard where some letter keys do not work. All other keys on the keyboard work properly.

Given a string `text` of words separated by a single space (no leading or trailing spaces) and a string `brokenLetters` of all **distinct** letter keys that are broken, return _the **number of words** in_ `text` _you can fully type using this keyboard_.

**Example 1:**

**Input:** text = "hello world", brokenLetters = "ad"
**Output:** 1
**Explanation:** We cannot type "world" because the 'd' key is broken.

**My JavaScript Solution:**

```js
var canBeTypedWords = function (text, brokenLetters) {
  var ss = text.split(" ");
  var cnt = 0;
  var valid;
  for (var s of ss) {
    valid = true;
    for (var c of [...brokenLetters]) {
      if (s.indexOf(c) >= 0) {
        valid = false;
        break;
      }
    }
    if (valid) {
      cnt++;
    }
  }
  return cnt;
};
```

<div id="question-2"/>

### [1936. Add Minimum Number of Rungs](https://leetcode.com/problems/add-minimum-number-of-rungs/)

You are given a **strictly increasing** integer array `rungs` that represents the **height** of rungs on a ladder. You are currently on the **floor** at height `0`, and you want to reach the last rung.

You are also given an integer `dist`. You can only climb to the next highest rung if the distance between where you are currently at (the floor or on a rung) and the next rung is **at most** `dist`. You are able to insert rungs at any positive **integer** height if a rung is not already there.

Return _the **minimum** number of rungs that must be added to the ladder in order for you to climb to the last rung._

**Example 1:**
**Input:** rungs = [1,3,5,10], dist = 2
**Output:** 2
**Explanation:** You currently cannot reach the last rung.
Add rungs at heights 7 and 8 to climb this ladder.
The ladder will now have rungs at [1,3,5,7,8,10].

**Reference:**

- my article: https://leetcode.com/problems/add-minimum-number-of-rungs/discuss/1345072/JavaScript-Compute-each-Gap-with-math

**My JavaScript Solution:**

```js
var addRungs = function (rungs, dist) {
  var cnt = 0;
  rungs.unshift(0); // #1
  for (var i = 1; i < rungs.length; i++) {
    var gap = rungs[i] - rungs[i - 1];
    if (gap > dist) {
      cnt += Math.ceil(gap / dist) - 1; // #2
    }
  }
  return cnt;
};
```

<div id="question-3"/>

### [1937. Maximum Number of Points with Cost](https://leetcode.com/problems/maximum-number-of-points-with-cost/)

You are given an `m x n` integer matrix `points` (**0-indexed**). Starting with `0` points, you want to **maximize** the number of points you can get from the matrix.

To gain points, you must pick one cell in **each row**. Picking the cell at coordinates `(r, c)` will **add** `points[r][c]` to your score.

However, you will lose points if you pick a cell too far from the cell that you picked in the previous row. For every two adjacent rows `r` and `r + 1` (where `0 <= r < m - 1`), picking cells at coordinates `(r, c1)` and `(r + 1, c2)` will **subtract** `abs(c1 - c2)` from your score.

Return _the **maximum** number of points you can achieve_.

**Contest**: 😂😂😂 Luckily Passed due to JS's fast speed, maybe it's NOT TLE.

Bruteforce `O(m*n*n)` - JS passed

```js
var maxPoints = function (points) {
  var m = points.length;
  var n = points[0].length;
  // dp[2][n];
  // dp[1][j] = Math.max(for each dp[0][i] - |i-j| )
  var pre = [...points[0]];
  var cur;
  if (m === 1) {
    return Math.max(...pre);
  }
  for (var i = 1; i < m; i++) {
    cur = [...points[i]];
    for (var j = 0; j < n; j++) {
      var max = -Infinity;
      for (var k = 0; k < n; k++) {
        max = Math.max(max, pre[k] - Math.abs(j - k));
      }
      cur[j] += max;
    }
    pre = [...cur];
  }
  return Math.max(...pre);
};
```

**Optimized** to `O(m*n)`:

```js
/**
 * @param {number[][]} points
 * @return {number}
 */
var maxPoints = function (points) {
  var m = points.length;
  var n = points[0].length;
  var dp = new Array(m).fill(0).map((el) => new Array(n).fill(0));
  dp[0] = [...points[0]];
  if (m === 1) {
    return Math.max(...dp[0]);
  }
  for (var i = 1; i < m; i++) {
    var left = new Array(n).fill(dp[i - 1][0]);
    var right = new Array(n).fill(dp[i - 1][n - 1] - n + 1);

    // left to right: dp[i][j] = dp[i-1][k] - (j - k) + p[i][j]
    for (var k = 1; k < n; k++) {
      left[k] = Math.max(left[k - 1], dp[i - 1][k] + k);
    }

    // right to left: dp[i][j] = dp[i-1][k] - (k - j) + p[i][j]
    for (var k = n - 2; k >= 0; k--) {
      right[k] = Math.max(right[k + 1], dp[i - 1][k] - k);
    }

    // add current point[i][j]
    for (var j = 0; j < n; j++) {
      dp[i][j] = Math.max(left[j] - j, right[j] + j) + points[i][j];
    }
  }
  return Math.max(...dp[m - 1]);
};
```

**Reference Discussion Article:**
https://leetcode.com/problems/maximum-number-of-points-with-cost/discuss/1344888/C%2B%2B-dp-from-O(m-*-n-*-n)-to-O(m-*-n)

<div id="question-4" />

### [1938. Maximum Genetic Difference Query](https://leetcode.com/problems/maximum-genetic-difference-query/)

Not solved! TODO: dfs + trie

<div id="question-5"/>

### Summary

- **407** / 13694 ✌️
- 😂😂😂 JavaScript is NOT TLE saved me to the higher rank in Q3, but i know my algorithm is not the optimized!
- Q4: TODO: how to improve DFS with trie insert/delete dynamically
