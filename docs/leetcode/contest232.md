## Leetcode Contest 232 - JavaScript

#### I. [1790. Check if One String Swap Can Make Strings Equal](#question-1)

#### II. [1791. Find Center of Star Graph](#question-2)

#### III. [1792. Maximum Average Pass Ratio](#question-3)

#### IV. [1793. Maximum Score of a Good Subarray](#question-4)

#### V. [Summary](#summary)


<div id="question-1"/>

### [1790. Check if One String Swap Can Make Strings Equal](https://leetcode.com/problems/check-if-one-string-swap-can-make-strings-equal/)

You are given two strings  `s1`  and  `s2`  of equal length. A  **string swap**  is an operation where you choose two indices in a string (not necessarily different) and swap the characters at these indices.

Return  `true`  _if it is possible to make both strings equal by performing  **at most one string swap** on  **exactly one**  of the strings._ Otherwise, return  `false`.

**Example 1:**
**Input:** s1 = "bank", s2 = "kanb"
**Output:** true
**Explanation:** For example, swap the first character with the last character of s2 to make "bank".

**Example 2:**
**Input:** s1 = "attack", s2 = "defend"
**Output:** false
**Explanation:** It is impossible to make them equal with one string swap.

```js
var areAlmostEqual = function(s1, s2) {
    if(s1===s2)
        return true;
    var first = undefined, second = undefined;
    for(var i=0; i<s1.length; i++)
    {
        if(s1[i] !== s2[i])
        {
            if(first === undefined)
            {
                first = i;
            }
            else if(second === undefined){
                second = i;
            }
            else
            {
                return false;
            }
        }
    }
    if(first>=0  && second>=0 && s1[first] === s2[second] && s1[second]=== s2[first] )
    {
            return true;      
    }
    return false;
};
```

Reference discussion article here: [Javascript solution](https://leetcode.com/problems/check-if-one-string-swap-can-make-strings-equal/discuss/1108853/JavaScript-Swap-char)

<div id="question-2"/>

### [1791. Find Center of Star Graph](https://leetcode.com/problems/find-center-of-star-graph/)

There is an undirected  **star**  graph consisting of  `n`  nodes labeled from  `1`  to  `n`. A star graph is a graph where there is one  **center**  node and  **exactly**  `n - 1`  edges that connect the center node with every other node.

You are given a 2D integer array  `edges`  where each  `edges[i] = [ui, vi]`  indicates that there is an edge between the nodes  `ui`  and  `vi`. Return the center of the given star graph.

**Javascript solution:**
use `map()` to build adjacent list	
```js
var findCenter = function(edges) {
    var n = edges.length + 1;
    var map = new Map();
    for(var e of edges)
    {
        if(!map.has(e[0]))
        {
            map.set(e[0], []);
        }
        if(!map.has(e[1]))
        {
            map.set(e[1], []);
        }
        map.get(e[0]).push(e[1]);
        map.get(e[1]).push(e[0]);
    }
    for(var [k,v] of map.entries())
    {
        if(v.length === n-1)
        {
            return k;
        }
    }
    return -1; // no center  
};
```

See the discussion article here: [JavaScript: map + adjacent-list](https://leetcode.com/problems/find-center-of-star-graph/discuss/1108868/JavaScript-Map-%2B-Adjacent-List)

<div id="question-3"/>

### [1792. Maximum Average Pass Ratio](https://leetcode.com/problems/maximum-average-pass-ratio/)

There is a school that has classes of students and each class will be having a final exam. You are given a 2D integer array  `classes`, where  `classes[i] = [passi, totali]`. You know beforehand that in the  `ith`  class, there are  `totali`  total students, but only  `passi`  number of students will pass the exam.

You are also given an integer  `extraStudents`. There are another  `extraStudents`  brilliant students that are  **guaranteed**  to pass the exam of any class they are assigned to. You want to assign each of the  `extraStudents`  students to a class in a way that  **maximizes**  the  **average**  pass ratio across  **all**  the classes.

The  **pass ratio**  of a class is equal to the number of students of the class that will pass the exam divided by the total number of students of the class. The  **average pass ratio**  is the sum of pass ratios of all the classes divided by the number of the classes.

Return  _the  **maximum**  possible average pass ratio after assigning the_ `extraStudents` _students._ Answers within  `10-5`  of the actual answer will be accepted.

**Example 1:**
**Input:** classes = [[1,2],[3,5],[2,2]], `extraStudents` = 2
**Output:** 0.78333
**Explanation:** You can assign the two extra students to the first class. The average pass ratio will be equal to (3/4 + 3/5 + 2/2) / 3 = 0.78333.

**Analysis:**
-   how to sort?  
    Since we need to get the sum of max_ratio, then everytime, we need to try to get max-increased ratio value:
    
    ```
    increased_ratio_diff = (class[0] + 1 / class[1] + 1) - (class[0] / class[1]);
    
    ```
    
-   increase 1 on the peek/front class of the MaxPriorityQueue

#### See the discussion article here:[Javascript: Priority Queue](https://leetcode.com/problems/maximum-average-pass-ratio/discuss/1108438/JavaScript-Priority-Queue)

<div id="question-4" />

### [1793. Maximum Score of a Good Subarray](https://leetcode.com/problems/maximum-score-of-a-good-subarray/)

You are given an array of integers  `nums`  **(0-indexed)**  and an integer  `k`.

The  **score**  of a subarray  `(i, j)`  is defined as  `min(nums[i], nums[i+1], ..., nums[j]) * (j - i + 1)`. A  **good**  subarray is a subarray where  `i <= k <= j`.

Return  _the maximum possible  **score**  of a  **good**  subarray._

**Example 1:**
**Input:** nums = [1,4,3,7,4,5], k = 3
**Output:** 15
**Explanation:** The optimal subarray is (1, 5) with a score of min(4,3,7,4,5) * (5-1+1) = 3 * 5 = 15. 

**Example 2:**
**Input:** nums = [5,5,4,5,4,1,1,1], k = 0
**Output:** 20
**Explanation:** The optimal subarray is (0, 4) with a score of min(5,5,4,5,4) * (4-0+1) = 4 * 5 = 20.

Solution1: 
```js
var maximumScore = function(nums, k) {
    var l = k, r = k;
    var min = nums[k], res = nums[k];
    var n = nums.length;
    for(;min>=1; min--)
    {
        while(l>0 && nums[l-1] >= min)
        {
            l--;
        }
        while(r<n-1 && nums[r+1] >= min)
        {
            r++;
        }
        res = Math.max(res, (r - l + 1) * min);    
    }
    return res; 
};
```

Solution2:
```js
var maximumScore = function(nums, k) {
    var l = k, r = k;
    var min = nums[k], res = nums[k];
    var n = nums.length;
    while(l>0 || r<n-1)
    {
        if(l>0 && nums[l-1]>=min)
        {
            l--;
        }
        else if(r<n-1 && nums[r+1]>=min)
        {
            r++;
        }
        else
        {
            var ll=-1,rr=-1;
            if(l>0)
            {
                ll=nums[l-1];
            }
            if(r<n-1)
            {
                rr = nums[r+1];
            }
            if(ll>rr)
            {
                l--;
                min=ll;
            }
            else
            {
                r++;
                min=rr;
            }        
        }
        res = Math.max(res, (r-l+1)*min);
    }
    return res;
};
```

see the discussion article here: [two-pointers](https://leetcode.com/problems/maximum-score-of-a-good-subarray/discuss/1108544/JavaScript-Two-Pointers)

<div id="summary"/>

### Summary
- Q3 - how to sort the next max_ratio_increased in PQ
- Q4:  two pointer, consider the `next_left_min & next_ right_min`


