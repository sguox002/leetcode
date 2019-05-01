disjoint set or union find
disjoint set can often be approached using dfs/bfs

complexity analysis
path compression
union find implementation

generally union-find is not that hard, we shall gain intuition from these problems.

128. Longest Consecutive Sequence
connect to a group 
```cpp
    int longestConsecutive(vector<int>& nums) {
        //they don't need to keep order, disjoint set
        int n=nums.size();
        if(n<2) return n;
        vector<int> parent(n),size(n,1);
        unordered_map<int,int> mp;
        for(int i=0;i<n;i++) parent[i]=i;
        for(int i=0;i<n;i++)
        {
            if(mp.count(nums[i])) continue;//ignore duplicates
            mp[nums[i]]=i;
            if(mp.count(nums[i]-1)) merge(i,mp[nums[i]-1],parent,size);
            if(mp.count(nums[i]+1)) merge(i,mp[nums[i]+1],parent,size);
        }
        return *max_element(size.begin(),size.end());
    }
    
    int getparent(int i,vector<int>& parent)
    {
        while(i!=parent[i]) i=parent[i];
        return i;
    }
    void merge(int i,int j,vector<int>& parent,vector<int>& size)
    {
        int pi=getparent(i,parent),pj=getparent(j,parent);
        if(pi==pj) return;
        if(pi<pj) {parent[pj]=pi;size[pi]+=size[pj];size[pj]=0;}
        else {parent[pi]=pj;size[pj]+=size[pi];size[pi]=0;}
    }
```	

130. Surrounded Regions
capture all regions surrounded by x
dfs or union find
dfs only process boundary 'o's.
```cpp
    void solve(vector<vector<char>>& board) {
        //mark all those O area attached to the boundary
        if(board.size()==0) return;
        int m=board.size(),n=board[0].size();
        for(int i=0;i<n;i++)
        {
            if(board[0][i]=='O') dfs(board,0,i);
            if(board[m-1][i]=='O') dfs(board,m-1,i);
        }
        for(int i=0;i<m;i++)
        {
            if(board[i][0]=='O') dfs(board,i,0);
            if(board[i][n-1]=='O') dfs(board,i,n-1);
        }
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(board[i][j]=='O') board[i][j]='X';
                else if(board[i][j]=='*') board[i][j]='O';
            }
        }
    }
    void dfs(vector<vector<char>>& board,int i,int j)
    {
        if(i<0 || j<0 ||i>=board.size()||j>=board[0].size()) return;
        if(board[i][j]=='O')
        {
            board[i][j]='*';
            dfs(board,i+1,j);
            dfs(board,i-1,j);
            dfs(board,i,j+1);
            dfs(board,i,j-1);
        }
    }
```

200. Number of Islands
dfs or union find
```cpp
    int numIslands(vector<vector<char>>& grid) {
        //dfs coloring
        if(grid.size()==0) return 0;
        int m=grid.size(),n=grid[0].size();
        int ans=0;
        for(int i=0;i<m;i++)
            for(int j=0;j<n;j++)
                if(grid[i][j]=='1'){ans++;dfs(grid,i,j);}
        return ans;
    }
    void dfs(vector<vector<char>>& grid,int i,int j)
    {
        if(i<0 || i>=grid.size() || j<0 || j>=grid[0].size()) return;
        if(grid[i][j]!='1') return;
        grid[i][j]='2';
        dfs(grid,i-1,j);
        dfs(grid,i+1,j);
        dfs(grid,i,j-1);
        dfs(grid,i,j+1);
    }
```

399. Evaluate Division
a/b->we can also get b/a
approach 1: graph
```cpp
    vector<double> calcEquation(vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries) {
       //build a graph a->b->c->d then we get a/d. need also reverse edge
        //can also use hashmap
        unordered_map<string,unordered_map<string,double>> mp;//need provide customized hash function
        for(int i=0;i<equations.size();i++)
        {
            mp[equations[i].first].insert(make_pair(equations[i].second,values[i]));
            mp[equations[i].second].insert(make_pair(equations[i].first,1.0/values[i]));
        }
        
        //print_map(mp);
        //dfs search to see if it can reaches to the other variable
        vector<double> ans(queries.size(),-1);
        
        for(int i=0;i<queries.size();i++)
        {
            unordered_set<string> visited;//everytime it needs clear up
            ans[i]=helper(queries[i],mp,visited);
            if(abs(ans[i])<1e-4) ans[i]=-1.0;
        }
        return ans;
    }
    
    double helper(pair<string,string>& q,unordered_map<string,unordered_map<string,double>>& mp,unordered_set<string>& visited)
    {
        if(q.first==q.second)
        {
            if(mp.count(q.first)) return 1.0;
            else return 0;
        }
        if(!mp.count(q.first) || !mp.count(q.second)) return 0;
        if(mp[q.first].count(q.second)) return mp[q.first][q.second];
        //both are in the mp, need find a path from a to b. Note if input (a,a) the map will cause infinite loop
        //since it has two direction, many will lead to a loop and causing infinite searching
        //double res=1.0;
        //use a hashset to avoid revisit
        
        string str=q.first;
        for(auto it=mp[str].begin();it!=mp[str].end();it++)
        {
            //if(it->first==str) continue;//this will not return a value
            if(!visited.count(it->first))
            {
                visited.insert(it->first);
                pair<string,string> p=make_pair(it->first,q.second);
                //cout<<it->first<<"/"<<q.second<<endl;
                double tmp=helper(p,mp,visited);
                if(abs(tmp)>1e-4) return it->second*tmp;//if get results return 
            }
            
        }
        return 0;
    }
```

547. Friend Circles
graph in matrix format

dfs or union find
dfs can use coloring without visited matrix
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
        for(int i=0;i<M.size();i++)
        {
            if(M[node][i] && !visited[i]) visited[i]=1,dfs(M,i,visited);
        }
    }
```

union find
```cpp
    int findCircleNum(vector<vector<int>>& M) {
        if (M.empty()) return 0;
        int n = M.size();

        vector<int> leads(n, 0);
        for (int i = 0; i < n; i++) { leads[i] = i; }   // initialize leads for every kid as themselves

        int groups = n;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {   // avoid recalculate M[i][j], M[j][i]
                if (M[i][j]) {
                    int lead1 = find(i, leads);
                    int lead2 = find(j, leads);
                    if (lead1 != lead2) {       // if 2 group belongs 2 different leads, merge 2 group to 1
                        leads[lead1] = lead2;
                        groups--;
                    }
                }
            }
        }
        return groups;
    }

    int find(int x, vector<int>& parents) {
        return parents[x] == x ? x : find(parents[x], parents);
    }
```	

684. Redundant Connection
```cpp
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n=edges.size();
        vector<int> parents(2*n);
        for(int i=0;i<2*n;i++) parents[i]=i;
        for(int i=0;i<n;i++)
        {
            int f=edges[i][0],t=edges[i][1];
            if(find(parents,f)==find(parents,t)) return edges[i];
            else parents[find(parents,f)]=find(parents,t);
        }
        return vector<int>(2);
    }
    int find(vector<int>& parent,int f)
    {
        while(f!=parent[f]) f=parent[f];
        return f;
    }
```

685. Redundant Connection II
directed graph

```cpp
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> parent(n+1, 0), candA, candB;
        // step 1, check whether there is a node with two parents
        for (auto &edge:edges) {
            if (parent[edge[1]] == 0)
                parent[edge[1]] = edge[0]; 
            else {
                candA = {parent[edge[1]], edge[1]};
                candB = edge;
                edge[1] = 0;
            }
        } 
        // step 2, union find
        for (int i = 1; i <= n; i++) parent[i] = i;
        for (auto &edge:edges) {
            if (edge[1] == 0) continue;
            int u = edge[0], v = edge[1], pu = root(parent, u);
            // Now every node only has 1 parent, so root of v is implicitly v
            if (pu == v) {
                if (candA.empty()) return edge;
                return candA;
            }
            parent[v] = pu;
        }
        return candB;
    }

    int root(vector<int>& parent, int k) {
        if (parent[k] != k) 
            parent[k] = root(parent, parent[k]);
        return parent[k];
    }
```
	
721. Accounts Merge	
```cpp
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        int n=accounts.size();
        //make the disjoint set using the index
        unordered_map<string,int> email_parent;
        vector<int> parent(n);
        for(int i=0;i<n;i++)
        {
            parent[i]=i; //initially it points itself
            for(int j=1;j<accounts[i].size();j++) 
            {
                if(email_parent.count(accounts[i][j])) //this email has appeared
                {
                    merge(parent,i,email_parent[accounts[i][j]]);
                }
                else email_parent[accounts[i][j]]=i;
            }
        }
        //now each set logical relation is formed using disjoint set
        //using set to remove duplicates and sort them
        unordered_map<int,set<string>> parent_email;
        for(auto it=email_parent.begin();it!=email_parent.end();it++)
        {
            int id=find(parent,it->second);
            parent_email[id].insert(it->first);
        }
        //form the final answer
        vector<vector<string>> ans;
        for(auto it=parent_email.begin();it!=parent_email.end();it++)
        {
            ans.push_back(vector<string>());
            ans.back().push_back(accounts[it->first][0]); //owner name
            ans.back().insert(ans.back().begin()+1,it->second.begin(),it->second.end());
        }
        return ans;
    }
    int find(vector<int>& parent,int i) //find its parent id
    {
        while(parent[i]!=i) i=parent[i];
        return i;
    }
    
    void merge(vector<int>& parent,int i,int j)
    {
        int i_id=find(parent,i);
        int j_id=find(parent,j);
        if(i_id==j_id) return;
        if(i_id<j_id) parent[j_id]=i_id;
        else parent[i_id]=j_id;
    }
```

765. Couples Holding Hands
greedy

```cpp
    int minSwapsCouples(vector<int>& row) {
        for(int i=0;i<row.size();i++) row[i]/=2; //making couple the same number
        int cnt=0;
        for(int i=0;i<row.size();i+=2)
        {
            if(row[i]!=row[i+1]) 
            {
                auto it=find(row.begin()+i+2,row.end(),row[i]);
                swap(*it,row[i+1]);
                cnt++;
            }
        }
        return cnt;
    }
```

Quick Union 0(n^2)
Can be optimized to O(Log n) in worst case.
https://stackoverflow.com/questions/43036204/what-is-the-time-complexity-of-quick-union

Several key points to know:

The first person in couple should seat in even position.
e.g. 0, 2 ,4, 6 ... If it starts from 1, no couples can hold hands at position 0.

If two couple already hold hands and satisfy 1), then seat exchange is not needed.
0 1 4 3 2 5
0 and 1 are couples and hold hands, swap is not needed.
Couples 2 and 3, 4 and 5 need to exchange seats

If exchange seats is needed, the best chance is 1 swap fix two broken couples. The worse case is 1 swap only fix 1 couple.
0 1 4 3 2 5 // swap 4 and 2 would both fix (2,3) and (4,5)
0 4 1 3 2 5 // swap 4 and 2 only fix (4,5)

Our job is to count how many swaps are needed! How does union find help? If we do the swap for two couples for the first time, we count the swap and put these two couples into 1 group. If we need to do the swap for the same two couples again, the first swap already fix these two, and we skip the count part.

```java
		 public int minSwapsCouples(int[] row) {
        int N = row.length / 2;
        int swap = 0;
				
        // initially we have N couples, each couple will have the same group identifier
        // index :              0 1 2 3 4 5 6 7 8 9
        // couple:              0 1 2 3 4 5 6 7 8 9
        // group identifier:    0 0 1 1 2 2 3 3 4 4 
        int[] groups = new int[row.length];
        for(int group = 0; group < N; group++) {
            groups[group*2] = group;
            groups[group*2+1] = group;            
        }
        
        // Since number of seats are even and all couples want to hold hands
        // if i is the group number, 0 <= i < N, each couple should seat at positions of 2*i and 2*i+1
        for(int group = 0; group < N; group++) {
            int person1 = row[2*group];
            int person2 = row[2*group+1];
            
            int couple1 = person1 / 2;
            int couple2 = person2 / 2;
                        
            if(couple1 != couple2) {  // if these two person are not couples (each person from different couples)
                
                // if we do swap people from two couples for the first time, this swap is counted
                // if we want to swap people from the same two couples for the second time, we don't need to
                // do anything since the first swap already exchange the 2 people to meet their love partners.
                if(findGroup(groups, person1) != findGroup(groups, person2)) {
                    swap++;
										// union couples into same group
                    union(groups, person1, person2);
								}
            }
        }
        
        return swap;
    }
    
    private int findGroup(int[] groups, int person) {
        return groups[person];
    }
    
    private void union(int[] groups, int person1, int person2) {
        int group = groups[person1];
        for(int i = 0; i < groups.length; i++) {
            if(groups[i] == group)
                groups[i] = groups[person2];
        }
    }
```

778. Swim in Rising Water
binary search
or union find
```cpp
    int swimInWater(vector<vector<int>>& grid) {
        const int DIRECTIONS[4][2] = {{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
        int n = grid.size();
        
        if (grid[n - 1][n - 1] >= n * n - 2) return grid[n - 1][n - 1];
        
        vector<int> connection(n * n); // position: group
        vector<int> elevations(n * n); // elevation: position
        
        // bucket sort and init connection
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                elevations[grid[i][j]] = i * n + j;
                connection[i * n + j] = i * n + j;
            }
        }
        
        // union find, e is elevation
        for (int e = 0; e < n * n; e++) {
            // find 4 directions of the point
            for (int i = 0; i < 4; i++) {
                int x = elevations[e] / n + DIRECTIONS[i][0],
                    y = elevations[e] % n + DIRECTIONS[i][1];
                
                if (x < 0 || y < 0 || x == n || y == n) continue;
								
                // find the adjust block which elevation is lower than the current one
                // means we can connect them
                if (grid[x][y] < e) {
                    connection[find(elevations[e], connection)] = find(x * n + y, connection);
                }
            }
            
            // check if the graph connected
            if (find(0, connection) == find(n * n - 1, connection)) {
                return e;
            }
        }
        
        return n * n - 1;
    }
    
    int find(int i, vector<int>& data) {
        while (data[i] != data[data[i]]) {
            data[i] = data[data[i]];
        }
        return data[i];
    }
```

803. Bricks Falling When Hit	
this is a long solution

```cpp
/*understand the problem is the key
only those bricks (set) which are connected to the top will not drop.
So we just change those unit to be hit to zero
then we form the disjoint set only the first row!
we add the bricks reversely back, the total size of the disjoint set difference is the number of bricks dropping.*/
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
        parent.resize(m*n+1);//the first row will connect to a virtual node n*m
        size.resize(m*n+1);
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                //if(grid[i][j]>0) //we don't need this since every node needs to be a set
                {parent[i*n+j]=i*n+j;size[i*n+j]=1;count++;}
            }
        }
        parent[n*m]=n*m;size[n*m]=1;//dummy node
        //for(int j=0;j<n;j++) if(grid[0][j]>0) {parent[j]=n*m;size[n*m]++;}
        if(count) max_size=1;//not used
        
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
        if(i_id==j_id) {return;}
        if(i_id>j_id) {parent[j_id]=i_id;size[i_id]+=size[j_id];size[j_id]=0;max_size=max(max_size,size[i_id]);}
        else {parent[i_id]=j_id;size[j_id]+=size[i_id];size[i_id]=0;max_size=max(max_size,size[j_id]);}
        count--;
    }
    bool connected(int ni,int nj) {return find(ni)==find(nj);}
    int get_numset() {return count;}
    int get_size(int p) {return size[p];}
    int get_maxsize() {return max_size;}
    void print_size() {copy(size.begin(),size.end(),ostream_iterator<int>(cout," "));cout<<endl;}
};

class Solution {
public:
    vector<int> hitBricks(vector<vector<int>>& grid, vector<vector<int>>& hits) {
        //mark all those hits, need to differentiate previous 0 or 1
        int m=grid.size(),n=grid[0].size();
        for(int i=0;i<hits.size();i++) 
            if(grid[hits[i][0]][hits[i][1]]) grid[hits[i][0]][hits[i][1]]=-1;
       
        //form the disjoint set using the first row
        union_find uf(grid);
        int dir[][2]={{-1,0},{1,0},{0,-1},{0,1}};
        for(int j=0;j<n;j++) if(grid[0][j]>0) uf.merge(j,m*n);
        for(int i=1;i<m;i++)
            for(int j=0;j<n;j++)
                if(grid[i][j]>0)
                {
                    for(int k=0;k<4;k++)
                    {
                        int x=i+dir[k][0],y=j+dir[k][1];
                        if(x>=0&&x<m&&y>=0&&y<n&&grid[x][y]>0) uf.merge(x*n+y,i*n+j);
                    }
                 }
        //uf.print_size();
        
        //add the hits back
        vector<int> ans(hits.size());
        for(int i=hits.size()-1;i>=0;i--)
        {
            int x0=hits[i][0],y0=hits[i][1];
            if(grid[x0][y0]<0)
            {
                int prev=uf.get_size(m*n);
                grid[x0][y0]=1;//restore it
                //cout<<i<<":"<<prev<<endl;
                //check if its connected to any
                for(int k=0;k<4;k++)
                {
                    int x=x0+dir[k][0],y=y0+dir[k][1];
                    if(x>=0&&x<m&&y>=0&&y<n&&grid[x][y]>0)
                    {
                        //cout<<"merge: "<<x<<" "<<y<<"; "<<x0<<" "<<y0<<endl;
                        uf.merge(x*n+y,x0*n+y0);
                        //if(x && x0) uf.merge(x*n+y,x0*n+y0);
                        //else {uf.merge(x*n+y,m*n);uf.merge(x0*n+y0,m*n);} //if itself has no neigboring but it connects to top, this will miss.
                        //uf.print_size();
                    }
                }
                if(!x0) uf.merge(x0*n+y0,m*n);//this is very important: since the node itself shall be attached to the n*m                
                
                //uf.print_size();
                int diff;
                if(uf.connected(x0*n+y0,m*n)) diff=uf.get_size(n*m)-prev-1;
                else diff=uf.get_size(n*m)-prev;
                
                ans[i]=diff;
            }
        }
        return ans;        
    }
};
```

839. Similar String Groups
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

924. Minimize Malware Spread
remove the node to minimize

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

928. Minimize Malware Spread II
remove the node completely

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

947. Most Stones Removed with Same Row or Column
```cpp
    int removeStones(vector<vector<int>>& stones) {
        //use disjoint set to find number of connected group
        //each group need has at least two nodes
        int n=stones.size();
        vector<int> parent(n);
        //vector<int> size(n,1);
        int nset=n;
        for(int i=0;i<n;i++) parent[i]=i;
        for(int i=0;i<n;i++)
        {
            for(int j=i+1;j<n;j++)
            {
                if((stones[i][0]==stones[j][0] || stones[i][1]==stones[j][1]))
                {
                    //cout<<"merge "<<i<<" "<<j<<endl;
                    union_find(i,j,nset,parent);
                }
            }
        }
        //copy(size.begin(),size.end(),ostream_iterator<int>(cout, " "));
        int ans=n-nset;
        //for(int i=0;i<n;i++) if(size[i] && size[i]<2) ans--;
        return ans;
            
    }
    void union_find(int i,int j,int& nset,vector<int>& parent)
    {
        int pi=find_parent(i,parent);
        int pj=find_parent(j,parent);
        if(pi<pj) {parent[pj]=pi;nset--;}
        if(pi>pj) {parent[pi]=pj;nset--;}
    }
    int find_parent(int i,vector<int>& parent)
    {
        while(i!=parent[i]) i=parent[i];
        return i;
    }
```

952. Largest Component Size by Common Factor
```cpp
    int largestComponentSize(vector<int>& A) {
        int n=A.size();
        sort(A.begin(),A.end());
        vector<int> parent(n),size(n,1);
        for(int i=0;i<n;i++) parent[i]=i;
        //vector<unordered_set<int>> factors(n);
        unordered_map<int,set<int>> factors;
        unordered_set<int> tset;
        //build the prime factors for each number
        for(int i=0;i<n;i++)
        {
            tset=factorize(A[i]);
            for(auto tt:tset) factors[tt].insert(i);
        }
        for(auto t:factors)
        {
            auto t0=t.second;
            if(t0.size()>=2)
            {
                int node=*t0.begin();
                for(auto it=++t0.begin();it!=t0.end();it++) merge(node,*it,parent,size);
            }
        }
        return *max_element(size.begin(),size.end());
    }
    int get_parent(int i,vector<int>&parent)
    {
        while(i!=parent[i]) i=parent[i];
        return i;
    }
    int merge(int i,int j,vector<int>& parent,vector<int>& size)
    {
        int pi=get_parent(i,parent);
        int pj=get_parent(j,parent);
        if(pi<pj) {parent[pj]=pi,size[pi]+=size[pj],size[pj]=0;}
        if(pi>pj) {parent[pi]=pj,size[pj]+=size[pi],size[pi]=0;}
        return min(pi,pj);
    }
    unordered_set<int> factorize(int x)
    {
        unordered_set<int> prime;
        int d=2;
        while(d*d<=x)
        {
            if(x%d==0) 
            {
                while(x%d==0) x/=d; //remove all its multiples
                prime.insert(d);
            }
            d++;
        }
        if(x>1 || prime.empty()) prime.insert(x); //if there is no factor, itself is a prime number
        return prime;
    }
```

959. Regions Cut By Slashes
most important upsample the grid
dfs or union find
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
	
990. Satisfiability of Equality Equations
```cpp
    bool equationsPossible(vector<string>& equations) {
       //using disjoint set
        vector<int> parent(26);
        for(int i=0;i<26;i++) parent[i]=i;
        for(int i=0;i<equations.size();i++)
        {
            if(equations[i][1]=='=')
            {
                int t0=equations[i][0]-'a',t1=equations[i][3]-'a';
                merge(t0,t1,parent);
            }
        }
        for(int i=0;i<equations.size();i++)
        {
            if(equations[i][1]=='!')
            {
                int t0=equations[i][0]-'a',t1=equations[i][3]-'a';
                if(get_parent(t0,parent)==get_parent(t1,parent)) return 0;
            }
        }
        return 1;
    }
    void merge(int i,int j,vector<int>& parent)
    {
        int pi=get_parent(i,parent);
        int pj=get_parent(j,parent);
        if(pi==pj) return;
        if(pi<pj) {parent[pj]=pi;}
        else {parent[pi]=pj;}
    }
    int get_parent(int i,vector<int>& parent)
    {
        while(parent[i]!=i) i=parent[i];
        return i;
    }
```	
