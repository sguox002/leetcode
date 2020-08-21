## contest 114
953. Verifying an Alien Dictionary 4
954. Array of Doubled Pairs 5
955. Delete Columns to Make Sorted II 6
956. Tallest Billboard 8

953. Verifying an Alien Dictionary.md
### Problem Summary
Given the order of chars, check if the list of words is sorted

### idea
simple just using hashmap to map to normal order

### implementation
```cpp
    bool isAlienSorted(vector<string>& words, string order) {
        unordered_map<char,char> mp;
        for(int i=0;i<26;i++) mp[order[i]]='a'+i;
        char c=order[0];
        for(int i=0;i<words.size();i++)
        {
            for(int j=0;j<words[i].size();j++) words[i][j]=mp[words[i][j]];
        }
        
        return is_sorted(words.begin(),words.end());
    }
```    

954. Array of Doubled Pairs.md

### Problem Summary
Check if a list of numbers can be divided n and 2n pair. With negative positives.

### idea
In contest, I converted all negatives into positive and sort in a multiset.
Actually there is a flaw: even if we convert, we shall consider negatives and positives separately.

### implementation (with flaw)
```cpp
   bool canReorderDoubled(vector<int>& A) {
        multiset<int> pos,neg;
        for(int i=0;i<A.size();i++)
        {
            if(A[i]>=0) pos.insert(A[i]);
            else neg.insert(-A[i]);
        }
        if(pos.size()%2 || neg.size()%2) return 0;
        return checkpairs(pos) && checkpairs(neg);
     }
    bool checkpairs(multiset<int> ms)
    {
        auto it=ms.begin();
        while(ms.size())
        {
            it=ms.begin();
            auto it1=ms.upper_bound((*it)*2);
            --it1;
            if(it1!=ms.begin() && *it1==*it*2)
            {
                ms.erase(it1);
                ms.erase(ms.begin());
            }
            else return 0;
            
        }
        return 1;        
    }
```

955. Delete Columns to Make Sorted II.md

### Problem summary
Given a list of words with same length, get min number of columns to remove to make the strings sorted

### Idea
column by column check if the previous strings are sorted.
0 to jth column is not sorted, mark the jth column to all '*'
if found some strings are equal, we need check later columns.
If it is sorted, we are done. No need to go further

### implementation
```cpp
    int minDeletionSize(vector<string>& A) {
        //previous col is sorted then is over
        int ans=0;
        int m=A.size(),n=A[0].size();
        for(int j=0;j<n;j++)
        {
            bool need_next=0;
            int i=1;
            for(;i<m;i++)
            {
                if(A[i].substr(0,j+1)<A[i-1].substr(0,j+1)) {ans++;break;}
                else if(A[i].substr(0,j)==A[i-1].substr(0,j)) need_next=1;
            }
            if(i==m && !need_next) return ans; //sorted
            if(i<m) for(int k=0;k<m;k++) A[k][j]='*';
        }
        return ans;
    }
```

956. Tallest Billboard.md

### Problem Summary
Given a list of numbers (positive), we need build two sums with same value. Get the maximum sum. Cannot reuse the numbers

### Idea
This is a DP problem and I tried to use knapsack with status (since it needs two target sum), but I did not make it.
We have observed that the target sum<=totalsum/2.
If dp is hard to approach, we can try recursion + memoization method.
One observation is very useful for the approach:
for every number, we have option: not choose, 0, choose to positive, 1, choose to negative -1. Our goal is to make the sum to be 0.

dp[i][j] represents whether the sum of first i numbers can be j - 5000. dp[0][5000] = true.
Then dp[i + 1][j] = dp[i][j - rods[i]] | dp[i][j + rods[i]] | dp[i][j].
max[i][j] represents the largest sum of all positive numbers when the sum of first i numbers is j - 5000.

Time complexity: O(N*sum)

Note: 
1. the target sum can be from -5000 to 5000. so we elevated the target sum index to 5000 so the index can be from 0 to 10001
2. we only maximize the positive sum;
3. the target sum is 0.

### implementation
```cpp
    int tallestBillboard(vector<int>& rods) {
        int n=rods.size();
        int tsum=accumulate(rods.begin(),rods.end(),0)/2;
        vector<vector<int>> dp(n,vector<int>(10001));//dp[i,j]: -tsum to tsum
        return helper(rods,0,5000,dp);
    }
    
    int helper(vector<int>& rods,int start,int tsum,vector<vector<int>>& dp)
    {
        if(start==rods.size()) return tsum==5000?0:INT_MIN/2;
        if(dp[start][tsum]) return dp[start][tsum];
        int ans=helper(rods,start+1,tsum,dp); //do not use current
        ans=max(ans,helper(rods,start+1,tsum-rods[start],dp)); //choose rods[i] as negative
        ans=max(ans,helper(rods,start+1,tsum+rods[start],dp)+rods[start]); //choose rods[i] as positive
        //note the answer is the sum of positives
        return dp[start][tsum]=ans;
    }
```

Convert to dp is a knapsack problem with not using, using to positive, using to negative, and maximize the positve sum.

