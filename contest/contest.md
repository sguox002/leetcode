- 156: 526/6764 4/4
- 155: 101/6581 4/4

### contest 156

### 1207. Unique Number of Occurrences (*)
<em>problem: 
determine if the array value's occurrence is unique.</em>

idea: 

hashmap to get the count for each value and convert the count to hashmap. check if the size equals

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
	



