## 1489. Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree

####  Kruskal’s Algorithm – Minimum Spanning Tree (MST)

Reference:

[KruskalMST.java](https://algs4.cs.princeton.edu/43mst/KruskalMST.java.html)

[Kruskal’s Greedy Algo-2](https://www.geeksforgeeks.org/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/)

Kruskal's algorithm processes the edges in order of their weight values (smallest to largest), taking for the MST each edge that does not form a cycle with edges previously added, stopping after adding V-1 edges. Finally check all those edges form a forest of trees that can be a valid MST.
#### 1. Sort edges[][] array by weight
```
Arrays.sort(edges, (a, b) -> a[2]-b[2]);	
```

#### 2. Init the graph for our MST, using union-find algorithm, we need a parent[] array, with initial value set to itself. 
```
int[] p = new int[n];
for (int i = 0; i < p.length; i++) {
	p[i] = i;
}
```
Also prepare public methods `findParent()` and 	`union(node_x, node_y)
```
public int findParent(int a, int[] p) {
     if (p[a] != p[p[a]]) {
        p[a] = findParent(p[a], p);
    }
    return p[a];
}
public void union(int a, int b, int[] p)
{
    int x = findParent(a, p);
    int y = findParent(b, p);
    if(x != y)
    {
            p[x] = y;
    }
}
```

#### 3. Gradually try to add each edge from sorted array edges[][] into our try, `must avoid edge that will form a cycle`:
```
for(int i = 0 ; i<edges.length; i++)
{
	int x = findParent(edges[i][0], p);
	int y = findParent(edges[i][1], p);	
	if(x != y)
	{
		union(edges[i][0], edges[i][1], p);
	}
}
```
#### 4. Finally verify whether this is a valid MST
A valid MST is that every node has one same `root parent`,  all nodes are added to our tree.
```
for(int i = 0 ; i<n; i++)
{
	if(findParent(i, p) != findParent(0, p))
	{	// NOT FOUND
		return Integer.MAX_VALUE; 
	}
}        
```
### LC1489 Solution
#### 1. what is critical and pseudo-critical edges?
* `critical edge`:  if we delete this ege from the graph would cause the MST weight to increase i
* `pseudo-critical edge`:  is that which can appear in some MSTs but not all.
	So if we force to `add this edge` at first to include into our tree, then check if we can also find another valid solution with `same total weight`.
#### 2. data structure 
* Store the edge index in original array as 4th element in each edge, so we need to expand our edge
	```
	int m = edges.length;
	int[][] e = new int[m][4];
	for (int i = 0; i < m; i++) {
		System.arraycopy(edges[i], 0, e[i], 0, 3);
	    e[i][3] = i; // store index
	}
	```
* Use two HashSet<Integer> to store `added and removed` edges that in our each experiment to construct our MST, so during the process, we try to find the critical and pseudo.
	```
	 Set<Integer> add = new HashSet<>();
	 Set<Integer> remove = new HashSet<>();
	```
* Compute the initial value as total MST weight
	Pseudo Code:
	```
	for each sorted edge: 
	{
		remove current edge[i]
		newTotalWeight = computeNewMST()
		if(newTotalWeight > initialValue)
		{
			this is a critical edge !
		}
		else
		{
			add this edge;
			newTotalWeight = computeNewMST()
			if(newTotalWeight = initialValue)
			{
				// so this edge can also in some MST
				this is a pseudo-critical edge
			}
		}
	}
	```	
* Final solution: complete code
	```
	class Solution {
	
		public List<List<Integer>> findCriticalAndPseudoCriticalEdges(int n, int[][] edges) {
		
			List<List<Integer>> res = new ArrayList<>();
			List<Integer> critical = new ArrayList<>();
			List<Integer> pseudo = new ArrayList<>();
			
			int m = edges.length;
			int[][] e = new int[m][4];
			for (int i = 0; i < m; i++) {
				System.arraycopy(edges[i], 0, e[i], 0, 3);
				e[i][3] = i; // store index
			}
			
			// sort by weight
			Arrays.sort(e, (a, b) -> a[2]-b[2]);

			Set<Integer> add = new HashSet<>();
			Set<Integer> remove = new HashSet<>();
			
			int initialValue = mst(e, n, add, remove);
			for(int i= 0; i<m; i++)
			{
				remove.add(i);
				if(mst(e, n, add, remove) > initialValue)
				{
					critical.add(e[i][3]); 
				}
				else 
				{
					remove.remove(i);
					add.add(i);
					if(mst(e, n, add, remove) == initialValue)
					{
						pseudo.add(e[i][3]);
					}
					
				}
				remove.remove(i);
				add.remove(i);
			}
			res.add(critical);
			res.add(pseudo);
			return res;
		}
		
		public int mst(int[][] e, int n, Set<Integer> add, Set<Integer> remove)
		{
			int res = 0;
		
			// init graph
			int[] p = new int[n];
			for (int i = 0; i < p.length; i++) {
				p[i] = i;
			}
		
			for(int i : add)
			{
			union(e[i][0], e[i][1], p);
				res += e[i][2];
			}

			for(int i = 0 ; i<e.length; i++)
			{
				if(remove.contains(i))
					continue;
				int x = findParent(e[i][0], p);
				int y = findParent(e[i][1], p);
				if(x != y)
				{
					union(e[i][0], e[i][1], p);
					res += e[i][2];
				}
			}

			for(int i = 0 ; i<n; i++)
			{
				if(findParent(i, p) != findParent(0, p))
					return Integer.MAX_VALUE; // NOT FOUND
			}
			
			return res;
		}

		public int findParent(int a, int[] p) {
			if (p[a] != p[p[a]]) {
			p[a] = findParent(p[a], p);
			}
			return p[a];
		}

		public void union(int a, int b, int[] p)
		{
			int x = findParent(a, p);
			int y = findParent(b, p);
			if(x != y)
			{
				p[x] = y;
			}
		}
	}
	```
