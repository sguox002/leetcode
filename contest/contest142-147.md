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
this is essentially bellman-ford algorithm.
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
                int k = (dp[j].empty()?0:j) | skills[i]; //this is critical to make k correct.
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
		
## biweek 4
### 1118. Number of Days in a Month (**)
simply check if it is a lunar year

a lunar year: divisible by 4, and if divisible by 100 and 400

### 1119. Remove Vowels from a String (*)
simple

### 1120. Maximum Average Subtree (***)
idea: the average of a subtree requires the sum and num of nodes. traversal and get the two, calculate the average and max average on the fly.
```cpp
    double maximumAverageSubtree(TreeNode* root) {
        double ans=0;
        int num=0;
        dfs(root,num,ans);
        return ans;
    }
    pair<int,int> dfs(TreeNode* root,int num,double& ans){
        if(!root) return {0,0};
        auto left=dfs(root->left,num,ans);
        auto right=dfs(root->right,num,ans);
        num=1+left.second+right.second;
        int sum=(left.first+right.first+root->val);
        double leftavg=left.first*1.0/left.second;
        double rightavg=right.first*1.0/right.second;
        double curr=sum*1.0/num;
        ans=max({ans,leftavg,rightavg,curr});
        return {sum,num};
    }
```	

### 1121. Divide Array Into Increasing Sequences (***)
<em>Problem: 

Given a non-decreasing array of positive integers nums and an integer K, find out if this array can be divided into one or more disjoint increasing subsequences of length at least K.
</em>
Idea:

hashmap to get the histogram. The most frequent one determines at least how many groups we will have.
- maxfreq*K<n no solution
- maxfreq=1 only one
- greedy: we need use the max freq and reduce by 1, stop when all same maxfreq is reduced and cnt>=k.
- using a pq to reduce the maxfreq by 1 and then put back
```cpp
    struct comp{
        bool operator()(pair<int,int> a,pair<int,int> b){
            return a.second<b.second || (a.second==b.second && a.first<b.first);
        }
    };
    bool canDivideIntoSubsequences(vector<int>& nums, int K) {
        int n=nums.size();
        map<int,int> mp;
        int maxdup=0;
        for(int i: nums) {mp[i]++;maxdup=max(maxdup,mp[i]);}
        if(maxdup*K>n) return 0;
        if(maxdup==1) return 1;
        //we need to divide the array into at least maxdup parts
        //we first need to take one from the most frequent one until the top
        priority_queue<pair<int,int>,vector<pair<int,int>>,comp> pq(mp.begin(),mp.end());
        while(pq.size()){
            auto p=pq.top();
            int maxfreq=p.second;
            int cnt=0;
            vector<pair<int,int>> vp;
            while(pq.size() && (cnt<K || pq.top().second==maxfreq)){ //same freq shall be used all
                if(pq.top().second>1)
                    vp.push_back({pq.top().first,pq.top().second-1});
                //cout<<pq.top().first<<" ";
                cnt++;
                pq.pop();
            }
            if(cnt<K) return 0;
            for(auto t: vp) pq.push(t);
        }
        return 1;
    }
```	

## contest 144
### 1108. Defanging an IP Address (*)
simple

### 1109. Corporate Flight Bookings  (***)
<em>Problem:

There are n flights, and they are labeled from 1 to n.

We have a list of flight bookings.  The i-th booking bookings[i] = [i, j, k] means that we booked k seats from flights labeled i to j inclusive.

Return an array answer of length n, representing the number of seats booked on each flight in order of their label.

 

Example 1:

Input: bookings = [[1,2,10],[2,3,20],[2,5,25]], n = 5
Output: [10,55,45,25,25]
</em>

The description is very unclear. Actually ii,j,k means from flight i to j, there are k seats booking. (i,j) is a flight range not an edge defining a flight).
so actually it is defining a list of intervals and get each number.
typical interval problem

```cpp
    vector<int> corpFlightBookings(vector<vector<int>>& bookings, int n) {
        vector<int> ans(n);
        map<int,int> book;
        for(auto t: bookings){
            book[t[0]]+=t[2]; //add
            book[t[1]+1]-=t[2]; //remove
        }
        int prefix=0;
        for(int i=0;i<n;i++){
            if(book.count(i+1)) {
                prefix+=book[i+1];
            }
            ans[i]=prefix;
        }
        return ans;
    }
```	

### 1110. Delete Nodes And Return Forest (***)
atually post-order is better.

```cpp
    vector<TreeNode*> delNodes(TreeNode* root, vector<int>& to_delete) {
        vector<TreeNode*> ans;
        unordered_set<int> del(to_delete.begin(),to_delete.end());
        if(delNodes(root,del,ans)) ans.push_back(root);
        return ans;
    }
    TreeNode* delNodes(TreeNode* root,unordered_set<int>& del,vector<TreeNode*>& ans){
        if(!root) return 0;
        if(del.empty()) return root;
        //preorder
        if(del.count(root->val)){
            del.erase(root->val);//root is deleted
            TreeNode* left=delNodes(root->left,del,ans);
            TreeNode* right=delNodes(root->right,del,ans);
            if(left) ans.push_back(left);
            if(right) ans.push_back(right);
            return 0;
        }

        //its left and right will be modified.
        root->left=delNodes(root->left,del,ans);
        root->right=delNodes(root->right,del,ans);
        return root;
    }
```
### 1111. Maximum Nesting Depth of Two Valid Parentheses Strings	(***)
<em>
A string is a valid parentheses string (denoted VPS) if and only if it consists of "(" and ")" characters only, and:

It is the empty string, or
It can be written as AB (A concatenated with B), where A and B are VPS's, or
It can be written as (A), where A is a VPS.
We can similarly define the nesting depth depth(S) of any VPS S as follows:

depth("") = 0
depth(A + B) = max(depth(A), depth(B)), where A and B are VPS's
depth("(" + A + ")") = 1 + depth(A), where A is a VPS.
For example,  "", "()()", and "()(()())" are VPS's (with nesting depths 0, 1, and 2), and ")(" and "(()" are not VPS's.

 

Given a VPS seq, split it into two disjoint subsequences A and B, such that A and B are VPS's (and A.length + B.length = seq.length).

Now choose any such A and B such that max(depth(A), depth(B)) is the minimum possible value.

Return an answer array (of length seq.length) that encodes such a choice of A and B:  answer[i] = 0 if seq[i] is part of A, else answer[i] = 1.  Note that even though multiple answers may exist, you may return any of them.

 

Example 1:

Input: seq = "(()())"
Output: [0,1,1,1,1,0]
Example 2:

Input: seq = "()(())()"
Output: [0,0,0,1,1,0,1,1]
</em>


Idea: 
if the total depth is n, then break into A and B with n/2 and n-n/2, the difference will be minimized.
so we shall choose depth alternatively.
for parenthesis, we can use ++-- or stack.
- calculate the depth and assign even depth to 0 and odd depth to 1.
example:
((()))
123321
010010
```cpp
    vector<int> maxDepthAfterSplit(string seq) {
        vector<int> ans(seq.size()); //all assigned as 0
		int sum=0;
		for(int i=0;i<seq.size();i++){
			if(seq[i]=='(') {
				sum++;
				if(sum%2) ans[i]=1;
			}
			else{
				if(sum%2) ans[i]=1;
				sum--;
			}
		}
		return ans;
    }
```	

## contest 143
### 1103. Distribute Candies to People (***)
<em>Problem:

We distribute some number of candies, to a row of n = num_people people in the following way:

We then give 1 candy to the first person, 2 candies to the second person, and so on until we give n candies to the last person.

Then, we go back to the start of the row, giving n + 1 candies to the first person, n + 2 candies to the second person, and so on until we give 2 * n candies to the last person.

This process repeats (with us giving one more candy each time, and moving to the start of the row after we reach the end) until we run out of candies.  The last person will receive all of our remaining candies (not necessarily one more than the previous gift).

Return an array (of length num_people and sum candies) that represents the final distribution of candies.
</em>

Idea:
If we want to derive math equations for this, it would be pretty tricky. but if we treat it as a 2d matrix with roation, it is much simpler.

```cpp
    vector<int> distributeCandies(int candies, int n) {
		vector<int> ans(n);
		int cnt=1;
		int i=0;
		while(candies){
			int m=min(candies,cnt);
			ans[i%n]+=m;
			candies-=m;
		}
		return ans;
	}
```

### 1104. Path in zigzag labelled binary tree (***)
<em>In an infinite binary tree where every node has two children, the nodes are labelled in row order.

In the odd numbered rows (ie., the first, third, fifth,...), the labelling is left to right, while in the even numbered rows (second, fourth, sixth,...), the labelling is right to left.

Given the label of a node in this tree, return the labels in the path from the root of the tree to the node with that label
</em>

Analysis:
starting from layer 1.
it is a full tree.
given a node ->get the depth-> get the original id->id/2
id->depth: the left is 1,2,4,8
```cpp
    vector<int> pathInZigZagTree(int label) {
		vector<int> ans;
		int d=log2(label)+1;
		while(d){
			ans.push_back(label);
			int id=label2id(label,d);
			label=id2label(id/2,--d);
		}
		reverse(ans.begin(),ans.end());
		return ans;
    }
	int label2id(int l, int d){
		if(d%2) return l;
		return pow(2,d-1)+(pow(2,d)-1-l);
	}
	int id2label(int id,int d){
		if(d%2) return id;
		return pow(2,d)-1-(id-pow(2,d-1));
	}
```	
### 1105. Filling bookcase shelves (****)
<em>We have a sequence of books: the i-th book has thickness books[i][0] and height books[i][1].

We want to place these books in order onto bookcase shelves that have total width shelf_width.

We choose some of the books to place on this shelf (such that the sum of their thickness is <= shelf_width), then build another level of shelf of the bookcase so that the total height of the bookcase has increased by the maximum height of the books we just put down.  We repeat this process until there are no more books to place.

Note again that at each step of the above process, the order of the books we place is the same order as the given sequence of books.  For example, if we have an ordered list of 5 books, we might place the first and second book onto the first shelf, the third book on the second shelf, and the fourth and fifth book on the last shelf.

Return the minimum possible height that the total bookshelf can be after placing shelves in this manner.
</em>

Approach:
- shelf is bound by width. and each book has w x h
- book sequence is not to be changed.
- dp: a book can be placed after previous book if width is allowed. or it can start a new layer.
- subproblem shall be min height for n books. dp[i]
-  recurrence: if a new layer: dp[i]=dp[i-1]+h[i]. if append to previous: it can attach to i-1, i-2...j 
```cpp
    int minHeightShelves(vector<vector<int>>& books, int shelf_width) {
		int n=books.size();
		vector<int> dp(n+1, INT_MAX); //min height for i books.
		dp[0]=0; //no books
		for(int i=0;i<n;i++){
			dp[i+1]=dp[i]+books[i][1];
			int width=0,maxh=0;
			for(int j=i;j>=0;j--){
				if(width+books[j][0]<=shelf_width){
					maxh=max(maxh,books[j][1]);
					width+=books[j][0];
					dp[i+1]=min(dp[i+1],dp[j]+maxh);
				}
				else break; //this is essential otherwise it will not stop and gives wrong results.
			}
		}
		return dp[n];
    }
```	


### 1106. Parsing a boolean expression (*****)
<em>
Return the result of evaluating a given boolean expression, represented as a string.

An expression can either be:

"t", evaluating to True;
"f", evaluating to False;
"!(expr)", evaluating to the logical NOT of the inner expression expr;
"&(expr1,expr2,...)", evaluating to the logical AND of 2 or more inner expressions expr1, expr2, ...;
"|(expr1,expr2,...)", evaluating to the logical OR of 2 or more inner expressions expr1, expr2, ...
</em>
Intuition:

- recursive stack for syntax analysis.
- &: when any is false, return false, all true return true.
- |: any is true return true, all 0 returns 0
- !: toggle the bool value.
- polish style

need to skip.
```cpp
bool parseBoolExpr(string e) {
  if (e.size() == 1) return e == "t" ? true : false;
  if (e[0] == '!') return !parseBoolExpr(e.substr(2, e.size() - 3));
  bool isAnd = e[0] == '&' ? true : false, res = isAnd;
  for (auto i = 2, j = 2, cnt = 0; res == isAnd && i < e.size(); ++i) {
    if (e[i] == '(') ++cnt;
    if (e[i] == ')') --cnt;      
    if (i == e.size() - 1 || (e[i] == ',' && cnt == 0)) {
      if (isAnd) res &= parseBoolExpr(e.substr(j, i - j));
      else res |= parseBoolExpr(e.substr(j, i - j));
      j = i + 1;
    }
  }
  return res;
}
```	
the coding is pretty tricky.

## biweek 3
### 1099. Two sum less than K (***)
<em>Given an array A of integers and integer K, return the maximum S such that there exists i < j with A[i] + A[j] = S and S < K. If no i, j exist satisfying this equation, return -1.</em>
idea:

using hashmap with binary search
```cpp
    int twoSumLessThanK(vector<int>& A, int K) {
        set<int> ms;
        
        int ans=INT_MIN;
        for(int i: A){
            auto it=ms.lower_bound(K-i);
            if(it!=ms.begin()) ans=max(ans,i+*(--it));
            ms.insert(i);
        }
        return ans==INT_MIN?-1:ans;
    }
```	
### 1100. Find k-length substrings with no repeated characters
Given a string S, return the number of substrings of length K with no repeated characters.
idea:

sliding window with hashmap
```cpp
    int numKLenSubstrNoRepeats(string S, int K) {
        //sliding window with hashmap
        unordered_map<char,int> mp;
        int ans=0;
        for(int i=0;i<S.size();i++){
            if(i<K) mp[S[i]]++;
            else{
                mp[S[i]]++;
                mp[S[i-K]]--;
                if(mp[S[i-K]]==0) mp.erase(S[i-K]);
           }
            if(mp.size()==K) ans++;
        }
        return ans;
    }
```


### 1101. The earliest moment when everyone become friends (**)
simple: union-find and get the time when the number of union is 1.

### 1102. Path with max min value.
<em>Given a matrix of integers A with R rows and C columns, find the maximum score of a path starting at [0,0] and ending at [R-1,C-1].

The score of a path is the minimum value in that path.  For example, the value of the path 8 →  4 →  5 →  9 is 4.

A path moves some number of times from one visited cell to any neighbouring unvisited cell in one of the 4 cardinal directions (north, east, west, south).</em>

Idea:
convert to binary search problem. set all <mid value to be non-passable and find the value (last connection).
using bfs to check if connected.

```cpp
    int maximumMinimumPath(vector<vector<int>>& A) {
        int mn=INT_MAX,mx=INT_MIN;
        for(auto t: A){
            for(int i: t) mn=min(i,mn),mx=max(mx,i);
        }
        //binary search to see if it connects
        int l=mn,r=mx*2;
        while(l+1<r){
            int mid=l+(r-l)/2;
            if(connected(A,mid)) l=mid;
            else r=mid;
            //cout<<l<<" "<<r<<endl;
        }
        return l;
    }
    bool connected(vector<vector<int>>& A,int v){
        int m=A.size(),n=A[0].size();
        if(v>min(A[0][0],A[m-1][n-1])) return 0;
        queue<int> q;
        vector<vector<bool>> visited(m,vector<bool>(n));
        q.push(0);visited[0][0]=1;
        int dir[][2]={{-1,0},{1,0},{0,-1},{0,1}};
        while(q.size()){
            int sz=q.size();
            while(sz--){
                int t=q.front();
                q.pop();
                int x=t/n,y=t%n;
                if(x==m-1 && y==n-1) return 1;
                for(int i=0;i<4;i++){
                    int x0=x+dir[i][0],y0=y+dir[i][1];
                    if(x0<0||y0<0||x0>=m||y0>=n||visited[x0][y0]||A[x0][y0]<v) continue;
                    visited[x0][y0]=1;
                    q.push(x0*n+y0);
                }
            }
        }
        return 0;
    }
```	


## contest 142
1093. Statistics from a Large Sample
<em>We sampled integers between 0 and 255, and stored the results in an array count:  count[k] is the number of integers we sampled equal to k.

Return the minimum, maximum, mean, median, and mode of the sample respectively, as an array of floating point numbers.  The mode is guaranteed to be unique.</em>

min,max,mean
median: odd/even
mode: the most frequent element

1094. car pooling
often seen add/remove action, interval problem
attention: for the same location, we need first unload passenger and then load

```cpp
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        map<int,int> mp;
        for(auto t: trips){
            mp[t[1]]+=t[0];
            mp[t[2]]-=t[0];
        }
        //check prefix sum to see if over capacity
        //print(mp);
        int prefix=0;
        for(auto t: mp){
            prefix+=t.second;
            if(prefix>capacity) return 0;
        }
        return 1;
    }
```	
1095. find in mountain array
two binary search problem.
we can also find the peak first and then do 2 binary search.

```cpp
    int findInMountainArray(int target, MountainArray &mountainArr) {
        //binary search in the peak
        int left=find1side(target,mountainArr,1);
        if(left==-1) return find1side(target,mountainArr,0);
        return left;
    }
    int find1side(int target,MountainArray& mt,int side){
        int l=0,r=mt.length()-1;
        int n=mt.length();
        while(l<r)
        {
            int mid=l+(r-l)/2;
            int m=mt.get(mid);
            if(side){ //search left
                if((mid<n-1 && m>mt.get(mid+1))||(m==n-1 && m<mt.get(mid-1))) r=mid; //mid is on right side
                else {
                    if(m==target) return mid;
                    if(m>target) r=mid-1;
                    else l=mid+1;
                    //cout<<m<<" "<<l<<" "<<r<<endl;
                }
            }
            else //right side
            {
                if((mid<n-1 && m<mt.get(mid+1))||(mid==n-1 && m>mt.get(mid-1))) l=mid+1; //it is on the left side
                else{
                    if(m==target) return mid;
                    if(m>target) l=mid+1;
                    else r=mid;
                    //cout<<m<<" "<<l<<" "<<r<<endl;
                }
            }
        }
        return mt.get(l)==target?l:-1;
    }
```	
1096. Brace expansion II.
again, recursive stack

```cpp
    vector<string> braceExpansionII(string expression){
        unordered_set<string> res=parser(expression);
        vector<string> ans(res.begin(),res.end());
        sort(ans.begin(),ans.end());
        return ans;
    }
	unordered_set<string> parser(string expression)
	{
		string s=expression;
		stack<unordered_set<string>> st;
		char presign=',';
        //cout<<s<<": ";
		for(int i=0;i<s.length();i++)
		{
			char c=s[i];
			if(c=='{')
			{
				int j=i,p=1;
				while(s[j]!='}' || p!=0 ) //find the paired }
				{
					j++;
					if(s[j]=='{') p++;
					if(s[j]=='}') p--;
				}
				unordered_set<string> vs=parser(s.substr(i+1,j-i-1));
				if(presign=='*')
				{
					auto t=st.top();
					st.pop();
					st.push(merge(t,vs));
				}
				else st.push(vs);
				i=j;
				presign='*';
			}
			else if(isalpha(c))
			{
				unordered_set<string> vs;
				string t;
                while(isalpha(s[i])) t+=s[i++];
                i--;
                vs.insert(t);//not a single letter, it allows string
                
				if(presign=='*')
				{
					unordered_set<string> t=st.top();
					st.pop();
					st.push(merge(t,vs));
				}
                else st.push(vs);
				presign='*';
			}
			if(c==',' || i==s.length()-1)
			{
				presign=',';
			}
		}
        
		unordered_set<string> ans;
		while(st.size())
		{
            auto vt=st.top();
			for(string t: vt)
			{
				ans.insert(t);
			}
			st.pop();
		}
        //copy(ans.begin(),ans.end(),ostream_iterator<string>(cout," "));cout<<endl;
		//sort(ans.begin(),ans.end());
		return ans;
	}
	
	unordered_set<string> merge(unordered_set<string>& l1,unordered_set<string>& l2)
	{
		unordered_set<string> ans;
		for(string s: l1)
			for(string t: l2)
				ans.insert(s+t);
		return ans;
	}
```	

