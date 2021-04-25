## Leetcode Contest 238 - JavaScript

#### I. [1837.  Sum of Digits in Base K](#question-1)

#### II. [1838.  Frequency of the Most Frequent Element](#question-2)

#### III. [ 1839.  Longest Substring Of All Vowels in Order](#question-3)

#### IV. [1840.  Maximum Building Height](#question-4)

<div id="question-1"/>

### [1837.  Sum of Digits in Base K](https://leetcode.com/problems/sum-of-digits-in-base-k/)

Given an integer  `n`  (in base  `10`) and a base  `k`, return  _the  **sum**  of the digits of_ `n` _**after**  converting_ `n` _from base_ `10` _to base_ `k`.

After converting, each digit should be interpreted as a base  `10`  number, and the sum should be returned in base  `10`.

**Example 1:**
**Input:** n = 34, k = 6
**Output:** 9
**Explanation:** 34 (base 10) expressed in base 6 is 54. 5 + 4 = 9.

**Example 2:**
**Input:** n = 10, k = 10
**Output:** 1
**Explanation:** n is already in base 10. 1 + 0 = 1.

**JavaScript Solution:** 
```js
var sumBase = function(n, k) {
    let res = 0;
    while (n) {
        res += Math.floor(n % k);
        n = n / k;
    }
    return res;
};
```

<div id="question-2"/>

### [1838.  Frequency of the Most Frequent Element](https://leetcode.com/problems/frequency-of-the-most-frequent-element/)

The  **frequency**  of an element is the number of times it occurs in an array.

You are given an integer array  `nums`  and an integer  `k`. In one operation, you can choose an index of  `nums`  and increment the element at that index by  `1`.

Return  _the  **maximum possible frequency**  of an element after performing  **at most**_ `k` _operations_.

**Example 1:**
**Input:** nums = [1,2,4], k = 5
**Output:** 3 **Explanation:** Increment the first element three times and the second element two times to make nums = [4,4,4].
4 has a frequency of 3.

**JavaScript solution:**
```js
var maxFrequency = function(nums, k) {
    nums.sort((a,b)=>a-b);
    var sum = 0;
    var start = 0;
    var res = 0;
    for(var i= 0; i<nums.length; i++)
    {
        sum += nums[i];
        while(nums[i] * (i-start+1) - sum > k)
        {
            sum -= nums[start];
            start++;
        }
        res = Math.max(res, i-start+1);
    }
    return res;
};
```

Also see my discussion article here: [JS-Sliding-window-solution](https://leetcode.com/problems/frequency-of-the-most-frequent-element/discuss/1175240/JavaScript-Sliding-Window)

<div id="question-3"/>

### [1839.  Longest Substring Of All Vowels in Order](https://leetcode.com/problems/longest-substring-of-all-vowels-in-order/)

A string is considered  **beautiful**  if it satisfies the following conditions:

-   Each of the 5 English vowels (`'a'`,  `'e'`,  `'i'`,  `'o'`,  `'u'`) must appear  **at least once**  in it.
-   The letters must be sorted in  **alphabetical order**  (i.e. all  `'a'`s before  `'e'`s, all  `'e'`s before  `'i'`s, etc.).

For example, strings  `"aeiou"`  and  `"aaaaaaeiiiioou"`  are considered  **beautiful**, but  `"uaeio"`,  `"aeoiu"`, and  `"aaaeeeooo"`  are  **not beautiful**.

Given a string  `word`  consisting of English vowels, return  _the  **length of the longest beautiful substring**  of_ `word`_. If no such substring exists, return_ `0`.

A  **substring**  is a contiguous sequence of characters in a string.

**Example 1:**
**Input:** word = "aeiaaioaaaaeiiiiouuuooaauuaeiu"
**Output:** 13
**Explanation:** The longest beautiful substring in word is "aaaaeiiiiouuu" of length 13.

**Example 2:**
**Input:** word = "aeeeiiiioooauuuaeiou"
**Output:** 5
**Explanation:** The longest beautiful substring in word is "aeiou" of length 5.

**JS solution:**
See my discussion article here:
[JavaScript: Encode String & Regex solution](https://leetcode.com/problems/longest-substring-of-all-vowels-in-order/discuss/1175214/JavaScript-Encode-string-by-count-%2B-Regex)

<div id="question-4" />

### [1840.  Maximum Building Height](https://leetcode.com/problems/maximum-building-height/)

You want to build  `n`  new buildings in a city. The new buildings will be built in a line and are labeled from  `1`  to  `n`.

However, there are city restrictions on the heights of the new buildings:

-   The height of each building must be a non-negative integer.
-   The height of the first building  **must**  be  `0`.
-   The height difference between any two adjacent buildings  **cannot exceed**  `1`.

Additionally, there are city restrictions on the maximum height of specific buildings. These restrictions are given as a 2D integer array  `restrictions`  where  `restrictions[i] = [idi, maxHeighti]`  indicates that building  `idi`  must have a height  **less than or equal to**  `maxHeighti`.

It is guaranteed that each building will appear  **at most once**  in  `restrictions`, and building  `1`  will  **not**  be in  `restrictions`.

Return  _the  **maximum possible height**  of the  **tallest**  building_.

```js
var maxBuilding = function(n, restrictions) {
    var m = restrictions.length;
    if(!m)
    {
        return n-1;
    }
    restrictions.sort((a,b)=>a[0]-b[0]);
    var dp = new Array(m).fill(0);
    // from left to right
    dp[0] = Math.min(restrictions[0][0]-1, restrictions[0][1]);
    for(var i = 1; i<m; i++)
    {
        dp[i] = Math.min(dp[i-1] + restrictions[i][0]-restrictions[i-1][0],restrictions[i][1]);
    }
    // from right to left
    for(var i = m-2; i>=0; i--)
    {
        dp[i] = Math.min(dp[i], dp[i+1] + restrictions[i+1][0]-restrictions[i][0]);
    }
    // find max
    var max = Math.max(dp[0], dp[m-1]+ n - restrictions[m-1][0]); // 1st Restricted Building OR last Resticted Building until the end
    for(var i = 1; i<m; i++)
    {
        // between [i-1,...i] the highest is the middle part, height = max(i,i-1) + (interval - hegith_diff) / 2
        var middleHeight = Math.max(dp[i], dp[i-1]) + Math.floor((restrictions[i][0] - restrictions[i-1][0] - Math.abs(dp[i]-dp[i-1])) / 2); 
        max = Math.max(max, middleHeight);
    }
    return max;   
};
```

**Also you could see my discussion article here:**
[JavaScript - DP](https://leetcode.com/problems/maximum-building-height/discuss/1175352/JavaScript-DP-with-explanation)
