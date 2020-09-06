## Leetcode contest 205

### 1576.  Replace All ?'s to Avoid Consecutive Repeating Characters

Given a string `s`  containing only lower case English letters and the '?' character, convert  **all** the '?' characters into lower case letters such that the final string does not contain any  **consecutive repeating** characters. You  **cannot** modify the non '?' characters.

It is  **guaranteed** that there are no consecutive repeating characters in the given string  **except** for '?'.

Return the final string after all the conversions (possibly zero) have been made. If there is more than one solution, return any of them. It can be shown that an answer is always possible with the given constraints.

**Example 1:**
**Input:** s = "?zs"
**Output:** "azs"
**Explanation**: There are 25 solutions for this problem. From "azs" to "yzs", all are valid. Only "z" is an invalid modification as the string will consist of consecutive repeating characters in "zzs".

**Solution:**
```
public String modifyString(String s) {
	List<Integer> pos = new ArrayList<>();
	HashSet<Character> set = new HashSet<>();
	char[] ch = s.toCharArray();
	for(int i = 0; i< ch.length; i++)
	{
		char c = ch[i];
		if(c == '?')
		{
			for(char t ='a'; t<= 'z'; t++)
			{
				if(( i==0 || i>0 && ch[i-1] != t) && (i == ch.length-1 || ch[i+1] !=t ) )
                {
                        ch[i] = t;
                        break;
                }
            }
         }
    }
	return new String(ch);   
}
```

### 1577.  Number of Ways Where Square of Number Is Equal to Product of Two Numbers

Given two arrays of integers `nums1`  and  `nums2`, return the number of triplets formed (type 1 and type 2) under the following rules:

-   Type 1: Triplet (i, j, k) if  `nums1[i]2 == nums2[j] * nums2[k]`  where  `0 <= i < nums1.length`  and  `0 <= j < k < nums2.length`.
-   Type 2: Triplet (i, j, k) if  `nums2[i]2 == nums1[j] * nums1[k]`  where  `0 <= i < nums2.length`  and  `0 <= j < k < nums1.length`.

**Example 1:**
**Input:** nums1 = [7,4], nums2 = [5,2,8,9]
**Output:** 1
**Explanation:** Type 1: (1,1,2), nums1[1]^2 = nums2[1] * nums2[2]. (4^2 = 2 * 8). 

**Solution:**
1. waste so much time at first. 
Wrong way:
 like 3 sum, try to loop `i, j, k` , with memorization, but timeout!  **O(n^3)**.
2. how to save time !!! reduce to O(n^2)
use **HashMap** to memorize the count of the target value.
	```
	// for each num:
	map.put(target, getOrDefault(target,0)+1);
	```
My java solution:
```
class Solution {
    int cnt = 0;
    Set<String> set = new HashSet<>();
    public int numTriplets(int[] nums1, int[] nums2) {
       count(nums1, nums2);
       count(nums2, nums1);
       // System.out.println(set);
       return cnt; 
        
    }
    public void count(int[] nums1, int[] nums2)
    {
        int m = nums1.length;
        int n = nums2.length;
        HashMap<Long, Integer> map = new HashMap<>();
        for(int n1: nums1)
        {
             long target = (long)n1 * n1;
             map.put(target, map.getOrDefault(target,0)+1);
        }
        for(int j = n-1 ;j>0; j--)  
            for(int i = j-1; i>=0; i--)
            {
                long n2 = nums2[i];
                long n3 = nums2[j];
                if(map.containsKey(n2*n3 ))
                {
                      cnt+= map.get(n2*n3);
                }   
            }
         return;
    }
}
```

### 1578.  Minimum Deletion Cost to Avoid Repeating Letters

Given a string  `s`  and an array of integers  `cost`  where  `cost[i]` is the cost of deleting the character  `i` in  `s`.

Return the minimum cost of deletions such that there are no two identical letters next to each other.

Notice that you will delete the chosen characters at the same time, in other words, after deleting a character, the costs of deleting other characters will not change.

**Example 1:**

**Input:** s = "abaac", cost = [1,2,3,4,5]
**Output:** 3
**Explanation:** Delete the letter "a" with cost 3 to get "abac" (String without two identical letters next to each other).

**Solution**
1. wrong way!!!
Don't think too much!!!! Not use **DFS**
2. simply just to find the continuous letter like "aaaaa"
3. find the smallest cost sum in array `cost[] from start to end`.  **min_cost = total_cost - max_cost**

My java solution:
```
 public int minCost(String s, int[] cost) {
       char ch[] = s.toCharArray();
       int min = 0;
       for(int i = 0; i< ch.length-1; i++)
       {
           int j = i+1;
           while(j < ch.length && ch[i] == ch[j])
           {         
              j++;
           }
           j--;
           min += getMinCost(cost, i, j);
         
        i=j;
       }
        return min;
    }
   public int getMinCost(int arr[], int i, int j){
       int n = j-i+1;
       if(n == 2)
       {
           return Math.min(arr[i], arr[j]);
       }
       int max = 0;
       int sum = 0;
       for(int k = i; k<=j; k++)
       {
           max = Math.max(max, arr[k]);
           sum += arr[k];
       }  
       return sum - max;  
   }
```

### 1579.  Remove Max Number of Edges to Keep Graph Fully Traversable

Alice and Bob have an undirected graph of `n` nodes and 3 types of edges:

-   Type 1: Can be traversed by Alice only.
-   Type 2: Can be traversed by Bob only.
-   Type 3: Can by traversed by both Alice and Bob.

Given an array `edges` where `edges[i] = [typei, ui, vi]` represents a bidirectional edge of type `typei` between nodes `ui` and `vi`, find the maximum number of edges you can remove so that after removing the edges, the graph can still be fully traversed by both Alice and Bob. The graph is fully traversed by Alice and Bob if starting from any node, they can reach all other nodes.

Return  _the maximum number of edges you can remove, or return_  `-1`  _if it's impossible for the graph to be fully traversed by Alice and Bob._

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/08/19/ex1.png)**

**Input:** n = 4, edges = [[3,1,2],[3,2,3],[1,1,3],[1,2,4],[1,1,2],[2,3,4]]
**Output:** 2
**Explanation:** If we remove the 2 edges [1,1,2] and [1,1,3]. The graph will still be fully traversable by Alice and Bob. Removing any additional edge will not make it so. So the maximum number of edges we can remove is 2.

**Solution:**
1. similar past problem: critical-connection in MST, union-find, check if can remove current edge. Similar past problem:  [LC1498](https://leetcode.com/problems/find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree/)
2. First coming to my mind is the Union-find, build **two different DSU graph** for alice and bob.
3. How to check finally is valid result after removing edges?
Both graph size in DSU is N, and only one root parent for each node in both graph.
4. Remember to add a little bit more than regular simple DSU. `size[]` to count size at each node in graph.  When we union two nodes, add larger-size  group to smaller-szie group.
	```
	if(size[y] > size[x])
	{
		parent[y] = x;
		// also update the size
		size[x] += size[y];
		size[y] = size[x];
	}
	```
My java solution:
```
class Solution {
    public int maxNumEdgesToRemove(int n, int[][] edges) {
        List<int[]> both = new ArrayList<>();
        List<int[]> alice = new ArrayList<>();
        List<int[]> bob = new ArrayList<>();
        for (int[] e : edges)
        {
            e[1] -= 1;
            e[2] -= 1;
        }
        for(int[] e : edges)
        {
            if(e[0] == 1)
            {
                alice.add(new int[]{e[1], e[2]});
            }
            else if(e[0] == 2)
            {
                bob.add(new int[]{e[1], e[2]});
            }
            else if(e[0] == 3)
            {
                both.add(new int[]{e[1], e[2]});
            }
        }
        int cnt = 0;
        DSU dsu1 = new DSU(n);
        DSU dsu2 = new DSU(n);
        for(int[] e: both)
        {
            dsu1.union(e[0], e[1]);
            if(!dsu2.union(e[0], e[1]))
            {
                // cannot union
                // can remove
                cnt++;
            }
        }
        for(int[] e: alice)
        {
            if(!dsu1.union(e[0], e[1]))
            {
                cnt++;
            }
        }
        
        for(int[] e: bob)
        {
            if(!dsu2.union(e[0], e[1]))
            {
                cnt++;
            }
        }
        int size1 = dsu1.getSize(0);
        int size2 = dsu2.getSize(0);
        // System.out.println("size1 ="+ size1 + ", size2 = " + size2);
        if( size1 != n)
            return -1;
        if(size2 != n)
            return -1; 
        return cnt;
    
    }
}
class DSU {
    protected int[] p;
    protected int[] size;
    public DSU(int n) {
        p = new int[n];
        size = new int[n];
        Arrays.fill(size, 1);
        init();
    }
    public void init() {
        for (int i = 0; i < p.length; i++) {
            p[i] = i;
        }
    }
    public final int find(int a) {
        if (p[a] == p[p[a]]) {
            return p[a];
        }
        return p[a] = find(p[a]);
    }
    public final boolean union(int a, int b) {
        a = find(a);
        b = find(b);
        if (a == b) {
            return false;
        }
        if(size[a] > size[b])
        {
            int tmp = b;
            b = a;
            a = tmp;
        }
        p[b] = a;
        size[a] += size[b];
        size[b] = size[a];
        return true;
    }
    public final int getSize(int x)
    {
        return size[find(x)];
    }
}
```