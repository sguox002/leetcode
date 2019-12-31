# leetcode contests

## warmup contest
### 386. Lexicographical Numbers (***)
backtracking: starting with 1 to 9 and then append digits after it.
```cpp
    vector<int> lexicalOrder(int n) {
        vector<int> ans;
        string ns=to_string(n);
        int ndigit=ns.size();
        for(int i=1;i<=9;i++)
          backtrack(n,i,ans);
        return ans;
    }
    void backtrack(int n,int t,vector<int>& ans){
        int last_digit=t%10;
        if(t>n) return;
        if(t<=n) ans.push_back(t);
        for(int i=0;i<=9;i++){
            t=t*10+i;
            backtrack(n,t,ans);
            t/=10;
        }
    }
```

### 387. first unique character in a string (*)
using hashmap

### 388. longest absolute file path (****)
tree structure using stack to store the directory
also stringstream and string proces.
```cpp
    int lengthLongestPath(string input) {
        //file tree can use hashmap, but we can directly use the input for parsing
        //\n represent a line, \t represent the level
        stringstream ss(input);
        int level=0;
        vector<int> vlen;
        string name;
        int ans=0;
        char sbuf[256];
        while(ss.getline(sbuf,256,'\n')){ //it will skip \n and \t
            name=string(sbuf);
            level=0;
            while(name[level]=='\t') level++;
            if(name.find(".")!=string::npos){ //this is a file
                while(vlen.size()>level) vlen.pop_back();
                int tlen=0;
                for(int t: vlen) tlen+=t;
                ans=max(ans,tlen+(int)name.size());
            }
            else {//a directory name
                while(vlen.size()>level) vlen.pop_back();
                vlen.push_back(name.size()-level);
            }
        }
        return ans;
    }
```	
note: getline need use char* with delimiter.

## contest 2
### 389. find the difference (**)
find the extra char, using hashmap

### 390. elimination game (****)
dir change, step change
do not have to do the actual elimination.
```cpp
    int lastRemaining(int n) {
        //
        int head=1,dir=1,rem=n,step=1;
        while(rem>1){
            if(dir || rem%2) head+=step;
            dir^=1;
            rem/=2;
            step*=2;
        }
        return head;
    }
```

### 391. perfect rectangle (****)
the outside 4 points shall appear only one time, inside shall appear even times
total area shall equal to sum of all rectangles
```cpp
    bool isRectangleCover(vector<vector<int>>& rectangles) {
        //get the 4 points outside and the total area
        //inner side points shall mod 2, outside point shall =1
        int total=0;
        int x0=INT_MAX,y0=INT_MAX,x1=INT_MIN,y1=INT_MIN;
        unordered_map<string,int> mp;
        for(auto r: rectangles){
            x0=min(x0,r[0]);
            y0=min(y0,r[1]);
            x1=max(x1,r[2]);
            y1=max(y1,r[3]);
            mp[forms(r[0],r[1])]++;
            mp[forms(r[0],r[3])]++;
            mp[forms(r[2],r[1])]++;
            mp[forms(r[2],r[3])]++;
            total+=(r[0]-r[2])*(r[1]-r[3]);
        }
        if(total!=(x0-x1)*(y0-y1)) return 0;
        if(mp[forms(x0,y0)]!=1 || mp[forms(x0,y1)]!=1 ||
          mp[forms(x1,y0)]!=1 || mp[forms(x1,y1)]!=1) return 0;
        //all other shall be even
        mp.erase(forms(x0,y0));
        mp.erase(forms(x0,y1));
        mp.erase(forms(x1,y0));
        mp.erase(forms(x1,y1));
        for(auto t: mp) if(t.second%2) return 0;
        return 1;
    }
    string forms(int x,int y){
        return to_string(x)+","+to_string(y);
    }
```

## contest 3
### 392. Is subsequence (***)
greedy match using two pointer. When all chars in s are matched, T can be non-completed.	
```cpp
    bool isSubsequence(string s, string t) {
        //greedy using two pointer
        int i=0,j=0;
        while(i<s.size() && j<t.size()){
            while(j<t.size() && t[j]!=s[i]) j++;
            i++;j++;
        }
        return i==s.size() && j<=t.size();
    }
```
### 393. utf8 validation (***)
idea: find the first byte signature and then check remaining contains 10 in binary
```cpp
    bool validUtf8(vector<int>& data) {
        int i=0,rem=0;
        while(i<data.size()){
            if(rem==0){
                if(data[i]<0x80) rem=0;
                else if((data[i]&0xe0)==0xc0) rem=1;
                else if((data[i]&0xf0)==0xe0) rem=2;
                else if((data[i]&0xf8)==0xf0) rem=3;
                else return 0;
            }
            else{
                if((data[i]&0xc0)!=0x80) return 0;
                rem--;
            }
            i++;
        }
        return rem==0;
    }
```

### 394. decode string (****)
recursive stack: common practice, add decoded results into stack and recusively solve subproblem and update the one in the stack top.
```cpp
    string decodeString(string s) {
        //using stack to do recursive
        int ncopy=0;
        stack<string> st;
        string t;
        int i=0;
        //cout<<s<<": "<<endl;
        while(i<s.size()){
            char c=s[i];
            if(isdigit(c)){
                if(t.size()) st.push(t);
                t.clear();
                ncopy=c-'0';
                while(i+1<s.size() && isdigit(s[i+1])){
                    ncopy=ncopy*10+(s[++i]-'0');
                }
                i++;
            }
            else if(isalpha(c)){
                t=c;
                while(i+1<s.size() && isalpha(s[i+1])){
                    t+=s[++i];
                }
                i++;
            }
            else if(c=='['){
                int j=i+1,p=1;
                while(p){
                    if(s[j]=='[') p++;
                    if(s[j]==']') p--;
                    j++;
                }
                
                j--;
                //from i+1 to j-1 (inclusive)
                string t=decodeString(s.substr(i+1,j-i-1));
                //repeat:
                string nt;
                for(int k=0;k<ncopy;k++) nt+=t;
                if(st.size()) st.top()+=nt;
                else st.push(nt);
                i=j+1;
                t.clear();
            }
        }
        string ans=t;
        while(st.size()){
            ans=st.top()+ans;
            st.pop();
        }
        //cout<<ans<<endl;
        return ans;
    }
```
### 395. Longest Substring with At Least K Repeating Characters (*****)
divide and conquer
idea: using the char appear less than k times as a separator

```cpp
    int longestSubstring(string s, int k) {
        //divide and conquer
        //find the char less then k times and divide into two parts
        return helper(s,k,0,s.size());
    }
    int helper(string& s,int k,int start,int end){
        //cout<<s.substr(start,end-start)<<endl;
        if(end-start<k) return 0;
        vector<int> cnt(26);
        
        for(int i=start;i<end;i++) 
            cnt[s[i]-'a']++;
        int ans=0,l=0;
        bool found=0;
        for(int i=start;i<end;i++){
            int t=cnt[s[i]-'a'];
            if(t && t<k){
                ans=max({ans,helper(s,k,l,i)});
                l=i+1;
                found=1;
            }
        }
        if(found) ans=max(ans,helper(s,k,l,end));
        else ans=end-start;
        return ans;
    }
```

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
## contest 5
### 400. Nth digit (****)
idea: binary search the number in which area, and then find the number, finally get the digit in the number
```cpp
    int findNthDigit(int n) {
        if(n<10) return n;
        vector<long> num(10,1);//1,9,2*90+9
        num[1]=9;
        for(int i=2;i<10;i++){
            num[i]=9*i*pow(10,i-1)+num[i-1];
            //cout<<num[i]<<" ";
        }
        int ind=upper_bound(num.begin(),num.end(),n)-num.begin(); //>=n
        n-=num[ind-1];
        int t=n/ind-1+pow(10,ind-1); //starting from 10...0, number of digits is ind.
        //cout<<t;
        int rem=n%ind;
        if(rem) t++;else rem=ind; //rem=0, the last digit
        //cout<<t<<" "<<rem;
        string s=to_string(t);
        return s[rem-1]-'0';
    }
```

### 401. Binary Watch (****)
idea: 10 bit all combinations and then check all valid combinations. We can also do the validation during backtracking to save space.
```cpp
    vector<string> readBinaryWatch(int num) {
        //hr: 4bits, min: 6bits (hr max 3 lights, min max 5 lights)
        //use it as a whole is easier 
        //backtrack
        vector<string> ans;
        if(num>8) return ans;
        vector<bitset<10>> vbits;
        bitset<10> t(0);
        backtrack(num,t,0,vbits);
        for(auto t: vbits){
            int n=t.to_ulong();
            int hr=(n&0x3c0)>>6;
            int mi=n&0x3f;
            if(hr>11 || mi>59) continue;
            char str[6];
            sprintf(str,"%d:%02d",hr,mi);
            ans.push_back(string(str));
        }
        return ans;
    }
    void backtrack(int n,bitset<10> t,int start,vector<bitset<10>>& vbit){
        if(n==0){
            vbit.push_back(t);
            return;
        }
        for(int i=start;i<10;i++){
            t[i]=1;
            backtrack(n-1,t,i+1,vbit);
            t[i]=0;
        }
    }
```
### 402. Remove K Digits	(*****)
using stack to remove the peak digits recursively

```cpp
    string removeKdigits(string num, int k) {
        //greedy:
        //1432219: remove 1 digit ->132219
        //remove 2 digit: 12219
        //remove 3 digit: 1219. approach: from left to right, repeatedly remove the first peak digit
        //using stack: if the curr digit is less than previous, then remove the stack top
        
        stack<char> st;
        for(char c: num){
            while(st.size() && k && c<st.top()){
                st.pop();
                k--;
            }
            st.push(c);
            //if(k==0) break;
        }
        while(k--){ //still need to remove, now it is sorted
            st.pop();
        }
        string ans;
        while(st.size()) {
            ans+=st.top();
            st.pop();
        }
        //remove leading zeros
        while(ans.back()=='0') ans.pop_back();
        if(ans.empty()) ans="0";
        reverse(ans.begin(),ans.end());
        return ans;
    }
```

### 403. Frog Jump	(****)
bfs with 3 options: k-1, k, k+1
important optimization: if keep k+1, then A[i] and A[i+1] can only have distance less than i.
can also use dfs (recursive approach)

```cpp
    bool canCross(vector<int>& stones) {
        //assuming at ith stone and previous step is k, then next must have a stone at k+1, k or k-1 distance
        //can use bfs
        unordered_set<int> st;
        for(int i=1;i<stones.size();i++){
            if(stones[i]>stones[i-1]+i) return 0;
            st.insert(stones[i]);
        }
        unordered_set<string> v;
        queue<vector<int>> q; //stores position vs step
        int n=stones.size();
        q.push({0,0});
        while(q.size()) {
            int sz=q.size();
            while(sz--){
                auto p=q.front();
                q.pop();
                int pos=p[0],step=p[1];
                if(pos==stones.back()) return 1;
                int ind=0;
                string s;
                s=to_string(pos+step-1)+","+to_string(step-1);
                if(step>1 && st.count(pos+step-1) && !v.count(s)) {q.push({pos+step-1,step-1});v.insert(s);}
                s=to_string(pos+step)+","+to_string(step);
                if(step && st.count(pos+step)&& !v.count(s)) {q.push({pos+step,step});v.insert(s);}
                s=to_string(pos+step+1)+","+to_string(step+1);
                if(st.count(pos+step+1)&& !v.count(s)) {q.push({pos+step+1,step+1});v.insert(s);}
            }
        }
        return 0;
    }
```

## contest 6
### 404. sum of left leaves (**)
traversal
### 405. convert a number to hex (**)
with negatives
### 406. queue reconstruction (**)
greedy, arrange the height according to height first in descendant and index ascending order
corner case: input is empty

### 407. Trapping rain water II (*****)
BFS with priority-queue. 
need to observe the following facts:
- inside is bound by outside ring. the amount of water is determined by the lowest bar in outside
- taller bar can also form a enclosed area with water. (not in the same level)
- approach: put the outside cells into min heap (pq)
- start from the min and add its neighbors into pq
- if cell is lower than maxh, add water.
- if cell is higher than maxh, update maxh.
queue store the h and x y so that we can extend to neighbors
do not forget when we add elements to queue, mark them as visited.

```cpp
    int trapRainWater(vector<vector<int>>& hm) {
        if(hm.empty()) return 0;
        int m=hm.size(),n=hm[0].size();
        priority_queue<vector<int>,vector<vector<int>>,greater<vector<int>>> pq;
        //add the outside ring into pq
        vector<vector<bool>> v(m,vector<bool>(n));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i==0 || i==m-1 || j==0 || j==n-1) {
                    pq.push({hm[i][j],i,j});
                    v[i][j]=1;
                }
            }
        }
        int ans=0;
        int dir[][2]={{-1,0},{1,0},{0,-1},{0,1}};
        
        int maxh=0;
        while(pq.size()){
            auto t=pq.top();
            pq.pop();
            int h=t[0],x0=t[1],y0=t[2];
            maxh=max(h,maxh);
            for(auto d: dir){
                int x=x0+d[0],y=y0+d[1];
                if(x<0||x>=m||y<0||y>=n||v[x][y]) continue;
                v[x][y]=1;
                int h=hm[x][y];
                ans+=max(0,maxh-h);
                pq.push({h,x,y});
            }
        }
        return ans;
    }
```
就是四面都是围墙，从最低的往里走；
如果里面有更低的，当然就可以蓄水，蓄水的量就是围墙最低 减去 此处的高度；
如果里面的比当前围墙高，那这个方向的围墙高度就增加了。
然后永远围墙最低的地方开始搜，最后就能把整个水池搜一遍。

## contest 7

### 408. valid word abbreviation (***)
two pointer approach
when we find a digit, we get the whole number and advance the other pointer by this number
and then match
at the end, both string shall be exhausted.
```cpp
    bool validWordAbbreviation(string word, string abbr) {
        int i=0,j=0;
        int num=0;
        while(i<word.size()&&j<abbr.size()){
            if(isdigit(abbr[j])){
                if(abbr[j]=='0') return 0; //leading zero not allowed
                num=0;
                while(j<abbr.size() && isdigit(abbr[j])) {num=num*10+abbr[j]-'0';j++;}
                i+=num;
            }
            else {
                if(word[i]!=abbr[j]) return 0;
                i++,j++;
            }
        }
        return i==word.size() && j==abbr.size();
    }
```
corner case: number of 0 or leading 0

### 409. longest palindrome (**)
greedy: palindrome will have 0 or 1 char with odd occurrence.

### 410. split array largest sum (****)
split the array into m subarray and minimize the maxsum of the subarrays
convert to binary search problem: given target max sum and to see the number of parts
```cpp
    int splitArray(vector<int>& nums, int m) {
        //binary search
        long tsum=accumulate(nums.begin(),nums.end(),0ll);
        long max0=*max_element(nums.begin(),nums.end());
        long l=max0,r=tsum;
        while(l<r){
            long mid=l+(r-l)/2;
            int num=split(nums,mid);
            if(num<=m) r=mid;
            else l=mid+1;
        }
        return l;
    }
    int split(vector<int>& nums,long target){
        int ans=0;
        long sum=0;
        for(int t: nums){
            if(t+sum>target){
                ans++;
                sum=0;
            }
            sum+=t;
        }
        ans+=(sum!=0);
        cout<<ans<<endl;
        return ans;
    }
```	

### 411. min unique word abbreviation (*****)
similar see: 408. validate abbreviation
320. generate all abbreviation

given a list of dictionary words and a word, find the min length abbreviation which is not conflict with abbr forms of the dictionary words
approach:
to not conflict with dictionary words, we need find a char which is different from the dict chars
bitset approach:
- generate the bits using each dict word (set bit if the char is different)
- we may need to keep 1 or more bits in the candidate
trie approach:
- build the trie using dictionary word's abbreviation form (may need backtracking to generate all abbrs)
- build the word's abbreviation form and check the first one not in the trie. (can use binary search)
- backtracking: keep 1,2,...n chars 

see the reference: https://leetcode.com/problems/minimum-unique-word-abbreviation/discuss/89920/6-ms-C%2B%2B-solution-based-on-modified-320's-generateAbbreviations-(DFS-backtracking)-and-408's-validWordAbbreviation
By applying following optimizations to reduce computations.

filter words in dictionary with length different than target's;
when generating abbreviations of target, stops when "length" of abbreviation is going to exceed the length of current best abbreviation;
when generating abbreviations of target, starts with larger number first, then small number, then letter.

```cpp
class Solution {
public:
    bool validWordAbbreviation(string &word, string &abbr) {
        int i = 0, j = 0, m = word.length(), n = abbr.length();
        while (i < m && j < n) {
            if (word[i] == abbr[j])
                ++i, ++j;
            else if (abbr[j] == '0')
                return false;
            else if (isdigit(abbr[j])) {
                int len = 0;
                while (j < n && isdigit(abbr[j]))
                    len = len * 10 + abbr[j++] - '0';
                i += len;
            }
            else
                return false;
        }
        return i == m && j == n;
    }
    
    void helper(vector<string> &dict, string &ans, int &ansLen, string &s, int len, string &word, int i) {
        if (i == word.length()) {
            if (len < ansLen) {
                int valid = false;
                for (string w : dict) {
                    if (validWordAbbreviation(w, s)) {
                        valid = true;
                        break;
                    }
                }
                
                if (!valid) {
                    ansLen = len;
                    ans = s;
                }
            }
            return;
        }
        
        if (len == ansLen)
            return;
        
        if (s.empty() || !isdigit(s.back())) {
            for (int j = word.length() - 1; j >= i; --j) {
                int pos = s.length();
                s += to_string(j - i + 1);
                ++len;
                helper(dict, ans, ansLen, s, len, word, j + 1);
                --len;
                s.erase(pos);
            }
        }
        
        s.push_back(word[i]);
        ++len;
        helper(dict, ans, ansLen, s, len, word, i + 1);
        --len;
        s.pop_back();
    }

    string minAbbreviation(string target, vector<string>& dictionary) {
        if (target.empty())
            return "";
        
        vector<string> dict;
        for (string word : dictionary) {
            if (word.length() == target.length())
                dict.push_back(word);
        }
        
        string s, ans = target;
        int ansLen = target.length();
        helper(dict, ans, ansLen, s, 0, target, 0);
        return ans;
    }
};
```
	
## contest 8
### 415. add strings. (**)
simple with cf.

### 416. partition equal subset sum (****)
dp knapsack to a target sum tsum/2
knapsack key points:
current item is not chosen
current item is chosen
subproblem: array length l and target w.
```cpp
    bool canPartition(vector<int>& num) {
      int tsum=accumulate(num.begin(),num.end(),0);
      if(tsum%2) return 0;
      int target=tsum/2;
      int n=num.size();
      vector<vector<bool>> dp(target+1,vector<bool>(n+1)); //
      //boundary condtion
      for(int i=0;i<=n;i++) dp[0][i]=1;//for w=0, choose none.
      for(int i=1;i<=n;i++){ //loop on the array
        for(int t=0;t<=target;t++){
          dp[t][i]=dp[t][i-1];
          if(t>=num[i-1]) 
			dp[t][i]=dp[t-num[i-1]][i-1] || dp[t][i];
        }
      }
      return dp[target][n];
    }
```	
we can optimize the space complexity.
```cpp
    bool canPartition(vector<int>& num) {
      int tsum=accumulate(num.begin(),num.end(),0);
      if(tsum%2) return 0;
      int target=tsum/2;
      int n=num.size();
      vector<bool> dp(target+1); //
      dp[0]=1;//boundary condtion
      for(int c: num){ //loop on the array
        for(int w=target;w>0;w--){
          if(w>=c) 
			dp[w]=dp[w-c] || dp[w]; //we need previous time smaller w so we need reverse loop
        }
      }
      return dp[target];
    }
```	

### 417. pacific atlantic water flow (****)
dfs from top left using type 1, dfs from bottom right using type 2
```cpp
    typedef vector<vector<int>> vvi;
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& matrix) {
        vvi ans;
        if(matrix.empty()) return ans;
        int m=matrix.size(),n=matrix[0].size();
        vvi v(m,vector<int>(n));
        
        for(int i=0;i<m;i++){
            dfs(matrix,i,0,INT_MIN,1,v,ans); //left
            dfs(matrix,i,n-1,INT_MIN,2,v,ans); //right
        }
        for(int j=0;j<n;j++){
            dfs(matrix,0,j,INT_MIN,1,v,ans); //top
            dfs(matrix,m-1,j,INT_MIN,2,v,ans); //bottom
        }
        return ans;
    }
    void dfs(vvi& mat,int i, int j,int pre,int type,vvi& v,vvi& ans){
        if(i<0||j<0||i>=mat.size()||j>=mat[0].size()||(v[i][j]&type)||mat[i][j]<pre) return;
        v[i][j]|=type;
        if(v[i][j]==3) ans.push_back({i,j});
        pre=mat[i][j];
        dfs(mat,i-1,j,pre,type,v,ans);
        dfs(mat,i+1,j,pre,type,v,ans);
        dfs(mat,i,j-1,pre,type,v,ans);
        dfs(mat,i,j+1,pre,type,v,ans);
    }
```	

### 418. sentence screen fitting (****)
this is a very interesting problem but with a very smart approach.
first assemble the words into a sentence with space separator. (ending also with a space)
then use a pointer to indicate the length or char we are pointing (circular)
first we add a row to the pointer
and check if it is space, then next line do not need space, pointer++
otherwise, we shall add space to shift the current word to next line
last row do not need space if necessary, but it is already covered by pointer++

```cpp
    int wordsTyping(vector<string>& sentence, int rows, int cols) {
        string s;
        for(string w: sentence) s+=w+" ";
        int len=s.size();
        int start=0;
        for(int i=0;i<rows;i++){ //try each row
            start+=cols; //first advance the pointer by cols
            //if previous row ending word ends with no space, then start++
            //if previous row ending word ends with more than one space, then start--
            if(s[start%len]==' ') start++;
            else { //next char is not a space, it is a char
                while(start && s[(start-1)%len]!=' ') start--;
            }
        }
        return start/len;        
    }
```	
