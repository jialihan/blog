## Leetcode Contest 223- JavaScript

I.  [1720.  Decode XORed Array](#question-1)

II. [1721.  Swapping Nodes in a Linked List](#question-2)
- [1 ) My javascript solution at contest](#q2-1)
- [2 ) Optimized simple and clean solution](#q2-1)

III. [1722.  Minimize Hamming Distance After Swap Operations](#question-3)

IV. [1723.  Find Minimum Time to Finish All Jobs - not solved](#question-4)

V. [Summary](#summary)


<div id="question-1" />

### [1720.  Decode XORed Array](https://leetcode.com/problems/decode-xored-array/)

There is a  **hidden**  integer array  `arr`  that consists of  `n`  non-negative integers.

It was encoded into another integer array  `encoded`  of length  `n - 1`, such that  `encoded[i] = arr[i] XOR arr[i + 1]`. For example, if  `arr = [1,0,2,1]`, then  `encoded = [1,2,3]`.

You are given the  `encoded`  array. You are also given an integer  `first`, that is the first element of  `arr`, i.e.  `arr[0]`.

Return  _the original array_  `arr`. It can be proved that the answer exists and is unique.

**Example 1:**
**Input:** encoded = [1,2,3], first = 1
**Output:** [1,0,2,1]
**Explanation:** If arr = [1,0,2,1], then first = 1 and encoded = [1 XOR 0, 0 XOR 2, 2 XOR 1] = [1,2,3]

**Note:** XOR operation
```
a ^ b = c
c ^ a = b
b ^ c = a
```

**My JavaScript Solution:**
```
/**
 * @param {number[]} encoded
 * @param {number} first
 * @return {number[]}
 */
var decode = function(encoded, first) {
    var n = encoded.length + 1;
    var res = [...Array(n)];
    res[0] = first;
    for(var i = 1; i<n; i++)
        {
            res[i] = encoded[i-1] ^ res[i-1];
        }
    return res; 
};
```

<div id="question-2"/>

### 1721.  Swapping Nodes in a Linked List

You are given the  `head`  of a linked list, and an integer  `k`.

Return  _the head of the linked list after  **swapping**  the values of the_ `kth`  _node from the beginning and the_ `kth`  _node from the end (the list is  **1-indexed**)._

**Example 1:**
**Input:** head = [1,2,3,4,5], k = 2
**Output:** [1,4,3,2,5]

Analysis: 
Think too much at contest, don't need to swap the real object node in the linked list, can switch the value.

<div id="q2-1" />

#### 1 ) My javascript solution at contest
Brute force solution: too complex, too stupid
```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
var swapNodes = function(head, k) {
    // first: k-1
    // last: n-k
    var n = 0;
    var dummy = new ListNode(-1);
    dummy.next = head;
    var pre = dummy;
    var p = dummy.next;
    while(p !== null)
    {
        n++;
        p = p.next;
    }
    if(k-1 === n-k)
    {
        return head;
    }
    if(k>Math.floor(n/2))
        {
            k = n-k+1;
    }
    p = head;
    var i = 0;
    while(p !== null)
    {
        if(i === k-2)
        {
            pre = p;
        }
        console.log("i=", i, "k=", k, " n=",n);
        if(i === n-k-1)
        {
            if(k === n-k)
                {
                    // swap p & p.next
                    var tmp = p.next;
                    pre.next = tmp;
                    p.next = tmp.next;
                    tmp.next = p;
                }
            else
                {
                var last = p.next;
                p.next = p.next.next;
                var first = pre.next;
                pre.next = last;
                last.next = first.next;
                first.next = p.next;
                p.next = first;
                break;
                }
        }
        p = p.next;
        i++;
    }
    return dummy.next;  
};
```

<div id="q2-2" />

#### 2 ) Optimized simple and clean solution
```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
var swapNodes = function(head, k) {
    // first: k-1
    // last: n-k
    let nodes = [];
    let p = head;
    while(p !== null)
    {
            nodes.push(p);
            p = p.next;
    }
    const n = nodes.length;
    const first =nodes[k-1];
    const last = nodes[n-k];
    const val1 = first.val;
    const val2 = last.val;

    first.val = val2;
    last.val = val1;
    return head; 
};
```


<div id="question-3"/>

### [1722. Minimize Hamming Distance After Swap Operations](https://leetcode.com/problems/minimize-hamming-distance-after-swap-operations/)

Steps:
- union find to build using the allowed-Swaps
- classify the source index to group the source by root parent 
- count and find the target index, deciding which can be matched and canceled

```js
class DSU {
	constructor(n) {
		this.p = [...Array(n)];
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
			
			this.p[px] = py;
		}
	}
}
var minimumHammingDistance = function(source, target, allowedSwaps) {
    const map = new Map();
    const n = source.length;
    
    // union find 
    const dsu = new DSU(n);
    allowedSwaps.forEach(el=>{
        dsu.union(el[0], el[1]);
    });
    
    // calc source
    source.forEach((el,i)=>{
        const p = dsu.findParent(i);
        if(!map.has(p))
            {
                map.set(p, []);
            }
        map.get(p).push(el);
    });
    
    var res = 0;
    target.forEach((el, i)=>{
        const p = dsu.findParent(i);
        if(map.get(p).includes(el))
            {
                var index = map.get(p).indexOf(el);
                if (index > -1) {
                  map.get(p).splice(index, 1);
                }
            }
        else
            res++;
    });
    return res;  
};
```


<div id="question-4"/>

### 1723.  [Find Minimum Time to Finish All Jobs](https://leetcode.com/problems/minimize-hamming-distance-after-swap-operations/)

Not Solved!

### Summary
* swap in linked list, do not need to swap all real object
* keep moving next week!