## Leetcode contest 206


### 1582.  Special Positions in a Binary Matrix

Given a `rows x cols` matrix `mat`, where  `mat[i][j]`  is either  `0`  or  `1`, return  _the number of special positions in  `mat`._

A position  `(i,j)`  is called  **special** if `mat[i][j] == 1`  and all other elements in row  `i` and column  `j` are  `0`  (rows and columns are  **0-indexed**).

**Example 1:**
**Input:** mat = [[1,0,0],
              [0,0,**1**],
              [1,0,0]]
**Output:** 1
**Explanation:** (1,2) is a special position because mat[1][2] == 1 and all other elements in row 1 and column 2 are 0.

**Example 2:**
**Input:** mat = [[**1**,0,0],
              [0,**1**,0],
              [0,0,**1**]]
**Output:** 3
**Explanation:** (0,0), (1,1) and (2,2) are special positions.
**Java Solution:**
```
public int numSpecial(int[][] mat) {    
        int res = 0;
        int m = mat.length;
        int n = mat[0].length;
        int col[] = new int[n];
        int row[] = new int[m];
        for(int i =0 ;i<m;i++)
            for(int j = 0;j<n;j++)
            {
                if(mat[i][j] == 1)
                {
                    col[j]++;
                    row[i]++;
                }
            }
        for(int i =0 ;i<m;i++)
            for(int j = 0;j<n;j++)
            {
                if(mat[i][j] == 1 && col[j] == 1 && row[i] ==1)
                {
                    res++;
                }
            }
        return res;
        
    }
```
### 1583.  Count Unhappy Friends

You are given a list of `preferences` for `n` friends, where  `n`  is always  **even**.

For each person  `i`, `preferences[i]` contains a list of friends **sorted**  in the  **order of preference**. In other words, a friend earlier in the list is more preferred than a friend later in the list. Friends in each list are denoted by integers from  `0`  to  `n-1`.

All the friends are divided into pairs. The pairings are given in a list `pairs`, where  `pairs[i] = [xi, yi]`  denotes  `xi` is paired with  `yi`  and  `yi`  is paired with  `xi`.

However, this pairing may cause some of the friends to be unhappy. A friend  `x` is unhappy if  `x` is paired with  `y` and there exists a friend  `u` who is paired with  `v` but:

-   `x` prefers  `u` over  `y`, and
-   `u` prefers  `x` over  `v`.

Return  _the number of unhappy friends_.

**Example 1:**
**Input:** n = 4, preferences = [[1, 2, 3], [3, 2, 0], [3, 1, 0], [1, 2, 0]], pairs = [[0, 1], [2, 3]]
**Output:** 2
**Explanation:**
Friend 1 is unhappy because:
- 1 is paired with 0 but prefers 3 over 0, and
- 3 prefers 1 over 2.
Friend 3 is unhappy because:
- 3 is paired with 2 but prefers 1 over 2, and
- 1 prefers 3 over 0.
Friends 0 and 2 are happy.

**Example 2:**
**Input:** n = 2, preferences = [[1], [0]], pairs = [[1, 0]]
**Output:** 0
**Explanation:** Both friends 0 and 1 are happy.

**Analysis:**
- spend so much time to figure out `x` and `v` index to depend that whether `u`  is happy.
- first want to save time, it's wrong to use **binarySearch(x), and binarySearch(v)**, it's wrong, because the value in preference array is not sorted! 
-  just use **O(n)** to find the index of `x` and `v`
- if any friend is NO.1 matched, so it's totally good!

**Java solution**
```
 public int unhappyFriends(int n, int[][] preferences, int[][] pairs) {
        Map<Integer,Integer> map = new HashMap<>();
        for(int[] p: pairs)
        {
            map.put(p[0], p[1]);
            map.put(p[1], p[0]);
        }
        int cnt = 0;

        for(int x: map.keySet())
        {
            int p[] = preferences[x];
            int y = map.get(x);
            if(p[0] == y)
                continue;
            int i = 0;         
            while(p[i] != y)
            {
                int u = p[i];
                int[] p2 = preferences[u];
                int v = map.get(u);
                 // System.out.println(x + ", " + y+ ", "+ u + ", "+v);
                boolean find = false;
                for(int j = 0; j<p2.length; j++)
                {
                    if(p2[j] == x)
                    {
                       // System.out.println("find: "+ x);
                        find = true;
                        break;
                    }
                    if(p2[j] == v)
                    {
                        break;
                    }
                }
                if(find)
                {
                     cnt++;
                    break;
                }
                i++;
            }
        }
        return cnt;   
    }
```

### 5513.  Min Cost to Connect All Points ( not solved)
You are given an array `points` representing integer coordinates of some points on a 2D-plane, where  `points[i] = [xi, yi]`.

The cost of connecting two points  `[xi, yi]`  and  `[xj, yj]`  is the  **manhattan distance**  between them: `|xi  - xj| + |yi  - yj|`, where  `|val|`  denotes the absolute value of `val`.

Return _the minimum cost to make all points connected._  All points are connected if there is  **exactly one**  simple path between any two points.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/26/d.png)

**Input:** points = [[0,0],[2,2],[3,10],[5,2],[7,0]]

**Output:** 20

**Explanation:** 

![](https://assets.leetcode.com/uploads/2020/08/26/c.png)

We can connect the points as shown above to get the minimum cost of 20.
Notice that there is a unique path between every pair of points.

**Wrong idea**
- want to sort by x and y
- want to store **map<x, List( Points )>**, for each point poll the closest one, and find the min-cost.
- find closest x: compare points
- find closest y: compare points
**Better solution reference from others:**
* Union-find: connect all points into one graph
* make edges **( point_1, point2 )**,  sort by computed cost: `|x1-x2| + |y1-y2|`

```
class Solution {
    class Pair {
        int x;
        int y;
        int cost;
        public Pair(int x, int y, int cost)
        {
            this.x = x;
            this.y = y;
            this.cost = cost;
        }
    }
    public int minCostConnectPoints(int[][] points) {
        int n = points.length;
        List<Pair> list = new ArrayList<>();
        for(int i=0; i<n; i++)
            for(int j=i+1; j<n; j++) {
                list.add( new Pair(i,j, Math.abs(points[i][0]-points[j][0])+Math.abs(points[i][1]-points[j][1]) ));
        }
        
        Collections.sort(list, (a,b)->a.cost-b.cost);
        
        int res = 0;
        int[] parent = new int[n];
        for(int i=0; i<n; i++)
        {
            parent[i] = i;
        }   
        for(Pair p: list)
        {
            int x = findParent(parent, p.x);
            int y = findParent(parent, p.y);
            if(x != y)
            {
                parent[x] = y;
                res += p.cost;
            }
        }    
        return res; 
    }
    public int findParent(int[] parent, int x)
    {
        if(parent[x] != x)
        {
            parent[x] = findParent(parent, parent[x]);
        }
        return parent[x];
    }      
}
```

### 5514.  Check If String Is Transformable With Substring Sort Operations (not solved)

Given two strings `s`  and  `t`, you want to transform string `s`  into string `t`  using the following operation any number of times:

-   Choose a  **non-empty**  substring in `s` and sort it in-place so the characters are in **ascending order**.

For example, applying the operation on the underlined substring in `"14234"` results in  `"12344"`.

Return  `true`  if  _it is possible to transform string  `s` into string  `t`_. Otherwise, return  `false`.

A  **substring** is a contiguous sequence of characters within a string.

**Example 1:**
**Input:** s = "84532", t = "34852"
**Output:** true
**Explanation:** You can transform s into t using the following sort operations:
"84532" (from index 2 to 3) -> "84352"
"84352" (from index 0 to 2) -> "34852"

**Example 2:**
**Input:** s = "34521", t = "23415"
**Output:** true
**Explanation:** You can transform s into t using the following sort operations:
"34521" -> "23451"
"23451" -> "23415"

**Example 3:**
**Input:** s = "12345", t = "12435"
**Output:** false

**Analysis:**
What is the false condition that we cannot reverse it?
the example3:
|  | num | index
|--|--|--|
|t | 3 | |
|s | 3 | 3|
|s | 4 | 4|

Because when we reverse at `num=3`, the larer number `num=4` will always be after it, so that we can never make these two string equal
*  "345"
* "35"

So we need to scan target string from last character to first character to ensure the order is correct.  The judging condition will be: 
* num = target[i];
* s-index = find the last target_num index in string s;
* check if any number larger than current num at string s 
* larger number index in s should be smaller than s-index ( happy path )
* larger number index in s larger than s-index (failure path)

**Java Solution:**
```
public boolean isTransformable(String s, String t) {
        if(s.length() != t.length())
            return false;
        if(t.length() == 1)
        {
            return s.equals(t);
        }
       
        int n = t.length();
        int[] cnt1 = new int[10];
        int[] cnt2 = new int[10];
        
        for(int i = 0; i<n; i++)
        {
            cnt1[s.charAt(i)-'0']++;
            cnt2[t.charAt(i)-'0']++;
        }
          
        for(int i = 0; i<10; i++)
        {
            if(cnt1[i] != cnt2[i])
            {
                return false;
            }
        }
        List<Integer>[] list1 = new List[10];
         for(int i = 0; i<n; i++)
         {
             int s1 = s.charAt(i) - '0';
             if(list1[s1] == null)
             {
                 list1[s1] = new ArrayList<>();
             }
             list1[s1].add(i);
         }
        for(int i = n-1; i>=0; i--)
        {
            int num = t.charAt(i) - '0';
            int sIndex = list1[num].get(list1[num].size()-1);
            list1[num].remove(list1[num].size()-1);
            for(int j = num+1; j<10; j++)
            {
                if(list1[j] == null || list1[j].size() == 0)
                    continue;
                int lastIndex = list1[j].get(list1[j].size()-1);
                if(lastIndex > sIndex)
                {
                    return false;
                }
            }   
        }
        return true;
    }
```

My rank: keep moving in next week!
3791 / 13291