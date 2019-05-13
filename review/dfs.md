# Chapter 12 dfs
dfs is generally recursive, reducing to similar subproblems
generally involves push and pop a new path
generally uses coloring to indicate visited nodes
dfs/bfs/disjoint set sometimes can be all used for similar problems

some tree problems are not discussed here in detail.

### 559. Maximum Depth of N-ary Tree
max depth is 1+maxdepth(children)
bfs is even faster
see tree

### 872. Leaf-Similar Trees
dfs all tree first left and then right, combined to a string or an array
see tree

### 897. Increasing Order Search Tree
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
or use a prev pointer to connect

### 104. Maximum Depth of Binary Tree
same as 559, but we only have left and right child

	
### 100. Same Tree
check left sub and right sub equal and root equal

### 108. Convert Sorted Array to Binary Search Tree
convert the sorted array into balanced binary tree
choose the mid number as the root, then solve left and right subtree recursively
see tree

### 257. Binary Tree Paths
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

### 101. Symmetric Tree
recursively check whether l->left equals r->right and l->right equals r->left.

### 110. Balanced Binary Tree
recursively check if the left sub height vs right sub height difference is <=1

### 112. Path Sum
check if the tree has a root to leaf path sum=target
dfs or recursive. reduce to sub problem with target-root on left sub or right sub

### 111. Minimum Depth of Binary Tree
similar for max depth 1+min(left height,right height)

### 513. Find Bottom Left Tree Value
Given a binary tree, find the leftmost value in the last row of the tree
if right height is bigger, find in right subtree
else find in left subtree

### 515. Find Largest Value in Each Tree Row
can use hash map for each layer and get the max using traversal (any order)

### 199. Binary Tree Right Side View
bfs the last one is the anwer

### 129. Sum Root to Leaf Numbers
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

### 114. Flatten Binary Tree to Linked List
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

### 863. All Nodes Distance K in Binary Tree
given a node find all nodes in distance K
One method: build a graph and bfs. To build the graph, the tree parent needs to be passed in recursive traversal.

### 98. Validate Binary Search Tree
straightforward: in order traversal and see if they are sorted. Or we do not need extra space for this.
must be surrounded by x, boundary o does not count.
union find, by connecting all boundary cells to a parent.
dfs: we only need to find those regions attached to the 4 boundaries and they shall not change. other nodes all change to x

### 979. Distribute coins in binary tree
### 1026 Max difference between node and ancestor

## dfs or recursive

### 690. Employee Importance
employee's importance=itself's importance+all its subordinate's importance.
this is a n-tary tree with its sum.
first build a tree map, and then use recursion:
importance=self+sum(child's importance), child's importance is a subproblem
```cpp
    int getImportance(vector<Employee*> employees, int id) {
        //this is a tree structure
        //find the employee and calculate its importance
        //to speed up search we need a hash table
        unordered_map<int,Employee*> mp;
        for(int i=0;i<employees.size();i++) mp[employees[i]->id]=employees[i];
        int importance=0;
        if(mp.count(id))
        {
            Employee* p=mp[id];
            importance=calcImportance(p,mp);
        }
        return importance;
    }
    int calcImportance(Employee* p,unordered_map<int,Employee*>& mp)
    {
        int ans=p->importance;
        
        for(int i=0;i<p->subordinates.size();i++)
        {
            int id=p->subordinates[i];
            ans+=calcImportance(mp[id],mp);    
        }
        return ans;
    }
```
more like a  recursive.

### 733. Flood Fill
typical dfs
```cpp
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        if(newColor==image[sr][sc]) return image;
        dfs(image,sr,sc,image[sr][sc],newColor);
        return image;
    }
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
same color will cause infinite loop

### 743. Network Delay Time
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

### 959. Regions Cut By Slashes  ***
upsample and do dfs
need at least upsample to 3x3, otherwise connected region will be separated.
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
Note: 1. we are not couting slashes but regions, 2. regular dfs
	
### 841. Keys and Rooms
N rooms, and each room has a list of keys to open other rooms
check if we can open all rooms,initially at room 0
that is dfs, we need check all possible keys and see if all rooms are visited.
bfs is also ok. 
use set or other mechanism to record visited.
```cpp
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        unordered_set<int> visited;
        queue<int> to_visit;
        to_visit.push(0);
        while(!to_visit.empty()) {
            int curr = to_visit.front();
            to_visit.pop();
            visited.insert(curr);
            for (int k : rooms[curr]) 
                if (visited.find(k) == visited.end()) to_visit.push(k);
        }
        return visited.size() == rooms.size();
    }
```
dfs version:
```cpp
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        int n=rooms.size();
        unordered_set<int> tovisit(n);
        for(int i=0;i<n;i++) tovisit.insert(i);
        dfs(rooms,0,tovisit);
        return tovisit.empty();
    }
    void dfs(vector<vector<int>>& rooms,int node,unordered_set<int>& tovisit)
    {
        if(tovisit.count(node))
        {
            tovisit.erase(node);
            for(int i=0;i<rooms[node].size();i++)
                dfs(rooms,rooms[node][i],tovisit);
        }
    }
```	
### 695. Max Area of Island
dfs and count the number and mark it as different color
dfs is much simpler than bfs.
The following code is used for finding the area of one connected region

```cpp
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int m=grid.size(),n=grid[0].size();
        int maxarea=0;
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]==1) maxarea=max(maxarea,dfs(grid,i,j));
            }
        }
        return maxarea;
    }
    int dfs(vector<vector<int>>& grid,int i,int j)
    {
        int m=grid.size(),n=grid[0].size();
        if(i<0 || j<0 || i>=m || j>=n) return 0;
        if(grid[i][j]!=1) return 0;
        grid[i][j]=2;
       return dfs(grid,i-1,j)+dfs(grid,i+1,j)+dfs(grid,i,j-1)+dfs(grid,i,j+1)+1;
    }
```    

### 547. Friend Circles
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
dfs version
```cpp
    int findCircleNum(vector<vector<int>>& M) {
        //if (i,j) then visit ith row and jth row
        //only use the upper triangle
        int n=M.size();
        int ans=0;
        for(int i=0;i<n;i++)
        {
            for(int j=i;j<n;j++)
                if(M[i][j]) {dfs(M,i,j),ans++;}
        }
        return ans;
    }
    void dfs(vector<vector<int>>& M,int i,int j)
    {
        if(i<0 || j<0 ||i>=M.size() || j>=M.size() || M[i][j]==0) return;
        M[i][j]=0;
        //cout<<i<<" "<<j<<endl;
        for(int k=i;k<M.size();k++) 
            if(M[i][k]) dfs(M,i,k);
        for(int k=0;k<=j;k++)
            if(M[k][j]) dfs(M,k,j);
    }
```
note: row and col shall include the diagonal element.

### 529. Minesweeper
given a minesweeper and a click on (i,j) return the next status
if click on M, then change to x, game over
if click on E, change to B and reveal all its adjacent empty cell. If the empty cell has mines adjacent, change to the number of mines.
adjacent is 8 directions.
dfs with 8 directions

This is a typical Search problem, either by using DFS or BFS. Search rules:

If click on a mine ('M'), mark it as 'X', stop further search.
If click on an empty cell ('E'), depends on how many surrounding mine:
2.1 Has surrounding mine(s), mark it with number of surrounding mine(s), stop further search.
2.2 No surrounding mine, mark it as 'B', continue search its 8 neighbors.

```cpp
     vector<vector<char>> updateBoard(vector<vector<char>>& board, vector<int>& click) {
        if(board[click[0]][click[1]]=='M') 
        {
            board[click[0]][click[1]]='X';
            return board;
        }
        reveal(board,click[0],click[1]);
        return board;
    }
    void reveal(vector<vector<char>>& b,int i,int j)
    {
        if(i<0 || j<0 ||i>=b.size() || j>=b[0].size()) return;
        int dir[][2]={{0,1},{0,-1},{1,0},{-1,0},{1,1},{-1,-1},{1,-1},{-1,1}};
        if(b[i][j]=='E')
        {
            int cnt=0;
            for(int k=0;k<8;k++)
            {
                int x=i+dir[k][0],y=j+dir[k][1];
                if(x<0 || y<0 ||x>=b.size() || y>=b[0].size()) continue;
                if(b[x][y]=='M') cnt++;
            }
            if(cnt) b[i][j]=cnt+'0';
            else //there is no mines
            {
                b[i][j]='B';
                for(int k=0;k<8;k++) reveal(b,i+dir[k][0],j+dir[k][1]);
            }
        }
    }
```

### 947. Most Stones Removed with Same Row or Columns
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

### 1020. Number of enclaves
count inland lands
The first cycle does DFS for the boundary cells. The second cycle counts the remaining land.
```cpp
    int numEnclaves(vector<vector<int>>& A) {
        int m=A.size(),n=A[0].size();
        for(int i=0;i<m;i++){
            if(A[i][0]) dfs(A,i,0);
            if(A[i][n-1]) dfs(A,i,n-1);
        }
        for(int j=0;j<n;j++){
            if(A[0][j]) dfs(A,0,j);
            if(A[m-1][j]) dfs(A,m-1,j);
        }
        int ans=0;
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
                if(A[i][j]) ans++;
        }
        return ans;
    }
    void dfs(vector<vector<int>>& A,int i,int j)
    {
        if(i<0 || j<0 ||i>=A.size() || j>=A[0].size() || A[i][j]==0) return;
        A[i][j]=0;
        dfs(A,i-1,j);
        dfs(A,i+1,j);
        dfs(A,i,j-1);
        dfs(A,i,j+1);
    }
```

### 756. Pyramid Transition Matrix
It is very hard to understand the question. Let me explain:
Given a list of allowed triplets, the pyramid is built layer by layer, each triangle (upper layer node with its adjacent lower layer two nodes) must be in the allowed list. 
The question is: given the bottom layer and the allowed list, can we build the pyramid? (all the way to the top).

So the solution is:
get the possible string for upper layer and recursively solve each problem.
we can build a map with two chars with the other char. Building all kinds of combinations from lower layer can use dfs.
```cpp
    bool pyramidTransition(string bottom, vector<string>& allowed) {
        //first build the relation
        unordered_map<string,string> allow;
        for(string s: allowed) allow[s.substr(0,2)].push_back(s[2]);
        return helper(bottom,allow);
    }
    bool helper(string& bottom,unordered_map<string,string>& mp)
    {
        if(bottom.size()==1) return 1;
        vector<string> vs;
        for(int i=0;i<bottom.size()-1;i++)
        {
            string key=bottom.substr(i,2);
            if(mp.count(key)) vs.push_back(mp[key]);
            else return 0;
        }
        //dfs to get all allowed combinations
        unordered_set<string> combin;
        dfs(vs,combin,"");
        for(string s: combin)
            if(helper(s,mp)) return 1;
        return 0;
    }
    void dfs(vector<string>& vs,unordered_set<string>& res,string s)
    {
        if(s.size()==vs.size()) {res.insert(s);return;}
        int ind=s.size();
        for(char c: vs[ind])
        {
            s+=c;
            dfs(vs,res,s);
            s.pop_back();
        }
    }
```
from a bottom string, we build a combination of strings, then we try each combination to see if we can reach the top
*. when bottom cannot find a char, return 0. this is needed to avoid error
*. dfs just get all combinations, judge if the string OK waits for upper level function helper

### 638. Shopping Offers
this is more like a dp or knapsack problem
```cpp
void operator+=(vector<int> &a, const vector<int> &b) {
    for (int i = 0; i < a.size(); i++)
        a[i] += b[i];
}

void operator-=(vector<int> &a, const vector<int> &b) {
    for (int i = 0; i < a.size(); i++)
        a[i] -= b[i];
}

bool operator<(const vector<int> &a, const int &n) {
    for (int i : a)
        if (i < n)
            return true;
    return false;
}

int operator*(const vector<int> &a, const vector<int> &b) {
    int res = 0;
    for (int i = 0; i < a.size(); i++)
        res += a[i] * b[i];
    return res;
}    
class Solution {
public:
    int shoppingOffers(vector<int>& price, vector<vector<int>>& special, vector<int>& needs, int cost = 0) {
        if (needs < 0) return INT_MAX;

        int m = cost + needs * price;//buy everything separately

        for (auto &offer : special) 
        {
            if (cost + offer.back() >= m) // pruning
                continue;
            needs -= offer; //add this offer
            m = min(m, shoppingOffers(price, special, needs, cost + offer.back()));
            needs += offer; //put it back (pop)
        }

        return m;
    }
```
this is a combined condition, but if we considered it as a whole it is much simpler.

### 337. House Robber III
dp problem in binary tree

### 851. Loud and Rich
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
### 7 is less rich than 3 but not sure if 7 is richer than 1, so the anwer is 5 (who is loudest)

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

### 494. Target Sum
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

### 394. Decode String
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

### 934. Shortest Bridge
two islands, the shortest bridge (4 direction connection)
dfs to two parties and calculate the mutual distance and get the min
note the distance is not sqrt distance
```cpp
    int shortestBridge(vector<vector<int>>& A) {
        //two party problem can use paint
        //or use dfs/bfs to reach the other party
        //we can calculate the min distance between the two parties
        vector<vector<int>> partyA,partyB;
        for(int i=0;i<A.size();i++)
        {
            for(int j=0;j<A[0].size();j++)
            {
                if(A[i][j]==1) 
                {
                    if(partyA.size()==0) dfs(A,i,j,partyA);
                    else dfs(A,i,j,partyB);
                }
            }
        }
        //cout<<"OK";
        int mindist=INT_MAX;
        for(int i=0;i<partyA.size();i++)
        {
            for(int j=0;j<partyB.size();j++)
                mindist=min(mindist,abs(partyA[i][0]-partyB[j][0])+abs(partyA[i][1]-partyB[j][1]));
        }
        return mindist-1;
    }
    void dfs(vector<vector<int>>& A,int i,int j,vector<vector<int>>& pa)
    {
        //4 directions
        A[i][j]=2; 
        int n=A.size(),m=A[0].size();
        pa.push_back({i,j});
        if(i>0 && A[i-1][j]==1) dfs(A,i-1,j,pa);
        if(j>0 && A[i][j-1]==1) dfs(A,i,j-1,pa);
        if(i<n-1 && A[i+1][j]==1) dfs(A,i+1,j,pa);
        if(j<m-1 && A[i][j+1]==1) dfs(A,i,j+1,pa);
    }
```
	
### 802. Find Eventual Safe States
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

### 785. Is Graph Bipartite?
edge only occurs between different parties but not inside.
two coloring: a->b we color a as 0 and b as 1, then b->c we color c as 1... if we found no conflicts it is two parites.
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

### 491. Increasing Subsequences
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


### 886. Possible Bipartition
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

### 200. Number of Islands
disjoint set also ok
### 4 direction. use coloring and dfs
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

### 473. Matchsticks to Square
using sticks with different length to form a square
You have to use all the sticks once

first calculate the side length, which is the target sum.
dfs to get the target
```cpp
    bool dfs(vector<int>& sticks,int target,int start_ind,unordered_set<int>& visited)
    {
        if(target==0) return 1;
        if(target<0) return 0;
        for(int i=start_ind;i<sticks.size();i++)
        {
            if(visited.count(i)) continue;
            visited.insert(i);
            if(dfs(sticks,target-sticks[i],i+1,visited)) return 1;
            visited.erase(i); //put them back if failed
        }
        return 0;
    }
```
if path fail, we need pop back the node visited.

### 1034 coloring a border
```cpp
    vector<vector<int>> colorBorder(vector<vector<int>>& grid, int r0, int c0, int color) {
        int oc=grid[r0][c0];
        dfs(grid,r0,c0,oc,color);
        for(int i=0;i<grid.size();i++)
            for(int j=0;j<grid[0].size();j++)
                if(grid[i][j]<0) grid[i][j]=color;
        return grid;
    }
    void dfs(vector<vector<int>>& grid,int r,int c,int oc,int nc)
    {
        if(r<0 ||c<0 ||r>=grid.size() || c>=grid[0].size() || grid[r][c]!=oc) return;
        grid[r][c]=-oc;
        dfs(grid,r+1,c,oc,nc);
        dfs(grid,r-1,c,oc,nc);
        dfs(grid,r,c+1,oc,nc);
        dfs(grid,r,c-1,oc,nc);
        if(r && c && r<grid.size()-1 && c<grid[0].size()-1)
        {
            if((abs(grid[r-1][c])==oc) && 
               (abs(grid[r+1][c])==oc) && 
               (abs(grid[r][c+1])==oc) && 
               (abs(grid[r][c-1])==oc))
                grid[r][c]=oc;
        }
    }
```

### 430. Flatten a Multilevel Doubly Linked List
```cpp
    Node* flatten(Node* head)
    {
        Node* last=0;
        return helper(head,last);
    }
    Node* helper(Node* head,Node*& last) {
        if(!head) return 0;
        Node* node=head;
        while(node)
        {
            //cout<<node->val<<endl;
            if(node->child)
            {
                Node* next=node->next;//save the next
                node->next=helper(node->child,last);
                node->next->prev=node;
                node->child=0;
                last->next=next;
                if(next) next->prev=last;
                node=last;
            }
            if(!node->next) last=node;
            node=node->next;
            
        }
        return head;
    }
```
	
### 542. 01 Matrix
find the nearest distance to 0 for each cell
bfs for each non zero cell.
```cpp
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
      //first scan to update all distance=1
        //use a queue to keep all not solved elements
        int m=matrix.size(),n=matrix[0].size();
        vector<vector<int>> ans(m,vector<int>(n));
        
        queue<int> q;
        for(int i=0;i<m;i++)
            for(int j=0;j<n;j++) if(matrix[i][j]) ans[i][j]=update_closest(matrix,i,j,q);
        //print(matrix);
        int dir[][2]={{0,1},{0,-1},{1,0},{-1,0}};
        //note process 1 first, 2 second,3,4,5....., otherwise there would have a problem depending on the update sequence
        int level=1;
        while(!q.empty()) //process the element in q: if there is an element is >0 around it, use the min()+1
        {
            int sz=q.size();
            for(int tt=0;tt<sz;tt++)
            {
                int t=q.front();q.pop();
                int i=t/n,j=t%n;
                int min_val=INT_MAX;
                for(int t=0;t<4;t++)
                {
                    int x=i+dir[t][0],y=j+dir[t][1];
                    if(x>=0 && x<m && y>=0 && y<n && ans[x][y]>0) min_val=min(min_val,ans[x][y]);
                }
                if(min_val==level) ans[i][j]=min_val+1;
                else {q.push(i*n+j);}
            }
            level++;
            //cout<<i<<" "<<j<<":"<<ans[i][j]<<endl;
            //cout<<"updated matrix:"<<endl;print(ans);
        }

        return ans;
    }
    
    int update_closest(vector<vector<int>>& matrix,int i,int j,queue<int>& q)
    {
        int m=matrix.size(),n=matrix[0].size();
        int dir[][2]={{0,1},{0,-1},{1,0},{-1,0}};
        for(int t=0;t<4;t++)
        {
            int x=i+dir[t][0],y=j+dir[t][1];
            if(x>=0 && x<m && y>=0 && y<n && !matrix[x][y]) return 1;
        }
        q.push(i*n+j);
        return -1;
    }
```
	

### 576. Out of Boundary Paths
recursive or dp
```cpp
    int findPaths(int m, int n, int N, int i0, int j0) {
        vector<vector<vector<uint>>> dp(N + 1, vector<vector<uint>>(m, vector<uint>(n, 0)));
        auto paths = [&](int k, int i, int j) { return (i < 0 || i >= m || j < 0 || j >= n) ? 1 : dp[k][i][j]; };
        for (int k = 1; k <= N; k++) 
        {
            for (int i = 0; i < m; i++) 
            {
                for (int j = 0; j < n; j++) 
                {
                    dp[k][i][j] = paths(k - 1, i - 1, j) + paths(k - 1, i + 1, j) + paths(k - 1, i, j - 1) + paths(k - 1, i, j + 1);
                    dp[k][i][j] %= 1000000007;
                }
            }
        }
        return dp[N][i0][j0];
    }
```
recursive is a top down and dp is bottom up. the relation is similar.
	
### 332. Reconstruct Itinerary
The nice thing about DFS is it tries a path, and if that's wrong (i.e. path does not lead to solution), DFS goes one step back and tries another path. It continues to do so until we've found the correct path (which leads to the solution). You need to always bear this nice feature in mind when utilizing DFS to solve problems.

In this problem, the path we are going to find is an itinerary which:

uses all tickets to travel among airports
preferably in ascending lexical order of airport code
Keep in mind that requirement 1 must be satisfied before we consider 2. If we always choose the airport with the smallest lexical order, this would lead to a perfectly lexical-ordered itinerary, but pay attention that when doing so, there can be a "dead end" somewhere in the tickets such that we are not able visit all airports (or we can't use all our tickets), which is bad because it fails to satisfy requirement 1 of this problem. Thus we need to take a step back and try other possible airports, which might not give us a perfectly ordered solution, but will use all tickets and cover all airports.

Thus it's natural to think about the "backtracking" feature of DFS. We start by building a graph and then sorting vertices in the adjacency list so that when we traverse the graph later, we can guarantee the lexical order of the itinerary can be as good as possible. When we have generated an itinerary, we check if we have used all our airline tickets. If not, we revert the change and try another ticket. We keep trying until we have used all our tickets.

good to know what backtracking is: try a solution, if failure, abandom it and try next.

Adjacent matrix: always sort the nodes (using multiset can sort it automatically)
```cpp
    bool dfs(unordered_map<string,multiset<string>>& mp,string node,multiset<pair<string,string>>& to_visit,vector<string>& ans)
    {
        if(to_visit.size()==0) return 1;//visited all
        if(mp[node].size()==0) return 0;//dead end
        
        for(auto it=mp[node].begin();it!=mp[node].end();it++)
        {
            string next=*it;
            pair<string,string> p=make_pair(node,next);
            auto it1=to_visit.find(p);
            if(it1!=to_visit.end())
            {
                to_visit.erase(it1);//remove this edge
                ans.push_back(next);
                if(dfs(mp,next,to_visit,ans)) {return 1;}
                to_visit.insert(p);//put back this edge if failed
                ans.pop_back();
            }
        }
        return 0;
    }
```    

### 133. Clone Graph
when clone the graph, those pointers are invalidated and they shall point to new nodes
mapping pointers to id, id to pointer then we can rebuild the relation

### 753. Cracking the Safe
n boxes with n digits password using k digits only (from 0 to k-1)
the min length of input to guarantee opening it. (auto match the password)
the string shall contain all possible combinations of the k digits
and we shall combine these combinations with the largest common. 123, 231 shall be combined as 1231
Note each digit can be repeated: so the total combination is k^n (k max is 10, n max is 4, k^n max is 4096)
greedy choice: 
two digits: 00, 01, 11, 10-->00110. every time only the last n-1 digits are reserved and append a new digit in reversed way, ie. from k-1 to 0
if we try from 0 to k-1: 00, 01, 10, 11-->001011, which is 6, not the smallest one. 
if a cycle is seen, continue next.
