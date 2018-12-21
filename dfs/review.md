```dfs is generally recursive, reducing to similar subproblems
generally involves push and pop a new path
generally uses coloring to indicate visited nodes
dfs/bfs/disjoint set sometimes can be all used for similar problems

559. Maximum Depth of N-ary Tree
max depth is 1+maxdepth(children)
bfs is even faster

872. Leaf-Similar Trees
dfs all tree first left and then right, combined to a string or an array

897. Increasing Order Search Tree
inorder traversal of the tree and arrange the nodes in a linked list.
naive solution is trivial
inplace is more meaningful
connection is left->root->right. we need the last node to connect.
first subproblem: the left connects the root as the last node, modify the root's left=NULL. and its right shall be the subproblem for the right subtree.
it returns the new root, 
```cpp
    TreeNode* increasingBST(TreeNode* root, TreeNode* tail = NULL) {
        if (!root) return tail;
        TreeNode* res = increasingBST(root->left, root);
        root->left = NULL;
        root->right = increasingBST(root->right, tail);
        return res;
    }
```

104. Maximum Depth of Binary Tree
same as 559, but we only have left and right child

690. Employee Importance
employee's importance=itself's importance+all its subordinate's importance.
this is a n-tary tree with its sum.
first build a tree map, and then use recursion:
importance=self+sum(child's importance), child's importance is a subproblem

100. Same Tree
check left sub and right sub equal and root equal

733. Flood Fill
typical dfs
```cpp
    void dfs(vector<vector<int>>& image,int sr,int sc,int color,int newColor)
    {
        int m=image.size(),n=image[0].size();
        if(sr<0 || sc<0 || sr>=m || sc>=n) return;
        if(image[sr][sc]==color)
        {
            image[sr][sc]=newColor;
            dfs(image,sr-1,sc,color,newColor);
            dfs(image,sr+1,sc,color,newColor);
            dfs(image,sr,sc-1,color,newColor);
            dfs(image,sr,sc+1,color,newColor);
        }
    }
```
Note: need first change the color before processing the subproblem, otherwise, it will go into infinite loop

108. Convert Sorted Array to Binary Search Tree
convert the sorted array into balanced binary tree
choose the mid number as the root, then solve left and right subtree recursively

257. Binary Tree Paths
output the path in a string
classical dfs: when get a node, add into the string, dfs the left, pop out the node, dfs the right
```cpp
    void dfs(TreeNode* root, string path, vector<string>& vs)
    {
        if(!root) return;
        if(!root->left && !root->right) 
        {
            path+=to_string(root->val);
            vs.push_back(path);
            return;
        }
        path+=to_string(root->val)+"->";
        dfs(root->left,path,vs);
        dfs(root->right,path,vs);
    }
```
Note:
- need process the leaf node to add the answer (cannot use null node for this since it will give wrong path)
- use value passing for path, so we do not need to push and pop the string

101. Symmetric Tree
recursively check whether l->left equals r->right and l->right equals r->left.

110. Balanced Binary Tree
recursively check if the left sub height vs right sub height difference is <=1

743. Network Delay Time
times[i] = (u, v, w), from node u to node v, takes travel time w
from node K, what is the time required for all nodes to receive it.
max time of all min time from K to other nodes.
this could be a dp, dijkstra, bellman-ford problem. Trying to relax edges from k to all other nodes.
dfs: from k to all other node, calculate the min path sum.
dijkstra: using priority_queue or multiset (min heap) to try the smallest first to relax the edge
```cpp
    typedef pair<int,int> pii;
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<pii> g[n + 1];
        for (const auto& t : times) g[t[0]].push_back(make_pair(t[1], t[2]));
        const int inf = 1e9;
        vector<int> dist(n + 1, inf);
        dist[k] = 0;//k to k
        priority_queue<pii, vector<pii>, greater<pii> > pq;
        pq.push(make_pair(0, k));//distance, destination
        int u, v, w;
		vector<bool> visited(n + 1);
        while (!pq.empty()) 
        {
            pii p = pq.top(); pq.pop();
            u = p.second;
			if (visited[u]) continue;
            for (auto& to : g[u]) 
            {
                v = to.first, w = to.second;
                if (dist[v] > dist[u] + w) 
                {
                    dist[v] = dist[u] + w;
                    pq.push(make_pair(dist[v], v));
                }
            }
			visited[u] = 1;
        }
        int ans = *max_element(dist.begin() + 1, dist.end());
        return ans == inf ? -1 : ans;
    }
```
the dijkstra:
- form the graph
- maintain a min distance from k to all other nodes (from k to k is 0)
- priority-queue: using the distance for compare, min heap (try smallest distance first)
- use the other nodes to relax the edge and get the min dist.
- when the edge can be relaxed, add the updated distance back to priority-queue
- stop when no more distance can be relaxed.
- this is not an easy problem.

112. Path Sum
check if the tree has a root to leaf path sum=target
dfs or recursive. reduce to sub problem with target-root on left sub or right sub

111. Minimum Depth of Binary Tree
similar for max depth 1+min(left height,right height)

959. Regions Cut By Slashes
upsample and do dfs
need at least upsample to 3x3, otherwise connected region will be separated.

841. Keys and Rooms
N rooms, and each room has a list of keys to open other rooms
check if we can open all rooms,initially at room 0
that is dfs, we need check all possible keys and see if all rooms are visited.
bfs is also ok. 
use set or other mechanism to record visited.

513. Find Bottom Left Tree Value
Given a binary tree, find the leftmost value in the last row of the tree
if right height is bigger, find in right subtree
else find in left subtree

515. Find Largest Value in Each Tree Row
can use hash map for each layer and get the max using traversal (any order)

695. Max Area of Island
dfs and count the number and mark it as different color
dfs is much simpler than bfs.
The following code is used for finding the area of one connected region

```cpp
    int dfs(vector<vector<int>>& grid,int i,int j)
    {
        int m=grid.size(),n=grid[0].size();
        if(i<0 || j<0 || i>=m || j>=n) return 0;
        if(grid[i][j]!=1) return 0;
        grid[i][j]=2;
       return dfs(grid,i-1,j)+dfs(grid,i+1,j)+dfs(grid,i,j-1)+dfs(grid,i,j+1)+1;
    }
```    





















