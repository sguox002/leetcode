shortest distance problem
Bellman Ford's algorithm is used to find the shortest paths from the source vertex to all other vertices in a weighted graph. It depends on the following concept: 
Shortest path contains at most n-1 edges, 
because the shortest path couldn't have a cycle.
Algorithm Steps:

The outer loop traverses from  : .
Loop over all edges, check if the next node distance > current node distance + edge weight, in this case update the next node distance to "current node distance + edge weight".
This algorithm depends on the relaxation principle where the shortest distance for all vertices is gradually replaced by more accurate values until eventually reaching the optimum solution. In the beginning all vertices have a distance of "Infinity", but only the distance of the source vertex = , then update all the connected vertices with the new distances (source vertex distance + edge weights), then apply the same concept for the new vertices with new distances and so on.

complexity O(EV) is pretty high.

Dijkstra's algorithm:
most commonly used from source node to all other nodes
- Set all vertices distances = infinity except for the source vertex, set the source distance = .
- Push the source vertex in a min-priority queue in the form (distance , vertex), as the comparison in the min-priority queue will be according to vertices distances.
- Pop the vertex with the minimum distance from the priority queue (at first the popped vertex = source).
- Update the distances of the connected vertices to the popped vertex in case of "current vertex distance + edge weight < next vertex distance", then push the vertex
with the new distance to the priority queue.
- If the popped vertex is visited before, just continue without using it.
- Apply the same algorithm again until the priority queue is empty.

complexity: O(N^2), with min heap it is O(N+ElogN)

if we want to find all pairs shortest distance, bellman-ford and dijkstra are both costly.

Floyd-Warshall's algorithm
find all pairs shortest distance using dp principle

for(int k = 1; k <= n; k++){
    for(int i = 1; i <= n; i++){
        for(int j = 1; j <= n; j++){
            dist[i][j] = min( dist[i][j], dist[i][k] + dist[k][j] );
        }
    }
}


787. Cheapest Flights Within K Stops
problem: 
given a list of edges from src to dest with cost. 
find the min cost using at most K stops

classification:
weighted edges, find the shortest distance to another node with k steps
note: one stop means two edges.

- bfs approach
try all paths from src to destination and get the min prices
- adjacent matrix is more convenient, but with weight, using map
- queue store the src and cost so far
```cpp
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        //bfs approach: from src to dst all the paths with k+1 steps, get the min costs
        unordered_map<int,unordered_map<int,int>> adj; //src vs (dst vs cost)
        for(auto f: flights)
            adj[f[0]][f[1]]=f[2];
        queue<vector<int>> q;
        q.push({src,0});
        int step=0;
        K++;
        int ans=INT_MAX;
        while(q.size()){
            int sz=q.size();
            while(sz--){
                auto t=q.front();
                q.pop();
                int src=t[0],cost=t[1];
                if(src==dst){
                    ans=min(ans,cost);
                }
                for(auto p: adj[src]){
                    if(p.second+cost<=ans) //no need to try higher price path
                        q.push({p.first,cost+p.second});
                }
            }
            step++;
            if(step>K) break;
        }
        return ans==INT_MAX?-1:ans;
    }
```
24ms with 14.4MB memory
we are trying layer by layer with the cost accumulated and find all the paths with <=K stops and get the min
important: all paths with cost > current min are all discard, which is quite normal practice.
complexity: 

- dijkstra algorithm
using pq to try the smallest edge first, once we find the destination, then we are done
since pq shall be sorted using cost, we reorganize the data structure
```cpp    
	int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        unordered_map<int,unordered_map<int,int>> mp;
        for(const vector<int>& flight : flights)   mp[flight[0]][flight[1]] = flight[2];
        priority_queue<vector<int>,vector<vector<int>>,greater<vector<int>>> minheap;
        minheap.push({0,src,K+1});
        while(!minheap.empty()){
            vector<int> top = minheap.top();
            minheap.pop();
            int price = top[0];
            int city = top[1];
            int stops = top[2];
            if (city == dst) return price;
            if(stops>0) 
                for(auto &t: mp[city] ) 
                    minheap.push({price+t.second, t.first, stops-1});
            }
        return -1;   
    }
```
complexity: O(KlogE)
48ms with 18Mb

- bellman-ford or dp
bellman ford is very close to dp:
```cpp
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        //shotest distance from one node to dest with k edges at most
        K++;
        vector<vector<int>> dp(K+1,vector<int>(n,INT_MAX));
        for(int i=0;i<=K;i++) dp[i][src]=0;
        for(int i=1;i<=K;i++){
            for(int j=0;j<n;j++)
                dp[i][j]=dp[i-1][j];
            for(auto f: flights){
                int src=f[0],dst=f[1],cost=f[2];
                //check if we can use this edge
                if(dp[i-1][src]<INT_MAX){
                    dp[i][dst]=min(dp[i][dst],dp[i-1][src]+cost);
                }
            }
        }
        return dp[K][dst]==INT_MAX?-1:dp[K][dst];
    }
```
80ms with 27MB
 reduce to 1d dp/bellman-ford
     int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        //bellman-ford
        vector<int> dp(n,INT_MAX);
        dp[src]=0;
        for(int k=0;k<=K;k++){
            vector<int> cur=dp;
            for(auto f: flights){
                int src=f[0],dst=f[1],cost=f[2];
                if(dp[src]<INT_MAX)
                    cur[dst]=min(cur[dst],dp[src]+cost);
            }
            dp=cur;
        }
        return dp[dst]==INT_MAX?-1:dp[dst];
    }
complexity O(KE)
	68ms with 27MB

317. Shortest Distance from All Buildings
find an empty space which the sum of distance to all building is minimized.
edges are not weighted.

- bfs
start from each building and get the shortest distance and accumulate and find the smallest
need to remove those not reachable, so using a map
this is not that hard.
- first bfs we get a distance map 
- next time, we get a new map and need to merge with previous (only leave common)
- find the shortest

```cpp
    int shortestDistance(vector<vector<int>>& grid) {
        int m=grid.size(),n=grid[0].size();
        vector<int> vbuild;
        map<int,int> dist;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==1) vbuild.push_back(i*n+j);
                //if(grid[i][j]==0) dist[i*n+j]=0;
            }
        }
        int cnt=0;
        for(int pos: vbuild){
            bfs(grid,pos,dist,cnt++);
        }
        if(dist.empty()) return -1;
        int ans=INT_MAX;
        for(auto t: dist) ans=min(ans,t.second);
        return ans;
    }
    void bfs(vector<vector<int>>& g,int pos,map<int,int>& dist,int ini){
        int m=g.size(),n=g[0].size();
        queue<int> q;
        vector<bool> v(m*n);
        map<int,int> mp;
        q.push(pos),v[pos]=1;
        
        int step=0;
        int dir[][2]={{-1,0},{1,0},{0,-1},{0,1}};
        while(q.size()){
            int sz=q.size();
            while(sz--){
                int p=q.front();
                q.pop();
                //cout<<p<<" ";
                int x0=p/n,y0=p%n;
                if(g[x0][y0]==0){
                    if(ini==0) dist[p]=step;
                    else {
                        if(dist.count(p)) mp[p]=step;
                        else continue; //not in previous set, no need consider it.
                    }
                }
                for(int d=0;d<4;d++){
                    int x=x0+dir[d][0],y=y0+dir[d][1];
                    if(x<0||y<0||x>=m||y>=n||v[x*n+y]||g[x][y]) continue;
                    q.push(x*n+y);
                    v[x*n+y]=1;
                }
            }
            step++;
        }
        if(ini){
            for(auto& t: mp) t.second+=dist[t.first];
            dist=mp;
        }
    }
```	

847	 Shortest Path Visiting All Nodes	47.8%	Hard
problem: N nodes, and you can start at any node.  you can resuse edges nodes
return the shortest distance visiting all nodes
classification: edge is not weighted, so we can use bfs with queue
visiting all nodes can use bits.
get the min: use dp to relax the distance
dp with visiting n nodes and status.
- first layer all the nodes with each node bit is set
- try to one layer and one layer to expand and reduce the distance using the edges
```cpp
    struct State{
        int mask,source;
        State(int m,int s):mask(m),source(s){}
    };
    int shortestPathLength(vector<vector<int>>& graph) {
        int m=graph.size();
        int len=1<<m;
        vector<vector<int>> dp(m,vector<int>(len,INT_MAX));
        queue<State> qs;
        for(int i=0;i<m;i++) 
        {
            dp[i][1<<i]=0; //self to self distance is 0
            qs.push(State(1<<i,i));
        }
        while(!qs.empty())
        {
            State state=qs.front();
            //cout<<state.source<<" "<<hex<<state.mask<<endl;
            qs.pop();
            for(int next:graph[state.source]) //connected nodes
            {
                int nextmask=state.mask|(1<<next);
                if(dp[next][nextmask]>dp[state.source][state.mask]+1) //passing this node is closer
                {
                    dp[next][nextmask]=dp[state.source][state.mask]+1;
                    qs.push(State(nextmask,next));
                }
            }
        }
        //shortest path 
        int ans=INT_MAX;
        for(int i=0;i<m;i++) ans=min(ans,dp[i][(1<<m)-1]);
        return ans;
    }
```
743. Network Delay Time
problem: from one node to all other nodes, get all the shortest distance and then choose the max
classifications: weighted edges and need all shortest distance (all pairs)
so it is a multiple dijkstra problem.
we try from source node and choose the shortest edge and if we found the node, that node is done
try other nodes....

dijkatra with min heapcheck
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

campus bike II
to find the min distance sum from M people to N bikes
classification: weighted edges, visiting all nodes (people) with edges, each once
typical traveller's problem
using dp:
m people, with status 2^n.
loop over subproblems
	loop over status
		loop over bikes.
		

```cpp
    int assignBikes(vector<vector<int>>& workers, vector<vector<int>>& bikes) {
        int m=workers.size(),n=bikes.size();
        vector<vector<int>> dist(m,vector<int>(n));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++)
                dist[i][j]=abs(workers[i][0]-bikes[j][0])+abs(workers[i][1]-bikes[j][1]);
        }
        //seeking a min path sum in the matrix
        vector<vector<int>> dp(m+1,vector<int>(1<<n,INT_MAX));
        dp[0][0]=0;
        int ans=INT_MAX;
        for(int i=1;i<=m;i++){
            for(int s=1;s<(1<<n);s++){
                for(int j=0;j<n;j++){
                    if(s&(1<<j)==0) continue;
                    int prev=s^(1<<j);
                    if(dp[i-1][prev]<INT_MAX)
                        dp[i][s]=min(dp[i][s],dp[i-1][prev]+dist[i-1][j]);
                    if(i==m && nbits(s)==m) ans=min(ans,dp[i][s]);
                }
            }
        }
        return ans;
    }
    int nbits(int n){
        int ans=0;
        while(n){
            ans+=n&1;
            n>>=1;
        }
        return ans;
    }
```	

864. shortest path to get all keys
classification: edge is not weighted. so bfs with queue could be possible
similar to traveller's problem we need a status 2^n.
- find the start position and number of keys
- visited is tricky, we need bind the status with the node, since we may revisit the node using different path
- once we reach status all 1, we get the answer
- do not modify the status since it will be used by the for loop

```cpp
    int shortestPathAllKeys(vector<string>& grid) {
        if(grid.empty()) return 0;
        int m=grid.size(),n=grid[0].size();
        //get the start point first
        int sx,sy,nkeys=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]=='@') {
                    sx=i,sy=j;
                }
                if(islower(grid[i][j])) nkeys++;
            }
        }
        int k=1<<nkeys; //the status
        queue<vector<int>> q;
        unordered_set<string> v;
        q.push({sx,sy,0});//
        v.insert(to_string(sx*n+sy)+","+to_string(0));
        int steps=0;
        int dir[][2]={{1,0},{-1,0},{0,-1},{0,1}};
        while(q.size()){
            int sz=q.size();
            while(sz--){
                auto t=q.front();
                q.pop();
                int status=t[2];
                
                if(status==k-1) return steps;
                int x0=t[0],y0=t[1];
                
                for(int d=0;d<4;d++){
                    int newstatus=status;
                    int x=x0+dir[d][0],y=y0+dir[d][1];
                    string s=to_string(x*n+y)+","+to_string(newstatus);
                    if(x<0||y<0||x>=m||y>=n||v.count(s)||grid[x][y]=='#') continue;
                    char c=grid[x][y];
                    
                    if(c>='A' && c<='F'){
                        int key=c-'A';
                        if((newstatus&(1<<key))==0) continue; //if there is no key
                    }
                    if(c>='a'&&c<='f'){
                        int key=c-'a';
                        newstatus|=(1<<key);
                    }
                    q.push({x,y,newstatus});
                    s=to_string(x*n+y)+","+to_string(newstatus);
                    v.insert(s);
                }
            }
            steps++;
        }
        return -1;
    }
```	




	

	


