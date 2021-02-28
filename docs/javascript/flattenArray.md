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