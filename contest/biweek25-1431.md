## biweek 25
### 1431. Kids With the Greatest Number of Candies
<em>
Given the array candies and the integer extraCandies, where candies[i] represents the number of candies that the ith kid has.

For each kid check if there is a way to distribute extraCandies among the kids such that he or she can have the greatest number of candies among them. Notice that multiple kids can have the greatest number of candies.
</em>

straightforward: give the extra to himself and see if it is >=max.

```cpp
    vector<bool> kidsWithCandies(vector<int>& candies, int extraCandies) {
        int mx=*max_element(begin(candies),end(candies));
        vector<bool> ans;
        for(int i: candies)
            ans.push_back(i>=mx-extraCandies);
        return ans;
    }
```

### 1432. Max Difference You Can Get From Changing an Integer
<em>
You are given an integer num. You will apply the following steps exactly two times:

Pick a digit x (0 <= x <= 9).
Pick another digit y (0 <= y <= 9). The digit y can be equal to x.
Replace all the occurrences of x in the decimal representation of num by y.
The new integer cannot have any leading zeros, also the new integer cannot be 0.
Let a and b be the results of applying the operations to num the first and second times, respectively.

Return the max difference between a and b.
</em>

greedy: 
to get the max number, replace the first seen number <9 to 9.
to get the min number, 
- if the first number >1, replace it to 1
- else replace the first seen number >1 to 0

```cpp
    int maxDiff(int num) {
        //replace to make one largest and one smallest
        //to make it larger, replace the first one <9 to 9
        string s=to_string(num);
        char rep=-1;
        for(char& c: s){
            if(rep<0 && c<'9') {
                rep=c;
                c='9';
            }
            if(c==rep) c='9'; 
        }
        int a=stoi(s);
        s=to_string(num);
        //find the first one >0 and change it to 0, except the first one
        rep=-1;
        int b;
        char nc,minc='0';
        if(num<9) b=1; //cannot be 0
        else{
           if(s[0]>'1') {
               rep=s[0];
               s[0]=nc='1';
           }else{//this '1' cannot be the new char
               minc='1';
           }
            
           for(int i=1;i<s.size();i++){
               if(s[i]==rep){
                   s[i]=nc;
               }
               if(rep<0 && s[i]>minc){
                   rep=s[i];
                   s[i]=nc='0';
               }
           }
        }
        b=stoi(s);
        return a-b;
    }
```
This code is too long, we can use a function:

```cpp
    int maxDiff(int num) {
        string s = to_string(num);
        
		// Replace all occurrences of the first char that is NOT included in `chs` with `ch`.
        auto f = [](string str, string chs, char ch) {
            auto pos = str.find_first_not_of(chs);
            if (pos != string::npos) {
                char a = str[pos];
                replace(begin(str), end(str), a, ch);
            }
            return stoi(str);
        };
        
		// Replacing all occurrences of the first non-'9' with '9' gives the maximum.
        int mx = f(s, "9", '9');
		
		// To get the minimum, there are two cases:
		// a) s[0] == '1': Replace all occurrences of the first char that is neither '0' nor '1' with '0'.
		// b) s[0] != '1' (or s[0] > '1'): Replace all occurrences of s[0] with '1'.
        int mn = s[0] == '1' ? f(s, "01", '0') : f(s, "1", '1');
		
        return mx - mn;
    }
```

### 1433. Check If a String Can Break Another String
<em>
Given two strings: s1 and s2 with the same size, check if some permutation of string s1 can break some permutation of string s2 or vice-versa (in other words s2 can break s1).

A string x can break string y (both of size n) if x[i] >= y[i] (in alphabetical order) for all i between 0 and n-1.
</em>

equivalent: if sorted, the suffix string is consistently >= or <=

```cpp
    bool checkIfCanBreak(string s1, string s2) {
        sort(begin(s1),end(s1));
        sort(begin(s2),end(s2));
        if(s1>s2) swap(s1,s2);
        for(int i=0;i<s1.size();i++){
            if(s1[i]>s2[i]) return 0;
        }
        return 1;
    }
```
O(nlogn)

O(N) approach:
we can use bucket sort to reduce the complexity of sort. That's basically the hashmap
then we compare character by character:
note the one left shall bring forward
for example
s1: 3a2b
s2: 2a3b
then s1 has one a to go forward, leaving finally 1a vs 1b
aaabb
aabbb

```cpp
    bool checkIfCanBreak(string s1, string s2) {
		vector<int> cnt1(26),cnt2(26);
		for(char c: s1) cnt1[c-'a']++;
		for(char c: s2) cnt2[c-'a']++;
		//using prefix sum and compare
        int a=0,b=0,dir=0;
        for(int i=0;i<26;i++){
            a+=cnt1[i];
            b+=cnt2[i];
            if(dir==1 && a<b) return false;
            if(dir==2 && a>b) return false;
            if(a>b && dir==0) dir=1;
            else if(a<b && dir==0) dir=2;
        }
        return true;		
	}
```
	
### 1434. Number of Ways to Wear Different Hats to Each Other
<em>
There are n people and 40 types of hats labeled from 1 to 40.

Given a list of list of integers hats, where hats[i] is a list of all hats preferred by the i-th person.

Return the number of ways that the n people wear different hats to each other.

Since the answer may be too large, return it modulo 10^9 + 7.
</em>

Apparently this is a dp problem.
Since the bottom up relation is not that straightforward, let's start from the backtracking recursive approach first.

```cpp
    int mod=1e9+7;
	int numberWays(vector<vector<int>>& hats) {
		int n=hats.size(); //num of people
		vector<bool> v(41); //1 to 40
		return backtrack(hats,0,v);
    }
	
	long backtrack(vector<vector<int>>& hats,int start,vector<bool>& v){
		if(start>=hats.size()) return 1; //reach the end
		int ans=0;
		for(int h: hats[start]){
			if(v[h]) continue;
			v[h]=1;
			ans+=backtrack(hats,start+1,v);
			ans%=mod;
			v[h]=0;
		}
		return ans%mod;
	}
```

The approach is correct and could be verified using some example tests.
However, the complexity would be 40^n. (each person can have up to 40 choices)

Memoization approach:
memoization of above backtracking is not straightforward since it involves start and v.
v is a combination of 40 hats.
It is naturally to use bit for the v, so we need:
dp[n][1<<40], this is too much storage.

In this case, we need convert the problem equivalently to [40][1<<n] since n<10.

let's do the recursive approach first:
```cpp
    int mod=1e9+7;
	int numberWays(vector<vector<int>>& hats) {
		int n=hats.size(); //num of people
		//convert hats to people
		vector<vector<int>> h2p(41);
		for(int i=0;i<hats.size();i++){
			for(int j: hats[i]) h2p[j].push_back(i);
		}
		//now equivalent problem: with hats to differnent people, can we arrange all? 
		//if we get all bits set 1, we get one combination.
		//vector<bool> v(n); //now for n people, better using bits
		
		return backtrack(h2p,1,0,n);//start from hat 1 and state=0
    }
	
	long backtrack(vector<vector<int>>& h2p,int start,int state,int n){
		if(state==(1<<n)-1) return 1; //reach the end
        if(start>40) return 0; //tried all hats

        int ans=backtrack(h2p,start+1,state,n);//do not wear this hat.
		for(int p: h2p[start]){
			if(state&(1<<p)) continue;//if person is set.
			ans+=backtrack(h2p,start+1,state+(1<<p),n);
			ans%=mod;
		}
		return ans%mod;
	}
```
complexity would be 2^40*2^n.
- note for hats, it has two options, to be used or not used. This is similar to knapsack.

Now we can add memoization:
```cpp
    int mod=1e9+7;
	int numberWays(vector<vector<int>>& hats) {
		int n=hats.size(); //num of people
		//convert hats to people
		vector<vector<int>> h2p(41);
		for(int i=0;i<hats.size();i++){
			for(int j: hats[i]) h2p[j].push_back(i);
		}
		//now equivalent problem: with hats to differnent people, can we arrange all? 
		//if we get all bits set 1, we get one combination.
		//vector<bool> v(n); //now for n people, better using bits
		int m=1<<n;
		vector<vector<long>> dp(41,vector<int>(m,-1));
		return backtrack(h2p,1,0,n);//start from hat 1 and state=0
    }
	
	long backtrack(vector<vector<int>>& h2p,int start,int state,int n){
		if(state==(1<<n)-1) return 1; //reach the end
        if(start>40) return 0; //tried all hats
		if(dp[start][state]>=0) return dp[start][state];
        int ans=backtrack(h2p,start+1,state,n);//do not wear this hat.
		for(int p: h2p[start]){
			if(state&(1<<p)) continue;//if person is set.
			ans+=backtrack(h2p,start+1,state+(1<<p),n);
			ans%=mod;
		}
		return dp[start][state]=ans%mod;
	}
```
now the complexity is O(40*2^n)

Based on the memoization, bottom up approach can be easily obtained.	
	
