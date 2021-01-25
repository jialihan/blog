## Leetcode Contest 225- JavaScript

#### I.  [1736.  Latest Time by Replacing Hidden Digits](#question-1)

#### II. [1737.  Change Minimum Characters to Satisfy One of Three Conditions](#question-2)

#### III. [1738.  Find Kth Largest XOR Coordinate Value](#question-3)

#### IV. [1739. Building Boxe - not solved](#question-4)

#### V. [Summary](#summary)


<div id="question-1"/>

### [1736.  Latest Time by Replacing Hidden Digits](https://leetcode.com/problems/latest-time-by-replacing-hidden-digits/)

You are given a string  `time`  in the form of  `hh:mm`, where some of the digits in the string are hidden (represented by  `?`).

The valid times are those inclusively between  `00:00`  and  `23:59`.

Return  _the latest valid time you can get from_  `time` _by replacing the hidden_  _digits_.

**Example 1:**
**Input:** time = "2?:?0"
**Output:** "23:50"
**Explanation:** The latest hour beginning with the digit '2' is 23 and the latest minute ending with the digit '0' is 50.

**Example 2:**
**Input:** time = "0?:3?"
**Output:** "09:39"

Cleaned **JavaScript Solution:**
```js
/**
 * @param {string} time
 * @return {string}
 */
var maximumTime = function(time) {
    var arr = [...time];   
    if(arr[0] === '?')
    {
        arr[0] = arr[1] <= '3' || arr[1]==='?' ? '2' : '1';     
    }
    if(arr[1] === '?')
    {
        arr[1] = arr[0] === '2' ? '3' : '9';     
    }
    if(arr[3] === '?')
    {
        arr[3] = '5';  
    }
    if(arr[4] === '?')
    {
        arr[4] = '9';
    }
    return arr.join('');   
};
```

<div id="question-2"/>

### [1737.  Change Minimum Characters to Satisfy One of Three Conditions](https://leetcode.com/problems/change-minimum-characters-to-satisfy-one-of-three-conditions/)

You are given two strings  `a`  and  `b`  that consist of lowercase letters. In one operation, you can change any character in  `a`  or  `b`  to  **any lowercase letter**.

Your goal is to satisfy  **one**  of the following three conditions:

-   **Every**  letter in  `a`  is  **strictly less**  than  **every**  letter in  `b`  in the alphabet.
-   **Every**  letter in  `b`  is  **strictly less**  than  **every**  letter in  `a`  in the alphabet.
-   **Both**  `a`  and  `b`  consist of  **only one**  distinct letter.

Return  _the  **minimum**  number of operations needed to achieve your goal._

**Example 1:**
**Input:** a = "aba", b = "caa"
**Output:** 2
**Explanation:** Consider the best way to make each condition true:
1) Change b to "ccc" in 2 operations, then every letter in a is less than every letter in b.
2) Change a to "bbb" and b to "aaa" in 3 operations, then every letter in b is less than every letter in a.
3) Change a to "aaa" and b to "aaa" in 2 operations, then a and b consist of one distinct letter.
The best way was done in 2 operations (either condition 1 or condition 3).

**Analysis:**
- Wrong way: hard to think, trap into the way to find some smart and some hidden rules and pattern to handle this string compare. waste a long time.
- Correct way:  just Burte-Force solution
	traverse **26 character** to check/verify each solution, and find the optimal minimum result.
- JavaScript String cannot directly get the ascii code from `0 - 25`,  use the method to get number value: [charCodeAt()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt)
For example:  
	```
	'a'-'b'
	// NaN
	```

	![image](../assets/jscharoperationerror.png ':size=189x83')
- **Edge case:**  'a' or 'z', because we cannot decrease 'a' any more, or we cannot increase 'z' anymore.

JavaScript Solution:
```js
/**
 * @param {string} a
 * @param {string} b
 * @return {number}
 */
var minCharacters = function(a, b) {
    var n1 = a.length;
    var n2 = b.length;
    var res = n1+n2;
    var cnt1 = [...Array(26)].fill(0);
    var cnt2 = [...Array(26)].fill(0);
    // pre-processing the counting in two strings
     for (var aa of [...a]) {
        cnt1[aa.charCodeAt(0) - 'a'.charCodeAt(0)]++;
     }
     for (var bb of [...b]) {
        cnt2[bb.charCodeAt(0) - 'a'.charCodeAt(0)]++;
         cnt2[bb-'a']++;
    }
          
    for (var i=0; i<26; i++) {
        if(i>0) // only starts from 'b', because we cannot decrease 'a'
        {
            // case1: a is strictly less than b
            // a: the letters < "i"
            // b: the letters >= "i"
            var r = 0;
            for(var j = 0; j<i; j++)
            {
                r += cnt1[j];
            }
            for(var j = i; j<26; j++)
            {
                r += cnt2[j];
            }
            res = Math.min(res, r);
            // case2: b is strictly less than a
            // a: the letters >= "i"
            // b: the letters < "i"
            r = 0;
            for(var j = i; j<26; j++)
            {
                r += cnt1[j];
            }
            for(var j = 0; j<i; j++)
            {
                r += cnt2[j];
            }
            res = Math.min(res, r);
        }
        // case 3: all unique letter equal to "i"
        r = 0;
        for(var j = 0; j<26; j++)
        {
           if(j !== i)
           {
               r += cnt1[j];
               r += cnt2[j];
           }
        }
         res = Math.min(res, r);
    }
    return res;
};
```

<div id="question-3"/>

### [1738.  Find Kth Largest XOR Coordinate Value](https://leetcode.com/problems/find-kth-largest-xor-coordinate-value/)

You are given a 2D  `matrix`  of size  `m x n`, consisting of non-negative integers. You are also given an integer  `k`.

The  **value**  of coordinate  `(a, b)`  of the matrix is the XOR of all  `matrix[i][j]`  where  `0 <= i <= a < m`  and  `0 <= j <= b < n`  **(0-indexed)**.

Find the  `kth`  largest value  **(1-indexed)**  of all the coordinates of  `matrix`.

**Example 1:**
**Input:** matrix = [[5,2],[1,6]], k = 1
**Output:** 7
**Explanation:** The value of coordinate (0,1) is 5 XOR 2 = 7, which is the largest value.

Analysis:
- how to store the accumulated sub-matrix XOR result? - `dp[i][j]`
- different in JAVA & JS 
	```java
	// java
	dp[i][j] = dp[i-1][j] ^ dp[i][j-1] ^ dp[i-1][j-1] ^ matrix[i][j];
	// javascript: no duplicated
	dp[i][j] = [0...j-1]_prev_colums ^ dp[i-1][j] ^ matrix[i][j]
	```
	
	![image](../assets/lc1738.png ':size=295x180')

- Java Solution vs JS solution: see discussion here:  [leetcode-discuss-link](https://leetcode.com/problems/find-kth-largest-xor-coordinate-value/discuss/1032283/JavaScript-vs-Java-solution-DP)

**JavaScript Solution:**
```js
var kthLargestValue = function(matrix, k) {
    var m = matrix.length;
    var n = matrix[0].length;
    var dp = [...Array(m)].fill([...Array(n)].fill(0));
    var res = [];
    for(var i = 0; i < m; i++) 
    {
        var temp = 0;
        for(var j = 0; j < n; j++){
            temp ^= matrix[i][j];
            if (i > 0) {
                dp[i][j] = temp ^ dp[i-1][j];
            }
            else
            {
                     dp[i][j] = temp;
            }
            res.push(dp[i][j]);
        }
    }
    res.sort((a,b)=>a-b);
    return res[n*m-k];
};
```

<div id="question-4"/>

### [1739. Building Boxes](https://leetcode.com/problems/building-boxes/)

**Not Solved !**

### Summary
- javascript char ascii code cannot directly get by `+` or `-` operators, remember, use `charCodeAt()` method
	```
	'a' - 'b'
	// NaN
	```
- JS: pass by value, pass by reference, better to use a new primitive value to calculate during the **matrix accumulated computing.** might cause some messed up values.
- think about : Brute-force solution when **26 character problems** !!!
- keep moving !!! 