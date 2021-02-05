Selected topics and problems

recursion based algorithm:

## backtracking

backtracking is used to get the required combinations or path. 
similar to dp: take the first step and solve the subsequent subproblem.

46. permutation
input is unique.
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

1307. Verbal Arithmetic Puzzle

char digit map
digit char map 

the two map shall set up unique and consistent mapping.
no leading zero.
natural: column by column from right to left (or reverse from left to right)
using col index, and row index for 2d back. if current col fail then we need backtrack to previous col.

```
    int d2c[10],c2d[26],notallowed[26]={0};
    bool isSolvable(vector<string>& words, string result) {
        memset(d2c,-1,10*sizeof(int));
        memset(c2d,-1,26*sizeof(int));
        if(result.size()>1) notallowed[result[0]-'A']=1;
        int mxl=0;
        for(auto w: words) {
            if(w.size()>1) notallowed[w[0]-'A']=1;
            mxl=max(mxl,(int)w.size());
        }
        if(mxl>result.size()) return 0;
        reverse(begin(result),end(result));
        for(auto& w: words) reverse(begin(w),end(w));
        return backtrack(words,result,0,0,0);
    }
    
    bool backtrack(vector<string>& left,string& right,int row,int col,int cf){
        //cout<<row<<" "<<col<<": ";print(c2d);
        if(col==right.size()) return cf==0;
        if(row==left.size()){ //finished a col
            int d=cf%10;
            cf/=10;
            int ind=right[col]-'A';
            if(c2d[ind]>=0){ //mapped
                if(c2d[ind]==d) return backtrack(left,right,0,col+1,cf);
                return 0;//fail
            }
            else{ //not mapped yet
                if(d2c[d]>=0) return 0;
                if(!d && notallowed[ind]) return 0;
                c2d[ind]=d;
                d2c[d]=ind;
                //used[d]=1;
                if(backtrack(left,right,0,col+1,cf)) return 1;
                c2d[ind]=-1;
                d2c[d]=-1;
                //used[d]=0;
                return 0;//fail this is critical
            }
        }
        //process col
        if(col>=left[row].size()) return backtrack(left,right,row+1,col,cf);
        int ind=left[row][col]-'A';
        if(c2d[ind]>=0) return backtrack(left,right,row+1,col,cf+c2d[ind]);
        for(int i=0;i<=9;i++){
            //if(used[i]) continue;
            if(d2c[i]>=0) continue;
            if(!i && notallowed[ind]) continue;
            c2d[ind]=i;
            d2c[i]=ind;
            //used[i]=1;
            if(backtrack(left,right,row+1,col,cf+i)) return 1;
            //used[i]=0;
            d2c[i]=-1;
            c2d[ind]=-1;
        }
        return 0;
    }
    
    void print(int a[]){
        for(int i=0;i<26;i++){
            if(a[i]>=0) cout<<"["<<char('A'+i)<<":"<<a[i]<<"],";
        }
        cout<<endl;
    }
```

some critical:
- missing return 0 for mapping result char failure.
- edge case when result is shorter than longest left.

425. Word Squares
kth row and kth column read the same.
if we choose the string as the first row:
then we need pick string starting with 2nd char in string.
this makes it a trie combined approach.

```
    struct TrieNode{
        TrieNode* child[26];
        vector<int> wl;//store list of word index
        bool isleaf;
        TrieNode(){isleaf=0;memset(child,0,26*sizeof(TrieNode*));}
    };
    void addWord(TrieNode* root,string w,int ind){
        for(char c: w){
            if(root->child[c-'a']==0) 
                root->child[c-'a']=new TrieNode;
            root=root->child[c-'a'];
            root->wl.push_back(ind);
        }
        root->isleaf=1;
    }
    vector<int> startWith(TrieNode* root, string pre){
        for(char c: pre){
            if(root->child[c-'a']==0) return {};
            root=root->child[c-'a'];
        }
        return root->wl;
    }
    vector<vector<string>> wordSquares(vector<string>& words) {
        TrieNode* root=new TrieNode;
        for(int i=0;i<words.size();i++) addWord(root,words[i],i);
        vector<vector<string>> ans;
        for(int i=0;i<words.size();i++){ //choose any word to start
            backtrack(root,words,{words[i]},ans);
        }
        return ans;
    }
    void backtrack(TrieNode* root,vector<string>& words,vector<string> vt,vector<vector<string>>& ans){
        int n=words[0].size();
        if(vt.size()==n){
            ans.push_back(vt);
            return;
        }
        int sz=vt.size();
        string prefix;
        for(int j=0;j<sz;j++) prefix+=vt[j][sz];
        auto t=startWith(root,prefix);
        for(auto ind: t){
            vt.push_back(words[ind]);
            backtrack(root,words,vt,ans);
            vt.pop_back();
        }
    }
```
	
### dfs - recursion 
if we include the tree traversals, the dfs covers quite a few problems.
some dfs connection problems can be solved using union-find (disjoint set) and is not recursive.

regular dfs search

79. Word Search

```
    bool exist(vector<vector<char>>& board, string word) {
        for(int i=0;i<board.size();i++){
            for(int j=0;j<board[0].size();j++){
                if(dfs(board,i,j,0,word)) return 1;
            }
        }
        return 0;
    }
    
    bool dfs(vector<vector<char>>& b,int i,int j,int start,string& word){
        if(start>=word.size()) return 1;
        if(i<0||j<0||i>=b.size()||j>=b[0].size()||b[i][j]!=word[start]) return 0;
        char c=b[i][j];
        b[i][j]='.';
        bool ans=dfs(b,i+1,j,start+1,word)||
            dfs(b,i-1,j,start+1,word)||
            dfs(b,i,j+1,start+1,word)||
            dfs(b,i,j-1,start+1,word);
        b[i][j]=c;
        return ans;
    }
```

212. word search II
combined with trie search.

```
    struct TrieNode{
        TrieNode* child[26];
        bool leaf;
        TrieNode(){leaf=0;memset(child,0,26*sizeof(TrieNode*));}
    };
    void addWord(string& w,TrieNode* root){
        for(char c: w){
            if(root->child[c-'a']==0) 
                root->child[c-'a']=new TrieNode;
            root=root->child[c-'a'];
        }
        root->leaf=1;
    }
    
    bool startWith(string& pre,TrieNode* root){
        for(char c: pre){
            if(root->child[c-'a']==0) return 0;
            root=root->child[c-'a'];
        }
        return 1;
    }
    
    bool find(string& w,TrieNode* root){
        for(char c: w){
            if(root->child[c-'a']==0) return 0;
            root=root->child[c-'a'];
        }
        return root->leaf;
    }
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        TrieNode* root=new TrieNode;
        for(auto w: words) addWord(w,root);
        vector<string> ans;
        for(int i=0;i<board.size();i++){
            for(int j=0;j<board[0].size();j++){
                dfs(board,i,j,root,"",ans); //replace the index with the root
            }
        }
        unordered_set<string> ms(begin(ans),end(ans)); //eliminate duplicates
        return {begin(ms),end(ms)};
    }
    
    void dfs(vector<vector<char>>& b,int i,int j,TrieNode* parent,string t,vector<string>& ans){
        if(i<0||j<0||i>=b.size()||j>=b[0].size()||b[i][j]=='.') return;
        char oc=b[i][j];
        
        if(!parent->child[oc-'a']) return; //prune if we cannot find this branch.
        
        b[i][j]='.';
        parent=parent->child[oc-'a'];
        if(parent->leaf) ans.push_back(t+oc);
        if(parent){ //i,j becomes the parent
            dfs(b,i+1,j,parent,t+oc,ans);
            dfs(b,i-1,j,parent,t+oc,ans);
            dfs(b,i,j+1,parent,t+oc,ans);
            dfs(b,i,j-1,parent,t+oc,ans);
        }
        b[i][j]=oc;
    }
```
this includes duplicates, since may exist multiple same instance.
can use hashset to reduce duplicates

note it is tricky when to judge that we found an answer.

	
### divide and conquer


95 unique binary search tree II

```
    vector<TreeNode*> generateTrees(int n,int start=1) {
        vector<TreeNode*> ans;
        if(n<=0) return {0}; //null pointer
        for(int i=start;i<n+start;i++){ 
            auto left=generateTrees(i-start,start);//start  //start to i-1
            auto right=generateTrees(n+start-i-1,i+1);//i+1 to n+start
            for(auto l: left)
                for(auto r: right){
                    TreeNode* root=new TreeNode(i);
                    root->left=l,root->right=r;
                    ans.push_back(root);
                }
        }
        return ans;
    }
```

395. Longest Substring with At Least K Repeating Characters

It divides into multiple blocks (instead of two blocks)
```
    int longestSubstring(string s, int k) {
        //divide and conquer
        return maxsub(s,k,0,s.size());
    }
    
    int maxsub(string& s,int k,int l,int r){
        if(r-l<k) return 0; 
        vector<int> cnt(26);
        for(int i=l;i<r;i++) cnt[s[i]-'a']++;
        bool valid=1;
        for(int i: cnt) if(i && i<k) {valid=0;break;}
        if(valid) return r-l;
        int maxlen=0;
        for(int i=l;i<r;i++){
            if(cnt[s[i]-'a']<k){ //this char cannot be inside the substr
                maxlen=max({maxlen,maxsub(s,k,l,i)});
                l=i+1;
            }
        }
        maxlen=max(maxlen,maxsub(s,k,l,r)); //the last one is not covered.
        return maxlen;
    }
```	
	
315. Count of Smaller Numbers After Self
```
    vector<int> countSmaller(vector<int>& nums) {
        vector<vector<int>> vp;
        for(int i=0;i<nums.size();i++) vp.push_back({nums[i],i});
        vector<int> ans(nums.size());
        sort_count(vp,0,nums.size(),ans);
        return ans;
    }
    void sort_count(vector<vector<int>>& vp,int l,int r,vector<int>& ans){
        if(l+1>=r) return;
        int m=l+(r-l)/2;
        sort_count(vp,l,m,ans);
        sort_count(vp,m,r,ans);
        int j=m;
        for(int i=l;i<m;i++){
            while(j<r && vp[j][0]<vp[i][0]) j++;
            ans[vp[i][1]]+=j-m;
        }
        inplace_merge(begin(vp)+l,begin(vp)+m,begin(vp)+r);
    }
```
similar problems:
493	Reverse Pairs
327	Count of Range Sum    		35.7%	Hard	

all relates to two sorted list count in O(n) time.
	
	
## sliding window

76	Minimum Window Substring
two pointer to find the substr and then shrink to the min.

```
    string minWindow(string s, string t) {
        unordered_map<char,int> mp;
        for(char c: t) mp[c]++;
        int i=0,j=0,mxlen=s.size()+1,mxind=-1;
        while(j<s.size()){
            mp[s[j]]--;
            while(valid(mp)) {
                if(mxlen>j-i+1){
                    mxlen=j-i+1;
                    mxind=i;
                }
                mp[s[i]]++,i++;
            }
            j++;
        }
        return mxind>=0?s.substr(mxind,mxlen):"";
    }
    bool valid(unordered_map<char,int>& mp){
        for(auto t: mp) if(t.second>0) return 0;
        return 1;
    }
```
similar using two pointers:
209	Minimum Size Subarray Sum 
159	Longest Substring with At Most Two Distinct Characters 
keep growing until not valid
340	Longest Substring with At Most K Distinct Characters  
keep growing until not valid
424	Longest Repeating Character Replacement 
485	Max Consecutive Ones
487	Max Consecutive Ones II
1004	Max Consecutive Ones III

992	Subarrays with K Different Integers  
get the number of subarray
idea: using 3 pointer, i<=j<=k, i to k for a valid subarray, j to k the min subarray.

```
    int subarraysWithKDistinct(vector<int>& A, int K) {
        int ans=0;
        int i=0,j=0,k=0;//use 3 pointers, i, j for the two left i<j
        unordered_map<int,int> mp;
        while(k<A.size())
        {
            mp[A[k]]++;
            if(mp.size()>K) {i=j+1;mp.erase(A[j]);}
            if(j<i) j=i;
            if(mp.size()==K) 
            {
                while(mp[A[j]]>1) {
                    mp[A[j]]--;
                    j++;
                }
                ans+=j-i+1;
            }
            k++;
        }
        return ans;
    }
```
1358	Number of Substrings Containing All Three Characters 

1703. Min adjacent swaps for k consecutive ones.
sliding window to find a subarray which contains k 1s. How do we get the min swaps to get them together?
10001011->10000111->01000111->00100111->00010111->00001111
the index: [0,4,6,7]->[5,5,5,5]->[4,5,6,7]  
                               ->[3,4,5,6]
[0,4,6,7]->[5,5,5,5] need 5-0+5-4+6-5+7-5=9
[5,5,5,5]->[4,5,6,7] need 1+1+2=4 this is extra move. total 9-4=5							   
							   
- the inner distribution matters.
- we can choose to move to one position (moving to median is the min sum).
above example: median=(4+6)/2=5
then divide into left and right: 
```
    int minMoves(vector<int>& nums, int k) {
        if(k==1) return 0;
        vector<int> ind;
        for(int i=0;i<nums.size();i++)
            if(nums[i]) ind.push_back(i);
        int ans=INT_MAX;
        for(int i=0;i<=ind.size()-k;i++){
            ans=min(ans,getswaps(ind,i,i+k));
            //cout<<ans<<" "<<i<<endl;
        }
        return ans==INT_MAX?0:ans;
    }
    
    int getswaps(vector<int>& ind,int l,int r){
        //odd: left and right the same, even: two median position
        int k=r-l;
        int med1=(ind[l+(k-1)/2]+ind[l+k/2])/2; //this is not correct for example [0,6,7], the median is 6
        int ans=0;
        //expand from the median
        int it=upper_bound(begin(ind)+l,begin(ind)+r,med1)-begin(ind);
        int tind=it;
        it--; //<=
        
        int cnt=0;
        while(it>=l) ans+=med1-ind[it--]-cnt++;
        cnt=1;
        while(tind<r) ans+=ind[tind++]-med1-cnt++;
        return ans;
    }
```
this will TLE since it is O(N^2)

need to optmize the getswaps using O(1)
- all moves to median position
- and then compensate the extras.
- using prefix to avoid O(N)

```
    int minMoves(vector<int>& nums, int k) {
        vector<long> A, B(1);
        for (int i = 0; i < nums.size(); ++i)
            if (nums[i])
                A.push_back(i);
        long n = A.size(), res = 2e9;
        for (int i = 0; i < n; ++i)
            B.push_back(B[i] + A[i]);
        for (int i = 0; i < n - k + 1; ++i)
            res = min(res, B[i + k] - B[k / 2 + i] - B[(k + 1) / 2 + i] + B[i]);
        res -= (k / 2) * ((k + 1) / 2);
        return res;
    }
```	
	

	
	
	
	
