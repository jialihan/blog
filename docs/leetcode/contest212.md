## Leetcode Contest 212 - JavaScript

I. [1629. Slowest Key](#question-1)

II. [1630. Arithmetic Subarrays](#question-2)

III. [1631. Path With Minimum Effort - not solved](#question-3)

IV. [1632. Rank Transform of a Matrix - not solved](#question-4)

V. [Summary](#summary)

<div id="question-1"/>

### 1629. Slowest Key

A newly designed keypad was tested, where a tester pressed a sequence of `n` keys, one at a time.

You are given a string `keysPressed` of length `n`, where `keysPressed[i]` was the `ith` key pressed in the testing sequence, and a sorted list `releaseTimes`, where `releaseTimes[i]` was the time the `ith` key was released. Both arrays are **0-indexed**. The `0th` key was pressed at the time `0`, and every subsequent key was pressed at the **exact** time the previous key was released.

The tester wants to know the key of the keypress that had the **longest duration**. The `ith` keypress had a **duration** of `releaseTimes[i] - releaseTimes[i - 1]`, and the `0th` keypress had a duration of `releaseTimes[0]`.

Note that the same key could have been pressed multiple times during the test, and these multiple presses of the same key **may not** have had the same **duration**.

_Return the key of the keypress that had the **longest duration**. If there are multiple such keypresses, return the lexicographically largest key of the keypresses._

**Example 1:**

**Input:** releaseTimes = [9,29,49,50], keysPressed = "cbcd"
**Output:** "c"
**Explanation:** The keypresses were as follows:
Keypress for 'c' had a duration of 9 (pressed at time 0 and released at time 9).
Keypress for 'b' had a duration of 29 - 9 = 20 (pressed at time 9 right after the release of the previous character and released at time 29).
Keypress for 'c' had a duration of 49 - 29 = 20 (pressed at time 29 right after the release of the previous character and released at time 49).
Keypress for 'd' had a duration of 50 - 49 = 1 (pressed at time 49 right after the release of the previous character and released at time 50).
The longest of these was the keypress for 'b' and the second keypress for 'c', both with duration 20.
'c' is lexicographically larger than 'b', so the answer is 'c'.

**My JavaScript Solution:**

```
/**
 * @param {number[]} releaseTimes
 * @param {string} keysPressed
 * @return {character}
 */
var slowestKey = function(releaseTimes, keysPressed) {
    var n = releaseTimes.length;
    var pre = 0;
    var max = 0;
    var res = '';
    for(var i = 0; i< n; i++)
        {
            var diff = releaseTimes[i] - pre;
            if(diff > max)
                {
                    max = diff;
                    res  = keysPressed[i];
                }
            else if(diff === max)
                {
                    if(keysPressed[i] > res)
                        {
                            res = keysPressed[i];
                        }
                }
            pre = releaseTimes[i];
        }
    return res;
};
```

<div id="question-2"/>

### 1630. Arithmetic Subarrays

A sequence of numbers is called **arithmetic** if it consists of at least two elements, and the difference between every two consecutive elements is the same. More formally, a sequence `s` is arithmetic if and only if `s[i+1] - s[i] == s[1] - s[0]` for all valid `i`.

For example, these are **arithmetic** sequences:

1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9

The following sequence is not **arithmetic**:

1, 1, 2, 5, 7

You are given an array of `n` integers, `nums`, and two arrays of `m` integers each, `l` and `r`, representing the `m` range queries, where the `ith` query is the range `[l[i], r[i]]`. All the arrays are **0-indexed**.

Return _a list of_ `boolean` _elements_ `answer`_, where_ `answer[i]` _is_ `true` _if the subarray_ `nums[l[i]], nums[l[i]+1], ... , nums[r[i]]` _can be **rearranged** to form an **arithmetic** sequence, and_ `false` _otherwise._

**Example 1:**
**Input:** nums = `[4,6,5,9,3,7]`, l = `[0,0,2]`, r = `[2,3,5]`
**Output:** `[true,false,true]`
**Explanation:**
In the 0th query, the subarray is [4,6,5]. This can be rearranged as [6,5,4], which is an arithmetic sequence.
In the 1st query, the subarray is [4,6,5,9]. This cannot be rearranged as an arithmetic sequence.
In the 2nd query, the subarray is `[5,9,3,7]. This` can be rearranged as `[3,5,7,9]`, which is an arithmetic sequence.

**My JavaScript Solution:**
attention: be careful of [array.sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) :
The `**sort()**` method sorts the elements of an array _[in place](https://en.wikipedia.org/wiki/In-place_algorithm)_ and returns the sorted array. The default sort order is ascending, built upon converting the elements into strings, then comparing their sequences of UTF-16 code units values.

So here we need to implement to compare numbers in ascending order.

```
var checkArithmeticSubarrays = function(nums, l, r) {
    function isValid(arr){
        var n = arr.length;
        if(n <= 2)
            return true;
        // sort by asc order
        arr.sort(function(a,b){return a-b});
        var base = arr[1] - arr[0];
        for(var i = 2; i<arr.length; i++)
        {
              if(arr[i] - arr[i-1] !== base)
                  {
                      return false;
                  }
        }
        return true;
    }
    var res = [];
    var len = l.length;
    for(var i = 0; i<len; i++)
        {
            var a = [];
            for(var j = l[i]; j<= r[i]; j++)
                {
                    a.push(nums[j]);
                }
            res.push(isValid(a));
        }
    return res;

};
```

<div id="question-3"/>

### 1631. Path With Minimum Effort

You are a hiker preparing for an upcoming hike. You are given `heights`, a 2D array of size `rows x columns`, where `heights[row][col]` represents the height of cell `(row, col)`. You are situated in the top-left cell, `(0, 0)`, and you hope to travel to the bottom-right cell, `(rows-1, columns-1)` (i.e., **0-indexed**). You can move **up**, **down**, **left**, or **right**, and you wish to find a route that requires the minimum **effort**.

A route's **effort** is the **maximum absolute difference** in heights between two consecutive cells of the route.

Return _the minimum **effort** required to travel from the top-left cell to the bottom-right cell._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/04/ex1.png)
**Input:** heights = [[1,2,2],[3,8,2],[5,3,5]]
**Output:** 2
**Explanation:** The route of [1,3,5,3,5] has a maximum absolute difference of 2 in consecutive cells.
This is better than the route of [1,2,2,2,5], where the maximum absolute difference is 3.

**Wrong Solution**

- DFS solution: time limit exceeded
- DFS + MEMO: try to add a memoriazation, but result messed up.

my initial direct DFS time limit exceeded solution:

```
var minimumEffortPath = function(heights) {
    var dir = [[0,1], [0,-1], [1,0], [-1,0]];
    var m = heights.length;
    var n = heights[0].length;
    var visited = new Set();
    var memo = {};
    var minRes = 1000000;
    function dfs(arr, visited, x, y, cur)
    {
        var code = x+""+y;
        if(visited.has(code))
        {
              return -1;
        }
        if(x === m-1 && y === n-1)
        {
            minRes = Math.min(minRes, cur);
            return cur;
        }
        visited.add(code);
        for(var d of dir)
            {
                var xx = x + d[0];
                var yy = y + d[1];
                if(xx<0 || xx>=m ||yy<0 || yy>=n)
                {
                    continue;
                }
                var diff = Math.abs(arr[x][y] - arr[xx][yy]);
                var max = Math.max(diff, cur);
                 // console.log("cur max= ", max)
                dfs(arr, visited, xx, yy, max);

            }
        visited.delete(code);
    }
    dfs(heights, visited, 0, 0, 0);
    return minRes;
};
```

**Correct solution: DFS + BinarySearch**
Tip:

- when time limit exceeded, need to consider a faster way of search, when memoriazation not working, think about BFS
- `Math.floor()` value in Javascript divide operation, because it will return a **double value**. Then we compute the middle value as `mid = Math.floor((l + r) / 2)`.

```
var minimumEffortPath = function (heights) {
  var dir = [
    [0, 1],
    [0, -1],
    [1, 0],
    [-1, 0]
  ];
  var m = heights.length;
  var n = heights[0].length;
  var visited = new Set();
  function dfs(arr, x, y, mid) {
    var code = x + "," + y;
    if (visited.has(code)) {
      return false;
    }
    if (x === m - 1 && y === n - 1) {
      return true;
    }
    visited.add(code);
    for (var d of dir) {
      var xx = x + d[0];
      var yy = y + d[1];
      if (xx < 0 || xx >= m || yy < 0 || yy >= n) {
        continue;
      }
      var diff = Math.abs(arr[x][y] - arr[xx][yy]);
      if (diff > mid) continue;

      if (dfs(arr, xx, yy, mid)) {
        return true;
      }
    }
    return false;
  }
  var l = 0;
  var r = 1000000;
  while (l < r) {
    var mid = Math.floor((l + r) / 2);
    visited = new Set();
    if (dfs(heights, 0, 0, mid)) {
      // can finish dfs
      r = mid;
    } else {
      l = mid + 1;
    }
  }
  return l;
};
```

<div id="question-4"/>

### [1632. Rank Transform of a Matrix](https://leetcode.com/contest/weekly-contest-212/problems/rank-transform-of-a-matrix/)

Not Solved!

### Summary

- DFS timeout the first thing come to my mind is adding a memorization to improve speed, but it didn't work! Think about **Binary Search** next time !
- array sort method should add implementation! be careful next time
- javascript divide will return **double value**, be careful
- rank: 2483 / 10984
- keep moving next week!
