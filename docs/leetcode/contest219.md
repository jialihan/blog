##  Leetcode Contest 219 - JavaScript

I.  [1688.  Count of Matches in Tournament](#question-1)

II. [1689.  Partitioning Into Minimum Number Of Deci-Binary Numbers - not finished ](#question-2)

III. [1690.  Stone Game VII](#question-3)

IV. [1691.  Maximum Height by Stacking Cuboids - not finished](#question-4)

V. [Summary](#summary)


<div id="question-1"/>

### [1688.  Count of Matches in Tournament](https://leetcode.com/problems/count-of-matches-in-tournament/)

You are given an integer  `n`, the number of teams in a tournament that has strange rules:

-   If the current number of teams is  **even**, each team gets paired with another team. A total of  `n / 2`  matches are played, and  `n / 2`  teams advance to the next round.
-   If the current number of teams is  **odd**, one team randomly advances in the tournament, and the rest gets paired. A total of  `(n - 1) / 2`  matches are played, and  `(n - 1) / 2 + 1`  teams advance to the next round.

Return  _the number of matches played in the tournament until a winner is decided._

**Example 1:**
**Input:** n = 7
**Output:** 6
**Explanation:** Details of the tournament: 
- 1st Round: Teams = 7, Matches = 3, and 4 teams advance.
- 2nd Round: Teams = 4, Matches = 2, and 2 teams advance.
- 3rd Round: Teams = 2, Matches = 1, and 1 team is declared the winner.
Total number of matches = 3 + 2 + 1 = 6.

**My JavaScript Solution:**
```
/**
 * @param {number} n
 * @return {number}
 */
var  numberOfMatches = function (n) {
	var  res = 0;
	while (n !== 1) {
		if (n % 2 === 1) {
			res += Math.floor(n - 1) / 2;
			n = Math.floor(n - 1) / 2 + 1;
		}
		else {
			res += Math.floor(n / 2);
			n = Math.floor(n / 2);
		}
	}
	return  res;
};
```


<div id="question-2"/>

### [1689.  Partitioning Into Minimum Number Of Deci-Binary Numbers](https://leetcode.com/problems/partitioning-into-minimum-number-of-deci-binary-numbers/)

A decimal number is called  **deci-binary**  if each of its digits is either  `0`  or  `1`  without any leading zeros. For example,  `101`  and  `1100`  are  **deci-binary**, while  `112`  and  `3001`  are not.

Given a string  `n`  that represents a positive decimal integer, return  _the  **minimum**  number of positive  **deci-binary**  numbers needed so that they sum up to_ `n`_._

**Example 1:**
**Input:** n = "32"
**Output:** 3
**Explanation:** 10 + 11 + 11 = 32

**Example 2:**
**Input:** n = "82734"
**Output:** 8

**My JavaScript  Solution:** 
Don't think too complicated, use simple way !
Find the largest Number in Single Digit !
```
/**
 * @param {string} n
 * @return {number}
 */
var minPartitions = function(n) {  
    var num = [...n]
    var res = 0;
    num.forEach(el=>{
        res = Math.max(el-'0', res);
    });
    return res;  
};
```

<div id="question-3"/>

### [1690.  Stone Game VII](https://leetcode.com/problems/stone-game-vii/)

Alice and Bob take turns playing a game, with  **Alice starting first**.

There are  `n`  stones arranged in a row. On each player's turn, they can  **remove**  either the leftmost stone or the rightmost stone from the row and receive points equal to the  **sum**  of the remaining stones' values in the row. The winner is the one with the higher score when there are no stones left to remove.

Bob found that he will always lose this game (poor Bob, he always loses), so he decided to  **minimize the score's difference**. Alice's goal is to  **maximize the difference**  in the score.

Given an array of integers  `stones`  where  `stones[i]`  represents the value of the  `ith`  stone  **from the left**, return  _the  **difference**  in Alice and Bob's score if they both play  **optimally**._

**Example 1:**
**Input:** stones = [5,3,1,4,2]
**Output:** 6
**Explanation:** 
- Alice removes 2 and gets 5 + 3 + 1 + 4 = 13 points. Alice = 13, Bob = 0, stones = [5,3,1,4].
- Bob removes 5 and gets 3 + 1 + 4 = 8 points. Alice = 13, Bob = 8, stones = [3,1,4].
- Alice removes 3 and gets 1 + 4 = 5 points. Alice = 18, Bob = 8, stones = [1,4].
- Bob removes 1 and gets 4 points. Alice = 18, Bob = 12, stones = [4].
- Alice removes 4 and gets 0 points. Alice = 18, Bob = 12, stones = [].
The score difference is 18 - 12 = 6.

**Tips:**
- similar problem:  [Leetcode 877. Stone Game](https://leetcode.com/problems/stone-game/): pick left or right
- changes: here is to pick up the remaining of left or right
- edge case: when "**array.length <= 1**",  eg: [1], the score is **0** .
-  use **PreSum**[i] to calculate the score

**My JavaScript Solution：**
```js
/**
* @param  {number[]}  stones
* @return  {number}
*/
var  stoneGameVII = function (piles) {
	// build dp[] and preSum array
	var  n = piles.length;
	var  preSum = [...Array(n + 1)].fill(0);
	for (var  i = 1; i <= n; i++) {
		preSum[i] = preSum[i - 1] + piles[i - 1];
	}
	var  dp = []; // from j to i, Diff = Ali score - bob score;
	for (var  i = 0; i < n; i++) {
		dp.push([...Array(n)].fill(0));
	}
	// run the dp
	var  tmp;
	for (var  i = 0; i < n; i++)
		for (var  j = i; j >= 0; j--) {
			if (i == j) {
				dp[j][i] = 0;
			}
			else {
				var  value1 = preSum[i] - preSum[j] - dp[j][i - 1]; // take i
				var  value2 = preSum[i + 1] - preSum[j + 1] - dp[j + 1][i]; // take j
				tmp = Math.max(value1, value2);
				dp[j][i] = Math.max(dp[j][i], tmp);
			}
	}
	return  dp[0][n - 1];
};
```

<div id="question-4"/>

### [1691.  Maximum Height by Stacking Cuboids](https://leetcode.com/problems/maximum-height-by-stacking-cuboids/)

 - not finished

<div id="summary"/>

### Summary
- the 2nd problem should think it easy ! didn't understand its meaning
- good to solve the 3rd problem dp, reference to previous Stone Game, and modify the **DP solution**
- 1728 / 9290
- keep moving