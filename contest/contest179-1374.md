## contest 180

### 1374. Generate a String With Characters That Have Odd Counts
<em>
Given an integer n, return a string with n characters such that each character in such string occurs an odd number of times.

The returned string must contain only lowercase English letters. If there are multiples valid strings, return any of them.  
</em>

Approach:
if it is odd, then put all 'a'
if it is even, then put n-1 'a' and 1 'b'

```cpp
    string generateTheString(int n) {
        string ans;
        if(n%2==0){
            ans.append(n-1,'a');
            ans+='b';
        }
        else ans.append(n,'a');
        return ans;
    }
```

### 1375. Bulb Switcher III
<em>
There is a room with n bulbs, numbered from 1 to n, arranged in a row from left to right. Initially, all the bulbs are turned off.

At moment k (for k from 0 to n - 1), we turn on the light[k] bulb. A bulb change color to blue only if it is on and all the previous bulbs (to the left) are turned on too.

Return the number of moments in which all turned on bulbs are blue.	
</em>

Approach:
only after previous lights are all turned on, then it is all blue
use prefix sum. O(N)
```cpp
    int numTimesAllBlue(vector<int>& light) {
        long prefix=0;
        int ans=0;
        for(int i=0;i<light.size();i++){
            prefix+=light[i];
            if(prefix==(long)(i+1)*(i+2)/2) ans++;
        }
        return ans;
    }
```	

### 1376. Time Needed to Inform All Employees
<em>
A company has n employees with a unique ID for each employee from 0 to n - 1. The head of the company has is the one with headID.

Each employee has one direct manager given in the manager array where manager[i] is the direct manager of the i-th employee, manager[headID] = -1. Also it's guaranteed that the subordination relationships have a tree structure.

The head of the company wants to inform all the employees of the company of an urgent piece of news. He will inform his direct subordinates and they will inform their subordinates and so on until all employees know about the urgent news.

The i-th employee needs informTime[i] minutes to inform all of his direct subordinates (i.e After informTime[i] minutes, all his direct subordinates can start spreading the news).

Return the number of minutes needed to inform all the employees about the urgent news.
</em>

approach:

- the structure forms a tree with the root as the headID
- dfs to traversal all nodes and get the max time.

```cpp
    int numOfMinutes(int n, int headID, vector<int>& manager, vector<int>& informTime) {
        //the depth of the tree structure with each 
        vector<vector<int>> adj(n);
        for(int i=0;i<manager.size();i++)
            if(manager[i]>=0) adj[manager[i]].push_back(i);
        //dfs to get the max sum
        int ans=0;
        //cout<<"OK";
        return dfs(adj,informTime,headID);
        
    }
    int dfs(vector<vector<int>>& adj,vector<int>& time,int root){
        int ans=0;
        for(int ch: adj[root]){
            ans=max(ans,time[root]+dfs(adj,time,ch));
        }
        return ans;
    }
```

### 1377. Frog Position After T Seconds
<em>
Given an undirected tree consisting of n vertices numbered from 1 to n. A frog starts jumping from the vertex 1. In one second, the frog jumps from its current vertex to another unvisited vertex if they are directly connected. The frog can not jump back to a visited vertex. In case the frog can jump to several vertices it jumps randomly to one of them with the same probability, otherwise, when the frog can not jump to any unvisited vertex it jumps forever on the same vertex. 

The edges of the undirected tree are given in the array edges, where edges[i] = [fromi, toi] means that exists an edge connecting directly the vertices fromi and toi.

Return the probability that after t seconds the frog is on the vertex target.
</em>

Approach:

- it forms a n-ary tree.
- bfs to get layer by layer
- when T elapsed, and node is not reached, probability is 0
- when T elasped, and node is reached, probability is PI(1/Ni) Ni is the number of node at layer i.
- when T not elapsed, and node is reached, probability is 0
wrong: Ni shall not be the number of nodes in each layer, only the subtree.
it is easy from target to root, but we need also number of child and a directed tree.
dfs is better suited.

```cpp
class Solution {
public:
    double frogPosition(int n, vector<vector<int>>& edges, int t, int target) {
        vector<vector<int>> tree(n);
        for(auto t: edges){
            tree[t[0]-1].push_back(t[1]-1);
            tree[t[1]-1].push_back(t[0]-1);
        }
        target--;
        vector<bool> v(n);
        return dfs(tree,0,t,target,n,v);
    }
    double dfs(vector<vector<int>>& tree,int root,int t,int target,int n,
               vector<bool>& v){
        double ans=0.0;
        //cout<<root+1<<": "<<t<<endl;
        if(t==0) return root==target?1:0;
        if(t<0) return 0;
        v[root]=1;
        int nchild=tree[root].size()-1;
        if(!root) nchild++;
        if(nchild==0) return t>=0 && target==root?1:0;//it is a leaf node, nowhere to go.
        for(int child: tree[root]){
            if(v[child]==0){
                double tt=dfs(tree,child,t-1,target,n,v)/nchild;
                ans+=tt;
            }
        }
        //cout<<"Prob: "<<root+1<<" "<<ans<<endl;
        return ans;
    }
};
```
- note: when the target is a leaf node, and t>=0, then shall return 1.0

