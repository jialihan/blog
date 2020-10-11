## Leetcode Contest 210 - JavaScript

### 1614. Maximum Nesting Depth of the Parentheses

A string is a **valid parentheses string** (denoted **VPS**) if it meets one of the following:

- It is an empty string `""`, or a single character not equal to `"("` or `")"`,
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are **VPS**'s, or
- It can be written as `(A)`, where `A` is a **VPS**.

We can similarly define the **nesting depth** `depth(S)` of any VPS `S` as follows:

- `depth("") = 0`
- `depth(A + B) = max(depth(A), depth(B))`, where `A` and `B` are **VPS**'s
- `depth("(" + A + ")") = 1 + depth(A)`, where `A` is a **VPS**.

For example, `""`, `"()()"`, and `"()(()())"` are **VPS**'s (with nesting depths 0, 1, and 2), and `")("` and `"(()"` are not **VPS**'s.
Given a **VPS** represented as string `s`, return _the **nesting depth** of_ `s`.

**Example 1:**
**Input:** s = "(1+(2\*3)+((8)/4))+1"
**Output:** 3
**Explanation:** Digit 8 is inside of 3 nested parentheses in the string.

**Example 2:**
**Input:** s = "(1)+((2))+(((3)))"
**Output:** 3

**My JavaScript Solution:**

```
var maxDepth = function(s) {
    let res = 0;
    let cnt = 0;
    for(let c of [...s])
        {
            if(c === '(')
                {
                    cnt++;
                }
            else if(c ===')')
                {
                    cnt--;
                }
            if(cnt > 0)
                {
                    res = Math.max(res, cnt);
                }
        }
    return res;
};
```

1615. Maximal Network Rank

There is an infrastructure of `n` cities with some number of `roads` connecting these cities. Each `roads[i] = [ai, bi]` indicates that there is a bidirectional road between cities `ai` and `bi`.

The **network rank** of **two different cities** is defined as the total number of **directly** connected roads to **either** city. If a road is directly connected to both cities, it is only counted **once**.

The **maximal network rank** of the infrastructure is the **maximum network rank** of all pairs of different cities.

Given the integer `n` and the array `roads`, return _the **maximal network rank** of the entire infrastructure_.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/09/21/ex1.png)**
**Input:** n = 4, roads = [[0,1],[0,3],[1,2],[1,3]]
**Output:** 4
**Explanation:** The network rank of cities 0 and 1 is 4 as there are 4 roads that are connected to either 0 or 1. The road between 0 and 1 is only counted once.

**Analysis:**

- at first: try to understand the goal: think about find max outing edges, or union-find to see if two points are connected.
- Goal: **find any two node that has the max sum of neighbor edges count, if this pair of nodes are directly connected, ignore the duplicate count.**
- Build adjacent list like: **{ node_i : [n1, n2, n3,.....] }**
- Use Map `<index, [array of neighbors ]>`
  **My Javascript Solution at contest: not OPT**

```
var maximalNetworkRank = function(n, roads) {
    if(!roads || roads.length === 0)
    {
            return 0;
    }
    let map = new Map();
    let res = 0;
    for([r1, r2] of roads)
        {
            if(!map.has(r1))
                {
                    map.set(r1, []);
                }
            if(!map.has(r2))
                {
                    map.set(r2, []);
                }
            map.get(r1).push(r2);
            map.get(r2).push(r1);
        }

   for(let i = 0; i< n; i++)
    {
        if(!map.has(i))
            continue;
       for(let j = i+1; j<n; j++)
           {
               if(!map.has(j))
                   continue;
               const arr1 = map.get(i);
               const arr2 = map.get(j);
               let cur = arr1.length + arr2.length;
               if(arr1.includes(j))
                   {
                       cur--;
                   }
               res = Math.max(res, cur);
           }
    }
    return res;
};
```

**Cleaner and Optimized Javascript Solution:**

- use Set()
- use pre-filled array of N

```
var maximalNetworkRank = function(n, roads) {
    if(!roads || roads.length === 0)
    {
            return 0;
    }
    let adj = [...Array(n)].map(_ => new Set());
    for([r1, r2] of roads)
    {
            adj[r1].add(r2);
            adj[r2].add(r1);
    }
   let res = 0;
   for(let i = 0; i< n; i++)
       for(let j = i+1; j<n; j++)
       {
               const arr1 = adj[i];
               const arr2 = adj[j];
               let cur = arr1.size + arr2.size  - arr1.has(j);
               res = Math.max(res, cur);
       }
    return res;
};
```

### 1616. Split Two Strings to Make Palindrome

You are given two strings `a` and `b` of the same length. Choose an index and split both strings **at the same index**, splitting `a` into two strings: `aprefix` and `asuffix` where `a = aprefix + asuffix`, and splitting `b` into two strings: `bprefix` and `bsuffix` where `b = bprefix + bsuffix`. Check if `aprefix + bsuffix` or `bprefix + asuffix` forms a palindrome.

When you split a string `s` into `sprefix` and `ssuffix`, either `ssuffix` or `sprefix` is allowed to be empty. For example, if `s = "abc"`, then `"" + "abc"`, `"a" + "bc"`, `"ab" + "c"` , and `"abc" + ""` are valid splits.

Return `true` _if it is possible to form_ _a palindrome string, otherwise return_ `false`.

**Notice** that `x + y` denotes the concatenation of strings `x` and `y`.

**Example 1:**
**Input:** a = "x", b = "y"
**Output:** true
**Explaination:** If either a or b are palindromes the answer is true since you can split in the following way:
aprefix = "", asuffix = "x"
bprefix = "", bsuffix = "y"
Then, aprefix + bsuffix = "" + "y" = "y", which is a palindrome.

**My Analysis:**
Because I met this same problem when I had a on-site interview in the past, then I just try to finish it in javascript and debug.
Idea: two pointer

1. happy path

- when i === j, all are equal characters
- when a[i] !== b[j], but remaining part in `a( i to j )` is valid or `b( i to j )` is valid  
  For example:

  - a: 0,1,2,i, ......
  - b: 0,1,2,i, ........j, j+1, ..., bLen-1  
    Then there are two possible combinations:
    ```
    substring1 = a[ 0, i ) + b[ i, blen )
    substring2 = a[ 0,j+1 ) + b[ j, blen )
    ```

2.  false path

- whenever a[i] !== b[j]

**My final JavaScript Solution:**

```
var checkPalindromeFormation = function(a, b) {

    if(isPanlindrome(a) || isPanlindrome(b))
    {
            return true;
    }
    return helper(a,b) || helper(b,a);

};

function isPanlindrome(s)
{
    let reversed = s.split('').reverse().join('');
    return s === reversed;
}
function helper(a, b)
{
    // a start, b end
    let i = 0;
    let j = b.length-1;
    let flag = true;
    while(i<=j)
    {
         if(a[i] != b[j])
         {
               let s1 = a.slice(0,j+1) + b.slice(j+1,b.length);
               let s2 = a.slice(0,i)+b.slice(i, b.length);
               if(isPanlindrome(s1) || isPanlindrome(s2))
               {
                    return true;
               }
               flag = false;
               break;
          }
          i++;
          j--;
    }
    if(flag && i== j+1)
          return true;
    return false;
}
```

[1617. Count Subtrees With Max Distance Between Cities](https://leetcode.com/problems/count-subtrees-with-max-distance-between-cities/)

Not Solved!

### Summary

- totally use javascript for all problems in contest the first time
- slow in thinking in javascript, but it's the very start! good start!
- tried better and cleaner solution after the contest
- need to have more array operations in javascript
  [Global_Objects/Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

  [Global_Objects/Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)

  [Global_Objects/Array/fill](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)

  [String Operations](https://www.w3schools.com/js/js_string_methods.asp): length, substring, includes

- Result: 2914 / 11792
  Keep moving!
