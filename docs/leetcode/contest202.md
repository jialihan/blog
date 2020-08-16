## Leetcode Contest 202
### 1550.  Three Consecutive Odds

Given an integer array  `arr`, return  `true` if there are three consecutive odd numbers in the array. Otherwise, return `false`.

**Example 1:**

**Input:** arr = [2,6,4,1]
**Output:** false
**Explanation:** There are no three consecutive odds.

**Example 2:**

**Input:** arr = [1,2,34,3,4,5,7,23,12]
**Output:** true
**Explanation:** [5,7,23] are three consecutive odds.

 - [ ] My Java Solution
```
  public boolean threeConsecutiveOdds(int[] arr) {
	int n = arr.length;
    for(int i =0; i<n-2; i++)
    {
        if(arr[i] % 2 == 1 &&arr[i+1] % 2 == 1 && arr[i+2] % 2 == 1)
        {
            return true;
        }
    }
    return false;
}
```

### 1551.  Minimum Operations to Make Array Equal

You have an array  `arr`  of length  `n`  where  `arr[i] = (2 * i) + 1`  for all valid values of  `i`  (i.e.  `0 <= i < n`).

In one operation, you can select two indices  `x` and  `y`  where  `0 <= x, y < n`  and subtract  `1`  from  `arr[x]`  and add  `1`  to  `arr[y]` (i.e. perform  `arr[x] -=1` and  `arr[y] += 1`). The goal is to make all the elements of the array  **equal**. It is  **guaranteed**  that all the elements of the array can be made equal using some operations.

Given an integer  `n`, the length of the array. Return  _the minimum number of operations_  needed to make all the elements of arr equal.

**Example 1:**

**Input:** n = 3
**Output:** 2
**Explanation:** arr = [1, 3, 5]
First operation choose x = 2 and y = 0, this leads arr to be [2, 3, 4]
In the second operation choose x = 2 and y = 0 again, thus arr = [3, 3, 3].

 - [ ] Analysis
	 have to find the rules, and when you find the best way is to balance the`[i, n-1-1]`** pair, the equation is: (right_index*2+1) - (left_index*2+1) = **(right_index - left_index)**.
 - [ ] My Java solution
```
public int minOperations(int n) {
	int l = 0;
	int r = n-1;
	int cnt = 0;
	while(l<r)
	{
		cnt += r-l;
		l++;
		r--;
	}
	return cnt;
}
```

### 5489.  Magnetic Force Between Two Balls

In universe Earth C-137, Rick discovered a special form of magnetic force between two balls if they are put in his new invented basket. Rick has `n`  empty baskets, the  `ith`  basket is at  `position[i]`, Morty has  `m`  balls and needs to distribute the balls into the baskets such that the  **minimum magnetic force** between any two balls is  **maximum**.

Rick stated that magnetic force between two different balls at positions  `x`  and  `y`  is  `|x - y|`.

Given the integer array  `position` and the integer  `m`. Return  _the required force_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/11/q3v1.jpg)

**Input:** position = [1,2,3,4,7], m = 3
**Output:** 3
**Explanation:** Distributing the 3 balls into baskets 1, 4 and 7 will make the magnetic force between ball pairs [3, 3, 6]. The minimum magnetic force is 3. We cannot achieve a larger minimum magnetic force than 3.

 - [ ] My Wrong Idea (similar to insert the largest interval, but might not the global optimal space-between). Wrong understanding about the title.
 ```
// Wrong answser
PriorityQueue<int[]> pq = new PriorityQueue<>((a,b)->b[1]-b[0]-a[0]+a[1]);
        pq.offer(new int[]{0,n-1});
        while(m>2 && !pq.isEmpty())
        {
            int[] cur = pq.poll();
            int mid = (pos[cur[0]] + pos[cur[1]])/2;
            int i = Arrays.binarySearch(pos, cur[0]+1, cur[1], mid);
            if(i<0)
            {
                i= -i-1;
            }
            take[i] = true;
            // is valid interval
            if( cur[0]+1 < i )
            {
                pq.offer(new int[]{cur[0], i});
            }
            if( i+1 < cur[1])
            {
                pq.offer(new int[]{i, cur[1]});
            }
            m--;   
        }
 ```
 - [ ] Correct Solution: use binary search to find the largest and valid space
 ```
  public int maxDistance(int[] pos, int m) {
        int n = pos.length;
        Arrays.sort(pos);
        int l = 1;
        int r = 1000000000;
        while(l<=r)
        {
            int mid = (l+r+1)/2;
            // cnt valid 
            int start = -1000000000;
            int cnt = 0;
            for(int i: pos)
            {
                if(i-start>=mid)
                {
                    cnt++;
                    start = i;
                }
            }
            if(cnt >=m )
            {
                l = mid+1;
            }
            else
            {
                r = mid-1;
            }
        }
        return r;
    }
 ```

### 5490.  Minimum Number of Days to Eat N Oranges

There are  `n`  oranges in the kitchen and you decided to eat some of these oranges every day as follows:

-   Eat one orange.
-   If the number of remaining oranges (`n`) is divisible by 2 then you can eat n/2 oranges.
-   If the number of remaining oranges (`n`) is divisible by 3 then you can eat 2*(n/3) oranges.

You can only choose one of the actions per day.

Return the minimum number of days to eat  `n`  oranges.

 - [ ] Try to Find the rules, list all possible cases:
 - n == 1, return 1
 - n%2 == 0, return 1+helper(n/2)
 - n%2 == 1, return **1**(eat one)+**1**(can eat n/2)+**helper(n/2)**(remaining)
 - n%3 == 0, return 1+helper(n/3)
 - n%3 == 1, return **1**(eat one)+**1**(can eat n*2/3)+**helper(n/3)**(remaining)
 - n%3 == 2, return **2**(at least 2 days to eat 2)+**1**(eat n*2/3)+**helper(n/3)**(remaining)
 
 - [ ] Summary these rules and abstract cleaner logic
 result = min( 1 + n%2 + helper(n/2), 1+ n%3 + helper(n/3) );
 - [ ] My java recursion solution with memo:
	 ```
	 class Solution {
	    Map<Integer, Integer> dp = new HashMap<>();
	    public int minDays(int n) {
	        if(n == 0)
	            return 0;
	        if(n == 1)
	            return 1;
	        if(dp.containsKey(n))
	            return dp.get(n);
	        int res = n;
	        res = Math.min(n,Math.min(minDays(n/3)+n%3+1, minDays(n/2)+n%2+1));
	        dp.put(n, res);
	        return res;  
	    }
	}
	 ```
