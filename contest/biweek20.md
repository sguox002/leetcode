## biweek 20
this contest is simple

### 1356. Sort Integers by The Number of 1 Bits
sort by bits

```cpp
    vector<int> sortByBits(vector<int>& arr) {
        vector<vector<int>> v;
        for(int i: arr){
            int t=bitset<16>(i).count();
            v.push_back({i,t});
        }
        sort(begin(v),end(v),[](vector<int>& a,vector<int>& b){
            return a[1]<b[1] || (a[1]==b[1] && a[0]<b[0]);
        });
        vector<int> ans;
        for(auto t: v) ans.push_back(t[0]);
        return ans;
    }
```

### 1357 Apply discount
simple math. using hashmap

```cpp
    int cnt;
    unordered_map<int,int> mp;
    int discount;
    int n;
    Cashier(int n, int discount, vector<int>& products, vector<int>& prices) {
        cnt=0;
        for(int i=0;i<products.size();i++) mp[products[i]]=prices[i];
        this->discount=discount;
        this->n=n;
    }
    
    double getBill(vector<int> product, vector<int> amount) {
        cnt++;
        double ans=0;
        for(int i=0;i<product.size();i++){
            ans+=mp[product[i]]*amount[i];
        }
        if(cnt==n){
            ans*=(100-discount)/100.0;
            cnt=0;
        }
        return ans;
    }
```

### 1358. Number of Substrings Containing All Three Characters	
simple sliding window
important: all prefix is the substring,, we only need to keep the smallest substring.
```cpp
    int numberOfSubstrings(string s) {
        //mvoing window
        int ans=0;
        int cnt[3]={0};
        int i=0,j=0;
        //the min window which contains the abc
        while(j<s.size()){
            cnt[s[j]-'a']++;
            while(cnt[0] && cnt[1] && cnt[2]){
                cnt[s[i]-'a']--;
                i++;
            }
            ans+=i;
            j++;
        }
        return ans;
    }
```

### 1359. Count All Valid Pickup and Delivery Options
simple dp:
we have n options for the first position. then reduces to n-1 subproblem. D can be put in 2n-1 positions
the total dp[n]=n*(2n-1)*dp[n-1]
can be O(1) space

```cpp
    int countOrders(int n) {
        int mod=1e9+7;
        //permutation problem:
        //0: has n options, 1, has 2n-1 options,.... 2n has n options
        //once we fixed the first postion Pi, the reduces to similar problem, with Di can be inserted in it anywhere
        //dp[n]=n*[(2*n-1)*dp(n-1)]
        vector<long> dp(n+1);
        dp[0]=1;
        for(int i=1;i<=n;i++){
            dp[i]=(long)i*(2*i-1)*dp[i-1];
            dp[i]%=mod;
        }
        return dp[n];
    }
```

- be sure to use long to avoid overflow.
	
