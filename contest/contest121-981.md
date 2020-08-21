## contest 121

981. Time Based Key-Value Store.md

### Problem Summary
set get key value and timestamp
get: return the most recent key value with ts<=timestamp

### Approach
using a hashmap, the value is a map sorted with the timestamp.

```cpp
    unordered_map<string, map<int, string>> m;
    void set(string key, string value, int timestamp) {
      m[key].insert({ timestamp, value });
    }
    string get(string key, int timestamp) {
      auto it = m[key].upper_bound(timestamp);
      return it == m[key].begin() ? "" : prev(it)->second;
    }  
```    

982. Triples with Bitwise AND Equal To Zero.md

### Problem summary
return the number of all triplets so that A&B&C=0

### Approach
straightforward: first evaluate all A&B. Store the results into a hashmap the result vs occurences.
and then evaluate the result & C

```cpp
    int countTriplets(vector<int>& A) {
        unordered_map<int,int> mp;
        int n=A.size();
        int ans=0;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++) mp[A[i]&A[j]]++;
        }
        for(int i=0;i<n;i++)
        {
            for(auto it=mp.begin();it!=mp.end();it++)
                if((it->first&A[i])==0) ans+=it->second;
        }
        return ans;
    }
```    

983. Minimum Cost For Tickets.md

### Problem Summary
Given a list of days, we can buy tickets for single day, 7-day and 30-days at different prices. To cover all the days, what is the lowest cost

### Approach
This is a dp problem, can also be combined with two pointer approach
dp[i] is the min cost for the subproblem ending at days[i]

It is:
min(dp[i-1]+cost1,dp[7th]+cost7,dp[30th]+cost30)

```cpp
    int mincostTickets(vector<int>& days, vector<int>& costs) {
        int n=days.size();
        sort(days.begin(),days.end());
        vector<int> dp(n+1);//dp[i] is the min cost ending at ith day
        for(int i=1;i<=n;i++)
        {
            dp[i]=dp[i-1]+costs[0];
            for(int j=i-1;j>0;j--)
            {
                if(days[i-1]-days[j-1]>=30) break;
                dp[i]=min(dp[i],dp[j-1]+costs[2]);
                if(days[i-1]-days[j-1]<7) dp[i]=min(dp[i],dp[j-1]+costs[1]);
            }
        }
        return dp[n];
    }
```

984. String Without AAA or BBB.md

### Problem Summary
given m A and n B, return a string without AAA or BBB

### Greedy solution
typical: make a greedy move and reduce to sub problem

```cpp
    string strWithout3a3b(int A, int B) {
        if(A == 0) return string(B, 'b');
        else if(B == 0) return string(A, 'a');
        else if(A == B) return "ab" + strWithout3a3b(A - 1, B - 1);
        else if(A > B) return "aab" + strWithout3a3b(A - 2, B - 1);
        else return strWithout3a3b(A - 1, B - 2) + "abb";
    }
```

