## Leetcode Contest 216 - JavaScript

I. [1662. Check If Two String Arrays are Equivalent](#question-1)

II. [1663. Smallest String With A Given Numeric Value](#question-2)

III. [1664. Ways to Make a Fair Array](#question-3)

IV. [1665. Minimum Initial Energy to Finish Tasks - not solved](#question-4)

V. [Summary](#summary)

<div id="question-1"/>

### [1662. Check If Two String Arrays are Equivalent](https://leetcode.com/problems/check-if-two-string-arrays-are-equivalent/)

[JavaScript-source-code](https://github.com/jialihan/LeetCode-JavaScript/blob/main/contest/contest216/lc1662.js)

<div id="question-2"/>

### [1663. Smallest String With A Given Numeric Value](https://leetcode.com/problems/smallest-string-with-a-given-numeric-value/)

Challenges: 
* Greedy
* javascript character get charCode: [fromCharCode()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode)

[JavaScript-source-code](https://github.com/jialihan/LeetCode-JavaScript/blob/main/contest/contest216/lc1663.js)


<div id="question-3"/>

### [1664. Ways to Make a Fair Array](https://leetcode.com/problems/ways-to-make-a-fair-array/)

Challenges:
1. Interesting thing: TIME out when I do O(3*N)
- scan to calculate preSum
- scan to calculate last-remaining Sum // this can be optimized!
- scan on current index

2. Optimized 2 * N time complexity
- scan preSum
- scan current: last = total - preSum

[JavaScript-LeetCode Discussion](https://leetcode.com/problems/ways-to-make-a-fair-array/discuss/944628/JavaScript-O(2*n)-preSum)

<div id="question-4"/>

### [1665. Minimum Initial Energy to Finish Tasks - not solved at contest](https://leetcode.com/problems/minimum-initial-energy-to-finish-tasks/)

Tips:
- sort by largest earned energy


<div id="summary"/>

### Summary

- rank: 3234 / 9573
- Question 3: **O(3N)** timeout, then think too much, there is no more quick way than O(n), so finally try **O(2N)** solved! waste a lot of time.


