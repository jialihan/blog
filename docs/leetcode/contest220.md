##  Leetcode Contest 220 - JavaScript

I.  [1694. Reformat Phone Number](#question-1)

II. [1695. Maximum Erasure Value](#question-2)

III. [1696. Jump Game VI](#question-3)

IV. [1697. Checking Existence of Edge Length Limited Paths](#question-4)

V. [Summary](#summary)


<div id="question-1"/>

### [1694. Reformat Phone Number](https://leetcode.com/problems/reformat-phone-number/)

You are given a phone number as a string  `number`.  `number`  consists of digits, spaces  `' '`, and/or dashes  `'-'`.

You would like to reformat the phone number in a certain manner. Firstly,  **remove**  all spaces and dashes. Then,  **group**  the digits from left to right into blocks of length 3  **until**  there are 4 or fewer digits. The final digits are then grouped as follows:

-   2 digits: A single block of length 2.
-   3 digits: A single block of length 3.
-   4 digits: Two blocks of length 2 each.

The blocks are then joined by dashes. Notice that the reformatting process should  **never**  produce any blocks of length 1 and produce  **at most**  two blocks of length 2.

Return  _the phone number after formatting._

**Example 1:**
**Input:** number = "1-23-45 6"
**Output:** "123-456"
**Explanation:** The digits are "123456".
Step 1: There are more than 4 digits, so group the next 3 digits. The 1st block is "123".
Step 2: There are 3 digits remaining, so put them in a single block of length 3. The 2nd block is "456".
Joining the blocks gives "123-456".


**My JavaScript Solution:**
- string manipulation
- consider two situation: last 4 digits or last 5 digits
```
var reformatNumber = function(number) {
    var nums = [];
    for(var c of [...number])
    {
        if (c >= '0' && c <= '9') {
            nums.push(c);   
        }     
    }
    var s = nums.join('');
    
    var res = [];
    var lastIndex=0;
    var len = s.length;
    for(var i = 0; i<len; i += 3)
    {
        if(len - i === 4  || len-i === 2)
        {
            lastIndex = i;
            break;
        }
        res.push(s.slice(i,i+3));
    }

    if(  len - lastIndex === 4)
        {
            res.push(s.substring(lastIndex, lastIndex+2));
            res.push(s.substring(lastIndex+2, len));
        }
    else if( len - lastIndex === 2)
        {
            res.push(s.substring(lastIndex, len));
        }
   
    return res.join('-');
   
};
```


<div id="question-2"/>

### [1695.  Maximum Erasure Value](https://leetcode.com/problems/maximum-erasure-value/)

You are given an array of positive integers  `nums`  and want to erase a subarray containing **unique elements**. The  **score**  you get by erasing the subarray is equal to the  **sum**  of its elements.

Return  _the  **maximum score**  you can get by erasing  **exactly one**  subarray._

An array  `b`  is called to be a  subarray  of  `a`  if it forms a contiguous subsequence of  `a`, that is, if it is equal to  `a[l],a[l+1],...,a[r]`  for some  `(l,r)`.

**Example 1:**
**Input:** nums = [4,2,4,5,6]
**Output:** 17
**Explanation:** The optimal subarray here is [2,4,5,6].

**Example 2:**
**Input:** nums = [5,2,1,2,5,2,1,2,5]
**Output:** 8
**Explanation:** The optimal subarray here is [5,2,1] or [1,2,5].

**My JavaScript  Solution:** two pointer
- Sliding window
- similar to previous problem: 
```
/**
 * @param {number[]} nums
 * @return {number}
 */
var maximumUniqueSubarray = function(nums) {
    var cnt = [...Array(10001)].fill(0);
    var res = 0;
    var start = 0;
    var cur = 0;
    for(var i=0;i< nums.length;i++)
    {
        while(cnt[nums[i]] > 0)
        {
           cur -= nums[start];
           cnt[nums[start++]]--;
         
        }
        cur += nums[i];   
        cnt[nums[i]]++;
        res = Math.max(cur, res);
    }
    
    return res;
};
```

<div id="question-3"/>

### [1696.  Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

You are given a  **0-indexed**  integer array  `nums`  and an integer  `k`.

You are initially standing at index  `0`. In one move, you can jump at most  `k`  steps forward without going outside the boundaries of the array. That is, you can jump from index  `i`  to any index in the range  `[i + 1, min(n - 1, i + k)]`  **inclusive**.

You want to reach the last index of the array (index  `n - 1`). Your  **score**  is the  **sum**  of all  `nums[j]`  for each index  `j`  you visited in the array.

Return  _the  **maximum score**  you can get_.

**Example 1:**
**Input:** nums = [1,-1,-2,4,-7,3], k = 2
**Output:** 7
**Explanation:** You can choose your jumps forming the subsequence [1,-1,4,3] (underlined above). The sum is 7.

**Example 2:**
**Input:** nums = [10,-5,-2,4,0,3], k = 3
**Output:** 17
**Explanation:** You can choose your jumps forming the subsequence [10,4,3] (underlined above). The sum is 17.

**Tips:**
- greedy + dp: scanning from begin to end
- similar problem:  [1425. Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum/)


**My JavaScript Solution：**
```
var maxResult = function(nums, k) {
    var n = nums.length;
    var dp = [...Array(n)].fill(-Infinity);
    dp[0] = nums[0];
    
    for(var i = 0 ; i<nums.length; i++)
    {
        for(var j = 1; j <= k && j+i < n; j++)
        {
            dp[i+j] = Math.max(dp[i+j], dp[i]+nums[i+j]);
            if(nums[i+j] > 0)
                {
                    break;
                }
        }
    }
    return dp[n-1];  
};
```

<div id="question-4"/>

### [1697.  Checking Existence of Edge Length Limited Paths](https://leetcode.com/problems/checking-existence-of-edge-length-limited-paths/)

An undirected graph of  `n`  nodes is defined by  `edgeList`, where  `edgeList[i] = [ui, vi, disi]`  denotes an edge between nodes  `ui`  and  `vi`  with distance  `disi`. Note that there may be  **multiple**  edges between two nodes.

Given an array  `queries`, where  `queries[j] = [pj, qj, limitj]`, your task is to determine for each  `queries[j]`  whether there is a path between  `pj`  and  `qj`  such that each edge on the path has a distance  **strictly less than**  `limitj`  .

Return  _a  **boolean array**_ `answer`_, where_ `answer.length == queries.length`  _and the_ `jth`  _value of_ `answer`  _is_ `true` _if there is a path for_ `queries[j]` _is_ `true`_, and_ `false` _otherwise_.

**My Javascript Solution:**
- Union Find: create a DSU class
- sort edges by weight, **ascending** order
- adding valid edges by weight limit, asce**strong text**nding order

Template **DSU** class (JavaScript): with rank
```js
class  DSU {
	constructor(n) {
		this.p = [...Array(n)];
		this.rank = [...Array(n)].fill(1);
		for (let  i = 0; i < n; i++) {
			this.p[i] = i;
		}
	}

	findParent(x) {
		if (this.p[x] !== x) {
			this.p[x] = this.findParent(this.p[x]);
		}
		return  this.p[x];
	}

	union(x, y) {
		const  px = this.findParent(x);
		const  py = this.findParent(y);
		if (px !== py) {
			if (this.rank[px] >= this.rank[py]) {
				this.p[py] = px;
				this.rank[px]++;
			}
			else {
				this.p[px] = py;
				this.rank[py]++;
			}
		}
	}
	
	isConnected(x, y) {
		return  this.findParent(x) === this.findParent(y);
	}
}
```

This problem's solution:

```js
var  distanceLimitedPathsExist = function (n, edgeList, queries) {
	// sort edges and queires by weight ascending
	edgeList.sort((a, b) => {
		return  a[2] - b[2];
	})
	queries = queries.map((el, i) => [...el, i]).sort((a, b) =>  a[2] - b[2]);
	
	// union-find
	const  dsu = new  DSU(n);
	let  res = Array(queries.length);
	for (let  i = 0; i < queries.length; i++) {
	const  limit = queries[i][2];
	while (edgeList.length > 0 && edgeList[0][2] < limit) {
		const  edge = edgeList.shift();
		dsu.union(edge[0], edge[1]);
	}
	if (dsu.isConnected(queries[i][0], queries[i][1])) {
		res[queries[i][3]] = true;
	}
	else {
		res[queries[i][3]] = false;
	}
	}
	return  res;
};
```


<div id="summary"/>

### Summary
- get familiar with **Sliding window**, and **counting sort**
- javascript string operation: `slice()`, `str[i]`
- Max of an array: `Math.max(...array)`
- DSU: javascript ES6 class
-  only finish 1 problem in this contest
- keep moving
