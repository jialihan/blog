## Leetcode Contest 228 - JavaScript

#### I. [1758.  Minimum Changes To Make Alternating Binary](#question-1)

#### II. [1759.  Count Number of Homogenous Substrings](#question-2)

#### III. [1760.  Minimum Limit of Balls in a Bag](#question-3)

#### IV. [1755.  Closest Subsequence Sum](#question-4)

#### V. [Summary](#summary)


<div id="question-1"/>

###  [1758.  Minimum Changes To Make Alternating Binary](https://leetcode.com/problems/minimum-changes-to-make-alternating-binary-string/)


You are given a string  `s`  consisting only of the characters  `'0'`  and  `'1'`. In one operation, you can change any  `'0'`  to  `'1'`  or vice versa.

The string is called alternating if no two adjacent characters are equal. For example, the string  `"010"`  is alternating, while the string  `"0100"`  is not.

Return  _the  **minimum**  number of operations needed to make_  `s`  _alternating_.

**Example 1:**
**Input:** s = "0100"
**Output:** 1
**Explanation:** If you change the last character to '1', s will be "0101", which is alternating.

**Analysis:**
- Only two situation
- starts with `'0'`
- starts with `'1'`

<div id="q1-1"/>

#### 1.1 original JavaScript solution
```js
var minOperations = function(s) {
    var cnt1 = 0;
    var cnt2 = 0;
    var c = s[0];
    for(var i=0;i<s.length;i++)
    {
        if(i%2===1 && s[i] === c || i%2===0 && s[i] !== c)
        {
            cnt1++;
        }
         if(i%2===1 && s[i] !== c || i%2===0 && s[i] === c)
        {
            cnt2++;
        }
    }
    return Math.min(cnt1, cnt2);
};
```

<div id="q1-2"/>

#### 1.2 Cleaner Code Solution
```
Tip: XOR is a fast way to tell when two items are different.
0^1 = 1^0 = 1  when a ≠ b then a^b ≠ 0
0^0 = 1^1 = 0  when a = b then a^b = 0
```
```js
var minOperations = function(s) {
    var cnt1 = 0;
    var cnt2 = 0;
    for (var i =0; i<s.length; i++)
    {
        cnt1 += (i&1) ^ (s[i] == '1') // first bit is 0
        cnt2 += (i&1) ^ (s[i] == '0') // first bit is 1
    }
    return Math.min(cnt1, cnt2);
};
```

<div id="question-2"/>

### [1759.  Count Number of Homogenous Substrings](https://leetcode.com/problems/count-number-of-homogenous-substrings/)

Given a string  `s`, return  _the number of  **homogenous**  substrings of_ `s`_._  Since the answer may be too large, return it  **modulo**  `109  + 7`.

A string is  **homogenous**  if all the characters of the string are the same.

A  **substring**  is a contiguous sequence of characters within a string.

**Example 1:**
**Input:** s = "abbcccaa"
**Output:** 13
**Explanation:** The homogenous substrings are listed as below:
"a"   appears 3 times.
"aa"  appears 1 time.
"b"   appears 2 times.
"bb"  appears 1 time.
"c"   appears 3 times.
"cc"  appears 2 times.
"ccc" appears 1 time.
3 + 1 + 2 + 1 + 3 + 2 + 1 = 13.

**JavaScript Solution: (Math)**

see discussion article here:  [Simple Math Solution](https://leetcode.com/problems/count-number-of-homogenous-substrings/discuss/1064714/JavaScript-Simple-Math-Solution)

<div id="question-3"/>

### [1760.  Minimum Limit of Balls in a Bag](https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag/)

You are given an integer array  `nums`  where the  `ith`  bag contains  `nums[i]`  balls. You are also given an integer  `maxOperations`.

You can perform the following operation at most  `maxOperations`  times:

-   Take any bag of balls and divide it into two new bags with a  **positive** number of balls.
-   For example, a bag of  `5`  balls can become two new bags of  `1`  and  `4`  balls, or two new bags of  `2`  and  `3`  balls.

Your penalty is the  **maximum**  number of balls in a bag. You want to  **minimize**  your penalty after the operations.

Return  _the minimum possible penalty after performing the operations_.

**Example 1:**
**Input:** nums = [9], maxOperations = 2
**Output:** 3
**Explanation:** 
- Divide the bag with 9 balls into two bags of sizes 6 and 3. [**9**] -> [6,3].
- Divide the bag with 6 balls into two bags of sizes 3 and 3. [**6**,3] -> [3,3,3].
The bag with the most number of balls has 3 balls, so your penalty is 3 and you should return 3.

**Example 2:**
**Input:** nums = [2,4,8,2], maxOperations = 4
**Output:** 2
**Explanation:**
- Divide the bag with 8 balls into two bags of sizes 4 and 4. [2,4,**8**,2] -> [2,4,4,4,2].
- Divide the bag with 4 balls into two bags of sizes 2 and 2. [2,**4**,4,4,2] -> [2,2,2,4,4,2].
- Divide the bag with 4 balls into two bags of sizes 2 and 2. [2,2,2,**4**,4,2] -> [2,2,2,2,2,4,2].
- Divide the bag with 4 balls into two bags of sizes 2 and 2. [2,2,2,2,2,**4**,2] -> [2,2,2,2,2,2,2,2].
The bag with the most number of balls has 2 balls, so your penalty is 2 an you should return 2.

**JavaScript Solution:**

Difficulty:
- direct way: how to divide to be optimal? - **Time Limit Exceed**
- Binary Search to find the best solution rather than construct the answer


see discussion here: [Javascript Binary Search Solution](https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag/discuss/1064702/JavaScript%3A-Binary-Search-solution-with-explanation)

<div id="question-4"/>

### [1761.  Minimum Degree of a Connected Trio in a Graph](https://leetcode.com/problems/minimum-degree-of-a-connected-trio-in-a-graph/)

You are given an undirected graph. You are given an integer  `n`  which is the number of nodes in the graph and an array  `edges`, where each  `edges[i] = [ui, vi]`  indicates that there is an undirected edge between  `ui`  and  `vi`.

A  **connected trio**  is a set of  **three**  nodes where there is an edge between  **every**  pair of them.

The  **degree of a connected trio**  is the number of edges where one endpoint is in the trio, and the other is not.

Return  _the  **minimum**  degree of a connected trio in the graph, or_  `-1`  _if the graph has no connected trios._

**JavaScript Solution:**

- **Adjacent list** is ok enough to determine a "trio" for 3 nodes
- brute force: note might **"TLE"**, here `|E| >> |V|`, so try to loop the nodes, rather than edge

See discussion here: [BruteForce + Pruning solution](https://leetcode.com/problems/minimum-degree-of-a-connected-trio-in-a-graph/discuss/1064588/JavaScript%3A-BruteForce-%2B-Pruning-O(n3))

### Summary
- When it's hard to construct the optimal solution, try BS to save time and only validate
- When TLE, try to think about the time complexity, different loops or binary search
- 1945 / 9869
- keep moving !!! 
