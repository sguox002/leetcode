- 156: 526/6764 4/4
- 155: 101/6581 4/4
- bi9: 112/3075 4/4

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











