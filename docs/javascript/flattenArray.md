## Flatten an Array in Javascript

### Solution1: reduce() 

```js
var flat = function(arr)
{
     var result = arr.reduce( (acc,cur) => {
        if(Array.isArray(cur))
        {
            acc = acc.concat(flat(cur));
        }
        else
        {
            //   acc.push(cur);
              acc = [...acc, cur];
        }
        return acc;
     }, []);
     return result;
 }
 // usage
 const result = flat([1,[2,[3,4]]]);
```

### Flatten with depth

**link**: [bfe 3.](https://bigfrontend.dev/problem/implement-Array-prototype.flat)

There is already  [Array.prototype.flat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)  in JavaScript (ES2019), which reduces the nesting of Array.

Could you manage to implement your own one?
Here is an example to illustrate:
```js
const arr = [1, [2], [3, [4]]];
flat(arr)
// [1, 2, 3, [4]]
flat(arr, 1)
// [1, 2, 3, [4]]
flat(arr, 2)
// [1, 2, 3, 4]
```
**follow up**
Are you able to solve it both recursively and iteratively?

#### 2.1 recursive solution
```js
function flat(arr, depth = 1) {
  var res = [];
  arr.forEach(el=>{
    if(Array.isArray(el) && depth > 0)
    {
      res.push(...flat(el, depth-1));
    }
    else
    {
      res.push(el);
    }
  });
  return res;
}
```

**Optimized** `reduce()` solution:
```js
function flat(arr, depth = 1) {
  return depth > 0 ? arr.reduce((acc,cur)=>{
    var item = Array.isArray(cur) ? flat(cur, depth-1) : cur;
    return acc.concat(item);
  }, []) : arr;
}
```

#### 2.2 iterative

Use `queue` model:
- dequeue at the front
- process current level
- next level `(depth - 1)`
- enqueue next level at the end 

```js
/**
 * @param { Array } arr
 * @param { number } depth
 * @returns { Array }
 */
function flat(arr, depth = 1) {
  var queue = arr.map(el=>[el, depth]);
  var res = [];
  while(queue.length>0)
  {
    var [cur, dep] = queue.shift(); // dequeue at the front
    if(Array.isArray(cur) && dep >0)
    {
      queue.push(...cur.map(el=>[el, dep-1])); // enqueue at the end
    }
    else
    {
      res.push(cur); // add to results
    }
  }
  return res;
}
```