## Leetcode Contest 242 - JavaScript

#### I. [1869. Longer Contiguous Segments of Ones than Zerosr](#question-1)

#### II. [1870. Minimum Speed to Arrive on Time](#question-2)

#### III. [1871.  Jump Game VII](#question-3)

#### IV. [1872. Stone Game VIII](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1869. Longer Contiguous Segments of Ones than Zeros](https://leetcode.com/problems/longer-contiguous-segments-of-ones-than-zeros/)

Given a binary string  `s`, return  `true` _if the  **longest**  contiguous segment of_ `1`_s is  **strictly longer**  than the  **longest**  contiguous segment of_ `0`_s in_ `s`. Return  `false` _otherwise_.

-   For example, in  `s = "110100010"`  the longest contiguous segment of  `1`s has length  `2`, and the longest contiguous segment of  `0`s has length  `3`.

Note that if there are no  `0`s, then the longest contiguous segment of  `0`s is considered to have length  `0`. The same applies if there are no  `1`s.

**Example 1:**
**Input:** s = "1101"
**Output:** true
**Explanation:**
The longest contiguous segment of 1s has length 2: "1101"
The longest contiguous segment of 0s has length 1: "1101"
The segment of 1s is longer, so return true.

**Example 2:**
**Input:** s = "111000"
**Output:** false
**Explanation:**
The longest contiguous segment of 1s has length 3: "111000"
The longest contiguous segment of 0s has length 3: "111000"
The segment of 1s is not longer, so return false.

Also see my **discussion article** here: [JS solution](https://leetcode.com/problems/longer-contiguous-segments-of-ones-than-zeros/discuss/1224810/JavaScript-Simple-count-solution-O%28n%29)

**My JavaScript Solution:**
```js
var checkZeroOnes = function(s) {
    var cnt ={"1": 0, "0":0};
    for(var i = 0; i<s.length; i++)
    {
        var j = i+1;
        while(j<s.length && s[j]===s[i])
        {
            j++;
        }
        cnt[s[i]] = Math.max(cnt[s[i]], j-i);
        i = j-1;
    }
    return cnt["1"] > cnt["0"];
};
```

<div id="question-2"/>

### [1870. Minimum Speed to Arrive on Time](https://leetcode.com/problems/minimum-speed-to-arrive-on-time/)

You are given a floating-point number  `hour`, representing the amount of time you have to reach the office. To commute to the office, you must take  `n`  trains in sequential order. You are also given an integer array  `dist`  of length  `n`, where  `dist[i]`  describes the distance (in kilometers) of the  `ith`  train ride.

Each train can only depart at an integer hour, so you may need to wait in between each train ride.

-   For example, if the  `1st`  train ride takes  `1.5`  hours, you must wait for an additional  `0.5`  hours before you can depart on the  `2nd`  train ride at the 2 hour mark.

Return  _the  **minimum positive integer**  speed  **(in kilometers per hour)**  that all the trains must travel at for you to reach the office on time, or_ `-1` _if it is impossible to be on time_.

Tests are generated such that the answer will not exceed  `107`  and  `hour`  will have  **at most two digits after the decimal point**.

**Example 1:**
**Input:** dist = [1,3,2], hour = 6
**Output:** 1
**Explanation:** At speed 1:
- The first train ride takes 1/1 = 1 hour.
- Since we are already at an integer hour, we depart immediately at the 1 hour mark. The second train takes 3/1 = 3 hours.
- Since we are already at an integer hour, we depart immediately at the 4 hour mark. The third train takes 2/1 = 2 hours.
- You will arrive at exactly the 6 hour mark.

**JavaScript solution:**
Please see my **discussion article** here: [Javascript -  Binary Search solution](https://leetcode.com/problems/minimum-speed-to-arrive-on-time/discuss/1224772/JavaScript-Binary-Search-with-explanation)

```js
var minSpeedOnTime = function(dist, hour) {
    var n = dist.length;
    if(hour<=n-1)
    {
        return -1;
    }
    
    // find possible min speed
    var l = 1;
    
    // find possible max speed: distMax / 0.01 = 10^5 / 0.01 = 10^7
    var r = 10000000;
    
    // binary search the min speed that can arrive on time
    while(l<r)
    {
        var mid = Math.floor((l+r)/2);
        if(!canArrive(dist, mid, hour))
        {
           
            l = mid+1;
        }
        else
        {
            r = mid;
        }
    }
    return l;
};
function canArrive(dist, speed, hour)
{
        var n = dist.length;
        var t = 0;
        for(var i=0; i<n; i++)
        {
            if(i<n-1)
            {
                t += Math.ceil(dist[i] / speed);
            }
            else
            {
                var last  = dist[i] / speed;
                t += last;
            }
            if(t>hour)
            {
                return false;
            }
        }
        return true;
}
```

<div id="question-3"/>

### [1871.  Jump Game VII](https://leetcode.com/problems/jump-game-vii/)

You are given a  **0-indexed**  binary string  `s`  and two integers  `minJump`  and  `maxJump`. In the beginning, you are standing at index  `0`, which is equal to  `'0'`. You can move from index  `i`  to index  `j`  if the following conditions are fulfilled:

-   `i + minJump <= j <= min(i + maxJump, s.length - 1)`, and
-   `s[j] == '0'`.

Return  `true` _if you can reach index_ `s.length - 1` _in_ `s`_, or_ `false` _otherwise._

**Example 1:**
**Input:** s = "011010", minJump = 2, maxJump = 3
**Output:** true
**Explanation:**
In the first step, move from index 0 to index 3. 
In the second step, move from index 3 to index 5.

**Example 2:**
**Input:** s = "01101110", minJump = 2, maxJump = 3
**Output:** false
**JavaScript solution:**

See my discussion article here: [JavaScript-Easy-DP](https://leetcode.com/problems/jump-game-vii/discuss/1224707/JavaScript-Easy-DP)

Also Another better O(n) solution here:
https://leetcode.com/problems/jump-game-vii/discuss/1224804/JavaC%2B%2BPython-One-Pass-DP
https://leetcode.com/problems/jump-game-vii/discuss/1224936/DP-%2B-Sliding-Window-or-O(N)-or-Single-Pass


```js
var canReach = function(s, minJump, maxJump) {
    var n = s.length;
    s = [...s].reverse().join('');
    if(s[0] === '1') {
        return false;
    }
    var cur = 0;
    while(cur< n)
    {
        if(s[cur]==='1')
        {
            cur++;
            continue;
        }
        var min = cur + minJump;
        var max = cur + maxJump;
        if(min <= n-1 && max >= n-1)
        {
            return true;
        }
        
    }
    var dp = new Array(n).fill(false);
    dp[0] = true;
    var cur = 0;
    for(var i =0 ;i<n; i++)
    {
        if(s[i] === '1')
        {
            continue;
        }
        else
        {
            for(var j= i-1; j>=0; j--)
            {
                if(dp[j] &&  (i-j)>=minJump && (i-j)<=maxJump)
                {
                    dp[i] = true;
                    break;
                }
            }
        }
    }
    return dp[n-1];
};
```

<div id="question-4" />

### [1872. Stone Game VIII](https://leetcode.com/problems/stone-game-viii/)

**Referenced solution:**
https://leetcode.com/problems/stone-game-viii/discuss/1224640/C%2B%2B-DP-O(N)-time-O(1)-space

**My JS Solution:**
- Understand the game rules, starting at a simple result, take all the stones consider `dp[n-1] = Sum[0,...n-1]`
- Iterate each step, understand the merge a new stone, actually means the **next player**  pick at `[0,....j]` on original array, then it's the same `dp[j]` for next player
	
```js
var stoneGameVIII = function(stones) {
    // dp[i]: diff score wheb the first player takes (0,...i) to start the game
    // dp[i] = max(sum[0..i] - dp[j]); (j= i+1, .... n-1)
    // first player pick: [0,...,i]
    // remaining for next player: [preSum(0..i), i+1, i+2,...,n-1]
    // So, it's the same as next player starts picking at [0,...j], here j = i+1, i+2, ..., n-1.

    var n = stones.length;
    var dp = new Array(n).fill(0);
    for(var i = 1; i<n; i++)
    {
        stones[i] += stones[i-1];
    }
    dp[n-1] = stones[n-1];
    
    var curBest = dp[n-1]; // to store the best dp[j] value
    var res = -Infinity;
    for(var i = n-2; i>=0; i--)
    {
        res = Math.max(curBest, res);
        curBest = Math.max(stones[i]-curBest, curBest);
    }
    return res;
}
```

<div id="question-5"/>

### Summary

- 1571 / 12400
- keep moving!
- summary the DP solution for Stone games !!!!
