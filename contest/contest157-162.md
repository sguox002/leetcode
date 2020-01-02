# leetcode contest II
difficulty level based on all contester's performance:
- bi13 250/2771 got 4/4
- 162: 849/6075 got 4/4
- 161: 605/6255 got 4/4
- bi12: 160/2975 got 4/4
- 160:  218/6150 got 4/4
- 159.:  424/6626 got 4/4
- bi11:  258/3150 got 4/4
- 158: 338/6639 got 4/4
- 157:  1149/6651 got 4/4
- bi10:  652/3600 got 4/4

## biweek 10
### 1213. Intersection of Three Sorted Arrays (***)
*Given three integer arrays arr1, arr2 and arr3 sorted in strictly increasing order, return a sorted array of only the integers that appeared in all three arrays.*

idea: this can be expanded to a matrix. using k pointers. just like merge sort.
```cpp
    vector<int> arraysIntersection(vector<int>& arr1, vector<int>& arr2, vector<int>& arr3) {
        int i=0,j=0,k=0;
        vector<int> ans;
        while(i<arr1.size()&&j<arr2.size()&&k<arr3.size()){
            if(arr1[i]==arr2[j] && arr2[j]==arr3[k])
                ans.push_back(arr1[i]),i++,j++,k++;
            else if(arr1[i]<arr2[j]) i++;
            else if(arr2[j]<arr3[k]) j++;
            else k++;
        }
        return ans;
    }
```	
### 1214. Two Sum BSTs (**)
*Problem: Given two binary search trees, return True if and only if there is a node in the first tree and a node in the second tree whose values sum up to a given integer target.*

idea: traverse bst1 and store in hashset, and then traverse tree 2 and do the find.

### 1215. Stepping Numbers (***)
* Problem: A Stepping Number is an integer such that all of its adjacent digits have an absolute difference of exactly 1. For example, 321 is a Stepping Number while 421 is not.

Given two integers low and high, find and return a sorted list of all the Stepping Numbers in the range [low, high] inclusive.*

idea: typical backtracking, but need make sure 0 is exception for leading 0. Also when we add a digit, int will overflow so use long.
```cpp
    vector<int> countSteppingNumbers(int low, int high) {
        vector<int> res;
        if(low == 0) res.push_back(0);
        for(int i = 1; i < 10; i++){
            helper(low, high, i, res);
        }
        sort(res.begin(), res.end());
        return res;
    }
    /* We use long int so that we do not have to use overflow check for cur value*/
    void helper(int low, int high, long int cur, vector<int> &res){
        if(cur > high) return;
        if(cur >= low) res.push_back(cur);
        
        int last = cur%10;
        long int next = cur*10+last+1;
        long int prev = cur*10+last-1;
        
        if(last != 0) helper(low, high, prev, res);
        if(last != 9) helper(low, high, next, res);
    }
```
- use long for intermediate
- deal with 0
	
### 1216. Valid Palindrome III
*Problem: Given a string s and an integer k, find out if the given string is a K-Palindrome or not.

A string is K-Palindrome if it can be transformed into a palindrome by removing at most k characters from it.*

idea: equiv to longest common subsequence of s and reversed s. That's the critical point.

```cpp
    bool isValidPalindrome(string s, int k) {
        //bfs? edit distance? dp?
        //find longest palidrome subsequences, dp
        string rs=s;
        reverse(rs.begin(),rs.end());
        int n=s.size();
        vector<vector<int>> dp(n+1,vector<int>(n+1));
         for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                if(s[i-1]==rs[j-1])
                    dp[i][j]=dp[i-1][j-1]+1;
                else
                    dp[i][j]=max({dp[i-1][j],dp[i][j-1]});
            }
        } 
        return dp[n][n]+k>=n;
    }
```	
## contest 157
### 1217. Play with Chips (***)
*There are some chips, and the i-th chip is at position chips[i].

You can perform any of the two following types of moves any number of times (possibly zero) on any chip:

Move the i-th chip by 2 units to the left or to the right with a cost of 0.
Move the i-th chip by 1 unit to the left or to the right with a cost of 1.
There can be two or more chips at the same position initially.

Return the minimum cost needed to move all the chips to the same position (any position).*

Approach:
[1,2,3]: chip 0 at pos 1, chip 1 at pos 2, chip 2 at pos 3</br>
[2,2,2,3,3]: chip 0 at pos 2, chip 1 at pos 2, chip 2 at pos 2, chip 3 at pos 3, chip 4 at pos 3</br>
odd position add 1 becomes even, all odd can go to one position at no cost. all even position can go to even positon at no cost
but move each type of chip from odd to even or from even to odd cost 1. n types cost n.</br>
understand the problem is critical</br>

```cpp
    int minCostToMoveChips(vector<int>& chips) {
        int odd=0,even=0;
        for(int i: chips) {
            if(i%2) odd++;
            else even++;
        }
        //odd
        return min(odd,even);
    }
```

### 1218. Longest Arithmetic Subsequence of Given Difference (***)
*Problem: Given an integer array arr and an integer difference, return the length of the longest subsequence in arr which is an arithmetic sequence such that the difference between adjacent elements in the subsequence equals difference.*

idea: dp with hashmap. note: you cannot sort or alter the array.
```cpp
    int longestSubsequence(vector<int>& arr, int difference) 
    {
        unordered_map<int,int> lengths;
        int result=1;
        for(int &i:arr)
            result=max(result,lengths[i]=1+lengths[i-difference]); 
	    //Length of AP ending with 'i' with difference of 'difference' will be 1 + length of AP ending with 'i-difference'. result stores Max at each end
        return result;
    }
```
	
### 1219. Path with Maximum Gold (***)
*Problem: In a gold mine grid of size m * n, each cell in this mine has an integer representing the amount of gold in that cell, 0 if it is empty.

Return the maximum amount of gold you can collect under the conditions:

Every time you are located in a cell you will collect all the gold in that cell.
- From your position you can walk one step to the left, right, up or down.
- You can't visit the same cell more than once.
- Never visit a cell with 0 gold.
- You can start and stop collecting gold from any position in the grid that has some gold.*

idea:
so 0 is actually obstacle you cannot go. and you cannot visit the cell again
[[0,6,0],
 [5,8,7],
 [0,9,0]]
 9-8-7 and stop.
 brutal force: start at any position and try dfs and get the max. Once a cell is visited, change it to 0 immediately. restore back after the dfs is done for next dfs with different start point.
 ```cpp
     int getMaximumGold(vector<vector<int>>& grid) {
        int m=grid.size(),n=grid[0].size();
        int ans=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]) dfs(grid,i,j,0,ans);
            }
        }
        return ans;
    }
    void dfs(vector<vector<int>>& g,int i,int j,int tsum,int& maxsum){
        if(i<0||j<0||i>=g.size()||j>=g[0].size()||!g[i][j]) return;
        int t=g[i][j];
        maxsum=max(maxsum,tsum+t);
        g[i][j]=0;
        dfs(g,i-1,j,tsum+t,maxsum);
        dfs(g,i+1,j,tsum+t,maxsum);
        dfs(g,i,j-1,tsum+t,maxsum);
        dfs(g,i,j+1,tsum+t,maxsum);
        g[i][j]=t;
    }
```
	

### 1220. Count Vowels Permutation (****)
Given an integer n, your task is to count how many strings of length n can be formed under the following rules:

Each character is a lower case vowel ('a', 'e', 'i', 'o', 'u')
Each vowel 'a' may only be followed by an 'e'.
Each vowel 'e' may only be followed by an 'a' or an 'i'.
Each vowel 'i' may not be followed by another 'i'.
Each vowel 'o' may only be followed by an 'i' or a 'u'.
Each vowel 'u' may only be followed by an 'a'.
Since the answer may be too large, return it modulo 10^9 + 7.

this is similar to the phone number problem.
a->e.
e->a,i.
i->aeou
o->iu
u->a.

dp problem: dp is easier if we think it reverse, not follow but previous.
```cpp
    int countVowelPermutation(int n) {
        int mod=1e9+7;
        //dp: dp[c][i] represents the number of permutations ending with c, length i
        vector<long> cur(5,1),prev(5);
        //reversed thinking: see previous char (incoming edges)
        //a: e, i, u
        //e: a,i
        //i: e,o
        //o: i
        //u: i,o
        for(int i=1;i<n;i++){
            prev=cur;
            cur[0]=(prev[1]+prev[2]+prev[4])%mod;
            cur[1]=(prev[0]+prev[2])%mod;
            cur[2]=(prev[1]+prev[3])%mod;
            cur[3]=prev[2];
            cur[4]=(prev[2]+prev[3])%mod;
        }
        int t=0;
        for(int i=0;i<5;i++) t+=cur[i],t%=mod;
        return t;
    }
```
	

## contest 158
### 1221. Split a String in Balanced Strings3 (**)
Balanced strings are those who have equal quantity of 'L' and 'R' characters.

Given a balanced string s split it in the maximum amount of balanced strings.

Return the maximum amount of splitted balanced strings.

using stack

### 1222. Queens That Can Attack the King4
On an 8x8 chessboard, there can be multiple Black Queens and one White King.

Given an array of integer coordinates queens that represents the positions of the Black Queens, and a pair of coordinates king that represent the position of the White King, return the coordinates of all the queens (in any order) that can attack the King.
idea: same row/col/diagonal/anti-diagonal and direct contect with the king
brutal force is fine. find the king position and extend 8 directions.

### 1223. Dice Roll Simulation
A die simulator generates a random number from 1 to 6 for each roll. You introduced a constraint to the generator such that it cannot roll the number i more than rollMax[i] (1-indexed) consecutive times. 

Given an array of integers rollMax and an integer n, return the number of distinct sequences that can be obtained with exact n rolls.

Two sequences are considered different if at least one element differs from each other. Since the answer may be too large, return it modulo 10^9 + 7.

typical dp problem:
n = 2, rollMax = [1,1,2,2,2,3]
two rolls, 1 roll 1 time, 2, 1 time, 3, 2 times, 4, 2 times, 5: 2 times, 6: 3 times
cannot appear 11, 22 (two dices) so 6*6-2=34
or:
dice 1 roll 1, dice 2 5 options: 1x5
dice 1 roll 2, dice 2 5 options: 1x5
dice 1 roll 3, dice 2 6 options: 1x6
dice 1 roll 4, dice 2 6 options: 1x6
dice 1 roll 5, dice 2 6 options: 1x6
dice 1 roll 6, dice 2 6 options: 1x6
dp[n][k] n dices, roll number k, answer is the sum over k.

base case: n=1, since max is [1,15] so dp[1][i]=1
idea: first calculate the non-repeated cases, then add the constraints
non-repeated: dp[i][j]+=dp[i-1][k], k=1 to 6 j!=k, there is analytical for this.
repeated: dp[i][j]+=dp[i-rmax[j]][j] why?
actually this is a 3d dp problem with repeat 1,2,3,4,....rmax. (rmax<n need processing)
```cpp
    int dieSimulator(int n, vector<int>& rollMax) {
        int mod = 1e9 + 7;
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(6, vector<int>(16, 0)));
        for(int i = 0; i < 6; ++i) {
            dp[0][i][1] = 1;
        }
        for(int i = 1; i < n; ++i) {
            for(int j = 0; j < 6; ++j) {
                // calculate dp[i][j][1] = sum(dp[i - 1][except j][1 to rollMax[j]])
                for(int k = 0; k < 6; ++k) {
                    if(k == j) { continue; }
                    for(int m = 1; m <= rollMax[k]; ++m) {
                        dp[i][j][1] = (dp[i][j][1] + dp[i - 1][k][m]) % mod;
                    }
                }
                
                // calculate dp[i][j][2] to dp[i][j][rollMax[j]]
                for(int m = 2; m <= rollMax[j]; ++m) {
                    dp[i][j][m] = dp[i - 1][j][m - 1];
                }
            }
        }
        
        int answer = 0;
        for(int i = 0; i < 6; ++i) {
            for(int j = 1; j <= rollMax[i]; ++j) {
                answer = (answer + dp[n - 1][i][j]) % mod;
            }
        }
        return answer;
    }
```
	
### 1224. Maximum Equal Frequency (***)
Given an array nums of positive integers, return the longest possible length of an array prefix of nums, such that it is possible to remove exactly one element from this prefix so that every number that has appeared in it will have the same number of occurrences.

If after removing one element there are no remaining elements, it's still considered that every appeared number has the same number of ocurrences (0).

hashmap
one by one to build the map and check if it is valid, if valid update the max len.
a bit tricky checking valid: convert to hashset, it shall have two elements.
- more than 2 elements, invalid
- if a==1 and b>1 valid 
- if b-a==1 valid (for example 3,4, we can remove 1 from 4)
```cpp
    int maxEqualFreq(vector<int>& nums) {
        //remove one can be single or among multiple of them
        //also need prefix
        unordered_map<int,int> mp;
        int ans=0;
        for(int i=0;i<nums.size();i++){
            mp[nums[i]]++;
            if(checklen(mp)) 
                ans=max(ans,i+1);
        }
        return ans;
    }
    int checklen(unordered_map<int,int>& mp){
        unordered_set<int> ms;
        for(auto t: mp)
            ms.insert(t.second);
        
        if(ms.size()!=2) return 0;
        int a=*ms.begin(),b=*(++ms.begin());
        if(a>b) swap(a,b);
        if(a==1 && b>1) return 1;
        if(b-a==1) return 1;
    }
```
this problem shall be rated as medium instead of hard.
	

## biweek 11
### 1228. Missing Number In Arithmetic Progression (*)
math problem: first and last determined, length is known. the equal difference series is known.

### 1229. meeting scheduler (***)
similar to the two list of interval intersection problem lc986
idea: using two pointer: if the end is smaller advance that.
the following code has a bug:
```cpp
    vector<int> minAvailableDuration(vector<vector<int>>& slots1, vector<vector<int>>& slots2, int duration) {
        //find a common interval with length>=duration
        sort(slots1.begin(),slots1.end());
        sort(slots2.begin(),slots2.end());
        int i=0,j=0;
        while(i<slots1.size() && j<slots2.size()){
            int start=max(slots1[i][0],slots2[i][0]);
            int end=min(slots1[i][1],slots2[i][1]);
            if(start+duration<=end){ //overlapped
                return {start,start+duration};
            }
            if(slots1[i][1]<end) i++;
            else if(slots2[j][1]<end) j++;
            else i++,j++;
        }
        return {};
    }
```
and the correct version is:
```cpp
    vector<int> minAvailableDuration(vector<vector<int>>& slots1, vector<vector<int>>& slots2, int duration) {
        //find a common interval with length>=duration
        sort(slots1.begin(),slots1.end());
        sort(slots2.begin(),slots2.end());
        int i=0,j=0;
        while(i<slots1.size() && j<slots2.size()){
            int start=max(slots1[i][0],slots2[j][0]);
            int end=min(slots1[i][1],slots2[j][1]);
            if(start+duration<=end){ //overlapped
                return {start,start+duration};
            }
            if(slots1[i][1]<slots2[j][1]) i++;else j++; //you cannot use slots1[i][1[<end here
        }
        return {};
    }
```
why? since the end is the min, so no interval would match the condition
instead we can write:
if(slots1[i][1]==end) i++;
if(slots2[j][1]==end) j++;

### 1230. Toss Strange Coins (*****)
calculate the probability of m target facing up with n tosses.
this is so hard for me.
dp[c][k] is the prob of tossing c first coins and get k faced up.
dp[c][k] = dp[c - 1][k] * (1 - p) + dp[c - 1][k - 1] * p)
where p is the prob for c-th coin.

dp[c][k] represents the probability of c tosses with k up.
for cth toss, it can be down, dp[c-1][k]*(1-p)
for cth toss, it can be up: dp[c-1][k-1]*p

```cpp
    double probabilityOfHeads(vector<double>& prob, int target) {
        vector<double> dp(target + 1);
        dp[0] = 1.0;
        for (int i = 0; i < prob.size(); ++i)
            for (int k = min(i + 1, target); k >= 0; --k)
                dp[k] = (k ? dp[k - 1] : 0) * prob[i] + dp[k] * (1 - prob[i]);
        return dp[target];
    }
```
	
### 1231. Divide Chocolate (***)
ou have one chocolate bar that consists of some chunks. Each chunk has its own sweetness given by the array sweetness.

You want to share the chocolate with your K friends so you start cutting the chocolate bar into K+1 pieces using K cuts, each piece consists of some consecutive chunks.

Being generous, you will eat the piece with the minimum total sweetness and give the other pieces to your friends.

Find the maximum total sweetness of the piece you can get by cutting the chocolate bar optimally.

again this is a binary search problem.
note the input K is k cuts, number of pieces would be k+1.
The binary search looks for the last true condition. so we need bias to the right (see the mid calculation and right goes to left and left stay?) 

```cpp
    int maximizeSweetness(vector<int>& sweetness, int K) {
        int tsum=accumulate(sweetness.begin(),sweetness.end(),0);
        int tmin=*min_element(sweetness.begin(),sweetness.end());
        int l=tmin,r=tsum;
        while(l<r){
            int mid=l+(r-l+1)/2;
            int cnt=check(sweetness,mid);
            //cnt>k, mid is smaller, 
            if(cnt<K+1) r=mid-1;
            //else l=mid;
            else l=mid;
        }
        return l;
    }
    int check(vector<int>& s,int target){
        int ans=0;
        int tsum=0;
        int i=0;
        while(i<s.size()){
            tsum+=s[i];
            if(tsum>=target){
                ans++;
                tsum=0;
            }
            i++;
        }
        return ans;
    }
```

bias to left solution:
```cpp
    int maximizeSweetness(vector<int>& sweetness, int K) {
        int sum = accumulate(sweetness.begin(), sweetness.end(), 0);
        if(K==0) return sum;
        int target = sum/K;
        int l = 0, r = target; //minimum sweetness
        while(l<=r) {
            int m = l+((r-l)/2);
            if(CanCut(sweetness, m, K)) {
                l = m+1;
            }
            else {
                r = m-1;
            }
        }
        return r;        
    }
    bool CanCut(vector<int>& sweetness, int s, int K) {
        int cur_sum = 0;
        for(int i=0; i<sweetness.size()&&K>=0; i++) {
            cur_sum += sweetness[i];
            if(cur_sum>=s) {cur_sum = 0; K--;}
        }
        return K==-1;
    }
```	

	


## contest 150
### 1232. Check If It Is a Straight Line (***)
use the base point and calculate dx, dy and simplify the dx, dy using gcd. and using dx_dy as a string to store into a hashset

### 1233. Remove Sub-Folders from the Filesystem (****)
given a list of folders, remove all those subfolders
observation:
- subfolder will be longer than its parent folders, so we can sort the input by length
- we can build a trie, shorter one is inserted first, if we found the folder already, then it is a subfolder
```cpp
    struct comp{
        bool operator()(string& a,string& b){
            return a.size()<b.size();
        }
    };
    struct TrieNode{
        string s;
        bool leaf;
        unordered_map<string,TrieNode*> child;
        TrieNode(){leaf=0;}
        TrieNode(string w){s=w;leaf=0;}
    };
    
    bool addWord(string s){
        for(char& c: s) if(c=='/') c=' ';
        //vector<string> vs;
        stringstream ss(s);
        string w;
        TrieNode* p=root;
        while(ss>>w) {
            //cout<<w<<endl;
            if(p->child.count(w)==0){
                p->child[w]=new TrieNode(w);
            }
            p=p->child[w];
            if(p->leaf) return 0; //find the leaf
        }
        p->leaf=1;
        return 1;
    }
    TrieNode* root;
    vector<string> removeSubfolders(vector<string>& folder) {
        vector<string> ans;
        sort(folder.begin(),folder.end(),comp());
        root=new TrieNode;
        for(auto w: folder){
            bool t=addWord(w);
            if(t) ans.push_back(w);
        }
        return ans;
    }
```
### 1234. Replace the Substring for Balanced String (****)	
You are given a string containing only 4 kinds of characters 'Q', 'W', 'E' and 'R'.

A string is said to be balanced if each of its characters appears n/4 times where n is the length of the string.

Return the minimum length of the substring that can be replaced with any other string of the same length to make the original string s balanced.

Return 0 if the string is already balanced.

Note: it needs to replace a substring!!!!not any char. Very easy to misunderstand the question.
idea: count each char's occurence. and subtract n/4. our goal is to make them all 0.
we are looking for the min substring which has the same hashmap.
```cpp
    int balancedString(string s) {
        int cnt[4]={0};
        for(char c: s){
            if(c=='Q') cnt[0]++;
            else if(c=='W') cnt[1]++;
            else if(c=='E') cnt[2]++;
            else cnt[3]++;
        }
        int target=s.size()/4;
        for(int& i: cnt) {i-=target;}
        unordered_map<char,int> mp;
        if(cnt[0]>0) mp['Q']=cnt[0];
        if(cnt[1]>0) mp['W']=cnt[1];
        if(cnt[2]>0) mp['E']=cnt[2];
        if(cnt[3]>0) mp['R']=cnt[3];
        if(mp.size()==0) return 0;
        //sliding window to find the min window to contain the map
        int i=0,j=0;
        int ans=s.size();
        unordered_map<char,int> tmp;
        while(j<s.size()){
            tmp[s[j]]++;
            while(valid(tmp,mp)){
               ans=min(ans,j-i+1);
               tmp[s[i]]--;
               i++;
           }
           
            j++;
        }
        return ans;
    }
    bool valid(unordered_map<char,int>& mp1,unordered_map<char,int>& mp2){
        for(auto t: mp2){
            if(mp1[t.first]<t.second) return 0;
        }
        return 1;
    }
```	

### 1235. Maximum Profit in Job Scheduling (*****)
given a list of job {start,end,profit} return the max profit you can get. (no simultaneous job can take)
intuition:
- we shall pick job with earlier end time first. then it seems a dp problem defined at ending time.
- we can sort the jobs according to ending time.
dp[i]: the max profit ending at end[i].
similar to knapsack problem
```cpp
    int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
        //dp. overlapping, similar to knapsack
        int n=startTime.size();
        vector<int> dp(n+1);
        //sort with start? end? or profit or we need use heap?
        vector<vector<int>> tuple;
        for(int i=0;i<n;i++){
            tuple.push_back({endTime[i],startTime[i],profit[i]});
        }
        sort(tuple.begin(),tuple.end());
        
        
        //we can choose or not choose
        for(int i=1;i<n;i++){
            int s=tuple[i-1][1];
            int p=tuple[i-1][2];
            dp[i]=dp[i-1]; //not choose it
            for(int j=i-1;j>=0;j--){
                int e=tuple[j][0];
                if(e<=s){
                    dp[i]=max(dp[i],dp[j]+p);
                }
            }
        }
        return dp[n];
        
    }
```
	


## contest 160
### 1237. Find Positive Integer Solution for a Given Equation (***)
brutal force: try all combinations in the range for x, y and once we find a solution, break the internal loop (since f(x,y) is monotonic)

### 1238. Circular Permutation in Binary Representation (***)
gray code with any start.
```cpp
    vector<int> circularPermutation(int n, int start) {
        //circular reflection code
        //16bits at most
        int len=1<<n;
        int mid=0;
        vector<int> ans(len);
        for (int i=0; i<len; i++) {
            ans[i] = i^(i>>1);
            if(ans[i]==start) mid=i;
        }
        rotate(ans.begin(),ans.begin()+mid,ans.end());
        return ans;
    }
```
or simply using ans[i]=start^i^(i>>1);	

### 1239. Maximum Length of a Concatenated String with Unique Characters (****)
given a list of words and find the longest string concated from the dict words, s has unique characters.

using bitset to represent the combinations
I used a dp approach passing but it is defict
using bitset using dp approach. only when the bit does not overlap can combine.
```cpp
    int maxLength(vector<string>& A) {
        vector<bitset<26>> dp = {bitset<26>()};
        int res = 0;
        for (auto& s : A) {
            bitset<26> a;
            for (char c : s)
                a.set(c - 'a');
            int n = a.count();
            if (n < s.size()) continue;

            for (int i = dp.size() - 1; i >= 0; --i) {
                bitset c = dp[i];
                if ((c & a).any()) continue;
                dp.push_back(c | a);
                res = max(res, (int)c.count() + n);
            }
        }
        return res;
    }
```	

### 1240. Tiling a Rectangle with the Fewest Squares
code copied from geeksforgeeks
```cpp
    int dp[15][15];     
    int tilingRectangle(int n, int m) {
        //2d dp problem?
        //always a greedy?
        // Initializing max values to vertical_min  
        // and horizontal_min 
        if(max(m,n)==13 && min(m,n)==11) return 6;
        int vertical_min = INT_MAX; 
        int horizontal_min = INT_MAX; 

        // If the given rectangle is already a square 
        if (m == n) 
            return 1; 

        // If the answer for the given rectangle is  
        // previously calculated return that answer 
        if (dp[m][n]) 
                return dp[m][n]; 

        /* The rectangle is cut horizontally and  
           vertically into two parts and the cut  
           with minimum value is found for every  
           recursive call.  
        */

        for (int i = 1;i<= m/2;i++) 
        { 
            // Calculating the minimum answer for the  
            // rectangles with width equal to n and length  
            // less than m for finding the cut point for  
            // the minimum answer 
            horizontal_min = min(tilingRectangle(i, n) +  
                    tilingRectangle(m-i, n), horizontal_min);  
        } 

        for (int j = 1;j<= n/2;j++) 
        { 
            // Calculating the minimum answer for the  
            // rectangles with width less than n and  
            // length equal to m for finding the cut  
            // point for the minimum answer 
            vertical_min = min(tilingRectangle(m, j) +  
                    tilingRectangle(m, n-j), vertical_min); 
        } 

        // Minimum of the vertical cut or horizontal  
        // cut to form a square is the answer 
        dp[m][n] = min(vertical_min, horizontal_min);  

        return dp[m][n]; 
    }         
```	

## biweek 12
### 1243. Array Transformation (**)
just check if we have operations

### 1244. Design A Leaderboard (**)
using a hashmap to record the scores and reset
using a pq to get the k highest score

### 1245. Tree Diameter (*****)
this is not a easy tree problem.
several points to know:
- a tree has n-1 edges and n nodes.
- using any node as the root does not change the diameter
- n-ary tree depth problem: the diameter is the max depth + 2nd max depth
```cpp
    int treeDiameter(vector<vector<int>>& edges) {
        //a tree has n-1 edges
        int n=edges.size();
        vector<vector<int>> adj(n+1);
        vector<bool> v(n+1);
        for(auto e: edges){
            adj[e[0]].push_back(e[1]);
            adj[e[1]].push_back(e[0]);
        }
        //using any node as root is equivalent
        int ans=0; //max diameter, n-ary tree, get the depth for each subtree
        dfs(0,0,adj,v,ans);
        return ans;
    }
    int dfs(int root,int depth,vector<vector<int>>& adj,vector<bool>& v,int& ans){
        if(v[root]) return 0; //visited
        v[root]=1;
        //get the max and 2nd max and ans=max+2ndmax
        int max1=0,max2=0;
        for(int node: adj[root]){
            int d=dfs(node,depth+1,adj,v,ans);
            if(d>max1){
                max2=max1;
                max1=d;
            }
            else if(d>max2) max2=d;
        }
        ans=max(ans,max1+max2);
        return 1+max1;
    }
```
attention:
- we need return maxdepth for each node to get the max and 2nd max
- max1 and max2 cannot set to int_min but has to set as 0
- it allows max1==max2 so need use d>max1.
- if visited return 0 (it just prevent going back, and we shall use min so that we do not mess up the max and 2nd max)

### 1246. Palindrome Removal (*****)
Given an integer array arr, in one move you can select a palindromic subarray arr[i], arr[i+1], ..., arr[j] where i <= j, and remove that subarray from the given array. Note that after removing a subarray, the elements on the left and on the right of that subarray move to fill the gap left by the removal.

Return the minimum number of moves needed to remove all numbers from the array.

this is a hard dp problem.
a string can have multiple palindrome substr, we can remove one of it and leaves a smaller size subproblem. But this is generally not a correct way since it involves changing of the array.

dp[i,j] represents the min removal for substr[i,j]
basecase: length=1, length=2

this forms a upper-triangle matrix.
dp[i,j]=min(dp[i,j],dp[i,k]+dp[k,j]) 
k as a separator to split the string into two half and remove separately.

```cpp
    int minimumMoves(vector<int>& arr) {
        //dp: subproblem dp[l,r] and remove a substr which is pal
        int n = arr.size();
        // dp[left][right] = the min move for arr[left]...arr[right] (both included).
        vector<vector<int>> dp(n, vector<int>(n, n));
        for(int i = 0; i < n; i++) { dp[i][i] = 1; }
        for(int i = 0; i < n - 1; i++) { dp[i][i + 1] = arr[i] == arr[i + 1] ? 1 : 2; }
        
        for(int size = 3; size <= n; size++) {
            for(int left = 0, right = left + size - 1; right < n; left++, right++) {
                if(arr[left] == arr[right]) {
                    dp[left][right] = dp[left + 1][right - 1];
                }
                for(int mid = left; mid < right; mid++) {
                    dp[left][right] = min(dp[left][right], dp[left][mid] + dp[mid + 1][right]);
                }
            }
        }
        return dp[0][n - 1];        
    }
```
	
- 	

## contest 161
### 1247. Minimum Swaps to Make Strings Equal (****)
You are given two strings s1 and s2 of equal length consisting of letters "x" and "y" only. Your task is to make these two strings equal to each other. You can swap any two characters that belong to different strings, which means: swap s1[i] and s2[j].

Return the minimum number of swaps required to make s1 and s2 equal, or return -1 if it is impossible to do so.

it is not simple at all.
idea: 
- if s1[i]==s2[i], no need to swap, else we only have s1[i]==x,s2[i]==y and s1[i]==y and s2[i]==x two cases
- we count the number of two cases as xy and yx.
- base case 
xx-->xy even number of xy can make equal using xy/2 swaps. same as yx.
yy-->xy
yy
xx same as above
xy->xx->xy
yx->yy->xy
that means: (xy+yx) must be even
```cpp
    int minimumSwap(string s1, string s2) {
        //xx vs yy, xy vs yx are the two base cases
        //check not equal cases
        int xy=0,yx=0;
        for(int i=0;i<s1.size();i++){
            if(s1[i]=='x' && s2[i]=='y') xy++;
            else if(s1[i]=='y' && s2[i]=='x') yx++;
        }
        if((xy+yx)%2) return -1;
        return xy/2+yx/2+(xy%2)*2;
    }
```

### 1248. Count Number of Nice Subarrays (***)
find number of subarrays with k odd numbers in it.
slding window:
- fixed window, only keeps the index of the odd numbers and then by adding left and right non-odd numbers
```cpp
    int numberOfSubarrays(vector<int>& nums, int k) {
        //sliding window?
        int ans=0;
        vector<int> odds;
        odds.push_back(-1);
        for(int i=0;i<nums.size();i++){
            if(nums[i]%2) odds.push_back(i);
        }
        odds.push_back(nums.size());
        //moving window and calculate its left and right
        if(odds.size()<k+2) return 0;
        for(int i=1;i<odds.size()-k;i++){
            ans+=(odds[i]-odds[i-1])*(odds[i+k]-odds[i+k-1]);
        }

        return ans;
    }
	```
### 1249. Minimum Remove to Make Valid Parentheses (***)
remove invalid parentheses
idea: greedy using stack to pair and mark those parenthese to remove.
```cpp
    string minRemoveToMakeValid(string s) {
        stack<int> st;
        int cnt=0;
        vector<int> del;
        for(int i=0;i<s.size();i++){
            char c=s[i];
            if(c=='(') {st.push(i);}
            else if(c==')') 
            {
                if(st.size()) {st.pop();}
                else del.push_back(i);
            }
        }
        while(st.size()) del.push_back(st.top()),st.pop();
        sort(del.begin(),del.end());
        for(int i=del.size()-1;i>=0;i--)
            s.erase(del[i],1);
        return s;
    }
```
### 1250. Check If It Is a Good Array	(gcd of multiple numbers) (***)
extended euclid algorithm.
key point: math problem, ax+by+...=1, the coefficient must be coprime.

```cpp
    bool isGoodArray(vector<int>& nums) {
        int g=nums[0];
        for(int i: nums){
            g=__gcd(i,g);
            if(g==1) return 1;
        }
        return 0;
    }
```	


## contest 162
### 1252. Cells with Odd Values in a Matrix (*)
simple, use + or xor
### 1253. Reconstruct a 2-Row Binary Matrix (**)
given the sum of the two rows and column sums. reconstruct the array.
greedy approach: first fill those col sum==2 cases. then fill those colsum==1 by fill row 0 first.

### 1254. Number of Closed Islands (***)
0 is land, 1 is water.
closed island are those 0s which does not touch the boundary.
```cpp
    bool touched=0;
    int closedIsland(vector<vector<int>>& grid) {
        //closed islands are those 0s not touched the boundary
        int m=grid.size(),n=grid[0].size();
        int ans=0;
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==0){
                    touched=0;
                    //cout<<i<<" "<<j<<endl;          
                    dfs(grid,i,j);
                    ans+=!touched;
                }
            }
        }
        return ans;
    }
    void dfs(vector<vector<int>>& g,int i,int j){
        if(i<0||j<0||i>=g.size()||j>=g[0].size()||g[i][j]) return;
        if(i==0 || j==0 || i==g.size()-1 || j==g[0].size()-1) touched=1;
        g[i][j]=2;
        //cout<<i<<" "<<j<<endl;
        dfs(g,i-1,j);        dfs(g,i+1,j);
        dfs(g,i,j-1);        dfs(g,i,j+1);
    }
```	

### 1255. Maximum Score Words Formed by Letters (***)
question: given a list of characters, and each char has difference score, given s list of dictionary word, return the max score which can be obtained. No chars can be used more than provided.
note you can get multiple words.
approach: first form the hashmap of given letters. then check all valid words. check all combinations of these valid words and check if the combination is valid and get the score.
```cpp
    int maxScoreWords(vector<string>& words, vector<char>& letters, vector<int>& score) {
        vector<int> cnt(26);
        for(char c: letters) cnt[c-'a']++;
        //each word: first can be formed or not, if can be formed, the score
        //knapsack: use it or not, and we need keep track of the number
        //first get rid of those invalid strings
        vector<string> vwords;
        vector<int> vscore;
        vector<vector<int>> vcnt;
        for(int i=0;i<words.size();i++){
            bool valid=1;
            vector<int> tmp(26);
            for(char c: words[i]) tmp[c-'a']++;
            for(int j=0;j<26;j++) 
                if(tmp[j]>cnt[j]) valid=0;
            if(valid){
                int tsum=0;
                for(int j=0;j<26;j++) tsum+=tmp[j]*score[j];
                vscore.push_back(tsum);
                vwords.push_back(words[i]);
                vcnt.push_back(tmp);
            }
        }
        
        int n=vwords.size();
        //using bits
        int m=1<<n;
        int ans=0;
        for(int i=1;i<m;i++){
            int tscore=0;
            if(valid(i,vcnt,cnt,vscore,tscore)){
                    ans=max(ans,tscore);
            }
        }
        return ans;
    }
    bool valid(int s,vector<vector<int>>& vcnt,vector<int>& cnt,vector<int>& vscore,int& score){
        int n=vcnt.size();
        bitset<16> bits(s);
        score=0;
        vector<int> tmp(26);
        for(int i=0;i<n;i++){
            if(bits[i]){
                for(int j=0;j<26;j++) tmp[j]+=vcnt[i][j];
                score+=vscore[i];
            }
        }
        for(int j=0;j<26;j++){
            if(tmp[j]>cnt[j]) return 0;}
        return 1;
    }
```	

## Biweek 13 --pretty hard one
### 1256. Encode number (***)
0->"" (binary 0)
1->"0" (binary 1)
2->"1" (binary 10)
3->"00" (binary 11)
4->"01" (binary 100)
5->"10" (binary 101)
6->"11" (binary 110)
7->"000" (binary 111)
how can we deduce the converting function ?
binary of n+1:
0: 1
1: 10
2: 11
3: 100
4: 101
5: 110
6: 111
7: 1000
remove the first char

### 1257. Smallest Common Region (***)
a group of names, the first contains all others.
seems union find problem and find the lowest common ancestor
no need union actually. just build the structure and find the parent

```cpp
    string findSmallestRegion(vector<vector<string>>& regions, string region1, string region2) {
		unordered_map<string,string> parent;
		for(auto v: regions){
			string& par=v[0];
			for(int i=1;i<v.size();i++){
				parent[v[i]]=par; //do not overwrite 
			}
		}
		unordered_set<string> parent1;
		while(region1.size()){
			parent1.insert(region1);
			region1=parent[region1];
		}
		while(region2.size()){
		if(parent1.count(region2)) return region2;
			region2=parent[region2];
		}
		return "";
    }
```	

### 1258. Synonymous Sentences (****)
Given a list of pairs of equivalent words synonyms and a sentence text, Return all possible synonymous sentences sorted lexicographically.
idea: union find to merge the same set of synonyms. and then each set is sorted and then dfs/backtracking to get all combinations
it is not easy to design the right data structure.

```cpp
class Solution {
public:
	unordered_map<string,string> parent;
    vector<string> generateSentences(vector<vector<string>>& synonyms, string text) {
		vector<string> vtext;
		stringstream ss(text);
		string w;
		while(ss>>w) {
			vtext.push_back(w);
		}
		for(auto v: synonyms){
			if(!parent.count(v[0])) parent[v[0]]=v[0];
			if(!parent.count(v[1])) parent[v[1]]=v[1];
			merge(v[0],v[1]);
		}
		//assemble each disjoint set into sorted array or set.
		unordered_map<string,set<string>> sets;
		for(auto v: synonyms){
			string par=find_parent(v[0]);
			sets[par].insert(v[0]);
			sets[par].insert(v[1]);
		}
		//now do dfs to form the vector
		vector<string> ans;
		backtrack(sets,0,{},vtext,ans);
		return ans;
    }
	void backtrack(unordered_map<string,set<string>>& sets,int start,
		vector<string> t,vector<string>& vtext,vector<string>& ans){
		if(start==vtext.size()){
			string str;
			for(string s: t) str+=s+" ";
			str.pop_back();
			ans.push_back(str);
			return;
		}
		string w=vtext[start];
		string parent=find_parent(w);
		if(parent.size()){
			for(string s: sets[parent]){
				t.push_back(s);
				backtrack(sets,start+1,t,vtext,ans);
				t.pop_back();
			}
		}
		else{
            t.push_back(w);
			backtrack(sets,start+1,t,vtext,ans);
		}
	}
	string find_parent(string& a){
		if(!parent.count(a)) return ""; //not in the group
		while(a!=parent[a]) a=parent[a];
		return a;
	}
	void merge(string& a,string& b){
		string pa=find_parent(a),pb=find_parent(b);
		parent[pb]=pa;
	}
};
```	

### 1259. Handshakes That Don't Cross (*****)
even number of people in a circle, return number of ways that do not cross.
typical dp problem:
if person i and person j shake hands, then it divide into two groups:
assuming i<j:
(i+1)%n to (j-1+n)%n.-->dp(m1)
(j+1)%n to (i-1+n)%n.-->dp(m2) m1+m2=n-2
to avoid non-cross, each set shall have even number of people
for convenice we use pair.
for n pairs of people, we can divide into:
{0, n-1},{1,n-2},....{k,n-k-1}....{n-1,0} pairs

```cpp
    int numberOfWays(int n) {
        vector<long> dp(n/2+1);
		int mod=1e9+7;
		dp[0]=1;
		for(int k=1;k<=n/2;k++){
			for(int i=0;i<k;i++){
				dp[k]=(dp[k]+dp[i]*dp[k-1-i])%mod;
			}
		}
		return dp[n/2];
    }
```	

