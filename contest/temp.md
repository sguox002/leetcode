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
	
	

