## Leetcode Contest 231 - JavaScript

#### I.  [1784. Check if Binary String Has at Most One Segment of Ones](#question-1)

#### II. [1785. Minimum Elements to Add to Form a Given Sum](#question-2)

#### III. [1786.  Number of Restricted Paths From First to Last Node](#question-3)

#### IV. [1787. Make the XOR of All Segments Equal to Zero - not solved](#question-4)

#### V. [Summary](#summary)


<div id="question-1"/>

### [1784.  Check if Binary String Has at Most One Segment of Ones](https://leetcode.com/problems/check-if-binary-string-has-at-most-one-segment-of-ones/)

Given a binary string  `s`  **​​​​​without leading zeros**, return  `true`​​​  _if_ `s` _contains  **at most one contiguous segment of ones**_. Otherwise, return  `false`.

**Example 1:**
**Input:** s = "1001"
**Output:** false
**Explanation:** The ones do not form a contiguous segment.

**Example 2:**
**Input:** s = "110"
**Output:** true

**JavaScript Solution 1:**
```js
var checkOnesSegment = function(s) {
    var cnt = 0;
    for(var i = 1; i<s.length;i++)
    {
        if(s[i]==='1' && s[i-1] === '0')
        {
            return false;
        }
    }
    return true;
};
```

**Regex Solution 2: one-line simple**
Check there is no anti-pattern like: `0...01...1`
```js
var checkOnesSegment = function(s) {
    return !s.match(/0+1+/);
};
```

<div id="question-2"/>

### [1785.  Minimum Elements to Add to Form a Given Sum](https://leetcode.com/problems/minimum-elements-to-add-to-form-a-given-sum/)

You are given an integer array  `nums`  and two integers  `limit`  and  `goal`. The array  `nums`  has an interesting property that  `abs(nums[i]) <= limit`.

Return  _the minimum number of elements you need to add to make the sum of the array equal to_ `goal`. The array must maintain its property that  `abs(nums[i]) <= limit`.

Note that  `abs(x)`  equals  `x`  if  `x >= 0`, and  `-x`  otherwise.

**Example 1:**
**Input:** nums = [1,-1,1], limit = 3, goal = -4
**Output:** 2
**Explanation:** You can add -2 and -3, then the sum of the array will be 1 - 1 + 1 - 2 - 3 = -4.

**Example 2:**
**Input:** nums = [1,-10,9,1], limit = 100, goal = 0
**Output:** 1

**Javascript solution:**
Using  `math.ceil()`  helps a lot ! 😄
```js
var minElements = function(nums, limit, goal) {
    var sum =nums.reduce((a,b)=>a+b);
    goal = Math.abs(goal - sum);
    return Math.ceil(goal/ limit);
};
```

See the discussion article here: [JavaScript: Very simple math solution (greedy)](https://leetcode.com/problems/minimum-elements-to-add-to-form-a-given-sum/discuss/1097433/JavaScript-Very-simple-math-solution-(greedy))

<div id="question-3"/>

### [1786.  Number of Restricted Paths From First to Last Node](https://leetcode.com/problems/number-of-restricted-paths-from-first-to-last-node/)

There is an undirected weighted connected graph. You are given a positive integer  `n`  which denotes that the graph has  `n`  nodes labeled from  `1`  to  `n`, and an array  `edges`  where each  `edges[i] = [ui, vi, weighti]`  denotes that there is an edge between nodes  `ui`  and  `vi`  with weight equal to  `weighti`.

A path from node  `start`  to node  `end`  is a sequence of nodes  `[z0, z1,  z2, ..., zk]`  such that  `z0 = start`  and  `zk  = end`  and there is an edge between  `zi`  and  `zi+1`  where  `0 <= i <= k-1`.

The distance of a path is the sum of the weights on the edges of the path. Let  `distanceToLastNode(x)`  denote the shortest distance of a path between node  `n`  and node  `x`. A  **restricted path**  is a path that also satisfies that  `distanceToLastNode(zi) > distanceToLastNode(zi+1)`  where  `0 <= i <= k-1`.

Return  _the number of restricted paths from node_  `1`  _to node_  `n`. Since that number may be too large, return it  **modulo**  `109  + 7`.

**JavaScript solution:**
- Dijkstra + DFS  - Timeout during contest
- Fix: Dijkstra + **DFS(with memo)**
- Improve: Dijkstra + **BFS (DP)**

**See the discussion article here:**  [Javascript: Two ways - Dijkstra+BFS/DFS](https://leetcode.com/problems/number-of-restricted-paths-from-first-to-last-node/discuss/1097404/Javascript-two-ways:-Dijkstra-+-%28-BFS%28DP%29-or-DFS-%28with-memo%29-%29)

<div id="question-4" />

### [1787. Make the XOR of All Segments Equal to Zero - not solved](https://leetcode.com/problems/make-the-xor-of-all-segments-equal-to-zero/)

Not solved !

<div id="summary"/>

### Summary
- Q2 - good and smart math solution !
- Q3: **pure DFS time out,** remember to use memo with DFS
- 2584 / 12900, keep moving!
