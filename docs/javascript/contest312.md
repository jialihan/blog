## Leetcode Contest 312 - JavaScript

#### I. [2418. Sort the People](#question-1)

#### II. [2419. Longest Subarray With Maximum Bitwise AND](#question-2)

#### III. [2420. Find All Good Indices](#question-3)

#### IV. [2421. Number of Good Paths](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [22418. Sort the People](https://leetcode.com/problems/sort-the-people/)

Solution:

```
var sortPeople = function(names, heights) {
    const arr = names.map((name,i)=>{
        return [name, heights[i]];
    });
    arr.sort((a,b)=>b[1] - a[1]);
    return arr.map(el=>el[0]);
};
```

<div  id="question-2"/>

### [2419. Longest Subarray With Maximum Bitwise AND](https://leetcode.com/problems/longest-subarray-with-maximum-bitwise-and/)

Solution:
https://leetcode.com/problems/longest-subarray-with-maximum-bitwise-and/discuss/2662554/JavaScript-longest-max-value-subarray

<div  id="question-3"/>

### [2420. Find All Good Indices](https://leetcode.com/problems/find-all-good-indices/)

```
var goodIndices = function(nums, k) {
    const n = nums.length;
    let res = [];
    let dec = [];
    let left = new Array(n).fill( false);
    for(let i = 0; i<n - k; i++) {
        if(i<k){
            if(i>0 && nums[i]> nums[i-1]) {
                dec = [nums[i]];
            }
            else {
               dec.push(nums[i]);
            }
        }
        else if(i<n-k) {
            if(dec.length>=k) {
                left[i] = true;
                dec.shift();
            }
            if(nums[i] <= nums[i-1]) {
                  dec.push(nums[i]);
            }
            else {
               dec = [nums[i]];
            }
        }
        else {
            break;
        }
    }
    // check right
    nums = nums.reverse();
    dec =[];
    for(let i = 0; i<n - k; i++) {
        if(i<k){
            if(i>0 && nums[i]> nums[i-1]) {
                dec = [nums[i]];
            }
            else {
               dec.push(nums[i]);
            }
        }
        else if(i<n-k) {

                if(dec.length < k) {
                    left[n-i-1] = false;
                }
                else {
                    dec.shift();
                }

                if(nums[i]<= nums[i-1]) {
                    dec.push(nums[i]);
                }
               else {
                 dec = [nums[i]];
              }
        }
        else {
             break;
         }
    }

    // return results
    for(let i = k; i<n-k; i++) {
        if(left[i]) {
            res.push(i);
        }
    }
    return res;
};
```

<div  id="question-4"  />

### [2421. Number of Good Paths - not solved](https://leetcode.com/problems/number-of-good-paths/)

**Solution: DFS - TLE at one test case**
https://leetcode.com/problems/number-of-good-paths/discuss/2624624/JavaScript-DFS-traverse-tree-once

<div  id="question-5"/>

### Summary

- 3/4 completed
- 6137 / 24973
