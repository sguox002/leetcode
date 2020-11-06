## heap

c++ heap data structure
set/multiset: inner data structure red-black tree or balanced Binary search tree (BST)
map/multimap: same as set/multiset
priority_queue: using heap data structure (array, full binary tree)
priority_queue uses make_heap/push_heap/pop_heap and a inside data array.

so set/map is good for traversal
priority_queue is good for push/pop.

## priority_queue and heap

input: [10,20,30,5,15]
heap: [30,20,10,5,15]
    30
   /  \
  20  10
 /  \
5  15

pop_heap: [20,15,10,5] it will swap 30 with 15, and then sink 15 down
    30         
   /  \
  20  10  -->swap 3o with back 15
 /  \
5  15

    15
   /  \
  20  10  -->remove back and sink down the root
 /  
5  

    20
   /  \
  15  10 -->[20,15,10,5]
 /  
5  

push_heap [99,20,10,5,15]
    20
   /  \
  15  10 -->add 99 to back
 /  
5  
    20
   /  \
  15  10  --> sieve back up
 /  \ 
5   99

    20
   /  \
  99  10 -->sieve up
 / \
5  15

    99
   /  \
  20  10 -->[99,20,10,5,15]
 / \
5  15

using full binary tree, each node's index is fixed, and swapping nodes are random access.

## red-black tree
a red–black tree is a kind of self-balancing binary search tree. 
Each node stores an extra bit representing color, used to ensure that the tree remains approximately balanced during insertions and deletions.
set/map inherits the tree data structure.
When the tree is modified, the new tree is rearranged and repainted to restore the coloring properties that constrain how unbalanced the tree can become in the worst case. The properties are designed such that this rearranging and recoloring can be performed efficiently.

The re-balancing is not perfect, but guarantees searching in O(log n) time, where n is the number of nodes of the tree. The insertion and deletion operations, along with the tree rearrangement and recoloring, are also performed in O(log n) time.[3]


some facts:
- heap can push/pop and size can be reduced (in some cases, only partial data is needed to keep, this is good)
- they all uses tree data structure, insert/delete are all log(n)
- build a heap is less efficient than sort.

heap related algorithms
- dijkstra

common practice:
- max/min heap
- using composited data structure.
- iterate set/map
- sort along one dimension and then using heap along other dimension

leetcode heap related practice questions

1642. Furthest Building You Can Reach ***
using bricks or ladders, prefer ladders for larger jump and bricks for smaller jump.
- binary search using greedy. (using heap is less efficient than sort, so heap will TLE).
- approach 1: max heap, use bricks first, then replace ladder with the largest bricks jump.
- approach 2: min heap, when ladders are used up, pop the smallest jump for bricks.

1439. Find the Kth Smallest Sum of a Matrix With Sorted Rows ****
rows are sorted. choose one element from each row and get the kth smallest sum.
- the first column is the smallest (0,0)...(m-1,0)
- add all its neighboring elements into heap
- choose the min.
- we shall use visited to avoid revisit some combinations.
use hashmap (convert to string) for the visited
or use set to store the vector for the visited.

this is so similar to bfs or dijkstra

1631. Path With Minimum Effort ****
get the minmimum max absolute difference from top left to bottom right.
dijkstra: using pq and tries the smallest difference first. record all difference,
- for x,y, add its neighborings into pq, and update the effort
```
    int minimumEffortPath(vector<vector<int>>& heights) {
        int m=heights.size(),n=heights[0].size();
        vector<vector<int>> dp(m,vector<int>(n,INT_MAX));
        priority_queue<vector<int>,vector<vector<int>>,greater<vector<int>>> pq;
        pq.push({0,0,0});//effort, coordinate
        int dir[][2]={{-1,0},{1,0},{0,-1},{0,1}};
        //dp[0][0]=0;
        while(pq.size()){
            auto vt=pq.top();
            pq.pop();
            int eff=vt[0],x=vt[1],y=vt[2];
            //cout<<x<<" "<<y<<" "<<eff<<endl;
            if(x==m-1 && y==n-1) return eff;
            if(eff>=dp[x][y]) continue; //prune, discard this path
            dp[x][y]=min(dp[x][y],eff);
            for(auto d: dir){
                int x0=x+d[0],y0=y+d[1];
                if(x0<0||y0<0||x0>=m||y0>=n) continue;
                int tmp=max(eff,abs(heights[x0][y0]-heights[x][y]));
                pq.push({tmp,x0,y0});
            }
        }
        return dp[m-1][n-1];
    }
```
- it allows 4 directions
- if path are greater than previous, discard it.
- do not need visited. eff>=dp[x,y] to avoid revisit (but it is pushed twice).
- O(m*n*log(mm))
TLE: without visited, those visited elements are pushed more than once.
```cpp
    int minimumEffortPath(vector<vector<int>>& heights) {
        int m=heights.size(),n=heights[0].size();
        vector<vector<int>> dp(m,vector<int>(n,INT_MAX));
        priority_queue<vector<int>,vector<vector<int>>,greater<vector<int>>> pq;
        pq.push({0,0,0});//effort, coordinate
        int dir[][2]={{-1,0},{1,0},{0,-1},{0,1}};
        vector<bool> v(m*n);
        v[0]=1;
        while(pq.size()){
            auto vt=pq.top();
            pq.pop();
            int eff=vt[0],x=vt[1],y=vt[2];
            if(x==m-1 && y==n-1) return eff;
            v[x*n+y]=1;
            if(eff>=dp[x][y]) continue; //prune, discard this path
            dp[x][y]=min(dp[x][y],eff);
            for(auto d: dir){
                int x0=x+d[0],y0=y+d[1];
                if(x0<0||y0<0||x0>=m||y0>=n||v[x0*n+y0]) continue;
                int tmp=max(eff,abs(heights[x0][y0]-heights[x][y]));
                pq.push({tmp,x0,y0});
            }
        }
        return dp[m-1][n-1];
    }
```
visited is a bit different than the bfs, need wait for the min done to set it true
(4 directions are all pushed in and choose the min one).

binary search + bfs is faster. 
binary search + dfs fastest. (less memory and faster). the worst case will try all paths.
binary search + union find (check if two nodes are connected).


1054.distant barcode ***
arrange so neighboring is different. --several other similar questions
greedy: arrange most frequent first.

1046. Last Stone Weight *
straightforward max heap.

973. K Closest Points to Origin **
using pq to keep only k points, pop those largest.

882. Reachable Nodes In Subdivided Graph

871. Minimum Number of Refueling Stops *****
a list of stations with positions and amount of gases. check if we can drive to target with given initial fuel.
- we need to be able to reach the station to use its gas.
- bfs: is not quite right since we only use one station (which has the most gas), but in order to reach target, we can use more than one in the range.
ie we have more choices. if n stations in the range, we can choose 1,2...n stations.

- heap: using max heap. adding those reachable stations into pq, choose the max if there are stations in the pq.
heap approach is more understandable.
```cpp
	int minRefuelStops(int target, int startFuel, const std::vector<std::vector<int>>& stations) {
        int gas = startFuel;
        int stops = 0;
        int last_reachable = 0; // Index of last reachable station - 1. 
    
        priority_queue<int> pq;
    
        while(gas < target){
            // Visit reachable stations.
            while(last_reachable<stations.size() && gas>=stations[last_reachable][0]){
                pq.push(stations[last_reachable++][1]);
            }
            if(pq.empty()) return -1;
            
            gas += pq.top(); //pick the best
            pq.pop();
            ++stops;
        }
        
        return stops;
    }
```	
- dp approach: dp[t] the largest distance using t stations for refuel. 
then dp[t+1]=max(dp[t+1],dp[t]+s[i][1])

864. Shortest path to get all keys
given a 2d grid with keys and locks pair, and a start position. You have to carry the key to open the corresponding lock. Afte unlock, we can walk over it.
find the min number of moves to acquire all keys.
idea: assume we have a set of keys, we are looking for the min moves to get other remaining keys. 
The next step we may have a set of keys reachable, which one to pick?
We may use greedy to choose the closest one.
this one shall mark as bfs problem + bitset for the state.








