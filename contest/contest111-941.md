## contest 111
941. Valid Mountain Array (array, *)
942. Delete columns to make sorted (2d array, *)
943. DI string match (greedy, **)
944. Find the shortest superstring (dp, bitset, travelling businessman shortest distance)

941. Valid Mountain Array
```cpp
    bool validMountainArray(vector<int>& A) {
        if(A.empty()) return 0;
        int i=0,n=A.size(),j=n-1;
        while(i+1<n && A[i+1]>A[i]) i++;
        while(j && A[j-1]>A[j]) j--;
        return i<n-1 && j>0 && i==j;
    }
```
944. Delete Columns to Make Sorted
```cpp
    int minDeletionSize(vector<string>& A) {
        int ans=0;
        if(A.empty()) return 0;
        for(int j=0;j<A[0].size();j++){
            char c=A[0][j];
            for(int i=1;i<A.size();i++){
                if(A[i][j]<c) {ans++;break;}
                c=A[i][j];
            }
        }
        return ans;
    }
```
942. DI String Match
greedy: from two end to meet.
```cpp
    vector<int> diStringMatch(string S) {
        //greedy: I from smallest, D from largest
        int n=S.size();
        int i=0,j=n;
        vector<int> ans;
        for(char c: S){
            if(c=='I') ans.push_back(i++);
            else ans.push_back(j--);
        }
        ans.push_back(i);
        return ans;
    }
```
	
943. Find the Shortest Superstring.md

### Problem Summary
Given an array A of strings, find any smallest string that contains each string in A as a substring.

We may assume that no string in A is substring of another string in A.

 
Example 1:

Input: ["alex","loves","leetcode"]
Output: "alexlovesleetcode"
Explanation: All permutations of "alex","loves","leetcode" would also be accepted.
Example 2:

Input: ["catg","ctaagt","gcta","ttca","atgcatc"]
Output: "gctaagttcatgcatc"
 

Note:

1 <= A.length <= 12
1 <= A[i].length <= 20

### Analysis
- equivalent: think it as a graph, max the total sum of overlap, visiting each node once.
- traveling salesman problem

By calculating the total appending length from si to sj, the problem is the shortest path visiting each node once problem (graph[i,j])
By calculating the overlap length of si and sj, the problem is the max path (with weight) visiting each node once problem

steps:
1. form the graph, the edge i,j represents the number of char to add.
(define this is critical: A->B->C len(A)+len(B)-len(AB)+len(C)-len(BC), len(AB) is the A's suffix vs A.prefix common length. we define len(B)-len(AB) the edge weight, the number of chars to add. 
from A to destination we will get len(A)+sum(edge)).
2. using the bitset and check if we can relax using all other nodes
3. keep the shortest path
4. dp[i,j]: the status i and ending node j.
5. relax: dp[i,j]=min(dp[i,j],dp[i-(1<<j),k]+dist(k,j)). 
6. record the path and do backtrace.

```cpp
    string shortestSuperstring(vector<string>& A) {
        int n=A.size(); //number of nodes
        vector<vector<int>> graph(n,vector<int>(n));
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++)
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
        //dp[i,j]: min distance ending with node j. 
        for(int i=1;i<m;i++) //0x0001 to 0xffff
        {
            for(int j=0;j<n;j++)
            {
                if(i & (1<<j)) //if node j is visited
                {
                    int prev=i-(1<<j); //previous status
                    if(prev==0) dp[i][j]=A[j].length(); //in case j is the starting node.
                    else //try to use other edge to relax  
                    {
                        for(int k=0;k<n;k++)
                        {
                            if(dp[prev][k]<INT_MAX && dp[i][j]>dp[prev][k]+graph[k][j])
                            {
                                dp[i][j]=dp[prev][k]+graph[k][j];
                                path[i][j]=k; //save the node
                            }
                        }
                    }
                }
                if(i==m-1 && gmin>dp[i][j]) {gmin=dp[i][j];last=j;} //all visited status==m-1, ending with different nodes.
            }
        }

        //backtrace to get the results, path stored in path[i][j]
        int curr=m-1; //0xffff
        vector<int> seq;
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
            int num_app=graph[seq[i+1]][seq[i]];
            int len=A[seq[i]].length();
            ans+=A[seq[i]].substr(len-num_app);
        }
        return ans;
    }
    
    int calc(string& a,string& b) //a -> b the adding length.
    {
        int m=a.size(),n=b.size();
        for(int i=1;i<a.size();i++) //no duplicates
        {
            if(b.substr(0,m-i)==a.substr(i)) return n-(m-i);
        }
        return n;
    }
```
