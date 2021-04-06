## Leetcode Contest 235- JavaScript

#### I. [1816.  Truncate Sentence](#question-1)

#### II. [1817. Finding the Users Active Minutes](#question-2)

#### III. [1818. Minimum Absolute Sum Difference](#question-3)

#### IV. [1819. Number of Different Subsequences GCDs](#question-4)

<div id="question-1"/>

### [1816.  Truncate Sentence](https://leetcode.com/contest/weekly-contest-235/problems/truncate-sentence/)

A  **sentence**  is a list of words that are separated by a single space with no leading or trailing spaces. Each of the words consists of  **only**  uppercase and lowercase English letters (no punctuation).

-   For example,  `"Hello World"`,  `"HELLO"`, and  `"hello world hello world"`  are all sentences.

You are given a sentence  `s`​​​​​​ and an integer  `k`​​​​​​. You want to  **truncate**  `s`​​​​​​ such that it contains only the  **first**  `k`​​​​​​ words. Return  `s`​​​​_​​ after  **truncating**  it._

**Example 1:**
**Input:** s = "Hello how are you Contestant", k = 4
**Output:** "Hello how are you"
**Explanation:**
The words in s are ["Hello", "how" "are", "you", "Contestant"].
The first 4 words are ["Hello", "how", "are", "you"].
Hence, you should return "Hello how are you".

**JavaScript:** one line solution:
```js
var truncateSentence = function(s, k) {
    return s.split(' ').slice(0, k).join(' '); 
};
```

Reference discussion article here: [Javascript running sum solution](https://leetcode.com/problems/maximum-ascending-subarray-sum/discuss/1119865/JavaScript-Linear-Running-Sum-solution)

<div id="question-2"/>

### [1817. Finding the Users Active Minutes](https://leetcode.com/problems/finding-the-users-active-minutes/)

You are given the logs for users' actions on LeetCode, and an integer  `k`. The logs are represented by a 2D integer array  `logs`  where each  `logs[i] = [IDi, timei]`  indicates that the user with  `IDi`  performed an action at the minute  `timei`.

**Multiple users**  can perform actions simultaneously, and a single user can perform  **multiple actions**  in the same minute.

The  **user active minutes (UAM)**  for a given user is defined as the  **number of unique minutes**  in which the user performed an action on LeetCode. A minute can only be counted once, even if multiple actions occur during it.

You are to calculate a  **1-indexed**  array  `answer`  of size  `k`  such that, for each  `j`  (`1 <= j <= k`),  `answer[j]`  is the  **number of users**  whose  **UAM**  equals  `j`.

Return  _the array_ `answer` _as described above_.

**Example 1:**
**Input:** logs = [[0,5],[1,2],[0,2],[0,5],[1,3]], k = 5
**Output:** [0,2,0,0,0]
**Explanation:**
The user with ID=0 performed actions at minutes 5, 2, and 5 again. Hence, they have a UAM of 2 (minute 5 is only counted once).
The user with ID=1 performed actions at minutes 2 and 3. Hence, they have a UAM of 2.
Since both users have a UAM of 2, answer[2] is 2, and the remaining answer[j] values are 0.

**JavaScript solution:**
Map + Set
```js
var findingUsersActiveMinutes = function(logs, k) {
    var ans = new Array(k+1).fill(0);
    var map = new Map();
    for(var i = 0; i<logs.length; i++)
    {
        var id = logs[i][0];
        var time = logs[i][1];
        if(!map.has(id))
        {
            map.set(id, new Set());
        }
        map.get(id).add(time);
    }
    for(var val of map.values())
    {
        ans[val.size]++;
    }
    ans.shift();
    return ans;  
};
```

<div id="question-3"/>

### [1818.  Minimum Absolute Sum Difference](https://leetcode.com/problems/minimum-absolute-sum-difference/)

You are given two positive integer arrays  `nums1`  and  `nums2`, both of length  `n`.

The  **absolute sum difference**  of arrays  `nums1`  and  `nums2`  is defined as the  **sum**  of  `|nums1[i] - nums2[i]|`  for each  `0 <= i < n`  (**0-indexed**).

You can replace  **at most one**  element of  `nums1`  with  **any**  other element in  `nums1`  to  **minimize**  the absolute sum difference.

Return the  _minimum absolute sum difference  **after**  replacing at most one  element in the array  `nums1`._  Since the answer may be large, return it  **modulo**  `109  + 7`.

`|x|`  is defined as:

-   `x`  if  `x >= 0`, or
-   `-x`  if  `x < 0`.

**Example 1:**
**Input:** nums1 = [1,7,5], nums2 = [2,3,5]
**Output:** 3
**Explanation:** There are two possible optimal solutions:
- Replace the second element with the first: [1,**7**,5] => [1,**1**,5], or
- Replace the second element with the third: [1,**7**,5] => [1,**5**,5].
Both will yield an absolute sum difference of `|1-2| + (|1-3| or |5-3|) + |5-5| =` 3.

**See the discussion article here:**  
- [DO NOT use the O(N) solution only reduce at Max_Diff index](https://leetcode.com/problems/minimum-absolute-sum-difference/discuss/1141630/DO-NOT-use-the-O(N)-solution-only-reduce-at-Max_Diff-index)
- [JavaScript: BinarySearch lower_bound O(n log n)](https://leetcode.com/problems/minimum-absolute-sum-difference/discuss/1141590/JavaScript-BinarySearch-lower_bound-O(n-log-n))

<div id="question-4" />

### [1819. Number of Different Subsequences GCDs](https://leetcode.com/problems/number-of-different-subsequences-gcds/)

You are given an array  `nums`  that consists of positive integers.

The  **GCD**  of a sequence of numbers is defined as the greatest integer that divides  **all**  the numbers in the sequence evenly.

-   For example, the GCD of the sequence  `[4,6,16]`  is  `2`.

A  **subsequence**  of an array is a sequence that can be formed by removing some elements (possibly none) of the array.

-   For example,  `[2,5,10]`  is a subsequence of  `[1,2,1,**2**,4,1,**5**,**10**]`.

Return  _the  **number**  of  **different**  GCDs among all  **non-empty**  subsequences of_  `nums`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/17/image-1.png)

**Input:** nums = [6,10,3]
**Output:** 5
**Explanation:** The figure shows all the non-empty subsequences and their GCDs.
The different GCDs are 6, 10, 3, 2, and 1.

**JavaScript Solution:**
see the discussion article here: [Javascript Math Solution](https://leetcode.com/problems/number-of-different-subsequences-gcds/discuss/1141743/Javascript-Math)
