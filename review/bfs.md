BFS<br/>
bfs uses queue. when the stack using dfs is too deep, bfs could overcome the problem<br/>
bfs is most suitable for shortest distance between two nodes<br/>
<br/>

## contents
559	Maximum Depth of N-ary Tree		Easy	<br/>
429	N-ary Tree Level Order Traversal		Easy	<br/>
690	Employee Importance		Easy	<br/>
993	Cousins in Binary Tree		Easy	<br/>
107	Binary Tree Level Order Traversal II		Easy	<br/>
994	Rotting Oranges		Easy	<br/>
101	Symmetric Tree		Easy	<br/>
111	Minimum Depth of Binary Tree		Easy	<br/>
<br/>
<br/>
513	Find Bottom Left Tree Value		Medium	<br/>
515	Find Largest Value in Each Tree Row		Medium	<br/>
529	Minesweeper		Medium	<br/>
323	Number of Connected Components in an Undirected Graph 	Medium	<br/>
286	Walls and Gates 	Medium	<br/>
102	Binary Tree Level Order Traversal		Medium	<br/>
199	Binary Tree Right Side View		Medium	<br/>
490	The Maze 	Medium	<br/>
863	All Nodes Distance K in Binary Tree		Medium	<br/>
752	Open the Lock		Medium	<br/>
934	Shortest Bridge		Medium	<br/>
505	The Maze II 	Medium	<br/>
785	Is Graph Bipartite?		Medium	<br/>
279	Perfect Squares		Medium	<br/>
743	Network Delay Time		Medium	<br/>
103	Binary Tree Zigzag Level Order Traversal		Medium	<br/>
200	Number of Islands		Medium	<br/>
261	Graph Valid Tree 	Medium	<br/>
207	Course Schedule		Medium	<br/>
417	Pacific Atlantic Water Flow		Medium	<br/>
542	01 Matrix		Medium	<br/>
210	Course Schedule II		Medium	<br/>
787	Cheapest Flights Within K Stops		Medium	<br/>
909	Snakes and Ladders		Medium	<br/>
310	Minimum Height Trees		Medium	<br/>
133	Clone Graph		Medium	<br/>
127	Word Ladder		Medium	<br/>
130	Surrounded Regions		Medium	<br/>
<br/>
773	Sliding Puzzle		Hard	<br/>
847	Shortest Path Visiting All Nodes		Hard	<br/>
815	Bus Routes		Hard	<br/>
301	Remove Invalid Parentheses		Hard	<br/>
407	Trapping Rain Water II		Hard	<br/>
1036 Escape a Large Maze		Hard	<br/>
317	Shortest Distance from All Buildings 	Hard	<br/>
499	The Maze III 	Hard	<br/>
864	Shortest Path to Get All Keys		Hard	<br/>
854	K-Similar Strings		Hard	<br/>
675	Cut Off Trees for Golf Event		Hard	<br/>
913	Cat and Mouse		Hard	<br/>
126	Word Ladder II		Hard<br/>

## easy
559	Maximum Depth of N-ary Tree		Easy	<br/>
see tree
429	N-ary Tree Level Order Traversal		Easy	<br/>
see tree
690	Employee Importance		Easy	<br/>
see dfs
993	Cousins in Binary Tree		Easy	<br/>
see tree
107	Binary Tree Level Order Traversal II		Easy	<br/>
see tree
994	Rotting Oranges		Easy	<br/>
simple, one step bfs
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

101	Symmetric Tree		Easy	<br/>
see tree
111	Minimum Depth of Binary Tree		Easy	<br/>
see tree

## medium
513	Find Bottom Left Tree Value		Medium	<br/>
see tree
515	Find Largest Value in Each Tree Row		Medium	<br/>
see tree
529	Minesweeper		Medium	<br/>
see dfs
323	Number of Connected Components in an Undirected Graph 	Medium	<br/>
locked. bfs or union-find
286	Walls and Gates 	Medium	<br/>
locked
102	Binary Tree Level Order Traversal		Medium	<br/>
see tree
199	Binary Tree Right Side View		Medium	<br/>
see tree
490	The Maze 	Medium	<br/>
locked
863	All Nodes Distance K in Binary Tree		Medium	<br/>
see tree
752	Open the Lock		Medium	<br/>
You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'. The wheels can rotate freely and wrap around: for example we can turn '9' to be '0', or '0' to be '9'. Each move consists of turning one wheel one slot.

The lock initially starts at '0000', a string representing the state of the 4 wheels.

You are given a list of deadends dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a target representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible.

Example 1:
Input: deadends = ["0201","0101","0102","1212","2002"], target = "0202"
Output: 6
Explanation:
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels of the lock become stuck after the display becomes the dead end "0102

Approach: each digit can add 1 or -1. use bfs to reach the target and skip those deadends.
```cpp
    int openLock(vector<string>& deadends, string target) {
        unordered_set<string> dead(deadends.begin(),deadends.end());
        //scr='0000'to target
        //using dfs or bfs searching
        //each char has two option: +1 or -1
        if(dead.count(target) ||dead.count("0000")) return -1;
        if(target=="0000") return 0;
        return bfs(target,dead);
    }

    int bfs(string target,unordered_set<string>& dead)
    {
        queue<string> q;
        q.push("0000");
        unordered_set<string> visited;
        visited.insert("0000");
        int level=0;
        while(!q.empty())
        {
            int sz=q.size();
            for(int i=0;i<sz;i++)
            {
                string s=q.front();q.pop();
                //add its 8 neighbors to the queue
                for(int j=0;j<4;j++)
                {
                    string t=s;
                    int c;
                    c=s[j]-'0';
                    t[j]=(c+1)%10+'0';
                    if(dead.count(t) || visited.count(t)) {}
                    else
                    {
                        if(t==target) return level+1;
                        q.push(t);visited.insert(t);
                    }
                    t[j]=(c-1+10)%10+'0';
                    if(dead.count(t) || visited.count(t)) continue;
                    if(t==target) return level+1;
                    q.push(t);visited.insert(t);//}
                }
            }
            level++;
        }
        return -1;
    }
```	
934	Shortest Bridge		Medium	<br/>
see dfs.

505	The Maze II 	Medium	<br/>
locked

785	Is Graph Bipartite?		Medium	<br/>
see dfs 
279	Perfect Squares		Medium	<br/>
see dp for knapsack

743	Network Delay Time		Medium	<br/>
There are N network nodes, labelled 1 to N.

Given times, a list of travel times as directed edges times[i] = (u, v, w), where u is the source node, v is the target node, and w is the time it takes for a signal to travel from source to target.

Now, we send a signal from a certain node K. How long will it take for all nodes to receive the signal? If it is impossible, return -1.

bellman-ford algorithm
```cpp
    //Bellman Ford algorithm
    int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        vector<int> dist(N + 1, INT_MAX);
        dist[K] = 0;
        for (int i = 0; i < N; i++) 
        {
            //for (vector<int> e : times) 
            for(int j=0;j<times.size();j++)
            {
                vector<int>&  e=times[j];
                int u = e[0], v = e[1], w = e[2];
                if (dist[u] != INT_MAX && dist[v] > dist[u] + w) 
                {
                    dist[v] = dist[u] + w;
                }
            }
        }

        int maxwait = 0;
        for (int i = 1; i <= N; i++)
            maxwait = max(maxwait, dist[i]);
        return maxwait == INT_MAX ? -1 : maxwait;
    }
```
dijkstra algorithm
```cpp
    struct Node
    {
        int node,dist;
        Node(int n,int d):node(n),dist(d){}
    };
    struct comp
    {
        bool operator()(const Node&a,const Node&b) {return a.dist<b.dist|| ((a.dist==b.dist) && (a.node<b.node));}
        //bool operator()(const Node&a,const Node&b) {return a.dist<b.dist;}//|| ((a.dist==b.dist) && (a.node<b.node));}
    };
    int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        //dijkstra algorithm using binary heap priority-queue
        //first build the adjacent matrix
        vector<vector<Node>> adj(N+1);//add an extra just for convenience
        for(int i=0;i<times.size();i++) adj[times[i][0]].push_back(Node(times[i][1],times[i][2]));
        multiset<Node,comp> mset;//unknown region
        vector<int> dist(N+1,INT_MAX);
        dist[K]=0;dist[0]=0;
        //for(int i=0;i<adj[K].size();i++) {dist[adj[K][i].node]=adj[K][i].dist;mset.insert(adj[K][i]);}
        for(int i=1;i<=N;i++) mset.insert(Node(i,dist[i]));
        //print(mset);
        while(mset.size())
        {
            Node nd=*mset.begin();
            int u=nd.node;
            mset.erase(mset.begin());
            for(int i=0;i<adj[u].size();i++) //relax all the distance
            {
                int v=adj[u][i].node;
                int d=adj[u][i].dist;
                if(dist[v]>(long long)dist[u]+d)
                {
                    auto it=mset.find(Node(v,dist[v]));
                    dist[v]=dist[u]+d;
                    if(it!=mset.end())
                    {
                        mset.erase(it);
                        mset.insert(Node(v,dist[v]));
                    }
                }
            }
        }
        //copy(dist.begin(),dist.end(),ostream_iterator<int>(cout," "));
        int res=*max_element(dist.begin()+1,dist.end());
        return res==INT_MAX?-1:res;
    }
```
dijkstra algorithm usually uses a priority_queue

103	Binary Tree Zigzag Level Order Traversal		Medium	<br/>
see tree
200	Number of Islands		Medium	<br/>
see union-find

261	Graph Valid Tree 	Medium	<br/>
locked

207	Course Schedule		Medium	<br/>
dfs with three states to detect a cycle. 
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
	

417	Pacific Atlantic Water Flow		Medium	<br/>
see dfs with bitset status

542	01 Matrix		Medium	<br/>
find the distance to nearest 0

bfs
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
                    if(x>=0 && x<m && y>=0 && y<n && ans[x][y]>0) 
						min_val=min(min_val,ans[x][y]);
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
210	Course Schedule II		Medium	<br/>
return the ordering of the courses
approach 1:
first, generate the graph adjacency matrix
second, scan for source nodes (no incoming edges), and remove these source nodes 
repeat until there are no source nodes.

```cpp
    vector<int> findOrder(int numCourses, vector<pair<int, int>>& prerequisites) {
        //all edges shall be visited and only once
        unordered_map<int,unordered_set<int>> v(numCourses);
        
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
787	Cheapest Flights Within K Stops		Medium	<br/>
dijkstra, bellman-ford, bfs, or dp
```cpp
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K)
    {
        K++;
        vector<vector<int>> dp(n, vector<int>(K+1,INT_MAX));
        dp[src][0] = 0;
        for(int i = 1; i <= K; i++)
        {
            for(int j = 0; j < n; j++)   //To update ans[j][i](using i steps), we copy ans[j][i-1] first
                dp[j][i] = dp[j][i-1];
            for(const vector<int>& f: flights) //iterate all flights: f[0]: from, f[1]: to, f[2], price
            {
                if(dp[f[0]][i-1]<INT_MAX)
                dp[f[1]][i] = min(dp[f[1]][i], dp[f[0]][i-1] + f[2]);
            }
        }
        if(dp[dst][K] == INT_MAX) return -1;
        return dp[dst][K];
    }
```	
909	Snakes and Ladders		Medium	<br/>

310	Minimum Height Trees		Medium	<br/>
see tree
133	Clone Graph		Medium	<br/>
see hashtable

127	Word Ladder		Medium	<br/>
from start, each move just change one char.
return the shortest move.

```cpp
    int ladderLength(string beginWord, string endWord, vector<string>& Dict) {
        unordered_set<string> wordDict(Dict.begin(),Dict.end());
        //wordDict.insert(endWord);//make sure the endword is in the set
        if(wordDict.count(endWord)==0) return 0;
        queue<string> toVisit;
        addNextWords(beginWord, wordDict, toVisit);
        int dist = 2;
        while (!toVisit.empty()) 
        {
            int num = toVisit.size();
            for (int i = 0; i < num; i++) 
            {
                string word = toVisit.front();toVisit.pop();
                if (word == endWord) return dist;
                addNextWords(word, wordDict, toVisit);
            }
            dist++;
        }
        return 0;
    }
    void addNextWords(string word, unordered_set<string>& wordDict, queue<string>& toVisit) {
        if(wordDict.count(word)) wordDict.erase(word);
        for (int p = 0; p < (int)word.length(); p++) 
        {
            char letter = word[p];
            for (char k = 'a'; k <'z'; k++) 
            { 
                word[p] = k;
                if (wordDict.count(word)) 
                {
                    toVisit.push(word);
                    wordDict.erase(word);
                }
            }
            word[p] = letter;//restore the word for next char change
        } 
    } 
```	
130	Surrounded Regions		Medium	<br/>
see union-find


## hard
773	Sliding Puzzle		Hard	<br/>
hua-rong-dao game
return the least number of moves to solve the puzzle.

Consider each state in the board as a graph node, we just need to find out the min distance between start node and final target node "123450". 
Since it's a single point to single point questions, Dijkstra is not needed here. 
We can simply use BFS, and also count the level we passed. 
Every time we swap 0 position in the String to find the next state. 
Use a hashTable to store the visited states.

```cpp
    int slidingPuzzle(vector<vector<int>>& board) {
        //convert the board to string and using bfs
        string s;
        for(int i=0;i<board.size();i++)
            for(int j=0;j<board[i].size();j++) s+=board[i][j]+'0';
        string target="123450";
        if(s==target) return 0;
		// all the positions 0 can be swapped to at posion 0,1,2,3,4,5
        vector<vector<int>> dir{{1,3},{0,2,4},{1,5},{0,4},{1,3,5},{2,4}};;
        //we need keep track the zero position and try all possible move, or use search
        //not sure if there are repeatable 
        queue<string> q;
        unordered_set<string> visited;
        q.push(s);visited.insert(s);
        int step=0;
        while(!q.empty())
        {
            int sz=q.size();
            for(int i=0;i<sz;i++)
            {
                string t=q.front();q.pop();
                int ind0=t.find_first_of('0');
                for(int j=0;j<dir[ind0].size();j++)
                {
                    string tt=t;
                    swap(tt[ind0],tt[dir[ind0][j]]);
                    if(tt==target) return step+1;
                    if(visited.count(tt)==0) {q.push(tt);visited.insert(tt);}
                }
            }
            step++;
        }
        return -1;
    }
```	
847	Shortest Path Visiting All Nodes		Hard	<br/>
An undirected, connected graph of N nodes (labeled 0, 1, 2, ..., N-1) is given as graph.

graph.length = N, and j != i is in the list graph[i] exactly once, if and only if nodes i and j are connected.

Return the length of the shortest path that visits every node. You may start and stop at any node, you may revisit nodes multiple times, and you may reuse edges.

```cpp
    struct State{
        int mask,source;
        State(int m,int s):mask(m),source(s){}
    };
    int shortestPathLength(vector<vector<int>>& graph) {
        int m=graph.size();
        int len=1<<m;
        vector<vector<int>> dp(m,vector<int>(len,m*m));
        queue<State> qs;
        for(int i=0;i<m;i++) 
        {
            dp[i][1<<i]=0; //self to self distance is 0
            qs.push(State(1<<i,i));
        }
        while(!qs.empty()) //until no node is in the queue, which means no node can make it closer
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

815	Bus Routes		Hard	<br/>
We have a list of bus routes. Each routes[i] is a bus route that the i-th bus repeats forever. For example if routes[0] = [1, 5, 7], this means that the first bus (0-th indexed) travels in the sequence 1->5->7->1->5->7->1->... forever.

We start at bus stop S (initially not on a bus), and we want to go to bus stop T. Travelling by buses only, what is the least number of buses we must take to reach our destination? Return -1 if it is not possible.

```cpp
    int numBusesToDestination(vector<vector<int>>& routes, int S, int T) {
        //this is a BFS problem
        //need convert the data structure to adjacency data structure
        if(S==T) return 0;
        unordered_map<int,vector<int>> mp; //each node has a group number
        for(int i=0;i<routes.size();i++)
        {
            for(int j=0;j<routes[i].size();j++) mp[routes[i][j]].push_back(i);
        }
        
        //we first generate a group connection map: group number to a set of other group
        vector<set<int>> connection_mp(routes.size());
        for(auto it=mp.begin();it!=mp.end();it++) 
        {
            if(it->second.size()>1)
            {
                for(int i=0;i<it->second.size();i++)
                {
                    connection_mp[it->second[i]].insert(it->second.begin(),it->second.end());//=it->second; //include itself for simplicity
                }
            }
        }
        //print(connection_mp);
        //cout<<"S: ";copy(mp[S].begin(),mp[S].end(),ostream_iterator<int>(cout," "));cout<<endl;
        //cout<<"T: ";copy(mp[T].begin(),mp[T].end(),ostream_iterator<int>(cout," "));cout<<endl;
        //S and T could be on one or more groups!
        int min_jmp=INT_MAX;
        for(auto it=mp[S].begin();it!=mp[S].end();it++)
        {
            for(auto it1=mp[T].begin();it1!=mp[T].end();it1++)
            {
                int jmp=min_steps(*it,*it1,connection_mp);
                min_jmp=min(min_jmp,jmp);
            }
        }
        if(min_jmp==INT_MAX) return -1;
        return min_jmp;
    }

    int min_steps(int s,int t,vector<set<int>>& mp)
    {
        //using BFS to get the shortest jmp
        int ans=INT_MAX;
        set<int> visited;
        queue<int> q;
        q.push(s);//visited.insert(s);
        int steps=0;
        while(!q.empty())
        {
            int sz=q.size();
            steps++;
            for(int i=0;i<sz;i++) //the layer
            {
                int tp=q.front();q.pop();
                if(visited.count(tp)) continue;
                if(tp==t) return steps;
                visited.insert(tp);
                for(auto it=mp[tp].begin();it!=mp[tp].end();it++)
                {
                    if(*it==tp) continue; //itself
                    q.push(*it);
                    
                }
            }
            
        }
        return ans;
    }
```	

301	Remove Invalid Parentheses		Hard	<br/>
Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.
approach 1: dfs, from left and right and from right to left.
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
407	Trapping Rain Water II		Hard	<br/>
2d problem
Given an m x n matrix of positive integers representing the height of each unit cell in a 2D elevation map, compute the volume of water it is able to trap after raining.

bfs
```cpp
    int trapRainWater(vector<vector<int>>& heightMap) {
        typedef pair<int,int> cell;
        priority_queue<cell, vector<cell>, greater<cell>> q;
        int m = heightMap.size();
        if (m == 0) return 0;
        int n = heightMap[0].size();
        vector<int> visited(m*n, 0);
        
        for (int i = 0; i < m; ++i)
        for (int j = 0; j < n; ++j) 
        {
            if (i == 0 || i == m-1 || j == 0  || j == n-1) 
            {
                if (!visited[i*n+j]) q.push(cell(heightMap[i][j], i*n+j));
                visited[i*n+j] = 1;
            }
        }
        
        int dir[4][2] = {{0,1}, {0, -1}, {1, 0}, {-1, 0}};
        int ans = 0;
        while(!q.empty()) 
        {
            cell c = q.top();q.pop();
            int i = c.second/n, j = c.second%n;
            
            for (int r = 0; r < 4; ++r) 
            {
                int ii = i+dir[r][0], jj = j+dir[r][1];
                if (ii < 0 || ii >= m || jj < 0 || jj >= n || visited[ii*n+jj]) continue;
                ans += max(0, c.first - heightMap[ii][jj]);
                q.push(cell(max(c.first, heightMap[ii][jj]), ii*n+jj));
                visited[ii*n+jj] = true;
            }
        }
        return ans;
    }
```
	
1036 Escape a Large Maze		Hard	<br/>
dfs using radius

```cpp
    long n=1e6;
    int max_steps=20000;
    int sx,sy;
    bool isEscapePossible(vector<vector<int>>& blocked, vector<int>& source, vector<int>& target) {
        unordered_set<long> obs;
        int steps=0;
        for(auto t: blocked) obs.insert(t[0]*n+t[1]);
        unordered_set<long> visited;
        bool res=dfs(obs,sx=source[0],sy=source[1],target[0],target[1],steps,visited);
        steps=0;visited.clear();
        res=res&&dfs(obs,sx=target[0],sy=target[1],source[0],source[1],steps,visited);
        return res;
    }
    bool dfs(unordered_set<long>& obs,int i,int j,int x,int y,int& steps,unordered_set<long>& visited)
    {
      if(i<0 || j<0 || i>=n || j>=n) return 0;
      long t=i*n+j;
      if(obs.count(t) || visited.count(t)) return 0;
      if(i==x && j==y) return 1;
      //if(steps>=max_steps) return 1;
      if(abs(i-sx)+abs(j-sy)>obs.size()) return 1;
      visited.insert(t);
      steps++;
      
      bool res=dfs(obs,i-1,j,x,y,steps,visited);
      if(!res) res=dfs(obs,i+1,j,x,y,steps,visited);
      if(!res) res=dfs(obs,i,j-1,x,y,steps,visited);
      if(!res) res=dfs(obs,i,j+1,x,y,steps,visited);
      return res;
    }
```
	
317	Shortest Distance from All Buildings 	Hard	<br/>
locked
499	The Maze III 	Hard	<br/>
locked

864	Shortest Path to Get All Keys		Hard	<br/>
854	K-Similar Strings		Hard	<br/>
675	Cut Off Trees for Golf Event		Hard	<br/>
```cpp
    int cutOffTree(vector<vector<int>>& forest) {
        vector<vector<int>> trees;
        int m=forest.size(),n=forest[0].size();
        for(int i=0;i<m;i++)
            for(int j=0;j<n;j++)
                if(forest[i][j]>1) trees.push_back({forest[i][j],i,j});
        sort(trees.begin(),trees.end());
        int curx=0,cury=0;
        int ans=0;
        
        for(int i=0;i<trees.size();i++)
        {
            int steps=bfs(forest,curx,cury,trees[i][1],trees[i][2]);
            //cout<<curx<<" "<<cury<<"->"<<trees[i][1]<<" "<<trees[i][2]<<": "<<steps<<endl;
            if(steps<0) return -1;
            ans+=steps;
            curx=trees[i][1],cury=trees[i][2];
            forest[curx][cury]=1;
        }
        return ans;
    }
    
    int bfs(vector<vector<int>>& forest,int sx,int sy,int tx,int ty)
    {
        if(sx==tx && sy==ty) return 0;
        int m=forest.size(),n=forest[0].size();
        queue<int> q;
        vector<bool> visited(m*n);
        q.push(sx*n+sy);
        visited[sx*n+sy]=1;
        int steps=0;
        int dir[][2]={{-1,0},{1,0},{0,1},{0,-1}};
        while(!q.empty())
        {
            int sz=q.size();
            for(int i=0;i<sz;i++)
            {
                int pos=q.front();q.pop();
                int x=pos/n,y=pos%n;

                for(int d=0;d<4;d++)
                {
                    int xx=x+dir[d][0],yy=y+dir[d][1];
                    int ps=xx*n+yy;
                    if(xx==tx && yy==ty) return steps+1;
                    if(xx<0 || xx>=m || yy<0 || yy>=n || visited[ps] || forest[xx][yy]==0) continue;
                    
                    q.push(ps);visited[ps]=1;
                }
            }
            steps++;
        }
        return -1;
    }
```
	
913	Cat and Mouse		Hard	<br/>
126	Word Ladder II		Hard<br/>

