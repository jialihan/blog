## Leetcode Contest 211 - JavaScript

I. [1624. Largest Substring Between Two Equal Characters](#question-1)

II. [1625. Best Team With No Conflicts](#question-2)

III. [1626. Best Team With No Conflicts](#question-3)

IV. [1627. Graph Connectivity With Threshold](#question-4)

V. [Summary](#summary)

<div id="question-1"/>

### 1624. Largest Substring Between Two Equal Characters

Given a string `s`, return _the length of the longest substring between two equal characters, excluding the two characters._ If there is no such substring return `-1`.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**
**Input:** s = "aa"
**Output:** 0
**Explanation:** The optimal substring here is an empty substring between the two `'a's`.

**Example 2:**
**Input:** s = "abca"
**Output:** 2
**Explanation:** The optimal substring here is "bc".

**My JavaScript Solution:**

```
var maxLengthBetweenEqualCharacters = function(s) {
    let res = -1;
    for (let i = 0; i<s.length; i++)
        {
            const cur = s[i];
            const last = s.lastIndexOf(cur);
            res = Math.max(res, last - i - 1);
        }
    return res;
};
```

<div id="question-2"/>

### 1625. Lexicographically Smallest String After Applying Operations

You are given a string `s` of **even length** consisting of digits from `0` to `9`, and two integers `a` and `b`.

You can apply either of the following two operations any number of times and in any order on `s`:

- Add `a` to all odd indices of `s` **(0-indexed)**. Digits post `9` are cycled back to `0`. For example, if `s = "3456"` and `a = 5`, `s` becomes `"3951"`.
- Rotate `s` to the right by `b` positions. For example, if `s = "3456"` and `b = 1`, `s` becomes `"6345"`.

Return _the **lexicographically smallest** string you can obtain by applying the above operations any number of times on_ `s`.

A string `a` is lexicographically smaller than a string `b` (of the same length) if in the first position where `a` and `b` differ, string `a` has a letter that appears earlier in the alphabet than the corresponding letter in `b`. For example, `"0158"` is lexicographically smaller than `"0190"` because the first position they differ is at the third letter, and `'5'` comes before `'9'`.

**Example 1:**
**Input:** s = "5525", a = 9, b = 2
**Output:** "2050"
**Explanation:** We can apply the following operations:

```
Start:  "5525"
Rotate: "2555"
Add:    "2454"
Add:    "2353"
Rotate: "5323"
Add:    "5222"
​​​​​​​Add:    "5121"
​​​​​​​Rotate: "2151"
​​​​​​​Add:    "2050"​​​​​​​​​​​​
There is no way to obtain a string that is lexicographically smaller then "2050".
```

**Analysis:**

1. think about the DFS a lot, what is the stopping condition to check?
2. Referenced Solution after contest:

- start string
- For each DFS
  - do operation1: add a
    - update min result if needed
    - check visited ? do DFS
  - do operation2: shift b
    - update min result if needed
    - check visited ? do DFS3

3. Pay attention to the string compare in javascript:
   ~~do not use: **s1 > s2**~~
   use `localeCompare()`: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare

4. JavaScript Solution:

```
let res = '';
let visited = new Set();
var findLexSmallestString = function(s, a, b) {
    res = s;
    visited.add(s);
    dfs(s,a,b);
    return res.toString();
};
var dfs = function(s,a,b)
{
    let s1 = add(s,a);
    if(!visited.has(s1))
    {
           visited.add(s1);
           if( s1.localeCompare(res) < 0)
           {
                res = s1;

           }
         // console.log("add: ",res);
            dfs(s1,a,b);
    }
    let s2 = rotate(s,b);
    if(!visited.has(s2))
    {
            visited.add(s2);
           if( s2.localeCompare(res) < 0)
           {
                res = s2;

           }
         // console.log("rotate: ",res);
            dfs(s2,a,b);
    }
    function add(s,a)
    {
        let chs = s.split('');
        for(let i = 1; i<chs.length; i+=2)
        {
            const cur =  chs[i]-0;
            let num =  (cur + a) % 10;
            chs[i] = num.toString();
        }
        return chs.join('');
    }
    function rotate(s, b)
    {
	    return s.slice(b) + s.slice(0,b)
    }
}

```

<div id="question-3"/>

### 1626. Best Team With No Conflicts

You are the manager of a basketball team. For the upcoming tournament, you want to choose the team with the highest overall score. The score of the team is the **sum** of scores of all the players in the team.

However, the basketball team is not allowed to have **conflicts**. A **conflict** exists if a younger player has a **strictly higher** score than an older player. A conflict does **not** occur between players of the same age.

Given two lists, `scores` and `ages`, where each `scores[i]` and `ages[i]` represents the score and age of the `ith` player, respectively, return _the highest overall score of all possible basketball teams_.

**Example 1:**
**Input:** scores = [1,3,5,10,15], ages = [1,2,3,4,5]
**Output:** 34
**Explanation:** You can choose all the players.

**Example 2:**
**Input:** scores = [4,5,6,5], ages = [2,1,2,1]
**Output:** 16
**Explanation:** It is best to choose the last 3 players. Notice that you are allowed to choose multiple people of the same age.
**Analysis:**

- computed a new scores array by sorting increasing by ages

* translated question: **How to find the largest sum in increasing sequence?**
* Take a long time to solve it in array, but it needs DP

**Solution after Contest:**

```
var bestTeamScore = function(scores, ages) {
    let arr = [];
    for(let i = 0; i<scores.length; i++)
    {
            arr.push([scores[i], ages[i]]);
    }
    arr.sort(function comp(a,b){
        if(a[1] === b[1])
        {
           return a[0]-b[0];
        }
        return a[1]-b[1];
    });
    let nums = [];
    for(const [s,a] of arr)
    {
            nums.push(s);
    }

    // find the non descending sum array ?????
    let res = -1;
    let dp = new Array(nums.length);
        for (let i = 0; i < nums.length; i++)
        {
            dp[i] = nums[i];
            for (let j = 0; j < i; ++j){
                if (nums[j] > nums[i])
                    continue;
                dp[i] =  Math.max(dp[i], dp[j] + nums[i]);
            }
            res = Math.max(res, dp[i]);
           //  console.log("cur=",nums[i],"local max= ", res);
        }
    return res;
};
```

<div id="question-4"/>

### 1627. Graph Connectivity With Threshold

We have `n` cities labeled from `1` to `n`. Two different cities with labels `x` and `y` are directly connected by a bidirectional road if and only if `x` and `y` share a common divisor **strictly greater** than some `threshold`. More formally, cities with labels `x` and `y` have a road between them if there exists an integer `z` such that all of the following are true:

- `x % z == 0`,
- `y % z == 0`, and
- `z > threshold`.

Given the two integers, `n` and `threshold`, and an array of `queries`, you must determine for each `queries[i] = [ai, bi]` if cities `ai` and `bi` are connected (i.e. there is some path between them).

Return _an array_ `answer`_, where_ `answer.length == queries.length` _and_ `answer[i]` _is_ `true` _if for the_ `ith` _query, there is a path between_ `ai` _and_ `bi`_, or_ `answer[i]` _is_ `false` _if there is no path._
**Analysis:**

1.  at first the GCD solution have some error case because of the connected graph paths, then this problem needs to be solved by Unin find.
2.  Referenced java solution: [Referenced Another Java Solution](https://leetcode.com/problems/graph-connectivity-with-threshold/discuss/899656/JAVA-easy-to-understand-UF)
3.  translated into JS:

```
var areConnected = function(n, threshold, queries) {
    function find(p, x)
    {
        if(p[x] !== x)
            {
                p[x] = find(p, p[x]);
            }
        return p[x];
    }
    // union fimd
    if(threshold <= 0)
        return new Array(queries.length).fill(true);
    var parent = [];
    for (let i = 0; i <= n; i++)
        parent[i] = i;

    for(let i = threshold + 1; i <= n; i++)
    {
         // i is the common divisor of i and j
          for(let j = 2 * i; j <= n; j += i){
                var p1 = find(parent, i);
                var p2 = find(parent, j);
                if (p1 !== p2)
                {
                    parent[p1] = p2;
                }
            }
    }
    var res = [];
    for(let [p1,p2] of queries)
    {
           if (find(parent, p1) === find(parent, p2))
               {
                   res.push(true);
               }
            else
                {
                    res.push(false);
                }
    }
    return res;
};
```

<div id="summary" />

### Summary

- it's a totally failure! sad!
- algorithm: how to write the correct DFS or BFS
- think about DP: operations to find the best result in sequence of an array.
- 4435 / 11960
