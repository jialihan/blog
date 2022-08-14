## Leetcode Contest 306 - JavaScript

#### I. [2373. Largest Local Values in a Matrix](#question-1)

#### II. [2374. Node With Highest Edge Score](#question-2)

#### III. [2375. Construct Smallest Number From DI String](#question-3)

#### IV. [2376. Count Special Integers- not solved](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [2373. Largest Local Values in a Matrix](https://leetcode.com/problems/largest-local-values-in-a-matrix/)

1. My brute force solution **O(n^3)**
2. Optimized hashSet solution **O(n)**

Solution:
https://leetcode.com/problems/largest-local-values-in-a-matrix/discuss/2422529/javascript-brute-force-with-8-directions

<div  id="question-2"/>

### [2374. Node With Highest Edge Score](https://leetcode.com/contest/weekly-contest-306/problems/node-with-highest-edge-score/)

Count in array with index:

Solution:
https://leetcode.com/problems/node-with-highest-edge-score/discuss/2422539/Javascript-Array-and-Counting-O(n)

<div  id="question-3"/>

### [2375. Construct Smallest Number From DI String](https://leetcode.com/problems/construct-smallest-number-from-di-string/)

Array & Reverse

https://leetcode.com/problems/construct-smallest-number-from-di-string/discuss/2422585/JavaScript-brute-force-swap-at-%22D%22

<div  id="question-4"  />

### [2376. Count Special Integers](https://leetcode.com/problems/count-special-integers/)

1. bit Mask
2. dfs from digit 0 to digit (n-1)

```
var countSpecialNumbers = function(n) {
    // total 9 digits
    // 11111 11111: empty
    // 01111 11111: 9 is taken
    // 11111 11110: 0 is taken
    let ans = 0;
    let bitMask = 1023; // '11111 11111'
    function dfs(num, mask) {
        if(num<=n) {
            ans++;
        }
        if(num>n || mask === 0) {
            return;
        }
        for(let i = num === 0 ? 1 : 0; i<=9; i++)
        {
            if(mask & (1 << i)) // empty
            {
                dfs(num*10 + i, mask ^ (1<<i));
            }
        }
    }
    dfs(0, bitMask);
    return ans-1; // minus initial dfs run with num === 0
};
```

Reference Solution:
https://leetcode.com/problems/count-special-integers/discuss/2422519/Simple-Backtracking%2BBitmask

<div  id="question-5"/>

### Summary

- 3/4 completed
- 2689 / 24853
