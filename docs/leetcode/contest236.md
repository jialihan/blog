## Leetcode Contest 236- JavaScript

#### I. [1822. Sign of the Product of an Array](#question-1)

#### II. [1823.  Find the Winner of the Circular Game](#question-2)

#### III. [1824.  Minimum Sideway Jumps](#question-3)

#### IV. [1825.  Finding MK Average - not solved](#question-4)

<div id="question-1"/>

### [1822. Sign of the Product of an Array](https://leetcode.com/contest/weekly-contest-235/problems/truncate-sentence/)

There is a function  `signFunc(x)`  that returns:

-   `1`  if  `x`  is positive.
-   `-1`  if  `x`  is negative.
-   `0`  if  `x`  is equal to  `0`.

You are given an integer array  `nums`. Let  `product`  be the product of all values in the array  `nums`.

Return  `signFunc(product)`.

**Example 1:**
**Input:** nums = [-1,-2,-3,-4,3,2,1]
**Output:** 1
**Explanation:** The product of all values in the array is 144, and signFunc(144) = 1

**JavaScript:** one line solution:
- `return` will NOT break the `forEach()` loop
```js
var arraySign = function(nums) {
    var sign = 1;
    nums.forEach(num=>{
        if(num <0)
        {
            sign *= -1;
        }
        else if(num === 0)
        {
            // fail: Javascript: forEach : a return will not exit the calling function
            // return 0;
            sign = 0;
        }
    });
    return sign; 
};
```

Reference discussion article here: [Javascript simple solution](https://leetcode.com/problems/sign-of-the-product-of-an-array/discuss/1153074/JavaScript-Simple-solution)

<div id="question-2"/>

### [1823.  Find the Winner of the Circular Game](https://leetcode.com/problems/find-the-winner-of-the-circular-game/)

There are  `n`  friends that are playing a game. The friends are sitting in a circle and are numbered from  `1`  to  `n`  in  **clockwise order**. More formally, moving clockwise from the  `ith`  friend brings you to the  `(i+1)th`  friend for  `1 <= i < n`, and moving clockwise from the  `nth`  friend brings you to the  `1st`  friend.

The rules of the game are as follows:

1.  **Start**  at the  `1st`  friend.
2.  Count the next  `k`  friends in the clockwise direction  **including**  the friend you started at. The counting wraps around the circle and may count some friends more than once.
3.  The last friend you counted leaves the circle and loses the game.
4.  If there is still more than one friend in the circle, go back to step  `2`  **starting**  from the friend  **immediately clockwise**  of the friend who just lost and repeat.
5.  Else, the last friend in the circle wins the game.

Given the number of friends,  `n`, and an integer  `k`, return  _the winner of the game_.

**JavaScript solution:**
```js
var findTheWinner = function(n, k) {
    let arr = [];
    for(var i = 1; i<=n; i++)
    {
        arr.push(i);
    }
    var start = 0;
    while(arr.length>1)
    {
        n = arr.length; // lenght of array will change since we delete the array in each round
        var index =(start + k -1) % n; // the index to delete
        arr = arr.filter((el,i)=>i !== index);
        start =index; // since delete one number,  next starting index === index
    }
    return arr[0];
};
```

<div id="question-3"/>

### [1824.  Minimum Sideway Jumps](https://leetcode.com/problems/minimum-sideway-jumps/)


There is a  **3 lane road**  of length  `n`  that consists of  `n + 1`  **points**  labeled from  `0`  to  `n`. A frog  **starts**  at point  `0`  in the  **second** lane  and wants to jump to point  `n`. However, there could be obstacles along the way.

You are given an array  `obstacles`  of length  `n + 1`  where each  `obstacles[i]`  (**ranging from 0 to 3**) describes an obstacle on the lane  `obstacles[i]`  at point  `i`. If  `obstacles[i] == 0`, there are no obstacles at point  `i`. There will be  **at most one**  obstacle in the 3 lanes at each point.

-   For example, if  `obstacles[2] == 1`, then there is an obstacle on lane 1 at point 2.

The frog can only travel from point  `i`  to point  `i + 1`  on the same lane if there is not an obstacle on the lane at point  `i + 1`. To avoid obstacles, the frog can also perform a  **side jump**  to jump to  **another**  lane (even if they are not adjacent) at the  **same**  point if there is no obstacle on the new lane.

-   For example, the frog can jump from lane 3 at point 3 to lane 1 at point 3.

Return _the  **minimum number of side jumps**  the frog needs to reach  **any lane**  at point n starting from lane  `2`  at point 0._

**Note:**  There will be no obstacles on points  `0`  and  `n`.

**JS Solution:**
```js
var minSideJumps = function(obstacles) {
    var res = 0;
    var dp = new Array(4).fill(0);
    var n = obstacles.length -1;
    dp[1] = 1;
    dp[3] = 1; 
    for(var i = 1; i<=n; i++)
    {
        var ob = obstacles[i];
        var cur = new Array(4).fill(0);
        for(var lane = 1; lane<=3; lane++)
        {
            if(ob === lane)
            {
                // when current lane is the obstacle, cannot reach
                cur[lane] = Infinity;
            }
            else
            {
                // compute current min jump from all possible prev lane
                var min = Infinity;
                for(var l = 1; l<=3; l++)
                {   
                    // cannot jump when prev === current obstacle OR prev lane cannot reach
                    if(l === ob || dp[l] === Infinity)
                    {
                        continue;
                    }
                    if(l === lane)
                    {
                        cur[l] = dp[l];
                    }
                    else
                    {
                       cur[l] = dp[l] + 1;
                    }
                    min = Math.min(min, cur[l]);
                }
                cur[lane] = min;
            }
            dp[lane] = cur[lane];
        }
    }
    dp.shift();
    return Math.min(...dp);
};
```

<div id="question-4" />

### [1825.  Finding MK Average](https://leetcode.com/problems/finding-mk-average/)

- Not solved
- Same Google interview problem: https://leetcode.com/discuss/interview-question/1094948/find-out-mk-average-of-integer-stream