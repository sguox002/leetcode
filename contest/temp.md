## Contest 141

### 1089. Duplicate Zeros (***)
very interesting problem. Need in-place to duplicate zero elements.

idea: 

using two pointer left to right and then right to left to fill zeros

```cpp
    void duplicateZeros(vector<int>& arr) {
        int i=0,j=0;
        int cnt0=0;
        while(i<arr.size()){
            if(arr[i]==0) j++;
            i++,j++;
        }
        i--,j--;
        while(i>=0){
            if(j<arr.size()) arr[j]=arr[i];
            if(arr[i]==0) {
                j--;
                if(j<arr.size()) arr[j]=0;
            }
            i--,j--;
        }
    }
```

	
### 1090. Largest Values From Labels
<em>Problem: 

We have a set of items: the i-th item has value values[i] and label labels[i].

Then, we choose a subset S of these items, such that:

|S| <= num_wanted
For every label L, the number of items in S with label L is <= use_limit.
Return the largest possible sum of the subset S.</em>
Idea:
keep each label only use_limit from large to small. Then pick global max.
pq is perfect for this.

```cpp
    int largestValsFromLabels(vector<int>& values, vector<int>& labels, int num_wanted, int use_limit) {
        unordered_map<int,multiset<int,greater<int>>> mp;
        for(int i=0;i<values.size();i++) mp[labels[i]].insert(values[i]);
        //choose limit from the k-sorted list
        priority_queue<int> pq;
        int ans=0;
        for(auto t: mp){ //each set we can take limit from it
            for(int i=0;i<use_limit;i++) 
                if(t.second.size()) {
                    pq.push(*t.second.begin());
                    t.second.erase(t.second.begin());
                }
        }
        while(num_wanted-- && pq.size()) ans+=pq.top(),pq.pop();
        return ans;
    }
```

### 1091. Shortest Path in Binary Matrix
<em>In an N by N square grid, each cell is either empty (0) or blocked (1).

A clear path from top-left to bottom-right has length k if and only if it is composed of cells C_1, C_2, ..., C_k such that:

Adjacent cells C_i and C_{i+1} are connected 8-directionally (ie., they are different and share an edge or corner)
C_1 is at location (0, 0) (ie. has value grid[0][0])
C_k is at location (N-1, N-1) (ie. has value grid[N-1][N-1])
If C_i is located at (r, c), then grid[r][c] is empty (ie. grid[r][c] == 0).
Return the length of the shortest such clear path from top-left to bottom-right.  If such a path does not exist, return -1.
</em>
regular bfs with 8 directions:

```cpp
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        //bfs
        int n=grid.size();
        if(n<2) return n;
        vector<int> visited(n*n);
        if(grid[0][0]||grid[n-1][n-1]) return -1;
        queue<int> q;
        q.push(0);visited[0]=1;
        int step=1;
        int dir[][2]={{-1,0},{1,0},{0,-1},{0,1},{-1,-1},{1,1},{1,-1},{-1,1}};
        while(q.size())
        {
            int sz=q.size();
            while(sz--){
                int t=q.front();q.pop();
                if(t==n*n-1) return step;
                int i=t/n,j=t%n;
                for(auto v: dir){
                    int x=i+v[0],y=j+v[1];
                    if(x<0 ||y<0 ||x>=n || y>=n || grid[x][y] || visited[x*n+y]) continue;
                    //cout<<x<<" "<<y<<endl;
                    visited[x*n+y]=1;
                    q.push(x*n+y);
                }
            }
            step++;
        }
        return -1;
    }
```

### 1092. Shortest Common Supersequence
<em>Given two strings str1 and str2, return the shortest string that has both str1 and str2 as subsequences.  If multiple answers exist, you may return any of them.

(A string S is a subsequence of string T if deleting some number of characters from T (possibly 0, and the characters are chosen anywhere from T) results in the string S.)

Example 1:

Input: str1 = "abac", str2 = "cab"
Output: "cabac"
Explanation: 
str1 = "abac" is a subsequence of "cabac" because we can delete the first "c".
str2 = "cab" is a subsequence of "cabac" because we can delete the last "ac".
The answer provided is the shortest such string that satisfies these properties.
</em>
idea: 
shortest common supersequence = A+B-Intersection(A,B)
we can find the longest common subsequence and then form the shortest supersequence.
A bit different: we are looking for lcs string, not the length.

```cpp
    string shortestCommonSupersequence(string& A, string& B) {
        int i = 0, j = 0;
        string res = "";
        for (char c : lcs(A, B)) {
            while (A[i] != c)
                res += A[i++];
            while (B[j] != c)
                res += B[j++];
            res += c, i++, j++;
        }
        return res + A.substr(i) + B.substr(j);
    }

    string lcs(string& A, string& B) {
        int n = A.size(), m = B.size();
        vector<vector<string>> dp(n + 1, vector<string>(m + 1, ""));
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < m; ++j)
                if (A[i] == B[j])
                    dp[i + 1][j + 1] = dp[i][j] + A[i];
                else
                    dp[i + 1][j + 1] = dp[i + 1][j].size() > dp[i][j + 1].size() ?  dp[i + 1][j] : dp[i][j + 1];
        return dp[n][m];
    }
```

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

## contest 141
### 1078. Occurrences After Bigram
<em>Given words first and second, consider occurrences in some text of the form "first second third", where second comes immediately after first, and third comes immediately after second.

For each such occurrence, add "third" to the answer, and return the answer.</em>
Approach:
find a two word pattern matching. store each word in a 2 word storage.

### 1079. Letter Tile Possibilities
<em>
You have a set of tiles, where each tile has one letter tiles[i] printed on it.  Return the number of possible non-empty sequences of letters you can make.</em>

Intuition:

Apparently this is permutation with duplicates. Also it allows different length of target.
- we can do combination and then sort the string and calculate the permutation.
- we can backtrack to get all permutation
combination:
```cpp
    int numTilePossibilities(string tiles) {
        unordered_set<string> ms;
        int ans=0;
        dfs(tiles,0,"",ms);
        //for each string in the ms, we can calculate number of its permutation
        for(auto t: ms)
            ans+=numperm(t);
        return ans;
    }
    void dfs(string& s,int start,string t,unordered_set<string>& ms)
    {
        if(t.size()) {
			string tt=t;
			sort(tt.begin(),tt.end());
			ms.insert(tt);
		}
        
        for(int i=start;i<s.size();i++)
        {
            t+=s[i];
            dfs(s,i+1,t,ms);
            t.pop_back();
        }
    }
    int numperm(string& t)
    {
        //cout<<t<<endl;
        vector<int> cmap(26);
        for(char c: t) cmap[c-'A']++;
        int n=t.length();
        int ans=1;
        for(int i=0;i<26;i++) //C(n,m) for each
            if(cmap[i]>0) {
                ans*=comb(n,cmap[i]);
                n-=cmap[i];
            }
        return ans;
    }
    //see 377
    int comb(int m,int n)
	{//m!/(n!*(m-n)!)
		int ans=1;
    	for(int i=n+1,j=1;i<=m || j<=m-n;i++,j++) {if(i<=m)ans*=i;if(j<=m-n) ans/=j;}
		return ans;
	}
```	
backtracking using hashmap:
```cpp
    int numTilePossibilities(string tiles) {
        vector<int> cnt(26);
        for(char c: tiles) cnt[c-'A']++;
        return dfs(cnt);
    }
    int dfs(vector<int>& cnt){
        int ans=0;
        for(int i=0;i<26;i++){
            if(cnt[i]==0) continue;
            ans++;
            cnt[i]--;
            ans+=dfs(cnt);
            cnt[i]++;
        }
        return ans;
    }
```

### 1080. Insufficient Nodes in Root to Leaf Paths
<em>Given the root of a binary tree, consider all root to leaf paths: paths from the root to any leaf.  (A leaf is a node with no children.)

A node is insufficient if every such root to leaf path intersecting this node has sum strictly less than limit.

Delete all insufficient nodes simultaneously, and return the root of the resulting binary tree.	
</em>

Idea:

sum(path)<limit. if we do preorder traverse, we subtract the parent value and get new limit.
two cases: leaf node itself, inner node causing subtree removal.
```cpp
    TreeNode* sufficientSubset(TreeNode* root, int limit) {
        if(!root) return 0;
		if(root->left==root->right) 
            return root->val<limit?0:root; //decide if leaf node shall removed
		root->left=sufficientSubset(root->left,limit-root->val);
		root->right=sufficientSubset(root->right,limit-root->val);
		return root->left==root->right?0:root; //decide if substree shall be removed
    }
```	
- leaf node shall return 0 or root, if let leaf to process by recursion, then it is incorrect.

### 1081. Smallest Subsequence of Distinct Characters
<em>Return the lexicographically smallest subsequence of text that contains all the distinct characters of text exactly once.
Example 1:

Input: "cdadabcc"
Output: "adbc"</em>

Intuition:

hashmap with monotonic stack.
we keep a hashmap of each char's hist, and then char by char to store in stack.
if current char is smaller than stack top, we pop it if it still has, otherwise we keep.

```cpp
    string smallestSubsequence(string s, string res = "") {
      int cnt[26] = {}, used[26] = {};
      for (auto ch : s) ++cnt[ch - 'a'];
      for (auto ch : s) {
        --cnt[ch - 'a'];
        if (used[ch - 'a']++ > 0) continue;
        while (!res.empty() && res.back() > ch && cnt[res.back() - 'a'] > 0) {
          used[res.back() - 'a'] = 0;
          res.pop_back();
        }
        res.push_back(ch);
      }
      return res;
    }   
```
	
## contest 139
1071. Greatest Common Divisor of Strings
gcd for strings. using similar algorithm for numbers. remove the shorter one from the longer one.

```cpp
    string gcdOfStrings(string str1, string str2) {
        if (str1.size() < str2.size()) return gcdOfStrings(str2, str1);
        if (str2.empty()) return str1;
        if (str1.substr(0, str2.size()) != str2) return "";
        return gcdOfStrings(str1.substr(str2.size()), str2);
    }
```
1072. Flip Columns For Maximum Number of Equal Rows
<em>Given a matrix consisting of 0s and 1s, we may choose any number of columns in the matrix and flip every cell in that column.  Flipping a cell changes the value of that cell from 0 to 1 or from 1 to 0.

Return the maximum number of rows that have all values equal after some number of flips.</em>

Idea:

It seems hard to find the row col relation. let us think k same row. if we flip any column, it must change k rows. but the property not change. So it is equivalent to find the most frequent duplicate 	 row. We can use string serialization for row using hashmap.

```cpp
    int maxEqualRowsAfterFlips(vector<vector<int>>& matrix) {
		//row must be same or flip each bit the same.
		//so problem is converted to find the max number of such rows.
		int m=matrix.size(),n=matrix[0].size();
		unordered_map<string,int> mp;
		for(auto& v: matrix){
			int t=v[0];
			string s;
			for(int i: v) {if(i==t) s+='1';else s+='0';}
			mp[s]++;
		}
		//find the max
		int ans=0;
		for(auto t: mp) ans=max(ans,t.second);
		return ans;
	}
```

1073. Adding Two Negabinary Numbers
<em>Given two numbers arr1 and arr2 in base -2, return the result of adding them together.

Each number is given in array format:  as an array of 0s and 1s, from most significant bit to least significant bit.  For example, arr = [1,1,0,1] represents the number (-2)^3 + (-2)^2 + (-2)^0 = -3.  A number arr in array format is also guaranteed to have no leading zeros: either arr == [0] or arr[0] == 1.

Return the result of adding arr1 and arr2 in the same format: as an array of 0s and 1s with no leading zeros.</em>

base as negative problem:
its coefficient is still non-negative, 0 and 1. (the good thing using negative base: no need to have a sign bit)
a number can represent as sum(A[i]*b^i)-->how to represent a number using negative base):
similarly, n%b->A[i] for example 3=(-2)^2+(-2)^1+(-2)^0=111
the operation for mod we shall use & and / shall use shift.

```cpp
    vector<int> addNegabinary(vector<int>& arr1, vector<int>& arr2) {
        int cf=0;
        int m=arr1.size(),n=arr2.size();
        vector<int> ans;
        int i=m-1,j=n-1;
        while(cf || i>=0 || j>=0)
        {
            int d=cf+(i>=0?arr1[i]:0)+(j>=0?arr2[j]:0);
            //
            ans.push_back(d&1);
            cf=-(d>>1);
            i--,j--;
        }
        while(ans.size()>1 && ans.back()==0) ans.pop_back();
        reverse(ans.begin(),ans.end());
        
        return ans;
    }
```	

1074. Number of Submatrices That Sum to Target
<em>Given a matrix, and a target, return the number of non-empty submatrices that sum to target.

A submatrix x1, y1, x2, y2 is the set of all cells matrix[x][y] with x1 <= x <= x2 and y1 <= y <= y2.

Two submatrices (x1, y1, x2, y2) and (x1', y1', x2', y2') are different if they have some coordinate that is different: for example, if x1 != x1'.	
</em>
Classic problem: reduce to 1d sum to target problem.

```cpp
    int numSubmatrixSumTarget(vector<vector<int>>& matrix, int target) {
		int m=matrix.size(),n=matrix[0].size();
		int ans=0;
		for(int i=0;i<m;i++){
			vector<int> rsum(n);
			for(int j=i;j<m;j++){
				for(int k=0;k<n;k++) rsum[k]+=matrix[j][k];
				ans+=numsubarray_sum(rsum,target);
			}
		}
		return ans;
	}
	int numsubarray_sum(vector<int>& nums,int target){
		int sum=0;
		unordered_map<int,int> mp;
		mp[0]++;
		int ans=0;
		for(int i: nums){
			sum+=i; //prefix sum
			ans+=mp[sum-target];
			mp[sum]++;
		}
		return ans;
	}
```
	


