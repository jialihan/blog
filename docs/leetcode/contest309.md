## Leetcode Contest 309✌️ - JavaScript

#### I. [2399. Check Distances Between Same Letters](#question-1)

#### II. [2400. Number of Ways to Reach a Position After Exactly k Steps - not solved](#question-2)

#### III. [2401. Longest Nice Subarray](#question-3)

#### IV. [2402. Meeting Rooms III✌️](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [2399. Check Distances Between Same Letters](https://leetcode.com/problems/check-distances-between-same-letters/)

Solution:
https://leetcode.com/problems/check-distances-between-same-letters/discuss/2527675/Javascript-easy-way-%2B-hash-Set

<div  id="question-2"/>

### [2400. Number of Ways to Reach a Position After Exactly k Steps](https://leetcode.com/problems/longest-subsequence-with-limited-sum/)

Not solved, don't know a math way to calc combinations.

After contest: DFS + memo

```javascript
var numberOfWays = function (startPos, endPos, k) {
  const mod = 1000000007;
  const dp = new Array(4002).fill(0).map((el) => new Array(1001).fill(-1));
  function dfs(start, end, kk) {
    if (start === end && kk === 0) {
      return 1;
    }
    if (kk <= 0) {
      return 0;
    }
    if (dp[start + 1000][kk] !== -1) {
      return dp[start + 1000][kk];
    }
    const res =
      ((dfs(start + 1, end, kk - 1) % mod) +
        (dfs(start - 1, end, kk - 1) % mod)) %
      mod;
    dp[start + 1000][kk] = res;
    return res;
  }
  return dfs(startPos, endPos, k);
};
```

<div  id="question-3"/>

### [2401. Longest Nice Subarray](https://leetcode.com/problems/longest-nice-subarray/)

Solution:
https://leetcode.com/problems/longest-nice-subarray/discuss/2527702/JavaScript-brute-force-using-stack

<div  id="question-4"  />

### [2402. Meeting Rooms III](https://leetcode.com/problems/meeting-rooms-iii/)

Solution:
https://leetcode.com/problems/meeting-rooms-iii/discuss/2527637/JavaScript-Two-Priority-Queues

<div  id="question-5"/>

### Summary

- 3/4 completed
- Q2 is hard for me, the math problem or DP problem, i need more practice.
- 2031 / 25750
