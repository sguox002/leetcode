## contest 103

Smallest Range I3
Snakes and Ladders6
Smallest Range II6
Online Election

### 908. Smallest Range I
<em>
Given an array A of integers, for each integer A[i] we may choose any x with -K <= x <= K, and add x to A[i].

After this process, we have some array B.

Return the smallest possible difference between the maximum value of B and the minimum value of B.
</em>

given two number a<b:
if b-a>2k, the diff is b-a-2k.
if b-a<2k, the diff will be 0
```cpp
    int smallestRangeI(vector<int>& A, int K) {
        int min0=*min_element(A.begin(),A.end());
        int max0=*max_element(A.begin(),A.end());
        int d=max0-min0-2*K;
        return d>0?d:0;
    }
```

### 909. Snakes and Ladders
<em>
On an N x N board, the numbers from 1 to N*N are written boustrophedonically starting from the bottom left of the board, and alternating direction each row.  For example, for a 6 x 6 board, the numbers are written as follows:


You start on square 1 of the board (which is always in the last row and first column).  Each move, starting from square x, consists of the following:

You choose a destination square S with number x+1, x+2, x+3, x+4, x+5, or x+6, provided this number is <= N*N.
(This choice simulates the result of a standard 6-sided die roll: ie., there are always at most 6 destinations, regardless of the size of the board.)
If S has a snake or ladder, you move to the destination of that snake or ladder.  Otherwise, you move to S.
A board square on row r and column c has a "snake or ladder" if board[r][c] != -1.  The destination of that snake or ladder is board[r][c].

Note that you only take a snake or ladder at most once per move: if the destination to a snake or ladder is the start of another snake or ladder, you do not continue moving.  (For example, if the board is `[[4,-1],[-1,3]]`, and on the first move your destination square is `2`, then you finish your first move at `3`, because you do not continue moving to `4`.)

Return the least number of moves required to reach square N*N.  If it is not possible, return -1.
</em>

convert to 1d array and uses bfs.

```cpp
    int snakesAndLadders(vector<vector<int>>& board) {
		vector<int> b1d;
		int n=board.size();
		for(int i=n-1;i>=0;i--)
		{
			int trow=n-1-i;
			if(trow%2==0) for(int t: board[i]) b1d.push_back(t);
			else for(int j=n-1;j>=0;j--) b1d.push_back(board[i][j]);
		}
        queue<int> q;
        vector<bool> visited(n*n);
        q.push(0);
        visited[0]=1;
        int step=0;
        while(q.size()){
            int sz=q.size();
            //cout<<endl;
            while(sz--){
                int ind=q.front();
                q.pop();
                //cout<<ind<<" ";
                if(ind==n*n-1) return step;
                for(int i=1;i<=6;i++){
                    int tind=ind+i;
                    if(tind<n*n){
                        if(b1d[tind]<0){
                            if(!visited[tind]) {q.push(tind),visited[tind]=1;}
                        }
                        else {
                            tind=b1d[tind]-1;
                            if(!visited[tind]) q.push(tind),visited[tind]=1;
                        }
                    }
                }
            }
            step++;
        }
        return -1;
    }
```

### 910. Smallest Range II
<em>
Given an array A of integers, for each integer A[i] we need to choose either x = -K or x = K, and add x to A[i] (only once).

After this process, we have some array B.

Return the smallest possible difference between the maximum value of B and the minimum value of B.
</em>

greedy: sort the array, and left +k, right -k, compare the two options

```cpp
    int smallestRangeII(vector<int>& A, int K) {
        sort(A.begin(), A.end());
        int res = A[A.size() - 1] - A[0];
        int left = A[0] + K, right = A[A.size() - 1] - K;
        for (int i = 0; i < A.size() - 1; i++) {
            int maxi = max(A[i] + K, right), mini = min(left, A[i + 1] - K);
            res = min(res, maxi - mini);
        }
        return res;
    }
```

### 911. Online Election

<em>
In an election, the i-th vote was cast for persons[i] at time times[i].

Now, we would like to implement the following query function: TopVotedCandidate.q(int t) will return the number of the person that was leading the election at time t.  

Votes cast at time t will count towards our query.  In the case of a tie, the most recent vote (among tied candidates) wins.
</em>
approach: binary search.
```cpp
    map<int, int> m;
    TopVotedCandidate(vector<int> persons, vector<int> times) {
        int n = persons.size(), lead = -1;
        unordered_map<int, int> count;
        for (int i = 0; i < n; ++i) {
            lead = ++count[persons[i]] >= count[lead] ? persons[i] : lead;
            m[times[i]] = lead;
        }
    }

    int q(int t) {
        return (--m.upper_bound(t))-> second;
    }
```

	
	
