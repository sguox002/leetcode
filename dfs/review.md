dfs is generally recursive, reducing to similar subproblems
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

547. Friend Circles
disjoint set could be perfect for this
if M[i,j]==1 then we merge i and j

for dfs: if M(i,j)==1 then we need dfs from i and dfs from j. the two are a group. That means we shall iterate on the person instead of the edges, hence coloring on the matrix is not good. instead we shall use the person if visited.

The relation matrix is symmetric. and can we only need to use half? The answer is no, since when we found i relates with j, j could relates with people ahead of i. so We need iterate on all people.
```cpp
    int findCircleNum(vector<vector<int>>& M) {
        int num_cycle=0;
        vector<bool> visited(M.size());
        for(int i=0;i<M.size();i++) //for each person
        {
            if(!visited[i]) dfs(M,i,visited),num_cycle++;
        }
        return num_cycle;
    }
    void dfs(vector<vector<int>>& M,int node,vector<bool>& visited)
    {
        for(int i=0;i<M.size();i++) //shall start from 0 instead of from node
        {
            if(M[node][i] && !visited[i]) visited[i]=1,dfs(M,i,visited);
        }
    }
```

529. Minesweeper
given a minesweeper and a click on (i,j) return the next status
if click on M, then change to x, game over
if click on E, change to B and reveal all its adjacent empty cell. If the empty cell has mines adjacent, change to the number of mines.
adjacent is 8 directions.
dfs with 8 directions

```cpp
    void reveal(vector<vector<char>>& board, int x, int y){
         int dir[][2]={{0,1},{0,-1},{1,0},{-1,0},{1,1},{1,-1},{-1,1},{-1,-1}};
        int m=board.size(),n=board[0].size();
        if(board[x][y] == 'E')
        {
            //search 8 adjacent squares
            int count = 0;
            for(int i=0;i<8;i++)
            {
                int tx=x+dir[i][0],ty=y+dir[i][1];
                if(tx>=0 && tx<m && ty>=0 && ty<n && board[tx][ty]=='M') count++;
            }
            if(count>0) board[x][y] = '0'+count;
            else
            {
                board[x][y] = 'B';
                for(int i=0;i<8;i++)
                {
                    int tx=x+dir[i][0],ty=y+dir[i][1];
                    if(tx>=0 && tx<m && ty>=0 && ty<n) reveal(board,tx,ty);
                }
            }
        }
    }
```

947. Most Stones Removed with Same Row or Columns
stones share same row or column are  in a connected group. 
we can only remove a stone when stone has >1 stones in a group, leaving one stone at each group. 
So the max number of stones we can remove is N-num_groups
disjoint set is perfect for this.
let's try dfs way.
we can use dfs with coloring
Note: the input is the coordinates for the n stones. The coordinate could be much larger than n. So build a grid is pretty naive.
```cpp
    int removeStones(vector<vector<int>>& stones) {
        int num_groups=0;
        int n=stones.size();
        vector<bool> visited(n);
        for(int i=0;i<n;i++)
        {
            if(!visited[i]) dfs(stones,i,visited),num_groups++;
        }
        return n-num_groups;
    }
    
    void dfs(vector<vector<int>>& stones,int i,vector<bool>& visited)
    {
        visited[i]=1;
        for(int j=0;j<stones.size();j++)
        {
            if(visited[j]) continue;
            if(stones[i][0]==stones[j][0] || stones[i][1]==stones[j][1])
            {
                dfs(stones,j,visited);
            }
        }
    }
```
The above solution is much slower than using disjoint set (140ms vs 40ms). Since we use a loop to find all those nodes sharing coordinates, this could be reduced using a hashmap.

756. Pyramid Transition Matrix
It is very hard to understand the question. Let me explain:
Given a list of allowed triplets, the pyramid is built layer by layer, each triangle (upper layer node with its adjacent lower layer two nodes) must be in the allowed list. 
The question is: given the bottom layer and the allowed list, can we build the pyramid? (all the way to the top).

So the solution is:
get the possible string for upper layer and recursively solve each problem.
we can build a map with two chars with the other char. Building all kinds of combinations from lower layer can use dfs.

638. Shopping Offers
this is more like a dp or knapsack problem

337. House Robber III
dp problem

199. Binary Tree Right Side View
bfs the last one is the anwer

851. Loud and Rich
N people
richer: richer(x,y) means x is richer than y, only a subset is provided.
quite[x] is the quiteness of person x
find the person with money >= person x and is the loudest. 
for n people, that is n problems.
example: [[1,0],[2,1],[3,1],[3,7],[4,3],[5,3],[6,3]], quiet = [3,2,5,4,6,1,7,0]
for person 0:
who is richer than him? 1
who is richer than 1? 2,3
who is richer than 2,3? 4,5,6
7 is less rich than 3 but not sure if 7 is richer than 1, so the anwer is 5 (who is loudest)

just build a map for each person with a list of richer person. and do dfs.
```cpp
    unordered_map<int, vector<int>> richer2;
    vector<int> res;
    vector<int> loudAndRich(vector<vector<int>>& richer, vector<int>& quiet) {
        for (auto v : richer) richer2[v[1]].push_back(v[0]);
        res = vector<int> (quiet.size(), -1);
        for (int i = 0; i < quiet.size(); i++) dfs(i, quiet);
        return res;
    }

    int dfs(int i, vector<int>& quiet) {
        if (res[i] >= 0) return res[i];
        res[i] = i;
        for (int j : richer2[i]) if (quiet[res[i]] > quiet[dfs(j, quiet)]) res[i] = res[j];
        return res[i];
    }
```

494. Target Sum
with + and - for each number, find number of ways to get target sum S.
can be a dp or recursion with memoization
Note the S shall be in the range of [-m, m] where m is the accumulate sum.
each number has +/- two selections, the complexity would be O(2^N) without the memoization to avoid repeatedly solving overlapped subproblems.

```cpp
    int findTargetSumWays(vector<int>& nums, int S) {
        int n=nums.size();
        int m=accumulate(nums.begin(),nums.end(),0);
        if(m<abs(S)) return 0;
        vector<unordered_map<int,int>> status(n);
        return helper(nums,S,0,status,m);
    }
    int helper(vector<int>& nums,int target,int cur_pos,vector<unordered_map<int,int>>& status,int offset)
    {
        if(cur_pos==nums.size()) return target?0:1;
        //(status[cur_pos][target+offs]>=0) return status[cur_pos][target+offset];
        if(status[cur_pos].count(target+offset)) return status[cur_pos][target+offset];
        int res=0;
        res+=helper(nums,target-nums[cur_pos],cur_pos+1,status,offset);//it overflows target+/-num
        res+=helper(nums,target+nums[cur_pos],cur_pos+1,status,offset);
        status[cur_pos][target+offset]=res;
        return res;
    }
```    

863. All Nodes Distance K in Binary Tree
given a node find all nodes in distance K
One method: build a graph and bfs. To build the graph, the tree parent needs to be passed in recursive traversal.

394. Decode String
stack, recursion problem.
The subproblem is:
the first non-repeated string
the repeat number [
the to be repeated string (subproblem since it may contain [] again)
]

This effectively avoid the problem to do it first in the stack and expand the string which make the problem really complicated.
```cpp
    string decodeString(const string& s, int& i) {
        string res;
        while (i < s.length() && s[i] != ']') 
        {
            if (!isdigit(s[i])) res += s[i++];
            else 
            {
                int n = 0;
                while (i < s.length() && isdigit(s[i])) n = n * 10 + s[i++] - '0';
                i++; // '['
                string t = decodeString(s, i);
                i++; // ']'
                while (n-- > 0) res += t;
            }
        }
        return res;
    }
```

934. Shortest Bridge
two islands, the shortest bridge (4 direction connection)
dfs to two parties and calculate the mutual distance and get the min
note the distance is not sqrt distance

802. Find Eventual Safe States
given a directed graph, starting from some node if we can reach a terminal node (not a cycle)
the node is called safe node, return all the safe nodes.
so: a node start, it must goes to a terminal for all paths. If any path fails (cycle) it is not a safe node.
if the path contains any non-safe node, it is not safe either.

visited can also use coloring, they are equivalent, but coloring is more efficient.
```cpp
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        //dfs to see if it can walk to a terminal (all paths)
        vector<int> ans;
        vector<int> safenodes(graph.size(),-1);
        for(int i=0;i<graph.size();i++)
        {
            unordered_set<int> visited;
            if(isSafeNode(graph,i,visited,safenodes)) ans.push_back(i);
        }
        return ans;
    }
    
    bool isSafeNode(vector<vector<int>>& graph,int start,unordered_set<int> visited,vector<int>& safenodes)
    {
        //all paths reaches to terminal. If one case fail, then fail
        //failed case: we had a cycle
        //we need add memoization 
        if(safenodes[start]>-1) return safenodes[start];
        if(graph[start].size()==0) {return 1;} //cannot mark it as safe if only one path is safe
        if(visited.count(start)) {return 0;} //
        for(int i=0;i<graph[start].size();i++)
        {
            visited.insert(start);
            if(!isSafeNode(graph,graph[start][i],visited,safenodes)) 
            {
                //all visited nodes shall mark as not safe
                safenodes[start]=0;
                return 0;
            }
            visited.erase(start);
        }
        safenodes[start]=1;
        return 1;
    }
```

785. Is Graph Bipartite?
edge only occurs between different parties but not inside.
two coloring: a->b we color a as 0 and b as 1, then b->c we color c as 1... if we found no conflicts it is two parites.

0: Haven't been colored yet.
1: Blue.
-1: Red.
For each node,

If it hasn't been colored, use a color to color it. Then use the other color to color all its adjacent nodes (DFS).
If it has been colored, check if the current color is the same as the color that is going to be used to color it.
```cpp
    bool isBipartite(vector<vector<int>>& graph) {
        //use coloring 0: not color, 1, -1
        int n=graph.size(); //num of nodes
        vector<int> color(n); 
        for(int i=0;i<n;i++)
        {
            if(!color[i] && !dfs(graph,i,color,1)) return 0;
        }
        return 1;
    }
    bool dfs(vector<vector<int>>& graph,int i,vector<int>& color,int nc)
    {
        if(color[i]) return color[i]==nc;
        color[i]=nc;
        for(int j=0;j<graph[i].size();j++)
        {
            if(!dfs(graph,graph[i][j],color,-nc)) return 0; //adjacent reverse color
        }
        return 1;
    }
```

491. Increasing Subsequences
typical backtracking problem

```cpp
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> myv;
        helper(nums,0,ans,myv);//no need loop here, often made mistakes here

        return ans;
    }
    
    void helper(vector<int>& nums,int ind,vector<vector<int>>& res,vector<int>& v)
    {
        if(v.size()>1) res.push_back(v);
        unordered_set<int> visited;
        for(int i=ind;i<nums.size();i++)
        {
            if((!v.size() || nums[i]>=v.back()) && !visited.count(nums[i]))
            {
                v.push_back(nums[i]);
                helper(nums,i+1,res,v);//i+1 instead ind+1
                v.pop_back();
                visited.insert(nums[i]);
            }
        }
    }
```
use reference to push and pop the last element and try new path

129. Sum Root to Leaf Numbers
each path represents a number and add all path
backtracking
```cpp
    void sumtree(TreeNode* root,long long num,long long & sum)
    {
        //root is valid
        num=num*10+root->val;
        if(root->left) sumtree(root->left,num,sum);//if root is leaf will call twice
        if(root->right) sumtree(root->right,num,sum);
        if(!root->left && !root->right) {sum+=num;return;}
    }
```
using value passing so no need to push/pop

114. Flatten Binary Tree to Linked List
arrange the tree in preorder
inplace: we can do different order.
we have two subproblem, flatten the left and flatten the right and then connect using the root as root->left->right
The linked list generally uses the reverse sequence right->left->root
ie. we first flatten the right subtree, then left subtree, finally the root, root->right connects the right first and now the root becomes the top node. The top node is null first.
```cpp
    void flatten(TreeNode* root) {
        TreeNode* prev=0;
        helper(root,prev);
    }
    void helper(TreeNode* root, TreeNode*& prev)
    {
        if(!root) return;
        helper(root->right,prev);
        helper(root->left,prev);
        root->right=prev;
        root->left=0;
        prev=root;
    }
```
we have to use reference passing since the change of prev shall carry to the next step.

886. Possible Bipartition
disliked person cannot be in the same group
use coloring. three state 0: not assigned, 1/-1
build the input to ajacent matrix.

```cpp
    bool possibleBipartition(int N, vector<vector<int>>& dislikes) {
        vector<int> color(N);
        vector<vector<int>> adj(N);
        for(int i=0;i<dislikes.size();i++)
        {
            adj[dislikes[i][0]-1].push_back(dislikes[i][1]-1);
            adj[dislikes[i][1]-1].push_back(dislikes[i][0]-1);
        }
        for(int i=0;i<N;i++)
        {
            if(!color[i] && !dfs(adj,i,color,1)) return 0;
        }
        return 1;
    }
    bool dfs(vector<vector<int>>& adj,int i,vector<int>& color,int nc)
    {
        if(color[i]) return nc==color[i];
        color[i]=nc;
        for(int j=0;j<adj[i].size();j++) //adj[i] could be empty
        {
            int node=adj[i][j];
            if(color[node] && color[node]!=-nc) return 0;
            if(!color[node] && !dfs(adj,node,color,-nc)) return 0;
        }
        return 1;
    }
```
Note: the precheck color[node]!=-nc is critical: since the following condition only checks non-set colors. if already set, we need check if it is the color we expected. otherwise it is 0. If it is same color, we need continue checking (cannot return)

200. Number of Islands
disjoint set also ok
4 direction. use coloring and dfs
```cpp
    int numIslands(vector<vector<char>>& grid) {
        int ans=0;
        for(int i=0;i<grid.size();i++)
        {
            for(int j=0;j<grid[0].size();j++)
            {
                if(grid[i][j]=='1') dfs(grid,i,j),ans++;
            }
        }
        return ans;
    }
    void dfs(vector<vector<char>>& grid,int i,int j)
    {
        if(i<0 ||j<0 ||i>=grid.size()||j>=grid[0].size()) return;
        if(grid[i][j]=='1')
        {
            grid[i][j]='2';
            dfs(grid,i-1,j);
            dfs(grid,i+1,j);
            dfs(grid,i,j-1);
            dfs(grid,i,j+1);
        }
    }
```





    






































