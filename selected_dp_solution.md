dp solutions

### partition problem

472. Concatenated Words
```
    vector<string> findAllConcatenatedWordsInADict(vector<string>& words) {
        vector<string> ans;
        sort(begin(words),end(words),[](string& a,string& b){return a.size()<b.size();});
        unordered_set<string> dict;
        dict.insert(words[0]);
        for(int i=1;i<words.size();i++){
            if(isValid(words[i],dict))
                ans.push_back(words[i]);
            dict.insert(words[i]);
        }
        return ans;
    }
    
    bool isValid(string& w,unordered_set<string>& ms){
        //using dp to check if w can be decompsed into dictionary words
        int n=w.size();
        vector<bool> dp(n+1);
        dp[0]=1;
        for(int i=1;i<=n;i++){
            for(int j=i;j>0;j--){
                if(dp[j-1] && ms.count(w.substr(j-1,i-j+1))) {dp[i]=1;break;}
            }
        }
        return dp[n];
    }
```	
dp[j-1] and j start from i. 


140. Word Break II
```
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        vector<string> ans;
        unordered_set<string> dict(begin(wordDict),end(wordDict));
        int n=s.size();
        vector<bool> dp(n+1);
        vector<vector<int>> next(n);
        dp[0]=1;
        for(int i=1;i<=n;i++){
            for(int j=i;j>0;j--){
                if(dp[j-1] && dict.count(s.substr(j-1,i-j+1))){
                    dp[i]=1;
                    next[j-1].push_back(i);
                }
            }
        }
        if(dp[n]==0) return {};
        backtrack(s,next,0,"",ans);
        return ans;
    }
    void backtrack(string& s,vector<vector<int>>& next,int start,string t,vector<string>& ans){
        if(start==s.size()){
            ans.push_back(t);
            return;
        }
        for(int n: next[start])
            backtrack(s,next,n,(t.empty()?"":t+" ")+s.substr(start,n-start),ans);
    }
```	
- cannot break since we need get all path
- use next instead of pre is more convenient
- j-1 points to next new word start position

-132	Palindrome Partitioning II    		30.7%	Hard
```
    int minCut(string s) {
        int n=s.size();
        vector<int> dp(n,-1);
        dfs(s,0,dp);
        return dp[0];
    }
    int dfs(string& s,int start,vector<int>& dp){
        if(ispal(s,start,s.size()-1)) return dp[start]=0;
        if(dp[start]>=0) return dp[start];
        int ans=INT_MAX;
        for(int i=start;i<s.size();i++){
            if(ispal(s,start,i))
                ans=min(ans,1+dfs(s,i+1,dp));
        }
        return dp[start]=ans;
    }
    bool ispal(string& s,int i,int j){
        if(i==j) return 1;
        while(i<j){
            if(s[i]!=s[j]) return 0;
            i++,j--;
        }
        return 1;
    }
```

or grow odd even palindrome at each element.
```
    int minCut(string s) {
        int n = s.size();
        vector<int> cut(n+1, 0);  // number of cuts for the first k characters
        for (int i = 0; i <= n; i++) cut[i] = i-1;
        for (int i = 0; i < n; i++) {
            for (int j = 0; i-j >= 0 && i+j < n && s[i-j]==s[i+j] ; j++) // odd length palindrome
                cut[i+j+1] = min(cut[i+j+1],1+cut[i-j]);

            for (int j = 1; i-j+1 >= 0 && i+j < n && s[i-j+1] == s[i+j]; j++) // even length palindrome
                cut[i+j+1] = min(cut[i+j+1],1+cut[i-j+1]);
        }
        return cut[n];
    }
```

-1278	Palindrome Partitioning III    		60.2%	Hard	*****

```
    int palindromePartition(string s, int K) {
        int n=s.size();
        vector<vector<int>> dp(n+1,vector<int>(n+1,INT_MAX));
        vector<vector<int>> cost(n,vector<int>(n));

        for(int i=0;i<=n;i++) dp[i][i]=0;
        for(int i=0;i<n;i++){
            for(int j=i+1;j<n;j++)
                cost[i][j]=calc_cost(s,i,j);
        }
        for(int i=1;i<=n;i++) dp[i][1]=cost[0][i-1];
        
        for(int k=2;k<=K;k++){
            for(int i=k+1;i<=n;i++){ //length > k
                for(int j=i;j>=k;j--){ //previous >=k-1 chars
                    dp[i][k]=min(dp[i][k],cost[j-1][i-1]+dp[j-1][k-1]);
                }
            }
        }
        return dp[n][K];
    }
    int calc_cost(string& s,int i,int j){
        int ans=0;
        while(i<j) ans+=s[i++]!=s[j--];
        return ans;
    }
```	

- base condition: the col 1 and the diagonal
- cost[j-1][i-1] j<i, I spent a long time to find the mistake.
example:
"tcymekt"
4

0 - - - - - - - 
- 0 - - - - - - 
- 1 0 - - - - - 
- 1 1 0 - - - - 
- 2 1 1 0 - - - 
- 2 2 1 1 0 - - 
- 3 2 2 1 - 0 - 
- 2 3 2 2 - - 0 



