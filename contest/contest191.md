## contest 191

### 1464. Maximum Product of Two Elements in an Array
<em>Given the array of integers nums, you will choose two different indices i and j of that array. Return the maximum value of (nums[i]-1)*(nums[j]-1).
</em>

Trivial, just use O(N^2) two loop
sort and use the max and 2nd max. O(nlogn)
O(N): find max and 2nd max one pass
```cpp
    int maxProduct(vector<int>& nums) {
        int a=INT_MIN,b=INT_MIN;
        for(int i: nums){
            if(i>a){
                b=a;
                a=i;
            }
            else if(i>b){
                b=i;
            }
        }
        return (a-1)*(b-1);
    }
```

### 1465. Max area of a piece of cake after horizontal and vertical cuts

although the problem description is confusing, but the problem is simple.
just add a top and bottom, left and right (virtual cut) and sort the cuts and get the max horizontal interval and vertical interval.

```cpp
    int maxArea(int h, int w, vector<int>& horizontalCuts, vector<int>& verticalCuts) {
        int mod=1e9+7;
        horizontalCuts.push_back(h);
        verticalCuts.push_back(w);
        sort(begin(horizontalCuts),end(horizontalCuts));
        sort(begin(verticalCuts),end(verticalCuts));
        //intervals, get the vertical max and horiztonal max
        int mxh=0,mxv=0;
        int pre=0;
        for(auto h: horizontalCuts)
            mxh=max(mxh,h-pre),pre=h;
        pre=0;
        for(auto v: verticalCuts)
            mxv=max(mxv,v-pre),pre=v;
        return (long)mxh*mxv%mod;
    }
```

### 1466. Reorder Routes to Make All Paths Lead to the City Zero
<em>
There are n cities numbered from 0 to n-1 and n-1 roads such that there is only one way to travel between two different cities (this network form a tree). Last year, The ministry of transport decided to orient the roads in one direction because they are too narrow.

Roads are represented by connections where connections[i] = [a, b] represents a road from city a to b.

This year, there will be a big event in the capital (city 0), and many people want to travel to this city.

Your task consists of reorienting some roads such that each city can visit the city 0. Return the minimum number of edges changed.

It's guaranteed that each city can reach the city 0 after reorder.
</em>

key: the graph is a tree root at 0.
building the graph with the edge direction information and then do a dfs from 0, count those direction not exist.

```cpp
    int minReorder(int n, vector<vector<int>>& connections) {
        //0 is the root, and all other shall be subtree
        int ans=0;
        vector<vector<pair<int,int>>>  adj(n);
        for(auto c: connections){
            adj[c[1]].push_back({c[0],1}); //incoming
            adj[c[0]].push_back({c[1],0});//incoming
        }
        //dfs
        vector<bool> v(n);
        dfs(adj,0,v,ans);
        return ans;
    }
    void dfs(vector<vector<pair<int,int>>>& adj,int root,vector<bool>& v,int& ans){
        v[root]=1;
        for(auto p: adj[root]){ //only check incoming 
            int node=p.first,incoming=p.second;
            if(v[node]) continue;
            if(!incoming) ans++;
            dfs(adj,node,v,ans);
        }
    }
```

use positive and negative for direction can simplify the data structure a bit.

### 1467. Probability of a Two Boxes Having The Same Number of Distinct Balls
<em>
Given 2n balls of k distinct colors. You will be given an integer array balls of size k where balls[i] is the number of balls of color i. 

All the balls will be shuffled uniformly at random, then we will distribute the first n balls to the first box and the remaining n balls to the other box (Please read the explanation of the second example carefully).

Please note that the two boxes are considered different. For example, if we have two balls of colors a and b, and two boxes [] and (), then the distribution [a] (b) is considered different than the distribution [b] (a) (Please read the explanation of the first example carefully).

We want to calculate the probability that the two boxes have the same number of distinct balls.
</em>

This is a permutation problem.
- before that we need to know the permutation with duplicates n!/(A1!*....*An!)
- then we only need to count the number of permutation with equal distinct color, which can be obtained using backtracking.
- backtracking is combination approach, after we get a valid combination we need to get the permutation
- backtracking for length n array would be (A+1)^n.
- use double for factorial.

The following code gives correct results however TLE.
```cpp
    double getProbability(vector<int>& balls) {
        int num2=accumulate(begin(balls),end(balls),0); //total number of balls
        int n=num2/2;
        double total=factor(num2);
        for(int i: balls) total/=factor(i);
        int nc=balls.size();
        double cnt=0;
        backtrack(balls,n,0,0,0,nc,{},cnt);
        return double(cnt)/total;
    }
    double factor(int n){
        if(n==0) return 1;
        double ans=1;
        for(int i=2;i<=n;i++) ans*=i;
        return ans;
    }
    double getperm(vector<int>& t,int n){
        double total=factor(n);
        for(int i: t) total/=factor(i);
        return total;
    }
    void backtrack(vector<int>& balls,int n,int start,int cnt,int nc0,int nc1,vector<int> t,double& ans){
        if(cnt==n && nc0==nc1){ //found one valid combination
            double t1=getperm(t,n);
            vector<int> tt=balls;
            for(int i=0;i<balls.size();i++) tt[i]=balls[i]-(i<t.size()?t[i]:0);
            double t2=getperm(tt,n);
            ans+=t1*t2;
            return;// ans;
        }
        if(start>=balls.size() || cnt>n) return;
        for(int j=0;j<=balls[start];j++){
            t.push_back(j);
            backtrack(balls,n,start+1,cnt+j,nc0+(j>0),nc1-(j==balls[start]),t,ans);
            t.pop_back();
        }
    }
```	

- using vector value passing is the key to slow down the speed.

the following code passes the tests with only change that the vector passing by reference:
```cpp
    double getProbability(vector<int>& balls) {
        //combination & permutation
        //first calculate number of total permutation
        int num2=accumulate(begin(balls),end(balls),0); //total number of balls
        int n=num2/2;
        //consider 2n slots and get the permutation, need get rid of those same
        //n!/(a1!*a2!....)
        double total=factor(num2);
        for(int i: balls) total/=factor(i);
        //now calculate the number of permutation with equal distinct color
        //get n balls, combination and then permutation
        int nc=balls.size();
        double cnt=0;
        //may need memoization
        //memset(dp,0,9*100*50*sizeof(double));
        vector<int> a(balls.size());
        backtrack(balls,n,0,0,0,nc,a,cnt);
        //cout<<cnt<<" "<<total<<endl;
        return double(cnt)/total;
    }
    double factor(int n){
        if(n==0) return 1;
        double ans=1;
        for(int i=2;i<=n;i++) ans*=i;
        return ans;
    }
    double getperm(vector<int>& t,int n){
        double total=factor(n);
        for(int i: t) total/=factor(i);
        return total;
    }
    double backtrack(vector<int>& balls,int n,int start,int cnt,int nc0,int nc1,vector<int>& t,double& ans){
        //cout<<n<<" "<<start<<" "<<cnt<<" "<<nc0<<" "<<nc1<<endl;
        if(cnt==n && nc0==nc1){ //found one valid combination
            //combination-->get the 
            double t1=getperm(t,n);
            vector<int> tt=balls;
            for(int i=0;i<balls.size();i++) tt[i]=balls[i]-t[i];
            double t2=getperm(tt,n);
            //cout<<"found"<<t1<<" "<<t2<<endl;
            ans+=t1*t2;
            return t1*t2;
            //return;// ans;
        }
        if(start>=balls.size() || cnt>n) return 0;
        double res=0;
        for(int j=0;j<=balls[start];j++){
            t[start]=j;
            res+=backtrack(balls,n,start+1,cnt+j,nc0+(j>0),nc1-(j==balls[start]),t,ans);
        }
        return res;
    }
```
- The complexity is O(M^N)

we can add memoization to reduce the complexity.
The state depends on start, cnt, nc. (the other side cnt and nc is totally dependent on this side's choice)
--wrong, cnt and nc cannot decide nc1. It also depends on each individual choices.
- or it totally depends on the each element choice.
similar to bitset, but each bit we have multiple choices, we can use string. Yes it works but still TLE since convert vector to string causes extra effort.

backtracking + memoization ==> dp.
```cpp
class Solution {
public:
    
    double getProbability(vector<int>& balls) {
        int num2=accumulate(begin(balls),end(balls),0); //total number of balls
        int n=num2/2;
        double total=factor(num2);
        for(int i: balls) total/=factor(i);
        int nc=balls.size();
        //memset(dp,0,9*100*50*50*sizeof(double));
        //vector<int> a(balls.size());
        string a(balls.size(),'0');;
        vector<unordered_map<string,double>> dp(balls.size());
        double cnt=backtrack(balls,n,0,0,0,nc,a,dp);
        return double(cnt)/total;
    }
    double factor(int n){
        if(n==0) return 1;
        double ans=1;
        for(int i=2;i<=n;i++) ans*=i;
        return ans;
    }
    double getperm(vector<int>& t,int n){
        double total=factor(n);
        for(int i: t) total/=factor(i);
        return total;
    }    
    double getperm(string& t,int n){
        double total=factor(n);
        for(char i: t) total/=factor(i-'0');
        return total;
    }
    double backtrack(vector<int>& balls,int n,int start,int cnt,int nc0,int nc1,string& t,vector<unordered_map<string,double>>& dp){
        
        if(cnt==n && nc0==nc1){ //found one valid combination
            double t1=getperm(t,n);
            vector<int> tt=balls;
            for(int i=0;i<balls.size();i++) tt[i]=balls[i]-(t[i]-'0');
            double t2=getperm(tt,n);
            return t1*t2;
        }
        if(start>=balls.size() || cnt>n) return 0;
        //string s=state2str(t);
        if(dp[start][t]>1e-6) return dp[start][t];
        double res=0;
        for(int j=0;j<=balls[start];j++){
            t[start]='0'+j;
            res+=backtrack(balls,n,start+1,cnt+j,nc0+(j>0),nc1-(j==balls[start]),t,dp);
        }
        return dp[start][t]=res;
    }
};
```

However this takes longer than the backtracking.
