## Leetcode Contest 221 - JavaScript

I. [1704.  Determine if String Halves Are Alike](#question-1)

II. [1705. Maximum Number of Eaten Apples](#question-2)

III. [1706. Where Will the Ball Fall](#question-3)

IV. [1707. Maximum XOR With an Element From Array - Not finished](#question-4)

<div id="question-1"/>

### [1704.  Determine if String Halves Are Alike](https://leetcode.com/problems/determine-if-string-halves-are-alike/)

You are given a string  `s`  of even length. Split this string into two halves of equal lengths, and let  `a`  be the first half and  `b`  be the second half.

Two strings are  **alike**  if they have the same number of vowels (`'a'`,  `'e'`,  `'i'`,  `'o'`,  `'u'`,  `'A'`,  `'E'`,  `'I'`,  `'O'`,  `'U'`). Notice that  `s`  contains uppercase and lowercase letters.

Return  `true` _if_ `a` _and_ `b` _are  **alike**_. Otherwise, return  `false`.

**Example 1:**
**Input:** s = "book"
**Output:** true
**Explanation:** a = "bo" and b = "ok". a has 1 vowel and b has 1 vowel. Therefore, they are alike.

**Tips:**
- small, upper case in JavaScript: [toLowerCase()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase), [toUpperCase()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase)

**My JavaScript Solution:**
```
var halvesAreAlike = function(s) {
    s = s.toLowerCase();
    var n = s.length;
    var cnt = 0;
    for(var i = 0; i<n; i++)
    {
        var el = s[i];
        if(el === 'a' || el === 'e' || el==='o' ||el==='i' || el==='u')
        {
              if(i< Math.floor(n/2))
                  {
                      // cnt[el] = cnt[el] + 1;
                      cnt++;
                  }
            else
                {
                    // cnt[el] = cnt[el] - 1;
                    cnt--;
                }
        }
    }
    return cnt === 0 ? true : false;  
};
```

<div id="question-2"/>

### [1705. Maximum Number of Eaten Apples](https://leetcode.com/problems/maximum-number-of-eaten-apples)

There is a special kind of apple tree that grows apples every day for  `n`  days. On the  `ith`  day, the tree grows  `apples[i]`  apples that will rot after  `days[i]`  days, that is on day  `i + days[i]`  the apples will be rotten and cannot be eaten. On some days, the apple tree does not grow any apples, which are denoted by  `apples[i] == 0`  and  `days[i] == 0`.

You decided to eat  **at most**  one apple a day (to keep the doctors away). Note that you can keep eating after the first  `n`  days.

Given two integer arrays  `days`  and  `apples`  of length  `n`, return  _the maximum number of apples you can eat._

**Example 1:**
**Input:** apples = [1,2,3,5,2], days = [3,2,1,4,2]
**Output:** 7
**Explanation:** You can eat 7 apples:
- On the first day, you eat an apple that grew on the first day.
- On the second day, you eat an apple that grew on the second day.
- On the third day, you eat an apple that grew on the second day. After this day, the apples that grew on the third day rot.
- On the fourth to the seventh days, you eat apples that grew on the fourth day.

**Analysis:**
- priority queue in JavaScript: [datastructures-js/priority-queue](https://github.com/datastructures-js/priority-queue)
    ```
    import { MinPriorityQueue, MaxPriorityQueue } from '@datastructures-js/priority-queue';
    ```
- use array sorting to mock the priority queue
- always eat/consume the latest expired apple, **sort by expire date**.

**My JavaScript Solution:**
```js
var eatenApples = function(apples, days) {
    var n = apples.length;
    var remain = [];
    var cnt = 0;
    var i = 0;
    while(i<n || remain.length>0)
    {
        // grow apples
        if(i<n && apples[i] > 0)
        {
               remain.push([i+days[i]-1, apples[i]]);
               remain.sort((a,b)=>a[0]-b[0]);
        }
 
        // pop out expired apples
        while(remain.length>0 && (remain[0][1] === 0 || remain[0][0]< i))
        {
              remain.shift();
        }
         
        // eat an apple and count
        if(remain.length>0)
        {
            cnt++;
            remain[0][1] = remain[0][1]-1;
        }
        i++;
    }
    return cnt;   
};
```

**JavaScript Solution2: use DataStructure**

```js
var eatenApples = function(apples, days) {
    // sort by expire day
    const pq = new MinPriorityQueue({ priority: (el) => el[0] });
    var n = apples.length;
    var cnt = 0;
    var i = 0;
    while(i<n || !pq.isEmpty())
    {
        // grow apples
        if(i<n && apples[i] > 0)
        {
               pq.enqueue([i+days[i]-1, apples[i]]);
        }
 
        // pop out expired apples
        while(!pq.isEmpty() && (pq.front().element[1] === 0 || pq.front().element[0]< i))
        {
              pq.dequeue();
        }
         
        // eat an apple and count
        if(!pq.isEmpty())
        {
            cnt++;
            let cur =[...pq.dequeue().element];
            cur[1] = cur[1] - 1;
            if(cur[1] > 0)
            {
                pq.enqueue(cur);
            }
        }
        i++;
    }
    return cnt;   
};
```

<div id="question-3"/>

### [1706. Where Will the Ball Fall](https://leetcode.com/problems/where-will-the-ball-fall)

**Analysis:**
- summarize the diag two scenario
- each diagonal line has 3 cases

![image](../assets/lc1706.png ':size=581x398')

**JavaScript Solution after Contest:**
```
/**
 * @param {number[][]} grid
 * @return {number[]}
 */
var findBall = function(grid) {
    var m = grid.length;
    var n = grid[0].length;
    var res = [...Array(n)].fill(-1);
    // for each ball
    for(var j = 0 ; j<n; j++)
    {
        var x = 0;
        var y = j;
        while(x<m)
        {
            if(grid[x][y] === 1)
            {
                if(y === n-1)
                    {
                        // wall block
                        break;
                    }
                else if(grid[x][y+1] === -1)
                    {
                        // block
                        break;
                    }
                else
                    {
                        x++;
                        y++;
                    }
            }
            else
            {
                    if(y === 0)
                    {
                        // wall block
                        break;
                    }
                else if(grid[x][y-1] === 1)
                    {
                        // block
                        break;
                    }
                else
                    {
                        x++;
                        y--;
                    }
            }
        }
        if(x=== m)
                {
                    res[j] = y;
                }
    }
        return res;  
};
```

<div id="question-4"/>

### [1707. Maximum XOR With an Element From Array](https://leetcode.com/problems/maximum-xor-with-an-element-from-array)

Not finished