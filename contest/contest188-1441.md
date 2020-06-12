## contest 188

### 1441. Build an Array With Stack Operations
<em>
Given an array target and an integer n. In each iteration, you will read a number from  list = {1,2,3..., n}.

Build the target array using the following operations:

Push: Read a new element from the beginning list, and push it in the array.
Pop: delete the last element of the array.
If the target array is already built, stop reading more elements.
You are guaranteed that the target array is strictly increasing, only containing numbers between 1 to n inclusive.

Return the operations to build the target array.

You are guaranteed that the answer is unique.
</em>

straightforward, just simulate it.
if the number is not inside the target array, push it first and then pop it.

```cpp
    vector<string> buildArray(vector<int>& target, int n) {
        vector<string> ans;
        int start=1;
        for(int i: target){
            for(int j=start;j<i;j++){
                ans.push_back("Push");
                ans.push_back("Pop");
            }
            ans.push_back("Push");
            start=i+1;
        }
        return ans;
    }
```

### 1442. Count Triplets That Can Form Two Arrays of Equal XOR
<em>
Given an array of integers arr.

We want to select three indices i, j and k where (0 <= i < j <= k < arr.length).

Let's define a and b as follows:

a = arr[i] ^ arr[i + 1] ^ ... ^ arr[j - 1]
b = arr[j] ^ arr[j + 1] ^ ... ^ arr[k]
Note that ^ denotes the bitwise-xor operation.

Return the number of triplets (i, j and k) Where a == b.
</em>

first intuition: using prefix xor (similar to prefix sum)
prefix(i+1)=arr[0]^......^arr[i]	
then prefix(i)^prefix(j)=arr[j]^.....arr[i-1]
then we can loop over j (from 1 to n-1) and separate into three parts
(note the right part can collapse to mid)

```cpp
    int countTriplets(vector<int>& arr) {
        //do it like prefix sum
        //xor(i,j)=pre(j)^pre(i)
        int n=arr.size();
        int ans=0;
        vector<int> pre(n+1);
        for(int i=0;i<n;i++) pre[i+1]=pre[i]^arr[i];
        
        //j start from 1 to n-1 and divide into left and right
        for(int j=1;j<n;j++){
            //left
            //cout<<j<<endl;
            unordered_map<int,int> left;
            for(int i=0;i<j;i++){ //i to j-1
                int t=pre[j]^pre[i];
                left[t]++;
            }
            for(int k=j;k<n;k++){ //j to k, K>=j
                int t=pre[k+1]^pre[j];
                if(left.count(t)) ans+=left[t];
            }
        }
        return ans;
    }
```	
complexity O(N^2)

more optimization -> O(N)
a==b.
means a^b=0, ie from i to k the xor is 0, or we are searching for the number of xor is the same as previous
pre[i]^pre(k+1)=0 with i<j<=k, equivalently the index distance>=2.
j can be any index from i+1 to k so the answer would be k-i-1 for each found region
for example we found three same xor at location a,b,c.
for b: we get b-a-1
for c: we get c-b-1+c-a-1=2*(c-b-1)+(b-a-1)
```cpp
    int countTriplets(vector<int>& A) {
        int n = A.size(), res = 0, prefix = 0;
        unordered_map<int, int> count = {{0, 1}}, total;
        for (int i = 0; i < n; ++i) {
            prefix ^= A[i];
            res += count[prefix]* i - total[prefix];
			count[prefix]++;
            total[prefix] += i + 1;
        }
        return res;
    }
```

### 1443. Minimum Time to Collect All Apples in a Tree
<em>
Given an undirected tree consisting of n vertices numbered from 0 to n-1, which has some apples in their vertices. You spend 1 second to walk over one edge of the tree. Return the minimum time in seconds you have to spend in order to collect all apples in the tree starting at vertex 0 and coming back to this vertex.

The edges of the undirected tree are given in the array edges, where edges[i] = [fromi, toi] means that exists an edge connecting the vertices fromi and toi. Additionally, there is a boolean array hasApple, where hasApple[i] = true means that vertex i has an apple, otherwise, it does not have any apple.

 </em>

Intuition:

- build a undirectional graph
- dfs from 0 and form a directional tree
- postorder tree traversal. if subtree sum is 0, then root has apple shall be counted, no apple, not counted
if subtree sum is not 0, root node shall be counted
- we are counting edges, so final result shall -1
- return to node 0 we shall double the results.

```cpp
    int minTime(int n, vector<vector<int>>& edges, vector<bool>& hasApple) {
		vector<vector<int>> adj(n);
		for(auto e: edges){
			adj[e[0]].push_back(e[1]);
			adj[e[1]].push_back(e[0]);
		}
		return max(2*(postorder(adj,0,-1,hasApple)-1),0);
    }
	int postorder(vector<vector<int>>& adj,int root,int parent,vector<bool>& hasApple){
		if(adj[root].size()==1 && parent>=0) return hasApple[root];//leaf
		int ans=0;
		for(int child: adj[root]){
			if(child!=parent){
				ans+=postorder(adj,child,root,hasApple);
			}
		}
		if(ans || hasApple[root]) ans++;
		return ans;
	}
```	

Note: leaf only has one child, but the root sometimes may also has only one child.

### 1444. Number of Ways of Cutting a Pizza
<em>
Given a rectangular pizza represented as a rows x cols matrix containing the following characters: 'A' (an apple) and '.' (empty cell) and given the integer k. You have to cut the pizza into k pieces using k-1 cuts. 

For each cut you choose the direction: vertical or horizontal, then you choose a cut position at the cell boundary and cut the pizza into two pieces. If you cut the pizza vertically, give the left part of the pizza to a person. If you cut the pizza horizontally, give the upper part of the pizza to a person. Give the last piece of pizza to the last person.

Return the number of ways of cutting the pizza such that each piece contains at least one apple. Since the answer can be a huge number, return this modulo 10^9 + 7.

</em>

Intuition:

clearly this is a dp problem. Every step we have two option: cut horizontally or vertically
every time we need to know: 
cut horizontally, the top piece and remaining piece shall both have apples
cut vertically, the left piece and remaining piece shall both have apples

conveniently if we know the remaining apples,

- first build a postfix sum to get to know number of apples in the matrix from [i,j] to [m-1,n-1]
- recursive dp, depends on k, i, j.

```cpp
    int mod=1e9+7;
    int ways(vector<string>& pizza, int k) {
        //dp: everytime we have two choices, 
        //try recursive 
        //state k, i,j (the top left corner coord)
        int m=pizza.size(),n=pizza[0].size();
        vector<vector<int>> app(m+1,vector<int>(n+1));
        vector<vector<vector<long>>> dp(m,vector<vector<long>>(n,vector<long>(k)));
        for(int i=m-1;i>=0;i--){
            for(int j=n-1;j>=0;j--){
                app[i][j]=app[i+1][j]+app[i][j+1]-app[i+1][j+1]+(pizza[i][j]=='A');
            }
        }
        return backtrack(app,k-1,0,0,dp)%mod;
    }
    long backtrack(vector<vector<int>>& app,int k,int i,int j,vector<vector<vector<long>>>& dp){
        if(k==0) {
            return app[i][j]>0;
        }
        if(app[i][j]<k) return 0; //there is no apple
        if(dp[i][j][k]) return dp[i][j][k];
        //now we want apply a cut
        //first cut horizontally
        long ans=0;
        for(int cut=i+1;cut<app.size()-1;cut++){
            //it cut into two parts, both need satisfy the condition
            int t=app[i][j]-app[cut][j];
            if(t==0) continue;
            ans+=backtrack(app,k-1,cut,j,dp)%mod;
            ans%=mod;
        }
        //or we apply cut vertically
        for(int cut=j+1;cut<app[0].size()-1;cut++){
            int t=app[i][j]-app[i][cut];
            if(t==0) continue;
            ans+=backtrack(app,k-1,i,cut,dp)%mod;
            ans%=mod;
        }
        return dp[i][j][k]=ans%mod;
    }
```	

