## Leetcode Contest 252 - JavaScript

#### I. [1952. Three Divisors](#question-1)

#### II. [1953. Maximum Number of Weeks for Which You Can Work](#question-2)

#### III. [1954. Minimum Garden Perimeter to Collect Enough Apples](#question-3)

#### IV. [1955. Count Number of Special Subsequences](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1952. Three Divisors](https://leetcode.com/problems/three-divisors/)

Given an integer `n`, return `true` _if_ `n` _has **exactly three positive divisors**. Otherwise, return_ `false`.

An integer `m` is a **divisor** of `n` if there exists an integer `k` such that `n = k * m`.

**Example 1:**
**Input:** n = 2
**Output:** false
**Explantion:** 2 has only two divisors: 1 and 2.

**Example 2:**
**Input:** n = 4
**Output:** true
**Explantion:** 4 has three divisors: 1, 2, and 4.

**My JavaScript Solution:**

```js
var isThree = function (n) {
  var set = new Set();
  for (var i = 1; i <= Math.sqrt(n) && set.size <= 3; i++) {
    if (n % i === 0) {
      set.add(i);
      set.add(n / i);
    }
  }
  return set.size === 3;
};
```

<div id="question-2"/>

### [1953. Maximum Number of Weeks for Which You Can Work](https://leetcode.com/problems/maximum-number-of-weeks-for-which-you-can-work/)

There are `n` projects numbered from `0` to `n - 1`. You are given an integer array `milestones` where each `milestones[i]` denotes the number of milestones the `ith` project has.

You can work on the projects following these two rules:

- Every week, you will finish **exactly one** milestone of **one** project. You **must** work every week.
- You **cannot** work on two milestones from the same project for two **consecutive** weeks.

Once all the milestones of all the projects are finished, or if the only milestones that you can work on will cause you to violate the above rules, you will **stop working**. Note that you may not be able to finish every project's milestones due to these constraints.

Return _the **maximum** number of weeks you would be able to work on the projects without violating the rules mentioned above_.

**Reference & Explanation:**

The Greedy idea is, if the max elements is larger than 2 times of the sum of the rest elements. then the max answer is 2 \* rest + 1, because the best strategy is pick max, pick other, pick max, pick other....pick max. otherwise, we can finish all of the milestones.

https://leetcode.com/problems/maximum-number-of-weeks-for-which-you-can-work/discuss/1375381/C%2B%2B-max-element-solution.-Greedy

**My JavaScript Solution:**

```js
var numberOfWeeks = function (arr) {
  var sum = arr.reduce((a, b) => a + b);
  var rest = sum - Math.max(...arr);
  return Math.min(sum, rest * 2 + 1);
};
```

<div id="question-3"/>

### [1954. Minimum Garden Perimeter to Collect Enough Apples](https://leetcode.com/problems/minimum-garden-perimeter-to-collect-enough-apples/)

In a garden represented as an infinite 2D grid, there is an apple tree planted at **every** integer coordinate. The apple tree planted at an integer coordinate `(i, j)` has `|i| + |j|` apples growing on it.

You will buy an axis-aligned **square plot** of land that is centered at `(0, 0)`.

Given an integer `neededApples`, return _the **minimum perimeter** of a plot such that **at least**_ `neededApples` _apples are **inside or on** the perimeter of that plot_.

The value of `|x|` is defined as:

- `x` if `x >= 0`
- `-x` if `x < 0`

**Analysis:**

- compute each `1/8 length` apple's count
- subtract duplicate counts on 8 corners: `4 corner with one apple` + `4 corner with 2*d apples`

**Math - JavaScript:**

```js
var minimumPerimeter = function (neededApples) {
  function countLayerApples(d) {
    // 1/8 len of apples = d+ (1+d)+ (2+d)+ ....+ (d+d)
    // = d * (d+1) + (1+2....+d)
    // subtract duplicates: 1 apple at 4 corner and (d+d) apple at 4 corder
    var sumd = ((1 + d) * d) / 2;
    return (d * (d + 1) + sumd) * 8 - (1 + d * 2) * 4;
  }
  var d = 0;
  while (neededApples > 0) {
    d++;
    var cur = countLayerApples(d);
    neededApples -= cur;
  }
  return d * 8;
};
```

<div id="question-4" />

### [1955. Count Number of Special Subsequences](https://leetcode.com/problems/count-number-of-special-subsequences/)

A sequence is **special** if it consists of a **positive** number of `0`s, followed by a **positive** number of `1`s, then a **positive** number of `2`s.

- For example, `[0,1,2]` and `[0,0,1,1,1,2]` are special.
- In contrast, `[2,1,0]`, `[1]`, and `[0,1,2,0]` are not special.

Given an array `nums` (consisting of **only** integers `0`, `1`, and `2`), return _the **number of different subsequences** that are special_. Since the answer may be very large, **return it modulo** `109 + 7`.

A **subsequence** of an array is a sequence that can be derived from the array by deleting some or no elements without changing the order of the remaining elements. Two subsequences are **different** if the **set of indices** chosen are different.

**Example 1:**
**Input:** nums = [0,1,2,2]
**Output:** 3
**Explanation:** The special subsequences are [0,1,2,2], [0,1,2,2], and [0,1,2,2].

**Analysis:**

```
// reverse order
0 1 2 2
for 2: (self) + (self + last2) + (last2) = 1 + cnt2 + cnt2
for 1: (self + last2) + (self + last1 + last2) = cnt2 + cnt1
for 0: (self + last1) + (self + last0 + last1) = cnt1 + cnt0
```

**JavaScript Solution:**

```js
var countSpecialSubsequences = function (nums) {
  var a = 0,
    b = 0,
    c = 0;
  var mod = 1000000007;
  for (var i = nums.length - 1; i >= 0; i--) {
    var num = nums[i];
    if (num === 2) {
      c = c * 2 + 1;
      c = c % mod;
    } else if (num === 1) {
      b += b + c;
      b = b % mod;
    } else if (num === 0) {
      a += a + b;
      a = a % mod;
    }
  }
  return a;
};
```

<div id="question-5"/>

### Summary

- 3694 / 14252
- Q2: Math problem doesn't finish this one, doing tasks problems need to think more on these questions. To find the math pattern inside it.
- Q4: **O(1)** space to count, need to understand and think about how to count these rules and patterns in Math.
