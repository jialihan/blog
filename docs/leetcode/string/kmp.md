##  String -  suffix & prefix - KMP algorithm

#### [1. Build Pattern: Longest prefix in suffix in a single string](#question-1)
- [happy path: matched](#q1-1)
- [Not matched, prefix go back to compare](#q1-2)
- [complete code: Leetcode 1392](#q1-3)

#### [2. Find SubString in String: a needle of a haystack](#question-2)
- [Problem: 28. Implement strStr()](#q2-1)
- [Analysis and break down ](#q2-2)
- [Javascript KMP solution](#q2-3)


#### [3. Other KMP - problems](#question-3)
- [Leetcode 459 - easy](#q3-1)
- [Leetcode 214 - Shortest Palindrome - hard](#q3-2)
- [Leetcode 572 - Subtree of Another Tree - easy](#q3-3)

<div id="question-1"/>

### 1. Build Pattern: Longest prefix in suffix in a single string

**Goal: build the suffix array**

```js
// i = 0 .. n-1
lps[i] = the length matched from the beginning character, based on 1.
```

<div id="q1-1"/>

#### 1.1 happy path: matched
Initial condition:
- Pattern string: `ABCAB`
- suffix index:  `j = 1`
- prefix index : `i = 0`

When `s[i] === p[j]`:
```
if(s[i]===s[j])
{
	lps[j] = i + 1; // length is 1 based
	i++;
	j++;
}
```
Result: 
`lps = [0, 0, 0, 1, 2]`

![image](../../assets/kmp-buildpattern.png ':size=511x189')

<div id="q1-2"/>

#### 1.2  Not matched, prefix go back to compare
- Only prefix index `i` goes back
- suffix index `j` is increasing all the time
- save time to compare, better than bruteforce

When `s[i] !== p[j]`:
```
if(s[i] !== s[j])
{
	if(i===0)
	{ // prefix "i" cannot go back anymore
		lps[j] = 0;
		j++;
	}
	else
	{
		i = lps[i-1];
	}
}
```
**Result:**

![image](../../assets/kmp-build-pattern-notmatch.png ':size=889x216')

<div id="q1-3"/>

#### 1.3 complete code: [Leetcode 1392](https://leetcode.com/problems/longest-happy-prefix/)
JavaScript solution:
```js
var longestPrefix = function(s) {
    var n = s.length;
    var lps = new Array(n).fill(0);
    var i = 0; // prefix
    var j = 1; // suffix
    while(j < n)
    {
        if(s[i] !== s[j])
        {
            if(i===0)
            { 
                // must start at the beginning
                lps[j] = 0;
                j++;
            }
            else
            {
                // prefix goes back
                i = lps[i-1];
            }
        }
        else
        { // matched
            lps[j] = i + 1; // matched length is 1 based 
            i++;
            j++;
        }
    }
    return s.slice(n - lps[n-1], n);  
};
````
<div id="question-2"/>

### 2. Find SubString in String: a needle of a haystack

<div id="q2-1"/>

#### 2.1 Problem: [28. Implement strStr()](https://leetcode.com/problems/implement-strstr/)

Return the index of the first occurrence of needle in haystack, or  `-1`  if  `needle`  is not part of  `haystack`.

**Clarification:**

What should we return when  `needle`  is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when  `needle`  is an empty string. This is consistent to C's [strstr()](http://www.cplusplus.com/reference/cstring/strstr/)  and Java's [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)).

**Example 1:**
**Input:** haystack = "hello", needle = "ll"
**Output:** 2

**Example 2:**
**Input:** haystack = "aaaaa", needle = "bba"
**Output:** -1

<div id="q2-2"/>

#### 2.2 Analysis and break down 
- Step1: build pattern string into **suffix array**: `lps[n]`
- Step2: find p_string in s_string
	- prefix:  `j` index in pattern (needle string), only `j` goes back
	- suffix: `i` index in string (haystack string)

**Attention to edge cases:** empty string
-   needle is empty "", return 0, find always at index 0
-   haystack is empty "", return -1, cannot find

<div id="q2-3"/>

#### 2.3 Javascript KMP solution
```js
var strStr = function(haystack, needle) {
	// step1: omit the detail code
	var lps = buildSuffixArray(needle);
    // step2
    var j = 0; // prefix
    var i = 0; // suffix
    var n = haystack.length;
    var m = needle.length;
    if(m === 0) // edge case
        return 0;
    if(n === 0) // edge case
        return -1;
    while(i < n)
    {
        if(haystack[i] === needle[j])
        {
            i++;
            j++;
        }
        else
        {
            if(j>0)
            {
                // prefix goes back to prev max matched length
                j = lps[j-1];
            }
            else
            {
                // must re-start compare at beginning
                i++;
            }
        }
        if(j === m)
        {
            // find
            return (i - j);
        }
    }
    return -1; // not found
};
```

<div id="question-3"/>

### 3. Other KMP - problems

<div id="q3-1"/>

#### 3.1 Leetcode 459 - Repeated Substring Pattern

Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.

**Example 1:**
**Input:** "abab"
**Output:** True
**Explanation:** It's the substring "ab" twice.

**Example 2:**
**Input:** "aba"
**Output:** False

**Analysis:**

![image](../../assets/lc459.png ':size=587x229')

Goal:
to find the longest LPS, then the valid "true" condition are:
- only when there is a valid LPS at the ending index: `lps[n-1] > 0`.
- only when the remaining part `[n - LPS]`, which also is the **smallest repeated cycle** here, can be module by n.
```
return lps[n-1] > 0 && (n % lps[n-1] === 0);
```

**Javascript solution:**
```js
var repeatedSubstringPattern = function(s) {
    var n = s.length;
    var lps = new Array(n).fill(0);
    var i = 0; // prefix
    var j = 1; // suffix
    while(j < n)
    {
        if(s[i] !== s[j])
        {
            if(i===0)
            { 
                // must start at the beginning
                lps[j] = 0;
                j++;
            }
            else
            {
                // prefix goes back
                i = lps[i-1];
            }
        }
        else
        { // matched
            lps[j] = i + 1; // matched length is 1 based 
            i++;
            j++;
        }
    }
    return lps[n-1] > 0 && (n % lps[n-1] === 0);
};
```

<div id="q3-2"/>

#### 3.2 Leetcode 214 - Shortest Palindrome

Given a string  _**s**_, you can convert it to a palindrome by adding characters in front of it. Find and return the shortest palindrome you can find by performing this transformation.

**Example 1:**
**Input:** s = "aacecaaa"
**Output:** "aaacecaaa"

**Example 2:**
**Input:** s = "abcd"
**Output:** "dcbabcd"

**Analysis:**

![image](../../assets/lc214.png ':size=778x228')

Goal:
- Transform this problem to be: **"find the longest palindrome prefix in this string"**.
- And this target string should be in the format: `A' | A | B`
- Try to find the rules in reverse string:
	s:            `A' | A | B`
	reverse: `B' | A'| A`
- the problem transform the 2nd problem: "find the longest suffix prefix in the new string"
	`A'| A| B| B'| A'| A`
	we need to find the LPS: `A'A string`, use KMP here
- **result** is: find the `B' string` part, and insert at the beginning of the original string.

**JavaScript solution:**
```js
var shortestPalindrome = function(s) {
    // reverse string
    var r = s.split('').reverse().join('');
    var str = s + "*" + r; 
    // find the LPS in str
    var n = str.length;
    var lps = new Array(n).fill(0);
    var i = 0; // prefix
    var j = 1; // suffix
    while(j < n)
    {
        if(str[i] !== str[j])
        {
            if(i===0)
            { 
                // must start at the beginning
                lps[j] = 0;
                j++;
            }
            else
            {
                // prefix goes back
                i = lps[i-1];
            }
        }
        else
        { // matched
            lps[j] = i + 1; // matched length is 1 based 
            i++;
            j++;
        }
    }
    var insertStr = r.slice(0, s.length - lps[n-1]);
    return (insertStr + s); 
};
```

<div id="q3-3"/>

#### 3.3 Leetcode 572 - Subtree of Another Tree

Given two  **non-empty**  binary trees  **s**  and  **t**, check whether tree  **t**  has exactly the same structure and node values with a subtree of  **s**. A subtree of  **s**  is a tree consists of a node in  **s**  and all of this node's descendants. The tree  **s**  could also be considered as a subtree of itself.

**Example 1:**  
Given tree s:

     3
    / \
   4   5
  / \
 1   2

Given tree t:

   4 
  / \
 1   2

Return  **true**, because t has the same structure and node values with a subtree of s.

- Solution 1: [recursive - brute force](https://leetcode.com/problems/subtree-of-another-tree/discuss/1053387/Javascript-recursive-solution)
- Solution 2: KMP
	- inorder traversal: push `-1` when **child node is null**
	- build kmp to pattern array, note: here pattern is a array of numbers.
	- find p in s

**Step1:** process `inorder` traversal

```js
var arr = [];
function inorder(node)
{
	if(!node)
	{
		arr.push(Infinity);
		return;
	}
	arr.push(node.val);
	inorder(node.left);
	inorder(node.right);
}
inorder(s);
var s1 = [...arr];
arr = [];
inorder(t);
var s2 = [...arr];
```

**Step2:** build KMP pattern suffix array
- ignore here, many previous code
```js
 var lps = buildLPS(s2);
```

**Step3:** find s2 array in s1 array
```js
var j = 0; // prefix
var i = 0; // suffix
var n = s1.length;
var m = s2.length;
while(i < n)
{
	if(s1[i] === s2[j])
	{
		i++;
		j++;
	}
	else
	{
		if(j>0)
		{
            // prefix goes back to prev max matched length
            j = lps[j-1];
        }
        else
        {
            // must re-start compare at beginning
            i++;
        }
    }
    if(j === m)
    {
         // find
         return true;
    }
}
return false; // not found
```

**Complete code solution:**
see this discussion here: [kmp-solution](https://leetcode.com/problems/subtree-of-another-tree/discuss/1054120/Javascript-inorder-traversal-+-KMP-advanced-solution)

