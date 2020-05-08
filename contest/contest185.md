## contest 185
### 1417. Reformat The String
<em>
Given alphanumeric string s. (Alphanumeric string is a string consisting of lowercase English letters and digits).

You have to find a permutation of the string where no letter is followed by another letter and no digit is followed by another digit. That is, no two adjacent characters have the same type.

Return the reformatted string or return an empty string if it is impossible to reformat the string
</em>

straightforward, divide into letters and numbers. if number is longer,try number first

```cpp
    string reformat(string s) {
        string lett,num;
        for(char c: s){
            if(c>='0'&&c<='9') num+=c;
            else lett+=c;
        }
        if(abs((int)lett.size()-(int)num.size())>1) return "";
        if(lett.size()<num.size()) swap(lett,num);
        int i=0,j=0;
        string ans;
        while(i<lett.size()||j<num.size()) {
            if(i<lett.size()) ans+=lett[i++];
            if(j<num.size()) ans+=num[j++];
        }
        return ans;
    }
```

### 1418. Display Table of Food Orders in a Restaurant
<em>
Given the array orders, which represents the orders that customers have done in a restaurant. More specifically orders[i]=[customerNamei,tableNumberi,foodItemi] where customerNamei is the name of the customer, tableNumberi is the table customer sit at, and foodItemi is the item customer orders.

Return the restaurant's “display table”. The “display table” is a table whose row entries denote how many of each food item each table ordered. The first column is the table number and the remaining columns correspond to each food item in alphabetical order. The first row should be a header whose first column is “Table”, followed by the names of the food items. Note that the customer names are not part of the table. Additionally, the rows should be sorted in numerically increasing order.
</em>

hashmap approach

```cpp
    vector<vector<string>> displayTable(vector<vector<string>>& orders) {
        vector<vector<string>> ans;
        map<int,unordered_map<string,int>> table;
        set<string> food;
        for(auto o: orders){
            table[stoi(o[1])][o[2]]++;
            food.insert(o[2]);
        }
        ans.push_back({});
        ans[0].push_back("Table");
        vector<string> vt(food.size()+1);
        for(auto s: food) ans[0].push_back(s);
        for(auto t: table){
            vt[0]=to_string(t.first);
            int i=1;
            for(auto s: food){
                if(t.second.count(s)) vt[i]=to_string(t.second[s]);
                else vt[i]="0";
                i++;
            }
            ans.push_back(vt);
        }
        return ans;
    }
```

### 1419. Minimum Number of Frogs Croaking
<em>
Given the string croakOfFrogs, which represents a combination of the string "croak" from different frogs, that is, multiple frogs can croak at the same time, so multiple “croak” are mixed. Return the minimum number of different frogs to finish all the croak in the given string.

A valid "croak" means a frog is printing 5 letters ‘c’, ’r’, ’o’, ’a’, ’k’ sequentially. The frogs have to print all five letters to finish a croak. If the given string is not a combination of valid "croak" return -1.
</em>

this is similar to segment overlap. a segment starts with c and end with k.
also we need keep the ordering or croak
```cpp
    int minNumberOfFrogs(string croakOfFrogs) {
        //if overlapped, non-overlapped
        int cnt[5]={0};
        for(char c: croakOfFrogs){
            if(c=='c') cnt[0]++;
            else if(c=='r') cnt[1]++;
            else if(c=='o') cnt[2]++;
            else if(c=='a') cnt[3]++;
            else if(c=='k') cnt[4]++;
            else return -1;
            //the count needs to keep cnt[0]>=cnt[1]>=cnt[2]>=cnt[3]>=cnt[4]
            for(int i=0;i<4;i++){
                if(cnt[i]<cnt[i+1]) return -1;
            }
        }
        if(cnt[1]!=cnt[0] || cnt[2]!=cnt[0] || cnt[3]!=cnt[0] || cnt[4]!=cnt[0]) return -1;
        //using hashmap to record, when we see a k, we need end a word. when we see a c, we need start a word
        int ans=0;
        int pref=0;
        for(char c: croakOfFrogs){
            if(c=='c') pref++;
            if(c=='k') pref--;
            ans=max(ans,pref);
        }
        return ans;
    }
```

### 1420. Build Array Where You Can Find The Maximum Exactly K Comparisons
<em>
Given three integers n, m and k. Consider the following algorithm to find the maximum element of an array of positive integers:
```cpp
	max=-1;
	mind=-1;
	cost=0;
	n=arr.size();
	for(int i=0;i<n;i++){
		if(max<arr[i]){
			max=arr[i];
			mind=i;
			cost++;
		}
	}
	return mind;
```
You should build the array arr which has the following properties:

arr has exactly n integers.
1 <= arr[i] <= m where (0 <= i < n).
After applying the mentioned algorithm to arr, the value search_cost is equal to k.
Return the number of ways to build the array arr under the mentioned conditions. As the answer may grow large, the answer must be computed modulo 10^9 + 7.	
</em>

Intuition: 3d dp problem
dp[i,j,k] i for length of the array, j: the max, k: the cost
base: i=1 just one element
final answer: dp[n,j,K] fix i and k but sum on all j.

```cpp
    long dp[51][101][101];
    int numOfArrays(int n, int m, int k) {
        int mod=1e9+7;
        //search cost=k, n: length, m: max
        //dp top down: number of ways
        //dp[i][j][k]: i length of array, j:the max value? k: the cost so far
        memset(dp,0,51*101*101*sizeof(long));
        //base case: k=0: the first element must be the max. (not a base casse)
        //base case: i=0,j=0,k=0, dp[0][0][0]=1
        //base case dp[0][x][y]=0, dp[]
        //dp[0][0][0]=1;
        for (int j = 1; j <= m; j++)
            dp[1][j][1] = 1;
        for(int i=2;i<=n;i++){
            for(int j=1;j<=m;j++){
                for(int l=1;l<=min(k,i);l++){ //cost is most the same as i
                    //current one contribute a cost or 0 cost
                    for(int a=1;a<j;a++){
                        dp[i][j][l]+=dp[i-1][a][l-1]; //current one is a cost, then it is the max, the previous could from 1 to j-1
                        dp[i][j][l]%=mod;
                    }
                    dp[i][j][l]+=j*dp[i-1][j][l]%mod; //current one is not a cost, the max is still the same, we can choose 1 to j         
                    dp[i][j][l]%=mod;
                }
            }
        }
        long ans=0;
        for(int j=1;j<=m;j++) ans+=dp[n][j][k]%mod,ans%=mod;
        return ans;
    }
```
	
