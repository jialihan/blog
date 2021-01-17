## Leetcode Contest 224 - JavaScript

#### I.  [1725.  Number Of Rectangles That Can Form The Largest Squar](#question-1)

#### II. [1726. Tuple with Same Product](#question-2)

#### III. [1727.  Largest Submatrix With Rearrangements - not solved](#question-3)

#### IV. [1728. Cat and Mouse II - not solved](#question-4)

#### V. [Summary](#summary)


<div id="question-1"/>

### [1725.  Number Of Rectangles That Can Form The Largest Square](https://leetcode.com/problems/number-of-rectangles-that-can-form-the-largest-square/)

You are given an array  `rectangles`  where  `rectangles[i] = [li, wi]`  represents the  `ith`  rectangle of length  `li`  and width  `wi`.

You can cut the  `ith`  rectangle to form a square with a side length of  `k`  if both  `k <= li`  and  `k <= wi`. For example, if you have a rectangle  `[4,6]`, you can cut it to get a square with a side length of at most  `4`.

Let  `maxLen`  be the side length of the  **largest**  square you can obtain from any of the given rectangles.

Return  _the  **number**  of rectangles that can make a square with a side length of_ `maxLen`.

**Example 1:**
**Input:** rectangles = [[5,8],[3,9],[5,12],[16,5]]
**Output:** 3
**Explanation:** The largest squares you can get from each rectangle are of lengths [5,3,5,5].
The largest possible square is of length 5, and you can get it out of 3 rectangles.

**JavaScript Solution: (very simple)**
```js
/**
 * @param {number[][]} rectangles
 * @return {number}
 */
var countGoodRectangles = function(rectangles) {
    const res = rectangles.map(el=>{
        return Math.min(el[0], el[1]);
    });
    let max = -1;
    res.forEach(el=>{
        max = Math.max(max,el)
    });
    let cnt = 0;
    // console.log(res)
    res.forEach(el=>{
        if(el=== max)
            cnt++;
    })
    return cnt;
};
```

<div id="question-2"/>

### [1726. Tuple with Same Product](https://leetcode.com/problems/tuple-with-same-product/submissions/)

Given an array  `nums`  of  **distinct**  positive integers, return  _the number of tuples_ `(a, b, c, d)` _such that_ `a * b = c * d` _where_ `a`_,_ `b`_,_ `c`_, and_ `d` _are elements of_ `nums`_, and_ `a != b != c != d`_._

**Example 1:**
**Input:** nums = [2,3,4,6]
**Output:** 8
**Explanation:** There are 8 valid tuples:
(2,6,3,4) , (2,6,4,3) , (6,2,3,4) , (6,2,4,3)
(3,4,2,6) , (4,3,2,6) , (3,4,6,2) , (4,3,6,2)

**Analysis:**
- How to count unique pairs of 4 ? O(n^3) - brute force solution when I am at contest.
- But luckily the data set is not large, so O(n^3) is accepted !
- After contest: optimized solution using hashmap - O(n^2)

JavaScript **Brute Force Solution:**
```js
var tupleSameProduct = function(nums) {
    nums.sort((a,b)=>a-b);
    var cnt = 0;
    var n = nums.length;
    for(var i = 0; i<n-3; i++)
        for(var j = i+1; j<n-2; j++)
            for(var k = j+1; k<n-1; k++)
            {
                var a = nums[i];
                var b = nums[j];
                var c = nums[k];
                if( b*c % a !== 0 )
                {
                      continue;
                }
                var d = b * c / a;
                if(d>nums[n-1])
                {
                    break;    
                }
                if(!nums.includes(d) || d === a || d===b || d===c)
                {
                      continue;
                }
                cnt++;
            }
    return cnt * 8; 
};
```

JavaScript **Optimized Solution:**
```js
var tupleSameProduct = function(nums) {
    var map = new Map();
    var n = nums.length;
    var res = 0;
    for(var i = 0; i<n; i++)
        for(var j = i+1; j<n; j++)
        {
            var prod= nums[i]*nums[j];
            if(map.has(prod))
            {
                res += 8*map.get(prod);
                map.set(prod, map.get(prod)+1);
            }
            else
            {
                map.set(prod, 1);
            }
        }
    return res;
};
```


<div id="question-3"/>

### [1727.  Largest Submatrix With Rearrangements](https://leetcode.com/problems/largest-submatrix-with-rearrangements/)

You are given a binary matrix  `matrix`  of size  `m x n`, and you are allowed to rearrange the  **columns**  of the  `matrix`  in any order.

Return  _the area of the largest submatrix within_ `matrix` _where  **every**  element of the submatrix is_ `1` _after reordering the columns optimally._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/29/screenshot-2020-12-30-at-40536-pm.png ':size=460x226')

**Input:** matrix = [[0,0,1],[1,1,1],[1,0,1]]
**Output:** 4
**Explanation:** You can rearrange the columns as shown above.
The largest submatrix of 1s, in bold, has an area of 4.

Analysis:
- [Maximal Rectangles - LC85](https://leetcode.com/problems/maximal-rectangle/) - hard
- But this one is medium, we can re-arrange, then we need to find a already existing way to find count and calculate the optimal results.
	- count from the bottom row
	- for each row, updating the accumulated histogram column sum
	- for each row, compute the current maximal rectangle
- Not solved at contest


JavaScript Solution after Contest:
```js
var largestSubmatrix = function(matrix) {
    let m = matrix.length;
    let n = matrix[0].length;
    let dp = [...Array(n)].fill(0);
    let max = 0;
    for(let i = m-1; i>=0; i--)
    {
        for(let j = 0; j<n; j++)
        {
            if(matrix[i][j] === 1)
            {
                dp[j]++;
            }
            else
                dp[j] = 0;
        }
        let tmp = [...dp];
        tmp.sort((a,b)=>a-b);
        for(let j = 0; j<n; j++)
        {
            if(tmp[j] > 0)
            {
                max = Math.max(tmp[j] * (n-j), max);
            }
        }
    }
    return max;
};
```

<div id="question-4" />

### [1728. Cat and Mouse II](https://leetcode.com/problems/cat-and-mouse-ii/)
	-- Not sovled

<div id="summary"/>

### Summary
- Question 2: optimized with hashmap, remember to use hashmap when finding pairs (assuming distinct numbers)
- Question 3: pretty close, how to count the histogram max rectangle, don't think it's complicated like LC-85
- 2565 / 11023, keep moving!
