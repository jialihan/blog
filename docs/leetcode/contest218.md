## After Leetcode Contest 218 - JavaScript

I.  [1678. Goal Parser Interpretation](#question-1)

II. [1679. Max Number of K-Sum Pairs](#question-2)

III. [1680. Concatenation of Consecutive Binary Numbers](#question-3)

IV. [1681. Minimum Incompatibility - not finish](#question-4)

V. [Summary](#summary)


<div id="question-1"/>

### [1678. Goal Parser Interpretation](https://leetcode.com/problems/goal-parser-interpretation/)

You own a  **Goal Parser**  that can interpret a string  `command`. The  `command`  consists of an alphabet of  `"G"`,  `"()"`  and/or  `"(al)"`  in some order. The Goal Parser will interpret  `"G"`  as the string  `"G"`,  `"()"`  as the string  `"o"`, and  `"(al)"`  as the string  `"al"`. The interpreted strings are then concatenated in the original order.

Given the string  `command`, return  _the  **Goal Parser**'s interpretation of_ `command`.

**Example 1:**
**Input:** command = "G()(al)"
**Output:** "Goal"
**Explanation:** The Goal Parser interprets the command as follows:
G -> G
() -> o
(al) -> al
The final concatenated result is "Goal".

**Tips:**
- String: [replace()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)
- Regex
- add `\` to skip the special characters "(" or ")"


**My JavaScript Solution:**
```
/**
 * @param {string} command
 * @return {string}
 */
var interpret = function(command) {
   return command.replace(/\(\)/g, "o").replace(/\(al\)/g, "al");
};
```

<div id="question-2"/>

### [1679. Max Number of K-Sum Pairs](https://leetcode.com/problems/max-number-of-k-sum-pairs/)

You are given an integer array  `nums`  and an integer  `k`.
In one operation, you can pick two numbers from the array whose sum equals  `k`  and remove them from the array.
Return  _the maximum number of operations you can perform on the array_.

**Example 1:**
**Input:** nums = [1,2,3,4], k = 5
**Output:** 2
**Explanation:** Starting with nums = [1,2,3,4]:
- Remove numbers 1 and 4, then nums = [2,3]
- Remove numbers 2 and 3, then nums = []
There are no more pairs that sum up to 5, hence a total of 2 operations.

**My JavaScript  Solution:** two pointer

```
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var maxOperations = function(nums, k) {
    nums.sort((a,b)=>a-b);
    var l = 0;
    var r = nums.length-1;
    var cnt = 0;
    while(l<r)
    {
         var sum = nums[l] + nums[r];
         if( sum === k)
                {
                    l++;
                    r--;
                    cnt++;
                }
            else if(sum < k)
                {
                    l++;
                }
            else
                {
                    r--;
                }
        }
    return cnt;
};
```

<div id="question-3"/>

### [1680. Concatenation of Consecutive Binary Numbers](https://leetcode.com/problems/concatenation-of-consecutive-binary-numbers/ß)

Given an integer  `n`, return  _the  **decimal value**  of the binary string formed by concatenating the binary representations of_ `1` _to_ `n` _in order,  **modulo**_ `109 + 7`.

**Example 1:**
**Input:** n = 1
**Output:** 1
**Explanation:** "1" in binary corresponds to the decimal value 1. 

**Example 2:**
**Input:** n = 3
**Output:** 27
**Explanation:** In binary, 1, 2, and 3 corresponds to "1", "10", and "11".
After concatenating them, we have "11011", which corresponds to the decimal value 27.

**Tips:**
- when to increase the length of bits: `1 to 10`
- javascript operator (ES7): `**`, [Exponentiation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Exponentiation)

**My JavaScript Solution：**
```
/**
 * @param {number} n
 * @return {number}
 */
var concatenatedBinary = function(n) {
    // (i-1) << shift_bits + i
    var mod = 1e9+7;
    var res = 0;
    for(var i=1, shift=0; i<=n; i++)
    {
        var singleBit = i&(i-1);
        if(singleBit == 0)
        {
             // different bit length
             shift++;   
        }
        res *= (2 ** shift); 
        res += i;
        res %= mod;
    }
    return res;
    
};
```
<div id="question-4"/>

### [1681. Minimum Incompatibility](https://leetcode.com/problems/minimum-incompatibility/)

Not finished


<div id="summary"/>

### Summary
- make up for this test 
- javascript math operators
- regex
- binary shift operation