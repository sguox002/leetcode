## biweek 2.
### 1085. Sum of Digits in the Minimum Number (*)
trivial

### 1086. high five
<em>Given a list of scores of different students, return the average score of each student's top five scores in the order of each student's id.

Each entry items[i] has items[i][0] the student's id, and items[i][1] the student's score.  The average score is calculated using integer division.
</em>
- less than 5 scores 
- same scores
can use multiset or pq.
map<int,pq<int>>

```cpp
	vector<vector<int>> highFive(vector<vector<int>>& items) {	
		vector<vector<int>> ans;
		map<int,priority_queue<int>> mp;
		for(auto t: items){
			mp[t[0]].push(t[1]);
		}
		for(auto t: mp){
			int sum=0,cnt=0;
			while(t.second.size() && cnt<5){
				sum+=t.second.top();
				t.second.pop();
                cnt++;
			}
			ans.push_back({t.first,sum/cnt});
		}
		return ans;
	}
```

### 1087. Brace Expansion
<em>A string S represents a list of words.

Each letter in the word has 1 or more options.  If there is one option, the letter is represented as is.  If there is more than one option, then curly braces delimit the options.  For example, "{a,b,c}" represents options ["a", "b", "c"].

For example, "{a,b,c}d{e,f}" represents the list ["ade", "adf", "bde", "bdf", "cde", "cdf"].

Return all words that can be formed in this manner, in lexicographical order.

Example 1:

Input: "{a,b}c{d,e}f"
Output: ["acdf","acef","bcdf","bcef"]
Example 2:

Input: "abcd"
Output: ["abcd"]
</em>

Intuition: backtracking.
- build the adjacent matrix from the string.
- backtracking or dfs.

```cpp
    vector<string> permute(string S) {
        vector<vector<char>> adj;
        vector<char> t;
        bool inbrack=0;
        for(char c: S){
            if(c!='{' && c!='}') {
                if(!inbrack) adj.push_back({c});
                else if(c!=',') t.push_back(c);
            }
            else if(c=='{'){
                inbrack=1;
            }
            else{
                if(!t.empty()) adj.push_back(t);
                t.clear();
                inbrack=0;
            }
        }
        vector<string> ans;
        dfs(adj,0,"",ans);
        sort(ans.begin(),ans.end());
        return ans;
    }
    
    void dfs(vector<vector<char>>& adj,int start,string t,vector<string>& ans){
        if(start==adj.size()) {ans.push_back(t);return;}

		for(int j=0;j<adj[start].size();j++)
		{
			dfs(adj,start+1,t+adj[start][j],ans);
		}
    }
```	

### 1088. Confusing Number II
<em>We can rotate digits by 180 degrees to form new digits. When 0, 1, 6, 8, 9 are rotated 180 degrees, they become 0, 1, 9, 8, 6 respectively. When 2, 3, 4, 5 and 7 are rotated 180 degrees, they become invalid.

A confusing number is a number that when rotated 180 degrees becomes a different number with each digit valid.(Note that the rotated number can be greater than the original number.)

Given a positive integer N, return the number of confusing numbers between 1 and N inclusive.

Example 1:

Input: 20
Output: 6
Explanation: 
The confusing numbers are [6,9,10,16,18,19].
6 converts to 9.
9 converts to 6.
10 converts to 01 which is just 1.
16 converts to 91.
18 converts to 81.
19 converts to 61.
</em>

Intuition: 

0-0, 1-1, 8-8, 9-6, 6-9 and final form need reverse.
we can check all combination of 01689, each digit can appear 0 to m times at different locations. O(n2^m)
backtracking solution:
```cpp
    int confusingNumberII(int N) {
        int total=0;
        dfs(N,0,0,0,0,total);
        return total;
    }   
    void dfs(int n,int start,int j,long t,long rt,int &ans){
        vector<int> digits={0,1,6,8,9},rdigits={0,1,9,8,6};
        if(t>n) return;
        if(t<=n && t!=rt) {ans++;}
        for(int i=0;i<5;i++){ //can choose multiple times
            if(j==0 && i==0) continue;
            dfs(n,i,j+1,t*10+digits[i],rt+rdigits[i]*pow(10,j),ans);
        }
    }
```
this will try a lot of invalid combinations. and the complexity is high.

Optimization:
we can use the fact that:
all numbers consist of 01689 only minus those symmetric numbers (strobogrammatric numbers)
so it reduces to two other questions:
- find the total number of 01689 only numbers (this is a combination problem)
for example 123: 
0xx: 5x5
10x: 5
11x: 5
total 25+10-1=34
- find the total number of strobogrammatic numbers (this is much easier, can use backtrack.)
