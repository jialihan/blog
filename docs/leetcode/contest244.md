## Leetcode Contest 244 - JavaScript

#### I. [1886. Determine Whether Matrix Can Be Obtained By Rotation](#question-1)

#### II. [1887. Reduction Operations to Make the Array Elements Equal](#question-2)

#### III. [1888. Minimum Number of Flips to Make the Binary String Alternating](#question-3)

#### IV. [1889. Minimum Space Wasted From Packaging](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1886. Determine Whether Matrix Can Be Obtained By Rotation](https://leetcode.com/problems/determine-whether-matrix-can-be-obtained-by-rotation/)

Given two `n x n` binary matrices `mat` and `target`, return `true` _if it is possible to make_ `mat` _equal to_ `target` _by **rotating**_ `mat` _in **90-degree increments**, or_ `false` _otherwise._

**My JavaScript Solution:**

```js
var findRotation = function (mat, target) {
  for (var i = 0; i < 4; i++) {
    if (isSame2DArray(mat, target)) {
      return true;
    }
    if (i < 3) {
      // only need rotate max of 3 times
      rotate90(mat);
    }
  }
  return false;
};
function isSame2DArray(arr1, arr2) {
  return JSON.stringify(arr1) === JSON.stringify(arr2);
}
function rotate90(matrix) {
  matrix.reverse(); // reverse by row
  const n = matrix.length;
  // transpose
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < i; j++) {
      [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]];
    }
  }
}
```

<div id="question-2"/>

### [1887. Reduction Operations to Make the Array Elements Equal](https://leetcode.com/problems/reduction-operations-to-make-the-array-elements-equal/)

Given an integer array `nums`, your goal is to make all elements in `nums` equal. To complete one operation, follow these steps:

1.  Find the **largest** value in `nums`. Let its index be `i` (**0-indexed**) and its value be `largest`. If there are multiple elements with the largest value, pick the smallest `i`.
2.  Find the **next largest** value in `nums` **strictly smaller** than `largest`. Let its value be `nextLargest`.
3.  Reduce `nums[i]` to `nextLargest`.

Return _the number of operations to make all elements in_ `nums` _equal_.

**Example 1:**
**Input:** nums = [5,1,3]
**Output:** 3
**Explanation:** It takes 3 operations to make all elements in nums equal:

1. largest = 5 at index 0. nextLargest = 3. Reduce nums[0] to 3. nums = [3,1,3].
2. largest = 3 at index 0. nextLargest = 1. Reduce nums[0] to 1. nums = [1,1,3].
3. largest = 3 at index 2. nextLargest = 1. Reduce nums[2] to 1. nums = [1,1,1].

**Example 2:**
**Input:** nums = [1,1,1]
**Output:** 0
**Explanation:** All elements in nums are already equal.

**My solution:**

```js
var reductionOperations = function (nums) {
  var cnt = new Map();
  nums.forEach((el) => {
    var newcnt = (cnt.get(el) || 0) + 1;
    cnt.set(el, newcnt);
  });
  var keys = Array.from(cnt.keys());
  keys.sort((a, b) => b - a);
  var vals = keys.map((num) => cnt.get(num));

  // build preSum for counts
  for (var i = 1; i < vals.length; i++) {
    vals[i] += vals[i - 1];
  }

  var ans = vals.reduce((acc, cur, index) => {
    if (index < vals.length - 1) {
      acc += cur;
    }
    return acc;
  }, 0);

  return ans;
};
```

**Improved cleaner solution:**
Referenced discussion article here: [JS - sort & count](https://leetcode.com/problems/reduction-operations-to-make-the-array-elements-equal/discuss/1254036/JavaScript-Sort-and-Count)

```js
var reductionOperations = function (nums) {
  var cnt = 0;
  var n = nums.length;
  nums.sort((a, b) => a - b);
  for (var i = n - 1; i >= 1; i--) {
    if (nums[i] !== nums[i - 1]) {
      cnt += n - i;
    }
  }
  return cnt;
};
```

<div id="question-3"/>

### [1888. Minimum Number of Flips to Make the Binary String Alternating](https://leetcode.com/problems/minimum-number-of-flips-to-make-the-binary-string-alternating/)

You are given a binary string `s`. You are allowed to perform two types of operations on the string in any sequence:

- **Type-1: Remove** the character at the start of the string `s` and **append** it to the end of the string.
- **Type-2: Pick** any character in `s` and **flip** its value, i.e., if its value is `'0'` it becomes `'1'` and vice-versa.

Return _the **minimum** number of **type-2** operations you need to perform_ _such that_ `s` _becomes **alternating**._

The string is called **alternating** if no two adjacent characters are equal.

- For example, the strings `"010"` and `"1010"` are alternating, while the string `"0100"` is not.

**Example 1:**
**Input:** s = "111000"
**Output:** 2
**Explanation**: Use the first operation two times to make s = "100011".
Then, use the second operation on the third and sixth elements to make s = "101010".

**Example 2:**
**Input:** s = "010"
**Output:** 0
**Explanation**: The string is already alternating.

**Analysis:**
Sliding window: count NOT matched char at each index, after index `>=n`, we need to minus the previous index error count **outside** of the left of 888. Minimum Number of Flips to Make the Binary String Alternatingthe window.

- even: Not '0' at even index and Not '1' at odd index
  ```
  at index i(even):
  flip = s[i]!=='0' && (i%2===0) + s[i]!=='1' && (i%2===1)
  = s[i]!=='0' && (i%2===0) + s[i]==='0' && (i%2===1)
  = s[i]!=='0' && (i%2===0) + s[i]==='0' && !(i%2===0)
  = s[i]!=='0' XOR (i%2===1)
  = s[i]!=='0' XOR (i%2)
  ```
- odd: Not '0' at odd index and Not '1' at even index

```
	at index i(odd):
	flip = s[i]!=='0' && (i%2===1) + s[i]!=='1' && (i%2===0)
	= s[i]!=='0' && (i%2===1) + s[i]==='0' && (i%2===0)
	= s[i]==='0' XOR (i%2===1)
	= s[i]==='0' XOR (i%2)
```

**JS solution:**

```js
var minFlips = function (s) {
  var n = s.length;
  var min = n;
  var odd = 0; // odd is 0
  var even = 0; // even is 0
  // 111000111000
  for (var i = 0; i < n * 2; i++) {
    even += (s[i % n] !== "0") ^ i % 2;
    odd += (s[i % n] === "0") ^ i % 2;
    if (i >= n - 1) {
      if (i >= n) {
        even -= (s[i - n] !== "0") ^ (i - n) % 2;
        odd -= (s[i - n] === "0") ^ (i - n) % 2;
      }
      min = Math.min(min, odd, even);
    }
  }
  return min;
};
```

<div id="question-4" />

### [1889. Minimum Space Wasted From Packaging](https://leetcode.com/problems/minimum-space-wasted-from-packaging/)

You have `n` packages that you are trying to place in boxes, **one package in each box**. There are `m` suppliers that each produce boxes of **different sizes** (with infinite supply). A package can be placed in a box if the size of the package is **less than or equal to** the size of the box.

The package sizes are given as an integer array `packages`, where `packages[i]` is the **size** of the `ith` package. The suppliers are given as a 2D integer array `boxes`, where `boxes[j]` is an array of **box sizes** that the `jth` supplier produces.

You want to choose a **single supplier** and use boxes from them such that the **total wasted space** is **minimized**. For each package in a box, we define the space **wasted** to be `size of the box - size of the package`. The **total wasted space** is the sum of the space wasted in **all** the boxes.

- For example, if you have to fit packages with sizes `[2,3,5]` and the supplier offers boxes of sizes `[4,8]`, you can fit the packages of size-`2` and size-`3` into two boxes of size-`4` and the package with size-`5` into a box of size-`8`. This would result in a waste of `(4-2) + (4-3) + (8-5) = 6`.

Return _the **minimum total wasted space** by choosing the box supplier **optimally**, or_ `-1` _if it is **impossible** to fit all the packages inside boxes._ Since the answer may be **large**, return it **modulo** `109 + 7`.

**Example 1:**

**Input:** packages = [2,3,5], boxes = [[4,8],[2,8]]
**Output:** 6
**Explanation**: It is optimal to choose the first supplier, using two size-4 boxes and one size-8 box.
The total waste is (4-2) + (4-3) + (8-5) = 6.

Analysis:

```
packages: [2,3,5]
eg: one box [4,8]
step1: try to use the min box to cover as much as possible - 4 for [2,3]
step2: continue to use the next smallest box to cover remaining - 5 for [8]

Pseudo code:
sort(packages);
forEach box:
	sort current box;
	var wasteSpace;
	for(smallest to largest)
	{
		var upperbound_index = binary-search the index strictly larger than box
		wasteSpace += number of covered box * box_size - preSum[start to upperbound_index-1];
		// += (upperbound-index - start) * box_size - preSum[start, upperbound_index - 1];
	}
	result = min(result, wasteSpace)
return result;
```

**JS Code:**

```js
var minWastedSpace = function (packages, boxes) {
  var mod = 1000000007;
  var n = packages.length;
  var m = boxes.length;
  packages.sort((a, b) => a - b);
  var preSum = new Array(n + 1).fill(0);
  for (var i = 1; i <= n; i++) {
    preSum[i] = preSum[i - 1] + packages[i - 1];
  }
  var min = Infinity;
  for (var arr of boxes) {
    arr.sort((a, b) => a - b);
    var m = arr.length;
    var preIdx = 0;
    if (arr[m - 1] < packages[n - 1]) {
      continue;
    }
    var cur = 0;
    for (var b of arr) {
      if (b < packages[0]) {
        continue;
      }
      var j = binarysearch_upper(packages, b);
      cur += (j - preIdx) * b - (preSum[j] - preSum[preIdx]);
      preIdx = j;
      if (j >= n) {
        break;
      }
    }
    min = Math.min(min, cur);
    min = min % mod;
  }
  return min === Infinity ? -1 : min;
};

function binarysearch_upper(arr, target) {
  // find the first index that > target
  var l = 0;
  var r = arr.length;
  while (l < r) {
    var mid = Math.floor((l + r) / 2);
    if (arr[mid] <= target) {
      l = mid + 1;
    } else {
      r = mid;
    }
  }
  return l;
}
```

<div id="question-5"/>

### Summary

- 2427 / 14467
- keep moving!
- good to started on Q4 in contest !
