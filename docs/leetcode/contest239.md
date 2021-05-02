## Leetcode Contest 239 - JavaScript

#### I. [1848.  Minimum Distance to the Target Element](#question-1)

#### II. [1849.  Splitting a String Into Descending Consecutive Values](#question-2)

#### III. [1850.  Minimum Adjacent Swaps to Reach the Kth Smallest Number](#question-3)

#### IV. [1851. Minimum Interval to Include Each Query - not solved](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1848.  Minimum Distance to the Target Element](https://leetcode.com/problems/minimum-distance-to-the-target-element/)

Given an integer array  `nums`  **(0-indexed)**  and two integers  `target`  and  `start`, find an index  `i`  such that  `nums[i] == target`  and  `abs(i - start)`  is  **minimized**. Note that `abs(x)` is the absolute value of  `x`.

Return  `abs(i - start)`.

It is  **guaranteed**  that  `target`  exists in  `nums`.

**Example 1:**
**Input:** nums = [1,2,3,4,5], target = 5, start = 3
**Output:** 1
**Explanation:** nums[4] = 5 is the only value equal to target, so the answer is abs(4 - 3) = 1.

**Example 2:**
**Input:** nums = [1], target = 1, start = 0
**Output:** 0
**Explanation:** nums[0] = 1 is the only value equal to target, so the answer is abs(0 - 0) = 1.

**JavaScript Solution:** 
```js
var getMinDistance = function(nums, target, start) {
    var i = nums.indexOf(target);
    var res = nums.length;
    while(i>=0)
    {
        res = Math.min(res, Math.abs(i-start));
        i = nums.indexOf(target, i+1);
    }
    return res;
};
```

<div id="question-2"/>

### [1849.  Splitting a String Into Descending Consecutive Values](https://leetcode.com/problems/splitting-a-string-into-descending-consecutive-values/)

You are given a string  `s`  that consists of only digits.

Check if we can split  `s`  into  **two or more non-empty substrings**  such that the  **numerical values**  of the substrings are in  **descending order**  and the  **difference**  between numerical values of every two  **adjacent**  **substrings**  is equal to  `1`.

-   For example, the string  `s = "0090089"`  can be split into  `["0090", "089"]`  with numerical values  `[90,89]`. The values are in descending order and adjacent values differ by  `1`, so this way is valid.
-   Another example, the string  `s = "001"`  can be split into  `["0", "01"]`,  `["00", "1"]`, or  `["0", "0", "1"]`. However all the ways are invalid because they have numerical values  `[0,1]`,  `[0,1]`, and  `[0,0,1]`  respectively, all of which are not in descending order.

Return  `true`  _if it is possible to split_  `s`​​​​​​  _as described above__, or_ `false` _otherwise._

A  **substring**  is a contiguous sequence of characters in a string.

**Example 1:**
**Input:** s = "1234"
**Output:** false
**Explanation:** There is no valid way to split s.

**Example 2:**
**Input:** s = "050043"
**Output:** true
**Explanation:** s can be split into ["05", "004", "3"] with numerical values [5,4,3].
The values are in descending order with adjacent values differing by 1.

**JavaScript solution:**
```js
var splitString = function(s) {
    // 050043  -> n = 6
    // dp[5] = 4 -> dp[5] = 3 + 1;
    // dp[4] = for j = i+1 ... n -> dp[j] && sub(i,j) === dp[j] + 1 ? sub(i,j) : -1;
    var n = s.length;
    if(n<2)
    {
        return false;
    }
    var dp = new Array(n).fill(-1).map(el=>new Set());
    dp[n-1].add(parseInt(s[n-1])); 
    for(var i = n-2; i>=0; i--)
    {
        for(var j=i+1; j<n;j++)
        {
            var cur = parseInt(s.slice(i,j));
            if(cur===0)
            {
                continue;
            }
            if(dp[j].size>=0 && dp[j].has(cur-1))
            {
                dp[i].add(cur);
            }
        }
        dp[i].add(parseInt(s.slice(i))); // Note: add substring s[i, n-1] after each index
    }
    return dp[0].size > 1;
};
```

Also see my discussion article here: [JS DP Solution O(n^2)](https://leetcode.com/problems/splitting-a-string-into-descending-consecutive-values/discuss/1186930/JavaScript-DP-with-explanation-O(n2))

<div id="question-3"/>

### [1850.  Minimum Adjacent Swaps to Reach the Kth Smallest Number](https://leetcode.com/problems/minimum-adjacent-swaps-to-reach-the-kth-smallest-number/)

You are given a string  `num`, representing a large integer, and an integer  `k`.

We call some integer  **wonderful**  if it is a  **permutation**  of the digits in  `num`  and is  **greater in value**  than  `num`. There can be many wonderful integers. However, we only care about the  **smallest-valued**  ones.

-   For example, when  `num = "5489355142"`:
    -   The 1st  smallest wonderful integer is  `"5489355214"`.
    -   The 2nd  smallest wonderful integer is  `"5489355241"`.
    -   The 3rd  smallest wonderful integer is  `"5489355412"`.
    -   The 4th  smallest wonderful integer is  `"5489355421"`.

Return  _the  **minimum number of adjacent digit swaps**  that needs to be applied to_ `num` _to reach the_ `kth` _**smallest wonderful**  integer_.

The tests are generated in such a way that  `kth` smallest wonderful integer exists.

**Example 1:**
**Input:** num = "5489355142", k = 4
**Output:** 2
**Explanation:** The 4th smallest wonderful number is "5489355421". To get this number:
- Swap index 7 with index 8: "5489355142" -> "5489355412"
- Swap index 8 with index 9: "5489355412" -> "5489355421"

**Analysis:**

**Find the next larger permutation:**
- Step1: find the 1st NON-Increasing from the end at **'i'** : `s[i] < s[i+1]`
- Step2: find the 1st digit larger than `s[i]` from the end at **'j'** : `s[j] > s[i]`
- Step3: swap `i`  & `j`
- Step4: sort in ascending order from `s[i+1,....n-1]` for remaining part

For example:
```text
// 5489355142  | n = 10
// 1. find the first Non-increasing index i = 7 ->  1 < 4
// 2. find first larger than num[i] from the high digit to low: j = 9 -> 2 > 1
// 3. swap j & i
// 4. sort(num, i+1, n-1)
```
```js
function nextLargerPermutation(arr)
{
    var n = arr.length;
    var i = n-2;
    while(i>=0 && arr[i] >= arr[i+1])
    {
        i--;
    }
    if(i>=0)
    {
        var j = n-1;
        while(j>i && arr[j]<=arr[i])
        {
            j--;
        }
        swap(arr, i, j);
        var p2 = arr.slice(i+1, n).sort((a,b)=>a-b);
        arr = arr.slice(0,i+1).concat(p2);
    }
    return arr;
}
function swap(a, i, j)
{
    var tmp = a[i];
    a[i] = a[j];
    a[j] = tmp;
}
```

**Count Numbers of swaps in two permutations:**
```js
function countSwaps(s1, s2, n) {
    // find Min Adjacemt swaps number between two permutations
    var i = 0, j = 0, cnt = 0;
    while (i < n){
        j = i;
        while (s1[j] !== s2[i]) 
            j++;
        while (j>i) {
            // each adjacent swap
            var tmp = s1[j];
            s1[j] = s1[j - 1];
            s1[j-1] = tmp;
            j--;
            cnt++;
        }
        i++;
    }
    return cnt;
}
```

**JavaScript Solution:**
```js
var getMinSwaps = function(num, k) {
    var arr = [...num].map(el=>parseInt(el));
    var n = num.length;
    while(k>0)
    {
        arr = nextLargerPermutation(arr);
        k--;
    }
    var arr2 = [...num].map(el=>parseInt(el));
    return countSwaps(arr, arr2, n);
};
```

<div id="question-4" />

### [1851. Minimum Interval to Include Each Query](https://leetcode.com/problems/minimum-interval-to-include-each-query/)

**Not solved !** 

<div id="question-5"/>

### Summary

- 1998 / 10870
- How to find next larger permutation ? 
- Keep moving
