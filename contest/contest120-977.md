## contest 120

977. Squares of a Sorted Array.md

### Problem Summary
given a list of sorted array, return sorted of its square

### approach
When squared, the negative could be larger
using two pointer from two ends and compare the square will get O(N) complexity.


978. Longest Turbulent Subarray.md

### Problem Summary
the subarray is turbulent if the comparison sign flips between each adjacent pair of elements in the subarray.
Return the length of a maximum size turbulent subarray of A.

### Approach
direct approach: count the +/- length

### Code
```cpp
    int maxTurbulenceSize(vector<int>& A) {
        int ans=1;
        int prev=0,curr=0,cnt=0;
        for(int i=1;i<A.size();i++)
        {
            if(A[i]>A[i-1]) curr=1;
            else if(A[i]<A[i-1]) curr=-1;

            else {curr=0;}
            if(prev && prev==-curr) cnt++;
            else {ans=max(ans,cnt+2);cnt=0;}
            prev=curr;
        }
        if(cnt) ans=max(ans,cnt+2);
        return ans;
    }
```

Note:
do not forget when the +/- ends at the last.

979. Distribute coins in binary tree

### Problem Summary
A tree with N nodes and N total coins, the objective is to distribute the coins to 1 at each node
each step you can pass a coin to parent or its direct child.

### Approach
For a node we shall calculate its left subtree and right subtree balance. (sum of coins - number of nodes)
the balance could be negative or positive. If negative, we shall pass in the balance from the root. If positive we need pass out balance to its parent, i.e the abs(coins)
each coin from root or to root need one step.
So, we can use postorder traversal to get the balance and add up the absolute values.

### code
```cpp
    int distributeCoins(TreeNode* root) {
        int steps=0;
        postorder(root,steps);
        return steps;
    }
    int postorder(TreeNode* root,int& steps)
    {
        if(!root) return 0;
        int coins=postorder(root->left,steps)+postorder(root->right,steps);
        coins+=root->val-1;
        steps+=abs(coins);//coins need to past to parent
        return coins;
    }
```

### comments
- get the balance of the subtree need not two dfs, but one is fine, to minus 1 from each node
- the steps, regardless the direction, count one step.


980. Unique Paths III.md

### Problem Summary
given a matrix, 1 is start, 2 is end, 0 is empty, -1 is obstacle.
Find the number of path: the path covers each empty cell once
4 directions

### Approach
Typical dfs problem with more constraints
-. need find the start and end, brutal force
-. need visit the cell only once, when visited, mark it as obstacle

### code
```cpp
    int uniquePathsIII(vector<vector<int>>& grid) {
        int m=grid.size(),n=grid[0].size();
        int nsteps=0,sx=0,sy=0;
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]==1) sx=i,sy=j;
                if(grid[i][j]!=-1) nsteps++;
            }
        }
        return dfs(grid,sx,sy,1,nsteps);
    }
    int dfs(vector<vector<int>>& grid,int sx,int sy,int step,int nsteps)
    {
        int m=grid.size(),n=grid[0].size();
        if(sx<0 || sx>=m || sy<0 || sy>=n || grid[sx][sy]==-1) return 0;
        if(grid[sx][sy]==2) return step==nsteps?1:0;
        grid[sx][sy]=-1; //mark it visited
        int ans=dfs(grid,sx+1,sy,step+1,nsteps);
        ans+=dfs(grid,sx-1,sy,step+1,nsteps);
        ans+=dfs(grid,sx,sy+1,step+1,nsteps);
        ans+=dfs(grid,sx,sy-1,step+1,nsteps);
        grid[sx][sy]=0;
        return ans;
    }
```

### comments
- we are counting non-obstacle cells, so need starting from 1
- visited then we mark it as different color, this tech is often used in dfs.



