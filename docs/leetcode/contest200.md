
## Leetcode Contest 200

### 1534.  Count Good Triplets
Given an array of integers  `arr`, and three integers `a`, `b` and `c`. You need to find the number of good triplets.
A triplet  `(arr[i], arr[j], arr[k])` is  **good**  if the following conditions are true:
-   `0 <= i < j < k < arr.length`
-   `|arr[i] - arr[j]| <= a`
-   `|arr[j] - arr[k]| <= b`
-   `|arr[i] - arr[k]| <= c`

Where  `|x|`  denotes the absolute value of  `x`.
Return _the number of good triplets_.

My solution
```
public int countGoodTriplets(int[] arr, int a, int b, int c) {
        int cnt = 0;
        int n = arr.length;
        for(int i = 0; i<n;i++)
            for(int j = i +1; j<n;j++)
            {
                if(Math.abs(arr[i] -arr[j]) > a )
                    continue;
                for(int k = j+1; k< n; k++)
                {
                   if( Math.abs(arr[j] -arr[k]) > b )
                       continue;
                    if( Math.abs(arr[i] -arr[k]) > c)
                        continue;
                    cnt++;

                }
            }
        return cnt; 
    }
```


### 1535.  Find the Winner of an Array Game

Given an integer array  `arr`  of  **distinct**  integers and an integer  `k`.

A game will be played between the first two elements of the array (i.e.  `arr[0]`  and  `arr[1]`). In each round of the game, we compare  `arr[0]`  with  `arr[1]`, the larger integer wins and remains at position  `0`  and the smaller integer moves to the end of the array. The game ends when an integer wins  `k`  consecutive rounds.
Return  _the integer which will win the game_.
It is  **guaranteed**  that there will be a winner of the game.

**Example 1:**
**Input:** arr = [2,1,3,5,4,6,7], k = 2
**Output:** 5
**Explanation:** Let's see the rounds of the game:
Round |       arr       | winner | win_count
  1   | [2,1,3,5,4,6,7] | 2      | 1
  2   | [2,3,5,4,6,7,1] | 3      | 1
  3   | [3,5,4,6,7,1,2] | 5      | 1
  4   | [5,4,6,7,1,2,3] | 5      | 2
So we can see that 4 rounds will be played and 5 is the winner because it wins 2 consecutive games.

* Idea:
   1. find the roles and compare each index with the numbers after it;
   2. if k >> max_length, return the max number in whole array
 * My java solution:
	 ```
	 public int getWinner(int[] arr, int k) {
	        int n = arr.length;
	        int cur = arr[0];
	        int cnt = 0;
	        for(int i = 1; i<n; i++)
	        {
	            if(cur > arr[i])
	            {
	                cnt++;
	            }
	            else
	            {
	                cnt = 1;
	                cur = arr[i];
	            }
	            if(cnt >= k )
	            {
	                return cur;
	            }
	        }
	        return cur; 
	    }
	}
	 ```

### 1536.  Minimum Swaps to Arrange a Binary Grid

Given an  `n x n` binary  `grid`, in one step you can choose two  **adjacent rows**  of the grid and swap them.
A grid is said to be  **valid**  if all the cells above the main diagonal are  **zeros**.
Return  _the minimum number of steps_  needed to make the grid valid, or  **-1**  if the grid cannot be valid.

The main diagonal of a grid is the diagonal that starts at cell  `(1, 1)`  and ends at cell  `(n, n)`.

**Example 1:**
![](https://assets.leetcode.com/uploads/2020/07/28/fw.jpg)

**Input:** grid = [[0,0,1],[1,1,0],[1,0,0]]
**Output:** 3


* Idea:
1. count the ending zeros  for each row
2. find the condition for valid count of zeros above diagonal: `count >= n-1-row`
3. how to swap to find the min swap steps?
	- Wrong way: target order is the descending order, but this might not be minimum. Waste a lot of time to solve the **wrong** problem: **to find the min swap from original array to match the target array**. 
	- Correct: from the first one, in order to `find the next closest swap` !

* My java solution after contest:
```
class Solution {
    public int minSwaps(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int cur[] = new int[m];
        for(int i = 0; i < m; i++)
        {
            int cnt  = 0;
            for(int j = n-1; j>=0; j--)
            {
                if(grid[i][j] == 0)
                {
                    cnt++;
                }
                else
                {
                    break;
                }

            }
            cur[i] = cnt;
        }
        
        // swap
        int res = 0;
         for(int i = 0; i<m; i++)
         {
             int j = i;
             while( j<m &&cur[j] < m-i-1 )
             {
                 j++;
             }
             if(j == n)
             {
                return  -1;
             }
             // swap with i -> j
             for(int k = j ; k>=i+1; k--)
             {
                 swap(cur, k-1, k);
                 res++;
             }
                 
         }

        return  res;
    } 
    public void swap (int[] a, int i, int j)
    {
        int tmp = a[j];
        a[j] = a[i];
        a[i] = tmp;
    }
} 
```

### 1537.  Get the Maximum Score

You are given two  **sorted**  arrays of distinct integers  `nums1`  and  `nums2.`
A  **valid  path**  is defined as follows:

-   Choose array nums1 or nums2 to traverse (from index-0).
-   Traverse the current array from left to right.
-   If you are reading any value that is present in  `nums1`  and  `nums2` you are allowed to change your path to the other array. (Only one repeated value is considered in the valid path).

_Score_  is defined as the sum of uniques values in a valid path.

Return the maximum  _score_  you can obtain of all possible **valid paths**.

Since the answer may be too large, return it modulo 10^9 + 7.

My solution after contest:
```
    public int maxSum(int[] nums1, int[] nums2) {
        int m = 10000001;
        int mod = 1000000007;
	    int[] a = new int[m];
	    int[] b = new int[m];
	    for(int num : nums1)
        {
                a[num] = num;
        }
	    for(int num : nums2)
        {
                b[num] = num;
        }
        
	    long sum1 = 0, sum2 = 0;
	    for(int i = 1; i < m;i++){
	        	if(a[i] > 0 )
                {
                    sum1 += a[i];
                }
	        	if(b[i] > 0)
                {
                    sum2 += b[i];
                }
	        	if(a[i]>0 && b[i]>0)
                {
	        		sum1 = Math.max(sum1, sum2);
	        		sum2 = sum1;
	        	}
	     }
	     int res = (int)(Math.max(sum1, sum2)%mod);
	     return res;
    }
```