## Leetcode Contest 197
###  1512. Number of Good Pairs
Given an array of integers `nums`.
A pair `(i,j)` is called  _good_  if `nums[i]`  ==  `nums[j]`  and  `i`  <  `j`.
Return the number of  _good_  pairs.

**Example 1:**
**Input:** nums = [1,2,3,1,1,3]
**Output:** 4
**Explanation:** There are 4 good pairs (0,3), (0,4), (3,4), (2,5) 0-indexed.
#### my solution: brute force
```
class Solution {
    public int numIdenticalPairs(int[] nums) {
        int cnt = 0;
        for(int i =0; i<nums.length; i++)
            for(int j = i+1; j<nums.length; j++)
            {
                if(nums[i] == nums[j])
                {
                    cnt++;
                }
            }
        return cnt;
        
    }
}
```
### 1513. Number of Substrings With Only 1s
Given a binary string `s` (a string consisting only of '0' and '1's).
Return the number of substrings with all characters 1's.
Since the answer may be too large, return it modulo 10^9 + 7.

**Example 1:**
**Input:** s = "0110111"
**Output:** 9
**Explanation:** There are 9 substring in total with only 1's characters.
"1" -> 5 times.
"11" -> 3 times.
"111" -> 1 time.

**Example 2:**
**Input:** s = "101"
**Output:** 2
**Explanation:** Substring "1" is shown 2 times in s.
#### My solution
1. Similar like the problem of count rectangles, any single unit rectangle or continuous unit size of rectangle in one row.
2.  Calc continuous sum at each index, then sum all position's count.

![image](../assets/lc1531.png ':size=280x92')

3. my java solution
```
class Solution {
    public int numSub(String s) {
        int mod = 1000000007;
        int n = s.length();
        int[] dp = new int[n+1];
        int res = 0;
        for(int i = 1; i<=n;i++)
        {
            if(s.charAt(i-1) == '1')
            {
                dp[i] = (dp[i-1] + 1) % mod;
            }else
            {
                dp[i] = 0;
            }
            res = (res+dp[i] ) % mod;
        }
        return res;
    }
}
``` 

### 1514. Path with Maximum Probability
You are given an undirected weighted graph of `n` nodes (0-indexed), represented by an edge list where `edges[i] = [a, b]` is an undirected edge connecting the nodes `a` and `b` with a probability of success of traversing that edge `succProb[i]`.

Given two nodes `start` and `end`, find the path with the maximum probability of success to go from `start` to `end` and return its success probability.

If there is no path from `start` to `end`,  **return 0**. Your answer will be accepted if it differs from the correct answer by at most  **1e-5**.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/09/20/1558_ex1.png)**

**Input:** n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.2], start = 0, end = 2
**Output:** 0.25000
**Explanation:** There are two paths from start to end, one having a probability of success = 0.2 and the other has 0.5 * 0.5 = 0.25.

`Attention`: 
**Constraints:**
-   `0 <= succProb[i] <= 1`
 Because of this constraints, regular dfs will timeout, even with memorization, still timeout. Here is the key point for answer. We can always poll out largest factor number to get the correct result by sorting.

#### My solution process
1. wrong solution 1:
DFS in undirected-graph: timeout
2. Use priorityQueue to sort by largest factor/weight on each edge, 
    use BFS, to search neighbors with sorted largest edge at first, then we can ensure to get the correct answer.
3. data structure
	```
	{
	    int node_index;
	    double dist; // total weight from source to current node
	}
	```
	And use array to record distance for each node to source: `double: dist[n]`
And init all distance to be `-1` from source, because at initial status, none of these other nodes can be reachable.
4. Solution:
```
class Solution {

    class Node{
         double dist;
         int id;
         public Node(int id, double dist)
         {
             this.id = id;
             this.dist = dist;
         }
     }
    public double maxProbability(int n, int[][] edges, double[] succProb, int start, int end) {
        Map<Integer, Map<Integer, Double>> map = new HashMap<>();
        for(int i=0; i<edges.length; i++)
        {
            int[] e = edges[i];
            map.putIfAbsent(e[0], new HashMap<>());
            map.putIfAbsent(e[1], new HashMap<>());
            map.get(e[0]).put(e[1], succProb[i]);
            map.get(e[1]).put(e[0], succProb[i]);
        }
        double dist[] = new double[n];
        Arrays.fill(dist, -1);
        dist[start] = 1;

        PriorityQueue<Node> q = new PriorityQueue<Node>((a,b)->{
            if(b.dist - a.dist> 0.00001)
            {
                return 1;
            }
            else
                 return -1;
           });
        q.offer(new Node(start, 1.0));
        while(!q.isEmpty())
        {
            Node node = q.poll();
            int cur = node.id;
            double d = node.dist;
            if(cur == end)
            {
                return d;
            }
            if(!map.containsKey(cur))
            {
                continue;
            }
            for(int next: map.get(cur).keySet())
            {
                double newDist = d * map.get(cur).get(next);
                if(newDist > dist[next])
                {
                    dist[next] = newDist;
                    q.offer(new Node(next, newDist));
                }
            }
            
        }
       
            return 0.0000;
    }
} 
```

### 5463. Best Position for a Service Centre
Description: [https://leetcode.com/problems/best-position-for-a-service-centre/](https://leetcode.com/problems/best-position-for-a-service-centre/)

Attention:
`Constraints:`
* 1 <= positions.length <= 50
#### Solution after contest
1. find average x_position, y_position for all nodes.
2. Need to search all the possible location around this point.
   Because of the constraints: we mostly have 50 points, so the longest distance is 100, and our calculated average x & y should be 50.
3. search range:  
* 4 directions: `{{0,-1},{0,1},{1,0},{-1,0}}` 
* `radius/offset` to center point `[avg_x, avg_y]` : from [0, 50]
* condition to compare minimum result: when `diff < 1e-8`
4. Helper functions:
* square distance of two points (a, b): 
`Math.sqrt( (b[0]-a[0])*(b[0]-a[0]) + (b[1]-a[1])*(b[1]-a[1]))`
* calculate possible total distance = center_point to all service_centers
5. My solution:
	```
	class Solution {
	    public double squareDist(double[] a, int[] b)
	    {
	        return Math.sqrt( (b[0]-a[0])*(b[0]-a[0]) + (b[1]-a[1])*(b[1]-a[1]));
	    }
	    public double calcAllPositions(int[][] pos, double[] center)
	    {
	        double res = 0;
	        for(int[] p: pos)
	        {
	            res += squareDist(center, p);
	        }
	        return res;
	    }
	    int[][] dir = {{0,-1},{0,1},{1,0},{-1,0}};
	    public double getMinDistSum(int[][] positions) {
	        double x = 0;
	        double y = 0;
	        int n = positions.length;
	        for(int[] pos: positions)
	        {
	            x += pos[0];
	            y += pos[1];
	        }
	        x = x / n;
	        y = y / n; 
	        double res = calcAllPositions(positions, new double[]{x,y});
	        double step = 100.0;
	        double eps =  Math.pow(10, -8);
	        while(step > eps)
	        {
	            boolean find = false;
	            for(int i = 0; i<4; i++)
	            {
	                double xx = x + dir[i][0]  * step;
	                double yy = y + dir[i][1] * step;
	                double dist = calcAllPositions(positions, new double[]{xx,yy});
	                if(dist < res)
	                {
	                    res = dist;
	                    find = true;
	                    x = xx;
	                    y = yy;
	                    break;
	                }
	            }
	            if(!find)
	            {
	                step = step / 2;
	            }
	        }
	        
	        return res;
	        
	    }
	}
	```