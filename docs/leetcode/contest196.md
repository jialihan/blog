### Leetcode Contest 196

### 1502. Can Make Arithmetic Progression From Sequence
Given an array of numbers  `arr`. A sequence of numbers is called an arithmetic progression if the difference between any two consecutive elements is the same.

Return  `true` if the array can be rearranged to form an arithmetic progression, otherwise, return  `false`.

**Example 1:**

**Input:** arr = [3,5,1]
**Output:** true
**Explanation:** We can reorder the elements as [1,3,5] or [5,3,1] with differences 2 and -2 respectively, between each consecutive elements.

**Example 2:**

**Input:** arr = [1,2,4]
**Output:** false
**Explanation:** There is no way to reorder the elements to obtain an arithmetic progression.

**Constraints:**

-   `2 <= arr.length <= 1000`
-   `-10^6 <= arr[i] <= 10^6`

#### My solution
```
public boolean canMakeArithmeticProgression(int[] arr) {
        Arrays.sort(arr);
        int step = 0;
        for(int i = 1; i<arr.length; i++)
        {
            if( i == 1)
            {
                step = arr[i] - arr[i-1];
            }
            else
            {
                if(arr[i] - arr[i-1] != step)
                    return false;
            }
        }
        return true;
        
    }
}
```
### 1503. Last Moment Before All Ants Fall Out of a Plank
We have a wooden plank of the length  `n`  **units**. Some ants are walking on the plank, each ant moves with speed  **1 unit per second**. Some of the ants move to the  **left**, the other move to the  **right**.

When two ants moving in two  **different**  directions meet at some point, they change their directions and continue moving again. Assume changing directions doesn't take any additional time.

When an ant reaches  **one end**  of the plank at a time  `t`, it falls out of the plank imediately.

Given an integer  `n`  and two integer arrays  `left`  and  `right`, the positions of the ants moving to the left and the right. Return  _the moment_  when the last ant(s) fall out of the plank.

**Example 1**
**Input:** n = 4, left = [4,3], right = [0,1]
**Output:** 4
**Explanation:** In the image above:
-The ant at index 0 is named A and going to the right.
-The ant at index 1 is named B and going to the right.
-The ant at index 3 is named C and going to the left.
-The ant at index 4 is named D and going to the left.
Note that the last moment when an ant was on the plank is t = 4 second, after that it falls imediately out of the plank. (i.e. We can say that at t = 4.0000000001, there is no ants on the plank).

**Example 2**
**Input:** n = 7, left = [], right = [0,1,2,3,4,5,6,7]
**Output:** 7
**Explanation:** All ants are going to the right, the ant at index 0 needs 7 seconds to fall.

#### My solution
Tricky part is that after analyze the graph and sequence about the movement, since after collision, two item reverse their direction, then it means `two item swapped`, but nothing affected in our data structure. 
So we know that our direction arrays `left[]` and `right[]` will always keep the same no matter what collision happens.

Goal: to find the max distance from endpoints to original position.
```
public int getLastMoment(int n, int[] left, int[] right) {
    int res = 0;
    for(int l : left)
    {
        res = Math.max(l, res);
    }
    for(int r : right)
    {
        res = Math.max(n-r, res);
    }
    return res; 
}
```
### 

### 1504.  Count Submatrices With All Ones

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

**Example 2:**
**Input:** mat = [[0,1,1,0],
              [0,1,1,1],
              [1,1,1,0]]
**Output:** 24
**Explanation:**
There are **8** rectangles of side 1x1.
There are **5** rectangles of side 1x2.
There are **2** rectangles of side 1x3. 
There are **4** rectangles of side 2x1.
There are **2** rectangles of side 2x2. 
There are **2** rectangles of side 3x1. 
There is **1** rectangle of side 3x2. 
Total number of rectangles = 8 + 5 + 2 + 4 + 2 + 2 + 1 = 24**.**

**Example 3:**
**Input:** mat = [[1,1,1,1,1,1]]
**Output:** 21

**Example 4:**
**Input:** mat = [[1,0,1],[0,1,0],[1,0,1]]
**Output:** 5

#### My solution
1. build row sum, to calculate consecutive ones, if current cell is 0, it will block.
Example for one row:
   row | 1  1  0  1  1
   sum | 1  2  0  1  2
 2. After build m rows for sum[m], we need to count from each row with all rows under it. When we loop at position (i,j), we have to calculate two parts of rectangles.
	 * Current row, only 1 unit of height formed from current row. The value is already stored in our processed rowSum array, because we loop column from `j: (n-1) to 0`, then the value is ordered by desc in default.
	 * larger rectangles formed with `(i+1) to (m-1) rows` with height is more than 2 units.
 ![image](../assets/rect.png ':size=536x192')
3. look at a whole, when we process the cell  `[0, n-1]` as starting position, what's the calculation here. Remember `0` cell will block in row and column.

![image](../assets/wholeloop.png ':size=414x209')

4. complete code in java:
	```
	 public int numSubmat(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;
        int dp[][] = new int[m+1][n+1];
        
        // process row sum
        for(int i = 1; i<= m; i++)
        {
            for(int j = 1;j<=n; j++)
            {
                if(mat[i-1][j-1] == 1)
                {
                    dp[i][j] = dp[i][j-1] + 1;
                }
                else
                {
                    dp[i][j] = 0;
                }
            }
        }
 
        // calculate from top-right to bottom-left
        int res = 0;
        for (int i = 0; i < m; i++)
            for (int j = n-1; j >=0; j--)
            {
                int max = 155;
                for(int k=i; k<m; k++)
                {
                    if(dp[k+1][j+1] > 0)
                    {
                        max = Math.min(max, dp[k+1][j+1]);
                        res += max;
                    }
                    else
                    {
                        break;
                    }
                }
            }
       return res;  
    }
	```

### 1505.  Minimum Possible Integer After at Most K Adjacent Swaps On Digits
Given a string  `num`  representing  **the digits**  of a very large integer and an integer  `k`.
You are allowed to swap any two adjacent digits of the integer  **at most**  `k`  times.
Return  _the minimum integer_  you can obtain also as a string.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/06/17/q4_1.jpg)

**Input:** num = "4321", k = 4
**Output:** "1342"
**Explanation:** The steps to obtain the minimum integer from 4321 with 4 adjacent swaps are shown.

**Example 2:**
**Input:** num = "100", k = 1
**Output:** "010"
**Explanation:** It's ok for the output to have leading zeros, but the input is guaranteed not to have any leading zeros.

**Example 3:**
**Input:** num = "36789", k = 1000
**Output:** "36789"
**Explanation:** We can keep the number without any swaps.

**Example 4:**
**Input:** num = "22", k = 22
**Output:** "22"

#### My solution
1. Within k  times,  what is the next smallest value we can swap in each step?
Think about when `k=1`, we only have one step, then what is the next smallest value we can find?
2. always try to replace the higher bit value at first, if we can replace highest bit value with the minimum value we can find, that will decrease the most!

![image](../assets/swapbit.png ':size=289x210')
 
 3. Terminate condition
 * either we reach K times 
 * or we already swapped for all bits from highest bit to lowest bit.
 4.  My java solution
 ```
 public String minInteger(String num, int k) {
        int n=num.length();
        char s[] = num.toCharArray();
        
        for(int i=0; i<n-1 && k>0; i++)
        {
            int pos=i;
            for(int j=i+1; j<n && j-i<=k ; j++)
            {
               
                if(s[j ]< s[pos])
                {
                    pos=j;
                }
            }
            // swap 
            swap(s, i, pos);
            
            k -= (pos-i);
        }
        return new String(s);
    }
    public void swap(char[] s, int i, int pos)
    {
        char t;
        for(int j = pos; j>i; j--)
        {
            t = s[j];
            s[j] = s[j-1];
            s[j-1] = t;
        }  
    }
 ```