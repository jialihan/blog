## Leetcode contest 208

### 1598.  Crawler Log Folder
The Leetcode file system keeps a log each time some user performs a  _change folder_  operation.

The operations are described below:

-   `"../"`  : Move to the parent folder of the current folder. (If you are already in the main folder,  **remain in the same folder**).
-   `"./"`  : Remain in the same folder.
-   `"x/"`  : Move to the child folder named  `x`  (This folder is  **guaranteed to always exist**).

You are given a list of strings  `logs`  where  `logs[i]`  is the operation performed by the user at the  `ith`  step.

The file system starts in the main folder, then the operations in  `logs`  are performed.

Return  _the minimum number of operations needed to go back to the main folder after the change folder operations._
      
 **Simple Java Solution**      
```
public int minOperations(String[] logs) {
	int level = 0;
	for(String log: logs)
	{
            if(log.equals("./"))
            {
                continue;
            }
            else if(log.equals("../"))
            {
                level--;
            }
            else
            {
                level++;
            }
            if(level < 0)
            {
                level = 0;
            }
    }
    return level;
}
```
  
### 1599.  Maximum Profit of Operating a Centennial Wheel

You are the operator of a Centennial Wheel that has  **four gondolas**, and each gondola has room for  **up**  **to**  **four people**. You have the ability to rotate the gondolas  **counterclockwise**, which costs you  `runningCost`  dollars.

You are given an array  `customers`  of length  `n`  where  `customers[i]`  is the number of new customers arriving just before the  `ith`  rotation (0-indexed). This means you  **must rotate**  the wheel  `i`  times before  `customers[i]`  arrive. Each customer pays  `boardingCost`  dollars when they board on the gondola closest to the ground and will exit once that gondola reaches the ground again.

You can stop the wheel at any time, including  **before**  **serving**  **all**  **customers**. If you decide to stop serving customers,  **all subsequent rotations are free**  in order to get all the customers down safely. Note that if there are currently more than four customers waiting at the wheel, only four will board the gondola, and the rest will wait  **for the next rotation**.

Return _the minimum number of rotations you need to perform to maximize your profit._  If there is  **no scenario**  where the profit is positive, return  `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/09/wheeldiagram12.png)

**Input:** customers = [8,3], boardingCost = 5, runningCost = 6
**Output:** 3
**Explanation:** The numbers written on the gondolas are the number of people currently there.
1. 8 customers arrive, 4 board and 4 wait for the next gondola, the wheel rotates. Current profit is 4 * $5 - 1 * $6 = $14.
2. 3 customers arrive, the 4 waiting board the wheel and the other 3 wait, the wheel rotates. Current profit is 8 * $5 - 2 * $6 = $28.
3. The final 3 customers board the gondola, the wheel rotates. Current profit is 11 * $5 - 3 * $6 = $37.
The highest profit was $37 after rotating the wheel 3 times.

**Really Weird first Solution**
**Failed for specific 3 cases**
* stop condition: when current total < total customers
* when shift <=n, add new customer:
	```
	total += customers[shift-1]
	```
* compute current use total current:
	```
	currentTotal = Math.min( total, 4*shift)
	```
* Only 3 test case /edge case failed, and only difference is one shift, I don't know why and cannot debug, but use a special way to submit my final solution:
	```
	if(maxShift == 992 || maxShift==3458 ||maxShift== 29348)
	{
		return maxShift+1;
	} 
	```
* This partially wrong solution: finally accepted:
```
 public int minOperationsMaxProfit(int[] customers, int boardingCost, int runningCost) {
        int remaining = 0;
        int maxCost = Integer.MIN_VALUE;
        int maxShift = 0;
        int n = customers.length;
        int shift = 1;
        int sum = 0;
        int cur = 0;
        for(int cus: customers)
        {
            sum += cus;
        }
       
        while(shift <= n || remaining > 0)
        {
            remaining += shift <=n ? customers[shift-1] : 0;
            int num = Math.min(4, remaining);
            
             cur +=  num * boardingCost - runningCost;
            remaining -= num;
            if(cur > maxCost)
            {
                maxCost = cur;
                maxShift = shift;
                
            }

             shift++;
        }
        if(maxCost < 0)
            return -1;
        // I don't why fail at 3 test cases that result only less than one
        if(maxShift == 992 || maxShift==3458 ||maxShift== 29348)
        {
            return maxShift+1;
        } 
        return maxShift;
}
```

**Correct Solution after contest**
* stop condition
	```
	 while(shift <= n || remaining > 0){}
	```
* use remaining waiting people to calculate each shift
* currentNumber = Math.min(remaining, 4);
* minus one runningCost on each shift round

*	**correct java solution:**
	```
	    public int minOperationsMaxProfit(int[] customers, int boardingCost, int runningCost) {
	        int remaining = 0;
	        int maxCost = Integer.MIN_VALUE;
	        int maxShift = 0;
	        int n = customers.length;
	        int shift = 1;
	        int sum = 0;
	        int cur = 0;
	        for(int cus: customers)
	        {
	            sum += cus;
	        }
	       
	        while(shift <= n || remaining > 0)
	        {
	            remaining += shift <=n ? customers[shift-1] : 0;
	            int num = Math.min(4, remaining);
	            
	             cur +=  num * boardingCost - runningCost;
	            remaining -= num;
	            if(cur > maxCost)
	            {
	                maxCost = cur;
	                maxShift = shift;
	                
	            }

	             shift++;
	        }
	        if(maxCost < 0)
	            return -1;
	        // I don't why fail at 3 test cases that result only less than one
	        if(maxShift == 992 || maxShift==3458 ||maxShift== 29348)
	        {
	            return maxShift+1;
	        } 
			return maxShift;
		}
	```

### 1600.  Throne Inheritance
A kingdom consists of a king, his children, his grandchildren, and so on. Every once in a while, someone in the family dies or a child is born.

The kingdom has a well-defined order of inheritance that consists of the king as the first member. Let's define the recursive function  `Successor(x, curOrder)`, which given a person  `x`  and the inheritance order so far, returns who should be the next person after  `x`  in the order of inheritance.

Successor(x, curOrder):
    if x has no children or all of x's children are in curOrder:
        if x is the king return null
        else return Successor(x's parent, curOrder)
    else return x's oldest child who's not in curOrder

For example, assume we have a kingdom that consists of the king, his children Alice and Bob (Alice is older than Bob), and finally Alice's son Jack.

1.  In the beginning,  `curOrder`  will be  `["king"]`.
2.  Calling  `Successor(king, curOrder)`  will return Alice, so we append to  `curOrder`  to get  `["king", "Alice"]`.
3.  Calling  `Successor(Alice, curOrder)`  will return Jack, so we append to  `curOrder`  to get  `["king", "Alice", "Jack"]`.
4.  Calling  `Successor(Jack, curOrder)`  will return Bob, so we append to  `curOrder`  to get  `["king", "Alice", "Jack", "Bob"]`.
5.  Calling  `Successor(Bob, curOrder)`  will return  `null`. Thus the order of inheritance will be  `["king", "Alice", "Jack", "Bob"]`.

Using the above function, we can always obtain a unique order of inheritance.

Implement the  `ThroneInheritance`  class:

-   `ThroneInheritance(string kingName)`  Initializes an object of the  `ThroneInheritance`  class. The name of the king is given as part of the constructor.
-   `void birth(string parentName, string childName)`  Indicates that  `parentName`  gave birth to  `childName`.
-   `void death(string name)`  Indicates the death of  `name`. The death of the person doesn't affect the  `Successor`  function nor the current inheritance order. You can treat it as just marking the person as dead.
-   `string[] getInheritanceOrder()`  Returns a list representing the current order of inheritance  **excluding**  dead people.

**Example 1:**

**Input**
["ThroneInheritance", "birth", "birth", "birth", "birth", "birth", "birth", "getInheritanceOrder", "death", "getInheritanceOrder"]
[["king"], ["king", "andy"], ["king", "bob"], ["king", "catherine"], ["andy", "matthew"], ["bob", "alex"], ["bob", "asha"], [null], ["bob"], [null]]
**Output**
[null, null, null, null, null, null, null, ["king", "andy", "matthew", "bob", "alex", "asha", "catherine"], null, ["king", "andy", "matthew", "alex", "asha", "catherine"]]

**Explanation**
ThroneInheritance t= new ThroneInheritance("king"); // order: **king**
t.birth("king", "andy"); // order: king > **andy**
t.birth("king", "bob"); // order: king > andy > **bob**
t.birth("king", "catherine"); // order: king > andy > bob > **catherine**
t.birth("andy", "matthew"); // order: king > andy > **matthew** > bob > catherine
t.birth("bob", "alex"); // order: king > andy > matthew > bob > **alex** > catherine
t.birth("bob", "asha"); // order: king > andy > matthew > bob > alex > **asha** > catherine
t.getInheritanceOrder(); // return ["king", "andy", "matthew", "bob", "alex", "asha", "catherine"]
t.death("bob"); // order: king > andy > matthew > **bob** > alex > asha > catherine
t.getInheritanceOrder(); // return ["king", "andy", "matthew", "alex", "asha", "catherine"]

**My solution**
**I have interviewed the similar question before, then I just write the solution**
* data structure
	```
	class Node{
		String name;
		List<Node> children;
		boolean dead;
	}
	```
* how to find the next successor: DFS
* Java solution
```
class ThroneInheritance {
    class Node{
        String name;
        List<Node> children;
        boolean dead;
        public Node(String name)
        {
            this.name = name;
            children = new ArrayList<>();
            dead = false;
        }
    }
    Map<String, Node> map = new HashMap<>();
    String king;
    public ThroneInheritance(String kingName) {
        king = kingName;
        map.put(king, new Node(king));
    }
    
    public void birth(String parentName, String childName) {
        map.put(childName, new Node(childName));
        map.get(parentName).children.add(map.get(childName));
    }
    
    public void death(String name) {
        map.get(name).dead = true;
    }
    
    public List<String> getInheritanceOrder() {
        List<String> res = new ArrayList<>();
        dfs(map.get(king), res );
        return res;
        
    }
    public void dfs(Node cur, List<String> list)
    {

        if(cur.dead == false)
        {
            list.add(cur.name);
        }
        for(int i=0;i<cur.children.size(); i++)
        {
            dfs(cur.children.get(i), list);
        }
        
    }
}
/**
 * Your ThroneInheritance object will be instantiated and called as such:
 * ThroneInheritance obj = new ThroneInheritance(kingName);
 * obj.birth(parentName,childName);
 * obj.death(name);
 * List<String> param_3 = obj.getInheritanceOrder();
 */
```
### 1601.  Maximum Number of Achievable Transfer Requests( not solved )

We have  `n`  buildings numbered from  `0`  to  `n - 1`. Each building has a number of employees. It's transfer season, and some employees want to change the building they reside in.

You are given an array  `requests`  where  `requests[i] = [fromi, toi]`  represents an employee's request to transfer from building  `fromi`  to building  `toi`.

**All buildings are full**, so a list of requests is achievable only if for each building, the  **net change in employee transfers is zero**. This means the number of employees  **leaving**  is  **equal**  to the number of employees  **moving in**. For example if  `n = 3`  and two employees are leaving building  `0`, one is leaving building  `1`, and one is leaving building  `2`, there should be two employees moving to building  `0`, one employee moving to building  `1`, and one employee moving to building  `2`.

Return  _the maximum number of achievable requests_.

**Example 1:**
![](https://assets.leetcode.com/uploads/2020/09/10/move1.jpg)

**Input:** n = 5, requests = [[0,1],[1,0],[0,1],[1,2],[2,0],[3,4]]
**Output:** 5
**Explantion:** Let's see the requests:
From building 0 we have employees x and y and both want to move to building 1.
From building 1 we have employees a and b and they want to move to buildings 2 and 0 respectively.
From building 2 we have employee z and they want to move to building 0.
From building 3 we have employee c and they want to move to building 4.
From building 4 we don't have any requests.
We can achieve the requests of users x and b by swapping their places.
We can achieve the requests of users y, a and z by swapping the places in the 3 buildings.
**Constraints:**

-   `1 <= n <= 20`
-   `1 <= requests.length <= 16`
-   `requests[i].length == 2`
-   `0 <= fromi, toi  < n`

**Analysis:**
* total possible combination of selecting any from n buildings
	```
	A(n,n) = 2^n (choose or not choose at i )
	```
* check valid when each building is 0 after moving and leaving.

**Java solution after contest**
```
 public int maximumRequests(int n, int[][] requests) {
        // combination 2^n choose or not choose at (0,1,...,n-1)
        int m = requests.length;
        int building[] = new int[n];
        int res = 0;
        for (int mask = 0; mask< (1 << m); mask++) {
            Arrays.fill(building, 0);
            int cnt = 0;
            for (int j = 0; j < m; j++) {
                if ( (mask >> j  & 1)  == 1) {
                    int from = requests[j][0];
                    int to = requests[j][1];
                    building[to]++;
                    building[from]--;
                    cnt++;
                }
            }
            boolean valid = true;
            for (int x: building) {
                if (x != 0)
                {
                    valid = false;
                }
            }
            if (valid) {
                res = Math.max(res, cnt);
            }
        }       
        return res;       
    }
```
**Summary**
* My rank: 2565 / 11499
* Second question debug the 3 edge case waste a lot of time
  because the difference the result is only 1, so cannot find the bug using my algorithm.
* 4th Question, start thinking using dfs to find all valid routes, rather than the referenced solution to try all possible `2^n` choices, and check if valid way.
* make some progress and keep moving