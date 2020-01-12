### 1317. Convert Integer to the Sum of Two No-Zero Integers

approach: 
just try from 1 to n and break into i and j and check if they contains 0.
we can use string find to check 0.

### 1318. Minimum Flips to Make a OR b Equal to c
approach: 
convert the 3 integers to bitset and check if the ith bit c[i]!=a[i]||b[i] then:
c[i]==0: ans+=a[i]+b[i]
c[i]==1: ans++ (a[i] and b[i] must be 0)
```cpp
    int minFlips(int a, int b, int c) {
        bitset<32> sa(a),sb(b),sc(c);
        int ans=0;
        for(int i=0;i<32;i++){
            if(sc[i]==(sa[i]||sb[i])) continue;
            if(sc[i]==0) ans+=sa[i]+sb[i]; //0 but there is 1
            else ans++;//1 but we get 0, need one flip
        }
        return ans;
    }
```

### 1319. Number of Operations to Make Network Connected
Approach:
- a connected graph with n nodes needs at least n-1 edges.
- equivalent to find number of disjoint set and connect each set needs only one edge.
```cpp
    vector<int> parent;
    int sz;
    int makeConnected(int n, vector<vector<int>>& connections) {
        //min spanning tree. we need n-1 edges
        if(connections.size()<n-1) return -1;
        parent.resize(n);
        sz=n;
        for(int i=0;i<n;i++) parent[i]=i;
        for(auto c: connections){
            int s=c[0],e=c[1];
            int pi=findp(s),pj=findp(e);
            if(pi!=pj) {
                parent[pi]=pj;
                sz--;
            }
        }
        return sz-1;
    }
    int findp(int i){
        while(i!=parent[i]){
            parent[i]=parent[parent[i]];
            i=parent[i];
        }
        return i;
    }
```
### 1310. Minimum Distance to Type a Word Using Two Fingers
Analysis:
- it is a dp problem. Someone also solved it using bfs.
- distance of two char can be calculated using manhantan distance easily.
- it is not easy to find the relation, top down is more understandable.
- state conversion for dp + top down is a better way to understand and find the solution
  * for this problem initial state (0,0,0), we start at char 0 and finger1 and finger 2 are undefined.
  * if we use finger 1 to word[i], then we get to new state (i+1,word[i],f2)
  * if we use finger 2 to word[i], then we get to new state (i+1,f1,word[i])
  
```cpp
    int memo[300][26][26];
    int minimumDistance(string word) {
        //dp: 
        memset(memo,-1,300*26*26*sizeof(int));
        return dp(0,0,0,word);
        
    }
    int dp(int i,char f1,char f2,string& word){
        if(i>=word.size()) return 0;
        if(f1 && f2 && memo[i][f1-'A'][f2-'A']>=0) 
            return memo[i][f1-'A'][f2-'A'];
        int ans1=dist(f1,word[i])+dp(i+1,word[i],f2,word); //use finger 1, new state (i+1,word[i],f2)
        int ans2=dist(f2,word[i])+dp(i+1,f1,word[i],word); //use finger 2, new state (i+1,f1,word[i])
        if(f1&&f2) memo[i][f1-'A'][f2-'A']=min(ans1,ans2);
        return min(ans1,ans2);
    }
    int dist(char a, char b){
        if(!a||!b) return 0;
        int i=a-'A',j=b-'A';
        return abs(i/6-j/6)+abs(i%6-j%6);
    }
```    

- initial finger position is 0 and we assume its distance to any char is 0.
- we always keep the state: current position, finger 1's char, finger 2's char and keep solving the subproblem.
- memoization to speed the process.
