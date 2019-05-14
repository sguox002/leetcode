# Chapter 12 dfs
dfs is generally recursive, reducing to similar subproblems
generally involves push and pop a new path
generally uses coloring to indicate visited nodes
dfs/bfs/disjoint set sometimes can be all used for similar problems

some tree problems are not discussed here in detail.

## dfs or recursive

## straightfoward **

### 690. Employee Importance
employee's importance=itself's importance+all its subordinate's importance.<br/>
this is a n-tary tree with its sum.<br/>
first build a tree map, and then use recursion:<br/>
importance=self+sum(child's importance), child's importance is a subproblem<br/>
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
typical dfs or coloring

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
dfs and count the number and mark it as different color<br/>
dfs is much simpler than bfs.<br/>
The following code is used for finding the area of one connected region<br/>

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
       return 1+dfs(grid,i-1,j)+dfs(grid,i+1,j)+dfs(grid,i,j-1)+dfs(grid,i,j+1);
    }
```    

### 547. Friend Circles
disjoint set could be perfect for this<br/>
if M[i,j]==1 then we merge i and j

for dfs: if M(i,j)==1 then we need dfs from i and dfs from j. the two are a group. 
That means we shall iterate on the person instead of the edges, hence coloring on the matrix is not good. 
instead we shall use the person if visited.

The relation matrix is symmetric. and can we only need to use half? 
The answer is no, since when we found i relates with j, j could relates with people ahead of i. 
so We need iterate on all people.
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
dfs version: using only the upper triangle.
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
The above solution is much slower than using disjoint set (140ms vs 40ms). 

Since we use a loop to find all those nodes sharing coordinates, this could be reduced using a hashmap.

### 1020. Number of enclaves
count inland lands<br/>
The first cycle does DFS for the boundary cells. The second cycle counts the remaining land.<br/>
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

### 934. Shortest Bridge
two islands, the shortest bridge (4 direction connection)<br/>
dfs to get the two parties and calculate the mutual distance and get the min<br/>
note the distance is not sqrt distance<br/>
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

### 200. Number of Islands
disjoint set also ok<br/>
4 direction. use coloring and dfs<br/>
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
note use a color which cannot be the same<br/>
second, restore the inner side cells<br/>

### 130. Surrounded Regions
dfs or union find. but if the group touches the boundary it shall be excluded.
```cpp
class union_find
{
    int m,n;
    int count;
    vector<int> parent;
public:
    union_find(vector<vector<char>>& grid)
    {
        count=0;
        m=grid.size(),n=grid[0].size();
        parent.resize(m*n+1);
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]=='O') {parent[i*n+j]=i*n+j;count++;}
            }
        }
        parent[n*m]=n*m;
    }
    int find(int node)
    {
        while(parent[node]!=node) node=parent[node];
        return node;
    }
    void merge(int ni,int nj)
    {
        int i_id=find(ni);
        int j_id=find(nj);
        if(i_id==j_id) return;
        if(i_id<j_id) parent[j_id]=i_id;
        else parent[i_id]=j_id;
        count--;
    }
    bool connected(int ni,int nj) {return find(ni)==find(nj);}
    int get_numset() {return count;}
};

//we are only interested in those region which touches the boundary!
//or we are only interested in those region which does not touch the boundary
//dummy node n*m is connected to those region touching the boundary
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        if(board.size()==0 || board[0].size()==0) return;
        int m=board.size(),n=board[0].size();
        union_find uf(board);
        int dx[]={-1,1,0,0},dy[]={0,0,-1,1};
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if((i==0||i==m-1||j==0||j==n-1)&&board[i][j]=='O') // if a 'O' node is on the boundry, connect it to the dummy node
                    uf.merge(i*n+j,n*m);
                if(board[i][j]=='O')
                for(int k=0;k<4;k++)    
                {
                    int x=i+dx[k],y=j+dy[k];
                    if(x>=0 && x<m && y>=0 && y<n && board[x][y]=='O') uf.merge(i*n+j,x*n+y);
                }
            }
        }
        //capture all those regions surrounded by X
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(!uf.connected(i*n+j,n*m))
                { // if a 'O' node is not connected to the dummy node, it is captured
                    board[i][j]='X';
                }
            }
        }        
    }
};
```


## more complicated ***

### 756. Pyramid Transition Matrix
It is very hard to understand the question. <br/>
Given a list of allowed triplets, the pyramid is built layer by layer, 
each triangle (upper layer node with its adjacent lower layer two nodes) must be in the allowed list. <br/>
The question is: given the bottom layer and the allowed list, can we build the pyramid? (all the way to the top).<br/>

So the solution is:<br/>
get the possible string for upper layer and recursively solve each problem.<br/>
we can build a map with two chars with the other char. Building all kinds of combinations from lower layer can use dfs.<br/>
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
this is more like a dp or knapsack problem.
given a list of special offsers and prices for buying individually
the target cannot be altered i.e we shall buy exactly the amount.

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
typical backtracing problem


### 851. Loud and Rich
N people<br/>
richer: richer(x,y) means x is richer than y, only a subset is provided.<br/>
quite[x] is the quiteness of person x<br/>
find the person with money >= person x and is the loudest. <br/>
for n people, that is n problems.<br/>
example: [[1,0],[2,1],[3,1],[3,7],[4,3],[5,3],[6,3]], quiet = [3,2,5,4,6,1,7,0]<br/>
for person 0:<br/>
who is richer than him? 1<br/>
who is richer than 1? 2,3<br/>
who is richer than 2,3? 4,5,6<br/>
7 is less rich than 3 but not sure if 7 is richer than 1, so the anwer is 5 (who is loudest)<br/>

just build a map for each person with a list of richer person. and do dfs to find the least quiet person.<br/>
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
        for (int j : richer2[i]) 
			if (quiet[res[i]] > quiet[dfs(j, quiet)]) res[i] = res[j];
        return res[i];
    }
```
using memoization

### 494. Target Sum
with + and - for each number, find number of ways to get target sum S.<br/>
can be a dp or recursion with memoization<br/>
Note the S shall be in the range of [-m, m] where m is the accumulate sum.<br/>
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
        
        if(status[cur_pos].count(target+offset)) return status[cur_pos][target+offset];
        int res=0;
        res+=helper(nums,target-nums[cur_pos],cur_pos+1,status,offset);//it overflows target+/-num
        res+=helper(nums,target+nums[cur_pos],cur_pos+1,status,offset);
        status[cur_pos][target+offset]=res;
        return res;
    }
```   
//two options + or -, we shall know how to do this 

### 394. Decode String
stack, recursion problem.<br/>
s = "3[a]2[bc]", return "aaabcbc".<br/>
s = "3[a2[c]]", return "accaccacc".<br/>
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".<br/>

The subproblem is:<br/>
the first non-repeated string<br/>
the repeat number [<br/>
the to be repeated string (subproblem since it may contain [] again)<br/>
]

This effectively avoid the problem to do it first in the stack and 
expand the string which make the problem really complicated.
```cpp
    string decodeString(const string& s, int& i) {
        string res;
        while (i < s.length() && s[i] != ']') 
        {
            if (!isdigit(s[i])) res += s[i++]; //not a digit
            else 
            {
                int n = 0;
                while (i < s.length() && isdigit(s[i])) n = n * 10 + s[i++] - '0';
                i++; // '['
                string t = decodeString(s, i);
                i++; // ']'
                while (n-- > 0) res += t; //repeat n times
            }
        }
        return res;
    }
```
to approach this kind of problem: we need consider the simplest form and consider the inner subproblem as a solved problem<br/>
we need to learn to process this type of problems.<br/>
	
### 802. Find Eventual Safe States
given a directed graph, starting from some node if we can reach a terminal node (not a cycle)<br/>
the node is called safe node, return all the safe nodes.<br/>
so: a node start, it must goes to a terminal for all paths. If any path fails (cycle) it is not a safe node.<br/>
if the path contains any non-safe node, it is not safe either.<br/>

visited can also use coloring, they are equivalent, but coloring is more efficient.<br/>
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
the key is all paths shall reach a terminal, if one fail, then fail.

### 785. Is Graph Bipartite?
edge only occurs between different parties but not inside.<br/>
two coloring: a->b we color a as 0 and b as 1, then b->c we color c as 1... <br/>
if we found no conflicts it is two parites.<br/>
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
	
 0: Haven't been colored yet.<br/>
 1: Blue.<br/>
 -1: Red.<br/>
For each node,<br/>

If it hasn't been colored, use a color to color it. Then use the other color to color all its adjacent nodes (DFS).<br/>
If it has been colored, check if the current color is the same as the color that is going to be used to color it.<br/>
three states, for example course schedule.


### 886. Possible Bipartition
disliked person cannot be in the same group<br/>
use coloring. three state 0: not assigned, 1/-1<br/>
build the input to ajacent matrix.<br/>

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
Note: the precheck color[node]!=-nc is critical: <br/>
since the following condition only checks non-set colors. 
if already set, we need check if it is the color we expected. otherwise it is 0. <br/>
If it is same color, we need continue checking (cannot return)<br/>


### 491. Increasing Subsequences
Given an integer array, your task is to find all the different possible increasing subsequences of the given array, and the length of an increasing subsequence should be at least 2 
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

### 721. Accounts Merge
union find is more suitable

### 207. Course Schedule
There are a total of n courses you have to take, labeled from 0 to n-1.<br/>

Some courses may have prerequisites, <br/>
for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]<br/>
Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?<br/>
```cpp
    bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
        vector<vector<int>> adj(numCourses);
        for(int i=0;i<prerequisites.size();i++) adj[prerequisites[i].second].push_back(prerequisites[i].first);
        vector<int> explored(numCourses,-1); //-1 checked, 0: is in check, 1: checked
        for(int i=0;i<numCourses;i++)
            if(!explore(adj,i,explored)) return 0;
        return 1;
    }
    bool explore(vector<vector<int>>& adj,int v,vector<int>& explored)
    {
        if(explored[v]>0) return 1;
        
        explored[v]=0;
        for(int i=0;i<adj[v].size();i++)
        {
            if(explored[adj[v][i]]==0) return 0;//a cycle
            if(explored[adj[v][i]]<0 && !explore(adj,adj[v][i],explored)) return 0;
        }
        explored[v]=1;
        return 1;
    }
```
3 states while exploring: (to avoid recursively explore)<br/>
-1: not checked, 0 it is in check, 1 checked.<br/>

### 210. Course Schedule II
There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

```cpp
    vector<int> findOrder(int numCourses, vector<pair<int, int>>& prerequisites) {
        //all edges shall be visited and only once
        unordered_map<int,unordered_set<int>> v;
        for(int i=0;i<numCourses;i++) v[i]=unordered_set<int>();
        for(int i=0;i<prerequisites.size();i++)
        {
            v[prerequisites[i].first].insert(prerequisites[i].second);
        }
        vector<int> ans;
        while(v.size())
        {
            vector<int> q=scanforSource(v);
            if(q.size()==0) return vector<int>();
            //semi bfs
            int sz=q.size();
            for(int i=0;i<sz;i++) remove_node(v,q[i]);
            for(int i=0;i<sz;i++) v.erase(q[i]);
            ans.insert(ans.end(),q.begin(),q.end());
        }
        return ans;        
    }
    vector<int> scanforSource(unordered_map<int,unordered_set<int>>& mp)
    {
        vector<int> ans;
        for(auto it=mp.begin();it!=mp.end();it++)
        {
           if(it->second.size()==0)  ans.push_back(it->first);
        }
        return ans;
    }
    
    void remove_node(unordered_map<int,unordered_set<int>>& mp,int ind)
    {
        for(auto it=mp.begin();it!=mp.end();it++)
        {
            if(it->second.size() && it->second.count(ind)) it->second.erase(ind);
        }
    }
```
better solution using dfs
```cpp
    typedef vector<vector<int>> graph;
    vector<int> findOrder(int numCourses, vector<pair<int, int>>& prerequisites) {
        graph g = buildGraph(numCourses, prerequisites);
        vector<int> order;
        vector<bool> todo(numCourses, false), done(numCourses, false);
        for (int i = 0; i < numCourses; i++) {
            if (!done[i] && !acyclic(g, todo, done, i, order)) {
                return {};
            }
        }
        reverse(order.begin(), order.end());
        return order;
    }

    graph buildGraph(int numCourses, vector<pair<int, int>>& prerequisites) {
        graph g(numCourses);
        for (auto p : prerequisites) {
            g[p.second].push_back(p.first);
        }
        return g;
    }
    
    bool acyclic(graph& g, vector<bool>& todo, vector<bool>& done, int node, vector<int>& order) {
        if (todo[node])  return false;
        
        if (done[node]) return true;
        
        todo[node] = done[node] = true;
        for (int neigh : g[node]) {
            if (!acyclic(g, todo, done, neigh, order)) {
                return false;
            }
        }
        order.push_back(node);
        todo[node] = false;
        return true;
    }
	
```	
### 417. Pacific Atlantic Water Flow	
top and left are pacfic, right and bottom are atlantic  <br/>
given a matrix of altitude of cells.<br/>
Find the list of grid coordinates where water can flow to both the Pacific and Atlantic ocean.<br/>

```cpp
    vector<pair<int, int>> res;
    vector<vector<int>> visited;

    vector<pair<int, int>> pacificAtlantic(vector<vector<int>>& matrix) {
        if (matrix.empty()) return res;
        int m = matrix.size(), n = matrix[0].size();
        visited.resize(m, vector<int>(n, 0));
        for (int i = 0; i < m; i++) 
        {
            dfs(matrix, i, 0, INT_MIN, 1);
            dfs(matrix, i, n - 1, INT_MIN, 2);
        }
        for (int i = 0; i < n; i++) 
        {
            dfs(matrix, 0, i, INT_MIN, 1);
            dfs(matrix, m - 1, i, INT_MIN, 2);
        }
        return res;
    }
	
    void dfs(vector<vector<int>>& m, int x, int y, int pre, int preval){
        if (x < 0 || x >= m.size() || y < 0 || y >= m[0].size()  
                || m[x][y] < pre || (visited[x][y] & preval) == preval) 
            return;
        visited[x][y] |= preval;
        if (visited[x][y] == 3) res.push_back({x, y});
        dfs(m, x + 1, y, m[x][y], visited[x][y]); 
        dfs(m, x - 1, y, m[x][y], visited[x][y]);
        dfs(m, x, y + 1, m[x][y], visited[x][y]); 
        dfs(m, x, y - 1, m[x][y], visited[x][y]);
    }
```
*. from boundary to inner side
*. mark 1 for pacific and 2 for atlantic, both are 3

	
### 473. Matchsticks to Square
using sticks with different length to form a square
You have to use all the sticks just once

first calculate the side length, which is the target sum.
dfs to get the target.
we have seen similar knapsack problems. (different choice may invalid the whole )
```cpp
    bool makesquare(vector<int>& nums) {
        //each side adds up to total/4
        if(nums.size()==0) return 0;
        int total=accumulate(nums.begin(),nums.end(),0);
        int side=total/4;
        if(total%4) return 0;
        unordered_set<int> visited;
        sort(nums.begin(),nums.end(),greater<int>());
        for(int i=0;i<4;i++)
        {
            if(!dfs(nums,side,0,visited)) return 0;
        }
        return visited.size()==nums.size();
    }
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
            for(int j=0;j<n;j++) 
				if(matrix[i][j]) 
					ans[i][j]=update_closest(matrix,i,j,q);

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
first, build the queue to solve. when a cell has a neighboring 0, we just mark it as 1. otherwise we add it into a queue (its surroudings are all 1)
second, we process a cell in the queue and update its value with its neighboring minimum (bfs), if the min==level, the answer is fixed to be min+1. otherwise we shall add back into queue for further processing.

	

### 576. Out of Boundary Paths
recursive or dp: outside probability is 0, inside is 1.
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
```cpp
    UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) {
        //name each node from 0 to n and create a map
        if(!node) return node;
        unordered_map<UndirectedGraphNode*,int> nodeid_map;
        unordered_map<int,UndirectedGraphNode*> idnode_map;
        //bfs search to traverse all nodes
        stack<UndirectedGraphNode*> st;
        st.push(node);
        UndirectedGraphNode *p,*newnode=0;
        int id=0;
        //to avoid dfs repetitive visit, also need a visited array.
        while(!st.empty()) //if it contains a cycle, this would be a problem!
        {
            p=st.top();st.pop();
            if(!nodeid_map.count(p)) //this also serves as a visited array
            {
                nodeid_map[p]=id;idnode_map[id]=p;id++;
                for(int i=0;i<p->neighbors.size();i++) 
                    if(p!=p->neighbors[i]) st.push(p->neighbors[i]);
            }
        }
        //now clone and make id-node map
        int num_nodes=nodeid_map.size();
        vector<UndirectedGraphNode*> vnode(num_nodes);
        for(auto it=nodeid_map.begin();it!=nodeid_map.end();it++)
        {
            p=new UndirectedGraphNode(it->first->label);
            vnode[it->second]=p; 
        }
        //fill its neighbors
        for(int i=0;i<vnode.size();i++)
        {
            p=idnode_map[i];
            for(int j=0;j<p->neighbors.size();j++)
            {
                int id=nodeid_map[p->neighbors[j]];
                vnode[i]->neighbors.push_back(vnode[id]);
            }
        }
        return vnode[0];
    }
```

### 329. Longest Increasing Path in a Matrix
dfs with memoization
```cpp
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if(matrix.size()==0) return 0;
        int m=matrix.size(),n=matrix[0].size();
        int maxlen=0;
        vector<vector<int>> dp(m,vector<int>(n));
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                maxlen=max(maxlen,dfs(matrix,i,j,INT_MIN,dp));
            }
        }
        return maxlen;
    }
    
    int dfs(vector<vector<int>>& mat,int i,int j,int prev,vector<vector<int>>& dp)
    {
        if(i<0 || j<0 || i>=mat.size() || j>=mat[0].size() || mat[i][j]<=prev) return 0;
        if(dp[i][j]) return dp[i][j];
        int d=mat[i][j];
        mat[i][j]=INT_MIN;
        int d0=1+dfs(mat,i-1,j,d,dp);
        int d1=1+dfs(mat,i+1,j,d,dp);
        int d2=1+dfs(mat,i,j-1,d,dp);
        int d3=1+dfs(mat,i,j+1,d,dp);
        mat[i][j]=d;
        return dp[i][j]=max({d0,d1,d2,d3});
    }
```
## hard
	
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

	
### 980. Unique Paths III	
On a 2-dimensional grid, there are 4 types of squares:

1 represents the starting square.  There is exactly one starting square.
2 represents the ending square.  There is exactly one ending square.
0 represents empty squares we can walk over.
-1 represents obstacles that we cannot walk over.
Return the number of 4-directional walks from the starting square to the ending square, that walk over every non-obstacle square exactly once.

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
first we get number of grids to walk, and get the starting cell and ending cell	
dfs will try all possible paths and we only need to count the paths satisfying the conditions.


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
```cpp
    string crackSafe(int n, int k) {
    string ans = string(n, '0');
    unordered_set<string> visited;
    visited.insert(ans);

    for(int i = 0;i<pow(k,n);i++)
    {
        string prev = ans.substr(ans.size()-n+1,n-1);
        for(int j = k-1;j>=0;j--)
        {
            string now = prev + to_string(j);
            if(!visited.count(now))
            {
                visited.insert(now);
                ans += to_string(j);
                break;
            }       
        }
    }
    return ans;
    }
```

### 827. Making A Large Island	
In a 2D grid of 0s and 1s, we change at most one 0 to a 1.

After, what is the size of the largest island? (An island is a 4-directionally connected group of 1s).

union find: to see if we can connect two groups
```cpp
class union_find
{
    int m,n;
    int count;
    vector<int> parent;
    vector<int> size;
    int max_size;
public:
    union_find(vector<vector<int>>& grid)
    {
        count=0;
        max_size=0;
        m=grid.size(),n=grid[0].size();
        parent.resize(m*n+1);
        size.resize(m*n+1);
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]) {parent[i*n+j]=i*n+j;size[i*n+j]=1;count++;}
            }
        }
        if(count) max_size=1;
        parent[n*m]=n*m;//dummy node
    }
    int find(int node)
    {
        while(parent[node]!=node) node=parent[node];
        return node;
    }
    void merge(int ni,int nj)
    {
        int i_id=find(ni);
        int j_id=find(nj);
        if(i_id==j_id) return;
        if(i_id<j_id) {parent[j_id]=i_id;size[i_id]+=size[j_id];size[j_id]=0;max_size=max(max_size,size[i_id]);}
        else {parent[i_id]=j_id;size[j_id]+=size[i_id];size[i_id]=0;max_size=max(max_size,size[j_id]);}
        count--;
    }
    bool connected(int ni,int nj) {return find(ni)==find(nj);}
    int get_numset() {return count;}
    int get_size(int p) {return size[p];}
    int get_maxsize() {return max_size;}
};
class Solution {
public:
    int largestIsland(vector<vector<int>>& grid) {
        //disjoint set and see which one flip will add the largest
        //using the i*n+j as the root
        //bfs search to merge 
        union_find uf(grid);
        int m=grid.size(),n=grid[0].size();
        int dir[][2]={{-1,0},{1,0},{0,-1},{0,1}};
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j])
                {
                    for(int k=0;k<4;k++)
                    {
                        int x=i+dir[k][0],y=j+dir[k][1];
                        if(x>=0 && x<m && y>=0 && y<n && grid[x][y])
                        {
                            uf.merge(i*n+j,x*n+y);
                        }
                    }
                }
            }
        }
        //now we get all the adjoint set, we will check the 0 to see which connect two or three or 4
        int maxlen=INT_MIN;
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(!grid[i][j])
                {
                    unordered_set<int> us;
                    for(int k=0;k<4;k++)
                    {
                        int x=i+dir[k][0],y=j+dir[k][1];

                        if(x>=0 && x<m && y>=0 && y<n && grid[x][y])
                        {
                            int p=uf.find(x*n+y);//same region cannot be added multiple times
                            us.insert(p);
                        }
                    }
                    int cnt=0;
                    for(auto it=us.begin();it!=us.end();it++) cnt+=uf.get_size(*it);
                    maxlen=max(maxlen,cnt);
                }
            }
        }
        return maxlen==INT_MIN?uf.get_maxsize():maxlen+1;
    }
};
```

### 679. 24 Game
You have 4 cards each containing a number from 1 to 9. You need to judge whether they could operated through *, /, +, -, (, ) to get the value of 24.
the division is floating division
```cpp
    bool judgePoint24(vector<int>& nums) {
        //recursive approach. 
        //for all combination check if we can get 24
        //pick any two numbers and do +-*/ and reduce to 3 numbers, then 2 and then 1
        vector<double> v(nums.size());
        for(int i=0;i<nums.size();i++) v[i]=nums[i];
        return search24(v);
    }
    
    bool search24(vector<double> v)
    {
        if(v.size()==1) return abs(v[0]-24)<1e-7;
        vector<double> list;
        char op[]={'+','*','-','/'};
        for(int i=0;i<v.size();i++)
        {
            for(int j=0;j<v.size();j++)
            {
                if(i==j) continue; //same cards cannot be used
                //pick v[i] op v[j]
                //+ * a+b=b+a a*b=b*a so do not have to compute
                for(int k=0;k<4;k++) //operations
                {
                    if((op[k]=='+' || op[k]=='*') && i>j) continue;
                    if(op[k]=='/' && v[j]==0) continue; //cannot divide 0
                    double res;
                    switch(op[k])
                    {
                        case '+': res=v[i]+v[j];break;
                        case '-': res=v[i]-v[j];break;
                        case '*': res=v[i]*v[j];break;
                        case '/': res=v[i]/v[j];break;
                    }
                    //remove i,j and add res int a new list
                    list.clear();
                    for(int l=0;l<v.size();l++) if(l!=i && l!=j) list.push_back(v[l]);
                    list.push_back(res);
                    //for(int l=0;l<list.size();l++) cout<<list[i]<<",";cout<<endl;
                    if(search24(list)) return 1;
                }
            }
        }
        return 0;
    }
```
more like a divide and conquer problem.


### 749. Contain Virus	
A virus is spreading rapidly, and your task is to quarantine the infected area by installing walls.

The world is modeled as a 2-D array of cells, where 0 represents uninfected cells, and 1 represents cells contaminated with the virus. A wall (and only one wall) can be installed between any two 4-directionally adjacent cells, on the shared boundary.

Every night, the virus spreads to all neighboring cells in all four directions unless blocked by a wall. Resources are limited. Each day, you can install walls around only one region -- the affected area (continuous block of infected cells) that threatens the most uninfected cells the following night. There will never be a tie.

Can you save the day? If so, what is the number of walls required? If not, and the world becomes fully infected, return the number of walls used.

### 514. Freedom Trail
dp solution can also use top down recursive solution
```cpp
    int findRotateSteps(string ring, string key) {
        //dfs
        int n=ring.size();
        int m=key.size();
        unordered_map<char,vector<int>> mp;
        for(int i=0;i<ring.size();i++) mp[ring[i]].push_back(i);
        //we are looking for a minimum path sum, ring initial is always 0
        //based on the memoization dfs approach, the dp approach is then simple
        vector<vector<int>> dp(n,vector<int>(m+1,INT_MAX));
        for(int i=0;i<n;i++) dp[i][m]=0;
        for(int j=m-1;j>=0;j--)
        {
            vector<int>& v=mp[key[j]];
            //for(int i=0;i<ring.size();i++)
            for(int i=n-1;i>=0;i--)
            {
                for(int k=0;k<v.size();k++) //all possible index of the current char
                {
                    int d=abs(v[k]-i); //
                    d=min(d,n-d);
                    dp[i][j]=min(dp[i][j],d+dp[v[k]][j+1]);
                }
            }
        }
        int minSteps=dp[0][0];
        return minSteps+m;
    }
```



### 924. Minimize Malware Spread	
In a network of nodes, each node i is directly connected to another node j if and only if graph[i][j] = 1.

Some nodes initial are initially infected by malware.  Whenever two nodes are directly connected and at least one of those two nodes is infected by malware, both nodes will be infected by malware.  This spread of malware will continue until no more nodes can be infected in this manner.

Suppose M(initial) is the final number of nodes infected with malware in the entire network, after the spread of malware stops.

We will remove one node from the initial list.  Return the node that if removed, would minimize M(initial).  If multiple nodes could be removed to minimize M(initial), return such a node with the smallest index.

Note that if a node was removed from the initial list of infected nodes, it may still be infected later as a result of the malware spread.

union find
```cpp
    int minMalwareSpread(vector<vector<int>>& graph, vector<int>& initial) {
        int n=graph.size();
        vector<int> parents(n),size(n,1);
        
        for(int i=0;i<n;i++) parents[i]=i;
        int num_set=n;
        for(int i=0;i<n;i++)
        {
            for(int j=i;j<n;j++)
            if(graph[i][j]) merge(parents,i,j,num_set,size);
        }
        //sort(initial.begin(),initial.end());
        //copy(size.begin(),size.end(),ostream_iterator<int>(cout," "));
        unordered_map<int,set<int>> mp;
        for(int i=0;i<initial.size();i++)
            mp[get_id(parents,initial[i])].insert(initial[i]);
        //if there are more than one, then it is 0
        int max0=0,ans=INT_MAX;
        for(auto it=mp.begin();it!=mp.end();it++)
        {
            if(it->second.size()>1) 
            {
                if(max0==0) ans=min(ans,*(it->second.begin()));
            }
            else
            {
                if(max0<size[it->first])
                {
                    max0=size[it->first];
                    ans=*(it->second.begin());
                }
                else if(max0==size[it->first])
                    ans=min(ans,*(it->second.begin()));
            }
        }
        return ans;
        
    }
    int get_id(vector<int>& parent,int i)
    {
        while(i!=parent[i]) i=parent[i];
        return i;
    }
    void merge(vector<int>& parent,int i,int j,int& num_set,vector<int>& size)
    {
        int i_id=get_id(parent,i);
        int j_id=get_id(parent,j);
        if(i_id==j_id) return;
        if(i_id<j_id) {parent[j_id]=i_id;size[i_id]+=size[j_id];size[j_id]=0;}
        else {parent[i_id]=j_id;size[j_id]+=size[i_id];size[i_id]=0;}
        num_set--;
    }    
```

### 928. Minimize Malware Spread II
We will remove one node from the initial list, completely removing it and any connections from this node to any other node.  Return the node that if removed, would minimize M(initial).  If multiple nodes could be removed to minimize M(initial), return such a node with the smallest index.

union find
```cpp
    int minMalwareSpread(vector<vector<int>>& graph, vector<int>& initial) {
        //the idea: build the disjoint set by removing all infected nodes
        //add the infected nodes except the removed one one by one and find the one with most infected
        //union-find is fast to do union but not good for splitting and that is why we use the above approach
        int n=graph.size();
        vector<int> parents(n),size(n,1);
        //set<int> infected(initials.begin(),initials.end());
        
        sort(initial.begin(),initial.end());
        int minsize=INT_MAX,ans=INT_MAX;        
        for(int k=0;k<initial.size();k++)
        {
            int remove=initial[k];
            int infected_size=0;
            //build the union by disable the remove node
            for(int i=0;i<n;i++) {parents[i]=i;size[i]=1;}
            int num_set=n;
            for(int i=0;i<n;i++)
            {
                for(int j=i;j<n;j++)
                {
                    if(i==remove || j==remove) continue;
                    if(graph[i][j]) merge(parents,i,j,num_set,size);
                }
            }
            //get the number of infected nodes
            unordered_set<int> myset;
            for(int t=0;t<initial.size();t++)
            {
                if(initial[t]==remove) continue;
                myset.insert(get_id(parents,initial[t]));
            }
            
            for(auto it=myset.begin();it!=myset.end();it++)
                infected_size+=size[*it];
            if(minsize>infected_size) minsize=infected_size,ans=remove;
        }
        return ans;
    }
    int get_id(vector<int>& parent,int i)
    {
        while(i!=parent[i]) i=parent[i];
        return i;
    }
    void merge(vector<int>& parent,int i,int j,int& num_set,vector<int>& size)
    {
        int i_id=get_id(parent,i);
        int j_id=get_id(parent,j);
        if(i_id==j_id) return;
        if(i_id<j_id) {parent[j_id]=i_id;size[i_id]+=size[j_id];size[j_id]=0;}
        else {parent[i_id]=j_id;size[j_id]+=size[i_id];size[i_id]=0;}
        num_set--;
    }    
```
	
### 301. Remove Invalid Parentheses
Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

Note: The input string may contain letters other than the parentheses ( and ).
```cpp
    vector<string> removeInvalidParentheses(string s) {
        vector<string> ans;
        helper(s,ans,0,0,{'(',')'});
        return ans;
    }
    void helper(string s,vector<string>& ans,int lasti,int lastj,vector<char> par)
    {
        int cnt=0;
        for(int i=lasti;i<s.size();i++)
        {
            if(s[i]==par[0]) cnt++;
            if(s[i]==par[1]) cnt--;
            if(cnt<0)
            {
                for(int j=lastj;j<=i;j++)
                {
                    if(s[j]==par[1] && (j==lastj || s[j-1]!=par[1]))
                        helper(s.substr(0,j)+s.substr(j+1),ans,i,j,par);
                }
                return; 
            }
        }
        string rs=s;
        reverse(rs.begin(),rs.end());
        if(par[0]=='(') helper(rs,ans,0,0,{')','('}); //finished left to right
        else ans.push_back(rs);//finished right to left
    }	
```

### 488. Zuma Game
Think about Zuma Game. You have a row of balls on the table, colored red(R), yellow(Y), blue(B), green(G), and white(W). You also have several balls in your hand.

Each time, you may choose a ball in your hand, and insert it into the row (including the leftmost place and rightmost place). Then, if there is a group of 3 or more balls in the same color touching, remove these balls. Keep doing this until no more balls can be removed.

Find the minimal balls you have to insert to remove all the balls on the table. If you cannot remove all the balls, output -1.

### 546. Remove Boxes
more dp than dfs
Given several boxes with different colors represented by different positive numbers. 
You may experience several rounds to remove boxes until there is no box left. Each time you can choose some continuous boxes with the same color (composed of k boxes, k >= 1), remove them and get k*k points.
Find the maximum points you can get.

```cpp
    int removeBoxes(vector<int> boxes) {
        int n = boxes.size();
        vector<vector<vector<int>>> dp(n,vector<vector<int>>(n,vector<int>(n)));
        return removeBoxesSub(boxes, 0, n - 1, 0, dp);
    }

    int removeBoxesSub(vector<int>& boxes, int i, int j, int k, vector<vector<vector<int>>>& dp) 
    {
        if (i > j) return 0;
        if (dp[i][j][k] > 0) return dp[i][j][k];
        for (; i + 1 <= j && boxes[i + 1] == boxes[i]; i++, k++); 
        // optimization: all boxes of the same color counted continuously from the first box should be grouped together
        int res = (k + 1) * (k + 1) + removeBoxesSub(boxes, i + 1, j, 0, dp);
        for (int m = i + 1; m <= j; m++) {
            if (boxes[i] == boxes[m]) {
                res = max(res, removeBoxesSub(boxes, i + 1, m - 1, 0, dp) + removeBoxesSub(boxes, m, j, k + 1, dp));
            }
        }
        dp[i][j][k] = res;
        return res;
    }
```

### 664. Strange Printer
There is a strange printer with the following two special requirements:

The printer can only print a sequence of the same character each time.
At each turn, the printer can print new characters starting from and ending at any places, and will cover the original existing characters.
Given a string consists of lower English letters only, your job is to count the minimum number of turns the printer needed in order to print it.

```cpp
    int strangePrinter(string s) {
        //this is similar to the Removing Boxes
        //define T(i,j,k) as the minimum number for substr(i,j) with k same numbers as s[j+1]
        if(s.length()<1) return 0;
        string ss;
        char c=s[0];
        ss+=s[0];
        for(int i=1;i<s.length();i++) if(s[i]!=c) {c=s[i];ss+=s[i];}
        //cout<<s.length()<<"->"<<ss.length();
        s=ss;
        int n=s.length();
        //vector<vector<vector<int>>> dp(n,vector<vector<int>>(n,vector<int>(2)));
        vector<vector<int>> dp(n,vector<int>(n));
        return helper(s,dp,0,n-1);
    }
    int helper(string& s,vector<vector<int>>& dp,int i,int j)
    {
        if(i>j) return 0;
        if(i==j) return 1;
        if(dp[i][j]) return dp[i][j];
        //k is the number of same char
        //for(;i<=j && s[i+1]==s[i];i++) k++; //same character shall be grouped
        int res=1+helper(s,dp,i+1,j);//no char attached
        //cout<<res;
        for(int m=i+1;m<=j;m++)
        {
            if(s[m]==s[i])
                res=min(res,helper(s,dp,i+1,m-1)+helper(s,dp,m,j));
        }
        dp[i][j]=res;
        return res;
    }
```

### 472. Concatenated Words
Given a list of words (without duplicates), please write a program that returns all concatenated words in the given list of words.
A concatenated word is defined as a string that is comprised entirely of at least two shorter words in the given array.

```cpp
bool cmp1(const string& s1,const string& s2) {return s1.length()<s2.length();}
class Solution {
public:
    
    struct cmp
    {//note the 3 const are required!!!!
        bool operator()(const string& s1,const string& s2) const {return s1.length()<s2.length();}
    };
    vector<string> findAllConcatenatedWordsInADict(vector<string>& words) {
        //sort the words using length
        vector<string> res;
        sort(words.begin(),words.end(),cmp1);
        //copy(words.begin(),words.end(),ostream_iterator<string>(cout," "));cout<<endl;
        unordered_set<string> dict;
        
        //multiset<string,cmp> dict(words.begin(),words.end()); //note if the length are the same it is not inserted!!!
        
        for(int i=0;i<words.size();i++)
        {
            if(canCombine(words[i],dict))
            {
                res.push_back(words[i]);
            }
            dict.insert(words[i]);
            //copy(dict.begin(),dict.end(),ostream_iterator<string>(cout," "));cout<<endl;
        }
        return res;
    }
    bool canCombine(string& word,unordered_set<string>& dict)
    {
        //set<string,cmp>::iterator it=dict.find(word);
        //auto it=dict.find(word);
        //int nelem=distance(dict.begin(),it);
        //if(nelem==0) return 0;
        if(dict.empty()) return 0;
        vector<bool> dp(word.length()+1);//dp[i]: [0...i-1] substr can be combined
        dp[0]=1;//always can form by an empty string
        for(int i=1;i<=word.length();i++)
        {
            for(int j=0;j<i;j++)
            {
                if(!dp[j]) continue;//previous one is not a word
                string t=word.substr(j,i-j);//note substr 2nd is the length
                //cout<<j<<","<<i<<":"<<t<<" "<<dict.count(t)<<endl;
                if(dict.count(t)) {dp[i]=1;break;} //cannot search the whole set!
            }
        }
        return dp[word.length()];
        
    }
```

### 839. Similar String Groups
Two strings X and Y are similar if we can swap two letters (in different positions) of X, so that it equals Y.

For example, "tars" and "rats" are similar (swapping at positions 0 and 2), and "rats" and "arts" are similar, but "star" is not similar to "tars", "rats", or "arts".

Together, these form two connected groups by similarity: {"tars", "rats", "arts"} and {"star"}.  Notice that "tars" and "arts" are in the same group even though they are not similar.  Formally, each group is such that a word is in the group if and only if it is similar to at least one other word in the group.

We are given a list A of strings.  Every string in A is an anagram of every other string in A.  How many groups are there?
union find
```cpp
class disjoint_set {
    vector<int> v;
    int sz;
public:
    disjoint_set(int n) {
        makeset(n);
    }

    void makeset(int n) {
        v.resize(n);
        iota(v.begin(), v.end(), 0);
        sz = n;
    }

    int find(int i) {
        if (i != v[i])
            v[i] = find(v[i]);
        return v[i];
    }
    
    void join(int i, int j) {
        int ri = find(i), rj = find(j);
        if (ri != rj) {
            v[ri] = rj;
            sz--;
        }
    }
    
    int size() {
        return sz;
    }
};
class Solution {
public:
    bool similar(string &a, string &b) {
        int n = 0;
        for (int i = 0; i < a.size(); i++)
            if (a[i] != b[i] && ++n > 2)
                return false;
        return true;
    }

    int numSimilarGroups(vector<string>& A) {
        disjoint_set ds(A.size());
        for (int i = 0; i < A.size(); i++)
            for (int j = i + 1; j < A.size(); j++)
                if (similar(A[i], A[j]))
                    ds.join(i, j);
        return ds.size();
    }

};
```

### 685. Redundant Connection II
In this problem, a rooted tree is a directed graph such that, there is exactly one node (the root) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with N nodes (with distinct values 1, 2, ..., N), with one additional directed edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of edges. Each element of edges is a pair [u, v] that represents a directed edge connecting nodes u and v, where u is a parent of child v.

Return an edge that can be removed so that the resulting graph is a rooted tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array.

```cpp
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        //use the fact that each node only has zero or one parent, and cannot be itself
        //note: when there is a cycle and two parents, need to remove the edge in the cycle
        //four cases: 1: loop, 2, two parents, 3, first loop then two parents 4. first two parents then loop
        unordered_map<int,int> parent;
        vector<int> twop(2,-1),loop(2,-1);
        for(int i=0;i<edges.size();i++)
        {
            int p=edges[i][0],c=edges[i][1];
            if(parent.count(c)) 
            {
                //we will first remove this edge and try later
                twop=edges[i];
                continue;
            }
            else parent[c]=p;
            if(iscycle(c,parent)) 
            {
                loop=edges[i];
                parent.erase(c);
            }
        }
        if(twop[0]<0) return loop;
        if(loop[0]<0) return twop;
        twop[0]=parent[twop[1]];
        return twop;
        
    }
    bool iscycle(int c,unordered_map<int,int>& parent)
    {
        //find its ultimate parent
        int p=c;
        while(parent.count(c)) {c=parent[c];if(c==p) break;}
        return p==c;
    }
```	


	
