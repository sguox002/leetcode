- 156: 526/6764 4/4
- 155: 101/6581 4/4
- bi9: 112/3075 4/4
- 154: 385/6063
- 153: 183/6214
- bi8: 69/1725 4/4
- 152:
- 151:
- bi7:
### contest 156

### 1207. Unique Number of Occurrences (*)
<em>problem: 
determine if the array value's occurrence is unique.</em>

idea: 

hashmap to get the count for each value and convert the count to hashset. check if the size equals

### 1208. Get Equal Substrings Within Budget (***)
<em>problem: two equal length string s and t, we want to change s to t by same locations at the cost of |s[i]-t[i]|.
given a maxcost, and find the longest substring <=maxcost.</em>

idea: 
- get the cost at each position and find the longest range sum<=maxcost.
- using prefix sum and hashmap.or using sliding window using two pointer

```cpp
    int equalSubstring(string s, string t, int maxCost) {
        vector<int> diff(s.size());
        for(int i=0;i<s.size();i++)
            diff[i]=abs(s[i]-t[i]);
        int ans=0;
        int i=0,j=0,n=diff.size();
        int tsum=0;
        while(j<n){
            tsum+=diff[j];
            while(tsum>maxCost){
                tsum-=diff[i++];
            }
            ans=max(ans,j-i+1);
            j++;
        }
        return ans;
    }
```	

### 1209. Remove All Adjacent Duplicates in String II (**)
<em>Problem:

remove k repeated chars until we cannot. return the final string.</em>

idea:

- using stack to remove the string. 
- stack store the char and its repeat number.

```cpp
    string removeDuplicates(string s, int k) {
        //using stack
        stack<pair<char,int>> st;
        for(char c: s){
            if(st.empty() || st.top().first!=c) st.push({c,1});
            else{
                if(st.top().first==c){
                    st.top().second++;
                    if(st.top().second==k)
                        st.pop();
                }
            }
        }
        string ans;
        while(st.size()){
            while(st.top().second--) ans+=st.top().first;
            st.pop();
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
```	

### 1210. Minimum Moves to Reach Target with Rotations (****)
<em>Problem:

In an n*n grid, there is a snake that spans 2 cells and starts moving from the top left corner at (0, 0) and (0, 1). The grid has empty cells represented by zeros and blocked cells represented by ones. The snake wants to reach the lower right corner at (n-1, n-2) and (n-1, n-1).

In one move the snake can:

- Move one cell to the right if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
- Move down one cell if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
- Rotate clockwise if it's in a horizontal position and the two cells under it are both empty. In that case the snake moves from (r, c) and (r, c+1) to (r, c) and (r+1, c).
- Rotate counterclockwise if it's in a vertical position and the two cells to its right are both empty. In that case the snake moves from (r, c) and (r+1, c) to (r, c) and (r, c+1).

Return the minimum number of moves to reach the target.

If there is no way to reach the target, return -1.</em>

idea:
- typical bfs problem.
- queue need store the snake's head and tail position.
- try all 4 possible position.
- using string hashset for visited.

```cpp
    int minimumMoves(vector<vector<int>>& grid) {
        int n=grid.size();
        //bfs approach
        queue<vector<int>> q;
        unordered_set<string> v;
        //the snake has two cells
        q.push({0,0,0,1});
        v.insert("0 0 0 1");
        int steps=0;
        while(q.size()){
            int sz=q.size();
            while(sz--){
                vector<int> pos=q.front();
                q.pop();
                if(pos==vector<int>({n-1,n-2,n-1,n-1})) // || pos==vector<int>({n-1,n-1,n-1,n-2}))
                    return steps;
                
                //check 4 directions
                int x0=pos[0],y0=pos[1],x1=pos[2],y1=pos[3];
                if(y1+1<n && !grid[x1][y1+1]){ //horizontal move, it can be horizontal or vertical
                    string s=to_string(x0)+" "+to_string(y0+1)+" "+to_string(x1)+" "+to_string(y1+1);
                    if((x0==x1 || (y0==y1 && !grid[x0][y0+1])) && !v.count(s) ){
                        q.push({x0,y0+1,x1,y1+1});
                        v.insert(s);
                    }
                }
                
                if(x1+1<n && !grid[x1+1][y1]){ //vertical
                    string s=to_string(x0+1)+" "+to_string(y0)+" "+to_string(x1+1)+" "+to_string(y1);
                    if((y0==y1 ||(x0==x1 && !grid[x0+1][y0])) && !v.count(s)){
                        q.push({x0+1,y0,x1+1,y1});
                        v.insert(s);
                    }
                }
                if(x0==x1 && x1+1<n && y1-1>=0 && !grid[x1+1][y1-1] && !grid[x1+1][y1]){ //horizontal clockwise from (x1,y1) to (x1+1,y1-1)
                    string s=to_string(x0)+" "+to_string(y0)+" "+to_string(x1+1)+" "+to_string(y1-1);
                    if(!v.count(s)){
                        q.push({x0,y0,x1+1,y1-1});
                        v.insert(s);
                    }
                }
                if(y0==y1 && x1-1>=0 && y1+1<n && !grid[x1-1][y1+1] && !grid[x1][y1+1]){ //vertical clockwise from (x1,y1) to (x1-1,y1+1)
                    string s=to_string(x0)+" "+to_string(y0)+" "+to_string(x1-1)+" "+to_string(y1+1);
                    if(!v.count(s)){
                        q.push({x0,y0,x1-1,y1+1});
                        v.insert(s);
                    }
                }
            }
            steps++;
        }
        return -1;
    }
```

## contest 155
### 1200. Minimum Absolute Difference (**)
<em>Problem:

Given an array of distinct integers arr, find all pairs of elements with the minimum absolute difference of any two elements. 

Return a list of pairs in ascending order(with respect to pairs), each pair [a, b] follows

a, b are from arr
a < b
b - a equals to the minimum absolute difference of any two elements in arr</em>

idea:

- sort the array and find the min diff. 
- The neigboring pairs will have the min difference. so only need check neighborings to get the answer.

```cpp
    vector<vector<int>> minimumAbsDifference(vector<int>& arr) {
        sort(arr.begin(),arr.end());
        vector<vector<int>> ans;
        vector<int> diff(arr.size()-1)        ;
        for(int i=1;i<arr.size();i++){
            diff[i-1]=arr[i]-arr[i-1];
        }
        int mindiff=*min_element(diff.begin(),diff.end());
        for(int i=0;i<diff.size();i++){
            if(diff[i]==mindiff)
                ans.push_back({arr[i],arr[i+1]});
        }
        return ans;
    }
```

### 1201. Ugly Number III (***)
<em>Problem:

Write a program to find the n-th ugly number.

Ugly numbers are positive integers which are divisible by a or b or c.</em>

Idea:
- a,b,c could be the same. 
- binary search to find how many numbers given a target:

n/a+n/b+n/c-n/lcm(a,b)-n/lcm(b,c)-n/lcm(a,c)+n/lcm(a,b,c)

```cpp
   int nthUglyNumber(int k, int A, int B, int C) {
        int lo = 1, hi = 2 * (int) 1e9;
        long a = long(A), b = long(B), c = long(C);
        long ab = a * b / __gcd(a, b);
        long bc = b * c / __gcd(b, c);
        long ac = a * c / __gcd(a, c);
        long abc = a * bc / __gcd(a, bc);
        while(lo < hi) {
            int mid = lo + (hi - lo)/2;
            int cnt = mid/a + mid/b + mid/c - mid/ab - mid/bc - mid/ac + mid/abc;
            if(cnt < k) 
                lo = mid + 1;
            else
			   //the condition: F(N) >= k
                hi = mid;
        }
        return lo;
    }
```
	
### 1202. Smallest String With Swaps (****)
<em>
Problem:

You are given a string s, and an array of pairs of indices in the string pairs where pairs[i] = [a, b] indicates 2 indices(0-indexed) of the string.

You can swap the characters at any pair of indices in the given pairs any number of times.

Return the lexicographically smallest string that s can be changed to after using the swaps.
</em>

Idea:

a pair of index can sort the two chars. if the pairs connected, we will be able to sort the link.
so, it seems it is a union-find problem. sort the chars in each set.

```cpp
    vector<int> parent;
    int sz;
    string smallestStringWithSwaps(string s, vector<vector<int>>& pairs) {
        //union-find
        int n=s.size();
        parent.resize(n);
        for(int i=0;i<n;i++) parent[i]=i;
        sz=n;
        for(auto t: pairs){
            int pi=find_parent(t[0]),pj=find_parent(t[1]);
            if(pi==pj) continue;
            if(pi<pj) parent[pj]=pi;
            else parent[pi]=pj;
            sz--;
        }
        //sort each group using lexi
        unordered_map<int,pair<string,vector<int>>> mp;
        //unordered_map<int,vector<int>> mp1;
        for(int i=0;i<n;i++){
            int pi=find_parent(i);
            mp[pi].first+=s[i];
            mp[pi].second.push_back(i);
        }
        
        for(auto &t: mp)
            sort(t.second.first.begin(),t.second.first.end());
        //for(auto t: mp)
            //cout<<t.first<<": "<<t.second.first<<endl;
        string ans=s;
        for(auto t: mp){
            for(int i=0;i<t.second.first.size();i++){
                ans[t.second.second[i]]=t.second.first[i];
            }
        }
        return ans;
    }
    int find_parent(int i){
        while(i!=parent[i]) i=parent[i];
        return i;
    }
```

### 1203. Sort Items by Groups Respecting Dependencies	(*****)
<em>Problem:

There are n items each belonging to zero or one of m groups where group[i] is the group that the i-th item belongs to and it's equal to -1 if the i-th item belongs to no group. The items and the groups are zero indexed. A group can have no item belonging to it.

Return a sorted list of the items such that:

The items that belong to the same group are next to each other in the sorted list.
There are some relations between these items where beforeItems[i] is a list containing all the items that should come before the i-th item in the sorted array (to the left of the i-th item).
Return any solution if there is more than one solution and return an empty list if there is no solution.

Input: n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = [[],[6],[5],[6],[3,6],[],[],[]]
Output: [6,3,4,1,5,2,0,7]

</em>
Approach:

understand the problem and example:
0: -1
1: -1 [6]
2: 1, [5]
3: 0, [6]
4: 0, [3,6]
5: 1, []
6: 0, []
7:-1, []
3,4,6 belong to group 0
2,5 belongs to group 1
other only bound by before condition
only graph can represent the relations.
basic intuition on this:
- inside group toposology sort
- group group topological sort
https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/discuss/402945/C%2B%2B-with-picture-generic-topological-sort
he adds one source node and one dest node for each group and use them to connect other groups.
and then the graph becomes a one level toposort
source node will have outcoming edges to all nodes in the group. destination node will have incoming edges from all nodes in the group
- first build the graph based on the input conditions.
- using vector<vector<int>> or vector<unordered_set<int>> for the adjacency matrix.
- toposort: has 3 state: not visted, in visit, visited. Also used to detect a cycle.

```cpp
bool topSort(vector<unordered_set<int>>& al, int i, vector<int>& res, vector<int>& stat) {
    if (stat[i] != 0) return stat[i] == 2;
    stat[i] = 1;
    for (auto n : al[i])
        if (!topSort(al, n, res, stat)) return false;
    stat[i] = 2;
    res.push_back(i);
    return true;
}
vector<int> sortItems(int n, int m, vector<int>& group, vector<vector<int>>& beforeItems) {
    vector<int> res_tmp, res(n), stat(n + 2 * m);
    vector<unordered_set<int>> al(n + 2 * m);
    for (auto i = 0; i < n; ++i) {
        if (group[i] != -1) {
            al[n + group[i]].insert(i);
            al[i].insert(n + m + group[i]);
        }
        for (auto j : beforeItems[i]) {
            if (group[i] != -1 && group[i] == group[j]) al[j].insert(i);
            else {
                auto ig = group[i] == -1 ? i : n + group[i];
                auto jg = group[j] == -1 ? j : n + m + group[j];
                al[jg].insert(ig);
            }
        }
    }
    for (int n = al.size() - 1; n >= 0; --n)
        if (!topSort(al, n, res_tmp, stat)) return {};
    reverse(begin(res_tmp), end(res_tmp));
    copy_if(begin(res_tmp), end(res_tmp), res.begin(), [&](int i) {return i < n; });
    return res;
}
```
- note: why we need to start from right to do topsort? because all nodes>n-1 are added nodes which are the source/dest nodes for the groups

## biweek 9
### 1196. How Many Apples Can You Put into the Basket (*)
<em>Problem:

You have some apples, where arr[i] is the weight of the i-th apple.  You also have a basket that can carry up to 5000 units of weight.

Return the maximum number of apples you can put in the basket.
</em>
idea:

greedy, from light to heavy, prefix sum.

### 1197. Minimum Knight Moves (****)
<em>Problem:

In an infinite chess board with coordinates from -infinity to +infinity, you have a knight at square [0, 0].

A knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.

Return the minimum number of steps needed to move the knight to the square [x, y].  It is guaranteed the answer exists.
|x|+|y|<300.
</em>

Approach:
- intuition is bfs for shortest move.
- any position can be limited in first quarter.
- bfs will grow substantially if layer number is large.
The critical point for this problem is limit the coordinate in 1st quarter.
```cpp
    int minKnightMoves(int tx, int ty)
    {
        tx = abs(tx), ty = abs(ty);
        int n = 400;
        vector<vector<int>> mat(n, vector<int>(n,-1));

        int dx[] = {-2, -2, -1, -1,  1, 1,  2, 2};
        int dy[] = {-1,  1, -2,  2, -2, 2, -1, 1};

        int row = 0, col = 0, x, y;

        queue<pair<int, int>> q;

        mat[row][col] = 0;
        q.push(make_pair(row,col));

        while(!q.empty())
        {
            if(mat[tx][ty] != -1)
                return mat[tx][ty];

            row = q.front().first;
            col = q.front().second;
            q.pop();

            for(int i = 0; i < 8; i++)
            {
                int x = abs(row + dx[i]);//limit it in 1st quarter.
                int y = abs(col + dy[i]);

                if(x >= 0 and y >= 0 and x < n and y < n and mat[x][y] == -1)
                {
                    mat[x][y] = 1 + mat[row][col];
                    q.push(make_pair(x,y));
                }
            }
        } 
        return mat[tx][ty];
    }
```	

### 1198. Find Smallest Common Element in All Rows (**)
<em>Problem:

Given a matrix mat where every row is sorted in increasing order, return the smallest common element in all rows.

If there is no common element, return -1.
</em>
idea:

- this is so similar to sorted list merge using k pointers with PQ. O(mn)
- using first row number and binary search in other rows. O(mnlog(m))
- count using hashmap.

### 1199. Minimum Time to Build Blocks
<em>Problem:

You are given a list of blocks, where blocks[i] = t means that the i-th block needs t units of time to be built. A block can only be built by exactly one worker.

A worker can either split into two workers (number of workers increases by one) or build a block then go home. Both decisions cost some time.

The time cost of spliting one worker into two workers is given as an integer split. Note that if two workers split at the same time, they split in parallel so the cost would be split.

Output the minimum time needed to build all blocks.

Initially, there is only one worker.
blocks = [1,2,3], split = 1
Split 1 worker into 2, then assign the first worker to the last block and split the second worker into 2.
Then, use the two unassigned workers to build the first two blocks.
The cost is 1 + max(3, 1 + max(1, 2)) = 4.

</em>
Analysis:

- n jobs need split n workers, and one worker can split into two workers.
- parallel work: the time is the max.
- assign worker to block matters to the total time.
- apparently, worker split and assign the longest job, the other guy split and assign him the longest job. seems a greedy approach works. above observation is incorrect, each guy has option to do work or split again, this makes it a binary tree. The leaf node is the time. each branch is the time to finish the job. and the answer is the max of all the branches.
- We can model this entire question as a binary tree that we need to construct with a minimum max depth cost. Each of the blocks is a leaf node, with a cost of its face value. And then each inner node will be of cost split. nodes that are sitting at the same level represent work that is done in parallel. We know there will be len(blocks) - 1 of these inner nodes, so the question now is how can we construct the tree such that it has the minimum depth.
- Huffman is used to build a tree with minimum total weighted path. we can choose either split or not split. This is the same as choosing 0 or 1 in Huffman code. Initially, the optimal tree can be built by using a huffman coding way. Then combine the least two nodes. After that, rebuild the huffman tree.
- thus we get the algorithm: build a min heap and pop the top 2 and add split to the larger one, and put it back until there is one left. This is called Huffman algorithm.
- this is reverse thinking: look each num in blocks as leaf, and merge these leaves with cost "split" until get one root.
for example [1,2,3] with split=1
first leaf [1,2,3]
the [1,2] merge with split, got max 3
3 merge with 3 with split, got 4.

```cpp
    int minBuildTime(vector<int>& blocks, int split) {
        priority_queue<int, vector<int>, greater<int>> pq;
        for(int num: blocks) {
            pq.push(num);
        }
        while (pq.size() > 1) {
            int a = pq.top(); pq.pop();
            int b = pq.top(); pq.pop();
            pq.push(b+split);
        }
        return pq.top();
    }
```
	
## contest 154
1189. Maximum Number of Balloons (**)
<em>Problem:
Given a string text, you want to use the characters of text to form as many instances of the word "balloon" as possible.

You can use each character in text at most once. Return the maximum number of instances that can be formed.</em>

idea: 

using hashmap to record the occurrence of each char in balloon and then divide the cnt in balloon and get the gcd

### 1190. Reverse Substrings Between Each Pair of Parentheses (***)
<em>Problem:

You are given a string s that consists of lower case English letters and brackets. 

Reverse the strings in each pair of matching parentheses, starting from the innermost one.

Your result should not contain any brackets.
</em>
idea: recursive stack.
```cpp
    string reverseParentheses(string s) {
        string ans=parse(s);
        reverse(ans.begin(),ans.end());
        return ans;
    }
        
    string parse(string s) {
        stack<string> st;
        int i=0;
        string t;
        while(i<s.size()){
            if(s[i]!='('){
                t+=s[i];
                i++;
            }
            else{
                if(t.size()){
                    st.push(t);
                    t.clear();
                }
                int j=i+1,p=1;
                while(p){
                    if(s[j]=='(') p++;
                    if(s[j]==')') p--;
                    j++;
                }
                j--;
                string tp=parse(s.substr(i+1,j-i-1));//reversed
                st.push(tp);
                i=j+1;
            }
        }
        
        if(t.size()){
            st.push(t);
        } 
        string ans;
        while(st.size()){
            string w=st.top();
            st.pop();
            reverse(w.begin(),w.end());
            ans+=w;
        }
        return ans;//{ans.rbegin(),ans.rend()};
        
    }
```	

### 1191. K-Concatenation Maximum Sum (****)
<em>Problem:

Given an integer array arr and an integer k, modify the array by repeating it k times.

For example, if arr = [1, 2] and k = 3 then the modified array will be [1, 2, 1, 2, 1, 2].

Return the maximum sub-array sum in the modified array. Note that the length of the sub-array can be 0 and its sum in that case is 0.

As the answer can be very large, return the answer modulo 10^9 + 7.</em>
idea:

- k=1:  find the max subarray sum in one array
- k=2:  find the max subarray sum in circular array: find the max and min at the same time, or use rotation. 
- k>2:  inside k-2 groups only count the sum of the array.
so only consider k=1 and 2 case.

```cpp
int kConcatenationMaxSum(vector<int>& arr, int k) {
    int n = arr.size(), sum = arr[0], mx = arr[0];
    int64_t total = accumulate(arr.begin(), arr.end(), 0), mod = 1e9+7;
    for (int i = 1; i < n * min(k, 2); i++) {
        sum = max(arr[i % n], sum + arr[i % n]);
        mx = max(mx, sum);
    }
    return max<int64_t>({0, mx, total * max(0, k - 2) + mx}) % mod;
}
```

### 1192. Critical Connections in a Network (*****)
<em>Problem:

There are n servers numbered from 0 to n-1 connected by undirected server-to-server connections forming a network where connections[i] = [a, b] represents a connection between servers a and b. Any server can reach any other server directly or indirectly through the network.

A critical connection is a connection that, if removed, will make some server unable to reach some other server.

Return all critical connections in the network in any order.
</em>

idea:
- brutal force: loop to union find the edge without current one to see if it combines one set.
it surely get TLE since the number of edges up to 10^5, and union find is O(N) and the complexity would be O(N^2)
without path suppresion, it is higher.
```cpp
    vector<int> parent;
    int sz;
    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
        vector<vector<int>> ans;
        parent.resize(connections.size());
        for(int i=0;i<connections.size();i++)
            if(single_set(connections,i,n)) ans.push_back(connections[i]);
        return ans;
    }
    bool single_set(vector<vector<int>>& conn,int rm,int num_node){
        parent.clear();
        int sz=num_node;
        for(int i=0;i<sz;i++) parent[i]=i;
        for(int i=0;i<conn.size();i++){
            if(i==rm) continue;
            int pi=findp(conn[i][0]),pj=findp(conn[i][1]);
            if(pi!=pj) {parent[pi]=pj;sz--;}
        }
        //cout<<rm<<" "<<sz<<endl;
        return sz!=1;
    }
    int findp(int i){
        while(i!=parent[i]){
			parent[i]=parent[parent[i]]; //path compression
			i=parent[i];
		}
        return i;
    }
```	
- efficient way: a critical edge is an edge not in a cycle. it is equivalent to find the bridge between two cycles.
if we use dfs to traverse, using visited states, we are able to find if there is a cycle. If no cycle found, all the edges are critical edge.
if there is one or more cycles, all those edges in the cycles need to be discarded.
so we need find a way to mark edges in each cycle.
we can mark our visit time:

0 - 2
 \  /
  1 - 3
visiting sequence:  
0-2-1-3 done. node 3 is done, and pop 3. try node 1's other path.
0-2-1-0 done, see the cycle. node 1 is done, pop 1. mark 0-2, 2-1, 1-0
0-2, 2's other path is done. pop 2. try 0's other path.
0-1 visited, done.

0 2 1 3 
0 (2)
found: 0-2
found: 1-0
found: 2-1
(1)

```cpp
vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
        unordered_map<int, unordered_set<int>> graph;
        unordered_set<long> st;
		for (auto& edge: connections){
            //if (edge[0] > edge[1]) swap(edge[0], edge[1]); //
            graph[edge[0]].insert(edge[1]);
            graph[edge[1]].insert(edge[0]);
			st.insert((long)edge[0]*n+edge[1]);
        }
        vector<int> rank(n, -2);
        dfs(0, 0, n, st, rank, graph);
        vector<vector<int>> ans;//
        for(auto t: st){
            ans.push_back({t/n,t%n});
        }
        return ans;
    }
    
    int dfs(int node, int depth, int n, unordered_set<long>& st, vector<int>& rank, unordered_map<int, unordered_set<int>>& graph) {
        if (rank[node] > 0) return rank[node];
        rank[node] = depth;
        int min_depth = n; //being visited mark the max n.

        for(auto& neghbor: graph[node]) {
            if (rank[neghbor] == depth - 1) continue; //the parent node just visited, avoid repeating.
            int cur_depth = dfs(neghbor, depth + 1, n, st, rank, graph); //current dfs min depth.
            if (cur_depth <= depth) {
                st.erase((long)min(node, neghbor)*n+max(node, neghbor)); //make sure we use start<end.
            }
            min_depth = min(min_depth, cur_depth);
        }
        rank[node] = n; //this line is not necessary. but actually not. it will not pass the test if removed.
        return min_depth;
    }
```	
- first make the graph, (undirectional), to make the edge 1-0 0-1 the same, we make sure the start<end.
- we use i*n+j to represent an edge.
- mark all nodes to be -1 or -2 (unvisited)
- mark a node being visited as n. ( or other value)
- mark a node being visited a timestamp or depth, or rank.
- dfs all its neighbors (prevent the parent again) and find the min_depth.
- if min depth is smaller than current node's depth, this edge with neighbor is in some cycle, mark  it as non-critical connection
- after visited, mark the rank as n. (some other path may be smaller and still get to the node).
- tarjan algorithm for this problem. it is used to find strongly connected components in a graph in linear time.
check the strongly connected graph text about this.


## contest 153
### 1184. Distance Between Bus Stops (**)
<em>Problem:

A bus has n stops numbered from 0 to n - 1 that form a circle. We know the distance between all pairs of neighboring stops where distance[i] is the distance between the stops number i and (i + 1) % n.

The bus goes along both directions i.e. clockwise and counterclockwise.

Return the shortest distance between the given start and destination stops.
</em>
idea: it is a cycle, we just compare start-end distance and total-start,end distance, make sure start<end

### 1185. Day of the week (**)
- use built in date functions
- known today's weekday and calculate how many days between given date and today. and %7. We need to know how to get the leap year.

### 1186. Maximum Subarray Sum with One Deletion (****)
<em>Problem:

Given an array of integers, return the maximum sum for a non-empty subarray (contiguous elements) with at most one element deletion. In other words, you want to choose a subarray and optionally delete one element from it so that there is still at least one element left and the sum of the remaining elements is maximum possible.

Note that the subarray needs to be non-empty after deleting one element.
</em>

idea:

- if in the subarray, min>0, we do not remove anything. if <0, then we remove the min.
- also remove the min helps to connect two subarrays
- dp (the max subarray sum is also a dp problem) and we add an extra constraint making it a 2d dp problem:
	- do not delete it: regular max subarray sum
	- delete it: only for negative number.
	- all negatives, return the max.
	
```cpp
    int maximumSum(vector<int>& arr) {
        vector<vector<int>> dp(arr.size(), vector<int>(2, 0)); //dp[i,0] no delete, dp[i,1] delete one element
        dp[0][0]=arr[0]; 
        int res=arr[0];
        for(int i=1;i<arr.size();i++) { 
            dp[i][0]=max(arr[i], dp[i-1][0]+arr[i]); //nodel
            dp[i][1]=max(arr[i], max(dp[i-1][1]+arr[i], dp[i-1][0])); //delete i or not delete i.
            res=max(res, max(dp[i][0], dp[i][1])); 
        }
        return res;
    }
```	
to make it more clear:
```cpp
    int maximumSum(vector<int>& arr) {
		int del=0,nodel=arr[0];
        int res=arr[0];
        for(int i=1;i<arr.size();i++) {
            int v=arr[i];
            del=max({v, del+v, nodel}); //delete i or not delete i: whether we shall connect to previous
			nodel=max(v, nodel+v); //nodel: either we shall connect to previous
            res=max({res, del, nodel}); 
        }
        return res;
    }
```

### 1187. Make Array Strictly Increasing (*****)
<em>Problem:

Given two integer arrays arr1 and arr2, return the minimum number of operations (possibly zero) needed to make arr1 strictly increasing.

In one operation, you can choose two indices 0 <= i < arr1.length and 0 <= j < arr2.length and do the assignment arr1[i] = arr2[j].

If there is no way to make arr1 strictly increasing, return -1.
</em>

Idea:
[1,5,3,6,7],[4,3,1]
sort the arr2 to [1,3,4]
replace 5 with 3, replace 3 with 4->[1,3,4,6,7]

- strictly increasing, so duplicate in arr2 is useless, we can remove those duplicates. and no element can be assigned more than one time.
- dp approach: 
  - first sort arr2 and remove duplicates, now the number of replace is the same as index in arr2
  - now think it is a variation of longest increasing subsequence, but now you can go to the other array
  arr1 = [1,5,3,6,7], arr2 = [1,3,2,4]
  first sort and remove duplicate of arr2=[1,2,3,4]
  1 5 3 6 7
    \  /
  1 2 3 4
  
it is like: find the longest increasing subsequence from arr1: 1,3,6,7 and check if we can fill the missing elements from arr2. The problem is we may have many LIS and we need to check all these combinations.
we can add INT_MIN to the head and INT_MAX to the tail of arr1 to make the base simpler.
using LIS, we only need to calculate the number of changes from j+1 to i-1.
```cpp
	int makeArrayIncreasing(vector<int>& arr1, vector<int>& arr2) {
		set<int> ms(arr2.begin(),arr2.end());
		arr2.clear();
		for(int t: ms) arr2.push_back(t);
		arr1.insert(arr1.begin(),INT_MIN);
		arr1.push_back(INT_MAX);
		int n=arr1.size();
		vector<int> dp(n,INT_MAX);
		dp[0]=0;
		for(int i=1;i<n;i++){
			for(int j=0;j<i;j++){
				if(arr1[i]>arr1[j] && dp[j]<INT_MAX){
				int change=check(arr1,j,i,arr2);
				if(change>=0) dp[i]=min(dp[i],dp[j]+change);
				}
			}
		}
		return dp[n-1]==INT_MAX?-1:dp[n-1];
	}
	//find the change number from start+1 to end-1
	int check(vector<int>& arr1,int start,int end,vector<int>& arr2){
		if(start+1==end) return 0;
		int mn=arr1[start],mx=arr1[end];
		int idx=upper_bound(arr2.begin(),arr2.end(),mn)-arr2.begin();
		int maxcount=end-start-1;
		int endi=idx+maxcount-1;
		if(endi<arr2.size() && arr2[endi]<mx) return maxcount;
		return -1;
	}
```
- it checks if replace all the element from start+1 to end-1 if arr2 has all the elements.
- the check function is quite hard to understand.

## biweek 8
### 1180. Count Substrings with Only One Distinct Letter (***)
<em>Problem: Given a string S, return the number of substrings that have only one distinct letter.</em>
idea: find the same char region i,j and the number of subarray is len*(len+1) len=j-i;
two pointers.
### 1181. Before and After Puzzle (***)
<em>Problem:

Given a list of phrases, generate a list of Before and After puzzles.

A phrase is a string that consists of lowercase English letters and spaces only. No space appears in the start or the end of a phrase. There are no consecutive spaces in a phrase.

Before and After puzzles are phrases that are formed by merging two phrases where the last word of the first phrase is the same as the first word of the second phrase.

Return the Before and After puzzles that can be formed by every two phrases phrases[i] and phrases[j] where i != j. Note that the order of matching two phrases matters, we want to consider both orders.

You should return a list of distinct strings sorted lexicographically.
</em>
idea:

brutal force:
- check phase[i] and phase[j] to see if they match the condition.
- using stringstream
- store in a set or sort.

### 1182. Shortest Distance to Target Color (***)
<em>Problem:

You are given an array colors, in which there are three colors: 1, 2 and 3.

You are also given some queries. Each query consists of two integers i and c, return the shortest distance between the given index i and the target color c. If there is no solution return -1.


Example 1:

Input: colors = [1,1,2,1,3,2,2,3,3], queries = [[1,3],[2,2],[6,1]]
Output: [3,0,3]
Explanation: 
The nearest 3 from index 1 is at index 4 (3 steps away).
The nearest 2 from index 2 is at index 2 itself (0 steps away).
The nearest 1 from index 6 is at index 3 (3 steps away).
</em>
idea:
1: [0,1,3]
2: [2,5,6]
3: [4,7,8]
given index and color: binary search the closest in the target color array for the given index.
find the upper_bound and then we get [idx-1,idx] to check which is closer.

### 1183. Maximum Number of Ones (****)
<em>Problem:

Consider a matrix M with dimensions width * height, such that every cell has value 0 or 1, and any square sub-matrix of M of size sideLength * sideLength has at most maxOnes ones.

Return the maximum possible number of ones that the matrix M can have.
</em>

idea:
greedy approach: fill the squares with 1s. the out boundary squares shall not fill if possible.

If we create the first square matrix, the big matrix will just be the copies of this one. (translation copies)
The value of each location in the square matrix will appear at multiple locations in the big matrix, count them.
Then assign the ones in the square matrix with more occurances with 1.

```cpp
    int maximumNumberOfOnes(int width, int height, int sideLength, int maxOnes) {
        //greedy: calculate the number of show times of each point in square
        vector<int> cnt;
        for(int i=0;i<sideLength;i++){
            for(int j=0;j<sideLength;j++){
                int cx=(width-i-1)/sideLength+1;
                int cy=(height-j-1)/sideLength+1;
                cnt.push_back(cx*cy);
            }
        }
        sort(cnt.begin(),cnt.end(),greater<int>());
        int ans=0;
        for(int i=0;i<maxOnes;i++) ans+=cnt[i];
        return ans;
    }
```	
- it basically calculate number of appearance of the points in the square shown in the rectangle.
for example [0,0] can be translated horizontally or veritcally.
we sort it according to its number and fill them first.
- we can also use a min heap to pop non-needed elements.
see other O(1) solutions in discussion on the site.

## contest 152
### 1175. Prime Arrangement (***)
<em>Problem:

Return the number of permutations of 1 to n so that prime numbers are at prime indices (1-indexed.)

(Recall that an integer is prime if and only if it is greater than 1, and cannot be written as a product of two positive integers both smaller than it.)

Since the answer may be large, return the answer modulo 10^9 + 7.
</em>
Idea:
suppose we have m primes, it takes m slots, and other takes (n-m) slots
m!*(n-m)!
- count prime numbers.
```cpp
    int mod=1e9+7;
    int numPrimeArrangements(int n) {
        int num_primes=getnum_primes(n);        
        return factor(num_primes)*factor(n-num_primes)%mod;
    }
    int getnum_primes(int n){
        vector<int> primes(n+1,1);
        primes[0]=primes[1]=0;
        for(int i=2;i<=sqrt(n);i++){
            for(long j=(long)i*i;j<=n;j+=i)
                primes[j]=0;
        }
        return accumulate(primes.begin(),primes.end(),0);
    }
    long factor(int n){
        long res=1;
        for(int i=1;i<=n;i++)
            res*=i,res%=mod;
        return res%mod;
	}
```

### 1176. Diet Plan Performance(*)
<em>Problem:

A dieter consumes calories[i] calories on the i-th day. 

Given an integer k, for every consecutive sequence of k days (calories[i], calories[i+1], ..., calories[i+k-1] for all 0 <= i <= n-k), they look at T, the total calories consumed during that sequence of k days (calories[i] + calories[i+1] + ... + calories[i+k-1]):

If T < lower, they performed poorly on their diet and lose 1 point; 
If T > upper, they performed well on their diet and gain 1 point;
Otherwise, they performed normally and there is no change in points.
Initially, the dieter has zero points. Return the total number of points the dieter has after dieting for calories.length days.

Note that the total points can be negative.	
</em>
Idea: sliding window at a fixed win size.
		
### 1177. Can Make Palindrome from Substring (***)
<em>Problem:

Given a string s, we make queries on substrings of s.

For each query queries[i] = [left, right, k], we may rearrange the substring s[left], ..., s[right], and then choose up to k of them to replace with any lowercase English letter. 

If the substring is possible to be a palindrome string after the operations above, the result of the query is true. Otherwise, the result is false.

Return an array answer[], where answer[i] is the result of the i-th query queries[i].

Note that: Each letter is counted individually for replacement so if for example s[left..right] = "aaa", and k = 2, we can only replace two of the letters.  (Also, note that the initial string s is never modified by any query.)
</em>

Idea:

- a palindrome string has zero or one char with odd occurrences, so just check if it is possible to reach this using k replace.
- a range count can be obtained using prefix sum. using 26 prefix sum.
```cpp
    vector<bool> canMakePaliQueries(string s, vector<vector<int>>& queries) {
        //count the prefix sum for each number
        vector<vector<int>> cnt(26);
        for(int i=0;i<s.size();i++) cnt[s[i]-'a'].push_back(i);
        int n=queries.size();
        vector<bool> ans(n);
        for(int i=0;i<queries.size();i++){
            ans[i]=valid(cnt,queries[i]);
        }
        return ans;
    }
    bool valid(vector<vector<int>>& cnt,vector<int>& q){
        int l=q[0],r=q[1],k=q[2];
        if(l>r) return 0;
        if(l==r) return 1;
        vector<int> tmp(26);
        int num_rep=0;
        for(int i=0;i<26;i++){
            if(cnt[i].empty()) continue;
            auto it1=lower_bound(cnt[i].begin(),cnt[i].end(),l);
            auto it2=upper_bound(cnt[i].begin(),cnt[i].end(),r);
            tmp[i]=it2-it1;
            if(tmp[i]%2) num_rep++;
        }
        return num_rep/2<=k; //we can only keep 1 odd, and need 
    }
```	

### 1178. Number of Valid Words for Each Puzzle (****)
<em>Problem:

With respect to a given puzzle string, a word is valid if both the following conditions are satisfied:
word contains the first letter of puzzle.
For each letter in word, that letter is in puzzle.
For example, if the puzzle is "abcdefg", then valid words are "faced", "cabbage", and "baggage"; while invalid words are "beefed" (doesn't include "a") and "based" (includes "s" which isn't in the puzzle).
Return an array answer, where answer[i] is the number of words in the given word list words that are valid with respect to the puzzle puzzles[i].
</em>

Approach:

- get the hist for each puzzle and also get hist for word, word must be a subset of puzzle O(m*n)
up to 10^9 which is too high complexity.
- trie: trie is generally a good choice for many string vs many string. but how?
	- build puzzle or word? for word
	- word need remove duplicates and sorted
	- word may have duplicates after preprocessing, need save count and hashset.
	```cpp
	    struct TrieNode{
        TrieNode* child[26];
        int count;
        set<char> ms;
        TrieNode(){count=0;memset(child,0,26*sizeof(TrieNode*));}
    };
    TrieNode* root;
	```
	- add words into trie:
	```cpp
	void addWord(string s){
        set<char> ms(s.begin(),s.end());
        //cout<<res<<endl;
        TrieNode* p=root;
        for(char c: ms){
            if(p->child[c-'a']==0)
                p->child[c-'a']=new TrieNode;
            p=p->child[c-'a'];
        }
        p->count++;
        p->ms=ms;
    }
	```
	- find match the puzzle with constraint: first letter in puzzle must in it.
		- sort the puzzle word (since trie is sorted)
		- dfs
		```cpp
	int find_word(string& s,bool match,int start,char first,TrieNode* p){
        //if(match && !p->child[first-'a']) return 0;
        if(start==s.size()) return 0;
        int ans=0;
        char c=s[start];
        if(!p->ms.empty() && p->ms.count(first))
            ans+=p->count;
        //cout<<c;
        for(int i=0;i<26;i++){
            if(p->child[i]==0) continue;
            char t='a'+i;
            int st=start;
            while(st<s.size() && s[st]!=t) st++;
            //bool mat=match||(t==first);
            ans+=find_word(s,match||(t==first),st,first,p->child[i]);
        }
       
        return ans;        
    }
	```

## contest 151
### 1169. Invalid Transactions
<em>Problem:

A transaction is possibly invalid if:

the amount exceeds $1000, or;
if it occurs within (and including) 60 minutes of another transaction with the same name in a different city.
Each transaction string transactions[i] consists of comma separated values representing the name, time (in minutes), amount, and city of the transaction.

Given a list of transactions, return a list of transactions that are possibly invalid.  You may return the answer in any order.
</em>
note:
- if A and B occurs in 60mins in different city with same name, A and B are both invalid.
- single invalid: >1000
- first we need to convert the string to data structure: name,time,amount,place
- we can sort the transactions by time (hidden). different person are not related. 
name: time, amount, place
- using same person hashmap and check against previous transaction.
```cpp
    vector<string> invalidTransactions(vector<string>& transactions) {
        unordered_set<string> res;
        unordered_map<string,vector<vector<string>>> m;
        for(auto& t:transactions){
            istringstream ss(t);
            vector<string> s(4,"");
            int i=0;
            while(getline(ss,s[i++],','));
            if(stoi(s[2])>1000) res.insert(t);
            for(vector<string> j:m[s[0]])
                if((j[3]!=s[3])&&abs(stoi(j[1])-stoi(s[1]))<=60){
                    res.insert(j[0]+","+j[1]+","+j[2]+","+j[3]);
                    //cout<<"remove: "<<t<<endl;
                    if(!res.count(t)) res.insert(t);
                }
            m[s[0]].push_back({s[0],s[1],s[2],s[3]});
        }
        vector<string> ans;
        for(auto& k:res) ans.push_back(k);
        return ans;
    }
```
It is not easy to get it right.

### 1170. Compare Strings by Frequency of the Smallest Character (**)
<em>Problem:

Let's define a function f(s) over a non-empty string s, which calculates the frequency of the smallest character in s. For example, if s = "dcce" then f(s) = 2 because the smallest character is "c" and its frequency is 2.

Now, given string arrays queries and words, return an integer array answer, where each answer[i] is the number of words such that f(queries[i]) < f(W), where W is a word in words.

 

Example 1:

Input: queries = ["cbd"], words = ["zaaaz"]
Output: [1]
Explanation: On the first query we have f("cbd") = 1, f("zaaaz") = 3 so f("cbd") < f("zaaaz").
</em>

approach: straightforward, just implement f(s) and count.


### 1171. Remove Zero Sum Consecutive Nodes from Linked List (****)
<em>Problem:

Given the head of a linked list, we repeatedly delete consecutive sequences of nodes that sum to 0 until there are no such sequences.

After doing so, return the head of the final linked list.  You may return any such answer.

 

(Note that in the examples below, all sequences are serializations of ListNode objects.)

Example 1:

Input: head = [1,2,-3,3,1]
Output: [3,1]
Note: The answer [1,2,1] would also be accepted.
</em>
Approach:
we need to find the prefix sum the same, and remove them, we can use stack.
it involves linkedlist and array operations.
```cpp
    ListNode* removeZeroSumSublists(ListNode* head) {
        vector<pair<ListNode*,int>> vnode;
        int prefix=0;
        unordered_map<int,ListNode*> mp;
        mp[0]=0; //insert 0
        ListNode* p=head;
        while(p){
            prefix+=p->val;
            //cout<<p->val<<endl;
            if(mp.count(prefix)){
                while(vnode.size()&&vnode.back().first!=mp[prefix]){
                    //cout<<"remove: "<<vnode.back()->val<<endl;
                    mp.erase(vnode.back().second);
                    vnode.pop_back();
                }
            }
            else {
                mp[prefix]=p;
                vnode.push_back({p,prefix});
            }
            
            p=p->next;
        }
        //cout<<vnode.size();
        ListNode* tail=0;
        while(vnode.size()){
            vnode.back().first->next=tail;
            tail=vnode.back().first;
            vnode.pop_back();
        }
        return tail;
    }
```
	

### 1172. Dinner Plate Stacks
<em>Problem:
You have an infinite number of stacks arranged in a row and numbered (left to right) from 0, each of the stacks has the same maximum capacity.

Implement the DinnerPlates class:

DinnerPlates(int capacity) Initializes the object with the maximum capacity of the stacks.
void push(int val) pushes the given positive integer val into the leftmost stack with size less than capacity.
int pop() returns the value at the top of the rightmost non-empty stack and removes it from that stack, and returns -1 if all stacks are empty.
int popAtStack(int index) returns the value at the top of the stack with the given index and removes it from that stack, and returns -1 if the stack with that given index is empty.
</em>
Approach:
- push from the left (smaller) and pop from the right (the bigger)
- map<int,vector<int>> the stack index vs the stack using vector.
- set<int> avail maintains all non-empty stack.

```cpp
    //that is similar to a growing matrix
    //to quick find the stack, we need get O(1)
    //use a map for non-empty stack and a map for empty
    int c;
    map<int, vector<int>> m;
    set<int> available;

    DinnerPlates(int capacity) {
        c = capacity;
    }

    void push(int val) {
        if (!available.size())
            available.insert(m.size());
        m[*available.begin()].push_back(val);
        if (m[*available.begin()].size() == c)
            available.erase(available.begin());
    }

    int pop() {
        if (m.size() == 0)
            return -1;
        return popAtStack(m.rbegin()->first);
    }

    int popAtStack(int index) {
        if (m[index].size() == 0)
            return -1;
        int val = m[index].back();
        m[index].pop_back();
        available.insert(index);
        if (m[index].size() == 0)
            m.erase(index);
        return val;
    }
```	

## biweek 7
### 1165. Single-Row Keyboard
<em>Problem:

There is a special keyboard with all keys in a single row.

Given a string keyboard of length 26 indicating the layout of the keyboard (indexed from 0 to 25), initially your finger is at index 0. To type a character, you have to move your finger to the index of the desired character. The time taken to move your finger from index i to index j is |i - j|.

You want to type a string word. Write a function to calculate how much time it takes to type it with one finger.

Example 1:

Input: keyboard = "abcdefghijklmnopqrstuvwxyz", word = "cba"
Output: 4
Explanation: The index moves from 0 to 2 to write 'c' then to 1 to write 'b' then to 0 again to write 'a'.
Total time = 2 + 1 + 1 = 4. </em>
idea: straightforward.

### 1166. Design File System
<em>Problem:

You are asked to design a file system which provides two functions:

createPath(path, value): Creates a new path and associates a value to it if possible and returns True. Returns False if the path already exists or its parent path doesn't exist.
get(path): Returns the value associated with a path or returns -1 if the path doesn't exist.
The format of a path is one or more concatenated strings of the form: / followed by one or more lowercase English letters. For example, /leetcode and /leetcode/problems are valid paths while an empty string and / are not.

Implement the two functions.

Please refer to the examples for clarifications.</em>
Idea:

File system is tree structure, we can use hashmap for it.
```cpp
    unordered_map<string,int> mp;
    FileSystem() {
        
    }
    
    bool create(string path, int value) {
        if(mp.count(path)) return 0;
        int ind=1,next;
        while((next=path.find_first_of('/',ind))!=string::npos){
            if(mp.count(path.substr(0,next))==0) return 0;
            ind=next+1;
        }
        mp[path]=value;
        return 1;
    }
    
    int get(string path) {
        if(mp.count(path)) return mp[path];
        return -1;
    }
```

### 1167. Minimum Cost to Connect Sticks
<em>
You have some sticks with positive integer lengths.

You can connect any two sticks of lengths X and Y into one stick by paying a cost of X + Y.  You perform this action until there is one stick remaining.

Return the minimum cost of connecting all the given sticks into one stick in this way.

Example 1:

Input: sticks = [2,4,3]
Output: 14	
</em>

this problem is very similar to huffman algorithm the worker split problem.
the idea: sort the input and arrange as the tree leaf, and combine to a parent (x+y) and parent combine with next leaf.
[1,8,3,5]==>[1,3,5,8]--> 4+9+17=30

     17
     /\
    9  \
   /\   \
  4  \   \
 /\   \   \
1  3   5   8
```cpp
    int connectSticks(vector<int>& sticks) {
        //previous is added again into sum
        //greedy: always add the two min
        priority_queue<int,vector<int>,greater<int>> pq(sticks.begin(),sticks.end());
        int ans=0;
        while(pq.size()>1){
            int i=pq.top();
            pq.pop();
            int j=pq.top();
            pq.pop();
            pq.push(i+j);
            ans+=i+j;
        }
        return ans;
    }
```

### 1168. Optimize Water Distribution in a Village
<em>Problem:

There are n houses in a village. We want to supply water for all the houses by building wells and laying pipes.

For each house i, we can either build a well inside it directly with cost wells[i], or pipe in water from another well to it. The costs to lay pipes between houses are given by the array pipes, where each pipes[i] = [house1, house2, cost] represents the cost to connect house1 and house2 together using a pipe. Connections are bidirectional.

Find the minimum total cost to supply water to all houses.	
</em>
Idea: consider drilling wells a cost to a common node. and then problem is simplified.
from the edges find the connected graph--> minimum spanning tree.
we can use union-find and greedy to get the MST.
greedy: try the shortest edge first.
```cpp
    vector<int> parent;
    struct comp{
        bool operator()(vector<int>& a,vector<int>& b){
            return a[2]<b[2];
        }
    };
    int minCostToSupplyWater(int n, vector<int>& wells, vector<vector<int>>& pipes) {
        //union find
        parent.resize(n+1);
        for(int i=0;i<=n;i++) parent[i]=i;
        for(int i=0;i<n;i++) pipes.push_back({0,i+1,wells[i]});
        sort(pipes.begin(),pipes.end(),comp()); //sort by cost
        //now the problem is to get the min sum visiting all nodes
        //greedy: use the smallest pipe to connect two unions
        int ans=0;
        for(int i=0;n>0;i++){
            int pi=find_parent(pipes[i][0]),pj=find_parent(pipes[i][1]);
            if(pi!=pj) //two different set
            {
                ans+=pipes[i][2];
                if(pi<pj) parent[pj]=pi;
                else parent[pi]=pj;
                n--; //reduce 1 set;
            }
        }
        return ans;
    }
    int find_parent(int i){
        while(i!=parent[i]) i=parent[i];
        return i;
    }
```
	
