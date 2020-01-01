## Biweek 13
250/2771 got all correct

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

