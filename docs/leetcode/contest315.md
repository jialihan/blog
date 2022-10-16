## Leetcode Contest 315 - JavaScript

#### I. [2441. Largest Positive Integer That Exists With Its Negative](#question-1)

#### II. [2442. Count Number of Distinct Integers After Reverse Operations](#question-2)

#### III. [2443. Sum of Number and Its Reverse](#question-3)

#### IV. [2444. Count Subarrays With Fixed Bounds - not solved](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [2441. Largest Positive Integer That Exists With Its Negative](https://leetcode.com/problems/largest-positive-integer-that-exists-with-its-negative/)

Solution:

```
var findMaxK = function(nums) {
    let max = 0;
    const set = new Set();
    const pos = [];
    for(const num of nums) {
        if(num<0){
            set.add(num);
        }
        else {
            pos.push(num);
        }
    }
    for(const num of pos) {
        if(num > max && set.has(-num)) {
            max = num;
        }
    }
    return max === 0 ? -1 : max;
};
```

<div  id="question-2"/>

### [2442. Count Number of Distinct Integers After Reverse Operations](https://leetcode.com/problems/count-number-of-distinct-integers-after-reverse-operations/)

Solution: [link](https://leetcode.com/problems/count-number-of-distinct-integers-after-reverse-operations/discuss/2708457/JavaScript-Set-%2B-reverse-string)

```
var countDistinctIntegers = function(nums) {
    const set = new Set();
    for(const num of nums) {
        set.add(num);
    }
    if(set.size === 1 && nums.length> 1) {
        return 1;
    }
    for(const num of nums) {
        const rev = parseInt(reverseString(num+''));
        set.add(rev);
    }
    return set.size;
};
function reverseString(str) {
    return str.split("").reverse().join("");
}
```

<div  id="question-3"/>

### [2443. Sum of Number and Its Reverse](https://leetcode.com/problems/sum-of-number-and-its-reverse/)

Solution: [link](https://leetcode.com/problems/sum-of-number-and-its-reverse/discuss/2708478/JavaScript-brute-force-passed-O%28n%29)

```
var sumOfNumberAndReverse = function(num) {
    if(num === 0) {
        return true;
    }
    for(let i = 1; i< num; i++) {
        const sum = i + parseInt(reverseString(i+''));
        if(sum === num) {
            return true;
        }
    }
    return false;
};
function reverseString(str) {
    return str.split("").reverse().join("");
}
```

<div  id="question-4"  />

### [2444. Count Subarrays With Fixed Bounds](https://leetcode.com/problems/count-subarrays-with-fixed-bounds/)

**Wrong solution: **
Timeout solution `O(n^2)`, loop each len of subarray:

```
var countSubarrays = function(nums, minK, maxK) {
    const n = nums.length;
    let ans = 0;
    let res = [];
    if(minK===maxK) {
        for(let i = 0; i<n; i++) {
            if(nums[i] === minK) {
                ans++;
            }
        }
    }
    for(let len = 2; len<=n ;len++){
       const q = [];
       let max = 0;
       let min = Infinity;
       for(let i = 0; i<n; i++) {
           if(i<len) {
               q.push(nums[i]);
               if(max < nums[i]) {
                   max = nums[i]
               }
               if(min > nums[i]) {
                   min = nums[i]
               }
               if(i===len-1) {
                   if(max === maxK && min === minK) {
                       ans++;
                   }
               }
           }
           else {
               const removed = q.shift();
               q.push(nums[i]);

               if(max === removed || min=== removed || nums[i] > max || nums[i] < min) {
                   // find next Max
                   max = Math.max(...q);
                   min = Math.min(...q);
               }

               if(max === maxK && min === minK) {
                     res.push(q.slice());
                     ans++;
               }
           } // end else
       }
    }

    return ans;
};
```

**Correct Solution: O(n) **

```
var countSubarrays = function(nums, minK, maxK) {
    const n = nums.length;
    let ans = 0;
    let min = -1, max = -1;
    let start = 0, end = 0;
    let hasMin = -1, hasMax= -1; // index of
    for(let i = 0; i<n; i++) {
        if(nums[i] < minK || nums[i] > maxK) {
            start = i+1;
            hasMin = -1;
            hasMax = -1;
        }
        else {
            if(nums[i] === minK) hasMin = i;
            if(nums[i] === maxK) hasMax = i;
            if(hasMin >= 0 && hasMax >=0) {
                ans += Math.min(hasMin, hasMax)+1;
            }
        }

    }
    return ans;
};
```

<div  id="question-5"/>

### Summary

- 3/4 completed
- 9836 / 23449
- keep moving!!!
