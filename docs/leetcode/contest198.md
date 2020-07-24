## Leetcode Contest 198

### 1518. Water Bottles

Given  `numBottles` full water bottles, you can exchange  `numExchange`  empty water bottles for one full water bottle.

The operation of drinking a full water bottle turns it into an empty bottle.

Return the  **maximum**  number of water bottles you can drink.

My solution:
```
public int numWaterBottles(int numBottles, int numExchange) {
        int res = numBottles;
        while(numBottles >= numExchange)
        {
            int cnt = numBottles / numExchange;
            res += cnt;
            numBottles = cnt + numBottles % numExchange; 
            
        }
        return res;
}
```

### 1519. Number of Nodes in the Sub-Tree With the Same Label

[Description leetcode 1519](https://leetcode.com/contest/weekly-contest-198/problems/number-of-nodes-in-the-sub-tree-with-the-same-label/)

My solution:
```
class Solution {
    int res[];  
    int cnt[];
    public int[] countSubTrees(int n, int[][] edges, String labels) {
        res = new int[n];
        cnt = new int[256];
        Map<Integer,List<Integer>> map = new HashMap<>();
        for(int[] e: edges)
        {
            if(!map.containsKey(e[0]))
                map.put(e[0], new ArrayList<Integer>());
            if(!map.containsKey(e[1]))
                map.put(e[1], new ArrayList<Integer>());
            map.get(e[0]).add(e[1]);
            map.get(e[1]).add(e[0]);
        }
        dfs(map, n, 0, labels, -1);
        return res; 
    }
    
    public void dfs(Map<Integer,List<Integer>> adj, int n, int cur,  String s, int prev)
    {
        
        // cur level
        char c = s.charAt(cur);
        int count = cnt[c]++;
        
        // next level
        if(adj.containsKey(cur))
        {     
            for(int next: adj.get(cur))
            {
                if(next == prev)
                {
                    continue;
                }
               dfs(adj, n, next, s, cur);
            }
        }
        res[cur] = cnt[c] - count;
    }
}
```

### 1520. Maximum Number of Non-Overlapping Substrings

Given a string  `s` of lowercase letters, you need to find the maximum number of  **non-empty**  substrings of `s` that meet the following conditions:

1.  The substrings do not overlap, that is for any two substrings  `s[i..j]`  and  `s[k..l]`, either  `j < k`  or  `i > l` is true.
2.  A substring that contains a certain character `c` must also contain all occurrences of  `c`.

Find  _the maximum number of substrings that meet the above conditions_. If there are multiple solutions with the same number of substrings,  _return the one with minimum total length._ It can be shown that there exists a unique solution of minimum total length.

Notice that you can return the substrings in  **any**  order.

* Analyze conditions:
	1 ) The substrings **do not overlap**: means we cannot choose one string and another string as solution at the same time, if they have index overlapped. For example: `aaca`, if we choose `aaca` then we cannot choose `c` as solution.
	2 )  	**contain all occurrences**: means we must find the right most valid index as our substring range. `*Hint for here is to find last index for each character.`

* Algorithm steps
   1 ) loop at each index, because we need to find the optimized solution: `max number` && `shortest length`.
   2 ) at each index, find the current valid solution with range of `[ start, rightMost ]`.
   
  ![image](../assets/lc1520_00.png ':size=430x164')

   3 ) Update final result:  The reason why we can ensure the **current is always a better solution than previous** is that previous solution is either:
   * current is inside of previous substring: then we get a  **shorter length**
   * current is outside of previous substring: then we can **increase number of result**
   ```
   if start > prev_Right
   {
	   // no overrlap
	   add to result;
   }
   else // start <= pre
   {
	   // overrlap, this is a better solution
   }
   ```
  
  ![image](../assets/lc1520_01.png ':size=415x206')

* complete java code:
```
 public List<String> maxNumOfSubstrings(String s) {
        
        List<String> res = new ArrayList<>();
        int[] f = new int[256];
        int[] l = new int[256];
        Arrays.fill(f, s.length());
        Arrays.fill(l, -1);
       char[] arr = s.toCharArray();
        for (int i = 0; i < s.length(); i++) {
            char c = arr[i];
            f[c] = Math.min(f[c], i);
            l[c] = Math.max(l[c], i);
        }
        int prev = -1; // check if in previous substring
        for(int i = 0; i<s.length(); i++)
        {
            char c = s.charAt(i);
            if(f[c] < i)
            {
                continue;
            }
            int end =  l[c];
            int pos = i;
            while(pos <= end && f[s.charAt(pos)] >= i)
            {
                char t = s.charAt(pos);
                
                end = Math.max(end, l[t]);
                
                pos++;
            }
            if(pos< end && f[s.charAt(pos)] < i)
            {
                continue;
            }
            String sub = s.substring(i, end+1);
            if(prev < i)
            {
                // add
                 res.add(sub);
            }
            else
                res.set(res.size()-1, sub); // replace
            
             prev = end;
        }
        return res;
    }
```

### 1521.  Find a Value of a Mysterious Function Closest to Target
Hard

![](https://assets.leetcode.com/uploads/2020/07/09/change.png)

Winston was given the above mysterious function  `func`. He has an integer array  `arr`  and an integer  `target`  and he wants to find the values `l`  and  `r` that make the value  `|func(arr, l, r) - target|`  minimum possible.

Return  _the minimum possible value_  of  `|func(arr, l, r) - target|`.

Notice that  `func`  should be called with the values `l`  and  `r`  where  `0 <= l, r < arr.length`.

* Analysis: 
	1 ) Important!!!!! here is the `bit and` operator not the XOR, so we can use the preSum[] idea to solve this
  2 )  **property of &**: x & x = x,  `x & other <= x` so that means, the more numbers you take, the definitely reesult is becoming smaller and smaller.
  3 ) try to find the Closest value to target, `Math.abs()`, then if once we hit 
  ```
  if cur-result < target
  {
	// calculate once
	// then break; 
	// because no more closest value can be found!!!	 
  }
  ```
* Solution 1:  n^2 but with break rules
```
public int closestToTarget(int[] arr, int target) {
        int n = arr.length;
        int res = Integer.MAX_VALUE;
        
        for(int i = 0; i<n;i++)
        {
            int bitres = arr[i];
              for(int j = i; j<n;j++)
              {
                   bitres = arr[j] & bitres;
                   res = Math.min(res, Math.abs(bitres - target));
                   if(bitres < target)
                   {
                       break;
                   }
              }
            if(bitres > target)
            {
                // here is the smallest value that > target
                break;
            }
        }
        return  res;
    }
```

* Solution 2: cleaner code with O(n)
 ```
public int closestToTarget(int[] arr, int target) {
        int n = arr.length;
        int sum = arr[0];
        int res = Math.abs(sum -target);
    
    for(int i=1;i<arr.length;i++){ 
        if(sum < target){
               sum =arr[i];
        }else{
               sum = sum & arr[i];
        } 
            
        res =Math.min(res,Math.min(Math.abs(arr[i]-target),Math.abs(sum-target)));
        }
        return res;   
    }
 ```
