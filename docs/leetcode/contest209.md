## Leetcode Contest 209 - Java and JavaScript

Don't use test case in your submission !!! I didn't pay attention to the rule, and got all my coin lost! Restart contest 209  from zero coin.


### 1608.  Special Array With X Elements Greater Than or Equal X

You are given an array  `nums`  of non-negative integers.  `nums`  is considered  **special**  if there exists a number  `x`  such that there are  **exactly**  `x`  numbers in  `nums`  that are  **greater than or equal to**  `x`.

Notice that  `x`  **does not**  have to be an element in  `nums`.

Return  `x`  _if the array is  **special**, otherwise, return_ `-1`. It can be proven that if  `nums`  is special, the value for  `x`  is  **unique**.

**Example 1:**
**Input:** nums = [3,5]
**Output:** 2
**Explanation:** There are 2 values (3 and 5) that are greater than or equal to 2.
**Constraints:**

-   `1 <= nums.length <= 100`
-   `0 <= nums[i] <= 1000`

**My java solution: (not a optimized solution)**
* use count-sort
* use accumulate sum in reverse order
* But time is better: **O(n) time complexity**
```
 public int specialArray(int[] nums) {
        int cnt[] = new int[1002];
        Arrays.fill(cnt, 0);
        for(int num: nums)
        {
           cnt[num]++;
        }
        int sum = 0;
        for(int i = cnt.length-1; i>=0; i--)
        {
            if(sum > 0)
            {
                cnt[i] += sum;
            }
            sum = cnt[i];
        } 
        for(int i = 0; i<cnt.length; i++)
        {
            if(i == cnt[i])
            {
                return i;
            }
        }
        return -1;
}
```
**After Contest:  JavaScript solution**
* Another idea: try every count from 0 - n (max is the length)
* time complexity: **O(100*n)**, here the constraint is max 100 numbers.
```
var specialArray = function(nums) {
    let res = 0
    for(let i = 1; i<= 100; i++)
    {
        let cnt = 0;
        for(const num of nums)
        {
            if(num>=i)
            {
                cnt++;
            }
        }
        if(cnt === i)
            {
                return i;
            }
    }
    return -1;
};
```

### 1609.  Even Odd Tree

[My Submissions](https://leetcode.com/contest/weekly-contest-209/problems/even-odd-tree/submissions/)[Back to Contest](https://leetcode.com/contest/weekly-contest-209/)

-   **User Accepted:**4234
-   **User Tried:**4576
-   **Total Accepted:**4314
-   **Total Submissions:**7898
-   **Difficulty:**Medium

A binary tree is named  **Even-Odd**  if it meets the following conditions:

-   The root of the binary tree is at level index  `0`, its children are at level index  `1`, their children are at level index  `2`, etc.
-   For every  **even-indexed**  level, all nodes at the level have  **odd**  integer values in  **strictly increasing**  order (from left to right).
-   For every  **odd-indexed**  level, all nodes at the level have  **even**  integer values in  **strictly decreasing**  order (from left to right).

Given the  `root`  of a binary tree,  _return_ `true` _if the binary tree is  **Even-Odd**, otherwise return_ `false`_._

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/09/15/sample_1_1966.png)**

**Input:** root = [1,10,4,3,null,7,9,12,8,6,null,null,2]
**Output:** true
**Explanation:** The node values on each level are:
Level 0: [1]
Level 1: [10,4]
Level 2: [3,7,9]
Level 3: [12,8,6,2]
Since levels 0 and 2 are all odd and increasing, and levels 1 and 3 are all even and decreasing, the tree is Even-Odd.

**My Java Solution:**
* BFS: level traverse
* Be careful to read and understand the title and constraints, waste more time here.
```
 public boolean isEvenOddTree(TreeNode root) {
        int level = 0;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty())
        {
            int size = q.size();
            int pre = level % 2 == 0 ? q.peek().val -1 : q.peek().val + 1;
            for(int i = 0; i<size; i++)
            {
                TreeNode cur = q.poll();
                int val = cur.val;
                 // System.out.println("cur val = " + val);
                if(level % 2 == 0)
                {
                    if(val % 2 == 0)
                    {
                        return false;
                    }
                    if(val <= pre)
                    {
                        // System.out.println("asc error at level: " + level + ", cur = " + val + ", pre = " + pre);
                        return false;
                    }
                }
                else
                {
                    if(val % 2 == 1)
                    {
                        return false;
                    }
                    if(val >= pre)
                    {
                        // System.out.println("desc error at level" + level + ", cur = " + val+ ", pre = " + pre);
                        return false;
                    }
                }
                pre = val;
                if(cur.left != null)
                {
                    q.offer(cur.left);
                }
                if(cur.right != null)
                {
                    q.offer(cur.right);
                }
            }
            // System.out.println("size= " + size+ ", level =" + level);
            level++;
        }
        return true;
    }
```
**How to write Javascript for BFS? First time thinking about this!!!**
**Good to try! pretty easy to write in JS and light weight**
* use Array operation to represent each level
```
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isEvenOddTree = function(root) {
    if(root === null)
        return false;
    // Same as Queue in Java
    let q = [[root, 0]];
    while(q.length)
    {
        const size = q.length;
        let cur = [];
        let level;
        for(let i = 0; i< size; i++)
        {
            const arr = q.shift();
            level  = arr[1];
            const node = arr[0];
            if(level % 2 == 0)
                {
                    if(node.val % 2 == 0)
                        {
                            return false;
                        }
                }
            else
                {
                    if(node.val % 2 == 1)
                        {
                            return false;
                        }
                }
            cur.push(node);
            if(node.left)
                {
                    q.push([node.left,level+1]);
                }
            
            if(node.right)
                {
                    q.push([node.right,level+1]);
                }
        }
        if(level % 2 == 0)
                {
                    if(!checkAsc(cur))
                        {
                            return false;
                        }
                }
        else
        {
            if(!checkDesc(cur))
            {
                 return false;
            }
        }
        
    }
    return true;
    
};

var checkAsc = function(arr)
{
    for(let i = 0; i<arr.length; i++)
        {
            if(i>0 && arr[i].val <= arr[i-1].val)
                {
                    return false;
                }
        }
    return true;
}
var checkDesc = function(arr)
{
    for(let i = 0; i<arr.length; i++)
        {
            if(i>0 && arr[i].val >= arr[i-1].val)
                {
                    return false;
                }
        }
    return true;
}
```

### [1610.  Maximum Number of Visible Points](https://leetcode.com/problems/maximum-number-of-visible-points/)

You are given an array  `points`, an integer  `angle`, and your  `location`, where  `location = [posx, posy]`  and  `points[i] = [xi, yi]`  both denote  **integral coordinates**  on the X-Y plane.

Initially, you are facing directly east from your position. You  **cannot move**  from your position, but you can  **rotate**. In other words,  `posx`  and  `posy`  cannot be changed. Your field of view in  **degrees**  is represented by  `angle`, determining how wide you can see from any given view direction. Let  `d`  be the amount in degrees that you rotate counterclockwise. Then, your field of view is the  **inclusive**  range of angles  `[d - angle/2, d + angle/2]`.

There can be multiple points at one coordinate. There may be points at your location, and you can always see these points regardless of your rotation. Points do not obstruct your vision to other points.

Return  _the maximum number of points you can see_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/30/89a07e9b-00ab-4967-976a-c723b2aa8656.png)

**Input:** points = [[2,1],[2,2],[3,3]], angle = 90, location = [1,1]
**Output:** 3
**Explanation:** The shaded region represents your field of view. All points can be made visible in your field of view, including [3,3] even though [2,2] is in front and in the same line of sight.

**Not solved in Contest, have one bug!!!**
**Solved after contest**
* use circular array to store the cross 360 to 0 degrees.
* use binary search to find the range
* attention to the **same origin point**: sometimes need to add to final result!!!
* use tangent to calculate degree between two points `[x1, y1]`
 to `[x0, y0]`
	 ```
	deg = Math.toDegrees(Math.atan2(y1 - y0, x1 - x0));
	```

[**My Java Solution after contest:**](https://leetcode.com/problems/maximum-number-of-visible-points/discuss/878068/Java-Circular-array-+-Binary-Search-Solution)
```
class Solution {
    int same = 0;
    public int visiblePoints(List<List<Integer>> points, int angle, List<Integer> location) {
        int loc[] = new int[]{location.get(0), location.get(1)};
        int n = points.size();
        double degs0[] = new double[n];
        for(int i = 0; i<n; i++ )
        {
            List<Integer> list =  points.get(i);
            int p[] = new int[]{list.get(0), list.get(1)};
            degs0[i] = calcDegree(loc, p);
        }
        Arrays.sort(degs0);
        
        double degs[] = new double[n*2];
        for(int i = 0; i<2*n; i++)
        {
            if(i>=n)
            {
                degs[i] = degs0[i%n] + 360;
            }
            else
            {
                degs[i] = degs0[i];
            }
        }
        
        int max = 0;
        for(int i = 0; i<n; i++)
        {
            double cur = degs0[i];
            if(i>0 && cur == degs0[i-1])
            {
                continue;
            }
            double target = cur + angle;
            int index = Arrays.binarySearch(degs, target+0.000001);
            if(index < 0)
                index = -index-1;
            index--;
            int k = index;
            int cnt = 0;
            if(k < i)
            {
                cnt = 0;
            }
            else
            {
               cnt = k-i+1;
            } 
            // Here: only the single circle and original search deg is not zero should have this composition!
            if( cur !=0 && k < n)
                cnt += same;

            max = Math.max(max, cnt); 
        }
        
        return max;
    }
    
    public double calcDegree(int[] p1, int[] p2)
    {
    
        double deg = -1;
        if(p1[0] == p2[0] && p1[1] == p2[1])
        {
            same++;
            return 0;
        }

        deg = Math.toDegrees(Math.atan2(p2[1] - p1[1], p2[0] - p1[0]));
        if(deg < 0)
            deg += 360;
       // System.out.println("deg = " + deg);
        return deg;
    }
}
```
**Solution2: Clean JavaScript Code ! So cool ! **
* Javascript calc degrees: [Math/atan2](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/atan2)
* Javascript sort array: [Array/sort](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
* Calculate degree more than 360 deg, save memory, use function
```
    var val = function(i){
        return i >= n ? degs[i % n] + 360 : degs[i % n];
    }
```
**Full Javascript solution: reference idea from [here](https://leetcode.com/problems/maximum-number-of-visible-points/discuss/878007/javascript-atan2-+-sliding-window)**
```
/**
 * @param {number[][]} points
 * @param {number} angle
 * @param {number[]} location
 * @return {number}
 */
var visiblePoints = function(points, angle, location) {
    var  calcAngleDegrees = function(x, y) {
        return Math.atan2(y, x) * 180 / Math.PI;
    }
    var degs = [];
    var same = 0;
    // process degree array
    for(const [p0, p1] of points)
    {
        if(p0 === location[0] && p1 === location[1])
            {
                same++;
            }
        else
        {
            var cur = calcAngleDegrees(p0-location[0],p1-location[1]);
            degs.push(cur);
        }
    }
    degs.sort((a,b)=> a-b);
    var n = points.length;
    var limit = 2*n;
    var res = 0;
    var val = function(i){
        return i >= n ? degs[i % n] + 360 : degs[i % n];
    }
    for(let i = 0; i<limit; i++)
    {
          let j = i;
          while(j < limit && val(j) - val(i) <= angle)
          {
                  j++;
          }
          var cnt = j-i;
          res = Math.max(cnt, res);
    }
    return res + same;
};
```


### 1611.  Minimum One Bit Operations to Make Integers Zero (Not solved)

Given an integer  `n`, you must transform it into  `0`  using the following operations any number of times:

-   Change the rightmost (`0th`) bit in the binary representation of  `n`.
-   Change the  `ith`  bit in the binary representation of  `n`  if the  `(i-1)th`  bit is set to  `1`  and the  `(i-2)th`  through  `0th`  bits are set to  `0`.

Return  _the minimum number of operations to transform_ `n` _into_ `0`_._

**Example 1:**
**Input:** n = 0
**Output:** 0

**Example 2:**
**Input:** n = 3
**Output:** 2
**Explanation:** The binary representation of 3 is "11".
"11" -> "01" with the 2nd operation since the 0th bit is 1.
"01" -> "00" with the 1st operation.

**Example 3:**
**Input:** n = 6
**Output:** 4
**Explanation:** The binary representation of 6 is "110".
"110" -> "010" with the 2nd operation since the 1st bit is 1 and 0th through 0th bits are 0.
"010" -> "011" with the 1st operation.
"011" -> "001" with the 2nd operation since the 0th bit is 1.
"001" -> "000" with the 1st operation.

**Analysis:**
* Have to find the rules:
when n = 1,2,3,4:
	- n = 001  -> min = 1
	- n = 010  -> min = 3
	- n = 011  -> min = 2
	- n = 100  -> min = 7

* Summary: rule1: the highest bit 1, 1xxxx needs (2^n -1), n is based on start index = 1.
* Solution reference: [https://leetcode.com/problems/minimum-one-bit-operations-to-make-integers-zero/discuss/877728/O(1)-JavaKotlin](https://leetcode.com/problems/minimum-one-bit-operations-to-make-integers-zero/discuss/877728/O(1)-JavaKotlin)

**Java solution:**
```
public int minimumOneBitOperations(int n) {
        int total = 0;
        int bit = 1;
        while(n!=0)
        {
            if( n % 2 == 1)
            {
                int cur = (1 << bit) -1 - total;
                total = cur;
            }
            bit++;
            n = n >> 1;
        }
        return total;
}
```
**Rewrite in JavaScript**
Reference: Javascript [Bitwise Operator](https://www.w3schools.com/js/js_bitwise.asp)
```
/**
 * @param {number} n
 * @return {number}
 */
var minimumOneBitOperations = function(n) {
    let total = 0;
    let bit = 1; // lowest bit index is starting from 1
    while(n)
    {
        // when lowest bit is 1
        if(n % 2 === 1)
        {
            let cur = (1 << bit) - 1 - total;
            total = cur;
        }
        bit++;
        n = n >> 1;
    }
    return total;
};
```

**Summary:**
4262 / 12138
Keep moving!
**Be careful about the rules!** Do not use test case to submit the solution!!!
**So good to try javascript !**