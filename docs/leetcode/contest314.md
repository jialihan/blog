## Leetcode Contest 314 - JavaScript

#### I. [2432. The Employee That Worked on the Longest Task](#question-1)

#### II. [2433. Find The Original Array of Prefix Xor](#question-2)

#### III. [2434. Using a Robot to Print the Lexicographically Smallest String - not solved](#question-3)

#### IV. [2435. Paths in Matrix Whose Sum Is Divisible by K - not solved](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [2432. The Employee That Worked on the Longest Task](https://leetcode.com/problems/the-employee-that-worked-on-the-longest-task/)

Solution:
https://leetcode.com/problems/the-employee-that-worked-on-the-longest-task/discuss/2688709/JavaScript-linear-scan-the-time-interval

<div  id="question-2"/>

### [2433. Find The Original Array of Prefix Xor](https://leetcode.com/problems/find-the-original-array-of-prefix-xor/)

Solution: [link](https://leetcode.com/problems/find-the-original-array-of-prefix-xor/discuss/2688722/JavaScript-simple-XOR)

Rule: `a ^ b = c` then `b = a ^ c`

```
var findArray = function(pref) {
    const res = [pref[0]];
    for(let i = 1; i < pref.length; i++) {
        res.push (pref[i-1] ^ pref[i]);
    }
    return res;
};
```

<div  id="question-3"/>

### [2434. Using a Robot to Print the Lexicographically Smallest String - not solved](https://leetcode.com/problems/using-a-robot-to-print-the-lexicographically-smallest-string/)

```
var robotWithString = function(s) {
    const cnt = new Array(26).fill(0);
    for(let i = 0; i<s.length; i++) {
        cnt[s[i].charCodeAt(0) - 97]++;
    }
    let smallest = 0; // index of remaining char in str's suffix
    const t = []; // stack
    let res='';
    for(const c of [...s]) {
        t.push(c);
        cnt[c.charCodeAt(0)-97]--;
        // update smallest in counter array
        while(cnt[smallest] === 0) {
            smallest++;
        }
        while(t.length > 0 && (t[t.length-1].charCodeAt(0) - 97) <= smallest) {
            res += t.pop();
        }
    }
    return res;
};
```

<div  id="question-4"  />

### [2435. Paths in Matrix Whose Sum Is Divisible by K - not solved](https://leetcode.com/problems/using-a-robot-to-print-the-lexicographically-smallest-string/)

**Solution: DFS / DP** [link](https://leetcode.com/problems/paths-in-matrix-whose-sum-is-divisible-by-k/discuss/2688651/JavaScript-recursion-+-memo)

```
var numberOfPaths = function(grid, k) {
    const m = grid.length;
    const n = grid[0].length;
    // max sum in one path: Math(the first element value,   sum % k (k)) = Math.max(100, 50);
    const dp = new Array(m).fill(0).map(el=>new Array(n).fill(0).map(e=>new Array(101).fill(-1)));

    const mod = 1000000007;
    function dfs(x,y,sum) {
        if(x<0 || y<0 || x>=m || y>=n) {
            return 0;
        }
        if(x=== m-1 && y === n-1) {
            return (sum + grid[m-1][n-1] ) % k === 0 ? 1 : 0;
        }
        if(dp[x][y][sum] !== -1) {
            return  dp[x][y][sum] % mod;
        }
        const res1 = dfs(x+1, y, (sum+grid[x][y]) % k ) % mod;
        const res2 = dfs(x, y+1, (sum+grid[x][y]) % k ) % mod;
        dp[x][y][sum] = (res1 + res2) % mod;
        return dp[x][y][sum];
    }
    return dfs(0,0,0) % mod;
};
```

<div  id="question-5"/>

### Summary

- 2/4 completed
- 4750 / 22620
