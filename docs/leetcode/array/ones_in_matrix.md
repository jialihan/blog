##  Squares, Rectangles, Ones in matrix 

####  I. [Ones in Matrix:  221. Maximal Square](#question1)

####  II. [Ones in Matrix:  85. Maximal Rectangle](#question2)

####  III. [Ones in Matrix:  1504. Count Submatrices With All Ones](#question3)

<div id="question1" />

### I. Ones in Matrix:  [221. Maximal Square](https://leetcode.com/problems/maximal-square/)

Given an  `m x n`  binary  `matrix`  filled with  `0`'s and  `1`'s,  _find the largest square containing only_  `1`'s  _and return its area_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)

**Input:** matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
**Output:** 4

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg)

**Input:** matrix = [["0","1"],["1","0"]]
**Output:** 1

#### Javascript Solution 1: DP - basic
 Basic DP using space complexity: `dp[][]: O(m*n)`
 ```js
 var maximalSquare = function(matrix) {
    var m = matrix.length;
    var n = matrix[0].length;
    var dp = new Array(m+1).fill(null).map(el=>new Array(n+1).fill(0));
    var maxLen = 0;
    for(var i = 1; i<=m; i++)
        for(var j = 1; j<=n; j++)
        {
            if(matrix[i-1][j-1] === '1')
            {
                dp[i][j] = Math.min(Math.min(dp[i-1][j], dp[i][j-1]), dp[i-1][j-1] ) + 1;   
                maxLen = Math.max(maxLen, dp[i][j]);
            }
        }
    return maxLen*maxLen;
};
 ```
 
#### Javascript Solution 2: DP - improve O(1) space

Just add up and store the DP results in the original arguments `matrix[m][n]`

```js
var maximalSquare = function(matrix) {
    var m = matrix.length;
    var n = matrix[0].length;
    var maxLen = 0;
    // O(1) space
    for(var i = 0; i<m; i++)
        for(var j = 0; j<n; j++)
        {
            if(matrix[i][j] === '0')
            {
                continue;
            }
            if(i>0&&j>0)
            {
                matrix[i][j] = Math.min(matrix[i-1][j], matrix[i][j-1], matrix[i-1][j-1] ) + 1;  
            }
            maxLen = Math.max(maxLen, matrix[i][j]);    
        }
    return maxLen ** 2;
};
```

<div id="question2" />

### [85.  Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)

Given a  `rows x cols` binary  `matrix`  filled with  `0`'s and  `1`'s, find the largest rectangle containing only  `1`'s and return  _its area_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/14/maximal.jpg)

**Input:** matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
**Output:** 6
**Explanation:** The maximal rectangle is shown in the above picture.

**Example 2:**
**Input:** matrix = []
**Output:** 0

**Analysis:**

![image](../../assets/lc85-dp.png ':size=417x179')

Steps to find max area here:
- reverse order from column `j = col n to col 1`
- from current row to last row: `i = row i to row k`, until `dp[k][j]  !== 0 `
- width_min =`min(preSum[i][j], preSum[k][j])`
- height = `k-i+1`
- maxArea = `max(maxArea, width_min * height)` 

**Solution:**
```js
var maximalRectangle = function(matrix) {
  if (matrix.length <= 0) 
      return 0;
  const m = matrix.length;
  const n = matrix[0].length;
  const dp = new Array(m).fill(null).map(() => new Array(n).fill(0));
   // build preSum on each row: DP[][]
   for(var i=0; i<m; i++)
      for(var j=0; j<n; j++)
      {
          if(j===0)
          {
              dp[i][j] = matrix[i][0]==='1' ? 1 : 0; 
          }
          else
          {
               dp[i][j] =  matrix[i][j]==='0' ? 0 : dp[i][j-1] + 1;   
          }
      }
    
  let maxArea = 0;
  for (let i = 0; i<m; i++) {
    for (let j = n-1; j >=0 ; j--) {
      if (matrix[i][j] == '0')
          continue;
      var min = dp[i][j];
      for (var k = i; k <m; k++) {
        if (dp[k][j] === '0') 
            break;
        min = Math.min(dp[k][j], min);
        maxArea = Math.max(maxArea, min * (k - i + 1));
      }
    }
  }
  return maxArea;
};
```

See the article discussion here: [JavaScript: DP ( preSum in row ) solution with explanation](https://leetcode.com/problems/maximal-rectangle/discuss/1098613/JavaScript-DP-(-preSum-in-row-)-solution-with-explanation)

<div id="question3" />

### [1504. Count Submatrices With All Ones](https://leetcode.com/problems/count-submatrices-with-all-ones/)
Given a `rows * columns` matrix  `mat`  of ones and zeros, return how many **submatrices**  have all ones.

**Example 1:**
**Input:** mat = [[1,0,1],
              [1,1,0],
              [1,1,0]]
**Output:** 13
**Explanation:** There are **6** rectangles of side 1x1.
There are **2** rectangles of side 1x2.
There are **3** rectangles of side 2x1.
There is **1** rectangle of side 2x2. 
There is **1** rectangle of side 3x1.
Total number of rectangles = 6 + 2 + 3 + 1 + 1 = **13.**

**Analysis:**
- `dp[i][j]:` count of continuous `1s` at `row_i`, from previous columns until current `col_j` .
- How to count and add all to results?

	![image](../../assets/lc85-dp.png ':size=417x179')

	Steps to find max area here:
	- reverse order from column `j = col n to col 1`
	- from current row to last row: `i = row i to row k`, until `dp[k][j]  !== 0 `
	- current Count =`min(dp[i][j], dp[k][j])`
	- `result += current Count;` 

**Javascript solution:**
```js
var numSubmat = function(matrix) {
  if (matrix.length <= 0) 
      return 0;
  const m = matrix.length;
  const n = matrix[0].length;
  const dp = new Array(m).fill(null).map(() => new Array(n).fill(0));
  // 1. build preSum on each row: dp[][]
   for(var i=0; i<m; i++)
      for(var j=0; j<n; j++)
      {
        dp[i][j] =  matrix[i][j]===0 ? 0 : (j===0? matrix[i][0]: dp[i][j-1] + 1);   
      }
    
  // 2. count all from reversed-column order
  var result = 0;
  for (var i = 0; i<m; i++) {
    for (var j = n-1; j >=0 ; j--) {
      if (matrix[i][j] === 0)
          continue;
      var min = dp[i][j];
      for (var k = i; k <m; k++) {
        if (dp[k][j] === 0) 
            break;
        min = Math.min(dp[k][j], min);
        result += min;
      }
    }
  }
  return result;
};
```

See the discussion article here: [\[JavaScript\] DP (preSum in row) with explanation](https://leetcode.com/problems/count-submatrices-with-all-ones/discuss/1098636/JavaScript-DP-%28preSum-in-row%29-with-explanation)



