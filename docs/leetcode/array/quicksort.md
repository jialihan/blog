## Quick Sort Algorithm

#### I. [Basics](#question1)
- [Quicksort function](#q1-2)
- [Partition function](#q1-2)

#### II. [215. Kth Largest Element in an Array](#question2)

#### III. [973. K Closest Points to Origin](#question3)

<div id="question1"/>

### 1. Basics

<div id="q1-1"/>

#### 1.1 Quicksort function
Didfferent ways to choose the pivot point:
- random
- left
- right
- middle: can improve the worst time to O(NlogN) -> I choose this
```js
function quicksort(arr, k)
    // find the kth smallest element in an array
    var n = arr.length;
    var l = 0;
    var r = n - 1;
    while (l <= r) {
        var pivot = partition(arr, l, r);
        // console.log("pivot at " + pivot);
        if (pivot === k - 1) {
            return arr[pivot];
        }
        else if (pivot < k - 1) {
            l = pivot + 1;
        }
        else {
            r = pivot - 1;
        }
    }
    return null; // not find
};
```

<div id="q1-2"/>

#### 1.2 Partition function

Goal: at index: `pivot`, move all **"smaller"** elements to the left, and keep all **"larger or equal to"** elements to the right.

```js
function partition(arr, l, r) {
    var mid = Math.floor((l + r) / 2);
    var pivotDist = getDist(arr, mid);
    // console.log("pivotDist: " + pivotDist + ", mid: " + mid);
    swap(arr, mid, r);
    var i = l;
    var j = r - 1;
    while (i <= j) {
        while (i < r && getDist(arr, i) < pivotDist) {
            i++;
        }
        while (j >= 0 && getDist(arr, j) >= pivotDist) {
            j--;
        }
        if (i <= j) {
            swap(arr, i, j);
        }
    }
    // put the pivot one back
    swap(arr, i, r);
    // console.log("mid="+mid+ "pivot at "+ i + ", current points: "+ arr);
    return i;
}
```

<div id="question2"/>

### [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

```js
var findKthLargest = function(nums, k) {
    // quick sort: worst time O(nlogn) if partition in middle
    return quicksort(nums, nums.length - k); 
    function quicksort(arr, k)
    {
        // find kth element in array
        var l = 0;
        var r = arr.length-1;
        while(l<=r)
        {
            var pivot = partition(arr, l, r);
            if(pivot === k)
            {
                return arr[pivot];
            }
            else if(pivot < k)
                {
                    l = pivot+1;
                }
            else
                {
                    r = pivot - 1;
                }
        }
        return  arr[l];
    }
    function partition(arr, l, r)
    {
        var mid = Math.floor((l+r)/2);
        var pivot = arr[mid];
        // put pivot mid to the last position, won't sort
        [arr[mid], arr[r]] = [arr[r], arr[mid]];
        // put all num < num[mid] to the left
        var i = l;
        var j = r-1;
        while(i<=j)
        {
            while(arr[i]<pivot)
                i++;
            while(arr[j]>=pivot)
                j--;
            if(i<=j)
            {
                [arr[i], arr[j]] = [arr[j], arr[i]]; 
            }
        }
        // swap back the pivot at the last position
        [arr[i], arr[r]] = [arr[r], arr[i]];
        return i;
    }
};
```

<div id="question3"/>

### [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

```js
var kClosest = function(points, k) {
    // from 0 to (k-1)th smallest points
    var n = points.length;
    var l = 0;
    var r = n-1;
    while(l<=r)
    {
        var pivot = partition(points, l, r);
        if(pivot === k-1)
        {
            return points.slice(0, k);
        }
        else if(pivot < k-1)
        {
            l = pivot+1;
        }
        else
        {
            r = pivot-1;
        }
    }
    return new Array(k).fill(null).map(()=>new Array(2));
};

function partition (arr, l, r)
{
    var mid = Math.floor((l+r)/2);
    var pivotDist = getDist(arr,mid);
    // swap mid to the last position
    swap(arr, mid, r);
    var i = l;
    var j = r-1;
    while(i<=j)
    {
        while(i<r && getDist(arr, i) < pivotDist)
        {
            i++;
        }
        while(j>=0 && getDist(arr, j) >= pivotDist)
        {
            j--;
        }
        if(i<=j)
        {
            swap(arr, i, j);
        }
    }
    // put the pivot one back
    swap(arr, i, r);
    return i;
}
function getDist(arr, i)
{
    return arr[i][0]**2 + arr[i][1]**2;
}
function swap(arr, i,j)
{
    tmpi = arr[i].slice();
    tmpj = arr[j].slice();
    arr[i] = tmpj
    arr[j] = tmpi;
}
```