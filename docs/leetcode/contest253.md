## Leetcode Contest 253 - JavaScript

#### I. [1961. Check If String Is a Prefix of Array](#question-1)

#### II. [1962. Remove Stones to Minimize the Total](#question-2)

#### III. [1963. Minimum Number of Swaps to Make the String Balanced](#question-3)

#### IV. [1964. Find the Longest Valid Obstacle Course at Each Position](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1961. Check If String Is a Prefix of Array](https://leetcode.com/problems/check-if-string-is-a-prefix-of-array/)

Given a string `s` and an array of strings `words`, determine whether `s` is a **prefix string** of `words`.

A string `s` is a **prefix string** of `words` if `s` can be made by concatenating the first `k` strings in `words` for some **positive** `k` no larger than `words.length`.

Return `true` _if_ `s` _is a **prefix string** of_ `words`_, or_ `false` _otherwise_.

**Example 1:**
**Input:** s = "iloveleetcode", words = ["i","love","leetcode","apples"]
**Output:** true
**Explanation:**
s can be made by concatenating "i", "love", and "leetcode" together.

**Example 2:**
**Input:** s = "iloveleetcode", words = ["apples","i","love","leetcode"]
**Output:** false
**Explanation:**
It is impossible to make s using a prefix of arr.

**My JavaScript Solution:**

```js
var isPrefixString = function (s, words) {
  var pre = "";
  for (var word of words) {
    pre += word;
    if (s === pre) {
      return true;
    }
    if (s.length < pre.length) {
      break;
    }
  }
  return false;
};
```

<div id="question-2"/>

### [1962. Remove Stones to Minimize the Total](https://leetcode.com/problems/remove-stones-to-minimize-the-total/)

You are given a **0-indexed** integer array `piles`, where `piles[i]` represents the number of stones in the `ith` pile, and an integer `k`. You should apply the following operation **exactly** `k` times:

- Choose any `piles[i]` and **remove** `floor(piles[i] / 2)` stones from it.

**Notice** that you can apply the operation on the **same** pile more than once.

Return _the **minimum** possible total number of stones remaining after applying the_ `k` _operations_.

`floor(x)` is the **greatest** integer that is **smaller** than or **equal** to `x` (i.e., rounds `x` down).

**Example 1:**
**Input:** piles = [5,4,9], k = 2
**Output:** 12
**Explanation:** Steps of a possible scenario are:

- Apply the operation on pile 2. The resulting piles are [5,4,5].
- Apply the operation on pile 0. The resulting piles are [3,4,5].
  The total number of stones in [3,4,5] is 12.

**Example 2:**
**Input:** piles = [4,3,6,7], k = 3
**Output:** 12
**Explanation:** Steps of a possible scenario are:

- Apply the operation on pile 3. The resulting piles are [4,3,3,7].
- Apply the operation on pile 4. The resulting piles are [4,3,3,4].
- Apply the operation on pile 0. The resulting piles are [2,3,3,4].
  The total number of stones in [2,3,3,4] is 12.

**My JavaScript Solution:**

```js
var minStoneSum = function (piles, k) {
  const q = new MaxPriorityQueue();
  var sum = 0;
  for (var p of piles) {
    sum += p;
    q.enqueue(p);
  }
  var remove = 0;
  var t = 0;
  while (!q.isEmpty() && t < k) {
    var cur = q.dequeue().element;
    var half = Math.floor(cur / 2);
    remove += half;
    q.enqueue(cur - half);
    t++;
  }
  return sum - remove;
};
```

<div id="question-3"/>

### [1963. Minimum Number of Swaps to Make the String Balanced](https://leetcode.com/problems/minimum-number-of-swaps-to-make-the-string-balanced/)

You are given a **0-indexed** string `s` of **even** length `n`. The string consists of **exactly** `n / 2` opening brackets `'['` and `n / 2` closing brackets `']'`.

A string is called **balanced** if and only if:

- It is the empty string, or
- It can be written as `AB`, where both `A` and `B` are **balanced** strings, or
- It can be written as `[C]`, where `C` is a **balanced** string.

You may swap the brackets at **any** two indices **any** number of times.

Return _the **minimum** number of swaps to make_ `s` _**balanced**_.

**Example 1:**
**Input:** s = "][]"
**Output:** 1
**Explanation:** You can make the string balanced by swapping index 0 with index 3.
The resulting string is "[[]]".

**Example 2:**
**Input:** s = "]]][[["
**Output:** 2
**Explanation:** You can do the following to make the string balanced:

- Swap index 0 with index 4. s = "[]][[]".
- Swap index 1 with index 5. s = "[[][]]".
  The resulting string is "[[][]]".

**Example 3:**
**Input:** s = "[]"
**Output:** 0
**Explanation:** The string is already balanced.

**Analysis:**
Find the rules:

```text
1 mismatch: ][][  --> 1 swap
2 mismatch: ]][[  --> 1 swap
3 mismatch: ]]][[[  --> 2 swap
4 mismatch: ]]]][[[[  --> 2 swap
5 mismatch: ]]]]][[[[[  --> 3 swap
...
```

**Steps:**

- find the "max" mismatch count
- return the `Math.ceil(max_mismatch_count / 2)`

**JavaScript:**

```js
var minSwaps = function (s) {
  var mismatch = 0;
  var cnt = 0;
  for (var i = 0; i < s.length; i++) {
    if (s[i] === "]") {
      cnt--;
    } else {
      cnt++;
    }
    if (cnt < 0) {
      mismatch = Math.min(mismatch, cnt);
    }
  }
  return Math.ceil(-mismatch / 2);
};
```

<div id="question-4" />

### [1964. Find the Longest Valid Obstacle Course at Each Position](https://leetcode.com/problems/count-number-of-special-subsequences/)

You want to build some obstacle courses. You are given a **0-indexed** integer array `obstacles` of length `n`, where `obstacles[i]` describes the height of the `ith` obstacle.

For every index `i` between `0` and `n - 1` (**inclusive**), find the length of the **longest obstacle course** in `obstacles` such that:

- You choose any number of obstacles between `0` and `i` **inclusive**.
- You must include the `ith` obstacle in the course.
- You must put the chosen obstacles in the **same order** as they appear in `obstacles`.
- Every obstacle (except the first) is **taller** than or the **same height** as the obstacle immediately before it.

Return _an array_ `ans` _of length_ `n`, _where_ `ans[i]` _is the length of the **longest obstacle course** for index_ `i` _as described above_.

**Example 1:**
**Input:** obstacles = [1,2,3,2]
**Output:** [1,2,3,3]
**Explanation:** The longest valid obstacle course at each position is:

- i = 0: [1], [1] has length 1.
- i = 1: [1,2], [1,2] has length 2.
- i = 2: [1,2,3], [1,2,3] has length 3.
- i = 3: [1,2,3,2], [1,2,2] has length 3.

**Example 2:**
**Input:** obstacles = [2,2,1]
**Output:** [1,2,1]
**Explanation:** The longest valid obstacle course at each position is:

- i = 0: [2], [2] has length 1.
- i = 1: [2,2], [2,2] has length 2.
- i = 2: [2,2,1], [1] has length 1.

**Wrong: TLE solution at first**

```js
var longestObstacleCourseAtEachPosition = function (obstacles) {
  var n = obstacles.length;
  var dp = new Array(n).fill(0);
  dp[0] = 1;
  for (var i = 1; i < n; i++) {
    var max = -1;
    for (var j = i - 1; j >= 0; j--) {
      if (obstacles[j] <= obstacles[i]) {
        max = Math.max(max, dp[j] + 1);
      }
    }
    dp[i] = max === -1 ? 1 : max;
  }
  return dp;
};
```

**References:**

- LC 300 : https://leetcode.com/problems/longest-increasing-subsequence/
- [C++/Python - Same with Longest Increasing Subsequence problem - Clean & Concise](https://leetcode.com/problems/find-the-longest-valid-obstacle-course-at-each-position/discuss/1390159/C%2B%2BPython-Same-with-Longest-Increasing-Subsequence-problem-Clean-and-Concise)

**JavaScript Solution:** LIS

```js
var longestObstacleCourseAtEachPosition = function (obstacles) {
  var n = obstacles.length;
  var lis = [];
  var res = new Array(n).fill(0);
  for (var i = 0; i < n; i++) {
    if (lis.length > 0 && obstacles[i] >= lis[lis.length - 1]) {
      lis.push(obstacles[i]);
      res[i] = lis.length;
    } else {
      // find the upper bound
      var l = 0;
      var r = lis.length;
      while (l <= r) {
        var mid = Math.floor((l + r) / 2);
        if (lis[mid] <= obstacles[i]) {
          l = mid + 1;
        } else {
          r = mid - 1;
        }
      }
      lis[l] = obstacles[i];
      res[i] = l + 1;
    }
  }
  return res;
};
```

<div id="question-5"/>

### Summary

- 3564 / 13587
- Q3: Lucy to find the rules
- Q4: TLE, forget the LIS related similar problem
