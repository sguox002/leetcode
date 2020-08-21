## contest 115
957. Check Completeness of a Binary Tree5
958. Prison Cells After N Days6
959. Regions Cut By Slashes7
960. Delete Columns to Make Sorted III

957. Prison Cells After N Days.md

### Problem Summary
8 cell, when neighboring cell is the same, then it become 1, otherwise it becomes 0

Key observation:
- 8 bits only have at most 256 combinations, so when N is really large it is just repeating the pattern
- the boundary cell, the leftmost and rightmost will become 0 at the second day.
- using a hashmap to store the status and its first position, when a status appear again, we know the period.
- skipping to the remaing part and do the iteration.

### Implementation
```cpp
    vector<int> prisonAfterNDays(vector<int>& cells, int N) {
        //neigboring 0,0=>1 1,1=>0, otherwise =>0
        int n=cells.size();
        unordered_map<int,int> status;
        vector<int> curr(n);
        int period;
        for(int i=1;i<=N;)
        {
            int res=0;
            for(int j=1;j<n-1;j++)
            {
                int t=cells[j-1]+cells[j+1];
                curr[j]=t==1?0:1;
                res+=(curr[j]<<j);
            }
            if(status.count(res)) //we have a repeat pattern
            {
                period=i-status[res];
                int nskip=(N-i)/period*period;
                i+=nskip;
                i++;
            }
            else {status[res]=i;i++;}
            cells=curr;
        }
        return cells;
    }
```    

### Comments


958. Check Completeness of a Binary Tree.md

### Approach
level traversal. Pushing node's child into queue (including null child). When we find the first null node, we mark it, if there is no more null nodes behind, then it is a full tree.

### Implementation
```cpp
    bool isCompleteTree(TreeNode* root) {
        //level order traversal
        if(!root) return 1;
        queue<TreeNode*> q;
        q.push(root);
        int level=0;
        bool flag_null=0;
        while(q.size())
        {
            int sz=q.size();
            for(int i=0;i<sz;i++)
            {
                TreeNode* node=q.front();
                q.pop();
                if(node && flag_null) return 0;
                if(!node) {flag_null=1;continue;}
                q.push(node->left);
                q.push(node->right);
            }
            level++;
        }
        return 1;
    }
```    

959. Regions Cut By Slashes.md

### Problem Summary
The problem description is a bit hard to understand. It is easy to think that the problem can be approached using dfs/bfs/disjoint set. The difficulty lies how to convert the / and \ into more understandable format.
A / or \ split a cell into half. But we don't know how to represent a half cell.
If we upsample the grid, we can easily split the area with an approximated line. 
2x2 is not sufficient since the line is too thick to block the connected area. Using a 3x3 grid is clear enough
for example this case:
//
 /

### Implementation
using dfs and mark those visited, very simple

```cpp
    int regionsBySlashes(vector<string>& grid) {
        //upsample the grid by 2, so we can represent the /\ in a whole cell
        int n=grid.size();
        vector<vector<int>> g(3*n,vector<int>(3*n));

        for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]=='\\') {g[3*i][3*j]=g[3*i+1][3*j+1]=g[3*i+2][3*j+2]=1;}
                else if(grid[i][j]=='/') {g[3*i][3*j+2]=g[3*i+1][3*j+1]=g[3*i+2][3*j]=1;}
            }
        }
        int numset=0;
        for(int i=0;i<3*n;i++)
        {
            for(int j=0;j<3*n;j++)
            {
                if(!g[i][j]) dfs(g,i,j),numset++;
            }
        }
        return numset;
    }
    void dfs(vector<vector<int>>& g,int i,int j)
    {
        if(i>=0 && j>=0 && i<g.size() && j<g.size()&& g[i][j]==0)
        {
            g[i][j]=1; //marks as visited
            dfs(g,i-1,j);
            dfs(g,i+1,j);
            dfs(g,i,j-1);
            dfs(g,i,j+1);
        }
    }
```

960. Delete Columns to Make Sorted III.md

### Problem summary
Given a list of words same length, each word in a row. return min number of columns removed so that every row is sorted.
Apparently this is a dp problem.

### approach
The final will be the longest increasing subsequence for all rows. We must apply the longest increasing subsequence to all strings at the same position. However if the position does not satisfy any string, it is not a candidate.

### Implementation
```cpp
    int minDeletionSize(vector<string>& A) {
        //longest increasing subsequence for all strings
        int m=A.size(),n=A[0].size();
        vector<int> dp(n);
        //check each column to see if it satisfy > previous
        int maxlen=0;
        for(int i=0;i<n;i++)
        {
            for(int k=0;k<i;k++) //compare to all previous
            {
                bool sorted=1;
                for(int j=0;j<m;j++) //all string
                {
                    if(A[j][i]<A[j][k]) {sorted=0;break;} //current i,k cannot be combined   
                }
                if(sorted) dp[i]=max(dp[i],dp[k]+1);
            }
            maxlen=max(maxlen,dp[i]+1);
        }
        return n-maxlen;
    }
```

### Comment
since we initialize dp to be 0, when another column is appended, this makes it two, but we only add 1.
so in the final step we add 1.


