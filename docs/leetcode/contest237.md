## Leetcode Contest 237 - All Solved - JavaScript

#### I. [1832. Check if the Sentence Is Pangram](#question-1)

#### II. [1833.  Maximum Ice Cream Bars](#question-2)

#### III. [1834.  Single-Threaded CPU](#question-3)

#### IV. [1835.  Find XOR Sum of All Pairs Bitwise AND](#question-4)

#### V. [Summary](#summary)

<div id="question-1"/>

### [1832. Check if the Sentence Is Pangram](https://leetcode.com/problems/check-if-the-sentence-is-pangram/)

A  **pangram**  is a sentence where every letter of the English alphabet appears at least once.

Given a string  `sentence`  containing only lowercase English letters, return  `true` _if_ `sentence` _is a  **pangram**, or_ `false` _otherwise._

**Example 1:**
**Input:** sentence = "thequickbrownfoxjumpsoverthelazydog"
**Output:** true
**Explanation:** sentence contains at least one of every letter of the English alphabet.

**JavaScript:** one line solution:
- `return` will NOT break the `forEach()` loop
```js
var checkIfPangram = function(sentence) { 
    return new Set([...sentence]).size === 26 ? true: false;
};
```

Reference discussion article here: [Javascript Set() solution](https://leetcode.com/problems/check-if-the-sentence-is-pangram/discuss/1164196/JavaScript%3A-Set()-solution)

<div id="question-2"/>

### [1833.  Maximum Ice Cream Bars](https://leetcode.com/problems/maximum-ice-cream-bars/)

It is a sweltering summer day, and a boy wants to buy some ice cream bars.

At the store, there are  `n`  ice cream bars. You are given an array  `costs`  of length  `n`, where  `costs[i]`  is the price of the  `ith`  ice cream bar in coins. The boy initially has  `coins`  coins to spend, and he wants to buy as many ice cream bars as possible.

Return  _the  **maximum**  number of ice cream bars the boy can buy with_ `coins` _coins._

**Note:**  The boy can buy the ice cream bars in any order.

**Example 1:**
**Input:** costs = [1,3,2,4,1], coins = 7
**Output:** 4
**Explanation:** The boy can buy ice cream bars at indices 0,1,2,4 for a total price of 1 + 3 + 2 + 1 = 7.

**JavaScript solution:**
```js
vvar maxIceCream = function(costs, coins) {
    costs.sort((a,b)=>a-b);
    if(costs[0] > coins)
    {
        return 0;
    }
    var cnt = 0;
    var i = 0;
    while(coins > 0 && i<costs.length)
    {
        if(coins<costs[i])
        {
            break;
        }
        coins -= costs[i];
        i++;
        cnt++;
    }
    return cnt;
    
};
```

<div id="question-3"/>

### [1834.  Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)

You are given  `n`​​​​​​ tasks labeled from  `0`  to  `n - 1`  represented by a 2D integer array  `tasks`, where  `tasks[i] = [enqueueTimei, processingTimei]`  means that the  `i​​​​​​th`​​​​ task will be available to process at  `enqueueTimei`  and will take  `processingTimei`  to finish processing.

You have a single-threaded CPU that can process  **at most one**  task at a time and will act in the following way:

-   If the CPU is idle and there are no available tasks to process, the CPU remains idle.
-   If the CPU is idle and there are available tasks, the CPU will choose the one with the  **shortest processing time**. If multiple tasks have the same shortest processing time, it will choose the task with the smallest index.
-   Once a task is started, the CPU will  **process the entire task**  without stopping.
-   The CPU can finish a task then start a new one instantly.

Return  _the order in which the CPU will process the tasks._

**JS solution:**
See the discussion article here:
[JavaScript: Sort to mock the priority queue](https://leetcode.com/problems/single-threaded-cpu/discuss/1164169/JavaScript-Sort-to-mock-the-PriorityQueue-in-JavaScript)

<div id="question-4" />

### [1835.  Find XOR Sum of All Pairs Bitwise AND](https://leetcode.com/problems/find-xor-sum-of-all-pairs-bitwise-and/)

The  **XOR sum**  of a list is the bitwise  `XOR`  of all its elements. If the list only contains one element, then its  **XOR sum**  will be equal to this element.

-   For example, the  **XOR sum**  of  `[1,2,3,4]`  is equal to  `1 XOR 2 XOR 3 XOR 4 = 4`, and the  **XOR sum**  of  `[3]`  is equal to  `3`.

You are given two  **0-indexed**  arrays  `arr1`  and  `arr2`  that consist only of non-negative integers.

Consider the list containing the result of  `arr1[i] AND arr2[j]`  (bitwise  `AND`) for every  `(i, j)`  pair where  `0 <= i < arr1.length`  and  `0 <= j < arr2.length`.

Return  _the  **XOR sum**  of the aforementioned list_.

**Example 1:**
**Input:** arr1 = [1,2,3], arr2 = [6,5]
**Output:** 0
**Explanation:** The list = [1 AND 6, 1 AND 5, 2 AND 6, 2 AND 5, 3 AND 6, 3 AND 5] = [0,1,2,0,2,1].
The XOR sum = 0 XOR 1 XOR 2 XOR 0 XOR 2 XOR 1 = 0.

**Example 2:**
**Input:** arr1 = [12], arr2 = [4]
**Output:** 4
**Explanation:** The list = [12 AND 4] = [4]. The XOR sum = 4.

**JS solution:**
```js
var getXORSum = function(arr1, arr2) {
    // (x & 2) xor (x & 3) [ !x OR (2 XOR 3)] XOR [!x OR (2 XOR 3)]
    // = !(x & 2)&(x&3) || (x&2)&!(x&3)
    // = (!x || !2) & x & 3 || x & 2 & (!x || !3)
    // = (!2 & x & 3 ) || (x & 2 & !3)
    // = x & (!2 & 3) || (!3 & 2)
    // = x & (2 XOR 3)
    // const ans = (2 XOR 3 XOR....) in arr2
    // The same principle: x, y,z... in arr1
    // res = (x & ans) XOR (y & ans) XOR .....
    //  = ans & (x XOR y XOR z .......)
    // Finally: res = (XOR: arr1) AND (XOR: arr2);
    var xor1 = arr1.reduce((acc,cur)=>acc^cur);
    var xor2 = arr2.reduce((acc,cur)=>acc^cur);
    return xor1 & xor2;  
};
```
Also you could see my discussion article here:
[JS - Math solution: use a rule to simplify](https://leetcode.com/problems/find-xor-sum-of-all-pairs-bitwise-and/discuss/1164069/JavaScript-Just-Math-use-a-rule-to-simply)

<div id="summary" />

### V. Summary

- Priority queue in JS: this lib not support customized compare function: https://github.com/datastructures-js/priority-queue
- XOR: try to find some **simple / smart way** to simplify the problem, XOR related problem, **NOT always to brute-force**, there usually some hint or easy way to workaround.
- 1865 / 11446
