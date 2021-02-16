## backtracking

backtracking is recursive based. So think it take a step and then solve the subproblem.
what defines the subproblem and what is the expected output and what are the exit condition.

backtracking is used to get the required combinations or path. 
similar to dp: take the first step and solve the subsequent subproblem. Many times it is also the base of top down dp approach.

backtracking is also similar to dfs, except we need collect information so we need do the backtracking by ourselves similar to dfs (done by recursive stack push and pop)

### general backtracking
dfs to get all required combinations or paths.
- it generally needs an array to store the results
- it generally includes add/removal elements from the array
- use reference passing of the array to save time.

- 22	Generate Parentheses    		64.2%	Medium	
generate all possible valid parentheses string with length n.
number of left parenthesis shall be always >= number of right parenthesis

```
    vector<string> generateParenthesis(int n) {
        vector<string> ans;
        backtrack(n,n,"",ans);
        return ans;
    }
    void backtrack(int m,int n,string t,vector<string>& ans){
        if(!m && !n){
            ans.push_back(t);
            return;
        }
        if(m) backtrack(m-1,n,t+'(',ans);
        if(n>m) backtrack(m,n-1,t+')',ans);
    }
```	
no push/pop since we use value passing and t is not changed in the code.

similar problems:
- 784	Letter Case Permutation    		65.7%	Medium	
- 17	Letter Combinations of a Phone Number    		48.0%	Medium	
- 967	Numbers With Same Consecutive Differences    		44.1%	Medium	
- 1215	Stepping Numbers    		42.7%	Medium	
- 1291	Sequential Digits    		57.4%	Medium	
- 320	Generalized Abbreviation    		52.9%	Medium	
- 797	All Paths From Source to Target    		78.2%	Medium	

It shall be sufficient to develope intuition on simple backtracking problem. 
Beginners may be often confused by the recursion or the loop. In this case, you need think you are solving subproblem  using recursion.

recursion is hard to debug and so focus more on the higher level concepts. Drawing a recursion tree may help to avoid many mistakes.

- 1601	Maximum Number of Achievable Transfer Requests    		47.2%	Hard	
to reach net 0 balance. if there is a outgoing edge, there must be a incoming edge.
this is hard and easily confused by the information.
but if thinking like this way: each edge has two options: chosen or not chosen. Then it becomes exhausting all possibilities.
```cpp
    int maximumRequests(int n, vector<vector<int>>& requests) {
        //each edge can be chosen or not chosen
        vector<int> net(n); //net balance for each node
        int ans=0;
        backtrack(requests,0,0,ans,net);
        return ans;
    }
    
    void backtrack(vector<vector<int>>& req,int start,int sum,int& mx,vector<int>& net){
        if(start>=req.size()){
            if(net==vector<int>(net.size())) mx=max(mx,sum);
            return;
        }
        int from=req[start][0],to=req[start][1];
        net[from]--,net[to]++;//choose the edge
        backtrack(req,start+1,sum+1,mx,net);
        net[from]++,net[to]--;//resotre and not choose the edge
        backtrack(req,start+1,sum,mx,net);
    }
```
similar problem:
- 465	Optimal Account Balancing    		47.6%	Hard	
edge is give [x,y,z] z is the money x gives to y.
find the min number of transactions to clear all the debt.
to get the min number of transactions, we need apply some greedy approach:
- each node may have incoming/outgoing edges, we can calculate the net debt.
- greedy approach: use nodes with different sign to cancel at least one node.
Note: using min and max to cancel one is not the optimal one, since one may be used to cancel two the same time. so we need try all combinations and get the min. 
our idea: for each node, we try to combine with any opposite sign on its right. (or use start to cancel other nodes)
```
    int minTransfers(vector<vector<int>>& transactions) {
        unordered_map<int,int> mp;
        for(auto t: transactions) mp[t[0]]-=t[2],mp[t[1]]+=t[2];
        vector<int> debt;
        for(auto p: mp) if(p.second) debt.push_back(p.second);
        //sort(begin(debt),end(debt)); //not needed
        return backtrack(debt,0);
    }
    
    int backtrack(vector<int>& debt,int start){
        while(start<debt.size() && debt[start]==0) start++;
        if(start>=debt.size()) return 0;
        int ans=INT_MAX/2;
        for(int i=start+1;i<debt.size();i++){
            if(debt[i] && debt[i]*debt[start]<0){
                debt[i]+=debt[start];
                ans=min(ans,1+backtrack(debt,start+1));
                debt[i]-=debt[start];
            }
        }
        return ans<INT_MAX/2?ans:0;
    }
```	
one possible optimization: debt[i]==debt[s] we do not want to check same elements.

- 1655	Distribute Repeating Integers    		40.0%	Hard	
each customer needs Q[i] same integers.
- sort decreasing order (if the largest customer is not able to satisfy then fail)
- after customer is served, change the array and proceed to next customer. (subproblem)
```
    bool canDistribute(vector<int>& nums, vector<int>& quantity) {
        //you can have multiple choices to fulfill the orders
        unordered_map<int,int> mp; //up to 50 values
        for(int i: nums) mp[i]++;
        vector<int> v;
        for(auto t: mp) v.push_back(t.second);
        sort(rbegin(quantity),rend(quantity));
        int m=quantity.size();
        return backtrack(v,quantity,0);
    }
    
    int backtrack(vector<int>& v,vector<int>& q,int start){
        if(start>=q.size()) return 1;
        for(int i=0;i<v.size();i++){
            if(v[i]>=q[start]){
                v[i]-=q[start];
                if(backtrack(v,q,start+1)) return 1;
                v[i]+=q[start];
            }
        }
        return 0;
    }
```

756. Pyramid Transition Matrix
using given triangle blocks to build the pyramid. 
a 2d backtracking: when done with a row, we start a new row (subproblem).

```
    unordered_map<string,vector<char>> mp;
    bool pyramidTransition(string bottom, vector<string>& allowed) {
        for(auto s: allowed) mp[s.substr(0,2)].push_back(s[2]);
        return backtrack(bottom,0,"");
    }
    bool backtrack(string& bott,int start,string next){
        if(bott.size()==1) return 1;
        if(start==bott.size()-1) return backtrack(next,0,"");
        
        for(char c: mp[bott.substr(start,2)]){
            if(backtrack(bott,start+1,next+c)) return 1;
        }
        
        return 0;
    }
```	

more practices:
- 440	K-th Smallest in Lexicographical Order    		29.3%	Hard	
- 386	Lexicographical Numbers    		52.8%	Medium	
- 1681. Minimum Incompatibility
-1239	Maximum Length of a Concatenated String with Unique Characters    		48.7%	Medium	****

### permutations

permutation: loop to use each element as the first position and then a subproblem
- with duplicates: using sort and skip, or using hashmap
- swap.
- permutation will not just go right, it shall loop the same whole range.

- 47	Permutations II    		48.4%	Medium	
generate a hashmap and loop over the whole map to get all permutations.
```
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        unordered_map<int,int> mp;
        for(int i: nums) mp[i]++;
        vector<vector<int>> ans;
        int n=nums.size();
        backtrack(mp,n,{},ans);
        return ans;
    }
    void backtrack(unordered_map<int,int>& mp,int n,vector<int> v,vector<vector<int>>& ans){
        if(v.size()==n) {
            ans.push_back(v);
            return;
        }
        for(auto& t: mp){
            if(t.second){
                t.second--;
                v.push_back(t.first);
                backtrack(mp,n,v,ans);
                v.pop_back();
                t.second++;
            }
        }
    }
```
similar problems:
- 46	Permutations    		65.2%	Medium	
- 267	Palindrome Permutation II    		36.9%	Medium	
- 266	Palindrome Permutation    		62.3%	Easy	
- 1079	Letter Tile Possibilities    		75.6%	Medium	
- 996	Number of Squareful Arrays    		47.5%	Hard	
- 332	Reconstruct Itinerary    		37.3%	Medium	
- 1467	Probability of a Two Boxes Having The Same Number of Distinct Balls    		61.3%	Hard	
	
- 247	Strobogrammatic Number II    		48.1%	Medium	
generate all strobogrammatic number with length n.
a strobogrammatic shall be symmetric (rotate 180 degrees equal).
so use two pointer to fill different digits. or only fill half. (if odd length the middle one shall be 0,1,8)
```
    unordered_map<char,char> mp={{'0','0'},{'1','1'},{'6','9'},{'9','6'},{'8','8'}};
    vector<string> findStrobogrammatic(int n) {
        vector<string> ans;
        string t(n,'0');
        backtrack(n,0,n-1,t,ans);
        return ans;
    }
    void backtrack(int n,int l,int r,string& t,vector<string>& ans){
        if(l>r) {ans.push_back(t);return;}
        
        for(auto p: mp){
            if(l==0 && l<r && p.first=='0') continue; //leading 0 not allowed.
            if(l==r && p.first!=p.second) continue; //the middle one shall be 0,1,8
            t[l]=p.first,t[r]=p.second;
            backtrack(n,l+1,r-1,t,ans)    ;
        }
    }
```	
similar problem:
- 248	Strobogrammatic Number III    		39.9%	Hard	

### combinations
combination generally loops over each element and only goes to right.
so the range is keeping smaller.

- 90	Subsets II    		48.0%	Medium	
- 78	Subsets    		63.7%	Medium	
- 77	Combinations    		56.2%	Medium	
- 377	Combination Sum IV    		45.7%	Medium	
- 216	Combination Sum III    		59.4%	Medium	
- 40	Combination Sum II    		49.4%	Medium	
- 39	Combination Sum    		58.1%	Medium	
- 254	Factor Combinations    		47.0%	Medium	
- 416	Partition Equal Subset Sum    		44.3%	Medium	

- 473	Matchsticks to Square    		37.9%	Medium	
- 1286	Iterator for Combination    		70.7%	Medium	
- 638	Shopping Offers    		52.1%	Medium	
- 491	Increasing Subsequences    		46.9%	Medium	
- 1258	Synonymous Sentences    		67.2%	Medium	
- 1735. Count Ways to Make Array With Product

- 698	Partition to K Equal Sum Subsets    		45.3%	Medium	
```
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        //use bitmask to indiate our selection
        //larger one pick first since smaller has more combination choice
        sort(rbegin(nums),rend(nums));
        int tsum=accumulate(begin(nums),end(nums),0);
        if(tsum%k) return 0;
        int target=tsum/k;
        return backtrack(nums,0,0,0,k,target);
    }
    bool backtrack(vector<int>& nums,int start,int sum,int mask,int k,int target){
        int n=nums.size();
        if(k==0) return 1;
        if(target==sum) 
            return backtrack(nums,0,0,mask,k-1,target);
        if(start>=n || sum>target) return 0;
        for(int i=start;i<n;i++){
            if(mask&(1<<i)) continue;
            if(backtrack(nums,i+1,sum+nums[i],mask|(1<<i),k,target)) return 1;
        }
        return 0;
    }
```

### dp similar: try all prefix and solve subproblem

- 282	Expression Add Operators    		36.2%	Hard	
- 290	Word Pattern    		38.1%	Easy	
- 291	Word Pattern II    		43.8%	Hard	
- 816	Ambiguous Coordinates    		47.6%	Medium	
- 842	Split Array into Fibonacci Sequence    		36.4%	Medium	
- 131. Palindrome Partitioning
```
    vector<vector<string>> partition(string s) {
        //backtracking
        vector<vector<string>> ans;
        int n=s.size();
        backtrack(s,0,{},ans);
        return ans;
    }
    void backtrack(string& s,int start,vector<string> t,vector<vector<string>>& ans){
        if(ispal(s,start,s.size()-1)){
            t.push_back(s.substr(start));
            ans.push_back(t);
            t.pop_back();
        }
        for(int i=start;i<s.size();i++){
            if(ispal(s,start,i)){
                t.push_back(s.substr(start,i-start+1));
                backtrack(s,i+1,t,ans);
                t.pop_back();
            }
        }
    }
    bool ispal(string& s,int i,int j){ //inclusive
        if(i>j) return 0;
        while(i<j){
            if(s[i]!=s[j]) return 0;
            i++,j--;
        }
        return 1;
    }
```
	
- 1593	Split a String Into the Max Number of Unique Substrings    		46.7%	Medium	
```
   int maxUniqueSplit(string s) {
        unordered_set<string> ms;
        return helper(s,ms);
    }
    int helper(string& s,unordered_set<string>& ms){
        int ans=0;
        //if(s.empty()) return 0;
        for(int i=0;i<s.size();i++){
            string cand=s.substr(0,i+1);
            if(ms.count(cand)==0){
                ms.insert(cand);
                //cout<<cand<<" "<<i+1<<endl;
                string ss=s.substr(i+1);
                ans=max(ans,1+helper(ss,ms));
                ms.erase(cand);
            }
        }
        return ans;
    }
```	
### board game
- 52	N-Queens II    		59.1%	Hard	
- 51	N-Queens    		48.2%	Hard	
- 1307	Verbal Arithmetic Puzzle    		37.6%	Hard	
- 425	Word Squares    		49.4%	Hard	
- 488	Zuma Game    		39.2%	Hard	
- 36 valid sudoko 
- 37 soduko solver 

