## Leetcode Contest 249 - JavaScript

#### I. [1929. Concatenation of Array](#question-1)

#### II. [1930. Unique Length-3 Palindromic Subsequences](#question-2)

#### III. [1931. Painting a Grid With Three Different Colors - not solved](#question-3)

#### IV. [1932. Merge BSTs to Create Single BST](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1929. Concatenation of Array](https://leetcode.com/problems/concatenation-of-array/)

Given an integer array `nums` of length `n`, you want to create an array `ans` of length `2n` where `ans[i] == nums[i]` and `ans[i + n] == nums[i]` for `0 <= i < n` (**0-indexed**).

Specifically, `ans` is the **concatenation** of two `nums` arrays.

Return _the array_ `ans`.

**My JavaScript Solution:**

```js
var getConcatenation = function (nums) {
  return [...nums, ...nums];
};
```

<div id="question-2"/>

### [1930. Unique Length-3 Palindromic Subsequences](https://leetcode.com/problems/unique-length-3-palindromic-subsequences/)

Given a string `s`, return _the number of **unique palindromes of length three** that are a **subsequence** of_ `s`.

Note that even if there are multiple ways to obtain the same subsequence, it is still only counted **once**.

A **palindrome** is a string that reads the same forwards and backwards.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

**Example 1:**
**Input:** s = "aabca"
**Output:** 3
**Explanation:** The 3 palindromic subsequences of length 3 are:

- "aba" (subsequence of "aabca")
- "aaa" (subsequence of "aabca")
- "aca" (subsequence of "aabca")

**My JavaScript Solution:**
find first and last index:

```js
/**
 * @param {string} s
 * @return {number}
 */
var countPalindromicSubsequence = function (s) {
  var visited = new Set();
  var res = new Set();
  var i = 0;
  while (i < s.length) {
    if (visited.has(s[i])) {
      i++;
      continue;
    }
    visited.add(s[i]);
    var j = s.lastIndexOf(s[i]);
    if (i === j) {
      i++;
      continue;
    }
    for (var k = i + 1; k < j; k++) {
      res.add(s[i] + s[k] + s[j]);
    }
    i++;
  }
  return res.size;
};
```

<div id="question-3"/>

### [1931. Painting a Grid With Three Different Colors](https://leetcode.com/problems/painting-a-grid-with-three-different-colors/)

You are given two integers `m` and `n`. Consider an `m x n` grid where each cell is initially white. You can paint each cell **red**, **green**, or **blue**. All cells **must** be painted.

Return _the number of ways to color the grid with **no two adjacent cells having the same color**_. Since the answer can be very large, return it **modulo** `109 + 7`.

**Not solved!**

<div id="question-4" />

### [1932. Merge BSTs to Create Single BST](https://leetcode.com/problems/merge-bsts-to-create-single-bst/)

You are given `n` **BST (binary search tree) root nodes** for `n` separate BSTs stored in an array `trees` (**0-indexed**). Each BST in `trees` has **at most 3 nodes**, and no two roots have the same value. In one operation, you can:

- Select two **distinct** indices `i` and `j` such that the value stored at one of the **leaves** of `trees[i]` is equal to the **root value** of `trees[j]`.
- Replace the leaf node in `trees[i]` with `trees[j]`.
- Remove `trees[j]` from `trees`.

Return _the **root** of the resulting BST if it is possible to form a valid BST after performing_ `n - 1` _operations, or_ `null` _if it is impossible to create a valid BST_.

A BST (binary search tree) is a binary tree where each node satisfies the following property:

- Every node in the node's left subtree has a value **strictly less** than the node's value.
- Every node in the node's right subtree has a value **strictly greater** than the node's value.

A leaf is a node that has no children.

**Reference:**

- https://leetcode.com/problems/merge-bsts-to-create-single-bst/discuss/1330172/Java-or-two-HashMaps
- https://leetcode.com/problems/merge-bsts-to-create-single-bst/discuss/1330973/Java-Solution-Using-HashMap-READABLE-code

**Edge Case**: cycle !!!

```
   3    and   2(replaced)
  /             \
 2 (replaced)     3
   \
     3
```

**JavaScript Solution:**

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode[]} trees
 * @return {TreeNode}
 */
var canMerge = function (trees) {
  var map = new Map(); // store original roots
  trees.forEach((t) => map.set(t.val, t));
  var parent = new Map(); // <childValue, parentNode>
  var visited = new Set(); // already removed root
  for (var root of trees) {
    // check merge left
    var p = parent.get(root);
    if (
      root.left &&
      map.has(root.left.val) &&
      !visited.has(map.get(root.left.val))
    ) {
      if (p !== map.get(root.left.val)) {
        // avoid parent node cycle
        // console.log("merge left: ", root.left.val )
        root.left = map.get(root.left.val);
        parent.set(map.get(root.left.val), root);
        visited.add(map.get(root.left.val));
        map.delete(root.left.val);
      }
    }
    // check merge right
    if (
      root.right &&
      map.has(root.right.val) &&
      !visited.has(map.get(root.right.val))
    ) {
      if (p !== map.get(root.right.val)) {
        // avoid parent node cycle
        // console.log("merge right: ", root.right.val )
        root.right = map.get(root.right.val);
        parent.set(map.get(root.right.val), root);
        visited.add(map.get(root.right.val));
        map.delete(root.right.val);
      }
    }
  }
  if (map.size !== 1) {
    return null;
  }
  for (var key of map.keys()) {
    var node = map.get(key);
    if (isValidBST(node)) {
      return node;
    } else {
      return null;
    }
  }
};
function isValidBST(node, left, right) {
  if (node === null) {
    return true;
  }
  if (left && node.val <= left.val) {
    return false;
  }
  if (right && node.val >= right.val) {
    return false;
  }
  return (
    isValidBST(node.left, left, node) && isValidBST(node.right, node, right)
  );
}
```

<div id="question-5"/>

### Summary

- 1842 / 12832
- Q3: DP for 3 colors still NOT solved, TODO, cannot understand the math pattern from row 1 to row 5
- Q4: BST tree rewrite in JS after contest, already understand!
