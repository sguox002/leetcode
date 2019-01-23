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


921. Minimum Add to Make Parentheses Valid
cannot just count the ( and ) for example )(. The ( shall have a ) followed. The ) shall have a preceding (. We use a stack for this, removing all those paired.
```cpp
    int minAddToMakeValid(string S) {
        //to make it balanced, the () shall be the same amount
        //one we have a ) we need have a balanced in its front
        //once we have a ( we need have a balanced in the behind
        stack<char> st;
        int ans=0;
        for(int i=0;i<S.size();i++)
        {
            if(S[i]=='(') st.push(S[i]);
            else 
            {
                if(st.size()) st.pop();
                else ans++; 
            }
        }
        return ans+st.size();
    }
```

using two pointers for the two conditions will save the extra storage using stack


861. Score After Flipping Matrix
a move choose any row or column and toggle its value.
Each row represents a binary number, return the largest sum of all these numbers.
greedy choice: 
the MSB shall all toggles to 1, by toggling the row
the non-MSB shall have more 1s than 0s. toggling columns

763. Partition Labels
split the string so that each char only appear in one part.
straightforward:
record each char's start and ending position and then merge the segments if they overlap.
efficient one:
don't use hashmap but use array for each char and record its ending index. Merging the segments only matters the ending.
so use two passes:
```cpp
    vector<int> partitionLabels(string S) {
        vector<int> charIdx(26, 0);
        for(int i = 0; i < S.size(); i++){
            charIdx[S[i]-'a'] = i;
        }
        
        vector<int> results;
        
        int maxIdx = -1, lastIdx = 0;
        for(int i = 0; i < S.size(); i++){
            maxIdx = max(maxIdx, charIdx[S[i]-'a']);
            if(i == maxIdx) {
                results.push_back(maxIdx - lastIdx + 1);
                lastIdx = i+1;
            }
        }
        return results;
    }
```

406. Queue Reconstruction by Height
each person has a height and number of person taller or equal to him in front of him.
reconstruct the queue
sort the people in descend order but the rank in ascending order, so the same height people is in order. Then insert each lower person.
This is a greedy choice.
for example: [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
sort as: [7,0], [7,1], [6,1],[5,0],[5,2],[4,4]
each step:
[7,0]
[7,0], [7,1]
[7,0],[6,1],[7,1]
[5,0],[7,0],[6,1],[7,1]
[5,0],[7,0],[6,1],[5,2],[7,1]
[5,0],[7,0],[6,1],[5,2],[4,4],[7,1]
sorting is the key to make sure all member inside the array are taller than current one.
```cpp
bool cmp(pair<int,int>& a,pair<int,int>& b) {return a.first>b.first || (a.first==b.first && a.second<b.second);}
    vector<pair<int, int>> reconstructQueue(vector<pair<int, int>>& people) {
        //reverse: we sort the highest person first, they are ordered
        //then the next, 
        sort(people.begin(),people.end(),cmp); //descending order
        vector<pair<int,int>> vs;
        for(int i=0;i<people.size();i++)
        {
            vs.insert(vs.begin()+people[i].second,people[i]);
            for(int i=0;i<vs.size();i++) cout<<"("<<vs[i].first<<" "<<vs[i].second<<") ";cout<<endl;
        }
        return vs;
    }
```

714. Best Time to Buy and Sell Stock with Transaction Fee
see dp solution
dp[i]=max(dp[i-1],dp[j]+price[i]-price[j]-fee) j=0 to i-1
dp[i]=max(dp[i-1],price[i]+max(dp[j]-price[j]-fee)) j=0 to i-1 and this can be updated iteratively and reducing the complexity from O(N^2) to O(N)
```cpp
    int maxProfit(vector<int>& prices, int fee) {
        int n=prices.size();
        vector<int> dp(n);//dp is the max profit ending at ith day
        int tmax=INT_MIN;
        for(int i=1;i<n;i++)
        {
            tmax=max(tmax,dp[i-1]-prices[i-1]-fee);
            dp[i]=max(dp[i-1],prices[i]+tmax);
        }
        return dp[n-1];
    }
```    
greedy choice:
when current price>previous price+fee, perform a transaction, is this a safe move?
a<b<c<d, and b>a+fee, d>c+fee, then b-a-fee+d-c-fee ==? d-a-fee not really. so this approach is incorrect.
for example fee is 1, 4,6,8,10, we get 3 using first approach, 5 for 2nd approach.
we buy at 4, sell at 6 (equiv sell at 5), buy at 6 sell at 8 (equiv sell at 7). In this case we cannot start a new transaction.
for example 4,6,3,8. buy at 4 sell at 6, profit is 1, buy at 3 sell at 8, profit is 4, total profit is 5. 
for example 4,6,5,8. buy at 4 sell at 6, profit is 1, buy at 5 sell at 8, profit is 2, total profit is 3. 
so what is the safe move?
we shall tolerate some variation, only when price[i]<price[i-1]-fee we can sell at i-1 and buy at i. (i.e the dip shall be deep enough to cover the transaction fee, otherwise it is not worthful).
```cpp
    int maxProfit(vector<int>& prices, int fee) {
        //greedy solution
        //only when price[i]<price[i-1]-fee we can start buy at ith day
        int n=prices.size();
        if(n<2) return 0;
        int minimum=prices[0];
        int maxprof=0;
        for(int i=1;i<n;i++)
        {
            if(prices[i]<minimum) //prices[i-1]-fee
                minimum=prices[i];
            else if(prices[i]>minimum+fee)
            {
                maxprof+=prices[i]-minimum-fee;
                minimum=prices[i]-fee;
            }
        }
        return maxprof;
    }
```









