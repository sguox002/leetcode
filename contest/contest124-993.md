## contest 124

993. Cousins in Binary Tree.md

### Problem 
In a binary tree, the root node is at depth 0, and children of each depth k node are at depth k+1.

Two nodes of a binary tree are cousins if they have the same depth, but have different parents.

We are given the root of a binary tree with unique values, and the values x and y of two different nodes in the tree.

Return true if and only if the nodes corresponding to the values x and y are cousins.

### Approach
traveral and record its depth and parent

```cpp
    bool isCousins(TreeNode* root, int x, int y) {
        TreeNode *px=0,*py=0;
        int hx=0,hy=0;
        dfs(NULL,root,x,y,px,py,hx,hy,0);
        return (px && py&& hx && hy) && px!=py && hx==hy;
    }
    void dfs(TreeNode* parent,TreeNode* root,int x,int y,TreeNode*& px,TreeNode*& py,int &hx,int &hy,int depth)
    {
        if(!root) return;
        dfs(root,root->left,x,y,px,py,hx,hy,depth+1);
        if(root->val==x) {hx=depth;px=parent;}
        if(root->val==y) {hy=depth;py=parent;}
        dfs(root,root->right,x,y,px,py,hx,hy,depth+1);
    }
```


994. Rotting Oranges.md

### Problem
In a given grid, each cell can have one of three values:

the value 0 representing an empty cell;
the value 1 representing a fresh orange;
the value 2 representing a rotten orange.
Every minute, any fresh orange that is adjacent (4-directionally) to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange.  If this is impossible, return -1 instead.

### Solution
- record each round all those rotten orange
- each round rot the surrounding orange
- each round shall have less fresh orange

```cpp
    int orangesRotting(vector<vector<int>>& grid) {
        //bfs to rotten the surrounding oranages
        int m=grid.size(),n=grid[0].size();
        int mins=0;
        int num_fresh=0;
        for(int i=0;i<m;i++)
            for(int j=0;j<n;j++) if(grid[i][j]==1) num_fresh++;
        while(num_fresh)
        {
            int prev=num_fresh;
            vector<vector<int>> ind;
            for(int i=0;i<m;i++)
            {
                for(int j=0;j<n;j++)
                {
                    if(grid[i][j]==2) ind.push_back({i,j});//{bfs(grid,i,j,num_fresh);}
                }
            }
            for(int k=0;k<ind.size();k++) bfs(grid,ind[k][0],ind[k][1],num_fresh);
            if(num_fresh==prev) return -1;
            mins++;
        }
        return mins;
    }
        
    void bfs(vector<vector<int>>& grid,int i,int j,int& num_fresh)
    {
        int m=grid.size(),n=grid[0].size();
        grid[i][j]=0;
        if(i-1>=0 && i-1<m && grid[i-1][j]==1) {grid[i-1][j]=2;num_fresh--;}
        if(i+1<m && grid[i+1][j]==1) {grid[i+1][j]=2;num_fresh--;}
        if(j-1>=0 && j-1<n && grid[i][j-1]==1) {grid[i][j-1]=2;num_fresh--;}
        if(j+1<n && grid[i][j+1]==1) {grid[i][j+1]=2;num_fresh--;}
    }
```

995. Minimum Number of K Consecutive Bit Flips.md

### Problem
In an array A containing only 0s and 1s, a K-bit flip consists of choosing a (contiguous) subarray of length K and simultaneously changing every 0 in the subarray to 1, and every 1 in the subarray to 0.

Return the minimum number of K-bit flips required so that there is no 0 in the array.  If it is not possible, return -1.

### Approach
this is a greedy solution
flip the zeros from left to right

```cpp
    int minKBitFlips(vector<int>& A, int K) {
        //the leading bit shall be flipped and reduce to sub problem
        int minflips=0;
        
        for(int i=0;i<A.size()-K+1;i++)
        {
            if(A[i]==0) {if(minkflip(A,K,i)) minflips++;else return -1;}
        }
        int sum=accumulate(A.begin(),A.end(),0);
        if(sum!=A.size()) return -1;
        return minflips;
    }
    int minkflip(vector<int>& A,int K,int ind)
    {
        int m=A.size();
        for(int i=ind;i<ind+K;i++) A[i]^=1;
        if(m-ind==K) //last flip
        {
            int sum=accumulate(A.begin()+ind,A.end(),0);
            return sum==K;
        }
        
        return 1;
    }
```

996. Number of Squareful Arrays.md

Given an array A of non-negative integers, the array is squareful if for every pair of adjacent elements, their sum is a perfect square.

Return the number of permutations of A that are squareful.  Two permutations A1 and A2 differ if and only if there is some index i such that A1[i] != A2[i].

### Approach
Direct using permutation will yield O(n!) complexity and is to get TLE
We can build a graph and using dfs (backtracking) to solve this problem

```cpp
    unordered_map<int, int> count;
    unordered_map<int, unordered_set<int>> cand;
    int res = 0;
    int numSquarefulPerms(vector<int>& A) {
        for (int &a : A) count[a]++;
        for (auto &i : count) {
            for (auto &j : count) {
                int x = i.first, y = j.first, s = sqrt(x + y);
                if (s * s == x + y)
                    cand[x].insert(y);
            }
        }
        for (auto e : count)
            dfs(e.first, A.size() - 1);
        return res;
    }

    void dfs(int x, int left) {
        count[x]--;
        if (!left) res++;
        for (int y : cand[x])
            if (count[y] > 0)
                dfs(y, left - 1);
        count[x]++;
    }
```

cand: the hashmap for the connection graph
count: the hashmap to record duplicate items and also used for record if the item is used or not
count[x]++ -- is for the backtracking. 

