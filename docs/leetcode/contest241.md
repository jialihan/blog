## Leetcode Contest 241 - JavaScript

#### I. [1863.  Sum of All Subset XOR Totals](#question-1)

#### II. [1864.  Minimum Number of Swaps to Make the Binary String Alternating](#question-2)

#### III. [1865. Finding Pairs With a Certain Sum](#question-3)

#### IV. [1866.  Number of Ways to Rearrange Sticks With K Sticks Visible](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1863.  Sum of All Subset XOR Totals](https://leetcode.com/problems/sum-of-all-subset-xor-totals/)


The  **XOR total**  of an array is defined as the bitwise  `XOR`  of **all its elements**, or  `0`  if the array is **empty**.

-   For example, the  **XOR total**  of the array  `[2,5,6]`  is  `2 XOR 5 XOR 6 = 1`.

Given an array  `nums`, return  _the  **sum**  of all  **XOR totals**  for every  **subset**  of_ `nums`.

**Note:**  Subsets with the  **same**  elements should be counted  **multiple**  times.

An array  `a`  is a  **subset**  of an array  `b`  if  `a`  can be obtained from  `b`  by deleting some (possibly zero) elements of  `b`.

**Example 1:**
**Input:** nums = [1,3]
**Output:** 6
**Explanation:** The 4 subsets of [1,3] are:
- The empty subset has an XOR total of 0.
- [1] has an XOR total of 1.
- [3] has an XOR total of 3.
- [1,3] has an XOR total of 1 XOR 3 = 2.
0 + 1 + 3 + 2 = 6


**JavaScript Solution:** 
```js
var subsetXORSum = function(nums) {
  var n = nums.length;
  if(n === 0) {  
      return 0;
  }
  var sum = 0;
  for (var i = 1; i < (1 << n ); i++) {
    var subset = [];
    for (var j = 0; j < n; j++)
      if (i & (1 << j))
      {
        subset.push(nums[j]);
      }
    var cur = subset.reduce((acc, cur)=>acc ^ cur);
    sum += cur;
  }
  return sum; 
};
```

Also see my **discussion article** here: [All combinations with Bitmask](https://leetcode.com/problems/sum-of-all-subset-xor-totals/discuss/1211226/Javascript-All-Combinations-with-Bitmask)

<div id="question-2"/>

### [1864.  Minimum Number of Swaps to Make the Binary String Alternating](https://leetcode.com/problems/minimum-number-of-swaps-to-make-the-binary-string-alternating/)

Given a binary string  `s`, return  _the  **minimum**  number of character swaps to make it  **alternating**, or_ `-1` _if it is impossible._

The string is called  **alternating**  if no two adjacent characters are equal. For example, the strings  `"010"`  and  `"1010"`  are alternating, while the string  `"0100"`  is not.

Any two characters may be swapped, even if they are **not adjacent**.

**Example 1:**
**Input:** s = "111000"
**Output:** 1
**Explanation:** Swap positions 1 and 4: "111000" -> "101010"
The string is now alternating.

**Example 2:**
**Input:** s = "010"
**Output:** 0
**Explanation:** The string is already alternating, no swaps are needed.

**Example 3:**
**Input:** s = "1110"
**Output:** -1

**JavaScript solution:**
```js
var minSwaps = function(s) {
    var cnt = { "1": 0, "0": 0};
    var arr = [...s];
    for(var c of arr)
    {
        cnt[c]++;
    }
    var ones = cnt["1"];
    var zeros = cnt["0"];
    if(Math.abs(ones-zeros)>=2)
    {
        return -1;
    }
    // when even is 0, e1 != 0  -> error count
    // when even is 1, e1 != 1 -> error count
    var e1 = 0, e2=0, o1 = 0, o2=0;
    for(var i = 0; i<s.length; i++)
    {
        if(i%2===0)
        {
            if(s[i]==='0')
            {
                e2++;
            }
            else
            {
                e1++;
            }
        }
        else
        {
            if(s[i]==='1')
            {
                o2++;
            }
            else
            {
                o1++;
            }
        }
    }
    if(e1 !== o1)
    {
        // cannot swap
        return e2;
    }
    else if(e2 !== o2)
    {
        // cannot swap
        return e1;
    }
    else;
    {
        return Math.min(e1, e2)
    }
};
```

Also see my **discussion article** here: [Javascript - Count at even & odd index](https://leetcode.com/problems/minimum-number-of-swaps-to-make-the-binary-string-alternating/discuss/1211214/JavaScript-Count-with-two-types-string-at-odd-and-even-index)

<div id="question-3"/>

### [1865. Finding Pairs With a Certain Sum](https://leetcode.com/problems/finding-pairs-with-a-certain-sum/)

You are given two integer arrays  `nums1`  and  `nums2`. You are tasked to implement a data structure that supports queries of two types:

1.  **Add**  a positive integer to an element of a given index in the array  `nums2`.
2.  **Count**  the number of pairs  `(i, j)`  such that  `nums1[i] + nums2[j]`  equals a given value (`0 <= i < nums1.length`  and  `0 <= j < nums2.length`).

Implement the  `FindSumPairs`  class:
-   `FindSumPairs(int[] nums1, int[] nums2)`  Initializes the  `FindSumPairs`  object with two integer arrays  `nums1`  and  `nums2`.
-   `void add(int index, int val)`  Adds  `val`  to  `nums2[index]`, i.e., apply  `nums2[index] += val`.
-   `int count(int tot)`  Returns the number of pairs  `(i, j)`  such that  `nums1[i] + nums2[j] == tot`.

**Analysis:**
- **Wrong**: Store every pair sum in Map, update time `O(Len1*2)`, query time `O(1)` ---- **TIME OUT**
- **Correct**: Only store Nums2 in Map, update time `O(1)`, query time `O(Len1)`

**JavaScript solution:**
```js
var FindSumPairs = function(nums1, nums2) {
    this.map = new Map();
    this.arr1 = nums1.slice();
    this.arr2 = nums2.slice();
    for(var i = 0; i <nums2.length; i++)
    {
        var cnt = this.map.get(nums2[i]) || 0;
        this.map.set(nums2[i], cnt+1);
    }
};

FindSumPairs.prototype.add = function(index, val) {
    var pre = this.arr2[index];
    this.map.set(pre, this.map.get(pre)-1);
    this.arr2[index] += val;
    var cur = this.arr2[index];
    this.map.set(cur, (this.map.get(cur)||0) + 1);
};

FindSumPairs.prototype.count = function(tot) {
    var res = 0;
    for(var num of this.arr1)
    {
        if(this.map.has(tot-num))
        {
            res += this.map.get(tot-num);
        }
    }
    return res;
};
```

<div id="question-4" />

### [1866.  Number of Ways to Rearrange Sticks With K Sticks Visible](https://leetcode.com/problems/number-of-ways-to-rearrange-sticks-with-k-sticks-visible/)

There are  `n`  uniquely-sized sticks whose lengths are integers from  `1`  to  `n`. You want to arrange the sticks such that  **exactly**  `k` sticks are  **visible**  from the left. A stick is  **visible**  from the left if there are no  **longer** sticks to the  **left**  of it.

-   For example, if the sticks are arranged  `[1,3,2,5,4]`, then the sticks with lengths  `1`,  `3`, and  `5`  are visible from the left.

Given  `n`  and  `k`, return  _the  **number**  of such arrangements_. Since the answer may be large, return it  **modulo**  `109  + 7`.

**Example 1:**
**Input:** n = 3, k = 2
**Output:** 3
**Explanation:** [1,3,2], [2,3,1], and [2,1,3] are the only arrangements such that exactly 2 sticks are visible.
The visible sticks are underlined.

**Example 2:**
**Input:** n = 5, k = 5
**Output:** 1
**Explanation:** [1,2,3,4,5] is the only arrangement such that all 5 sticks are visible.
The visible sticks are underlined.

**Analysis:**
- Referenced solution here: [Simple DP c++ with explanation](https://leetcode.com/problems/number-of-ways-to-rearrange-sticks-with-k-sticks-visible/discuss/1211099/Simple-2d-DP-C%2B%2B-(with-Intuition))

**My JS solution:**
```js
var rearrangeSticks = function(n, k) {
    var mod = 1000000007;
    
    // dp[n][k]: ways of n sticks that can see k
    var dp = new Array(n+1).fill(-1).map(el=>new Array(k+1).fill(0));
    
    // all visible only has 1 way
    dp[1][1] = 1;
    
    // bottom up - DP
    for(var i = 2; i<=n; i++)
    {
        for(var j = 1; j<=i; j++)
        {
            dp[i][j] = dp[i-1][j-1]; // if we put smallest 1 to the first index
            dp[i][j] += dp[i-1][j] * (i-1); // if we hide "1" to any places n-1, since we hide one, then we need to see one more [j-1+1] in total [i-1] sticks 
            dp[i][j] %= mod;
        }
    }
    return dp[n][k];
};
```

<div id="question-5"/>

### Summary

- 4616 / 11572
-  Q1 --> haha, Originally I don't suggest to use bitmask in JavaScript because JS is [64 bit double-presicion number](https://www.w3schools.com/js/js_bitwise.asp).
	- Before a bitwise operation is performed, JavaScript converts numbers to 32 bits signed integers.
	- After the bitwise operation is performed, the result is converted back to 64 bits JavaScript numbers.
- Q3: HashMap: a little bit improve is time saving !!! when **Two Sum** prob, remember only to store one number is enough, use `(num, target-num)` to query!
- Q4 is a Math problem or DP...... love the simple DP **bottom up** solution
- Keep moving - more practice next week !