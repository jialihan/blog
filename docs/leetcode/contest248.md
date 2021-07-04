## Leetcode Contest 248 - JavaScript

#### I. [1920.  Build Array from Permutation](#question-1)

#### II. [1921. Eliminate Maximum Number of Monsters](#question-2)

#### III. [1922. Count Good Numbers](#question-3)

#### IV. [1923. Longest Common Subpath - NOT solved !](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1920.  Build Array from Permutation](https://leetcode.com/problems/build-array-from-permutation/)

Given a  **zero-based permutation**  `nums`  (**0-indexed**), build an array  `ans`  of the  **same length**  where  `ans[i] = nums[nums[i]]`  for each  `0 <= i < nums.length`  and return it.

A  **zero-based permutation**  `nums`  is an array of  **distinct**  integers from  `0`  to  `nums.length - 1`  (**inclusive**).

**Example 1:**
**Input:** nums = [0,2,1,5,3,4]
**Output:** [0,1,2,4,5,3] **Explanation:** The array ans is built as follows: 
ans = [nums[nums[0]], nums[nums[1]], nums[nums[2]], nums[nums[3]], nums[nums[4]], nums[nums[5]]]
    = [nums[0], nums[2], nums[1], nums[5], nums[3], nums[4]]
    = [0,1,2,4,5,3]
    
**My JavaScript Solution:**
```js
var buildArray = function(nums) {
    var arr = [...nums];
    var n = nums.length;
    for(var i= 0; i<n; i++)
    {
       arr[i] = nums[nums[i]];
    }
    return arr;
};  
```

<div id="question-2"/>

### [1921. Eliminate Maximum Number of Monsters](https://leetcode.com/problems/eliminate-maximum-number-of-monsters/)

**Reference:** 
- [C++|| EASY TO UNDERSTAND|| SORTING || BEGINNER FRIENDLY](https://leetcode.com/problems/eliminate-maximum-number-of-monsters/discuss/1314346/c-easy-to-understand-sorting-beginner-friendly)
- [BigInt range- MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)

**Complains** from myself: 这什么破阅读理解语文题，我tm读题都读了20分钟， 到最后都没理解这kill monster 啥意思。。。搞什么阅读理解题。

**My JavaScript Version:**
```js
var eliminateMaximum = function(dist, speed) {
    var ans = 0;
    var curTime = 0; // kill monster at time 0
    var n = dist.length;
    // do i need floor or ceil the time? => yes, ceil the time
    var time = dist.map((d,i)=>Math.ceil(d/speed[i])).sort((a,b)=>a-b); 
    for(var i = 0; i < n; i++)
    {
        if(curTime < time[i])
        {
            ans++;
        }
        else
        {
            break;
        }
        curTime++;
    }
    return ans;
};
```

<div id="question-3"/>

### [1922. Count Good Numbers](https://leetcode.com/contest/weekly-contest-248/problems/count-good-numbers/)

A digit string is  **good**  if the digits  **(0-indexed)**  at  **even**  indices are  **even**  and the digits at  **odd**  indices are  **prime**  (`2`,  `3`,  `5`, or  `7`).

-   For example,  `"2582"`  is good because the digits (`2`  and  `8`) at even positions are even and the digits (`5`  and  `2`) at odd positions are prime. However,  `"3245"`  is  **not**  good because  `3`  is at an even index but is not even.

Given an integer  `n`, return  _the  **total**  number of good digit strings of length_ `n`. Since the answer may be large,  **return it modulo** `109  + 7`.

A  **digit string**  is a string consisting of digits  `0`  through  `9`  that may contain leading zeros.

**Example 1:**
**Input:** n = 1
**Output:** 5
**Explanation:** The good numbers of length 1 are "0", "2", "4", "6", "8".

**Referenced Solution LC. 1915:**
- [5^(even places) * 4^(odd places) Use divide and conquer to calculate powers](https://leetcode.com/problems/count-good-numbers/discuss/1314577/5(even-places)-*-4(odd-places)-Use-divide-and-conquer-to-calculate-powers)
- to save time and avoid duplicate to calc `pow(n)`, must use recursion with memo to cut in half to compute.
- my own article: [JS recursion to divide in half for Math.pow](https://leetcode.com/problems/count-good-numbers/discuss/1314585/JavaScript-recursion-to-divide-in-half-to-calc-math.pow)

**My JS Solution:**
```js
var countGoodNumbers = function(n) {
    var mod = 1000000007n;
    var even = Math.ceil(n/2);
    var odd = n - even; 
    function myPow(x,y)
    {
        if(y===0)
        {
            return 1n;
        }
        var res = 1n;
        res *= myPow(x, Math.floor(y/2));
        // console.log("half pow = " + res);
        res *= res;
        if(y%2===1)
        {
            res *= BigInt(x);
        }
        res %= mod;
        return res;
    }
    var ans = myPow(5, even) * myPow(4, odd);
    ans %= mod; 
    return Number(ans);
};

```

<div id="question-4" />

### [1923. Longest Common Subpath](https://leetcode.com/problems/longest-common-subpath/)

There is a country of  `n`  cities numbered from  `0`  to  `n - 1`. In this country, there is a road connecting  **every pair**  of cities.

There are  `m`  friends numbered from  `0`  to  `m - 1`  who are traveling through the country. Each one of them will take a path consisting of some cities. Each path is represented by an integer array that contains the visited cities in order. The path may contain a city  **more than once**, but the same city will not be listed consecutively.

Given an integer  `n`  and a 2D integer array  `paths`  where  `paths[i]`  is an integer array representing the path of the  `ith`  friend, return  _the length of the  **longest common subpath**  that is shared by  **every**  friend's path, or_ `0` _if there is no common subpath at all_.

A  **subpath**  of a path is a contiguous sequence of cities within that path.

**Example 1:**
**Input:** n = 5, paths = [[0,1,2,3,4],
                       [2,3,4],
                       [4,0,1,2,3]]
**Output:** 2
**Explanation:** The longest common subpath is [2,3].

**Example 2:**
**Input:** n = 3, paths = [[0],[1],[2]]
**Output:** 0
**Explanation:** There is no common subpath shared by the three paths.

**Example 3:**
**Input:** n = 5, paths = [[0,1,2,3,4],
                       [4,3,2,1,0]]
**Output:** 1
**Explanation:** The possible longest common subpaths are [0], [1], [2], [3], and [4]. All have a length of 1.

**NOT solved!**

reference - Memory failure
https://leetcode.com/problems/longest-common-subpath/discuss/1314442/Java-Trie-(TLE)


<div id="question-5"/>

### Summary

- ..... (such a big failure this week)
- defeated at the Q2 😅  and give a thumb down on the Q2 ! hard to understand the meaning of the problem
- Q4- hard to solve, not solved! Trie solution get Memory Exceed
- This week i am so happy and handling with my job offer, uhm, i didn't do well in this contest, sad, but it forces me to go back working on algorithms, next week hope I could do it well !!! 😭😭😭