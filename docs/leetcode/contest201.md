## Leetcode Contest 201

### 1544.  Make The String Great

Given a string  `s`  of lower and upper case English letters.

A good string is a string which doesn't have **two adjacent characters**  `s[i]`  and  `s[i + 1]`  where:

-   `0 <= i <= s.length - 2`
-   `s[i]`  is a lower-case letter and  `s[i + 1]`  is the same letter but in upper-case or  **vice-versa**.

To make the string good, you can choose  **two adjacent**  characters that make the string bad and remove them. You can keep doing this until the string becomes good.

Return  _the string_  after making it good. The answer is guaranteed to be unique under the given constraints.

**Notice**  that an empty string is also good.

```
 public String makeGood(String s) {
        StringBuilder sb = new StringBuilder(s);
        int i = 0;
        while(sb.length() > 0 && i<sb.length()-1)
        {
                if(Math.abs(sb.charAt(i)-sb.charAt(i+1)) == 32)
                {
                    sb.delete(i, i+2);
                    i = 0;
                }
                 else
                 {
                     i++;
                 }
        }
        return sb.toString();      
}
```

### 1545.  Find Kth Bit in Nth Binary String
Given two positive integers `n` and  `k`, the binary string `Sn` is formed as follows:

-   `S1 = "0"`
-   `Si = Si-1 + "1" + reverse(invert(Si-1))` for `i > 1`

Where `+` denotes the concatenation operation, `reverse(x)` returns the reversed string  x, and `invert(x)` inverts all the bits in  x  (0 changes to 1 and 1 changes to 0).

For example, the first 4 strings in the above sequence are:

-   `S1 = "0"`
-   `S2 = "0**1**1"`
-   `S3 = "011**1**001"`
-   `S4  = "0111001**1**0110001"`

Return  _the_  `kth`  _bit_  _in_ `Sn`. It is guaranteed that `k` is valid for the given `n`.

**Example 1:**
**Input:** n = 3, k = 1
**Output:** "0"
**Explanation:** S3 is "**0**111001". The first bit is "0".

**My Idea:**
* it shouldn't use brute-force to create the `n_th` string and then find the char at `kth bit`.
* Then we have to find the hidden rules, to reduce the time and step to revert to previous state `(n-1) th`
* Then we can find:
	```
	if index == len/2
		always is "1";
	if index < len/2
	    find the 'index' in (n-1)th string
	if index > len/2
	{
		revert between '0' and '1';
		find the '(len - 1- index)' in (n -1)th string
	}
	```
* Use a boolean flag to refer the final toggle state of '0' and '1'

**My solution: **
```
class Solution {
    boolean flag = true; // inorder, not reverse
    public char findKthBit(int n, int k) {
     
        char res = helper(n, k-1);
        if(flag == false)
        {
            res =  res=='1' ? '0' : '1';
        }

        return res;
    }
    
    public char helper(int n , int i)
    {
        
        int len = (int)Math.pow(2, n) - 1;
        if(i == 0)
        {
            return '0';
        }
        else if(i == len / 2)
        {

            return '1';
        }
        else if(i < len / 2)
        {
            
            return helper(n-1, i);
        }
        else
        {
            // 
            flag = !flag;
            return helper(n-1, len - 1- i);
        }
    }
}
```

### 1546.  Maximum Number of Non-Overlapping Subarrays With Sum Equals Target

Given an array  `nums`  and an integer  `target`.

Return the maximum number of  **non-empty** **non-overlapping**  subarrays such that the sum of values in each subarray is equal to  `target`.

**Example 1:**
**Input:** nums = [1,1,1,1,1], target = 2
**Output:** 2
**Explanation:** There are 2 non-overlapping subarrays [**1,1**,1,**1,1**] with sum equals to targe
#### Idea1:   dp[n] + bruteforce:  TIMEOUT(wrong)
```
 int res;
 public int maxNonOverlapping(int[] nums, int target) {
        res = 0;
        int n = nums.length;
        int[]sum = new int[n+1];
        for(int i = 1; i<=n;i++)
        {
            sum[i] = sum[i-1] + nums[i-1];
        }
        int[] dp = new int[n+1];
        
        // O(n^2) time out
        for(int i = 1; i<=n ; i++)
	        for(int j = i; j>=1; j--)
	        {
            int cur = sum[i] - sum[j-1];
            
            dp[i] = Math.max(dp[i], dp[j-1] + (cur == target? 1: 0));
            res = Math.max(res, dp[i]);
        }  
        return res;
    }
```
#### Idea2:   dp[n] + hashmap_memo (correct)
* use hashmap to store `<Sum, Index>`
*  use sum array to store the accumulate sum: `sum[n]`
* O(n) scan once with memo, similar like two sum problem.

My Solution:
```
class Solution {
    int res;
    public int maxNonOverlapping(int[] nums, int target) {
       
        int n = nums.length;
        Map<Integer,Integer> map = new HashMap<>(); // sum, index
        map.put(0,0);
        int[]sum = new int[n+1];
        int[] dp = new int[n+1];
        for(int i = 1; i<=n ; i++)
        {
            sum[i] = sum[i-1] + nums[i-1];
            dp[i] = dp[i-1];
            if (map.containsKey(sum[i]-target))
            {
                int pos = map.get(sum[i]-target);
                dp[i] = Math.max(dp[i], dp[pos]+1);
            }
            map.put(sum[i],i);
        }   
        return dp[n];     
    }
}
```

### 1547.  Minimum Cost to Cut a Stick
Given a wooden stick of length  `n`  units. The stick is labelled from  `0`  to  `n`. For example, a stick of length  **6**  is labelled as follows:

![](https://assets.leetcode.com/uploads/2020/07/21/statement.jpg)

Given an integer array  `cuts` where  `cuts[i]` denotes a position you should perform a cut at.

You should perform the cuts in order, you can change the order of the cuts as you wish.

The cost of one cut is the length of the stick to be cut, the total cost is the sum of costs of all cuts. When you cut a stick, it will be split into two smaller sticks (i.e. the sum of their lengths is the length of the stick before the cut). Please refer to the first example for a better explanation.

Return  _the minimum total cost_  of the cuts.

**Solution after the contest**
* Sort by interval length
* dp[i][j]: means the min cost to finish the `interval[i] to interval[j]` 
* pick an medium cut: the state equation is:
	```
	dp[i][j] = Min(dp[i][j], length[i][j] + dp[i][k] + dp[k+1][j] )
	```
Final Java Solution:
```
 public int minCost(int n, int[] cuts) {
                Arrays.sort(cuts);
        int m = cuts.length+1;
                Arrays.sort(cuts);
        int[]  a = new int[m];
        a[0] = cuts[0];
        for (int i = 1; i < m-1; ++i)
            a[i] = cuts[i]-cuts[i-1];
        
        a[m-1] = n - cuts[cuts.length-1];
        int[] pre = new int[m+1];
        for (int i = 1; i <= m; ++i) pre[i] = pre[i-1] + a[i-1];
        
        int[][] dp = new int[m][m];
        for (int i = m-1; i>=0; i--) {
            for (int j = i; j < m; j++) {
               
                if (j== i)
                {
                    dp[i][i] = 0;
                    continue;
                }
                 dp[i][j] = 2 * (int)Math.pow(10,9);
                for (int k = i; k < j; ++k) {
                    dp[i][j] = Math.min(dp[i][j], pre[j + 1] - pre[i] + dp[i][k] + dp[k+1][j]);
                }
            }
        }
        return dp[0][m-1];
    }
}
```



