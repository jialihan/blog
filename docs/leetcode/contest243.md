## Leetcode Contest 243 - JavaScript

#### I. [1880. Check if Word Equals Summation of Two Words](#question-1)

#### II. [1881. Maximum Value after Insertion](#question-2)

#### III. [1882. Process Tasks Using Servers](#question-3)

#### IV. [1883. Minimum Skips to Arrive at Meeting On Time](#question-4)

#### V. [Summary](#question-5)

<div id="question-1"/>

### [1880. Check if Word Equals Summation of Two Words](https://leetcode.com/problems/check-if-word-equals-summation-of-two-words/)

The **letter value** of a letter is its position in the alphabet **starting from 0** (i.e. `'a' -> 0`, `'b' -> 1`, `'c' -> 2`, etc.).

The **numerical value** of some string of lowercase English letters `s` is the **concatenation** of the **letter values** of each letter in `s`, which is then **converted** into an integer.

- For example, if `s = "acb"`, we concatenate each letter's letter value, resulting in `"021"`. After converting it, we get `21`.

You are given three strings `firstWord`, `secondWord`, and `targetWord`, each consisting of lowercase English letters `'a'` through `'j'` **inclusive**.

Return `true` _if the **summation** of the **numerical values** of_ `firstWord` _and_ `secondWord` _equals the **numerical value** of_ `targetWord`_, or_ `false` _otherwise._

**Example 1:**
**Input:** firstWord = "acb", secondWord = "cba", targetWord = "cdb"
**Output:** true
**Explanation:**
The numerical value of firstWord is "acb" -> "021" -> 21.
The numerical value of secondWord is "cba" -> "210" -> 210.
The numerical value of targetWord is "cdb" -> "231" -> 231.
We return true because 21 + 210 == 231.

**Example 2:**
**Input:** firstWord = "aaa", secondWord = "a", targetWord = "aab"
**Output:** false
**Explanation:**
The numerical value of firstWord is "aaa" -> "000" -> 0.
The numerical value of secondWord is "a" -> "0" -> 0.
The numerical value of targetWord is "aab" -> "001" -> 1.
We return false because 0 + 0 != 1.

**My solution:**

```js
var isSumEqual = function (firstWord, secondWord, targetWord) {
  var s1 = [...firstWord].map((el) => getDigit(el)).join("");
  var s2 = [...secondWord].map((el) => getDigit(el)).join("");
  var s3 = [...targetWord].map((el) => getDigit(el)).join("");
  return parseInt(s1) + parseInt(s2) === parseInt(s3);
};
function getDigit(c) {
  return c.charCodeAt(0) - 97;
}
```

**Reference:**
discussion article: [JavaScript Simple Parse Integer solution](https://leetcode.com/problems/check-if-word-equals-summation-of-two-words/discuss/1268824/JavaScript-Simple-ParseInt-Solution)

<div id="question-2"/>

### [1881. Maximum Value after Insertion](https://leetcode.com/problems/maximum-value-after-insertion/)

You are given a very large integer `n`, represented as a string,​​​​​​ and an integer digit `x`. The digits in `n` and the digit `x` are in the **inclusive** range `[1, 9]`, and `n` may represent a **negative** number.

You want to **maximize** `n`**'s numerical value** by inserting `x` anywhere in the decimal representation of `n`​​​​​​. You **cannot** insert `x` to the left of the negative sign.

- For example, if `n = 73` and `x = 6`, it would be best to insert it between `7` and `3`, making `n = 763`.
- If `n = -55` and `x = 2`, it would be best to insert it before the first `5`, making `n = -255`.

Return _a string representing the **maximum** value of_ `n`_​​​​​​ after the insertion_.

**Example 1:**
**Input:** n = "99", x = 9
**Output:** "999"
**Explanation:** The result is the same regardless of where you insert 9.

**Example 2:**
**Input:** n = "-13", x = 2
**Output:** "-123"
**Explanation:** You can make n one of {-213, -123, -132}, and the largest of those three is -123.

**Analysis:**
Rules:

- "negative": keep the number as smaller as possible, keep the `ascending order`, find the first index that `>= insert number`
- "positive": keep the number as large as possible, keep the `descending order`, find the first index that `<= insert number`

**My solution:**

```js
var maxValue = function (n, x) {
  var neg = false;
  if (n[0] === "-") {
    neg = true;
  }
  var res;
  for (var i = 0; i <= n.length; i++) {
    if (
      (!neg && parseInt(n[i]) < x) ||
      (neg && parseInt(n[i]) > x) ||
      i === n.length
    ) {
      res = n.slice(0, i) + x + n.slice(i, n.length);
      break;
    }
  }
  return res;
};
```

**Reference:**
discussion article: [JavaScript - insert char by rules](<https://leetcode.com/problems/maximum-value-after-insertion/discuss/1268843/JavaScript-Insert-Char-by-rules-O(N)>)

<div id="question-3"/>

### [1882. Process Tasks Using Servers](https://leetcode.com/problems/process-tasks-using-servers/)

You are given two **0-indexed** integer arrays `servers` and `tasks` of lengths `n`​​​​​​ and `m`​​​​​​ respectively. `servers[i]` is the **weight** of the `i​​​​​​th`​​​​ server, and `tasks[j]` is the **time needed** to process the `j​​​​​​th`​​​​ task **in seconds**.

Tasks are assigned to the servers using a **task queue**. Initially, all servers are free, and the queue is **empty**.

At second `j`, the `jth` task is **inserted** into the queue (starting with the `0th` task being inserted at second `0`). As long as there are free servers and the queue is not empty, the task in the front of the queue will be assigned to a free server with the **smallest weight**, and in case of a tie, it is assigned to a free server with the **smallest index**.

If there are no free servers and the queue is not empty, we wait until a server becomes free and immediately assign the next task. If multiple servers become free at the same time, then multiple tasks from the queue will be assigned **in order of insertion** following the weight and index priorities above.

A server that is assigned task `j` at second `t` will be free again at second `t + tasks[j]`.

Build an array `ans`​​​​ of length `m`, where `ans[j]` is the **index** of the server the `j​​​​​​th` task will be assigned to.

Return _the array_ `ans`​​​​.

**Example 1:**
**Input:** servers = [3,3,2], tasks = [1,2,3,2,1,2]
**Output:** [2,2,0,2,1,2]
**Explanation:** Events in chronological order go as follows:

- At second 0, task 0 is added and processed using server 2 until second 1.
- At second 1, server 2 becomes free. Task 1 is added and processed using server 2 until second 3.
- At second 2, task 2 is added and processed using server 0 until second 5.
- At second 3, server 2 becomes free. Task 3 is added and processed using server 2 until second 5.
- At second 4, task 4 is added and processed using server 1 until second 5.
- At second 5, all servers become free. Task 5 is added and processed using server 2 until second 7.

**Example 2:**
**Input:** servers = [5,1,4,3,2], tasks = [2,1,2,4,5,2,1]
**Output:** [1,4,1,4,1,3,2]
**Explanation:** Events in chronological order go as follows:

- At second 0, task 0 is added and processed using server 1 until second 2.
- At second 1, task 1 is added and processed using server 4 until second 2.
- At second 2, servers 1 and 4 become free. Task 2 is added and processed using server 1 until second 4.
- At second 3, task 3 is added and processed using server 4 until second 7.
- At second 4, server 1 becomes free. Task 4 is added and processed using server 1 until second 9.
- At second 5, task 5 is added and processed using server 3 until second 7.
- At second 6, task 6 is added and processed using server 2 until second 7.

**Analysis:**
**All I need is to make this imported library to support PQ is sorted by weight then index.**
**JS - PriorityQueue allowed in leetcode:** [https://github.com/datastructures-js/priority-queue#enqueueelement-priority](https://github.com/datastructures-js/priority-queue#enqueueelement-priority)

I don't like this library, not so good enough for JavaScript datastructure as I mentioned before.
**Finally, i figured out:**

```js
// sort by weight, then by index
// trick here: priority is (weight * max_length + index)
const pq = new MinPriorityQueue({ priority: (el) => el[0] * 200000 + el[1] });
```

**JS solution:**

```js
var assignTasks = function (servers, tasks) {
  var s = servers.map((el, i) => [el, i, 0]); // [weight, index, time]
  var n = s.length;
  var m = tasks.length;
  var ans = new Array(m).fill(null);

  // PQ: sort by smallest weight then by index
  // trick here: priority = (weight * max_length + index)
  const pq = new MinPriorityQueue({ priority: (el) => el[0] * 200000 + el[1] });
  // BQ: backup queue sort by time
  const bq = new MinPriorityQueue({ priority: (el) => el[2] });
  s.forEach((el) => {
    pq.enqueue(el);
  });

  var curTime = 0;
  var j = 0; // index of next task to be exec
  while (j < m) {
    curTime = Math.max(curTime, j);

    // pull over all valid time server from backup queue
    while (!bq.isEmpty() && bq.front().element[2] <= curTime) {
      var tmp = bq.front().element;
      bq.dequeue();
      pq.enqueue(tmp);
    }

    // when all servers are NOT available, update curTime
    if (pq.isEmpty()) {
      curTime = bq.front().element[2];
      continue;
    }

    // process current task
    cur = pq.front().element;
    pq.dequeue();
    ans[j] = cur[1];
    var newServer = [...cur];
    newServer[2] = curTime + tasks[j];
    bq.enqueue(newServer);

    // next task
    j++;
  }

  return ans;
};
```

**Reference:**
see this discussion article here: [use two PriorityQueue - tricky way to support customized sort](https://leetcode.com/problems/process-tasks-using-servers/discuss/1241306/JavaScript-use-two-PriorityQueue-tricky-way-to-support-customized-sort)

<div id="question-4" />

### [1883. Minimum Skips to Arrive at Meeting On Time](https://leetcode.com/problems/minimum-skips-to-arrive-at-meeting-on-time/)

You are given an integer `hoursBefore`, the number of hours you have to travel to your meeting. To arrive at your meeting, you have to travel through `n` roads. The road lengths are given as an integer array `dist` of length `n`, where `dist[i]` describes the length of the `ith` road in **kilometers**. In addition, you are given an integer `speed`, which is the speed (in **km/h**) you will travel at.

After you travel road `i`, you must rest and wait for the **next integer hour** before you can begin traveling on the next road. Note that you do not have to rest after traveling the last road because you are already at the meeting.

- For example, if traveling a road takes `1.4` hours, you must wait until the `2` hour mark before traveling the next road. If traveling a road takes exactly `2` hours, you do not need to wait.

However, you are allowed to **skip** some rests to be able to arrive on time, meaning you do not need to wait for the next integer hour. Note that this means you may finish traveling future roads at different hour marks.

- For example, suppose traveling the first road takes `1.4` hours and traveling the second road takes `0.6` hours. Skipping the rest after the first road will mean you finish traveling the second road right at the `2` hour mark, letting you start traveling the third road immediately.

Return _the **minimum number of skips required** to arrive at the meeting on time, or_ `-1` _if it is **impossible**_.

**Example 1:**
**Input:** dist = [1,3,2], speed = 4, hoursBefore = 2
**Output:** 1
**Explanation:**
Without skipping any rests, you will arrive in (1/4 + 3/4) + (3/4 + 1/4) + (2/4) = 2.5 hours.
You can skip the first rest to arrive in ((1/4 + 0) + (3/4 + 0)) + (2/4) = 1.5 hours.
Note that the second rest is shortened because you finish traveling the second road at an integer hour due to skipping the first rest.

**Example 2:**
**Input:** dist = [7,3,5,5], speed = 2, hoursBefore = 10
**Output:** 2
**Explanation:**
Without skipping any rests, you will arrive in (7/2 + 1/2) + (3/2 + 1/2) + (5/2 + 1/2) + (5/2) = 11.5 hours.
You can skip the first and third rest to arrive in ((7/2 + 0) + (3/2 + 0)) + ((5/2 + 0) + (5/2)) = 10 hours.

**Analysis:**

1. Floating number calculation precision Error:

```text
3.0000000000000004 ceiled num:  4  --> we want 3
9.000000000000002 ceiled num:  10 --> we want 9
```

2. Fix the dp with `eps - a very small number` to correct double number floating point
   Then I find the problem is that when we do addition & divide on floating point, it might sometimes **exceed** a little bit:

```js
dp[i][k] = Math.ceil(dp[i - 1][k] + d / speed);
```

Then the fix is that:

```
var eps = Math.pow(10, -8);
dp[i][k] = Math.ceil(dp[i-1][k] + (d / speed) - eps);
```

**JS solution:**

```js
var minSkips = function (dist, speed, hoursBefore) {
  // dp[i][k]: at index i total with k skips, k = 0,...i, i = 1,2,...n
  // initial value: dp[0][0] = 0
  // relation: dp[i][k] = min(dp[i-1][k] + non-skip, dp[i-1][k-1] + skip)
  // = min(dp[i-1][k]+Math.ceil(d/speed), dp[i-1][k-1]+(d/speed));
  var eps = Math.pow(10, -8);
  var n = dist.length;
  var dp = new Array(n + 1)
    .fill(0)
    .map((el) => new Array(n + 1).fill(Infinity)); // index is 1 based
  dp[0][0] = 0;
  for (var i = 1; i <= n - 1; i++) {
    var d = dist[i - 1];
    for (var k = 0; k <= i; k++) {
      dp[i][k] = Math.ceil(dp[i - 1][k] + d / speed - eps);
      if (k > 0) {
        dp[i][k] = Math.min(dp[i][k], dp[i - 1][k - 1] + d / speed);
      }
    }
  }
  // Goal: find the min time at the second last index
  // since last index is a fixed time NOT to wait
  var lastTime = dist[n - 1] / speed;
  for (var k = 0; k <= n - 1; k++) {
    if (dp[n - 1][k] + lastTime <= hoursBefore) {
      return k;
    }
  }
  return -1;
};
```

**Reference:**
see the discussion article here: [JavaScript - figured out: why I need to fix the floating point precision issue](https://leetcode.com/problems/minimum-skips-to-arrive-at-meeting-on-time/discuss/1248663/JavaScript-figured-out:-why-I-need-to-fix-the-floating-point-precision-issue)

<div id="question-5"/>

### Summary

- 4427 / 12835
- **Q3: learned the trick how to sort by one priority number**
- keep moving!
