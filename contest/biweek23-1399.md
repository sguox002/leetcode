## biweek 23, 4/4/2020

### 1399. Count Largest Group
<em>
Given an integer n. Each number from 1 to n is grouped according to the sum of its digits. 

Return how many groups have the largest size.
</em>

straightforward:

```cpp
    int countLargestGroup(int n) {
        unordered_map<int,int> mp;
        for(int j=1;j<=n;j++){
            int sum=0;
            int i=j;
            while(i){
                sum+=i%10;
                i/=10;
            }
            mp[sum]++;
        }
        int ans=0,mx=0; //number of max group
        for(auto t: mp) {
            if(t.second>mx){
                mx=t.second;
                ans=1;
            }
            else if(t.second==mx){
                ans++;
            }
        }
        return ans;
    }
```


### 1400. Construct K Palindrome Strings
<em>
Given a string s and an integer k. You should construct k non-empty palindrome strings using all the characters in s.

Return True if you can use all the characters in s to construct k palindrome strings or False otherwise.
</em>

Observation:
- odd characters <=k
- string size >=k

```cpp
    bool canConstruct(string s, int k) {
        //k pal, each string shall at least contain 1 char
        if(s.size()<k) return 0;
        //num types of char <k, we are ok to build
        vector<int> cnt(26);
        for(char c: s) cnt[c-'a']++;
        int nchar=0,nodd=0;
        for(int i: cnt) {
            nchar+=(i>0);
            nodd+=(i%2);
        }
        if(nchar<=k) return 1;
        //number of odd chars
        if(nodd>k) return 0;
        return 1;
    }
```

lee's solution:
```cpp
    bool canConstruct(string s, int k) {
        bitset<26> odd;
        for (char& c : s)
            odd.flip(c - 'a');
        return odd.count() <= k && k <= s.length();
    }
```

### 1401. Circle and Rectangle Overlapping
<em>
Given a circle represented as (radius, x_center, y_center) and an axis-aligned rectangle represented as (x1, y1, x2, y2), where (x1, y1) are the coordinates of the bottom-left corner, and (x2, y2) are the coordinates of the top-right corner of the rectangle.

Return True if the circle and rectangle are overlapped otherwise return False.

In other words, check if there are any point (xi, yi) such that belongs to the circle and the rectangle at the same time.
</em>

- check if rectangle boundary point <radius
- check if circle inside rectangle
```cpp
    bool checkOverlap(int radius, int x_center, int y_center, int x1, int y1, int x2, int y2) {
        //check all points on the rectangle distance to the center to see if any distance <=r
        
        for(int x=x1;x<=x2;x++){
            int d1=(x-x_center)*(x-x_center)+(y1-y_center)*(y1-y_center);
            int d2=(x-x_center)*(x-x_center)+(y2-y_center)*(y2-y_center);
            if(min(d1,d2)<=radius*radius) return 1;
        }
        for(int y=y1;y<=y2;y++){
            int d1=(x1-x_center)*(x1-x_center)+(y-y_center)*(y-y_center);
            int d2=(x2-x_center)*(x2-x_center)+(y-y_center)*(y-y_center);
            if(min(d1,d2)<=radius*radius) return 1;
        }
        //also circle is inside rect.
        if(x_center-radius>=x1 && x_center+radius<=x2 && y_center-radius>=y1 && y_center+radius<=y2) return 1;
        return 0;
    }
```

### 1402. Reducing Dishes
<em>
A chef has collected data on the satisfaction level of his n dishes. Chef can cook any dish in 1 unit of time.

Like-time coefficient of a dish is defined as the time taken to cook that dish including previous dishes multiplied by its satisfaction level  i.e.  time[i]*satisfaction[i]

Return the maximum sum of Like-time coefficient that the chef can obtain after dishes preparation.

Dishes can be prepared in any order and the chef can discard some dishes to get this maximum value.
</em>

- greedy: since initial order does not matter, so we need sort it. put the larger satisfaction value in the behind
- dp: O(N^2) 

```cpp
    int maxSatisfaction(vector<int>& satisfaction) {
        //sum(A[i]*(i+1))
        //dp: each dish can be discarded or not
        //dp[i][j]=max(dp[i-1][j-1]+s[i]*j,dp[i-1][j])
        sort(begin(satisfaction),end(satisfaction));
        int n=satisfaction.size();
        vector<vector<int>> dp(n+1,vector<int>(n+1,INT_MIN));
        int ans=0;
        for(int i=1;i<=n;i++){ //number of dishes
            for(int j=1;j<=i;j++){ //number of dishes cooked
                dp[i][j]=max((dp[i-1][j-1]==INT_MIN?0:dp[i-1][j-1])+satisfaction[i-1]*j,dp[i-1][j]);
                ans=max(ans,dp[i][j]);
            }
        }
        return ans;
    }
```	
Note: initialize as 0 is incorrect since it will give incorrect results for negative values.

	
