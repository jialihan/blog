## Leetcode Contest 251 - JavaScript

#### I. [1945. Sum of Digits of String After Convert](#question-1)

#### II. [1946. Largest Number After Mutating Substring](#question-2)

#### III. [1947. Maximum Compatibility Score Sum](#question-3)

#### IV. [1948. Delete Duplicate Folders in System](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1945. Sum of Digits of String After Convert](https://leetcode.com/problems/sum-of-digits-of-string-after-convert/)

You are given a string `s` consisting of lowercase English letters, and an integer `k`.

First, **convert** `s` into an integer by replacing each letter with its position in the alphabet (i.e., replace `'a'` with `1`, `'b'` with `2`, ..., `'z'` with `26`). Then, **transform** the integer by replacing it with the **sum of its digits**. Repeat the **transform** operation `k` **times** in total.

For example, if `s = "zbax"` and `k = 2`, then the resulting integer would be `8` by the following operations:

- **Convert**: `"zbax" ➝ "(26)(2)(1)(24)" ➝ "262124" ➝ 262124`
- **Transform #1**: `262124 ➝ 2 + 6 + 2 + 1 + 2 + 4 ➝ 17`
- **Transform #2**: `17 ➝ 1 + 7 ➝ 8`

Return _the resulting integer after performing the operations described above_.

**Example 1:**
**Input:** s = "iiii", k = 1
**Output:** 36
**Explanation:** The operations are as follows:

- Convert: "iiii" ➝ "(9)(9)(9)(9)" ➝ "9999" ➝ 9999
- Transform #1: 9999 ➝ 9 + 9 + 9 + 9 ➝ 36
  Thus the resulting integer is 36.

**Example 2:**
**Input:** s = "leetcode", k = 2
**Output:** 6
**Explanation:** The operations are as follows:

- Convert: "leetcode" ➝ "(12)(5)(5)(20)(3)(15)(4)(5)" ➝ "12552031545" ➝ 12552031545
- Transform #1: 12552031545 ➝ 1 + 2 + 5 + 5 + 2 + 0 + 3 + 1 + 5 + 4 + 5 ➝ 33
- Transform #2: 33 ➝ 3 + 3 ➝ 6
  Thus the resulting integer is 6.

**My JavaScript Solution:**

```js
/**
 * @param {string} s
 * @param {number} k
 * @return {number}
 */
var getLucky = function (s, k) {
  var str = [...s].map((el) => "" + (el.charCodeAt(0) - 96)).join("");
  for (var i = 0; i < k; i++) {
    str =
      [...str].reduce((acc, cur) => {
        acc += parseInt(cur);
        return acc;
      }, 0) + "";
  }
  return str;
};
```

<div id="question-2"/>

### [1946. Largest Number After Mutating Substring](https://leetcode.com/problems/largest-number-after-mutating-substring/)

You are given a string `num`, which represents a large integer. You are also given a **0-indexed** integer array `change` of length `10` that maps each digit `0-9` to another digit. More formally, digit `d` maps to digit `change[d]`.

You may choose to **mutate** any substring of `num`. To mutate a substring, replace each digit `num[i]` with the digit it maps to in `change` (i.e. replace `num[i]` with `change[num[i]]`).

Return _a string representing the **largest** possible integer after **mutating** (or choosing not to) any substring of_ `num`.

A **substring** is a contiguous sequence of characters within the string.

**Example 1:**
**Input:** num = "132", change = [9,8,5,0,3,6,4,2,6,8]
**Output:** "832"
**Explanation:** Replace the substring "1":

- 1 maps to change[1] = 8.
  Thus, "132" becomes "832".
  "832" is the largest number that can be created, so return it.

**Example 2:**
**Input:** num = "021", change = [9,4,3,5,7,2,1,9,0,6]
**Output:** "934"
**Explanation:** Replace the substring "021":

- 0 maps to change[0] = 9.
- 2 maps to change[2] = 3.
- 1 maps to change[1] = 4.
  Thus, "021" becomes "934".
  "934" is the largest number that can be created, so return it.

**Analysis:**

- find the highest bit that can change
- find continuous bits after that as much as possible: change when `curBit <= changeBit`

**My JavaScript Solution:**

```js
/**
 * @param {string} num
 * @param {number[]} change
 * @return {string}
 */
var maximumNumber = function (num, change) {
  var n = num.length;
  var cur = [...num];
  var i = 0;
  while (parseInt(cur[i]) >= change[cur[i]]) {
    i++;
  }
  if (i < n) {
    cur[i] = change[cur[i]]; // change at highest digit
  }
  if (i < n) {
    // find continous ranges which can be changed
    var j = i + 1;
    while (j < n && parseInt(cur[j]) <= change[cur[j]]) {
      cur[j] = change[cur[j]];
      j++;
    }
  }
  return cur.join("");
};
```

<div id="question-3"/>

### [1947. Maximum Compatibility Score Sum](https://leetcode.com/problems/maximum-compatibility-score-sum/)

There is a survey that consists of `n` questions where each question's answer is either `0` (no) or `1` (yes).

The survey was given to `m` students numbered from `0` to `m - 1` and `m` mentors numbered from `0` to `m - 1`. The answers of the students are represented by a 2D integer array `students` where `students[i]` is an integer array that contains the answers of the `ith` student (**0-indexed**). The answers of the mentors are represented by a 2D integer array `mentors` where `mentors[j]` is an integer array that contains the answers of the `jth` mentor (**0-indexed**).

Each student will be assigned to **one** mentor, and each mentor will have **one** student assigned to them. The **compatibility score** of a student-mentor pair is the number of answers that are the same for both the student and the mentor.

- For example, if the student's answers were `[1, 0, 1]` and the mentor's answers were `[0, 0, 1]`, then their compatibility score is 2 because only the second and the third answers are the same.

You are tasked with finding the optimal student-mentor pairings to **maximize** the **sum of the compatibility scores**.

Given `students` and `mentors`, return _the **maximum compatibility score sum** that can be achieved._

**Example 1:**
**Input:** students = [[1,1,0],[1,0,1],[0,0,1]], mentors = [[1,0,0],[0,0,1],[1,1,0]]
**Output:** 8
**Explanation:** We assign students to mentors in the following way:

- student 0 to mentor 2 with a compatibility score of 3.
- student 1 to mentor 0 with a compatibility score of 2.
- student 2 to mentor 1 with a compatibility score of 3.
  The compatibility score sum is 3 + 2 + 3 = 8.

**Analysis:**

- Think too much during contest
- just use bruteforce & O(n) to compute each pair of score
- NO trie!!!
- Add Memo during dfs to improve a little bit

**Reference Article:**

- discussion: [Java || Bactraking || Easy to Understand](https://leetcode.com/problems/maximum-compatibility-score-sum/discuss/1360853/Java-oror-Bactraking-oror-Easy-to-Understand)

**Bruteforce - JavaScript:**

```js
function maxCompatibilitySum(students, mentors) {
  var m = students.length;
  var max = 0;
  dfs(new Set(), 0, 0);
  function dfs(visited, index, score) {
    if (index >= m) {
      max = Math.max(max, score);
      return;
    }
    for (var j = 0; j < m; j++) {
      if (visited.has(j)) {
        continue;
      }
      visited.add(j);
      dfs(
        visited,
        index + 1,
        score + computeScore(students[index], mentors[j])
      );
      visited.delete(j);
    }
  }
  return max;
}
var memo = new Map();
function computeScore(arr1, arr2) {
  var key = arr1.toString() + arr2.toString();
  if (memo.has(key)) {
    return memo.get(key);
  }
  var n = arr1.length;
  var score = 0;
  for (var i = 0; i < n; i++) {
    if (arr1[i] === arr2[i]) {
      score++;
    }
  }
  memo.set(key, score);
  return score;
}
```

<div id="question-4" />

### [1948. Delete Duplicate Folders in System](https://leetcode.com/problems/delete-duplicate-folders-in-system/)

Due to a bug, there are many duplicate folders in a file system. You are given a 2D array `paths`, where `paths[i]` is an array representing an absolute path to the `ith` folder in the file system.

- For example, `["one", "two", "three"]` represents the path `"/one/two/three"`.

Two folders (not necessarily on the same level) are **identical** if they contain the **same non-empty** set of identical subfolders and underlying subfolder structure. The folders **do not** need to be at the root level to be identical. If two or more folders are **identical**, then **mark** the folders as well as all their subfolders.

- For example, folders `"/a"` and `"/b"` in the file structure below are identical. They (as well as their subfolders) should **all** be marked:
  - `/a`
  - `/a/x`
  - `/a/x/y`
  - `/a/z`
  - `/b`
  - `/b/x`
  - `/b/x/y`
  - `/b/z`
- However, if the file structure also included the path `"/b/w"`, then the folders `"/a"` and `"/b"` would not be identical. Note that `"/a/x"` and `"/b/x"` would still be considered identical even with the added folder.

Once all the identical folders and their subfolders have been marked, the file system will **delete** all of them. The file system only runs the deletion once, so any folders that become identical after the initial deletion are not deleted.

Return _the 2D array_ `ans` _containing the paths of the **remaining** folders after deleting all the marked folders. The paths may be returned in **any** order_.

**Notes:**

- use **"post order"** traversal to store & process all sub-children structure
- find and mark \*"same" / "visited" **identical children-path before parent node**
- **Reference**: [Discussion: C++ Tree building and trimming](https://leetcode.com/problems/delete-duplicate-folders-in-system/discuss/1360768/C%2B%2B-Tree-building-and-trimming)

**JavaScript Solution:**

```js
class Node {
  children = {};
  constructor(val) {
    this.val = val;
  }
}
/**
 * @param {string[][]} paths
 * @return {string[][]}
 */
var deleteDuplicateFolder = function (paths) {
  var root = buildTree(paths);
  var visited = new Map(); // <sub-paths, parentNode>
  function postOrder(node) {
    var sub = "";
    for (var key of Object.keys(node.children)) {
      var next = node.children[key];
      sub += postOrder(next);
    }
    if (sub) {
      if (visited.has(sub)) {
        visited.get(sub).delete = true;
        node.delete = true;
      } else {
        visited.set(sub, node);
      }
    }
    return "(" + sub + "," + node.val + ")";
  }
  postOrder(root);
  var res = [];
  for (var startNode of Object.values(root.children)) {
    if (!startNode.delete) {
      res.push(...printPaths(startNode));
    }
  }
  return res;
};
function printPaths(root) {
  var res = [];
  function helper(node, list) {
    list.push(node.val);
    res.push(list.slice());
    for (var next of Object.values(node.children)) {
      if (next.delete) {
        continue;
      }
      helper(next, list);
    }
    list.pop();
  }
  helper(root, []);
  return res;
}
function buildTree(paths) {
  var root = new Node(-1);
  for (var path of paths) {
    var cur = root;
    for (var val of path) {
      if (!cur.children[val]) {
        cur.children[val] = new Node(val);
      }
      cur = cur.children[val];
    }
  }
  return root;
}
```

<div id="question-5"/>

### Summary

- 5489 / 13663
- Q3: just brute force try all possible pairs: dfs or bitmask, TODO: understand the [DP + Bit Mask solution](https://leetcode.com/problems/maximum-compatibility-score-sum/discuss/1360742/C++-DP-+-Bitmask-or-4ms)
- Q4: **post order** in tree traversal
