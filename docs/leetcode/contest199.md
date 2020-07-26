## Leetcode Contest 199
### 1528.  Shuffle String
Given a string  `s` and an integer array  `indices`  of the  **same length**.

The string  `s`  will be shuffled such that the character at the  `ith`  position moves to `indices[i]`  in the shuffled string.

Return  _the shuffled string_.

My solution: 
**Just rebuild the array, and return the new array.**
My java code:
```
 public String restoreString(String s, int[] indices) {
        int n = s.length();
        char[] c = new char[n];
        for(int i = 0; i<n; i++)
        {
            char t = s.charAt(i);
            c[indices[i]] = t;
        }
        
        return new String(c);
    }
```

### 1529.  Bulb Switcher IV

There is a room with  `n` bulbs, numbered from  `0`  to `n-1`, arranged in a row from left to right. Initially all the bulbs are  **turned off**.

Your task is to obtain the configuration represented by  `target`  where `target[i]`  is '1' if the i-th bulb is turned on and is '0' if it is turned off.

You have a switch to flip the state of the bulb, a flip operation is defined as follows:

-   Choose  **any**  bulb (index `i`) of your current configuration.
-   Flip each bulb from index `i`  to `n-1`.

When any bulb is flipped it means that if it is 0 it changes to 1 and if it is 1 it changes to 0.

Return the  **minimum**  number of flips required to form  `target`.

**Example 1:**
**Input:** target = "10111"
**Output:** 3
**Explanation:** Initial configuration "00000".
flip from the third bulb:  "00000" -> "00111"
flip from the first bulb:  "00111" -> "11000"
flip from the second bulb:  "11000" -> "10111"
We need at least 3 flip operations to form target.

My solution:
You have to find the rules behind this. Summary is:
*  **first bit** == 1, we need flip once
* **continuous same bit** is safe, won't cause another flip
*  **different bit with prev bit(i-1)** will increase one flip

![image](../assets/lc1529.png ':size=378x177')

**My java code:**
```
public int minFlips(String target) {
        int res = 0;
        int n = target.length();
        if(target.charAt(0) == '1')
        {
            res++;
        }
        for(int i = 1; i<n ; i++)
        {
            if(target.charAt(i) == target.charAt(i-1))
            {
                continue;
            }
            res++;
        }
        return res; 
    }
}
```

### 1530.  Number of Good Leaf Nodes Pairs

Given the  `root`  of a binary tree and an integer  `distance`. A pair of two different  **leaf**  nodes of a binary tree is said to be good if the length of  **the shortest path**  between them is less than or equal to  `distance`.

Return  _the number of good leaf node pairs_  in the tree.

My solution:
* Use `dfs` print out and store each path from root to leaf
   Attention: a tip here is to `reverse the path` in order to find parent.
* check for every pair of leaf nodes, if it's good or not
* How to find the `most closest common ancestor`?
  Since we store each parent node path, the question becomes to find the lowest   common ancestor of two leaf nodes.

  ![image](../assets/lc1530.png ':size=351x221')

**My java code:**
```
class Solution {
    public int countPairs(TreeNode root, int distance) {
        Map<TreeNode, List<TreeNode>> map = new HashMap<>();
        dfs(root, map, new ArrayList<TreeNode>());
        List<TreeNode> l = new ArrayList<>();
        
        for(TreeNode nd: map.keySet())
        {
            l.add(nd);
        }
        int n = l.size();
        int res = 0;
        for(int i = 0; i<n; i++)
            for(int j = i+1; j<n; j++)
            {
               if( isGoodPair(map, l.get(i), l.get(j), distance))
                   res++;
            }
        
        return res;  
    }
    
    public boolean isGoodPair( Map<TreeNode, List<TreeNode>> map, TreeNode l1, TreeNode l2, int target)
    {
        int dist = 0;
        List<TreeNode> p1 = map.get(l1);
         List<TreeNode> p2 = map.get(l2);
        for(int i = 0; i<p1.size(); i++)
        {
            TreeNode parent = p1.get(i);
            if(p2.contains(parent))
            {
                for(int j = 0; j<p2.size(); j++)
                {
                    if(p2.get(j) == parent)
                    {
                        dist += j+1;
                        dist += i+1;
                        if(dist <= target)
                        {
                            return true;
                        }
                        else
                        {
                            return false;
                        }
                    }
                }
            }
        }
        return false;
    }
    
    public void dfs(TreeNode node,  Map<TreeNode, List<TreeNode>> map, List<TreeNode> list)
    {
        if(node.left == null && node.right == null)
        {
            List<TreeNode> tmp = new ArrayList<>(list);
            Collections.reverse(tmp);
            map.put(node, tmp);
        }
        
        list.add(node);
        
        if(node.left != null)
        {
            dfs(node.left, map, list);
        }
        if(node.right != null)
        {
            dfs(node.right, map, list);
        }
        list.remove(list.size()-1);
    }
}
```

