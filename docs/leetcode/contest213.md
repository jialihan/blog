## Leetcode Contest 213 - JavaScript

I. [1640. Check Array Formation Through Concatenation](#question-1)

II. [1641. Count Sorted Vowel Strings](#question-2)

III. [1642. Furthest Building You Can Reach - not solved](#question-3)

IV. [1643. Kth Smallest Instructions - not solved](#question-4)

V. [Summary](#summary)

<div id="question-1"/>

### 1640. Check Array Formation Through Concatenation

You are given an array of **distinct** integers `arr` and an array of integer arrays `pieces`, where the integers in `pieces` are **distinct**. Your goal is to form `arr` by concatenating the arrays in `pieces` **in any order**. However, you are **not** allowed to reorder the integers in each array `pieces[i]`.

Return `true` _if it is possible_ _to form the array_ `arr` _from_ `pieces`. Otherwise, return `false`.

**Example 1:**
**Input:** arr = [85], pieces = [[85]]
**Output:** true

**Example 2:**
**Input:** arr = [15,88], pieces = [[88],[15]]
**Output:** true
**Explanation:** Concatenate `[15]` then `[88]`

**My JavaScript Solution:**

```
var canFormArray = function(arr, pieces) {
    var indexes = {};
    for(var i = 0; i<arr.length; i++)
    {
            indexes[arr[i]] = i;
    }

    for(var p of pieces)
        {
            for (var i = 1; i< p.length; i++)
                {
                    var pre = indexes[p[i-1]];
                    var cur = indexes[p[i]];
                    if(pre !== cur-1)
                        {
                            return false;
                        }
                }
        }
    return true;
};
```

<div id="question-2"/>

### 1641. Count Sorted Vowel Strings

Given an integer `n`, return _the number of strings of length_ `n` _that consist only of vowels (_`a`_,_ `e`_,_ `i`_,_ `o`_,_ `u`_) and are **lexicographically sorted**._

A string `s` is **lexicographically sorted** if for all valid `i`, `s[i]` is the same as or comes before `s[i+1]` in the alphabet.

**Example 1:**
**Input:** n = 1
**Output:** 5
**Explanation:** The 5 sorted strings that consist of vowels only are `["a","e","i","o","u"].`

**Example 2:**
**Input:** n = 2
**Output:** 15
**Explanation:** The 15 sorted strings that consist of vowels only are
["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"].
Note that "ea" is not a valid string since 'e' comes after 'a' in the alphabet.

**My Brute Force Solution:**

I know it's the problem to find the rules and patterns for those 5 numbers and orders. But when i am in contest want to save time, and not sure I can think about the cool math patterns, so I **GIVE UP the analysis** 😥.

```
var countVowelStrings = function(n) {
    var res = 0;
    function dfs(n, s)
    {
        if(n === 0)
        {
                res++;
                return;
        }
        var cur = s.length === 0 ? 1 : parseInt(s.slice(-1));
        for(var i = cur; i<= 5; i++)
            {
                dfs(n-1, s+i);
            }
    }
    dfs(n, "")
    return res;
};
```

**After Contest Analysis:**

| Digits | a   | e   | i   | o   | u   |
| ------ | --- | --- | --- | --- | --- |
| X      | 1   | 1   | 1   | 1   | 1   |
| XX     | 5   | 4   | 3   | 2   | 1   |
| XXX    | 15  | 10  | 6   | 3   | 1   |
| XXXX   | 35  | 20  | 10  | 4   | 1   |
| ...    |

Then the rule is:
([reference solution:](https://leetcode.com/problems/count-sorted-vowel-strings/discuss/918507/Java-DP-O%28n%29-time-Easy-to-understand) from "Discussion" section. )

```
a = (a + e + i + o + u);
e = (e + i + o + u);
i = (i + o + u);
o = (o + u);
u = (u);
```

**JavaScript Better Solution**:

```
var countVowelStrings = function(n) {
    var a = 1, e = 1, i = 1, o = 1, u = 1;
    while(n > 1) {
            a = a + e + i + o + u;
            e = e + i + o + u;
            i = i + o + u;
            o = o + u;
            u = u;
            n--;
        }
    return a + e + i + o + u;
};
```

<div id="question-3"/>

### [1642. Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

You are given an integer array `heights` representing the heights of buildings, some `bricks`, and some `ladders`.

You start your journey from building `0` and move to the next building by possibly using bricks or ladders.

While moving from building `i` to building `i+1` (**0-indexed**),

- If the current building's height is **greater than or equal** to the next building's height, you do **not** need a ladder or bricks.
- If the current building's height is **less than** the next building's height, you can either use **one ladder** or `(h[i+1] - h[i])` **bricks**.

_Return the furthest building index (0-indexed) you can reach if you use the given ladders and bricks optimally._

**Example 1:** I really love this GIF animation example, haha, clear and cool ! 😎

**Wrong: TIME Out DFS Solution:**

```
var furthestBuilding = function(heights, bricks, ladders) {
    /**
    Timeout
    */
    var res = 0;
    function dfs(arr, i, b, l)
    {
        if(i === arr.length-1)
            {
                res = arr.length-1;
                return;
            }
        var diff = arr[i+1] - arr[i];
        if(diff <= 0)
            {
                dfs(arr, i+1, b,l);
            }
        else
            {
                if(b < diff && l <= 0)
                {
                    res = Math.max(res, i);
                    return;
                }
                else
                {
                    if(l>0)
                    {
                             dfs(arr, i+1, b, l-1);
                    }
                    if(b>= diff)
                    {
                            dfs(arr, i+1, b-diff, l);
                    }

                 }
            }
    }
    dfs(heights, 0, bricks, ladders);
    return res;
};
```

**My Correct Binary Search Solution:**

See my solution in this "discussion" section:
https://leetcode.com/problems/furthest-building-you-can-reach/discuss/918621/JavaScript-Binary-Search

<div id="question-4"/>

### 1643. Kth Smallest Instructions

Bob is standing at cell `(0, 0)`, and he wants to reach `destination`: `(row, column)`. He can only travel **right** and **down**. You are going to help Bob by providing **instructions** for him to reach `destination`.

The **instructions** are represented as a string, where each character is either:

- `'H'`, meaning move horizontally (go **right**), or
- `'V'`, meaning move vertically (go **down**).

Multiple **instructions** will lead Bob to `destination`. For example, if `destination` is `(2, 3)`, both `"HHHVV"` and `"HVHVH"` are valid **instructions**.

However, Bob is very picky. Bob has a lucky number `k`, and he wants the `kth` **lexicographically smallest instructions** that will lead him to `destination`. `k` is **1-indexed**.

Given an integer array `destination` and an integer `k`, return _the_ `kth` _**lexicographically smallest instructions** that will take Bob to_ `destination`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/12/ex1.png)

**Input:** destination = [2,3], k = 1
**Output:** "HHHVV"
**Explanation:** All the instructions that reach (2, 3) in lexicographic order are as follows:
["HHHVV", "HHVHV", "HHVVH", "HVHHV", "HVHVH", "HVVHH", "VHHHV", "VHHVH", "VHVHH", "VVHHH"].

**Referenced Solution after contest:**

- how to count ways starting with "H" or "V"?
- due to lexical order, if starting with "HXXXX", the remaining will be total combinations of select (total_H - 1) from total: `Comb(n, remaining_H)`
- For example: remaining is 4 digit "XXXX", there are `col` numbers of "H" ( go horizontal), then the total ways of "HXXXX" is C(4, col_H-1)

```
var kthSmallestPath = function(destination, k) {
    function comb(n,k)
    {
        // select k from n total combinations
        var res = 1;
        for(var i=0; i < k;i++){
            res *= n-i;
            res = Math.floor(res / (i+1));
        }
        return res;
    }
    let [row, col] = destination;
    let s = "";
    while(row>0 && col>0)
    {
        const cnt = comb(row+col-1, col-1); // starting with "H", total ways
        if(k<=cnt)
        {
                s = s + 'H';
                col--;
        }
        else
        {
                k -= cnt;
                s = s + 'V';
                row--;
        }
    }
    // if left extra row or col
    while(col>0)
    {
            s += 'H';
            col--;
    }
    while(row>0)
    {
            s += 'V';
            row--;
    }
    return s;
};
```

<div id="summary"/>

### Summary

- 3rd question Binary Search upper bound bug!!! that's killing my all time! Reference: [find upper bound in BinarySearch](https://medium.com/@CalvinChankf/how-to-deal-with-lower-upper-bound-binary-search-b9ce744673df)
- use **"Math.ceil()" or "(left+right+1) / 2"**, if there is a dead loop.
- Next time try to find patterns or rules behind the title, use DFS is kind of brute force solution.
- Good Point is: pretty close to fix the BinarySearch solution after the DFS time out.
- 3622 / 10630
