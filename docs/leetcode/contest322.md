## Leetcode Contest 322 - JavaScript

#### I. [2490. Circular Sentence](#question-1)

#### II. [2491. Divide Players Into Teams of Equal Skill](#question-2)

#### III. [2492. Minimum Score of a Path Between Two Cities😭](#question-3)

#### IV. [2493. Divide Nodes Into the Maximum Number of Groups](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [2490. Circular Sentence](https://leetcode.com/problems/circular-sentence/description/)

Solution:
https://leetcode.com/problems/circular-sentence/solutions/2875632/javascript-easy-solution-using-split/

<div  id="question-2"/>

### [2491. Divide Players Into Teams of Equal Skill](https://leetcode.com/problems/divide-players-into-teams-of-equal-skill/description/)

Solution:
https://leetcode.com/problems/divide-players-into-teams-of-equal-skill/solutions/2875637/javascript-sorting-the-array/

<div  id="question-3"/>

### [2492. Minimum Score of a Path Between Two Cities](https://leetcode.com/problems/minimum-score-of-a-path-between-two-cities/description/)

**Solution:** 😭😭😭
这题真把我给写 emo 了， contest 的时候写了 4 个 solution：

1. BFS （bug， 永远 TLE）， emo 了
2. 换成写 dijikstra， 完了也 TLE， 可能有 bug
3. 写 Bellman-Ford： 对了，但是 TLE， emo 了
4. 写 union-find： 错了

Fixed BFS solution:
https://leetcode.com/problems/minimum-score-of-a-path-between-two-cities/solutions/2875598/javascript-bfs/?orderBy=most_relevant

Fixed Union find:

```javascript
var minScore = function (n, roads) {
  const dist = new Array(n + 1).fill(Infinity);
  const pa = new Array(n + 1).fill(0);
  for (let i = 1; i <= n; i++) {
    pa[i] = i;
  }
  function findParent(node) {
    if (pa[node] !== node) {
      pa[node] = findParent(pa[node]);
    }
    return pa[node];
  }
  for (const [cur, next, d] of roads) {
    const x = findParent(cur);
    const y = findParent(next);
    if (x !== y) {
      pa[x] = y;
    }
    dist[cur] = Math.min(dist[cur], d);
    dist[next] = Math.min(dist[next], d);
  }
  let min = Infinity;
  for (let i = 1; i <= n; i++) {
    if (findParent(1) === findParent(i)) {
      min = Math.min(min, dist[i]);
    }
  }
  return min === Infinity ? -1 : min;
};
```

复习了一下 dijkstra 算法：
https://leetcode.com/problems/path-with-maximum-probability/solutions/2875884/javascript-dijikstra/

<div  id="question-4"  />

### [2493. Divide Nodes Into the Maximum Number of Groups](https://leetcode.com/problems/divide-nodes-into-the-maximum-number-of-groups/description/)

Not solved!

<div  id="question-5"/>

### Summary

- 6381 / 19626
- keep moving!!!
