##  String -  Palindrome Partitioning I, II, III, IV

- <font size="4">DFS: find all possible partition ways</font>
- <font size="4">DP: can partition or not <br />
	`DP[i][j]`: from index i to index j, substring is a panlindrome<font size="4">

#### [131. Palindrome Partitioning](#question-1)
- [Analysis and break down problem](#q1-1)
- [Javascript solution - DFS](#q1-2)

#### [132.  Palindrome Partitioning II](#question-2)
- [Analysis and break down problem](#q2-1)
- [Javascript solution - two DP](#q2-2)

#### [1278.  Palindrome Partitioning III](#question-3)
- [solution 1: DP + DFS](#q3-1)
- [solution 2 : two DP](#q3-2)

#### [1745.  Palindrome Partitioning IV](#question-4)
- [Analysis & break down problem](#q4-1)
- [Javascript solution - DP](#q4-2)


<div id="question-1"/>

### 131.  [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

Given a string  `s`, partition  `s`  such that every substring of the partition is a  **palindrome**. Return all possible palindrome partitioning of  `s`.

A  **palindrome**  string is a string that reads the same backward as forward.

**Example 1:**
**Input:** s = "aab"
**Output:** [["a","a","b"],["aa","b"]]

**Example 2:**
**Input:** s = "a"
**Output:** [["a"]]

<div id="q1-1" />

#### 1.1 Analysis and break down problem
- find all solutions: use DFS
- DFS in Javascript

<div id="q1-2" />

#### 1.2 Javascript solution - DFS

```js
var partition = function(s) {
    // find all solutions, use DFS
    var n = s.length;
    var res = [];
    function isPanlindrome(l,r)
    {
        while(l<r)
        {
            if(s[l] !==s[r])
            {
                return false;
            }
            l++;
            r--;
        }
        return true;
    }
    function dfs(s, start, cur)
    {
       if(start >= n)
       {
           res.push([...cur]);
           return;
       }
       for(var end = start; end<n; end++)
       {
           if(isPanlindrome(start, end))
           {
               cur.push(s.slice(start, end+1));
               dfs(s, end+1, cur);
               cur.pop();
           }
       }
    }
    dfs(s, 0, []);
    return res;
};
```

<div id="question-2"/>

### [132.  Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

Given a string  `s`, partition  `s`  such that every substring of the partition is a palindrome.

Return  _the minimum cuts needed_  for a palindrome partitioning of  `s`.

**Example 1:**
**Input:** s = "aab"
**Output:** 1
**Explanation:** The palindrome partitioning ["aa","b"] could be produced using 1 cut.

**Example 2:**
**Input:** s = "a"
**Output:** 0

<div id="q2-1"/>

#### 2.1 Analysis and break down problem
- Step1: `dp[i][j]`: build the substring `[i, j]`, is a pandlindrome
	```js
	dp[i][j] =  s[i] === s[j] && dp[i+1][j-1];
	```
- Step2: find the min cut, another `dp2[i]`:  from `[0, i]` the number of the minimum cuts
	```js
	dp2[i] = minCut in (dp[0] ...dp[i-1]) + 1;
	```
	
<div id="q2-2"/>

#### 2.2 Javascript solution - two DP
```js
var minCut = function(s) {
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
    // dp2[] : min cut from 0 to index i
    var dp2 = [...Array(n)].fill(Infinity);
    dp2[0] = 0;
    for(var end = 1; end<n; end++)
    {
        if(dp[0][end])
        {
            dp2[end] = 0;
        }
        else
        {
            var minCut = Infinity;
            for(var start = end; start>0; start--)
            {
                if(dp[start][end])
                {
                    minCut = Math.min(dp2[start-1], minCut);
                }
            }
            dp2[end] = minCut+1;
        }
    }
    return dp2[n-1];
};
```

<div id="question-3"/>

### [1278.  Palindrome Partitioning III](https://leetcode.com/problems/palindrome-partitioning-iii/)

You are given a string `s`  containing lowercase letters and an integer  `k`. You need to :

-   First, change some characters of  `s` to other lowercase English letters.
-   Then divide  `s` into  `k`  non-empty disjoint substrings such that each substring is palindrome.

Return the minimal number of characters that you need to change to divide the string.

**Example 1:**
**Input:** s = "abc", k = 2
**Output:** 1
**Explanation:** You can split the string into "ab" and "c", and change 1 character in "ab" to make it palindrome.

**Example 2:**
**Input:** s = "aabbc", k = 3
**Output:** 0
**Explanation:** You can split the string into "aa", "bb" and "c", all of them are palindrome.

<div id="q3-1"/>

#### 3.1 DP + DFS with Memo
- `dp[i][j]`: from i to j the min letter need to replace
- DFS: to find the min cuts from index 0 to  index n

Javascript solution:
```js
var palindromePartition = function(s, k) {
    var n = s.length;
    if(k===n) 
        return 0;
    var dp = new Array(n).fill(0);
    dp = dp.map(el=>new Array(n).fill(0));
    // dp[i][j]: from i to j the min letter need to replace
    for(var len = 2; len<=n;len++)
    {
        for(var i = 0; i<n-1; i++)
        {
            var j = i + len-1;
            if(j>=n)
                continue;
            if(len === 2)
            {
                dp[i][j] = s[i] === s[j] ? 0 : 1;
            }
            else
            {
                dp[i][j] = dp[i+1][j-1] + (s[i] === s[j] ? 0 : 1 ); 
            }
        }
    }

    var memo = new Array(n).fill(-1).map(el=>{
        return new Array(n).fill(-1).map(el=>new Array(k));
    });
    function dfs(memo, dp, start,end, group)
    {
        if(group === 1 )
            return dp[start][end];
        if(memo[start][end][group])
        {
            return memo[start][end][group];
        }
        var min = Infinity;
        // remaining at least (group - 1), i at most (n-group)
        for(var i = start+1; i<=n-group+1; i++)
        {
            min = Math.min(min, dp[start][i-1]+dfs(memo, dp, i, end, group-1));
        }
        memo[start][end][group] = min;
       //  console.log("min:", min);
        return  memo[start][end][group];
    }
    return dfs(memo,dp, 0, n-1, k);
    
    
};
```

<div id="q3-2"/>

#### 3.2 two DP
- `dp[i][j]`: from i to j the min letter need to replace
- `dp[k][i]`:  the min letters from 0 to i in k groups
	- when k = 1,  `dp2[1][i] = dp[0][i];`, string is from `[0, i]`.
	- when k >=2, need to find the min from index `[0,...i-1]`.
		```js
		for j = 0 to i-1
		{
			dp2[k][i] = Math.min(dp2[k][i], dp2[k-1][j]+dp[j+1][i]);
		}
		```
**Javascript solution:**
```js
var palindromePartition = function(s, k) {
    var n = s.length;
    if(k===n) 
        return 0;
    var dp = new Array(n).fill(0);
    dp = dp.map(el=>new Array(n).fill(0));
    // dp[i][j]: from i to j the min letter need to replace
    for(var len = 2; len<=n;len++)
    {
        for(var i = 0; i<n-1; i++)
        {
            var j = i + len-1;
            if(j>=n)
                continue;
            if(len === 2)
            {
                dp[i][j] = s[i] === s[j] ? 0 : 1;
            }
            else
            {
                dp[i][j] = dp[i+1][j-1] + (s[i] === s[j] ? 0 : 1 ); 
            }
        }
    }
    // dp2[k][i]: k group at the end of i the min letters to replace
    var dp2 = new Array(k+1).fill(0);
    dp2 = dp2.map(el=>new Array(n).fill(n));
    for(var i = 0; i<n; i++)
    {
            dp2[1][i] = dp[0][i];
    }
    for(var t = 2; t<=k; t++)
        for(var i = 0; i<n; i++)
            for(var j = 0; j<i; j++)
            {
                dp2[t][i] = Math.min(dp2[t][i], dp2[t-1][j]+dp[j+1][i]);
            } 
    return dp2[k][n-1];
};
```

<div id="question-4"/>

### [1745.  Palindrome Partitioning IV](https://leetcode.com/problems/palindrome-partitioning-iv/)

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

<div id="q4-1"/>

#### 4.1 Analysis & problem break down
 - `dp[i][j]`:  the substring `[i, j]` is a pandlindrome, true or false.
 - find 2 cuts whether partition into 3 groups at index `i`
	```js
	if(dp[0][i-1] && dp[i][j-1] && dp[j][n-1])
	{
		return true;
	}
	```

<div id="q4-2"/>

#### 4.2 Javascript solution - DP
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