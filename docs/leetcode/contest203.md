## Leetcode contest 203

### 1560.  Most Visited Sector in a Circular Track

Given an integer  `n`  and an integer array  `rounds`. We have a circular track which consists of  `n`  sectors labeled from  `1`  to  `n`. A marathon will be held on this track, the marathon consists of  `m`  rounds. The  `ith` round starts at sector  `rounds[i - 1]`  and ends at sector  `rounds[i]`. For example, round 1 starts at sector  `rounds[0]`  and ends at sector  `rounds[1]`

Return  _an array of the most visited sectors_  sorted in  **ascending**  order.

Notice that you circulate the track in ascending order of sector numbers in the counter-clockwise direction (See the first example).

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/14/tmp.jpg)

**Input:** n = 4, rounds = [1,3,1,2]
**Output:** [1,2]
**Explanation:** The marathon starts at sector 1. The order of the visited sectors is as follows:
1 --> 2 --> 3 (end of round 1) --> 4 --> 1 (end of round 2) --> 2 (end of round 3 and the marathon)
We can see that both sectors 1 and 2 are visited twice and they are the most visited sectors. Sectors 3 and 4 are visited only once.

#### Solution
it took sometime to understand the circle loop meaning. 
* not cross the loop
	count from sector **rounds[i]** to sector  **rounds[i+1]**, for example from 1 to 3, we need to count: **1,2,3**
* cross the loop
	for example from 2 to 1, we need to count: **2,3,4,1**

My brute-force java solution:
```
 public List<Integer> mostVisited(int n, int[] rounds) {
        int cnt[] = new int[n+1];
        cnt[rounds[0]]++;
        int pre = rounds[0];
        int cur = -1;
        for(int i=1; i<rounds.length; i++)
        {
             cur = rounds[i];
             if(cur < pre)
             {
                  cur += n;
             }
             for(int j = pre+1; j <= cur; j++)
             {
                 int val = j>n ? j%n : j;
                 cnt[val]++;
             }
            pre = rounds[i];
        }    
        // find the max count
        int max = 0;
        List<Integer> res = new ArrayList<>();
        for(int c: cnt)
        {
            max = Math.max(max, c);
        }
        for(int i = 1; i<= n; i++)
        {
            //System.out.println("r="+i + ", cnt="+cnt[i]);
            if(cnt[i] == max)
            {
                res.add(i);
            }
        }
        return res;
    }
```

### 1561.  Maximum Number of Coins You Can Get

There are 3n piles of coins of varying size, you and your friends will take piles of coins as follows:

-   In each step, you will choose  **any** 3 piles of coins (not necessarily consecutive).
-   Of your choice, Alice will pick the pile with the maximum number of coins.
-   You will pick the next pile with maximum number of coins.
-   Your friend Bob will pick the last pile.
-   Repeat until there are no more piles of coins.

Given an array of integers  `piles` where  `piles[i]`  is the number of coins in the  `ith`  pile.

Return the maximum number of coins which you can have.

**Example 1:**

**Input:** piles = [2,4,1,2,7,8]
**Output:** 9
**Explanation:** Choose the triplet (2, 7, 8), Alice Pick the pile with 8 coins, you the pile with **7** coins and Bob the last one.
Choose the triplet (1, 2, 4), Alice Pick the pile with 4 coins, you the pile with **2** coins and Bob the last one.
The maximum number of coins which you can have are: 7 + 2 = 9.
On the other hand if we choose this arrangement (1, **2**, 8), (2, **4**, 7) you only get 2 + 4 = 6

#### My solution
Just find the math rules and calculate directly.
Group should be two largest and one smallest, for example:
* **[n, n-1, 0]**: select n-1
* **[n-2, n-3, 1]**: select n-3
* ....
Then result should be n/3 times to get the middle value.
	```
	public int maxCoins(int[] piles) {
		int len = piles.length;
		int n = len/3;
		Arrays.sort(piles);
		int start = len-2;
	        
		for(int i = 0; i<n; i++)
		{
			res += piles[start];
			start = start-2;
		}
		return res;
	}
	```
### 1562.  Find Latest Group of Size M

Given an array  `arr` that represents a permutation of numbers from  `1` to  `n`. You have a binary string of size `n` that initially has all its bits set to zero.

At each step  `i` (assuming both the binary string and  `arr`  are 1-indexed) from  `1`  to `n`, the bit at position `arr[i]` is set to `1`. You are given an integer `m` and you need to find the latest step at which there exists a group of ones of length `m`. A group of ones is a contiguous substring of 1s such that it cannot be extended in either direction.

Return  _the latest step at which there exists a group of ones of length  **exactly**_ `m`.  _If no such group exists, return_ `-1`.

**Example 1:**
**Input:** arr = [3,5,1,2,4], m = 1
**Output:** 4
**Explanation:** Step 1: "00100", groups: ["1"]
Step 2: "00101", groups: ["1", "1"]
Step 3: "10101", groups: ["1", "1", "1"]
Step 4: "11101", groups: ["111", "1"]
Step 5: "11111", groups: ["11111"]
The latest step at which there exists a group of size 1 is step 4.

#### My solution:
Union find
pay attention to the group and merge the both side, for example:
`["101"]`, and we will fill the middle "0" to "1", we need to merge left, and also merge right.
```
class Solution {
    public int findParent(int[] p ,int x)
    {
        if(p[x] != x)
        {
            p[x] = findParent(p, p[x]);
        }
        return p[x];
    }
    public int findLatestStep(int[] arr, int m) {
        int n = arr.length;
        int[] size = new int[n+1]; // parent's size
        int[] parent = new int[n+2];
        int[] cnt = new int[n+1];
        Arrays.fill(size, 0);
        Arrays.fill(parent, -1);
        Arrays.fill(cnt, 0);
            int res = -1;
        for(int step = 1; step<=n; step++)
        {
            int i = arr[step-1];
            size[i] = 1;
            parent[i] = i;
            cnt[1]++;
            // Debug
            // if(parent[i+1] != -1 && parent[i-1] != -1)
            // {
                // //because already merged one side,
                // // don't need to add 1 again, 
            //  //it will take the new size at i, when at 2nd time merge

            //     cnt[1]++;
            // }
            // System.out.println("before:cnt1: "+cnt[1]);
            if(parent[i+1] != -1)
            {
                int x = findParent(parent, i+1);
                int y= findParent(parent, i);
                cnt[size[x]]--;
                // System.out.println("size[x]="+ size[x]+ ", cnt1:"+cnt[1]);
                cnt[size[y]]--;
                // System.out.println("size[y]="+ size[y]+ ", cnt1:"+cnt[1]);
                parent[y] = x;
                size[x] += size[y];
                cnt[size[x]]++;  
            }
            // System.out.println("cnt1:"+cnt[1]);
            if(parent[i-1] != -1)
            {
                int x = findParent(parent, i-1);
                int y= findParent(parent, i);
                cnt[size[x]]--;
                // System.out.println("size[x]="+ size[x]+ ", cnt1:"+cnt[1]);
                cnt[size[y]]--;
                // System.out.println("size[y]="+ size[y]+ ", cnt1:"+cnt[1]);
                parent[y] = x;
                size[x] += size[y];
                cnt[size[x]]++;  
            }
            // System.out.println("after:cnt1:"+cnt[m]);
            //  System.out.println("----------------------");
            if(cnt[m] > 0)
            {
                
                res = step;
            }   
        }
        return res;
    }
}
```

### 1563.  Stone Game V

There are several stones **arranged in a row**, and each stone has an associated value which is an integer given in the array `stoneValue`.

In each round of the game, Alice divides the row into  **two non-empty rows**  (i.e. left row and right row), then Bob calculates the value of each row which is the sum of the values of all the stones in this row. Bob throws away the row which has the maximum value, and Alice's score increases by the value of the remaining row. If the value of the two rows are equal, Bob lets Alice decide which row will be thrown away. The next round starts with the remaining row.

The game ends when there is only  **one stone remaining**. Alice's is initially  **zero**.

Return  _the maximum score that Alice can obtain_.

**Example 1:**
**Input:** stoneValue = [6,2,3,4,5,5]
**Output:** 18
**Explanation:** In the first round, Alice divides the row to [6,2,3], [4,5,5]. The left row has the value 11 and the right row has value 14. Bob throws away the right row and Alice's score is now 11.
In the second round Alice divides the row to [6], [2,3]. This time Bob throws away the left row and Alice's score becomes 16 (11 + 5).
The last round Alice has only one choice to divide the row which is [2], [3]. Bob throws away the right row and Alice's score is now 18 (16 + 2). The game ends because only one stone is remaining in the row.

**Example 2:**
**Input:** stoneValue = [7,7,7,7,7,7,7]
**Output:** 28

**Example 3:**
**Input:** stoneValue = [4]
**Output:** 0

#### My solution: DP
Split: [0,1,2,3,......n], 
left: exclusive, right: inclusive
**dp(i, k] = dp (i, j] or  dp (j, k]**
```
 public int stoneGameV(int[] stoneValue) {
        int n = stoneValue.length;
        int[] sum = new int[n+1];
        for(int i = 1; i<=n; i++)
        {
            sum[i] += sum[i-1] + stoneValue[i-1];
        }
        int[][] dp = new int[n+1][n+1];
        for(int i = n; i>=0; i--)
            for(int j = i+1; j< n; j++)
                for(int k = j+1; k<=n; k++)
                {
                    // split at [i, j], include j
                    int L = sum[j] - sum[i];
                    int R = sum[k] - sum[j];
                    if (L <= R) 
                        dp[i][k] = Math.max(dp[i][k], dp[i][j] + L);
                    if (L >= R) 
                         dp[i][k] = Math.max(dp[i][k], dp[j][k] + R);
              
                    // System.out.println("L="+L +", R="+R);
                    // System.out.println("dp["+i+"]["+k+"] = " + dp[i][k]);
                }
        return dp[0][n];
    }
```