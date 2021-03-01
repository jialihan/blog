## Leetcode Contest 230 - JavaScript

#### I.  [1773. Count Items Matching a Rule](#question-1)

#### II. [1774.  Closest Dessert Cost](#question-2)

#### III. [1775.  Equal Sum Arrays With Minimum Number of Operations](#question-3)

#### IV. [1776. Car Fleet II - not solved in contest](#question-4)
- [853. Car Fleet](#q4-1)
- [1776. Car Fleet II](#q4-2)

#### V. [Summary](#summary)


<div id="question-1"/>

### [1773. Count Items Matching a Rule](https://leetcode.com/problems/count-items-matching-a-rule/)

You are given an array  `items`, where each  `items[i] = [typei, colori, namei]`  describes the type, color, and name of the  `ith`  item. You are also given a rule represented by two strings,  `ruleKey`  and  `ruleValue`.

The  `ith`  item is said to match the rule if  **one**  of the following is true:

-   `ruleKey == "type"`  and  `ruleValue == typei`.
-   `ruleKey == "color"`  and  `ruleValue == colori`.
-   `ruleKey == "name"`  and  `ruleValue == namei`.

Return  _the number of items that match the given rule_.

**Example 1:**
**Input:** items = [["phone","blue","pixel"],["computer","silver","lenovo"],["phone","gold","iphone"]], ruleKey = "color", ruleValue = "silver"
**Output:** 1
**Explanation:** There is only one item matching the given rule, which is ["computer","silver","lenovo"].

**JavaScript Solution:  switch**
```js
var countMatches = function(items, ruleKey, ruleValue) {
    var k = -1;
    var cnt = 0;
    switch(ruleKey)
    {
        case 'type':
            k=0;
            break;
        case 'color':
            k=1;
            break;
        case 'name':
            k=2;
            break;
        default:
            k=1;
    }
    items.forEach(el=>{
        if(el[k] === ruleValue)
        {
            cnt++;
        }
    });
    return cnt;
};
```

<div id="question-2"/>

### [1774.  Closest Dessert Cost](https://leetcode.com/problems/closest-dessert-cost/)
You would like to make dessert and are preparing to buy the ingredients. You have  `n`  ice cream base flavors and  `m`  types of toppings to choose from. You must follow these rules when making your dessert:

-   There must be  **exactly one**  ice cream base.
-   You can add  **one or more**  types of topping or have no toppings at all.
-   There are  **at most two**  of  **each type**  of topping.

You are given three inputs:

-   `baseCosts`, an integer array of length  `n`, where each  `baseCosts[i]`  represents the price of the  `ith`  ice cream base flavor.
-   `toppingCosts`, an integer array of length  `m`, where each  `toppingCosts[i]`  is the price of  **one**  of the  `ith`  topping.
-   `target`, an integer representing your target price for dessert.

You want to make a dessert with a total cost as close to  `target`  as possible.

Return  _the closest possible cost of the dessert to_ `target`. If there are multiple, return  _the  **lower**  one._

**Example 1:**
**Input:** baseCosts = [1,7], toppingCosts = [3,4], target = 10
**Output:** 10
**Explanation:** Consider the following combination (all 0-indexed):
- Choose base 1: cost 7
- Take 1 of topping 0: cost 1 x 3 = 3
- Take 0 of topping 1: cost 0 x 4 = 0
Total: 7 + 3 + 0 = 10.

**Analysis:**
- how to generate all combinations ? pick 0, 1 or 2
- binary search or bruteforce, think too much in contest
- just try all combinations, no binary search, waste so many time.

**Key point:**
How to generate pick 0, 1, 2?
- DFS
- bitmask: but i only think up the pick 1 or 0, then I do another loop to add the result with itself, then it becomes 0, 1,2 toppings.
	```js
	 // cost is the array: all possible pick 0 or 1 toppings
	 for(var c1 of cost)
        for(var c2 of cost)
        {
	         var curCost = c1 + c2;
        }
	```
	
**Javascript solution:**
see the discussion article here: [all_combinations + bitmask](https://leetcode.com/problems/closest-dessert-cost/discuss/1085934/JavaScript-all-combinations-+-bitmask)


<div id="q2-2" />

#### 2.2  Optimized Solution: PreSum - O(n)

Scan from left to right, then from right to left, calculate preSum, then reduce time to O(n).

```js
var minOperations = function(boxes) {
    var n = boxes.length;
    var res = new Array(n).fill(0);
    var sum = 0;
    var preBalls = 0;
    for(var i = 0; i<n; i++)
    {
        res[i] += sum;
        if(boxes[i] === '1')
        {
            preBalls++;
        }
        sum += preBalls;
    }
    sum = 0;
    preBalls = 0;
    for(var i = n-1; i>=0; i--)
    {
        res[i] += sum;
        if(boxes[i] === '1')
        {
            preBalls++;
        }
        sum += preBalls;
    } 
    return res;  
};
```

<div id="question-3"/>

### 1775.  Equal Sum Arrays With Minimum Number of Operations

You are given two arrays of integers  `nums1`  and  `nums2`, possibly of different lengths. The values in the arrays are between  `1`  and  `6`, inclusive.

In one operation, you can change any integer's value in  **any** of the arrays to  **any**  value between  `1`  and  `6`, inclusive.

Return  _the minimum number of operations required to make the sum of values in_ `nums1` _equal to the sum of values in_ `nums2`_._  Return  `-1`​​​​​ if it is not possible to make the sum of the two arrays equal.

**Example 1:**
**Input:** nums1 = [1,2,3,4,5,6], nums2 = [1,1,2,2,2,2]
**Output:** 3
**Explanation:** You can make the sums of nums1 and nums2 equal with 3 operations. All indices are 0-indexed.
- Change nums2[0] to 6. nums1 = [1,2,3,4,5,6], nums2 = [**6**,1,2,2,2,2].
- Change nums1[5] to 1. nums1 = [1,2,3,4,5,**1**], nums2 = [6,1,2,2,2,2].
- Change nums1[2] to 2. nums1 = [1,2,**2**,4,5,1], nums2 = [6,1,2,2,2,2].

**Example 2:**
**Input:** nums1 = [1,1,1,1,1,1,1], nums2 = [6]
**Output:** -1
**Explanation:** There is no way to decrease the sum of nums1 or to increase the sum of nums2 to make them equal.

**JavaScript solution:**

see the discussion article here:  [Greedy + merging two array](https://leetcode.com/problems/equal-sum-arrays-with-minimum-number-of-operations/discuss/1085862/JavaScript-Greedy-similar-like-merging-two-array)

<div id="question-4" />

### [1776. Car Fleet II](https://leetcode.com/problems/car-fleet-ii/)

There are  `n`  cars traveling at different speeds in the same direction along a one-lane road. You are given an array  `cars`  of length  `n`, where  `cars[i] = [positioni, speedi]`  represents:

-   `positioni`  is the distance between the  `ith`  car and the beginning of the road in meters. It is guaranteed that  `positioni  < positioni+1`.
-   `speedi`  is the initial speed of the  `ith`  car in meters per second.

For simplicity, cars can be considered as points moving along the number line. Two cars collide when they occupy the same position. Once a car collides with another car, they unite and form a single car fleet. The cars in the formed fleet will have the same position and the same speed, which is the initial speed of the  **slowest**  car in the fleet.

Return an array  `answer`, where  `answer[i]`  is the time, in seconds, at which the  `ith`  car collides with the next car, or  `-1`  if the car does not collide with the next car. Answers within  `10-5`  of the actual answers are accepted.

**Example 1:**
**Input:** cars = [[1,2],[2,1],[4,3],[7,2]]
**Output:** [1.00000,-1.00000,3.00000,-1.00000]
**Explanation:** After exactly one second, the first car will collide with the second car, and form a car fleet with speed 1 m/s. After exactly 3 seconds, the third car will collide with the fourth car, and form a car fleet with speed 2 m/s.

**Example 2:**
**Input:** cars = [[3,4],[5,4],[6,3],[9,1]]
**Output:** [2.00000,1.00000,1.50000,-1.00000]

<div id="q4-1" />

#### 4.1 Previous Problem:  [853. Car Fleet](https://leetcode.com/problems/car-fleet/)

- sort by position
- checking from the last car to previous: `n-1` to `0`

See the discussion article here: [Javascript - sort](https://leetcode.com/problems/car-fleet/discuss/1087261/Javascript-sort)

<div id="q4-2" />

#### 4.2 LC 1776. Car Fleet II

Consider two cases, comparing cars from last one to the previous, `n-2` to `0`, the last car `n-1` will never collide, time is always `-1` to the `(n-1) th car`.

![image](../assets/lc1776.png ':size=688x422')

**JavaScript Solution:**

```js
var getCollisionTimes = function(cars) {
    var queue = [];
    var n = cars.length;
    queue.unshift(n-1);
    var time = new Array(n).fill(-1); 
    for(var i = n-2; i>=0; i--)
    {
        // case1: poll out cars that never collides with current car
        while(queue.length>0 && cars[i][1]<= cars[queue[0]][1])
        {
            queue.shift();
        }
        
        // case2: cur car will collide with lator cars in queue
        while(queue.length>0)
        {
            var next = queue[0];
            time[i] = (cars[next][0] - cars[i][0] ) / (cars[i][1] - cars[next][1]);
            time[i].toFixed(5);
            if(time[next] != -1 && time[i] >= time[next])
            {
                queue.shift();
            }
            else
            {
                break;
            }
        }
        queue.unshift(i);
    }
    return time;
};
```

See discussion article here: 
[JavaScript: Queue + explanation](https://leetcode.com/problems/car-fleet-ii/discuss/1087333/JavaScript-Queue-%2B-explanation)

<div id="summary"/>

### Summary
- Q2 - waste a lot of time, don't think complicated Binary search, try brute force first.
- Q3: passed at the 8:00pm, but not get calculated in my score, pitful !!!
- 3467 / 11654, keep moving!
