# Topic 4: Dynamic Programming Pattern II

In this post I will cover some harder dp problems.

## count number of ways
The idea: to reach current element, add all previous incoming path.
This type of problem you still need sufficient flexibility and intuition.

Focus on how to analyze and approach the problem.

-1641 Count Sorted Vowel Strings **
problem: number of ways for vowel strings (each string is sorted).
ending with dp, only add previous smaller ones.
dp[i,c]+=dp[i,x] x<=c.

-790	Domino and Tromino Tiling    		39.7%	Medium	*****
given 'I' and 'L' two types of tile. return the number of ways to tile a 2xN board.
ending with 'I' and 'L', two states interlaced.
final answer is ending with 'I'.

-115	Distinct Subsequences    		39.1%	Hard	***
find the number of distinct subsequence of s equals t. (position different)
to reach i,j: s[i]==t[j] you can choose to use it or not! 
dp[i,j]=dp[i-1,j]+dp[i-1,j-1]

-940	Distinct Subsequences II    		41.5%	Hard	***
count number of distinct non-empty subsequence of s.
distinct--ending with type of char. 
add current char to all previous ending with subproblems.

-647. Palindromic Substrings -dp or non-dp.

-730. Count Different Palindromic Subsequences *****
similar to 940, dp[i,j,c] represent the number of palindrome subsequence in [i,j]
c=s[i]==s[j]: dp[i,j,c]+=dp[i+1,j-1,x], it also add c and cc.
s[i]!=s[j]: dp[i,j,x]=dp[i+1,j,x]+dp[i,j-1,x]-dp[i+1,j-1,x]

-552	Student Attendance Record II    		37.0%	Hard	****
A separate case
ending with P,PL,PLL are all valid. or add the string to all previous valid string.
so dp[i]=dp[i-1]+dp[i-2]+dp[i-3].

-920	Number of Music Playlists    		47.4%	Hard	****
N songs, L=list length, song can be repeated after K other songs have played
dp[i,l] represent the number of playlists using i songs, k and l.
how do we reach this? 
i-1 songs used, then we can add the song i, i*dp[i-1,l-1] (any song can be the end)
i songs played, then we have i-k songs to choose, (k-i)*dp[i,l-1]

-1524	Number of Sub-arrays With Odd Sum    		38.9%	Medium	***
also can use two states transfer pattern.

-1155	Number of Dice Rolls With Target Sum    		47.7%	Medium	***
d dices, each dice f faces number from 1 to f. number of ways to reach target.
dp[i,w] using i dices to reach w.
dp[i,w]=dp[i-1,w-1]+dp[i-1,w-2]+....dp[i-1,w-f]

-1223	Dice Roll Simulation    		46.8%	Hard	****
a typical problem with many constraints:
you cannot roll i for rollMax[i] consecutive times. get the number of unique sequences using n rolls
unique: ending with dp pattern.

dp[i,j] represents the number of ways to use i rolls ending with j:
then we have multiple incoming path:
previous not j:
with i-1,i-2....i-rollmax[j]. Note when i<=rollMax there is no previous other digits so we shall add 1.

-634	Find the Derangement of An Array    		40.3%	Medium	****
number of derangement of 1 to n (no element is in its original position).
the relation is hard to find:
put i at j, j at the end, (i-1)*dp[i-2] (two fixed)
put i at j, but j not at the end, (i-1)*dp[i-1] (one fixed)

-629	K Inverse Pairs Array    		31.4%	Hard	*****
number of permutation of 1 to n with exact k inversions.
thinking when adding element i to the array, how do we affect the inversion.
we can put it at any positions and create different number of inversions. Thus we have dp[i-1,k-1],dp[i-1,k-2].....dp[i-1,1] to reach to dp[i,k]. 

-1359	Count All Valid Pickup and Delivery Options    		57.1%	Hard	****
pickup must be ahead of delivery. 
assume we are arranging ith pair, the pickup have 2i-1 options, We then arrange the delivery, and it has 2i options if not consider the order. Considering the order is 2i/2=i. dp[i]=i*(2i-1)*dp[i-1]

-1575	Count All Possible Routes    		58.4%	Hard	***
n cities on a line. given start and destination and start fuel, return the number of route from start to destination. 
dp[i,f]+=dp[k,f-dist(i,k)]: fp[i,f] represents the number of cities to go with fuel f. 
base case: any fuel if located (start) from finish you get one valid route.
dp[finish,f]=1 f from 0 to fuel.

-1621	Number of Sets of K Non-Overlapping Line Segments    		41.3%	Medium	
also a dp problem of partition to k segments.

-1569	Number of Ways to Reorder Array to Get Same BST    		50.0%	Hard ****
insert the elements in array by order. 
You can choose to insert left or right first.
also need math combinations. This is pretty hard one.
	
-1692. Count ways to distribute candies
dp[i,k] represent number of ways to distribute i candies into k bags
dp[i,k]=k*dp[i-1,k]+dp[i-1,k-1]

-1639	Number of Ways to Form a Target String Given a Dictionary    		38.9%	Hard	
choose a column or not, for each column you can choose different rows.

Practices:
-576	Out of Boundary Paths    		35.5%	Medium	
-935	Knight Dialer    		45.9%	Medium	
-1220	Count Vowels Permutation    		53.8%	Hard	
-91	Decode Ways    		25.5%	Medium	
-639	Decode Ways II    		27.2%	Hard	
-1269	Number of Ways to Stay in the Same Place After Some Steps    		43.1%	Hard	
-1411	Number of Ways to Paint N × 3 Grid    		60.6%	Hard	
-1259	Handshakes That Don't Cross    		53.9%	Hard	

-1397	Find All Good Strings    		37.9%	Hard	
number of string in range not containing the evil string.
-1444	Number of Ways of Cutting a Pizza    		53.4%	Hard	
-1416	Restore The Array    		36.2%	Hard	
numbers printed without space, return number of ways to restore it. the number is in [1,K].
-1301	Number of Paths with Max Score    		37.7%	Hard	
two subproblems: one is to get the max score, one is to get the number of ways to reach the max score.

example code:
-1223	Dice Roll Simulation    		46.8%	Hard	****
current roll ending with j, then to satisfy the max repeat number,
d[i,j]+=dp[i-1,k] k!=j.
when there is no previous different roll, i<=MaxRepeat, we need add 1.

```
    int dieSimulator(int n, vector<int>& rollMax) {
        vector<vector<int>> dp(n+1,vector<int>(6));
        for(int i=0;i<6;i++) dp[1][i]=1; //base case: 1 roll
        int mod=1e9+7;
        for(int i=2;i<=n;i++){
            for(int j=0;j<6;j++){ //ending with j
                for(int k=0;k<6;k++){ 
                    if(j==k) continue;//previous shall not be j
                    for(int l=1;l<=rollMax[j] && i>=l;l++) 
                        dp[i][j]+=dp[i-l][k],dp[i][j]%=mod;
                }
                if(i<=rollMax[j]) dp[i][j]++; //no previous other digits
                //n=2, repeat 2, it shall have 1x,2x,3x,4x,5x,6x 6 instead of 5
            }
        }
        long ans=0;
        for(int i=0;i<6;i++) ans+=dp[n][i],ans%=mod;
        return ans;
    }
```	

### knapsack
choose some elements (repeated or non-repeated) to reach a target or closest to target.
each element has two options, to be included or excluded.
- two options knapsack
- multiple option knapsack
- non-repeatitive: generally a 2d approach
- repeatitive: generally a 2d approach but can also use 1d approach (also can be reduced from 2d approach).
- convert to equivalent knapsack problem.

-518	Coin Change 2    		51.0%	Medium	****
coins can be reused. a 2d knapsack -> 1d knapsack
dp[i,j] to get amount i by using the first j coins.
dp[i,j]=dp[i,j-1]+dp[i-c,j] use or not use current coin (two paths)
to reduce j, dp[i]+=dp[i-c] the j loop shall be the outer loop.
for 2d, loop order does not matter, but for 1d it matters (optimized).
for repeatitive knapsack, you can always get it by 1d:
outer loop is coin: we add one new coin we apply it to all amount (and the new amount is derived from smaller amount, so coin is reused)
if outer loop is amount, then for a new amount, we update only each [w-c] thus non-repeative.

-1155	Number of Dice Rolls With Target Sum    		47.7%	Medium	
d dices, each dice f faces number from 1 to f. number of ways to reach target.

-494	Target Sum    		46.0%	Medium	
number of ways to get target sum applying +/- sign to each element.
convert to equivalent knapsack 
-879	Profitable Schemes    		39.9%	Hard	
-956	Tallest Billboard    		39.8%	Hard	
-1049	Last Stone Weight II    		44.3%	Medium	
-691	Stickers to Spell Word    		43.5%	Hard
-805	Split Array With Same Average    		26.5%	Hard	
check if possible to divide into two subsets with same average.
take m elements to A: 
sum(Ai)/m=Sum(Aj)/(n-m) ->
Sum(Ai)*(n-m)=Sum(Aj)*m ->
Sum(Ai)*n=m(Sum(Ai)+Sum(Aj))=m*T (T is the total sum)
Sum(Ai)=mT/n and now this is a knapsack problem with multiple possible targets
- using m elements to get target sum, combination sum using backtracking
- using 3d dp + knapsack: dp[i,j,target] array length i, j elements chosen, and target.
recommend using the first approach which is easier.

### shortest distance problem
bfs: for all same weight shortest distance problem, which will not be covered here.

dijkstra: using priority_queue, src node to all other nodes with non-negative weight (similar to bfs with visited array to terminate when reach the target).
dijkstra complexity O(V+ElogV). node is push/pop into min heap and each edge is tried.

bellman-ford: using all nodes and edges, src node to all other nodes, Bellman-ford can deal with negative weight.
complexity: O(VE)

floyd-warshal: find all pair node shortest distance using dp.
traveling salesman problem.

#### dijkstra for shortest distance:

a pattern for dijkstra:

- only give the source to source distance (other is not visited do not init the distance, otherwise will prevent adding the node into pq)
- visited array, only when the node is popped, and is visited. (since we will process its neighboring). It often causes problem when used improperly. With smaller distance pushed to pq, this is often not required.
- relax the distance using current node's neighboring nodes.

```
   int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<vector<pair<int,int>>> adj(n+1);
        for(auto e: times){
            adj[e[0]].push_back({e[1],e[2]});
        }
        vector<int> dp(n+1,INT_MAX),v(n+1);
        dp[k]=0;
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> pq;
        pq.push({0,k});
        while(pq.size()){
            auto p=pq.top();
            pq.pop();
            int node=p.second,dist=p.first;
            v[node]=1;
            for(auto e: adj[node]){
                int next=e.first,d=e.second;
                if(v[next]==0){
                    if(dp[next]>dp[node]+d){
                        dp[next]=dp[node]+d;
                        pq.push({dp[next],next});
                    }
                }
            }
        }
        int ans=*max_element(begin(dp)+1,end(dp));
        return ans==INT_MAX?-1:ans;
    }
```	

-505	The Maze II    		48.1%	Medium	
shortest distance from source to destination (with weight). dijkstra.
-499	The Maze III    		41.8%	Hard	
add a hole, return the shortest distance to drop in the hole.
dijkstra.
-743	Network Delay Time    		45.1%	Medium	

### bellman-ford.

-787	Cheapest Flights Within K Stops    		39.4%	Medium	
find src to dst shortest distance.
both dijkstra and bellman ford can apply.

```
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K)
    {
        K++;
        vector<vector<int>> dp(n, vector<int>(K+1,INT_MAX));
        dp[src][0] = 0;
        for(int i = 1; i <= K; i++)
        {
            for(int j = 0; j < n; j++)   //To update ans[j][i](using i steps), we copy ans[j][i-1] first
                dp[j][i] = dp[j][i-1];
            for(auto f: flights) //iterate all flights: f[0]: from, f[1]: to, f[2], price
            {
                if(dp[f[0]][i-1]<INT_MAX)
                dp[f[1]][i] = min(dp[f[1]][i], dp[f[0]][i-1] + f[2]);
            }
        }
        if(dp[dst][K] == INT_MAX) return -1;
        return dp[dst][K];
    }
```	

### shortest distance visiting all nodes 
generally we have to use bitmask dp: dp[i,state] for adding node i to have a state.
the idea: to add current node, try all previous combinations.

-847	Shortest Path Visiting All Nodes    		52.7%	Hard	
equal weight using bfs + bitmask dp.
```
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
-1125	Smallest Sufficient Team    		46.9%	Hard	
need satisfy all skills so uses bitmask. 
```cpp
    vector<int> smallestSufficientTeam(vector<string>& req_skills, vector<vector<string>>& people) {
		int n = req_skills.size();
        int m=people.size();
		vector<vector<int>> dp(1<<n);  // using unordered_map, we improve on time
        unordered_map<string,int> skill_map;
        for(int i=0;i<n;i++)
            skill_map[req_skills[i]]=i;
        vector<int> skills(m);
        for(int i=0;i<m;i++){
            int curr_skill = 0;
            for(int j=0;j<people[i].size();j++)
                curr_skill |= 1<<skill_map[people[i][j]];
            skills[i]=curr_skill;
        }
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<(1<<n);j++)
            {
                int k = (dp[j].empty()?0:j) | skills[i];
                if(dp[k].size()==0 || dp[k].size()>1+dp[j].size()) 
                    //check if add this person can relax
                {
                    dp[k]=dp[j]; //using its combination
                    dp[k].push_back(i);//add current person
                    //cout<<i<<" "<<j<<" "<<k<<endl;
                }       
            }
        }
        return dp[(1<<n) -1];        
    }
```

	
-943	Find the Shortest Superstring    		43.2%	Hard	
the idea: using bitmask to record the strings chosen. To add current string, we get previous state and ending with all possible previous string.
traveling salesman problem: each node is visited once, and the sequence order matters.
```
    string shortestSuperstring(vector<string>& A) {
        int n=A.size(); //number of nodes
        vector<vector<int>> graph(n,vector<int>(n));
        for(int i=0;i<n;i++)
        {
            for(int j=i+1;j<n;j++)
            {
                graph[i][j]=calc(A[i],A[j]);
                graph[j][i]=calc(A[j],A[i]);
            }
        }
        //dp: also need keep the path
        //other dimension shall be the bitset
        int m=1<<n;
        vector<vector<int>> dp(m,vector<int>(n,INT_MAX)),path(m,vector<int>(n));
        int last=-1,gmin=INT_MAX;
        //dp[i,j]: min distance starting from node j and visiting other nodes indicated in the bitset
        for(int state=1;state<m;state++) //0x0001 to 0xffff
        {
            for(int j=0;j<n;j++)
            {
                if(state & (1<<j)) //if node j is visited
                {
                    int prev=state^(1<<j); //previous status
                    if(prev==0) dp[state][j]=A[j].length();
                    else //try to use other edge to relax  
                    {
                        for(int k=0;k<n;k++)
                        {
                            //if(dp[prev][k]<INT_MAX && dp[state][j]>dp[prev][k]+graph[k][j])
                            if((prev&(1<<k)) && dp[state][j]>dp[prev][k]+graph[k][j])
                            {
                                dp[state][j]=dp[prev][k]+graph[k][j];
                                path[state][j]=k; //save the node
                            }
                        }
                    }
                }
                if(state==m-1 && gmin>dp[state][j]) {gmin=dp[state][j];last=j;}
            }
        }

        //backtrace to get the results, path stored in path[i][j]
        int curr=m-1; //0xffff
        vector<int> seq;
        cout<<last<<endl;
        while(curr)
        {
            seq.push_back(last);
            int t=path[curr][last];
            curr-=(1<<last);
            last=t;
        }
        //now connect the strings
        string ans=A[seq.back()];//the first
        for(int i=seq.size()-2;i>=0;i--)
        {
            //cout<<seq[i]<<": ";
            int num_app=graph[seq[i+1]][seq[i]];
            int len=A[seq[i]].length();
            //cout<<len<<" "<<num_app<<endl;
            ans+=A[seq[i]].substr(len-num_app);
        }
        return ans;
    }
    
    int calc(string& a,string& b)
    {
        int m=a.size(),n=b.size();
        for(int i=1;i<a.size();i++) //no duplicates
        {
            if(b.substr(0,m-i)==a.substr(i)) return n-(m-i);
        }
        return n;
    }
```	
### bitmask dp
bitmask dp is used for those problems when the subproblem is dependent on what the combination is. The complexity is generally O(n*2^n).
If you do backtracking and found that you need use a visited array and wants to add memoization, that is where bitmask dp for.
another hint: since it is exponential, so the number of nodes is generally small.

-526	Beautiful Arrangement    		59.3%	Medium	
```
    int countArrangement(int N) {
        int mask = 1<<15;
        vector<vector<int>> dp (N, vector<int>(mask,-1));
        return countBeautifulArray(0,0,dp,N);    
    }

    int countBeautifulArray(int pos, int mask, vector<vector<int>>& dp,int N){
        if(pos == N) return 1;
        if(dp[pos][mask]!=-1) return dp[pos][mask];

        int ans =0;
        for(int i = 0; i< N; i++){
            if( (mask & (1<<i))==0){
                if((i+1)%(pos+1)==0  ||(pos+1)%(i+1)==0)
                ans += countBeautifulArray(pos+1, mask|(1<<i), dp,N);
            }
        }
        return  dp[pos][mask] = ans;
    }
```
	
-1066	Campus Bikes II    		54.0%	Medium	
```
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

-1542	Find Longest Awesome Substring    		36.0%	Hard	
why cannot use sliding window, because adding one or removing one will not make it valid or invalid.
- a valid subarray (using bit xor) will get <=1 set bits. (10bits)
- xor is similar to add, we can use prefix xor to get range xor.
- this will be O(N^2)
optimization: 
- similar to hashmap to find same mask index difference or mask with 1 set bit difference.

```cpp
int longestAwesome(string s) {
    vector<int> dp(1024, s.size()); //set them as max.
    int res = 0, mask = 0;
    dp[0] = -1;
    for (auto i = 0; i < s.size(); ++i) {
        mask ^= 1 << (s[i] - '0');
        res = max(res, i - dp[mask]); //same mask
        for (auto j = 0; j <= 9; ++j) //mask with 1 bit difference.
            res = max(res, i - dp[mask ^ (1 << j)]);
        dp[mask] = min(dp[mask], i);
    }
    return res;
}
```

-1349	Maximum Students Taking Exam    		43.3%	Hard	
-1434	Number of Ways to Wear Different Hats to Each Other    		38.9%	Hard	
two factors and can use both as bitmask, then need use the smaller one. Convert the input so that bitmask on the smaller one can be done.

-1494	Parallel Courses II    		31.0%	Hard	
-1595	Minimum Cost to Connect Two Groups of Points    		41.9%	Hard	
-1659	Maximize Grid Happiness    		29.1%	Hard	

### need greedy and optimization in dp.
