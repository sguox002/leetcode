## biweek 27

### 1460. Make Two Arrays Equal by Reversing Sub-array

<em>
Given two integer arrays of equal length target and arr.

In one step, you can select any non-empty sub-array of arr and reverse it. You are allowed to make any number of steps.

Return True if you can make arr equal to target, or False otherwise.
</em>
- brtual force
loop from left to right, find the element in the other array and reverse to the correct position.

- hashmap: the other array need to be a permutation of the array
```cpp
    bool canBeEqual(vector<int>& target, vector<int>& arr) {
        unordered_map<int,int> mp;
        for(int i: target) mp[i]++;
        for(int i: arr) mp[i]--;
        for(auto t: mp) if(t.second) return 0;
		return 1;
	}
```	

### 1461. Check If a String Contains All Binary Codes of Size K
<em>
iven a binary string s and an integer k.

Return True if all binary codes of length k is a substring of s. Otherwise, return False.
</em>

sliding window and bool array
```cpp
    bool hasAllCodes(string s, int k) {
        int n=s.size(),m=1<<k;
        vector<bool> flag(m);
        //sliding window
        int num=0,i=0;
        while(i<n){
            num=num*2+(s[i]-'0');
            if(i>=k) num-=(s[i-k]-'0')*(1<<k);
            if(i>=k-1) flag[num]=1;
            //cout<<i<<" "<<num<<endl;
            i++;
        }
        return accumulate(begin(flag),end(flag),0)==m;
    }
```

### 1462. Course Schedule IV
<em>
There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have direct prerequisites, for example, to take course 0 you have first to take course 1, which is expressed as a pair: [1,0]

Given the total number of courses n, a list of direct prerequisite pairs and a list of queries pairs.

You should answer for each queries[i] whether the course queries[i][0] is a prerequisite of the course queries[i][1] or not.

Return a list of boolean, the answers to the given queries.

Please note that if course a is a prerequisite of course b and course b is a prerequisite of course c, then, course a is a prerequisite of course c.
</em>

- do dfs to check if we can from a to b, but it will takes too long.
- post order traversal on each node and save the parent-relation in a hashmap, query can be performed on the hashmap
```cpp
    vector<bool> checkIfPrerequisite(int n, vector<vector<int>>& prerequisites, vector<vector<int>>& queries) {
        vector<vector<int>> adj(n);
        for(auto p: prerequisites){
            adj[p[0]].push_back(p[1]);
        }
        //we can precalculate all ij pairs and all other false
        vector<bool> v(n);
        unordered_map<int,unordered_set<int>> mp;
        for(int i=0;i<n;i++){
            if(!v[i]) dfs(adj,i,-1,mp,v);
        }
        vector<bool> ans;
        
        for(auto q: queries){
            if(mp[q[0]].count(q[1])) ans.push_back(1);
            else ans.push_back(0);
        }
        return ans;
    }
    void dfs(vector<vector<int>>& adj,int root,int parent,unordered_map<int,unordered_set<int>>& mp,vector<bool>& v){
        //shall use postorder parent - all child
        for(int ch: adj[root]){
            dfs(adj,ch,root,mp,v);
        }
        v[root]=1;
        if(parent>=0) {
            for(auto t: mp[root])
            mp[parent].insert(t);
            mp[parent].insert(root);
        }
    }

```

we performed dfs on all nodes, so the complexity would be O(N^2), and it will TLE for large test cases.
- each node may have multiple parents and multiple children. that's why we do not use v in the dfs. but the in the main function, we use it if is alreaady visited.
- N is small up to 100, but why will it TLE?

optimization: 
in the dfs, when a node is visited, we just add the results into the hashmap.

```cpp
    vector<bool> checkIfPrerequisite(int n, vector<vector<int>>& prerequisites, vector<vector<int>>& queries) {
        vector<vector<int>> adj(n);
        for(auto p: prerequisites){
            adj[p[0]].push_back(p[1]);
        }
        //we can precalculate all ij pairs and all other false
        vector<bool> v(n);
        unordered_map<int,unordered_set<int>> mp;
        for(int i=0;i<n;i++){
            if(!v[i]) dfs(adj,i,-1,mp,v);
        }
        //print(mp);
        vector<bool> ans;
        
        for(auto q: queries){
            if(mp[q[0]].count(q[1])) ans.push_back(1);
            else ans.push_back(0);
        }
        return ans;
    }
    void dfs(vector<vector<int>>& adj,int root,int parent,unordered_map<int,unordered_set<int>>& mp,vector<bool>& v){
        //shall use postorder parent - all child
        //cout<<root<<endl;
        for(int ch: adj[root]){
            if(!v[ch]) dfs(adj,ch,root,mp,v);
            else{
                for(auto t: mp[ch]) mp[root].insert(t);
                mp[root].insert(ch);
            }
        }
        v[root]=1;
        if(parent>=0) {
            for(auto t: mp[root])
            mp[parent].insert(t);
            mp[parent].insert(root);
        }
    }
```

or we can build a matrix to record the connection. O(N^3).

### 1463. Cherry Pickup II

<em>
Given a rows x cols matrix grid representing a field of cherries. Each cell in grid represents the number of cherries that you can collect.

You have two robots that can collect cherries for you, Robot #1 is located at the top-left corner (0,0) , and Robot #2 is located at the top-right corner (0, cols-1) of the grid.

Return the maximum number of cherries collection using both robots  by following the rules below:

From a cell (i,j), robots can move to cell (i+1, j-1) , (i+1, j) or (i+1, j+1).
When any robot is passing through a cell, It picks it up all cherries, and the cell becomes an empty cell (0).
When both robots stay on the same cell, only one of them takes the cherries.
Both robots cannot move outside of the grid at any moment.
Both robots should reach the bottom row in the grid.
</em>

this is very similar to cherry pickup I where two branches start from the top left corner.
difference：
- robot can only move downward diagonal.
- for m rows, both robot walks m-1 steps.
- each row we can pick up to two cells only.
- if they step into the same cell, only one is picked.

robot 1 position (i, j1)
robot 2 position (i, j2)
at each position each robot has up to 2 choices: left or right.
4 combination
nums[i,j1]+nums[i,j2], j1 starts from 0, and j2 starts from n-1.
i+1,j1+1,j2+1
i+1,j1+1,j2-1
i+1,j1-1, j2+1
i+1,j1-1, j2-1
dp[i,j0,j1]
base case one row: 

We will try top down first:
```cpp
    int dp[70][70][70] = {};
    int cherryPickup(vector<vector<int>>& grid) {
        memset(dp, -1, sizeof(dp));
        int m = grid.size(), n = grid[0].size();
        return dfs(grid, m, n, 0, 0, n - 1);
    }
    int dfs(vector<vector<int>>& grid, int m, int n, int r, int c1, int c2) {
        if (r == m) return 0; // Reach to bottom row
        if (dp[r][c1][c2] != -1) return dp[r][c1][c2];
        int ans = 0;
        for (int i = -1; i <= 1; i++) {
            for (int j = -1; j <= 1; j++) {
                int nc1 = c1 + i, nc2 = c2 + j;
                if (nc1 >= 0 && nc1 < n && nc2 >= 0 && nc2 < n) {
                    ans = max(ans, dfs(grid, m, n, r + 1, nc1, nc2));
                }
            }
        }
		int cherries = c1 == c2 ? grid[r][c1] : grid[r][c1] + grid[r][c2];
        return dp[r][c1][c2] = ans + cherries;
    }
```
	


```cpp
	int dp[70][70][70]={0};
    int cherryPickup(vector<vector<int>>& grid) {
		int m=grid.size(),n=grid[0].size();
        memset(dp,0,70*70*70*sizeof(int));
        if(n>1)	dp[0][0][n-1]=grid[0][n-1]+grid[0][0];
        else dp[0][0][n-1]=grid[0][0]; //only one.
		int ans=dp[0][0][n-1];
		for(int i=1;i<m;i++){
			for(int j0=0;j0<n;j0++){
				for(int j1=0;j1<n;j1++){
					int mx=0;
					for(int k=-1;k<=1;k++) { //+1 0 -1
						for(int l=-1;l<=1;l++){
                            if(j0+k<0 || j0+k>=n || j1+l<0 || j1+l>=n) continue;
							mx=max(mx,dp[i-1][j0+k][j1+l]);
						}
					}
					if(j0==j1) dp[i][j0][j1]=grid[i][j0]+mx;
					else dp[i][j0][j1]=grid[i][j0]+grid[i][j1]+mx;
                    if(i==m-1) ans=max(ans,dp[i][j0][j1]);
				}
			}
		}
		return ans;
	}
	
```	
note:
it also calculated those not reachable cells which shall be excluded.
the robot one can only reach to m-1 max. 
the robot one can reach n-1-(m-1) max.

finally the correct code:

```cpp
	int dp[70][70][70]={0};
    int cherryPickup(vector<vector<int>>& grid) {
		int m=grid.size(),n=grid[0].size();
        memset(dp,-1,70*70*70*sizeof(int));
        if(n>1)	dp[0][0][n-1]=grid[0][n-1]+grid[0][0];
        else dp[0][0][n-1]=grid[0][0]; //only one.
		int ans=dp[0][0][n-1];
		for(int i=1;i<m;i++){
			for(int j0=0;j0<min(i+1,n);j0++){
				for(int j1=max(n-1-i,0);j1<n;j1++){
                    //cout<<i<<" "<<j0<<" "<<j1<<endl;
					int mx=0;
					for(int k=-1;k<=1;k++) { //+1 0 -1
						for(int l=-1;l<=1;l++){
                            if(j0+k<0 || j0+k>=n || j1+l<0 || j1+l>=n && dp[i-1][j0+k][j1+l]<0) continue;
							mx=max(mx,dp[i-1][j0+k][j1+l]);
						}
					}
					if(j0==j1) dp[i][j0][j1]=grid[i][j0]+mx;
					else dp[i][j0][j1]=grid[i][j0]+grid[i][j1]+mx;
                    if(i==m-1) ans=max(ans,dp[i][j0][j1]);
				}
			}
		}
		return ans;
	}
```	

- using -1 to indicate not visited and not reachable
- use the fact that robot 1 can only reach to i th column and robot 2 can only reach to n-1-i th column to save a bit computation






	
