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





