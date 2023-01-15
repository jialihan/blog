## Leetcode Contest 328 - JavaScript

#### I. [2535. Difference Between Element Sum and Digit Sum of an Array](#question-1)

#### II. [2536. Increment Submatrices by One](#question-2)

#### III. [2537. Count the Number of Good Subarrays](#question-3)

#### IV. [2538. Difference Between Maximum and Minimum Price Sum](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [2535. Difference Between Element Sum and Digit Sum of an Array](https://leetcode.com/problems/difference-between-element-sum-and-digit-sum-of-an-array/description/)

Solution:
https://leetcode.com/problems/difference-between-element-sum-and-digit-sum-of-an-array/solutions/3053565/js-easy-approach/

<div id="question-2"/>

### [2536. Increment Submatrices by One](https://leetcode.com/problems/increment-submatrices-by-one/description/)

Solution:
https://leetcode.com/problems/increment-submatrices-by-one/solutions/3053561/js-brute-force/

<div id="question-3"/>

### [2537. Count the Number of Good Subarrays](https://leetcode.com/problems/count-the-number-of-good-subarrays/description/)

Solution:
https://leetcode.com/problems/count-the-number-of-good-subarrays/solutions/3053553/js-sliding-window-count-at-index-range-start-i/

<div id="question-4" />

### [2538. Difference Between Maximum and Minimum Price Sum](https://leetcode.com/problems/difference-between-maximum-and-minimum-price-sum/description/)

Referenced idea:
https://leetcode.com/problems/difference-between-maximum-and-minimum-price-sum/solutions/3052773/c-dfs-memo/?orderBy=most_relevant

```js
var maxOutput = function (n, edges, price) {
  // 1. build adj
  const adj = new Map();
  for (const [e0, e1] of edges) {
    if (!adj.has(e0)) {
      adj.set(e0, []);
    }
    if (!adj.has(e1)) {
      adj.set(e1, []);
    }
    adj.get(e0).push(e1);
    adj.get(e1).push(e0);
  }

  // 2. dfs find the max path sum at one node
  let max;
  const memo = new Map();
  function dfs(parent, node, visited) {
    const key = `${node}:${parent}`;
    if (memo.has(key)) {
      return memo.get(key);
    }

    visited.add(node);
    let res = 0;
    if (adj.has(node)) {
      for (const next of adj.get(node)) {
        if (!visited.has(next)) {
          res = Math.max(res, dfs(node, next, visited));
        }
      }
    }
    memo.set(key, res + price[node]);
    return memo.get(key);
  }

  // 3. root at node only with one branch
  let diff = 0;
  for (let i = 0; i < n; i++) {
    if (adj.has(i) && adj.get(i).length === 1) {
      diff = Math.max(diff, dfs(-1, i, new Set()) - price[i]);
    }
  }
  return diff;
};
```

<div id="question-5"/>

### Summary

- 1093 / 22237
- Q4 TLE since use DFS, **but NOT with memo**
