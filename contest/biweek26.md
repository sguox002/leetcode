## biweek 26
### 1446. Consecutive Characters
<em>
Given a string s, the power of the string is the maximum length of a non-empty substring that contains only one unique character.

Return the power of the string.
</em>

Approach: straightforward
compare with previous
```cpp
    int maxPower(string s) {
        int ans=0,cnt=0;
        char prev=s[0];
        for(char c: s){
            if(c==prev) cnt++;
            else{
                ans=max(ans,cnt);
                cnt=1;
            }
            prev=c;
            ans=max(ans,cnt);
        }
        return ans;
    }
```


### 1447. Simplified Fractions
<em>
Given an integer n, return a list of all simplified fractions between 0 and 1 (exclusive) such that the denominator is less-than-or-equal-to n. The fractions can be in any order.
</em>

brutal force:
```cpp
    vector<string> simplifiedFractions(int n) {
        //must be gcd=1
        vector<string> ans;
        for(int i=2;i<=n;i++){
            for(int j=1;j<i;j++){
                if(__gcd(j,i)==1) ans.push_back(to_string(j)+"/"+to_string(i));
            }
        }
        return ans;
    }
```

### 1448. Count Good Nodes in Binary Tree
<em>
Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.

Return the number of good nodes in the binary tree.	
</em>

Approach: preorder dfs, prev is previous max.
```cpp
    int goodNodes(TreeNode* root) {
        return helper(root,INT_MIN);
    }
    int helper(TreeNode* root,int prev){
        if(!root) return 0;
        int ans=0;
        if(root->val>=prev) ans++;
        prev=max(prev,root->val);
        ans+=helper(root->left,prev)+helper(root->right,prev);
        return ans;
        
    }
```

### 1449. Form Largest Integer With Digits That Add up to Target
<em>
Given an array of integers cost and an integer target. Return the maximum integer you can paint under the following rules:

The cost of painting a digit (i+1) is given by cost[i] (0 indexed).
The total cost used must be equal to target.
Integer does not have digits 0.
Since the answer may be too large, return it as string.

If there is no way to paint any integer given the condition, return "0".
</em>

A very good problem, and I like it.

first intuition: knapsack with repetition but with more complexity (longest and largest)
2nd intution: backtracking with memoization

- backtracking
```cpp
    string largestNumber(vector<int>& cost, int target) {
        //we shall have more digits
        //if number of digits is fixed, then we need have the largest digit first.
        //first get the longest subsequence adds to target
        //then find the lex largest longest subsequence, each one can be used multiple times
        //shall use dp. backtracking to get the 
        string ans;
        int n=cost.size();
        vector<vector<string>> dp(n,vector<string>(target+1));
        //memset(dp,-1,5000*9*sizeof(int));
        backtrack(cost,0,target,"",ans,dp);
        
        return ans.empty()?"0":ans;
    }
    string backtrack(vector<int>& cost,int ind,int val,string t,string& ans,vector<vector<string>>& dp){
        if(val==0) {
            reverse(t.begin(),t.end());
            if(t.size()>ans.size()) ans=t;
            if(t.size()==ans.size()) ans=max(ans,t);
            return t;
        }

        if(ind>=cost.size() || val<0) return "";
        //if(dp[ind][val].size()) return dp[ind][val]; //disable memo.
        //we can use 0 to val/cost[ind] times
        string ts=backtrack(cost,ind+1,val,t,ans,dp);
        for(int i=1;i<=val/cost[ind];i++){
            t+='1'+ind;
            ts=max(ts,backtrack(cost,ind+1,val-i*cost[ind],t,ans,dp));
        }
        return dp[ind][val]=ts;
    }
	```
Note above memoization will not work, but backtrack gives all correct answer.
It is a good start though.
- note val==0 must be checked before the 2nd check
- in the loop we cannot update val using val-=cost[ind] since we use it also in the loop condition

let's get the memo correct. What is the problem with above code?
[5,4,4,5,5,5,5,5,5]
29
backtracking: 9333333
memo: 4333333

- if we did not get a solution for ind and val, we set it empty, this disables the memoization. 
we can set it as "0"
- two if are incorrect and we shall use if.. else for val==0 case.
- apparently it is due to this statement: if(dp[ind][val].size()) return dp[ind][val];


After struggling for quite a time, I found the memoization is incorrect, since we define the dp incorrectly.
dp[ind][v] shall be the solution for the subproblem, but actually we return the whole answer.
In this case, we can eliminate the ind dimension.
If we want to use the 2d dp, we need strictly keep the subproblem definition, where the dp shall save the subproblem solution only.

The dp actually does not depend on the index, and is a 1d dp problem.
we choose one element and then leave a smaller target with the same array. That's the knapsack with repetition.

```cpp
string dp[5001] = {};
string largestNumber(vector<int>& cost, int t) {
    if (t <= 0)
        return t == 0 ? "" : "0";
    if (dp[t].empty()) {
        dp[t] = "0";
        for (int n = 1; n <= 9; ++n) {
            auto res = largestNumber(cost, t - cost[n - 1]);
            if (res != "0" && res.size() + 1 >= dp[t].size())
                dp[t] = to_string(n) + res;
        }
    }
    return dp[t];
}
```
