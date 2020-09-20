## Leetcode contest 207

### 1592. Rearrange Spaces Between Words
You are given a string  `text`  of words that are placed among some number of spaces. Each word consists of one or more lowercase English letters and are separated by at least one space. It's guaranteed that  `text`  **contains at least one word**.

Rearrange the spaces so that there is an  **equal**  number of spaces between every pair of adjacent words and that number is  **maximized**. If you cannot redistribute all the spaces equally, place the  **extra spaces at the end**, meaning the returned string should be the same length as  `text`.

Return  _the string after rearranging the spaces_.

**Example 1:**
**Input:** text = "  this   is  a sentence "
**Output:** "this   is   a   sentence"
**Explanation:** There are a total of 9 spaces and 4 words. We can evenly divide the 9 spaces between the words: 9 / (4-1) = 3 spaces.

**Example 2:**
**Input:** text = " practice   makes   perfect"
**Output:** "practice   makes   perfect "
**Explanation:** There are a total of 7 spaces and 3 words. 7 / (3-1) = 3 spaces plus 1 extra space. We place this extra space at the end of the string.

**Example 5:**
**Input:** text = "a"
**Output:** "a"

**Constraints:**

-   `1 <= text.length <= 100`
-   `text` consists of lowercase English letters and `' '`.
-   `text` contains at least one word.

**Tips:**
* due to special test-case: if no space, just return
        if(space ==0)
            return text;
 * If only one word, still need to arrange the spaces, pay attention to the "zero" when we do **divide operation**.
	 ```
	 int avg =  w == 1 ? 0 : space / (w-1);
	 int remaining = w == 1 ? space : space % (w-1);
	 ```

**Java Solution:**
```
public String reorderSpaces(String text) {
        int n = text.length();
        int space = 0;
        for(int i = 0; i<n; i++)
        {
            if(text.charAt(i) == ' ')
            {
                space++;
            }
        }
        if(space ==0)
            return text;
        text.trim();
        String[] ss = text.split(" ");
        List<String> slist = new ArrayList<>();
        for(String s: ss)
        {
            if(s != null && s.length()>0)
            {
                slist.add(s);
            }
        }
        int w = slist.size();
        int avg =  w == 1 ? 0 : space / (w-1);
        int remaining = w == 1 ? space : space % (w-1);
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i< slist.size(); i++)
        {
            sb.append(slist.get(i));
            if( i == slist.size() -1)
            {
                if(remaining > 0)
                {
                    for(int k = 0; k< remaining; k++)
                    {
                        sb.append(" ");
                    }
                }
            }
            else
            {
                for(int k = 0; k< avg; k++)
                {
                    sb.append(" ");
                }
            }
        }
        return sb.toString();
    }
```

### 1593.  Split a String Into the Max Number of Unique Substrings

Given a string `s`, return  _the maximum number of unique substrings that the given string can be split into_.

You can split string `s`  into any list of **non-empty substrings**, where the concatenation of the substrings forms the original string. However, you must split the substrings such that all of them are  **unique**.

A  **substring**  is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "ababccc"
**Output:** 5
**Explanation**: One way to split maximally is ['a', 'b', 'ab', 'c', 'cc']. Splitting like ['a', 'b', 'a', 'b', 'c', 'cc'] is not valid as you have 'a' and 'b' multiple times.

**Example 2:**
**Input:** s = "aba"
**Output:** 2
**Explanation**: One way to split maximally is ['a', 'ba'].

**Example 3:**
**Input:** s = "aa"
**Output:** 1
**Explanation**: It is impossible to split the string any further.
**Constraints:**

-   `1 <= s.length <= 16`
    
-   `s`  contains only lower case English letters.
    
**Java Solution:**
* Direct DFS solution
*  with hashset() to memo the visited words to keep unique sub-string
```
class Solution {
    int max = 0;
    public int maxUniqueSplit(String s) {
        Set<String> visited = new HashSet<>();
        dfs(s, visited, 0);
        return max;
    }
    public void dfs(String cur, Set<String> visited, int groupCnt)
    {
        if(cur.length() == 0)
        {
            // can split
            max = Math.max(groupCnt, max);
            return;
        }
        for(int i = 0; i< cur.length(); i++)
        {
            String next = cur.substring(0, i+1);
            if(visited.contains(next))
            {
                continue;
            }
            visited.add(next);
            dfs(cur.substring(i+1, cur.length()), visited, groupCnt+1);
            visited.remove(next);
        }
    }
}
```
### 1594.  Maximum Non Negative Product in a Matrix (not solved)

You are given a `rows x cols` matrix `grid`. Initially, you are located at the top-left corner  `(0, 0)`, and in each step, you can only  **move right or down**  in the matrix.

Among all possible paths starting from the top-left corner `(0, 0)` and ending in the bottom-right corner `(rows - 1, cols - 1)`, find the path with the **maximum non-negative product**. The product of a path is the product of all integers in the grid cells visited along the path.

Return the _maximum non-negative product **modulo**_ `109 + 7`. _If the maximum product is  **negative**  return_ `-1`.

**Notice that the modulo is performed after getting the maximum product.**

**Example 1:**
**Input:** grid = [[-1,-2,-3],
               [-2,-3,-3],
               [-3,-3,-2]]
**Output:** -1
**Explanation:** It's not possible to get non-negative product in the path from (0, 0) to (2, 2), so return -1.

**Example 2:**
**Input:** grid = [[**1**,-2,1],
               [**1**,**-2**,1],
               [3,**-4**,**1**]]
**Output:** 8
**Explanation:** Maximum non-negative product is in bold (1 * 1 * -2 * -4 * 1 = 8).

**Wrong Solution: dfs to try all paths**
* because there's only 2 ways to do down or right, not need to try all path
* good at small data size in test cases, but final solution something wrong with larger array size in test-cases

**Correct Solution: DP:  only two ways to get local MIN/MAX**
* remember in array from top to bottom, if only 2 ways to traverse, we can use `dp[][]`  to solve the problem
* how to keep positive or negative value?
	- local min -> if negative product
	- local max -> our target positive final product
* use `long` during each step, only final destination use MOD on the final product if is valid positive result.

Java Solution:
```
 public int maxProductPath(int[][] grid) {
        int mod = 1000000007;
        int m = grid.length;
        int n = grid[0].length;
        long[][] dp1 = new long[m][n]; // min
        long[][] dp2 = new long[m][n]; // max
        //init
        dp1[0][0] = grid[0][0];
         dp2[0][0] = grid[0][0];
        for(int j = 1 ;j < n; j++)
        {
            dp1[0][j] = dp1[0][j-1] * grid[0][j];
            dp2[0][j] = dp2[0][j-1] * grid[0][j];
        }
        for(int i = 1 ;i < m; i++)
        {
            dp1[i][0] = dp1[i-1][0] * grid[i][0];
            dp2[i][0] = dp2[i-1][0] * grid[i][0];
        }
        
        // dp
        for(int i = 1 ;i< m; i++)
            for(int j=1; j<n; j++)
            {
                long min = Long.MAX_VALUE;
                long max = Long.MIN_VALUE;
                long temp = dp1[i - 1][j] * grid[i][j];
                min = Math.min(min , temp);
                max = Math.max(max , temp);

                temp = dp2[i - 1][j] * grid[i][j];
                min = Math.min(min , temp);
                max = Math.max(max , temp);

                temp = dp1[i][j - 1] * grid[i][j];
                min = Math.min(min , temp);
                max = Math.max(max , temp);

                temp = dp2[i][j - 1] * grid[i][j];
                min = Math.min(min , temp);
                max = Math.max(max , temp);

                dp1[i][j] = min;
                dp2[i][j] = max;
        }
        // result
        long res = dp2[m-1][n - 1];
        if (res >= 0) {
            return (int) (res % mod);
        } else {
            return - 1;
        }
}
```

### 1595.  Minimum Cost to Connect Two Groups of Points ( not solved)

You are given two groups of points where the first group has  `size1` points, the second group has  `size2` points, and `size1  >=  size2`.

The  `cost`  of the connection between any two points are given in an `size1  x  size2` matrix where  `cost[i][j]`  is the cost of connecting point  `i`  of the first group and point `j`  of the second group. The groups are connected if  **each point in both groups is connected to one or more points in the opposite group**. In other words, each point in the first group must be connected to at least one point in the second group, and each point in the second group must be connected to at least one point in the first group.

Return _the minimum cost it takes to connect the two groups_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/03/ex1.jpg)

**Input:** cost = [[15, 96], [36, 2]]
**Output:** 17
**Explanation**: The optimal way of connecting the groups is:
1--A
2--B
This results in a total cost of 17.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/03/ex2.jpg)

**Input:** cost = [[1, 3, 5], [4, 1, 1], [1, 5, 3]]
**Output:** 4
**Explanation**: The optimal way of connecting the groups is:
1--A
2--B
2--C
3--A
This results in a total cost of 4.
Note that there are multiple points connected to point 2 in the first group and point A in the second group. This does not matter as there is no limit to the number of points that can be connected. We only care about the minimum total cost.

**Example 3:**
**Input:** cost = [[2, 5, 1], [3, 4, 7], [8, 1, 2], [6, 2, 4], [3, 8, 8]]
**Output:** 10

**Analysis:**
1.  How to pair  group1( m points ) and group2( n points )

| row | | | col |
|--|--|--|--|
|  1|   | | 1|
|  2|   | | 2|
|  3|   | | 3|
|  4|   | | find row min| 
|  5|   | | find row min|

2. when **row == col**
for each column, try to find the min cost for column j
3. when **row > col**
for those unmatched point in row, just directly add the `rowMin` to the final cost sum.
4. how to track paired/matched points in all rows?
	* not use boolean, because same point can be used multiple times
	* use integer count to mark paired or not paired: **used[row] == 0**

**Java Solution:**
```
class Solution {
    int n;
    int m;
    int res;
    int[] rowMin;
    int[] colMin;
    int[] colSum;
    int[] used;
    public int connectTwoGroups(List<List<Integer>> cost) {       
        m = cost.size();
        n = cost.get(0).size();
        rowMin = new int[m];
        colMin = new int[n];
        res = Integer.MAX_VALUE;
        for (int i = 0; i < m; i++) {
            rowMin[i] = Integer.MAX_VALUE;
            for (int j = 0; j < n; j++)
                rowMin[i] = Math.min(rowMin[i], cost.get(i).get(j));
        }
        for (int j = 0; j < n; j++) {
            colMin[j] = Integer.MAX_VALUE;
            for (int i = 0; i < m; i++)
                colMin[j] = Math.min(colMin[j], cost.get(i).get(j));
        }
        colSum = new int[n+1];
        for (int j = n-1; j>=0;j--)
            colSum[j] = colSum[j+1] + colMin[j];
        
        // row used[] array to mark already paired row point
        used = new int[m];
        dfs(cost,0,0);
        return res;
    }
    public void dfs(List<List<Integer>> cost, int j, int sum)
    {
         if(j >= n) {
            for (int i = 0; i < m; i++)
                if (used[i] == 0) 
                    sum += rowMin[i];
            res = Math.min(res, sum);
            return;
        }
        
        // if remaining min col + current > result
        if (sum + colSum[j]>= res) 
            return;
        
        for (int i = 0; i < m; i++) {
            used[i]++;
            dfs(cost, j + 1, sum + cost.get(i).get(j));
            used[i]--;
        }
    }
}
```
  

My rank:
3307 / 13291
Started 20 min late than the beginning time! keep moving next week!
