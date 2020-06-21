## Leetcode Contest 194 

### 1486. XOR Operation in an Array
My original solution:
```
    public int xorOperation(int n, int start) {
        int res = start;
        for(int i = 1;i<n;i++)
        {
             int cur = start + 2* i;
             res = res ^ cur;
        }
	    return res;
    }
```
optimized solution after the contest, reference to some higher ranking's solutions. `XOR base is 0 will not change the value: 0 ^ 0 = 0, 0 ^ 1 = 1`, to reduce and clean the code:
```
 public int xorOperation(int n, int start) {
	 int res = 0; 
     for(int i = 0 ; i<n; i++)
     {
        res = res ^ (start+ 2*i);
     }
     return res;
}
```

 To find more information and details: [Leetcode 1486](https://leetcode.com/problems/xor-operation-in-an-array/)

  
### 1487. Making File Names Unique
Pay attention to the Example 3:
```
Input: names = ["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece"]
Output:["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece(4)"]
Explanation: When the last folder is created, the smallest positive valid k is 4, and it becomes "onepiece(4)".
```
Tricky part is the duplicated name like already existed: `a,a(1)`, but the new input is still `a`, so if the expected in order sequence is `a(1)` again, but we need to check if it's already existed again.

Build a map to store the `<String, Count(Integer)>`, count will be the next expected sequence number.
```
public String[] getFolderNames(String[] names) {
	Map<String, Integer> map = new HashMap<>();
    int n = names.length;
    String res[] = new String[n];
    for(int i = 0 ;i <n ;i ++)
    {
        if(!map.containsKey(names[i]))
        {
            res[i] = names[i];
            map.put(names[i], 1);
        }
        else
        {
            int  k = map.get(names[i]);
            res[i] = names[i]+"(" + k+ ")";
            while(map.containsKey(res[i]))
            {
	            k++;
                res[i] = names[i] + "(" + k+ ")";
            }
            map.put(names[i], k+1);
            map.put(res[i], 1);
        }
    }
    return res;
}
```
optimized solution after the contest, reference to some higher ranking's solutions. `XOR base is 0 will not change the value: 0 ^ 0 = 0, 0 ^ 1 = 1`, to reduce and clean the code:
```
 public int xorOperation(int n, int start) {
	 int res = 0; 
     for(int i = 0 ; i<n; i++)
     {
        res = res ^ (start+ 2*i);
     }
     return res;
}
```

 To find more information and details: [Leetcode 1487](https://leetcode.com/problems/making-file-names-unique/)

### Leetcode 1488 
![image](../assets/LC1488_1.png ':size=529x241')

Bugs that i met during the contest:
1. forget one error condition to end this function:
 When there is "0" options, but it's invalid and we cannot use this dry option to dry the current lake, for example the following rains[] input: `[0,2,2]`. Because the zero index is before our lake index, so that means, we don't have valid zero options to dry the lake 2 at the last index. So the final result will be `[]`.
 Use the following print out to debug:
 ```
// System.out.println("optional index = " + index.get(0) + ", lake index= "+lakeIndex.get(lake));
```
2. another tricky bug is that we need to record the `lake index` correctly. Because we depend on the previous lake index, so we need to update current lake index after all our logic. Then finally in the for loop, we update the lake index:
```
for(int i = 0 ;i <n ; i++) {
	int lake = rains[i];
	if(lake == 0) {
	}
	else {
		lakeIndex.put(lake, i);    
	}
}
```

3. another bug is that when we have extra unused `0`, we need to fill some random values larger than 0 to the final result. Then we need to add the following block:
```
if(index.size()>0) {
	for(int i: index) {
		res[i] = 1;
    }
}
```
Final solution during the contest:
```
 public int[] avoidFlood(int[] rains) {
        int n = rains.length;
        int[] res = new int[n];
        List<Integer> zero = new ArrayList<>();
        Set<Integer> flood = new HashSet<>();
        Map<Integer,Integer> lakeIndex = new HashMap<>();
        for(int i = 0 ;i <n ; i++)
        {
            int lake = rains[i];
            if(lake == 0)
            {
                zero.add(i);
            }
            else
            {
                res[i] = -1;
                if(flood.contains(lake))
                {
                    if(zero.size() == 0)
                    {
                        return new int[0];
                    }
                    else
                    {
                        int optionalIndex = findZeroIndex(zero, lakeIndex, lake);
                        if(optionalIndex == -1)
                        {
                            return new int[0]; // cannot dry the lake
                        }
                        else
                        {  
                            int idx = zero.get(optionalIndex);
                            res[idx] = lake;
                            zero.remove(optionalIndex);
                        } 
                    }
                }
                else
                {
                    flood.add(lake);
                    
                }
                lakeIndex.put(lake, i);    
            }
            
        }
        if(zero.size()>0)
        {
            for(int i: zero)
            {
                res[i] = 1;
            }
        }
        return res;
    }
    
    public int findZeroIndex( List<Integer> zero,  Map<Integer,Integer> lakeIndex, int lake ){
        int l = lakeIndex.get(lake);
        int optionalIndex = -1;
        for(int j = 0 ;j <zero.size(); j++)
        {
            if(zero.get(j)> l)
            {
                optionalIndex = j;
                break;
            }
        }
        return optionalIndex;
    }
```
### Leetcode 1489 Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree
Not complete during the contest. Here is the solution after the contest.


####  Summary:  1080 / 13807