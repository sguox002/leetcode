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
- note using stringstream to analyze the path need be careful. we need reconstruct the path.

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
	
