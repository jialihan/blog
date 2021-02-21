## Leetcode Contest 229 - JavaScript

#### I.  [1768.  Merge Strings Alternately](#question-1)

#### II. [1769.  Minimum Number of Operations to Move All Balls to Each Box](#question-2)

#### III. [1770.  Maximum Score from Performing Multiplication Operations](#question-3)

#### IV. [1771.  Maximize Palindrome Length From Subsequences](#question-4)

#### V. [Summary](#summary)


<div id="question-1"/>

### [1768.  Merge Strings Alternately](https://leetcode.com/problems/merge-strings-alternately/)

You are given two strings  `word1`  and  `word2`. Merge the strings by adding letters in alternating order, starting with  `word1`. If a string is longer than the other, append the additional letters onto the end of the merged string.

Return  _the merged string._

**Example 1:**
**Input:** word1 = "abc", word2 = "pqr"
**Output:** "apbqcr"
**Explanation:** The merged string will be merged as so:
word1:  a   b   c
word2:    p   q   r
merged: a p b q c r

**JavaScript Solution: (brute-force)**
```js
var mergeAlternately = function(word1, word2) {
    var i = 0;
    var j = 0;
    var res = [];
    while(i<word1.length && j < word2.length)
    {
        res.push(word1[i]);
        res.push(word2[j]);
        i++;
        j++;
    }
    if(i<word1.length)
    {
        res.push(word1.slice(i, word1.length));   
    }
    if(j<word2.length)
    {
        res.push(word2.slice(j, word2.length));   
    }
    return res.join('');
};
```

<div id="question-2"/>

### [1769.  Minimum Number of Operations to Move All Balls to Each Box](https://leetcode.com/problems/minimum-number-of-operations-to-move-all-balls-to-each-box/)

You have  `n`  boxes. You are given a binary string  `boxes`  of length  `n`, where  `boxes[i]`  is  `'0'`  if the  `ith`  box is  **empty**, and  `'1'`  if it contains  **one**  ball.

In one operation, you can move  **one**  ball from a box to an adjacent box. Box  `i`  is adjacent to box  `j`  if  `abs(i - j) == 1`. Note that after doing so, there may be more than one ball in some boxes.

Return an array  `answer`  of size  `n`, where  `answer[i]`  is the  **minimum**  number of operations needed to move all the balls to the  `ith`  box.

Each  `answer[i]`  is calculated considering the  **initial**  state of the boxes.

**Example 1:**
**Input:** boxes = "110"
**Output:** [1,1,3]
**Explanation:** The answer for each box is as follows:
1) First box: you will have to move one ball from the second box to the first box in one operation.
2) Second box: you will have to move one ball from the first box to the second box in one operation.
3) Third box: you will have to move one ball from the first box to the third box in two operations, and move one ball from the second box to the third box in one operation.

**Example 2:**
**Input:** boxes = "001011"
**Output:** [11,8,5,4,3,4]

<div id="q2-1" />

#### 2.1 Brute Force solution - O(n^2)
```js
var minOperations = function(boxes) {
    var n = boxes.length;
    var res = new Array(n).fill(0);
    for(var i = 0; i<n;i++)
    {
        if(boxes[i] === '1')
        {
            for(var j = 0; j<n;j++)
            {
                if(i !== j)
                {
                    res[j] += Math.abs(j-i);
                }
            }
        }
    }
    return res;  
};
```

<div id="q2-2" />

#### 2.2  Optimized Solution: PreSum - O(n)

Scan from left to right, then from right to left, calculate preSum, then reduce time to O(n).

```js
var minOperations = function(boxes) {
    var n = boxes.length;
    var res = new Array(n).fill(0);
    var sum = 0;
    var preBalls = 0;
    for(var i = 0; i<n; i++)
    {
        res[i] += sum;
        if(boxes[i] === '1')
        {
            preBalls++;
        }
        sum += preBalls;
    }
    sum = 0;
    preBalls = 0;
    for(var i = n-1; i>=0; i--)
    {
        res[i] += sum;
        if(boxes[i] === '1')
        {
            preBalls++;
        }
        sum += preBalls;
    } 
    return res;  
};
```

<div id="question-3"/>

### [1770.  Maximum Score from Performing Multiplication Operations](https://leetcode.com/problems/maximum-score-from-performing-multiplication-operations/)

You are given two integer arrays  `nums`  and  `multipliers`  of size  `n`  and  `m`  respectively, where  `n >= m`. The arrays are  **1-indexed**.

You begin with a score of  `0`. You want to perform  **exactly**  `m`  operations. On the  `ith`  operation  **(1-indexed)**, you will:

-   Choose one integer  `x`  from  **either the start or the end** of the array  `nums`.
-   Add  `multipliers[i] * x`  to your score.
-   Remove  `x`  from the array  `nums`.

Return  _the  **maximum**  score after performing_ `m`  _operations._

**Example 1:**
**Input:** nums = [1,2,3], multipliers = [3,2,1]
**Output:** 14
**Explanation:** An optimal solution is as follows:
- Choose from the end, [1,2,**3**], adding 3 * 3 = 9 to the score.
- Choose from the end, [1,**2**], adding 2 * 2 = 4 to the score.
- Choose from the end, [**1**], adding 1 * 1 = 1 to the score.
The total score is 9 + 4 + 1 = 14.

**JavaScript solution:**

see the discussion article here:  [DFS+Memo](https://leetcode.com/problems/maximum-score-from-performing-multiplication-operations/discuss/1075674/JavaScript:-DFS-+-Memo)

<div id="question-4" />

### [1771.  Maximize Palindrome Length From Subsequences](https://leetcode.com/problems/maximize-palindrome-length-from-subsequences/)

You are given two strings,  `word1`  and  `word2`. You want to construct a string in the following manner:

-   Choose some  **non-empty**  subsequence  `subsequence1`  from  `word1`.
-   Choose some  **non-empty**  subsequence  `subsequence2`  from  `word2`.
-   Concatenate the subsequences:  `subsequence1 + subsequence2`, to make the string.

Return  _the  **length**  of the longest  **palindrome**  that can be constructed in the described manner._ If no palindromes can be constructed, return  `0`.

A  **subsequence**  of a string  `s`  is a string that can be made by deleting some (possibly none) characters from  `s`  without changing the order of the remaining characters.

A  **palindrome**  is a string that reads the same forward as well as backward.

**Example 1:**
**Input:** word1 = "cacb", word2 = "cbba"
**Output:** 5
**Explanation:** Choose "ab" from word1 and "cba" from word2 to make "abcba", which is a palindrome.

**Example 2:**
**Input:** word1 = "ab", word2 = "ab"
**Output:** 3
**Explanation:** Choose "ab" from word1 and "a" from word2 to make "aba", which is a palindrome.

**JavaScript Solution:**

see discussion article here: [DP-O((m+n)^2)](https://leetcode.com/problems/maximize-palindrome-length-from-subsequences/discuss/1075597/JavaScript:-concat-string-+-DP-O%28%28m+n%292%29)

<div id="summary"/>

### Summary
- Q3 - not solved, give up
- Q4: very close to the solution, `dp[i][j]` built, but how to find the longest from word1&word2? get stuck here, need to loop all possible `i: [0, m-1]` and `j: [m, m+n-1]`.
- 1709 / 11178, keep moving!
