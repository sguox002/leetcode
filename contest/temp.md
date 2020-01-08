## contest 147
### 1137. N-th Tribonacci Number (*)
trival, using O(1) space and O(n) time.

### 1138. Alphabet Board Path (***)
<em>Problem:

On an alphabet board, we start at position (0, 0), corresponding to character board[0][0].

Here, board = ["abcde", "fghij", "klmno", "pqrst", "uvwxy", "z"], as shown in the diagram below.
We may make the following moves:

'U' moves our position up one row, if the position exists on the board;
'D' moves our position down one row, if the position exists on the board;
'L' moves our position left one column, if the position exists on the board;
'R' moves our position right one column, if the position exists on the board;
'!' adds the character board[r][c] at our current position (r, c) to the answer.
(Here, the only positions that exist on the board are positions with letters on them.)

Return a sequence of moves that makes our answer equal to target in the minimum number of moves.  You may return any path that does so.
</em>

Approach: using bfs is overkill for this problem. Manhatton distance is the shortest.
The alphaboard is fixed from 0 to 25 fixed by 5 cols. But be careful the last row, you have to go up down first. 
if you start from 'z', you first have to pass 'u'
if you go to 'z', you first have to go to 'u'

```cpp
    string alphabetBoardPath(string target) {
		int cx=0,cy=0;
		char prev=-1;
		string ans;
		for(char c: target){
			if(c=='z'&&prev=='z') {ans+='!';continue;} //do not always have to go to 'u'
			if(c=='z' ||prev=='z') ans+=nextchar('u',cx,cy); //dest is z.
			ans+=nextchar(c,cx,cy);
			ans+='!';
			prev=c;
		}
		return ans;
    }
	string nextchar(char c,int &cx,int &cy){
		string ans;
		int t=c-'a';
		int row=t/5,col=t%5;
		int dx=row-cx,dy=col-cy;
		if(dx>0){
			for(int i=0;i<dx;i++) ans+='D';
		}else{
			for(int i=0;i<-dx;i++) ans+='U';
		}
		if(dy>0){
			for(int i=0;i<dy;i++) ans+='R';
		}else{
			for(int i=0;i<-dy;i++) ans+='L';
		}
		cx=row,cy=col;
		return ans;
	}
```	
### 1139. Largest 1-Bordered Square (****)
<em>Problem:

Given a 2D grid of 0s and 1s, return the number of elements in the largest square subgrid that has all 1s on its border, or 0 if such a subgrid doesn't exist in the grid.

 

Example 1:

Input: grid = [[1,1,1],[1,0,1],[1,1,1]]
Output: 9
Example 2:

Input: grid = [[1,1,0,0]]
Output: 1
</em>

Analysys:
- return the area of the square with all boarder as 1s.
- this is very similar to other all 1s square or rectangle problem which can be solved using dp
- first from top left to bottom right and find at [i,j] the horizontal and vertical length of 1s
- then from bottom right to top left and find 

```cpp
    int largest1BorderedSquare(vector<vector<int>>& grid) {
        int m=grid.size(),n=grid[0].size();
        vector<vector<int>> hor(m,vector<int>(n)),ver(m,vector<int>(n));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]){
                    hor[i][j]=j==0?1:hor[i][j-1]+1;
                    ver[i][j]=i==0?1:ver[i-1][j]+1;
                }
            }
        }
        int maxlen=0;
        for(int i=m-1;i>=0;i--){
            for(int j=n-1;j>=0;j--){
                if(grid[i][j]){
                    int len=min({hor[i][j],ver[i][j]});
                    //check ver[i][j-len+1] and hor[i-len+1][j]
                    for(int l=len;l>=1;l--){
                        if(ver[i][j-l+1]>=l && hor[i-l+1][j]>=l){
                            maxlen=max(maxlen,l);
                            break;
                        }
                    }
                }
            }
        }
        return maxlen*maxlen;
    }
```

### 1140. Stone Game II (*****)
<em>Problem:

Alex and Lee continue their games with piles of stones.  There are a number of piles arranged in a row, and each pile has a positive integer number of stones piles[i].  The objective of the game is to end with the most stones. 

Alex and Lee take turns, with Alex starting first.  Initially, M = 1.

On each player's turn, that player can take all the stones in the first X remaining piles, where 1 <= X <= 2M.  Then, we set M = max(M, X).

The game continues until all the stones have been taken.

Assuming Alex and Lee play optimally, return the maximum number of stones Alex can get.

 

Example 1:

Input: piles = [2,7,9,4,4]
Output: 10
Explanation:  If Alex takes one pile at the beginning, Lee takes two piles, then Alex takes 2 piles again. Alex can get 2 + 4 + 4 = 10 piles in total. If Alex takes two piles at the beginning, then Lee can take all three piles left. In this case, Alex get 2 + 7 = 9 piles in total. So we return 10 since it's larger. 
</em>

dp problem with extra complexity.
```cpp
    int stoneGameII(vector<int>& piles) {
        int length = piles.size();
        vector<vector<int>>dp(length + 1, vector<int>(length + 1,0));
        vector<int> sufsum (length + 1, 0);
        for (int i = length - 1; i >= 0; i--) {
            sufsum[i] = sufsum[i + 1] + piles[i];
        }
        for (int i = 0; i <= length; i++) {
            dp[i][length] = sufsum[i];
        }
        for (int i = length - 1; i >= 0; i--) {
            for (int j = length - 1; j >= 1; j--) {
                for (int X = 1; X <= 2 * j && i + X <= length; X++) {
                    dp[i][j] = max(dp[i][j], sufsum[i] - dp[i + X][max(j, X)]);
                }
            }
        }
        return dp[0][1];
    }
```

## biweek 5
### 1133. Largest Unique Number (*)
heap map..

### 1134. Armstrong number (*)
sum(digit^k)=N

### 1135. connecting cities with min cost (***)
min spanning tree combining union find and greedy
```cpp
    vector<int> parent;
    int sz;
    int minimumCost(int N, vector<vector<int>>& connections) {
        //min spanning tree greedy or union-find
        //first need make sure they are in one set.
        parent.resize(N);
        sz=N;
        for(int i=0;i<N;i++) parent[i]=i;
        sort(connections.begin(),connections.end(),[](vector<int>& a,vector<int>& b){
            return a[2]<b[2];
        });
        int ans=0;
        for(auto t: connections){
            int pi=findp(t[0]-1),pj=findp(t[1]-1);
            if(pi!=pj){
                ans+=t[2];
                parent[pi]=pj;
                sz--;
            }
        }
        return sz==1?ans:-1;
    }
    int findp(int i){
        while(i!=parent[i]){
            parent[i]=parent[parent[i]];
            i=parent[i];
        }
        return i;
    }
```

### 1136. Parallel courses (***)
bfs source nodes in a graph. (source nodes has no incoming edges).

```cpp
    int minimumSemesters(int N, vector<vector<int>>& relations) {
        vector<unordered_set<int>> adj(N);
        for(auto r: relations){
            adj[r[1]-1].insert(r[0]-1); //incoming edges
        }
        queue<int> q;
        vector<bool> v(N);
        add_source(adj,q,v);
        int step=0;
        while(q.size()){
            int sz=q.size();
            unordered_set<int> nodes;
            while(sz--){
                int t=q.front();
                q.pop();
                for(auto& vn: adj){
                    if(vn.count(t)) vn.erase(t);
                }
            }
            add_source(adj,q,v);
            step++;
        }
        int res=accumulate(v.begin(),v.end(),0);
        
        return res==N?step:-1;
    }
    void add_source(vector<unordered_set<int>>& adj,queue<int>& q,vector<bool>& v){
        for(int i=0;i<adj.size();i++){
            if(adj[i].size()==0 && !v[i]) {
                q.push(i);
                v[i]=1;
            }
        }
    }
```
	
## contest 146
### 1128. Number of Equivalent Domino Pairs (**)
<em>Given a list of dominoes, dominoes[i] = [a, b] is equivalent to dominoes[j] = [c, d] if and only if either (a==c and b==d), or (a==d and b==c) - that is, one domino can be rotated to be equal to another domino.</em>


Return the number of pairs (i, j) for which 0 <= i < j < dominoes.length, and dominoes[i] is equivalent to dominoes[j].	

Approach:

sort the dominions, and using hashmap to get the same pairs.

```cpp
   int numEquivDominoPairs(vector<vector<int>>& dominoes) {
        int ans=0;
        unordered_map<string,int> mp;
        for(auto t: dominoes){
            if(t[0]>t[1]) swap(t[0],t[1]);
            string s=to_string(t[0])+","+to_string(t[1]);
            ans+=mp[s];
            mp[s]++;
        }
        return ans;
    }
```

### 1129. Shortest Path with Alternating Colors
<em>Consider a directed graph, with nodes labelled 0, 1, ..., n-1.  In this graph, each edge is either red or blue, and there could be self-edges or parallel edges.

Each [i, j] in red_edges denotes a red directed edge from node i to node j.  Similarly, each [i, j] in blue_edges denotes a blue directed edge from node i to node j.

Return an array answer of length n, where each answer[X] is the length of the shortest path from node 0 to node X such that the edge colors alternate along the path (or -1 if such a path doesn't exist).
</em>

Approach: bfs, record i,j,k as the status. (i start node, j dest node, k color)
```cpp
    vector<int> shortestAlternatingPaths(int n, vector<vector<int>>& red_edges, vector<vector<int>>& blue_edges) {
        //bfs with two options: starting with blue, starting with red.
        vector<int> ans(n,-1);
        ans[0]=0;
        //build graph i,j,k k is the color 0, 1
        vector<vector<vector<int>>> adj(n,vector<vector<int>>(2)); //[nx2xm]
        for(auto t: red_edges) adj[t[0]][0].push_back(t[1]); //red
        for(auto t: blue_edges) adj[t[0]][1].push_back(t[1]); //blue
        bool v[n][n][2];
        memset(v,0,n*n*2*sizeof(bool));
        queue<vector<int>> q;
        for(auto t: adj[0]){
            for(int r: adj[0][0]) {q.push({0,r,0});v[0][r][0]=1;}
            for(int b: adj[0][1]) {q.push({0,b,1});v[0][b][1]=1;}
        }
        int step=1;
        while(q.size()){
            int sz=q.size();
            while(sz--){
                auto t=q.front();
                q.pop();
                int i=t[0],j=t[1],k=t[2];
                if(ans[j]<0) ans[j]=step;
                for(auto e: adj[j][k^1]){
                    if(v[j][e][k^1]==0){
                        v[j][e][k^1]=1;
                        q.push({j,e,k^1});
                    }
                }
            }
            step++;
        }
        return ans;
    }
```	
- note: if you are using bool v[n][n][2], we have to initialize it, otherwise it is random.
	

### 1130. Minimum Cost Tree From Leaf Values (****)
<em>Problem:

Given an array arr of positive integers, consider all binary trees such that:

Each node has either 0 or 2 children;
The values of arr correspond to the values of each leaf in an in-order traversal of the tree.  (Recall that a node is a leaf if and only if it has 0 children.)
The value of each non-leaf node is equal to the product of the largest leaf value in its left and right subtree respectively.
Among all possible binary trees considered, return the smallest possible sum of the values of each non-leaf node.  It is guaranteed this sum fits into a 32-bit integer.
</em>

Approach:

- This is a very good question. but Be careful what the problem asks. You cannot change the order of the  array. It is already arranged as is for the tree leaf nodes.
- a number can only combine its nieghboring, cannot cross!
- sorted order will be simple.
Once we are clear about that, we can use greedy approach.
every leaf shall be combined with its left or right. For each element it shall combine to the next larger number to get rid of itself.
So the problem is equivalent to find next larger integer in its left or right.
And this can be solved using stack.
for example [7,12,8,10]:
add 7 to stack
12>7: we pop 7 and get 7*12 (now stack is empty so to make min work, we can add a INT_MAX in the stack.
add 12 to stack
add 8 to stack
10>8 and we get 8*10

```cpp
    int mctFromLeafValues(vector<int>& arr) {
		stack<int> st; //monotonic stack decreasing order, so we know its left next larger.
		st.push(INT_MAX);
		int ans=0;
		for(int t: arr){
			while(t>=st.top()){
				int mid=st.top();
				st.pop();
				ans+=mid*min(st.top(),t);
			}
			st.push(t);
		}
		//now stack is in decreasing order.
		while(st.size()>1){
			int t=st.top();
			st.pop();
			ans+=t*st.top();
		}
		return ans;
	}
```
- add int-max to keep the decreasing order and do not handle empty case
- while loop to keep poping those larger elements in the stack.

### 1131. Maximum of Absolute Value Expression (****)
<em>Problem:

Given two arrays of integers with equal lengths, return the maximum value of:

|arr1[i] - arr1[j]| + |arr2[i] - arr2[j]| + |i - j|

where the maximum is taken over all 0 <= i, j < arr1.length.
</em>

Approach:

- we need remove the abs operator.
- if the abs is removed, this would be equiv to:
A[i]-A[j]+B[i]-B[j]+i-j. ie (A[i]+B[i]+i)-(A[j]+B[j]+j) when A[i]>=A[j] && B[i]>=B[j] && i>=j
A[j]-A[i]+B[i]-B[j]+i-j. ie (B[i]-A[i]+i)-(B[j]-A[j]+j) when A[i]<=A[j] && B[i]>=B[j] && i>=j
A[i]-A[j]+B[j]-B[i]+i-j. ie (A[i]-B[i]+i)-(A[j]-B[j]+j) when A[i]>=A[j] && B[i]<=B[j] && i>=j
A[j]-A[i]+B[j]-B[i]+i-j. ie (B[i]-A[i]+i)-(B[j]-A[j]+j) when A[i]<=A[j] && B[i]<=B[j] && i>=j
A[i]-A[j]+B[i]-B[j]+j-i. ie (A[i]+B[i]-i)-(A[j]+B[j]-j) when A[i]>=A[j] && B[i]>=B[j] && i<=j
A[j]-A[i]+B[i]-B[j]+j-i. ie (B[i]-A[i]-i)-(B[j]-A[j]-j) when A[i]<=A[j] && B[i]>=B[j] && i<=j
A[i]-A[j]+B[j]-B[i]+j-i. ie (A[i]-B[i]-i)-(A[j]-B[j]-j) when A[i]>=A[j] && B[i]<=B[j] && i<=j
A[j]-A[i]+B[j]-B[i]+j-i. ie (B[i]-A[i]-i)-(B[j]-A[j]-j) when A[i]<=A[j] && B[i]<=B[j] && i<=j

this is to find 4 types of max-min in the array.
code is simple but math requires some intuition.
```cpp
    int maxAbsValExpr(vector<int>& arr1, vector<int>& arr2) {
        int n = arr1.size();
        vector<int> sum1(n),diff1(n),sum2(n),diff2(n);
        int max1=INT_MIN,max2=INT_MIN,max3=INT_MIN,max4=INT_MIN;
        int min1=INT_MAX,min2=INT_MAX,min3=INT_MAX,min4=INT_MAX;
        for(int i=0;i<n;i++) {
            sum1[i]=arr1[i]+arr2[i]+i;
            diff1[i]=arr1[i]-arr2[i]+i;
            sum2[i]=arr1[i]+arr2[i]-i;
            diff2[i]=arr1[i]-arr2[i]-i;
            max1=max(max1,sum1[i]);min1=min(min1,sum1[i]);
            max2=max(max2,diff1[i]);min2=min(min2,diff1[i]);
            max3=max(max3,sum2[i]);min3=min(min3,sum2[i]);
            max4=max(max4,diff2[i]);min4=min(min4,diff2[i]);
        }
        return max({max1-min1,max2-min2,max3-min3,max4-min4});
    }
```	

## contest 145
### 1122. Relative Sort Array (*)
simple using hashmap

### 1123. Lowest common ancestor of deepest leaves (***)
O(N^2) is simple using depth

O(N) approach finding depth and lca at the same time
```cpp
    int h=0;
    TreeNode* lca=0;
    TreeNode* lcaDeepestLeaves(TreeNode* root) {
        depth(root,0);
        return lca;
    }
    int depth(TreeNode* root,int d){
        h=max(h,d);
        if(!root) return d;
        int left=depth(root->left,d+1);
        int right=depth(root->right,d+1);
        if(left==h && right==h)
            lca=root;
        return max(left,right);
    }
```
### 1124. longest well-performing interval (***)
idea: >8 +1 and <=8 -1.
if sum>0 from 0 to i is a valid interval
else find the valid

```cpp
    int longestWPI(vector<int>& hours) {
        //we can conver to 1 0, and we are searching for longest window with more 1s than 0s.
        //then we can use prefix sum
        int n=hours.size();
        int ans=0;
        map<int,int> mp; //sum vs index
        int sum=0;
        for(int i=0;i<n;i++){
            sum+=hours[i]>8?1:-1;
            if(sum>0)
                ans=max(ans,i+1); //from 0 to i
            else { //find the one less
                auto it=mp.lower_bound(sum);
                auto it1=mp.begin();
                while(it1!=it) ans=max(ans,i-it1->second),it1++;
            }
            
            if(!mp.count(sum)){ //we have the sum seen
                mp[sum]=i;
            }
         }
        return ans;
    }
```
note: map/set has lower_bound member function

### 1125. Smallest Sufficient Team (****)
idea: 
- convert skills into bit set
- convert people to integer
- then use backtracking or dp to find the answer.

backtracking:
```cpp
    vector<int> smallestSufficientTeam(vector<string>& req_skills, vector<vector<string>>& people) {
        unordered_map<string,int> skills;
        int n=req_skills.size();
        int np=people.size();
        //using bits
        int target=0;
        for(int i=0;i<n;i++) {
            skills[req_skills[i]]=1<<i;
            target+=1<<i;
        }
        //convert the people skill into integer with bits.
        //to reduce same skill or a subset skill people
        //optimization: we need try person with large set first.
        vector<int> person(np); 
        for(int i=0;i<np;i++){
            int t=0;
            for(auto s: people[i]) 
                if(skills.count(s)) t+=skills[s];
            bool include=1;
            for(int j=i-1;j>=0;j--){
                if(person[j]==0) continue;
                if(t==person[j] || (t<person[j] && (t|person[j])==person[j])){
                    include=0;
                    break;
                }
                //if((t|person[j])==t) person[j]=0;
            }
            if(include) {
                person[i]=t;
                //seen.insert(t);
            }
        }
        //copy(person.begin(),person.end(),ostream_iterator<int>(cout," "));cout<<endl;
        vector<int> ans;
        dfs(person,0,0,{},ans,target);
        return ans;
    }
    void dfs(vector<int>& person,int start,int interm,vector<int> cand,vector<int>& ans,int target){
        //cout<<start<<" "<<interm<<" [";
        //copy(cand.begin(),cand.end(),ostream_iterator<int>(cout," "));cout<<"]"<<endl;

        if(interm==target){
            if(ans.size()==0) ans=cand;
            else if(cand.size()<ans.size()) ans=cand;
            return;
        }
        if(start>=person.size() || (ans.size() && cand.size()>=ans.size())) return;
        for(int i=start;i<person.size();i++){
            if(person[i]==0 || (interm|person[i])==interm) continue;
            cand.push_back(i);
            dfs(person,i+1,interm|person[i],cand,ans,target);
            cand.pop_back();
        }
    }
```

dp approach:
dp[1<<n,m+1]: note it needs the people combination instead min number of people.
```cpp
    vector<int> smallestSufficientTeam(vector<string>& req_skills, vector<vector<string>>& people) {
		int n = req_skills.size();
        int m=people.size();
		vector<vector<int>> dp(1<<n);  // using unordered_map, we improve on time
        unordered_map<string,int> skill_map;
        for(int i=0;i< req_skills.size();i++)
            skill_map[req_skills[i]]=i;
        
        for(int i=0;i<people.size();i++)
        {
            int curr_skill = 0;
            for(int j=0;j<people[i].size();j++)
                curr_skill |= 1<<skill_map[people[i][j]];
            
            for(int j=0;j<(1<<n);j++)
            {
                int k = j | curr_skill;
                if(dp[k].size()==0 || dp[k].size()>1+dp[j].size()) 
                    //check if add this person can relax
                {
                    dp[k]=dp[j]; //using its combination
                    dp[k].push_back(i);//add current person
                }       
            }
        }
        return dp[(1<<n) -1];        
    }
```
		
