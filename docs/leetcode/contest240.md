## Leetcode Contest 240 - JavaScript

#### I. [1854.  Maximum Population Year](#question-1)

#### II. [1855. Maximum Distance Between a Pair of Values](#question-2)

#### III. [1856. Maximum Subarray Min-Product](#question-3)

#### IV. [1857. Largest Color Value in a Directed Graph](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1854.  Maximum Population Year](https://leetcode.com/problems/maximum-population-year/)

You are given a 2D integer array  `logs`  where each  `logs[i] = [birthi, deathi]`  indicates the birth and death years of the  `ith`  person.

The  **population**  of some year  `x`  is the number of people alive during that year. The  `ith`  person is counted in year  `x`'s population if  `x`  is in the  **inclusive**  range  `[birthi, deathi  - 1]`. Note that the person is  **not**  counted in the year that they die.

Return  _the  **earliest**  year with the  **maximum population**_.

**Example 1:**
**Input:** logs = [[1993,1999],[2000,2010]]
**Output:** 1993
**Explanation:** The maximum population is 1, and 1993 is the earliest year with this population.

**My Solution:**  [JS - dynamic count Prev Years in asc order](https://leetcode.com/problems/maximum-population-year/discuss/1199070/JavaScript-WHY-this-problem-is-EASY-when-I-DON%27T-want-to-use-linear-count)

**Most common JavaScript Solution - Linear count:** 
find the advantages:
- code more readable
- no sort time cost

disadvantages:
- memory cost

```js
var maximumPopulation = function(logs) {
    // Total: [1950 - 2050] -> 101 years
    var cnt = new Array(101).fill(0);
    
    // linear count
    for(var i=0; i<logs.length; i++)
    {
        var [birth, death] = logs[i];
        for(var y = birth; y<death; y++)
        {
             cnt[y-1950]++;
        }
    }
    
    // find the earliest Max year
    var max = 0;
    var year = 0;
    for(var i = 0; i<cnt.length; i++)
    {
        if(max < cnt[i])
        {
            max = cnt[i];
            year = i + 1950;
        }
    }
    return year;
};
```

<div id="question-2"/>

### [1855. Maximum Distance Between a Pair of Values](https://leetcode.com/problems/maximum-distance-between-a-pair-of-values/)

You are given two  **non-increasing 0-indexed** integer arrays  `nums1`​​​​​​ and  `nums2`​​​​​​.

A pair of indices  `(i, j)`, where  `0 <= i < nums1.length`  and  `0 <= j < nums2.length`, is  **valid**  if both  `i <= j`  and  `nums1[i] <= nums2[j]`. The  **distance**  of the pair is  `j - i`​​​​.

Return  _the  **maximum distance**  of any  **valid**  pair_ `(i, j)`_. If there are no valid pairs, return_ `0`.

An array  `arr`  is  **non-increasing**  if  `arr[i-1] >= arr[i]`  for every  `1 <= i < arr.length`.

**Example 1:**
**Input:** nums1 = [55,30,5,4,2], nums2 = [100,20,10,10,5]
**Output:** 2
**Explanation:** The valid pairs are (0,0), (2,2), (2,3), (2,4), (3,3), (3,4), and (4,4).
The maximum distance is 2 with pair (2,4).

**Analysis:**
```
nums1: 0, 1, 2, ....i, ....
nums2: 0,... j, .......
Find the first i that nums1[i] <= nums2[j]
because any i+1, i+2, i+3,.... will always <= nums2[j]
```

**JavaScript solution:**
Please see my **discussion article** here: [Javascript - self implemented Binary Search](https://leetcode.com/problems/maximum-distance-between-a-pair-of-values/discuss/1198881/JavaScript-Self-implemented-BinarySearch)

<div id="question-3"/>

### [1856. Maximum Subarray Min-Product](https://leetcode.com/problems/maximum-subarray-min-product/)

The  **min-product**  of an array is equal to the  **minimum value**  in the array  **multiplied by**  the array's  **sum**.

-   For example, the array  `[3,2,5]`  (minimum value is  `2`) has a min-product of  `2 * (3+2+5) = 2 * 10 = 20`.

Given an array of integers  `nums`, return  _the  **maximum min-product**  of any  **non-empty subarray**  of_ `nums`. Since the answer may be large, return it  **modulo**  `109  + 7`.

Note that the min-product should be maximized  **before**  performing the modulo operation. Testcases are generated such that the maximum min-product  **without**  modulo will fit in a  **64-bit signed integer**.

A  **subarray**  is a  **contiguous**  part of an array.

**Example 1:**
**Input:** nums = [1,2,3,2]
**Output:** 14
**Explanation:** The maximum min-product is achieved with the subarray [2,3,2] (minimum value is 2).
2 * (2+3+2) = 2 * 7 = 14.

**JavaScript solution:**
```js
var maxSumMinProduct = function(nums) {
    var mod = BigInt(1000000007);
    var n = nums.length;
    var max = 0n;
    if(n === 1)
    {
        return (nums[0]*nums[0]) % mod;
    }
    var sum = new Array(n+1).fill(0);
    for(var i = 1; i<=n; i++)
    {
        sum[i] = sum[i-1] + nums[i-1];
    }
    for(var i = 0;i<n; i++)
    {
        var l = i-1;
        var r = i+1;
        while(l>=0 && nums[l]>=nums[i])
        {
            l--;
        }
        l++;
        while(r<n && nums[r]>=nums[i])
        {
            r++;
        }
        r--;
        var s = BigInt(sum[r+1] - sum[l]);
        var factor = BigInt(nums[i]);
        var prod = s*factor;
        if(prod > max)
        {
            max = prod;
        }
    }
    return Number(max % mod); 
};
```

See my discussion article here:  [JavaScript - Two pointer check at each index + BigInt](https://leetcode.com/problems/maximum-subarray-min-product/discuss/1198861/JavaScript-Two-pointer-check-at-each-index-%2B-BigInt)

<div id="question-4" />

### [1857. Largest Color Value in a Directed Graph](https://leetcode.com/problems/largest-color-value-in-a-directed-graph/)

**Referenced solution:**
https://leetcode.com/problems/largest-color-value-in-a-directed-graph/discuss/1198641/Python-topological-sort-%2B-DP

**My JS implementation:**
- build ADJ list
- DFS 
	- 1: visiting: for cycle detecting
	- 2: visited: can be use to store the whole visited path: `leaf -> root` order, to collect nodes in array `path: []`
	- 0: not visited
- How to count colors ? - DP
	- loop all possible colors - max 26 colors
	- since we only know the visited node's order, we update `from root to child`, each parent should be counted to all the children nodes
		```
		dp[parent]++;
		for next of adj[parent]:
			dp[next] = max( dp[next], dp[parent] );
		```
	
```js
var largestPathValue = function(colors, edges) {
    var n = colors.length;
    // build Adjacent List
    var adj = new Array(n).fill(null).map(el=> new Array(0));
    for(var e of edges)
    {  
        adj[e[0]].push(e[1]);
    }
    var cycle = false;
    var visited = new Array(n).fill(0);
    var path = [];
    for(var i = 0; i<n; i++)
    {
        if(!visited[i])
        {
            dfs(i);
        }
        if(cycle)
        {
            return -1;
        }
    }
    function dfs(cur)
    {
        if(visited[cur] === 1)
        {
            // visiting
            cycle = true;
            return;
        }
        if(visited[cur] === 2)
        {
            return;
        }
        visited[cur] = 1;
            for(var next of adj[cur])
            {
                dfs(next);
            }
        visited[cur] = 2;
        path.push(cur);
    }
    
    // count color in all paths from parent to children - DP
    path = path.reverse();
    var ans = 0
    for (var i=0; i<26; i++)
    {
            var c = String.fromCharCode(97 + i);
            if(colors.indexOf(c)>=0)
            {
                // dp[i]: max count of "c" until current node as the end
                var dp = new Array(n).fill(0);
                for (var node of path)
                {
                    if(colors[node]===c) {
                        dp[node]++;
                        ans=Math.max(ans,dp[node]);
                    }
                    // update it's next neighbor's dp[next] 
                    for (var next of adj[node]) {
                         var prev = dp[node];
                         dp[next]=Math.max(dp[next], prev);
                    }   
                }     
            } 
     }
    return ans; 
};
```

<div id="question-5"/>

### Summary

- 2953 / 11577
