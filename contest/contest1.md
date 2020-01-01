# leetcode contest II
- bi13 250/2771 got 4/4
- 162: 849/6075 got 4/4
- 161: 605/6255 got 4/4
- bi12: 160/2975 got 4/4
- 160:  218/6150 got 4/4

## contest 160
### 1237. Find Positive Integer Solution for a Given Equation (***)
brutal force: try all combinations in the range for x, y and once we find a solution, break the internal loop (since f(x,y) is monotonic)

### 1238. Circular Permutation in Binary Representation (***)
gray code with any start.
```cpp
    vector<int> circularPermutation(int n, int start) {
        //circular reflection code
        //16bits at most
        int len=1<<n;
        int mid=0;
        vector<int> ans(len);
        for (int i=0; i<len; i++) {
            ans[i] = i^(i>>1);
            if(ans[i]==start) mid=i;
        }
        rotate(ans.begin(),ans.begin()+mid,ans.end());
        return ans;
    }
```
or simply using ans[i]=start^i^(i>>1);	

### 1239. Maximum Length of a Concatenated String with Unique Characters
### 1240. Tiling a Rectangle with the Fewest Squares

## biweek 12
### 1243. Array Transformation (**)
just check if we have operations

### 1244. Design A Leaderboard (**)
using a hashmap to record the scores and reset
using a pq to get the k highest score

### 1245. Tree Diameter (*****)
this is not a easy tree problem.
several points to know:
- a tree has n-1 edges and n nodes.
- using any node as the root does not change the diameter
- n-ary tree depth problem: the diameter is the max depth + 2nd max depth
```cpp
    int treeDiameter(vector<vector<int>>& edges) {
        //a tree has n-1 edges
        int n=edges.size();
        vector<vector<int>> adj(n+1);
        vector<bool> v(n+1);
        for(auto e: edges){
            adj[e[0]].push_back(e[1]);
            adj[e[1]].push_back(e[0]);
        }
        //using any node as root is equivalent
        int ans=0; //max diameter, n-ary tree, get the depth for each subtree
        dfs(0,0,adj,v,ans);
        return ans;
    }
    int dfs(int root,int depth,vector<vector<int>>& adj,vector<bool>& v,int& ans){
        if(v[root]) return 0; //visited
        v[root]=1;
        //get the max and 2nd max and ans=max+2ndmax
        int max1=0,max2=0;
        for(int node: adj[root]){
            int d=dfs(node,depth+1,adj,v,ans);
            if(d>max1){
                max2=max1;
                max1=d;
            }
            else if(d>max2) max2=d;
        }
        ans=max(ans,max1+max2);
        return 1+max1;
    }
```
attention:
- we need return maxdepth for each node to get the max and 2nd max
- max1 and max2 cannot set to int_min but has to set as 0
- it allows max1==max2 so need use d>max1.
- if visited return 0 (it just prevent going back, and we shall use min so that we do not mess up the max and 2nd max)

### 1246. Palindrome Removal (*****)
Given an integer array arr, in one move you can select a palindromic subarray arr[i], arr[i+1], ..., arr[j] where i <= j, and remove that subarray from the given array. Note that after removing a subarray, the elements on the left and on the right of that subarray move to fill the gap left by the removal.

Return the minimum number of moves needed to remove all numbers from the array.

this is a hard dp problem.
a string can have multiple palindrome substr, we can remove one of it and leaves a smaller size subproblem. But this is generally not a correct way since it involves changing of the array.

dp[i,j] represents the min removal for substr[i,j]
this forms a upper-triangle matrix.
dp[i,j]=min(dp[i,j],dp[i,k]+dp[k,j]) 

```cpp
    int minimumMoves(vector<int>& arr) {
        //dp: subproblem dp[l,r] and remove a substr which is pal
        int n = arr.size();
        // dp[left][right] = the min move for arr[left]...arr[right] (both included).
        vector<vector<int>> dp(n, vector<int>(n, n));
        for(int i = 0; i < n; i++) { dp[i][i] = 1; }
        for(int i = 0; i < n - 1; i++) { dp[i][i + 1] = arr[i] == arr[i + 1] ? 1 : 2; }
        
        for(int size = 3; size <= n; size++) {
            for(int left = 0, right = left + size - 1; right < n; left++, right++) {
                if(arr[left] == arr[right]) {
                    dp[left][right] = dp[left + 1][right - 1];
                }
                for(int mid = left; mid < right; mid++) {
                    dp[left][right] = min(dp[left][right], dp[left][mid] + dp[mid + 1][right]);
                }
            }
        }
        return dp[0][n - 1];        
    }
```
	
- 	

## contest 161
### 1247. Minimum Swaps to Make Strings Equal (****)
You are given two strings s1 and s2 of equal length consisting of letters "x" and "y" only. Your task is to make these two strings equal to each other. You can swap any two characters that belong to different strings, which means: swap s1[i] and s2[j].

Return the minimum number of swaps required to make s1 and s2 equal, or return -1 if it is impossible to do so.

it is not simple at all.
idea: 
- if s1[i]==s2[i], no need to swap, else we only have s1[i]==x,s2[i]==y and s1[i]==y and s2[i]==x two cases
- we count the number of two cases as xy and yx.
- base case 
xx-->xy even number of xy can make equal using xy/2 swaps. same as yx.
yy-->xy
yy
xx same as above
xy->xx->xy
yx->yy->xy
that means: (xy+yx) must be even
```cpp
    int minimumSwap(string s1, string s2) {
        //xx vs yy, xy vs yx are the two base cases
        //check not equal cases
        int xy=0,yx=0;
        for(int i=0;i<s1.size();i++){
            if(s1[i]=='x' && s2[i]=='y') xy++;
            else if(s1[i]=='y' && s2[i]=='x') yx++;
        }
        if((xy+yx)%2) return -1;
        return xy/2+yx/2+(xy%2)*2;
    }
```

### 1248. Count Number of Nice Subarrays (***)
find number of subarrays with k odd numbers in it.
slding window:
- fixed window, only keeps the index of the odd numbers and then by adding left and right non-odd numbers
```cpp
    int numberOfSubarrays(vector<int>& nums, int k) {
        //sliding window?
        int ans=0;
        vector<int> odds;
        odds.push_back(-1);
        for(int i=0;i<nums.size();i++){
            if(nums[i]%2) odds.push_back(i);
        }
        odds.push_back(nums.size());
        //moving window and calculate its left and right
        if(odds.size()<k+2) return 0;
        for(int i=1;i<odds.size()-k;i++){
            ans+=(odds[i]-odds[i-1])*(odds[i+k]-odds[i+k-1]);
        }

        return ans;
    }
	```
### 1249. Minimum Remove to Make Valid Parentheses (***)
remove invalid parentheses
idea: greedy using stack to pair and mark those parenthese to remove.
```cpp
    string minRemoveToMakeValid(string s) {
        stack<int> st;
        int cnt=0;
        vector<int> del;
        for(int i=0;i<s.size();i++){
            char c=s[i];
            if(c=='(') {st.push(i);}
            else if(c==')') 
            {
                if(st.size()) {st.pop();}
                else del.push_back(i);
            }
        }
        while(st.size()) del.push_back(st.top()),st.pop();
        sort(del.begin(),del.end());
        for(int i=del.size()-1;i>=0;i--)
            s.erase(del[i],1);
        return s;
    }
```
### 1250. Check If It Is a Good Array	(gcd of multiple numbers) (***)
extended euclid algorithm.
key point: math problem, ax+by+...=1, the coefficient must be coprime.

```cpp
    bool isGoodArray(vector<int>& nums) {
        int g=nums[0];
        for(int i: nums){
            g=__gcd(i,g);
            if(g==1) return 1;
        }
        return 0;
    }
```	


## contest 162
### 1252. Cells with Odd Values in a Matrix (*)
simple, use + or xor
### 1253. Reconstruct a 2-Row Binary Matrix (**)
given the sum of the two rows and column sums. reconstruct the array.
greedy approach: first fill those col sum==2 cases. then fill those colsum==1 by fill row 0 first.

### 1254. Number of Closed Islands (***)
0 is land, 1 is water.
closed island are those 0s which does not touch the boundary.
```cpp
    bool touched=0;
    int closedIsland(vector<vector<int>>& grid) {
        //closed islands are those 0s not touched the boundary
        int m=grid.size(),n=grid[0].size();
        int ans=0;
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==0){
                    touched=0;
                    //cout<<i<<" "<<j<<endl;          
                    dfs(grid,i,j);
                    ans+=!touched;
                }
            }
        }
        return ans;
    }
    void dfs(vector<vector<int>>& g,int i,int j){
        if(i<0||j<0||i>=g.size()||j>=g[0].size()||g[i][j]) return;
        if(i==0 || j==0 || i==g.size()-1 || j==g[0].size()-1) touched=1;
        g[i][j]=2;
        //cout<<i<<" "<<j<<endl;
        dfs(g,i-1,j);        dfs(g,i+1,j);
        dfs(g,i,j-1);        dfs(g,i,j+1);
    }
```	

### 1255. Maximum Score Words Formed by Letters (***)
question: given a list of characters, and each char has difference score, given s list of dictionary word, return the max score which can be obtained. No chars can be used more than provided.
note you can get multiple words.
approach: first form the hashmap of given letters. then check all valid words. check all combinations of these valid words and check if the combination is valid and get the score.
```cpp
    int maxScoreWords(vector<string>& words, vector<char>& letters, vector<int>& score) {
        vector<int> cnt(26);
        for(char c: letters) cnt[c-'a']++;
        //each word: first can be formed or not, if can be formed, the score
        //knapsack: use it or not, and we need keep track of the number
        //first get rid of those invalid strings
        vector<string> vwords;
        vector<int> vscore;
        vector<vector<int>> vcnt;
        for(int i=0;i<words.size();i++){
            bool valid=1;
            vector<int> tmp(26);
            for(char c: words[i]) tmp[c-'a']++;
            for(int j=0;j<26;j++) 
                if(tmp[j]>cnt[j]) valid=0;
            if(valid){
                int tsum=0;
                for(int j=0;j<26;j++) tsum+=tmp[j]*score[j];
                vscore.push_back(tsum);
                vwords.push_back(words[i]);
                vcnt.push_back(tmp);
            }
        }
        
        int n=vwords.size();
        //using bits
        int m=1<<n;
        int ans=0;
        for(int i=1;i<m;i++){
            int tscore=0;
            if(valid(i,vcnt,cnt,vscore,tscore)){
                    ans=max(ans,tscore);
            }
        }
        return ans;
    }
    bool valid(int s,vector<vector<int>>& vcnt,vector<int>& cnt,vector<int>& vscore,int& score){
        int n=vcnt.size();
        bitset<16> bits(s);
        score=0;
        vector<int> tmp(26);
        for(int i=0;i<n;i++){
            if(bits[i]){
                for(int j=0;j<26;j++) tmp[j]+=vcnt[i][j];
                score+=vscore[i];
            }
        }
        for(int j=0;j<26;j++){
            if(tmp[j]>cnt[j]) return 0;}
        return 1;
    }
```	

## Biweek 13 --pretty hard one
### 1256. Encode number (***)
0->"" (binary 0)
1->"0" (binary 1)
2->"1" (binary 10)
3->"00" (binary 11)
4->"01" (binary 100)
5->"10" (binary 101)
6->"11" (binary 110)
7->"000" (binary 111)
how can we deduce the converting function ?
binary of n+1:
0: 1
1: 10
2: 11
3: 100
4: 101
5: 110
6: 111
7: 1000
remove the first char

### 1257. Smallest Common Region (***)
a group of names, the first contains all others.
seems union find problem and find the lowest common ancestor
no need union actually. just build the structure and find the parent

```cpp
    string findSmallestRegion(vector<vector<string>>& regions, string region1, string region2) {
		unordered_map<string,string> parent;
		for(auto v: regions){
			string& par=v[0];
			for(int i=1;i<v.size();i++){
				parent[v[i]]=par; //do not overwrite 
			}
		}
		unordered_set<string> parent1;
		while(region1.size()){
			parent1.insert(region1);
			region1=parent[region1];
		}
		while(region2.size()){
		if(parent1.count(region2)) return region2;
			region2=parent[region2];
		}
		return "";
    }
```	

### 1258. Synonymous Sentences (****)
Given a list of pairs of equivalent words synonyms and a sentence text, Return all possible synonymous sentences sorted lexicographically.
idea: union find to merge the same set of synonyms. and then each set is sorted and then dfs/backtracking to get all combinations
it is not easy to design the right data structure.

```cpp
class Solution {
public:
	unordered_map<string,string> parent;
    vector<string> generateSentences(vector<vector<string>>& synonyms, string text) {
		vector<string> vtext;
		stringstream ss(text);
		string w;
		while(ss>>w) {
			vtext.push_back(w);
		}
		for(auto v: synonyms){
			if(!parent.count(v[0])) parent[v[0]]=v[0];
			if(!parent.count(v[1])) parent[v[1]]=v[1];
			merge(v[0],v[1]);
		}
		//assemble each disjoint set into sorted array or set.
		unordered_map<string,set<string>> sets;
		for(auto v: synonyms){
			string par=find_parent(v[0]);
			sets[par].insert(v[0]);
			sets[par].insert(v[1]);
		}
		//now do dfs to form the vector
		vector<string> ans;
		backtrack(sets,0,{},vtext,ans);
		return ans;
    }
	void backtrack(unordered_map<string,set<string>>& sets,int start,
		vector<string> t,vector<string>& vtext,vector<string>& ans){
		if(start==vtext.size()){
			string str;
			for(string s: t) str+=s+" ";
			str.pop_back();
			ans.push_back(str);
			return;
		}
		string w=vtext[start];
		string parent=find_parent(w);
		if(parent.size()){
			for(string s: sets[parent]){
				t.push_back(s);
				backtrack(sets,start+1,t,vtext,ans);
				t.pop_back();
			}
		}
		else{
            t.push_back(w);
			backtrack(sets,start+1,t,vtext,ans);
		}
	}
	string find_parent(string& a){
		if(!parent.count(a)) return ""; //not in the group
		while(a!=parent[a]) a=parent[a];
		return a;
	}
	void merge(string& a,string& b){
		string pa=find_parent(a),pb=find_parent(b);
		parent[pb]=pa;
	}
};
```	

### 1259. Handshakes That Don't Cross (*****)
even number of people in a circle, return number of ways that do not cross.
typical dp problem:
if person i and person j shake hands, then it divide into two groups:
assuming i<j:
(i+1)%n to (j-1+n)%n.-->dp(m1)
(j+1)%n to (i-1+n)%n.-->dp(m2) m1+m2=n-2
to avoid non-cross, each set shall have even number of people
for convenice we use pair.
for n pairs of people, we can divide into:
{0, n-1},{1,n-2},....{k,n-k-1}....{n-1,0} pairs

```cpp
    int numberOfWays(int n) {
        vector<long> dp(n/2+1);
		int mod=1e9+7;
		dp[0]=1;
		for(int k=1;k<=n/2;k++){
			for(int i=0;i<k;i++){
				dp[k]=(dp[k]+dp[i]*dp[k-1-i])%mod;
			}
		}
		return dp[n/2];
    }
```	

