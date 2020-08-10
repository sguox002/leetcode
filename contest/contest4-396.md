## contest 4
### 396. Rotate Function (***)
find the recurrence relation using the example

```cpp
    int maxRotateFunction(vector<int>& A) {
        //f(k)=sum(i*Bi), f(k+1)=sum((i+1)%n*Bi)
        //f(1)-f(0)=A0+A1+....An-1-n*An-1
        //f(2)-f(1)=A0+A1+....An-1-n*An-2
        long tsum=accumulate(A.begin(),A.end(),0ll);
        int n=A.size();
        long ans=0;
        for(int i=0;i<n;i++)
            ans+=i*A[i];
        long sum=ans;
        for(int i=1;i<n;i++){
            //cout<<sum<<endl;
            sum+=tsum-(long)n*A[n-i];
            ans=max(ans,sum);
        }
        return ans;        
     }
```

### 397. Integer Replacement (***)
greedy: if (n+1)%4==0 then +1, if n-1%4==0 then -1
with exception n=3;

```cpp
    int integerReplacement(int n) {
        //equiv to from 1 to n using *2 and +/-1
        //greedy: if n+1 %4==0 +1, n-1%4==0 -1
        int ans=0;
        long nn=n;
        while(nn>1){
            if(nn%2==0) nn/=2;
            else{
                if(nn>3 && (nn+1)%4==0) nn++;
                else nn--;
            }
            ans++;
        }
        return ans;
    }
```

### 398. random pick index (***)
hashmap to store the index
```cpp
    unordered_map<int,vector<int>> mp;
    Solution(vector<int>& nums) {
        for(int i=0;i<nums.size();i++){
            mp[nums[i]].push_back(i);
        }
    }
    
    int pick(int target) {
        return mp[target][rand()%mp[target].size()];
    }
```
### 399. evaluate division (*****)
dfs approach: using previous value to cascade the division. using visited to avoid repeat visit.
union find: only then a and b in the same set, and then find the common parent a/b=(a/parent) * (parent/b)	
```cpp
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        //dfs: build a graph
        unordered_map<string,unordered_map<string,double>> mp;
        for(int i=0;i<equations.size();i++){
            mp[equations[i][0]].insert({equations[i][1],values[i]});
            mp[equations[i][1]].insert({equations[i][0],1.0/values[i]});
        }
        int n=queries.size();
        vector<double> ans(n,1);
        for(int i=0;i<n;i++) {
            unordered_set<string> v;
            ans[i]=solve(mp,queries[i][0],queries[i][1],1.0,v);
        }
        return ans;
    }
    double solve(unordered_map<string,unordered_map<string,double>>& mp,string a,string b,double prev,unordered_set<string>& v){
        if(!mp.count(a) || !mp.count(b)) return -1.0;
        if(a==b) return prev;
        v.insert(a);
        for(auto p: mp[a]){ 
            if(v.count(p.first)) continue;
            double ans=solve(mp,p.first,b,prev*p.second,v);
            if(ans>0) return ans;
        }
        return -1.0;
    }
```