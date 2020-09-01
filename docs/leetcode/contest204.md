## Leetcode Contest 204

Whoops! make up for this contest problems!
### 1566.  Detect Pattern of Length M Repeated K or More Times

Given an array of positive integers  `arr`, find a pattern of length  `m`  that is repeated  `k`  or more times.

A  **pattern**  is a subarray (consecutive sub-sequence) that consists of one or more values, repeated multiple times  **consecutively** without overlapping. A pattern is defined by its length and the number of repetitions.

Return `true` _if there exists a pattern of length_ `m` _that is repeated_ `k` _or more times, otherwise return_ `false`.

**Example 1:**
**Input:** arr = [1,2,4,4,4,4], m = 1, k = 3
**Output:** true
**Explanation:** The pattern **(4)** of length 1 is repeated 4 consecutive times. Notice that pattern can be repeated k or more times but not less.

**Example 2:**
**Input:** arr = [1,2,1,2,1,1,1,3], m = 2, k = 2
**Output:** true
**Explanation:** The pattern **(1,2)** of length 2 is repeated 2 consecutive times. Another valid pattern **(2,1) is** also repeated 2 times.

**Solution:**
1. don't brute force to compare every m*k times at every location
2. use a smarter O(n) to count position `i` and position `i+m`
3. if we can have a total count pairs of m * (k-1), because we use previous one group to start comparing. 
```
public boolean containsPattern(int[] arr, int m, int k) {
    int n = arr.length;
    if(n<m*k)
    {
        return false;
    }
    int cnt = 0;
	for(int i =0 ;i < n - m ; i++)
	{
        if(arr[i] == arr[i+m])
        {
           cnt++;
		}
	    else
        {
           cnt = 0;
        } 
        if(cnt == m*(k-1))
		{
			return true;
		}
	}
	return false;
}
```

### 1567.  Maximum Length of Subarray With Positive Product

[My Submissions](https://leetcode.com/contest/weekly-contest-204/problems/maximum-length-of-subarray-with-positive-product/submissions/)[Back to Contest](https://leetcode.com/contest/weekly-contest-204/)

-   **User Accepted:**2576
-   **User Tried:**4283
-   **Total Accepted:**2650
-   **Total Submissions:**10719
-   **Difficulty:**Medium

Given an array of integers `nums`, find the maximum length of a subarray where the product of all its elements is positive.

A subarray of an array is a consecutive sequence of zero or more values taken out of that array.

Return _the maximum length of a subarray with positive product_.

**Example 1:**
**Input:** nums = [1,-2,-3,4]
**Output:** 4
**Explanation:** The array nums already has a positive product of 24.

**Example 2:**
**Input:** nums = [0,1,-2,-3,-4]
**Output:** 3
**Explanation:** The longest subarray with positive product is [1,-2,-3] which has a product of 6.
Notice that we cannot include 0 in the subarray since that'll make the product 0 which is not positive.

**Solution:**
1. how to keep the latest positive and negative max-Length?
2. Eg: **'1,1,-1,1'**, at last index `i=3`, for these 4 numbers, negative-length = 4, positive-length = 1.
3. why?
	- when current `num > 0`: positive-length++, also if there is negative-length>0, means even product previous * current_num < 0, then negative-length++;
	- when current  `num < 0`: 
	eg: **'1,1,-1'**, at  index `i=2`, negative-length = prev_positive + 1= 3, current positive-length = 0.
	when it comes to **'1,1,-1,-1'**: at index `i=3`, negative-length = pre_positive + 1 = 0+1 = 1, but current positive-length =  prev_negative + 1 = 3 + 1 = 4.
	- when current `num = 0`, it will be a blocker, negativa & positive all goes back to 0.

4. summary: don't care about the even number of negative, we always keep track the current position the number of `positive-length and negative-length`.  
* `int neg` = until current index, the max length of negative result
* `int pos` = until current index, the max length of positive result

```
 public int getMaxLen(int[] nums) {
        int n = nums.length;
        int pos = 0;
        int neg = 0;
        int max = 0;
        for(int i = 1; i<= n; i++)
        {
            if(nums[i-1] > 0)
            {
                pos++;
                if(neg > 0)
                {
                    neg++;
                }
            }
            else if(nums[i-1] < 0)
            {
                int tmp = pos;
                if(neg > 0)
                {
                    pos = neg+1;
                }
                else
                {
                    pos = 0;
                }
                // key point 
                  neg = tmp + 1;     
            }
            else
            {
                neg = 0;
                pos = 0;
            }
            max = Math.max(max, pos);
        }     
        return max;  
}
```

### 1568.  Minimum Number of Days to Disconnect Island
Given a 2D `grid`  consisting of  `1`s (land) and  `0`s (water). An  _island_  is a maximal 4-directionally (horizontal or vertical) connected group of  `1`s.

The grid is said to be  **connected**  if we have  **exactly one island**, otherwise is said  **disconnected**.

In one day, we are allowed to change  **any** single land cell  `(1)`  into a water cell  `(0)`.

Return  _the minimum number of days_  to disconnect the grid.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/08/13/1926_island.png)**

**Input:** grid = [[0,1,1,0],[0,1,1,0],[0,0,0,0]]
**Output:** 2
**Explanation:** We need at least 2 days to get a disconnected grid.
Change land grid[1][1] and grid[0][2] to water and get 2 disconnected island.

**Example 2:**

**Input:** grid = [[1,1]]
**Output:** 2
**Explanation:** Grid of full water is also disconnected ([[1,1]] -> [[0,0]]), 0 islands.

**Solution:**
1. most important! the result will only be 0, 1, and 2.
2. why the max number is at most 2? Because every array will definitely have a corner for let us to change to 2 zero to separate. 
Eg: the top right corner
  1,1,0,1
  1,1,1,0
  1,1,1,1
  1,1,1,1
3. algorithm:
* check is already valid in disconnected status, return 0
* try to remove every "1" to see if there is a "1" way solution
* return 2 at last

```
class Solution {
    int m;
    int n;
    public int minDays(int[][] grid) {
        /*
            1,1,0,1
            1,1,1,0
            1,1,1,1
            1,1,1,1
            min is always 2 for any array, it has a corner to separate.
        */
        m = grid.length;
        n = grid[0].length;
        if(isDisconnected(grid))
        {
            return 0;
        }
        // check can be only 1 change
         for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++)
            {
                if(grid[i][j] == 1)
                {
                    grid[i][j] = 0;
                    if(isDisconnected(grid))
                    {
                        return 1;
                    }
                    grid[i][j] = 1;
                }
            }  
        return 2;
    }
    public boolean isDisconnected(int[][] grid)
    {
       
         int m = grid.length;
         int n = grid[0].length;
         boolean[][] visited = new boolean[m][n];
         int cnt = 0;
         for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++)
            {
                if(grid[i][j] == 1 && !visited[i][j])
                {
                   
                    dfs(grid, visited, i, j);
                    if(++cnt > 1)
                    {
                        return true;
                    }
                }
            }
         return cnt == 0 ? true: false;
    }
    int[][] dir = {{1,0},{0,1},{-1,0},{0,-1}};
    public void  dfs(int[][] grid, boolean[][] visited, int i, int j)
    {
        visited[i][j] = true;
        for(int[] d: dir)
        {
            int x = i + d[0];
            int y = j + d[1];
            if(x >=0 && x< m && y>=0 && y<n && grid[x][y] == 1 && !visited[x][y])
            {
                dfs(grid, visited, x, y);
            }
        }
    }
}
```
### 1569.  Number of Ways to Reorder Array to Get Same BST
Given an array  `nums` that represents a permutation of integers from `1` to `n`. We are going to construct a binary search tree (BST) by inserting the elements of `nums` in order into an initially empty BST. Find the number of different ways to reorder  `nums`  so that the constructed BST is identical to that formed from the original array `nums`.

For example, given `nums = [2,1,3]`, we will have 2 as the root, 1 as a left child, and 3 as a right child. The array `[2,3,1]` also yields the same BST but `[3,2,1]` yields a different BST.

Return  _the number of ways to reorder_ `nums` _such that the BST formed is identical to the original BST formed from_ `nums`.

Since the answer may be very large, **return it modulo** `10^9 + 7`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/12/bb.png)

**Input:** nums = [2,1,3]
**Output:** 1
**Explanation:** We can reorder nums to be [2,3,1] which will yield the same BST. There are no other ways to reorder nums which will yield the same BST.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2020/08/12/ex1.png)**

**Input:** nums = [3,4,5,1,2]
**Output:** 5
**Explanation:** The following 5 arrays will yield the same BST: 
[3,1,2,4,5]
[3,1,4,2,5]
[3,1,4,5,2]
[3,4,1,2,5]
[3,4,1,5,2]

**Solution**:
1. use recursive count
[root, left, right] = numbers of ( select **left positions** in total **(length-1)** positions) or ( select **right positions** in total **(length-1)** positions).
2. equation to get combination result: `C(n, k) = n! / k! (n-k)!` 
3. Recursion:  [root, left, right]  reuslt = **combination(n, left)** * **recursion(left array)** * **recursion(right array)** 

Wrong solution**! frustrating me a lot of times to handle the overflow!!! 
Because I use `long` and the test-case seems overflow the long.MAX_VALUE. 
I got the failure: obviously, it caused by overflow mod value.

![image](../assets/lc1569.png ':size=774x232')
```
class Solution {
    long mod = 1000000007;
    long fact[];
    public int numOfWays(int[] nums) {
        int n = nums.length;
        fact = new long[n+1];
        fact[0] = 1;
        for(int i = 1; i<=n; i++)
        {
            fact[i] = fact[i-1] * i;
        }

        return  (int)((ways(nums) -1 + mod) % mod);
    } 
    public long ways(int nums[])
    {
        if(nums.length<=2)
        {
            return 1;
        }
        List<Integer> left = new ArrayList<>();
        List<Integer> right = new ArrayList<>();
        for(int i = 1; i < nums.length; i++)
        {
            if(nums[i] < nums[0])
            {
                left.add(nums[i]);
            }
            else // no equal condition
            {
                 right.add(nums[i]);
            }
        }
        int l[] = new int[left.size()];
        for(int i = 0; i< left.size(); i++)
        {
            l[i] = left.get(i);
        }
        int r[] = new int[right.size()];
        for(int i = 0; i< right.size(); i++)
        {
            r[i] = right.get(i);
        }
        long lres = ways(l);
        long rres = ways(r);
        long comb =  combination(nums.length-1, left.size());
        System.out.println(lres+", " + rres + ", " + comb);
        long res = (comb * lres  % mod) * rres % mod;
        return res;
    }
    
    public long combination(int n , int l)
    {
         // select l from n items
        long res = (fact[n] / fact[l]  / fact[n-l] )% mod ;
         // System.out.println(res);
        return res;
    }
}
```
Thanks to reference another solution in disscussion for this problem:
[Leetcode Discuss - java accpeted solution](https://leetcode.com/problems/number-of-ways-to-reorder-array-to-get-same-bst/discuss/819448/Java-Accepted-Java-solution)
**BigInteger that solved!**
**import java.math.BigInteger;**
Revised solution to replace **long** with **BigInteger**:
it passed all the test case!
```
import java.math.BigInteger;
class Solution {
    long mod = 1000000007;
    BigInteger fact[];
    public int numOfWays(int[] nums) {
        int n = nums.length;
        fact = new BigInteger[n+1];
        fact[0] = BigInteger.ONE;
        for(int i = 1; i<=n; i++)
        {
            fact[i] = fact[i-1].multiply(BigInteger.valueOf(i));
        }
        return  (int)((ways(nums).longValue() -1 + mod) % mod);
    }
    
    public BigInteger ways(int nums[])
    {
        if(nums.length<=2)
        {
            return BigInteger.ONE;
        }
        List<Integer> left = new ArrayList<>();
        List<Integer> right = new ArrayList<>();
        for(int i = 1; i < nums.length; i++)
        {
            if(nums[i] < nums[0])
            {
                left.add(nums[i]);
            }
            else // no equal condition
            {
                 right.add(nums[i]);
            }
        }
        int l[] = new int[left.size()];
        for(int i = 0; i< left.size(); i++)
        {
            l[i] = left.get(i);
        }
        int r[] = new int[right.size()];
        for(int i = 0; i< right.size(); i++)
        {
            r[i] = right.get(i);
        }
        BigInteger lres = ways(l);
        BigInteger rres = ways(r);
        BigInteger comb =  combination(nums.length-1, left.size());
     
     BigInteger res = comb.multiply(lres).multiply(rres).mod(BigInteger.valueOf(mod));
        return res;
    }
    
    public BigInteger combination(int n , int l)
    {
         // select l from n items
        BigInteger res = fact[n].divide(fact[l]).divide(fact[n-l] ).mod(BigInteger.valueOf(mod));
        return res;
    }
}
```
