- 156: 526/6764 4/4
- 155: 101/6581 4/4

### contest 156

### 1207. Unique Number of Occurrences (*)
problem: determine if the array value's occurrence is unique.
idea: hashmap to get the count for each value and convert the count to hashmap. check if the size equals

### 1208. Get Equal Substrings Within Budget (***)
*problem: two equal length string s and t, we want to change s to t by same locations at the cost of |s[i]-t[i]|.
given a maxcost, and find the longest substring <=maxcost.*

idea: get the cost at each position and find the longest range sum<=maxcost.
using prefix sum and hashmap.
or using sliding window using two pointer

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
*Problem:

remove k repeated chars until we cannot. return the final string.*

idea:

using stack to remove the string. stack store the char and its repeat number.
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
*problem:

In an n*n grid, there is a snake that spans 2 cells and starts moving from the top left corner at (0, 0) and (0, 1). The grid has empty cells represented by zeros and blocked cells represented by ones. The snake wants to reach the lower right corner at (n-1, n-2) and (n-1, n-1).

In one move the snake can:

- Move one cell to the right if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
- Move down one cell if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
- Rotate clockwise if it's in a horizontal position and the two cells under it are both empty. In that case the snake moves from (r, c) and (r, c+1) to (r, c) and (r+1, c).
- Rotate counterclockwise if it's in a vertical position and the two cells to its right are both empty. In that case the snake moves from (r, c) and (r+1, c) to (r, c) and (r, c+1).

Return the minimum number of moves to reach the target.

If there is no way to reach the target, return -1.*

idea:

typical bfs problem.
queue need store the snake's head and tail position.
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
*Problem:

Given an array of distinct integers arr, find all pairs of elements with the minimum absolute difference of any two elements. 

Return a list of pairs in ascending order(with respect to pairs), each pair [a, b] follows

a, b are from arr
a < b
b - a equals to the minimum absolute difference of any two elements in arr*

idea:

sort the array and find the min diff. The neigboring pairs will have the min difference
so only need check neighborings to get the answer.

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
*Problem:

Write a program to find the n-th ugly number.

Ugly numbers are positive integers which are divisible by a or b or c.*

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


 
