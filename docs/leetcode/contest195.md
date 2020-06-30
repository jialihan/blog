## Leetcode Contest 195

### 1496. Path Crossing
Use [sparse matrix]([https://en.wikipedia.org/wiki/Sparse_matrix](https://en.wikipedia.org/wiki/Sparse_matrix)) to record the path at each position [x,y]. Use relative position starting at assuming original point [0,0].
Direction translation:
* N: go up [x, y+1]
* E: go right [x+1, y]
* S: go down [x, y-1]
* W: go left [x-1, y]

My solution:
```
 public boolean isPathCrossing(String path) {
        HashSet<String> visited = new HashSet<>();
        int x = 0, y = 0;
         visited.add("0:0");
        for(char c: path.toCharArray())
        {
            
            if(c == 'N')
            {y++;}
            else if(c == 'S')
            {
                y--;
            }
            else if(c =='E')
            {
                x++;
            }
            else if(c == 'W')
            {
                x--;
            }
            String code = "" +x+ ":"+ y;
            if(visited.contains(code))
                return true;
            visited.add(code);
        }
        return false;
        
    }
```

### 1497. Check If Array Pairs Are Divisible by k
tricky point: use remaining array to record count of each remaining numbers from 1 to (k-1). When pair's remainings sum equal to k, then this pair can be divided by k for sure. 
```
if((remain1 + remain2) == k)
{
	pair(remain1, remain2) are divisible by k
}
```
The pairing rules is like the picture below:

![image](../assets/remains.png ':size=710x137')

Also pay attention to the negative numbers here in array, so we use `(num % k + k)%k` as final remaining number.
my java solution:
```
    public boolean canArrange(int[] arr, int k) {
        int[] remains = new int[k]; 
        for(int a: arr)
        {
            remains[ ( a % k + k) % k]++;
        }
        // verify
        for(int i = 1; i<k; i++)
        {
            if(remains[i] != remains[k-i])
                return false;
        }
        return remains[0] % 2 ==0 ;
    }
```

###  1498. Number of Subsequences That Satisfy the Given Sum Condition

step1: consider no rules, and the total combinations to list all sub sequences. It equals to select any number from 1 to n from this whole array arr[n]. Select from n numbers, for each number, we have 2 options to choose it or not. the following picture shows we how to calculate the total possibilities, but we don't need the one empty case.

![image](../assets/combinations.png ':size=590x297')

step2: sort our array first in asc order

step3: process our array
use two pointers, `i->min`, `j->max`,  all possible options between [i, j] is `2^(j - i)`, I will explain in the following picture:

![image](../assets/choices.png ':size=393x224')


step 4: my final solution
```
 public int numSubseq(int[] nums, int target) {
	 int mod = 1000000007;
     int res = 0;
     int n = nums.length;
     int[] p = new int[n+1];
     p[0] = 1;
     for(int i = 1; i<=n; i++)
     {
        p[i] = p[i-1] * 2 % mod;
     }
     Arrays.sort(nums);
     int l = 0; 
     int r = n-1;
     while(l<=r)
     {
         while(l<=r && nums[l]+nums[r] > target)
             r--;
         if(l>r)
            break;
         res =  (p[r-l] + res) % mod;
         l++;
     }
     return res;
}
```

### 




