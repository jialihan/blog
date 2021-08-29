## Leetcode Contest 256 - JavaScript

#### I. [1984. Minimum Difference Between Highest and Lowest of K Scores](#question-1)

#### II. [1985. Find the Kth Largest Integer in the Array](#question-2)

#### III. [1986. Minimum Number of Work Sessions to Finish the Tasks](#question-3)

- [LC 698. Partition to K Equal Sum Subsets](#q3-1)

#### IV. [1987. Number of Unique Good Subsequences - not solved](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1984. Minimum Difference Between Highest and Lowest of K Scores](https://leetcode.com/problems/minimum-difference-between-highest-and-lowest-of-k-scores/)

You are given a **0-indexed** integer array `nums`, where `nums[i]` represents the score of the `ith` student. You are also given an integer `k`.

Pick the scores of any `k` students from the array so that the **difference** between the **highest** and the **lowest** of the `k` scores is **minimized**.

Return _the **minimum** possible difference_.

**My JavaScript Solution:**

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var minimumDifference = function (nums, k) {
  nums.sort((a, b) => a - b);
  if (k === 1) {
    return 0;
  }
  var n = nums.length;
  var res = Infinity;
  for (var i = 0; i <= n - k; i++) {
    res = Math.min(res, nums[i + k - 1] - nums[i]);
  }
  return res;
};
```

<div id="question-2"/>

### [1985. Find the Kth Largest Integer in the Array](https://leetcode.com/problems/find-the-kth-largest-integer-in-the-array/)

You are given an array of strings `nums` and an integer `k`. Each string in `nums` represents an integer without leading zeros.

Return _the string that represents the_ `kth` _**largest integer** in_ `nums`.

**Note**: Duplicate numbers should be counted distinctly. For example, if `nums` is `["1","2","2"]`, `"2"` is the first largest integer, `"2"` is the second-largest integer, and `"1"` is the third-largest integer.

**Constraints:**

- `1 <= k <= nums.length <= 104`
- `1 <= nums[i].length <= 100`
- `nums[i]` consists of only digits.
- `nums[i]` will not have any leading zeros.

**Solution 1: self-implemented string compare**

```js
var kthLargestNumber = function (nums, k) {
  k = nums.length - k;
  nums.sort((a, b) => compareString(a, b));
  return nums[k];
};
function compareString(s1, s2) {
  if (s1.length < s2.length) {
    return -1;
  } else if (s1.length > s2.length) {
    return 1;
  }
  var i = 0;
  while (i < s1.length) {
    if (parseInt(s1[i]) < parseInt(s2[i])) {
      return -1;
    } else if (parseInt(s1[i]) > parseInt(s2[i])) {
      return 1;
    }
    i++;
  }
  return 0;
}
```

**Solution 2: BigInt easy solution**

```js
var kthLargestNumber = function (nums, k) {
  k = nums.length - k;
  nums.sort((a, b) => {
    var n1 = BigInt(a);
    var n2 = BigInt(b);
    if (n1 > n2) {
      return 1;
    } else if (n1 < n2) {
      return -1;
    }
    return 0;
  });
  return nums[k];
};
```

<div id="question-3"/>

### [1986. Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

**JS Solution:**

```js
var minSessions = function (tasks, sessionTime) {
  // best: sum / sessionTime
  // worst case: tasks.length
  var sum = tasks.reduce((a, b) => a + b);
  var l = Math.ceil(sum / sessionTime);
  var r = tasks.length;
  while (l < r) {
    var mid = Math.floor((l + r) / 2);
    var dp = new Array(mid).fill(0);
    if (!canPartition(tasks, dp, 0, sessionTime)) {
      l = mid + 1;
    } else {
      r = mid;
    }
  }
  return l;
};
function canPartition(nums, dp, index, sessionTime) {
  // Try to divide nums into K groups forEach under sessionTime
  if (index === nums.length) {
    for (var val of dp) {
      if (val > sessionTime) {
        return false;
      }
    }
    return true;
  }
  for (var i = 0; i < dp.length; i++) {
    if (dp[i] + nums[index] <= sessionTime) {
      dp[i] += nums[index];
      if (canPartition(nums, dp, index + 1, sessionTime)) {
        return true;
      }
      dp[i] -= nums[index];
    }
  }
  return false;
}
```

<div id="q3-1" />

#### 3.1 Similar Problem

[LC 698. Partition to K Equal Sum Subsets](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/)

```js
var canPartitionKSubsets = function (nums, k) {
  var sum = nums.reduce((a, b) => a + b);
  if (sum < k || sum % k !== 0) {
    return false;
  }
  var avg = Math.round(sum / k);
  var visited = new Set();
  function canDivide(nums, visited, index, curSum, k, avg) {
    if (k === 1) {
      // 1 or 0 to be the ending
      return true;
    }
    if (curSum === avg) {
      return canDivide(nums, visited, 0, 0, k - 1, avg);
    }
    for (var i = index; i < nums.length; i++) {
      if (visited.has(i) || curSum + nums[i] > avg) {
        continue;
      }
      visited.add(i);
      if (canDivide(nums, visited, i + 1, curSum + nums[i], k, avg)) {
        return true;
      }
      visited.delete(i);
    }
    return false;
  }
  return canDivide(nums, visited, 0, 0, k, avg);
};
```

<div id="question-4" />

### [1987. Number of Unique Good Subsequences](https://leetcode.com/problems/number-of-unique-good-subsequences/)

TODO

<div id="question-5"/>

### Summary

- Q3: fail to write the DFS, how to divide K groups within a target sum. But Binary search is good to think up!
