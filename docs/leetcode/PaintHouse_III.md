## Leetcode 1473 PaintHouse III

### Description
There is a row of m houses in a small city, each house must be painted with one of the n colors (labeled from 1 to n), some houses that has been painted last summer should not be painted again.

A neighborhood is a maximal group of continuous houses that are painted with the same color. (For example: houses = [1,2,2,3,3,2,1,1] contains 5 neighborhoods  [{1}, {2,2}, {3,3}, {2}, {1,1}]).

Given an array houses, an m * n matrix cost and an integer target where:
* houses[i]: is the color of the house i, 0 if the house is not painted yet.
* cost[i][j]: is the cost of paint the house i with the color j+1.

Return the minimum cost of painting all the remaining houses in such a way that there are exactly target neighborhoods, if not possible return -1.

 Details: [Leetcode Link](https://leetcode.com/problems/paint-house-iii/)

### Solution
#### DP 
##### step1: define state
create 3 dimension array dp[n][m+1][targtet+1]
`initial value:` assign `Integer.MAX_VALUE` to each array element 
`dp[c][i][k]: ` means at location i using color c, total has k groups
for each color c from [0,...,n-1], we init dp[c][0][0] = 0, at first.
##### step2: write state transfer equation
at index `i` house, we have total k groups, using color c to paint, we have two choices:
1) use same color with at index `i-1`, since the group number will not change, we need to use previous state `k`, so use the previous state: dp[c][i-1][k]
    ```
    dp[c][i][k] =  Math.min( dp[c][i-1][k] + cost[i-1][c],dp[c][i][k]);
    ```
2) if we use different color with index `i-1`, since the group number will increase by 1, we need to use previous state `k-1`, so we use the previous state: dp[!c][i-1][k-1]
    ```
    for(int c2 = 0; c2 < n; c2++) {
        if(c2 != c){
            dp[c][i][k] = Math.min(dp[c2][i-1][k-1]+cost[i-1][c], dp[c][i][k] );
        }
    }
    ```
##### step3: process when house at `i` already painted
since house is already painted with color `houses[i]`, then we don't need to add current new cost to paint `i`th house, so don't add the `cost[color][i]` here, we only need to update current totoal cost compared to previous minimun cost at index `i-1`
1) if use same color, group k will not change:
    ```
    if(c == houses[i]) {
        dp[c][i][k] =  Math.min(dp[c][i-1][k],dp[c][i][k]);
    }
    ```
2) if use different color, group k will increase by 1:
    ```
    if(c != houses[i]) {
        dp[c][i][k] =  Math.min(dp[c][i-1][k-1],dp[c][i][k]);
    }
    ```
##### step5: Issue about loop ranges and integer overflow
1) `1<=k<=m`: if paint m houses with (k>m), that will be impossible, so don't update cost when k>m. then using the loop to limit `k<=Math.min(currentM, target)` 
```
 for(int i=1; i<=m; i++){
            for(int k = 1; k<= Math.min(i,target); k++){
                // todo
            }
    }
```
2) `overflow: prev_cost + cost[color][i] > Integer.MAX_VALUE` will cause wrong result
because we depend on previous state `dp[c][i-1][k]` or `dp[c][i-1][k-1]`, but sometimes, the previous state is not valid, not existed, not have a chance to update, it will cause integer overflow error to final result, then we need to add a constraint to avoid the invalid use case:
```
// if same color
if(dp[c][i-1][k] != Integer.MAX_VALUE) { // todo }
// if different color 
if(dp[c2][i-1][k-1] != Integer.MAX_VALUE) { // todo }
```

step6: return result
1)  the final state we use to return will be try to find the `min_cost` in `dp[color][m][target]`, we need a for loop for each color at final state to find the min:
    ```
    int res = Integer.MAX_VALUE;
    for(int c=0; c<n; c++) {
        res = Math.min(res, dp[c][m][target]);
    }
    ```
2) if not found, we need to find -1
    ```
    return res == Integer.MAX_VALUE ? -1 : res;
    ```
#### Complete Solution java:
time: 29ms
beat: 47.16% (hmmmm. might have faster opt solution!)
```
class Solution {
    int[][][] dp;
    public int minCost(int[] houses, int[][] cost, int m, int n, int target) {
        dp = new int[n][m+1][target+1];
         for(int i = 0; i<n; i++)
               for(int j =0; j<=m; j++)
               {
                   Arrays.fill(dp[i][j], Integer.MAX_VALUE);
               }
        
        for(int c = 0; c<n; c++)
        {
            dp[c][0][0] = 0;
        }
        
       for(int i=1; i<=m; i++) {
            for(int k = 1; k<= Math.min(i,target); k++) {
                if(houses[i-1] == 0) {
                    for(int c = 0; c<n; c++) {
                      for(int c2 = 0; c2<n;c2++) {   
                          if(c2 == c) {
                             if(dp[c][i-1][k] != Integer.MAX_VALUE)
                                dp[c][i][k] = k<=i-1 ? Math.min( dp[c][i-1][k] + cost[i-1][c],dp[c][i][k]) :  dp[c][i][k];
                          }
                          else {
                             if(dp[c2][i-1][k-1] != Integer.MAX_VALUE) {
                                dp[c][i][k] = Math.min(dp[c2][i-1][k-1]+cost[i-1][c], dp[c][i][k] );
                             }
                          }
                      }
                 }
               }else { // already painted
                    int c = houses[i-1]-1;
                    for(int c2 = 0; c2<n;c2++) {
                        if(c2 == c) {
                            dp[c][i][k] =  Math.min(dp[c][i-1][k],dp[c][i][k]);
                        } else {
                            dp[c][i][k] = Math.min(dp[c2][i-1][k-1], dp[c][i][k] );
                        }
                    } 
                }
            }
        }
        int res = Integer.MAX_VALUE;
        for(int c=0; c<n; c++)
        {
            res = Math.min(res, dp[c][m][target]);
        }
        return res == Integer.MAX_VALUE ? -1 : res;
    }
}
```
