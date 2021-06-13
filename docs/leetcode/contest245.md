## Leetcode Contest 245 - JavaScript

#### I. [1897. Redistribute Characters to Make All Strings Equal](#question-1)

#### II. [1898. Maximum Number of Removable Charactersl](#question-2)

#### III. [1899. Merge Triplets to Form Target Triplet](#question-3)

#### IV. [1900. The Earliest and Latest Rounds Where Players Compete - not solved!I](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1897. Redistribute Characters to Make All Strings Equal](https://leetcode.com/problems/redistribute-characters-to-make-all-strings-equal/)

You are given an array of strings `words` (**0-indexed**).

In one operation, pick two **distinct** indices `i` and `j`, where `words[i]` is a non-empty string, and move **any** character from `words[i]` to **any** position in `words[j]`.

Return `true` _if you can make **every** string in_ `words` _**equal** using **any** number of operations_, _and_ `false` _otherwise_.

**Example 1:**
**Input:** words = ["abc","aabc","bc"]
**Output:** true
**Explanation:** Move the first 'a' in `words[1] to the front of words[2], to make` `words[1]` = "abc" and words[2] = "abc".
All the strings are now equal to "abc", so return `true`.

**Example 2:**
**Input:** words = ["ab","a"]
**Output:** false
**Explanation:** It is impossible to make all the strings equal using the operation.

**Analysis:**
Count for each char which must satisfy the two rules:

- any char should be `>= n`
- any char total number should be times of `n`, `count % n === 0`

**My solution:**

```js
var makeEqual = function (words) {
  var n = words.length;
  if (n <= 1) return true;
  var cnt = {};
  [...words].forEach((w) => {
    for (var c of [...w]) {
      cnt[c] = (cnt[c] || 0) + 1;
    }
  });
  var values = Array.from(Object.values(cnt));
  for (var val of values) {
    if (val < n || val % n !== 0) {
      return false;
    }
  }
  return true;
};
```

**Reference:**
discussion article: [JavaScript Count each char](https://leetcode.com/problems/redistribute-characters-to-make-all-strings-equal/discuss/1268724/JavaScript-count-each-char)

<div id="question-2"/>

### [1898. Maximum Number of Removable Characters](https://leetcode.com/problems/maximum-number-of-removable-characters/)

You are given two strings `s` and `p` where `p` is a **subsequence** of `s`. You are also given a **distinct 0-indexed** integer array `removable` containing a subset of indices of `s` (`s` is also **0-indexed**).

You want to choose an integer `k` (`0 <= k <= removable.length`) such that, after removing `k` characters from `s` using the **first** `k` indices in `removable`, `p` is still a **subsequence** of `s`. More formally, you will mark the character at `s[removable[i]]` for each `0 <= i < k`, then remove all marked characters and check if `p` is still a subsequence.

Return _the **maximum**_ `k` _you can choose such that_ `p` _is still a **subsequence** of_ `s` _after the removals_.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

**Example 1:**
**Input:** s = "abcacb", p = "ab", removable = [3,1,0]
**Output:** 2
**Explanation**: After removing the characters at indices 3 and 1, "a**b**c**a**cb" becomes "accb".
"ab" is a subsequence of "**a**cc**b**".
If we remove the characters at indices 3, 1, and 0, "**ab**c**a**cb" becomes "ccb", and "ab" is no longer a subsequence.
Hence, the maximum k is 2.

**Example 2:**
**Input:** s = "abcbddddd", p = "abcd", removable = [3,2,1,4,5,6]
**Output:** 1
**Explanation**: After removing the character at index 3, "abc**b**ddddd" becomes "abcddddd".
"abcd" is a subsequence of "**abcd**dddd".

**Example 3:**
**Input:** s = "abcab", p = "abc", removable = [0,1,2,3,4]
**Output:** 0
**Explanation**: If you remove the first index in the array removable, "abc" is no longer a subsequence.

**Analysis:**
Binary search from count is `0 - K (length of removable)`, count is 1-based

- '0': means remove NONE of the indexes
- 'k': means remove from index `[0,...k-1]` , total k indexes

**My solution:**

```js
var maximumRemovals = function (s, p, removable) {
  var r = removable.length;
  var l = 0;
  while (l < r) {
    var mid = Math.floor((l + r + 1) / 2); // remove mid count of indexes: 1-based
    var arr = [...s];
    for (var t = 0; t < mid; t++) {
      arr[removable[t]] = "#";
    }
    var tmp = arr.filter((el) => el !== "#").join("");
    if (!isSubSeq(tmp, p)) {
      r = mid - 1;
    } else {
      l = mid;
    }
  }
  return l;
};
function isSubSeq(s, t) {
  var pre = -1;
  for (var i = 0; i < t.length; i++) {
    var cur = s.indexOf(t[i], pre + 1);
    if (cur < 0 || cur <= pre) {
      return false;
    }
    pre = cur;
  }
  return true;
}
```

**Reference:**
discussion article: [JavaScript - Binary Search](https://leetcode.com/problems/maximum-number-of-removable-characters/discuss/1268714/JavaScript-Binary-Search)

<div id="question-3"/>

### [1899. Merge Triplets to Form Target Triplet](https://leetcode.com/problems/merge-triplets-to-form-target-triplet/)

A **triplet** is an array of three integers. You are given a 2D integer array `triplets`, where `triplets[i] = [ai, bi, ci]` describes the `ith` **triplet**. You are also given an integer array `target = [x, y, z]` that describes the **triplet** you want to obtain.

To obtain `target`, you may apply the following operation on `triplets` **any number** of times (possibly **zero**):

- Choose two indices (**0-indexed**) `i` and `j` (`i != j`) and **update** `triplets[j]` to become `[max(ai, aj), max(bi, bj), max(ci, cj)]`.
  - For example, if `triplets[i] = [2, 5, 3]` and `triplets[j] = [1, 7, 5]`, `triplets[j]` will be updated to `[max(2, 1), max(5, 7), max(3, 5)] = [2, 7, 5]`.

Return `true` _if it is possible to obtain the_ `target` _**triplet**_ `[x, y, z]` _as an **element** of_ `triplets`_, or_ `false` _otherwise_.

**Example 1:**
**Input:** triplets = [[2,5,3],[1,8,4],[1,7,5]], target = [2,7,5]
**Output:** true
**Explanation\*\***:\*\* Perform the following operations:

- Choose the first and last triplets [[2,5,3],[1,8,4],[1,7,5]]. Update the last triplet to be [max(2,1), max(5,7), max(3,5)] = [2,7,5]. triplets = [[2,5,3],[1,8,4],[2,7,5]]
  The target triplet [2,7,5] is now an element of triplets.

**My brute force solution:**

- valid index for each `x, y, z`
- loop valid pairs: `for all i, j, k`

```js
var mergeTriplets = function (triplets, target) {
  var n = triplets.length;
  // <x, idx...>
  var [x, y, z] = target;
  var idx = {};
  idx[x] = [];
  idx[y] = [];
  idx[z] = [];
  triplets.forEach((t, i) => {
    if (t[0] === x) {
      idx[x].push(i);
    }
    if (t[1] === y) {
      idx[y].push(i);
    }
    if (t[2] === z) {
      idx[z].push(i);
    }
  });
  if (idx[x].length == 0 || idx[y].length == 0 || idx[z].length == 0) {
    return false;
  }
  for (var i of idx[x]) {
    for (var j of idx[y])
      for (var t of idx[z]) {
        if (
          Math.max(triplets[i][0], triplets[j][0], triplets[t][0]) === x &&
          Math.max(triplets[i][1], triplets[j][1], triplets[t][1]) === y &&
          Math.max(triplets[i][2], triplets[j][2], triplets[t][2]) === z
        ) {
          return true;
        }
      }
  }
  return false;
};
```

**Optimized Solution: O(n)**
Find the best local solution when at index `i`, only the current number `<= target[i]` is valid solution.

```js
var mergeTriplets = function (triplets, target) {
  var best = [0, 0, 0];
  var [x, y, z] = target;
  for (var t of triplets) {
    if (t[0] <= x && t[1] <= y && t[2] <= z) {
      best[0] = Math.max(t[0], best[0]);
      best[1] = Math.max(t[1], best[1]);
      best[2] = Math.max(t[2], best[2]);
    }
  }
  return best[0] === x && best[1] === y && best[2] === z;
};
```

<div id="question-4" />

### [1900. The Earliest and Latest Rounds Where Players Compete](https://leetcode.com/problems/the-earliest-and-latest-rounds-where-players-compete/)

Not-solved!

<div id="question-5"/>

### Summary

- 1994 / 12724
- keep moving!
- smarter solution needed in Q3 !
