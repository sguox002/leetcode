## contest 109

Number of Recent Calls4
Knight Dialer5
Shortest Bridge6
Stamping The Sequence

933. Number of Recent Calls
<em>
Write a class RecentCounter to count recent requests.

It has only one method: ping(int t), where t represents some time in milliseconds.

Return the number of pings that have been made from 3000 milliseconds ago until now.

Any ping with time in [t - 3000, t] will count, including the current ping.

It is guaranteed that every call to ping uses a strictly larger value of t than before.

 

Example 1:

Input: inputs = ["RecentCounter","ping","ping","ping","ping"], inputs = [[],[1],[100],[3001],[3002]]
Output: [null,1,2,3,3]
 

Note:

Each test case will have at most 10000 calls to ping.
Each test case will call ping with strictly increasing values of t.
Each call to ping will have 1 <= t <= 10^9.
</em>

using deque

```cpp
    deque<int> dq;
    RecentCounter() {
        
    }
    
    int ping(int t) {
        dq.push_back(t);
        while(t-dq.front()>3000) dq.pop_front();
        return dq.size();
    }
```

934. Shortest Bridge
<em>
In a given 2D binary array A, there are two islands.  (An island is a 4-directionally connected group of 1s not connected to any other 1s.)

Now, we may change 0s to 1s so as to connect the two islands together to form 1 island.

Return the smallest number of 0s that must be flipped.  (It is guaranteed that the answer is at least 1.)

 
</em>

equivalent: find the min distance between two islands
- find the two parties
- find the shortest distance

```cpp
    int shortestBridge(vector<vector<int>>& A) {
        //two party problem can use paint
        //or use dfs/bfs to reach the other party
        //we can calculate the min distance between the two parties
        vector<vector<int>> partyA,partyB;
        for(int i=0;i<A.size();i++)
        {
            for(int j=0;j<A[0].size();j++)
            {
                if(A[i][j]==1) 
                {
                    if(partyA.size()==0) dfs(A,i,j,partyA);
                    else dfs(A,i,j,partyB);
                }
            }
        }
        //cout<<"OK";
        int mindist=INT_MAX;
        for(int i=0;i<partyA.size();i++)
        {
            for(int j=0;j<partyB.size();j++)
                mindist=min(mindist,abs(partyA[i][0]-partyB[j][0])+abs(partyA[i][1]-partyB[j][1]));
        }
        return mindist-1;
    }
    void dfs(vector<vector<int>>& A,int i,int j,vector<vector<int>>& pa)
    {
        //4 directions
        A[i][j]=2; 
        int n=A.size(),m=A[0].size();
        pa.push_back({i,j});
        if(i>0 && A[i-1][j]==1) dfs(A,i-1,j,pa);
        if(j>0 && A[i][j-1]==1) dfs(A,i,j-1,pa);
        if(i<n-1 && A[i+1][j]==1) dfs(A,i+1,j,pa);
        if(j<m-1 && A[i][j+1]==1) dfs(A,i,j+1,pa);
    }
```
see similar problem 1568. Minimum Number of Days to Disconnect Island

935. Knight Dialer
<em>
The chess knight has a unique movement, it may move two squares vertically and one square horizontally, or two squares horizontally and one square vertically (with both forming the shape of an L). The possible movements of chess knight are shown in this diagaram:

A chess knight can move as indicated in the chess diagram below:
We have a chess knight and a phone pad as shown below, the knight can only stand on a numeric cell (i.e. blue cell).
Given an integer n, return how many distinct phone numbers of length n we can dial.

You are allowed to place the knight on any numeric cell initially and then you should perform n - 1 jumps to dial a number of length n. All jumps should be valid knight jumps.

As the answer may be very large, return the answer modulo 109 + 7.
</em>

- dp: each step we may have 10 starting positions.
- similar to climb stairs, there are several paths from prev to current positions.

```cpp
    int knightDialer(int N) {
        if(N==1) return 10;
        vector<vector<int>> adj={{4,6},{8,6},{7,9},{4,8},{0,3,9},{},{0,1,7},{2,6},{1,3},{2,4}};

        vector<int> curr(10),prev(10);
        for(int i=0;i<10;i++) prev[i]=1;

        int mod=1e9+7;
        for(int i=1;i<N;i++) //hops
        {
            for(int j=0;j<10;j++) //digits
            {
                curr[j]=0;
                for(int k=0;k<adj[j].size();k++)
                {
					curr[j]+=prev[adj[j][k]];
					curr[j]%=mod;
				}
            }
            prev=curr;
        }
        
        int sum=0;
        for(int i=0;i<10;i++) {sum+=prev[i];sum%=mod;}
        return sum;
        
    }
```	
	
936. Stamping The Sequence

<em>
You want to form a target string of lowercase letters.

At the beginning, your sequence is target.length '?' marks.  You also have a stamp of lowercase letters.

On each turn, you may place the stamp over the sequence, and replace every letter in the sequence with the corresponding letter from the stamp.  You can make up to 10 * target.length turns.

For example, if the initial sequence is "?????", and your stamp is "abc",  then you may make "abc??", "?abc?", "??abc" in the first turn.  (Note that the stamp must be fully contained in the boundaries of the sequence in order to stamp.)

If the sequence is possible to stamp, then return an array of the index of the left-most letter being stamped at each turn.  If the sequence is not possible to stamp, return an empty array.

For example, if the sequence is "ababc", and the stamp is "abc", then we could return the answer [0, 2], corresponding to the moves "?????" -> "abc??" -> "ababc".

Also, if the sequence is possible to stamp, it is guaranteed it is possible to stamp within 10 * target.length moves.  Any answers specifying more than this number of moves will not be accepted.
</em>

reverse operation and matching:

```cpp
    vector<int> movesToStamp(string stamp, string target) {
        //reverse operation: matched then change it to ***
        //until we change the target string into *****
        //note we can only match one end if it is covered
        int n=target.length();
        string final(n,'*');
        vector<int> ans;
        while(target!=final)
        {
            int ind=match_change(target,stamp);
            if(ind==-1) return vector<int>();
            ans.push_back(ind);
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
    int match_change(string& target,string stamp)
    {
        //find the first matching and return
        //at least has one non * char inside, * matches any char
        bool matched=0;
        for(int i=0;i<target.size();i++)
        {
            int cnt_match=0;
            int j=0;
            for(j=0;j<stamp.size();j++)
            {
                if(target[i+j]=='*') continue;
                if(target[i+j]==stamp[j]) cnt_match++;
                else break;
            }
            if(j==stamp.size()&& cnt_match) 
            {
                for(j=0;j<stamp.size();j++) target[i+j]='*';
                return i;
            }
        }
        return -1; //no matching
    }
```

	