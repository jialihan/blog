## Leetcode Contest 198


### 1520. Maximum Number of Non-Overlapping Substrings


Given a string  `s` of lowercase letters, you need to find the maximum number of  **non-empty**  substrings of `s` that meet the following conditions:

1.  The substrings do not overlap, that is for any two substrings  `s[i..j]`  and  `s[k..l]`, either  `j < k`  or  `i > l` is true.
2.  A substring that contains a certain character `c` must also contain all occurrences of  `c`.

Find  _the maximum number of substrings that meet the above conditions_. If there are multiple solutions with the same number of substrings,  _return the one with minimum total length._ It can be shown that there exists a unique solution of minimum total length.

Notice that you can return the substrings in  **any**  order.

* Analyze conditions:
	1 ) The substrings **do not overlap**: means we cannot choose one string and another string as solution at the same time, if they have index overlapped. For example: `aaca`, if we choose `aaca` then we cannot choose `c` as solution.
	2 )  	**contain all occurrences**: means we must find the right most valid index as our substring range. `*Hint for here is to find last index for each character.`

* Algorithm steps
   1 ) loop at each index, because we need to find the optimized solution: `max number` && `shortest length`.
   2 ) at each index, find the current valid solution with range of `[ start, rightMost ]`.
   
  ![image](../assets/lc1520_00.png ':size=430x164')

   3 ) Update final result:  The reason why we can ensure the **current is always a better solution than previous** is that previous solution is either:
   * current is inside of previous substring: then we get a  **shorter length**
   * current is outside of previous substring: then we can **increase number of result**
   ```
   if start > prev_Right
   {
	   // no overrlap
	   add to result;
   }
   else // start <= pre
   {
	   // overrlap, this is a better solution
   }
   ```
  
  ![image](../assets/lc1520_01.png ':size=415x206')

* complete java code:
```
 public List<String> maxNumOfSubstrings(String s) {
        
        List<String> res = new ArrayList<>();
        int[] f = new int[256];
        int[] l = new int[256];
        Arrays.fill(f, s.length());
        Arrays.fill(l, -1);
       char[] arr = s.toCharArray();
        for (int i = 0; i < s.length(); i++) {
            char c = arr[i];
            f[c] = Math.min(f[c], i);
            l[c] = Math.max(l[c], i);
        }
        int prev = -1; // check if in previous substring
        for(int i = 0; i<s.length(); i++)
        {
            char c = s.charAt(i);
            if(f[c] < i)
            {
                continue;
            }
            int end =  l[c];
            int pos = i;
            while(pos <= end && f[s.charAt(pos)] >= i)
            {
                char t = s.charAt(pos);
                
                end = Math.max(end, l[t]);
                
                pos++;
            }
            if(pos< end && f[s.charAt(pos)] < i)
            {
                continue;
            }
            String sub = s.substring(i, end+1);
            if(prev < i)
            {
                // add
                 res.add(sub);
            }
            else
                res.set(res.size()-1, sub); // replace
            
             prev = end;
        }
        return res;
    }
```
