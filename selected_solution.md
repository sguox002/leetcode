
backtracking:
46. permutation
with all unique.
similar but simpler as 47.

47. permutation II
- using hashmap (with duplicates you always can try this)
- more robust, not easy to make mistakes.
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

skip duplicates
```
    vector<vector<int> > permuteUnique(vector<int> &num) {
        sort(num.begin(), num.end());
        vector<vector<int> >res;
        backtrack(num, 0, res);
        return res;
    }
    void backtrack(vector<int> nums, int start, vector<vector<int> > &res) {
        if (start == nums.size()-1) {
            res.push_back(nums);
            return;
        }
        for (int i = start; i < nums.size(); i++) {
            if (i != start && nums[start] == nums[i]) continue;
            swap(nums[i], nums[start]);
            backtrack(nums, start+1, res);
        }
    }    
```
- it uses value passing, so no need to swap back.	
- it uses the same input vector as output (once swapped, the right part is not sorted, and it is OK to include duplicates on the right.)
- when done one, the input array is retored back and we move to next.
- if we change to reference passing, and restore the array, we get much more sets.

using visited array and each time try the whole array
```
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(begin(nums),end(nums));
        vector<bool> v(nums.size());
        vector<vector<int>> ans;
        backtrack(nums,{},v,ans);
        return ans;
    }
    void backtrack(vector<int>& nums,vector<int> t,vector<bool>& v,vector<vector<int>>& ans){
        if(t.size()==nums.size()){
            ans.push_back(t);
            return;
        }
        for(int i=0;i<nums.size();i++){
            if(v[i]) continue;
            if(i && nums[i]==nums[i-1] && !v[i-1]) continue;
            v[i]=1;
            t.push_back(nums[i]);
            backtrack(nums,t,v,ans);
            v[i]=0;
            t.pop_back();
        }
    }
```
to skip duplicates:-
when a number has the same value with its previous, we can use this number only if his previous is used.	
(which is tricky).

62. subsets

```
	vector<vector<int>> subsets(vector<int>& nums) {	
		vector<vector<int>> ans;
		backtrack(nums,0,{},ans);
		return ans;
	}
	void backtrack(vector<int>& nums,int ind,vector<int> vt,vector<vector<int>>& ans)
	{
		ans.push_back(vt);
		for(int i=ind;i<nums.size();i++)
		{
			vt.push_back(nums[i]);
			backtrack(nums,i+1,vt,ans);
			vt.pop_back();
		}
	}
```	
for example [1,2,3]
it starts with []
then add 1, [1]
then add 2, [1,2]
then add 3, [1,2,3]
pop 3
pop 2
pop 1
add 2 [2]
add 3 [2,3]
pop 3
pop 2
add 3 [3]

248	Strobogrammatic Number III 
```
    unordered_map<char,char> mp={{'0','0'},{'1','1'},{'6','9'},{'9','6'},{'8','8'}};
    int strobogrammaticInRange(string low, string high) {
        int m=low.size(),n=high.size();
        int ans=0;
        for(int len=m;len<=n;len++){
            string t=string(len,'0');
            ans+=backtrack(low,high,t,len,0,len-1);
        }
        return ans;
    }
    
    int backtrack(string& low,string& high,string& t,int len,int l,int r){
        if(l>r){
            if(t.size()>low.size() && t.size()<high.size()) return 1;
            if(t.size()==low.size()) {
                if(t.size()==high.size()) return t>=low && t<=high;
                return t>=low;
            }
            return t<=high;
        }
        int ans=0;
        for(auto p: mp){
            if(l==0 && l!=r && p.first=='0') continue;
            if(l==r && p.first!=p.second) continue;
            t[l]=p.first,t[r]=p.second;
            ans+=backtrack(low,high,t,len,l+1,r-1);
            
        }
        return ans;
    }
```


254. factor combinations
```
    vector<vector<int>> getFactors(int n) {
        if(n==1) return {};
        vector<vector<int>> ans;
        backtrack(n,2,{},ans);
        return ans;
    }
    void backtrack(int n,int start,vector<int> t,vector<vector<int>>& ans){
        if(n<start){ans.push_back(t);return;}
        for(int i=start;i*i<=n;i++){
            if(n%i==0){
                t.push_back(i),t.push_back(n/i);
                ans.push_back(t);
                t.pop_back();
                backtrack(n/i,i,t,ans);
                t.pop_back();
            }
        }
    }
```

267	Palindrome Permutation II  
```
    vector<string> generatePalindromes(string s) {
        unordered_map<char,int> mp;
        for(char c: s) mp[c]++;
        int nodd=0,len=0;
        char odd=0;
        for(auto& p: mp) {
            nodd+=p.second%2;
            if(p.second%2) odd=p.first;
            p.second/=2;
            len+=p.second;
        }
        if(nodd>1) return {};
        vector<string> ans;
        backtrack(mp,"",len,ans);
        for(auto& s: ans){
            string rs={rbegin(s),rend(s)};
            if(odd) s+=odd;
            s+=rs;
        }
        return ans;
    }
    
    void backtrack(unordered_map<char,int>& mp,string t,int len,vector<string>& ans){
        if(len==t.size()){
            ans.push_back(t);
            return;
        }
        for(auto& p: mp){
            if(p.second==0) continue;
            p.second--;
            t+=p.first;
            backtrack(mp,t,len,ans);
            t.pop_back();
            p.second++;
        }
    }
```


282. Expression Add Operators

```
    vector<string> addOperators(string num, int target) {
        vector<string> ans;
        if (num.empty()) return {};
        for (int i=1; i<=num.size(); i++) 
        {
            string s = num.substr(0, i);
            long cur = stol(s);
            if (to_string(cur).size() != s.size()) continue;//leading zeros
            backtrack(ans, num, target, s, i, cur, cur, '#');         // no operator defined.
        }

        return ans;
    }
    // cur: {string} expression generated so far.
    // pos: {int}    current visiting position of num.
    // cv:  {long}   cumulative value so far.
    // pv:  {long}   previous operand value.
    // op:  {char}   previous operator used.
    void backtrack(vector<string>& res, string& num, int target, string& cur, int pos, long cv, long pv, char op) {
        if (pos == num.size() && cv == target) {res.push_back(cur);return;} 
        for (int i=pos+1; i<=num.size(); i++) 
        {
            string t = num.substr(pos, i-pos);
            long now = stol(t);
            if (to_string(now).size() != t.size()) continue;
            string s;
            s=cur+"+"+t;backtrack(res, num, target, s, i, cv+now, now, '+');
            s=cur+"-"+t;backtrack(res, num, target, s, i, cv-now, now, '-');
            s=cur+"*"+t;backtrack(res, num, target, s, i, (op=='-')?cv+pv-pv*now:((op=='+')?cv-pv+pv*now : pv*now), pv*now, op);
        }
    }   
```
the complexity:
we divide the string into 1 to n parts, each part has 3 options.
so 3^n

use reference passing for the string is critical to pass the OJ.

39. Combination Sum

```
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> ans;
        backtrack(candidates,0,target,{},ans);
        return ans;
    }
    void backtrack(vector<int>& nums,int start,int target,vector<int> t,vector<vector<int>>& ans){
        if(0==target){
            ans.push_back(t);
            return;
        }
        if(0>target) return;
        for(int i=start;i<nums.size();i++){
            t.push_back(nums[i]);
            backtrack(nums,i,target-nums[i],t,ans);
            t.pop_back();
        }
    }
```
loop each element and process to the right.	
	
377. Combination Sum IV
the direct dp using array cannot pass any more
```
    int combinationSum4(vector<int>& nums, int target) {
        //knapsack with repetition
        vector<int> dp(target+1); //number of combinations
        dp[0]=1; //empty
        //sort(nums.begin(),nums.end());
        for(int t=1;t<=target;t++)
        {
            for(int i=0;i<nums.size();i++)
            {
                if(t>=nums[i]) dp[t]+=dp[t-nums[i]];
            }
        }
        return dp[target];
    }
```
for [3,33,333] target=10000
- not all target needs to be calculated
- size would be too large

using top down + recursive, which is more similar to backtracking.

377. combination IV.
```
    int combinationSum4(vector<int>& nums, int target) {
        int n=nums.size();
        //sort(begin(nums),end(nums));
        unordered_map<int,int> dp;
        
        return backtrack(nums,0,target,dp);
    }
    int backtrack(vector<int>& nums,int start,int target,unordered_map<int,int>& dp){
        if(target==0) return 1;
        if(start>=nums.size() || target<0) return 0;
        if(dp.count(target)) return dp[target];
        int ans=0;
        for(int i=0;i<nums.size();i++)
            ans+=backtrack(nums,i,target-nums[i],dp);
        return dp[target]=ans;
    }
```
since this is actually the permutation so we need start from 0.
	
320. generalized abbreviation

use it or not (if abbrev, previous shall not be digits.

```
    vector<string> generateAbbreviations(string word) {
        vector<string> ans;
        backtrack(word,0,"",ans,0);
        return ans;
    }
    void backtrack(string& w,int ind,string t,vector<string>& ans,bool prev_num){
        if(ind==w.size()){
            ans.push_back(t);
            return;
        }
        
        backtrack(w,ind+1,t+w[ind],ans,0); //use current char
        if(!prev_num){
            for(int i=ind;i<w.size();i++){
                int sz=i-ind+1;
                backtrack(w,i+1,t+to_string(sz),ans,1);
            }
        }
    }
```	
291. word pattern II

```
    bool wordPatternMatch(string pattern, string s) {
        unordered_map<string,char> s2c;
        unordered_map<char,string> c2s;
        return backtrack(s,pattern,0,0,c2s,s2c);
    }
    
    bool backtrack(string& s,string& pat,int i,int j,unordered_map<char,string>& c2s,unordered_map<string,char>& s2c){
        if(i==s.size() && j==pat.size()) return 1;
        if(i==s.size() || j==pat.size()) return 0;
        
        //using all possible prefix starting from i
        char c=pat[j];
        for(int k=i;k<s.size();k++){
            string t=s.substr(i,k-i+1);
            //check if the mapping
            if(s2c.count(t) && s2c[t]!=c) continue; 
            if(c2s.count(c) && c2s[c]!=t) continue;
            c2s[c]=t;
            s2c[t]=c;
            if(backtrack(s,pat,k+1,j+1,c2s,s2c)) return 1;
            c2s.erase(c);
            s2c.erase(t);
        }
        return 0;
    }
```

this will fail the case:
"sucks"
"teezmmmmteez"
why? the reason:
if the string is already mapped, we shall not pop them back
add a boolean to add the mapping.

756. Pyramid Transition Matrix
2d backtracking:

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

	
	
	
	
	
