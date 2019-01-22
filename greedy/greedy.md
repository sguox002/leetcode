Greedy:
choose a first move and proof it is a safe move which is consistent to a global optimal solution. Then the problem is reduced to a smaller sized subproblem.
Generally sort will improve the efficiency.

example: 
- n digits to form a largest number, each time choose the largest number from remaining digits
- min refueling stations: refill at the farthest reachable station
- min group of children with age difference <=1, sort first and use segment to cover the leftmost.
- fractional knapsack: sort with the value/weight, and choose as much as possible the current highest value.

If your first move is not safe, the solution is incorrect.

944. Delete Columns to Make Sorted
just count the unsorted column. To improve efficiency, we just compare neigboring char, O(NM)
```cpp
    int minDeletionSize(vector<string>& A) {
        //using dp: adding one char, if the column is sorted, it is the length of dp[i-1]
        int m=A.size(),n=A[0].size(); //m is row, n is col
        int ans=0;
        for(int i=0;i<n;i++)
        {
            for(int j=1;j<m;j++) 
                if(A[j][i]<A[j-1][i]) {ans++;break;}
        }
        return ans;
    }
```

122. Best Time to Buy and Sell Stock II
as many as transactions.
Greedy: just compare with previous price, if profit >0 then make it.
```cpp
    int maxProfit(vector<int>& prices) {
        int profit=0;
        for(int i=1;i<prices.size();i++)
        {
            profit+=max(prices[i]-prices[i-1],0);
        }
        return profit;
    }
```
A transaction is defined as buying at Prices[X] and selling at Prices[Y],

the profit of the transaction
= Prices[Y] - Prices[X] 
= Prices[Y] - Prices[Y-1] +
   Prices[Y-1] - Prices[Y-2] ...
    ....
   Prices[X+1] - Prices[X] 
= D[Y] + D[Y-1] + ... + D[X+1]
= sum of D from X+1 to Y

860. Lemonade Change
greedy: always gives changes of the large bill first. 

saving the change for 5, 10, and 20 separately.

455. Assign Cookies
given a list of children greedy size, and a list of candy size, return the number of satisifcation (each child one candy)
greedy choice: bucket sort or using map to sort both the greedy and the size. Greedy choice: assign the candy to the child who first meet the requirement.
```cpp
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        //use two pointers
        int j=0;
        for(int i=0;i<s.size() && j<g.size();i++)
            if(g[j]<=s[i]) {j++;}
        return j;
    }
```
using map for bucket sorting is slower and more complicated, two pointer is more efficient.

874. Walking Robot Simulation
starting from (0,0), -2: turn left, -1: turn right, x (1,9) forward nsteps. with obstacle, then stop before the obstacle.
get the largest distance from the origin.

each step we can calculate its position and then we get the largest distance

we have 4 directions: -x, +x, -y, +y which can be represented as (-1,0) (1,0) (0,-1) (0,1)
turn right: rotate -90 degrees: 
turn left: rotate 90 degrees: 
we can use rotation matrix for the direction changes.
how to easily judge if the obstacle is on the way?
can use a string to include the x and y coordinate and move step by step and stop when we meet an obstacle.








