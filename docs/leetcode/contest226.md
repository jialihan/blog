## Leetcode Contest 226- JavaScript

#### I.  [1742.  Maximum Number of Balls in a Box](#question-1)

#### II. [1743.  Restore the Array From Adjacent Pairs](#question-2)

#### III. [1744.  Can You Eat Your Favorite Candy on Your Favorite Day?](#question-3)

#### IV. [1745. Palindrome Partitioning IV](#question-4)

#### V. [Summary](#summary)


<div id="question-1"/>

### [1742.  Maximum Number of Balls in a Box](https://leetcode.com/problems/maximum-number-of-balls-in-a-box/)

You are working in a ball factory where you have  `n`  balls numbered from  `lowLimit`  up to  `highLimit`  **inclusive**  (i.e.,  `n == highLimit - lowLimit + 1`), and an infinite number of boxes numbered from  `1`  to  `infinity`.

Your job at this factory is to put each ball in the box with a number equal to the sum of digits of the ball's number. For example, the ball number  `321`  will be put in the box number  `3 + 2 + 1 = 6`  and the ball number  `10`  will be put in the box number  `1 + 0 = 1`.

Given two integers  `lowLimit`  and  `highLimit`, return _the number of balls in the box with the most balls._

**Example 1:**
**Input:** lowLimit = 1, highLimit = 10
**Output:** 2
**Explanation:**
Box Number:  1 2 3 4 5 6 7 8 9 10 11 ...
Ball Count:  2 1 1 1 1 1 1 1 1 0  0  ...
Box 1 has the most number of balls with 2 balls.

Cleaned **JavaScript Solution:**
```js
/**
 * @param {number} lowLimit
 * @param {number} highLimit
 * @return {number}
 */
var countBalls = function(lowLimit, highLimit) {
    var cnt = new Array(1025).fill(0);
    for (var num = lowLimit; num <= highLimit; num++) {
            var sum = 0;
            var n = num;
            while(n > 0)
            {
                sum += n % 10;
                n = Math.floor(n / 10);
            }
           cnt[sum]++;
       }
     return Math.max(...cnt);
};
```

<div id="question-2"/>

### [1743.  Restore the Array From Adjacent Pairs](https://leetcode.com/problems/restore-the-array-from-adjacent-pairs/)

There is an integer array  `nums`  that consists of  `n`  **unique** elements, but you have forgotten it. However, you do remember every pair of adjacent elements in  `nums`.

You are given a 2D integer array  `adjacentPairs`  of size  `n - 1`  where each  `adjacentPairs[i] = [ui, vi]`  indicates that the elements  `ui`  and  `vi`  are adjacent in  `nums`.

It is guaranteed that every adjacent pair of elements  `nums[i]`  and  `nums[i+1]`  will exist in  `adjacentPairs`, either as  `[nums[i], nums[i+1]]`  or  `[nums[i+1], nums[i]]`. The pairs can appear  **in any order**.

Return  _the original array_ `nums`_. If there are multiple solutions, return  **any of them**_.

**Example 1:**

**Input:** adjacentPairs = [[2,1],[3,4],[3,2]]
**Output:** [1,2,3,4]
**Explanation:** This array has all its adjacent pairs in adjacentPairs.
Notice that adjacentPairs[i] may not be in left-to-right order.

**Steps & Analysis:**
- find the Root: the adjacent list **only** have **1 neighbor**
- DFS find the path

**JavaScript Solution:**
```js
/**
 * @param {number[][]} adjacentPairs
 * @return {number[]}
 */
var restoreArray = function(pairs) {
    var map = new Map();
    for(var p of pairs)
    {
        if(!map.has(p[0]))
        {
            map.set(p[0], []);
        }
        if(!map.has(p[1]))
        {
            map.set(p[1], []);
        }
        map.get(p[0]).push(p[1]);
        map.get(p[1]).push(p[0]); 
    }
    var root = 0;
    for(var [key,val] of map.entries())
    {
        if(val.length === 1)
        {
            root = key;
            break;
        }
    }

    var n = pairs.length+1;
    function dfs(map,visited, cur)
    {
        if(res.length===n)
        {
            return;
        }
        if(visited.has(cur))
        {
            return;
        }
        res.push(cur);
        visited.add(cur);
        for(var next of map.get(cur))
        {
            dfs(map, visited, next);
        }
    }
    var visited = new Set();
    var res = [];
    dfs(map, visited, root);
    return res;
};
```

<div id="question-3"/>

### [1744.  Can You Eat Your Favorite Candy on Your Favorite Day?](https://leetcode.com/problems/can-you-eat-your-favorite-candy-on-your-favorite-day/)

You are given a  **(0-indexed)**  array of positive integers  `candiesCount`  where  `candiesCount[i]`  represents the number of candies of the `ith` type you have. You are also given a 2D array  `queries`  where  `queries[i] = [favoriteTypei, favoriteDayi, dailyCapi]`.

You play a game with the following rules:

-   You start eating candies on day  `**0**`.
-   You  **cannot**  eat  **any**  candy of type  `i`  unless you have eaten  **all**  candies of type  `i - 1`.
-   You must eat  **at least**  **one**  candy per day until you have eaten all the candies.

Construct a boolean array  `answer`  such that  `answer.length == queries.length`  and  `answer[i]`  is  `true`  if you can eat a candy of type  `favoriteTypei`  on day  `favoriteDayi`  without eating  **more than**  `dailyCapi`  candies on  **any**  day, and  `false`  otherwise. Note that you can eat different types of candy on the same day, provided that you follow rule 2.

Return  _the constructed array_ `answer`.

**Analysis:**
Very hard to read and understand the meaning of the title.
Two false edge case:
- rule 2: eat too much every day, but still cannot consume `type (i-1)`
- rule 3: eat the least one candy per day,  but cannot support until the `day`. 

**JavaScript Solution:**
```js
/**
 * @param {number[]} candiesCount
 * @param {number[][]} queries
 * @return {boolean[]}
 */
var canEat = function(candiesCount, queries) {
    var cnt = [...candiesCount];
    var preSum = [0];
    var n = candiesCount.length;
    for(var i = 1; i<=n;i++)
    {
        preSum.push(preSum[i-1]+candiesCount[i-1]);
    }
    var res = [];
    for(var q of queries)
    {
        var type = q[0] + 1;
        var day = q[1] + 1;
        // rule 2: type
        if(q[2] * day <= preSum[type-1])
        {
            res.push(false);
            continue;
        }
        // rule 3: at least one candy per day
        if(day-1 >= preSum[type])
        {
            res.push(false);
            continue;
        }
        res.push(true);
    }
    return res; 
};
```

<div id="question-4"/>

### [1745. Palindrome Partitioning IV](https://leetcode.com/problems/palindrome-partitioning-iv/)

Given a string  `s`, return  `true`  _if it is possible to split the string_  `s`  _into three  **non-empty**  palindromic substrings. Otherwise, return_ `false`.​​​​​

A string is said to be palindrome if it the same string when reversed.

**Example 1:**
**Input:** s = "abcbdd"
**Output:** true
**Explanation:** "abcbdd" = "a" + "bcb" + "dd", and all three substrings are palindromes.

**Example 2:**
**Input:** s = "bcbddxy"
**Output:** false
**Explanation:** s cannot be split into 3 palindromes.

Analysis:
- generate the dp[][] matrix in JS:
	```js
	 var dp = new Array(n).fill(null);
	 dp = dp.map(el=>new Array(n).fill(false));
	```
- DP

**JavaScript Solution:**
```js
var checkPartitioning = function(s) {
    var n = s.length;
    // dp[i][j]: is panlindrom from i to j, included 
    var dp = new Array(n).fill(null);
    dp = dp.map(el=>new Array(n).fill(false));
    
    for(var i=0;i<n;i++)
    {
        dp[i][i] = true;
        if(i<n-1 && s[i] === s[i+1])
        {
            dp[i][i+1] = true;
        }
    }
    for(var len = 3; len<=n; len++)
    {
        for(var i = 0; i<n-2; i++)
        {
            var j=i+len-1;
            dp[i][j] =  s[i] === s[j] && dp[i+1][j-1];
            
        }
    }
    for(var i = 1; i<n;i++)
        for(var j = i+1; j<n; j++)
        {
            if(dp[0][i-1] && dp[i][j-1] && dp[j][n-1])
                {
                    return true;
                }
        }
    return false; 
};
```

### Summary

- DP array `dp[n][n]`:  use `map()` to create the new object , **~~spread operator is wrong~~** !!!
	```js
	// wrong !!!
	var dp = [...Array(n)].fill([...Array(n)]);
	```
- Problems: Palindrome Partitioning **I, II, III, IV**
- keep moving !!! 