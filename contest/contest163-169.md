# leetcode contest
Participant performance:
- 169: 242/5933 got 4/4 4.08%
- bi16: 454/2788 got 4/4 16.2%
- 168: 807/5525 got 4/4 14.6%
- 167: 557/5475 got 4/4 10.0%
- bi15: 661/2525 got 4/4 26.2%
- 166: 1036/5600 got 4/4 18.5%
- 165: 615/5500 got 4/4 11.2%
- bi14: 323/2575 got 4/4 12.5%
- 164: 933/5925 got 4/4 15.7%
- 163: 199/5875 got 4/4 3.38%

## contest 169.
### 1304. find n unique integers sum up to 0 (rate: *)
greedy, pretty simple
if odd, add 0
then add +/-1, +/-2... pairs

### 1305. All elements in two binary search tree (rate: **)
inorder traversal each tree and store in array and then merge.

### 1306. Jump game III (rate: ***)
regular bfs to store the position and the jump step.
using hashset for visited flags

dfs is also OK:
```cpp
unordered_set<int> vis;
bool canReach(vector<int>& arr, int st) {
    if (st >= 0 && st < arr.size() && vis.insert(st).second) {
        return arr[st] == 0 ||
            canReach(arr, st + arr[st]) || canReach(arr, st - arr[st]);
    }
    return false;
}
```
note: unordered_set insert will return a pair:
first is the iterator, second is if it is inserted successfully.

### 1307. verbal arithmetic puzzle (rate: *****)
typical backtracking problem with extra complexity</br>
first approach: O(N!) complexity</br>
we collect all unique letters and repeat 0 to 9 with the help of hashmap.</br>
however this will get TLE.</br>
A common mistake I often made is: loop over the letters and loop over the digits</br>
the recursive backtracking itself is a loop over the letters. So do not loop over it again.</br>

```cpp
    unordered_set<char> leading,chars;
    unordered_map<char,int> mp;
    bool used[10];
    bool isSolvable(vector<string>& words, string result) {
        //backtracking
        for(string w: words){
            leading.insert(w[0]);
            for(char c: w) chars.insert(c);
        }
        leading.insert(result[0]);
        for(char c: result) chars.insert(c);
        vector<char> arr(chars.begin(),chars.end());
        return backtrack(arr,0,words,result);
    }
    bool backtrack(vector<char>& arr,int start,vector<string>& words,string& result){
        if(start==arr.size()){
            if(valid(words,result)) return 1;
            else return 0;
        }
        char c=arr[start];
        for(int i=0;i<10;i++){
            if(used[i] || (i==0 && leading.count(c))) continue;
            used[i]=1;
            mp[c]=i;
            if(backtrack(arr,start+1,words,result)) return 1;
            used[i]=0;
            mp.erase(c);
        }
        return 0;
    }
    bool valid(vector<string>& words,string& result){
        int sum=0;
        for(string w: words){
            int num=0;
            for(char c: w) num=num*10+mp[c];
            sum+=num;
        }
        int res=0;
        for(char c: result) res=res*10+mp[c];
        return res==sum;
    }
```	
-note above code will give wrong answer for some case since it did not check leading digit==0

it is easy to understand, however it tries a lot of unnecessary combinations.</br>
for example when we choose the definition for the LSB of each words, the result bit is determined.</br>
but this approach still tries all the combination.</br>
More efficient way is to try col by col from the LSB.</br>
This approach is more complicated since it is a two-backtracking problem:</br>
- backtracking on a single column with cf.
- backtracking on columns (when current col fails, we need backtrack to previous col)
It helps greatly if we define it a 2d backtracking problem with row and col.
- reverse words and result so we can do from LSB to MSB
- col reaches result size, whole process completes, check if sum==0 (no carrier flag)
- row reaches words size, current col is done
  * if result char in this col is not mapped, then another backtracking:
    map it
	try next col
	restore if it fails
  * if result char in this col is mapped, check if matches
     matched: do next col
	 not match, return 0
- skip current row if word does not have it
- add current row and go to next row if result is mapped
- otherwise try from 0 to 9 (typical one column backtracking problem)
```cpp
    char i2c[10];
    int c2i[26];    
    bool isSolvable(vector<string>& words, string result) {
        for (auto &s : words) if (s.size() > result.size()) return false;
        memset(c2i, -1, sizeof c2i);
        memset(i2c, '\0', sizeof i2c);
        for (auto& s: words) reverse(s.begin(), s.end());
        reverse(result.begin(), result.end());
        return backtrack(words, result, 0, 0, 0);
    }    
    bool backtrack(vector<string>& words, string& result, int row, int col, int sum) 
    {
        if (col == result.size()) return sum == 0;//finished all cols
        
        if (row == words.size()) //finished one col
        {
            int d=sum%10;
            int cf=sum/10;
            int t=result[col] - 'A';
            if (c2i[t] != -1) //used
            {
                if (d == c2i[t])
                    return backtrack(words, result, 0, col+1, cf);
            }
            else if (i2c[d] == '\0') //not used, another backtrack
            {
                c2i[t] = d;
                i2c[d] = result[col];
                if (backtrack(words, result, 0, col+1, cf)) return true;
                c2i[t] = -1;
                i2c[d] = '\0';
            }
            return false;
        }
        
        if (col >= words[row].size()) //skip if current word has no char in the col
            return backtrack(words, result, row+1, col, sum);
        int t=words[row][col] - 'A';
        if (c2i[t] != -1) //if current char is mapped
            return backtrack(words, result, row+1, col, sum + c2i[t]);
        char c=words[row][col];
        for (int i = 0; i < 10; ++i) //not mapped
        {
            if (i2c[i] != '\0') continue;
            if (i == 0 && col == words[row].size() - 1 && words[row].size() > 1) continue;
            i2c[i] = c;
            c2i[t] = i;
            if (backtrack(words, result, row+1, col, sum + i))
                return true;
            i2c[i] = '\0';
            c2i[t] = -1;
        }
        return false;
    }
```
This solution is at 100 times faster than the O(n!) solution.
what would be the complexity then?
- first the result char is mapped directly so we basically eliminate those chars from the loop selection
- using hashmap to store c2i will make the time from 8ms changed to 100ms, using array is much more efficient.
	
## biweek contest 16
### 1299. Replace elemets with greatest element on right side (**)
simple, just do it from right to left and find the max and add to answer

### 1300. sum of mutated array closest to target (****)
first we can sort the array to make calculate the sum more efficient. (but not necessary)
second, it is a binary search problem.
we are actually looking for a value between 0 and max_element so that the sum==target.
when sum>=target, r=mid, else l=mid+1
This is pretty similar to find target in a sorted array using binary search. (the target could or not in the array)
(the abs(sum-target) is a v-shape curve, without abs, it is monotonic shape).

```cpp
    int findBestValue(vector<int>& arr, int target) {
        int l=0,r=*max_element(arr.begin(),arr.end());
        int sum=accumulate(arr.begin(),arr.end(),0);
        if(sum<=target) return r; //cannot change 
        //find a value for sum==target
        while(l<r){
            int m=l+(r-l)/2;
            sum=0;
            for(int t: arr) sum+=t<m?t:m;
            if(sum>=target) r=m;
            else l=m+1;
        }
        //now we find l, check if it shall be l-1 or l
        //note binary search l shall be the one for sum>=target
        //note the sum is not for l
        int s1=0,s2=0;
        for(int t: arr) {
            s1+=t<l-1?t:l-1;
            s2+=t<l?t:l;
        }
        //cout<<sum<<endl;
        return abs(s2-target)<abs(s1-target)?l:l-1;
    }
```
	
### 1302. Deepest leaves sum. (***)
It is simple to use two pass traversal. first pass to get the max depth, second pass to get the sum.
```cpp
    int deepestLeavesSum(TreeNode* root) {
        int d=depth(root);
        //cout<<d;
        int ans=0;
        dfs(root,1,d,ans);
        return ans;
    }
    
    void dfs(TreeNode* root,int d,int md,int& ans){
        if(!root) return;
        if(d==md) ans+=root->val;
        dfs(root->left,d+1,md,ans);
        dfs(root->right,d+1,md,ans);
    }
    
    int depth(TreeNode* root){
        if(!root) return 0;
        return 1+max(depth(root->left),depth(root->right));
    }
```
one pass solution:
we need find the max depth and do the sum in one pass, a little bit tricky though
when current depth>max depth, we need update sum and depth
when current depth==max_depth, we need add the node to the sum.

```cpp
    int dh,sum;
    int deepestLeavesSum(TreeNode* root) {
        dh = sum = 0;
        dfs(root, 0);
        return sum;
    }
    void dfs(TreeNode* root, int d) {
        if(!root) return;
        if(d > dh) {
	    sum = root -> val;
            dh=d;
	}
	else if(d == dh) {
	    sum += root -> val;
	}
        dfs(root -> left, d + 1);
        dfs(root -> right, d + 1);
    }
```	
other approach: level order traversal using bfs

### 1301. number of paths with max score (***)
two dp problems:
one is max score from top left to bottom right
second is number of max sum paths (note it asks number of path with the max score)

some observations:
- it is easy to see that from bottom right to top left is equivalent to from top left to bottom right
- we can replace the top left and bottom right cell with 0 score cell to simplify
- dp0[i,j] represent the max score for [i-1,j-1]
- dp1[i,j] represent the number of max sum path for [i-1,j-1]
- score:
  *. current cell must be a number
  *. shall have one or more paths from left, top, topleft cell
- base condition: we can set dp1[0,0] or dp[0,1] or dp[1,0] to 1, which means we only have one path entering to board[0,0]
```cpp
    vector<int> pathsWithMaxScore(vector<string>& board) {
        //dp
        vector<int> ans={0,0};
        int mod=1e9+7;
        if(board.empty()) return ans;
        int m=board.size(),n=board[0].size();
        vector<vector<int>> dp0(m+1,vector<int>(n+1)),dp1(m+1,vector<int>(n+1)); //dp0: max, dp1 number of path for max
        dp1[0][0]=1; //diagonal can go to E
        board[0][0]=board[m-1][n-1]='0';
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(board[i][j]!='X'){
                    dp0[i+1][j+1]=max({dp0[i+1][j],dp0[i][j+1],dp0[i][j]});//+board[i][j]-'0';
                    if(dp0[i+1][j+1]==dp0[i+1][j]) dp1[i+1][j+1]+=dp1[i+1][j];
                    if(dp0[i+1][j+1]==dp0[i][j+1]) dp1[i+1][j+1]+=dp1[i][j+1];
                    if(dp0[i+1][j+1]==dp0[i][j]) dp1[i+1][j+1]+=dp1[i][j];
                    dp1[i+1][j+1]%=mod;
                    if(dp1[i+1][j+1]) dp0[i+1][j+1]+=board[i][j]-'0';//only when there is path we can add
                }
            }
        }
        return {dp0[m][n],dp1[m][n]};
    }
```	
 
## contest 168
### 1295. find numbers with even number of digits (*)
simple, convert each number to string

### 1296. divide array in sets of k consecutive numbers (***)
greedy: sort the array first, and start from the smallest and reduce the counting using hashmap
do not delete elements from hashmap since iterating will cause problem
```cpp
    bool isPossibleDivide(vector<int>& nums, int k) {
        map<int,int> mp;
        for(int t: nums) mp[t]++;
        auto it0=mp.begin();
        while(it0!=mp.end()){
            auto it1=it0;
            int v=it0->second;
            int n=0;
            
            while(n<k){
                if(it1->first==it0->first+n && it1->second>=v)
                    it1->second-=v;
                else return 0;
                n++;it1++;
            }
            bool found=0;
            for(it1=it0;it1!=mp.end();it1++) {
                if(it1->second){
                    found=1;
                    it0=it1;
                    break;
                }
            }
            if(!found) it0=mp.end();
        }
        return 1;
    }
```
### 1297. Maximum Number of Occurrences of a Substring	(***)
an important observation: larger length substr will be covered by smaller length substr.
two pointer with hashmap
```cpp
    int maxFreq(string s, int maxLetters, int minSize, int maxSize) {
        //greedy, the minsize window works also with maxsize
        int ans=0;
        int i=0,j=0;
        vector<int> mp(26);
        unordered_map<string,int> tmp;
        while(j<s.size()){
            mp[s[j]-'a']++;
            if(j-i+1==minSize){
                int count=0;
                for(int k=0;k<26;k++)
                    count+=(mp[k]>0);
                if(count<=maxLetters) 
                    ans=max(ans,++tmp[s.substr(i,minSize)]);
                mp[s[i]-'a']--;i++;
            }
            j++;
        }
        return ans;
    }
```
Note: when length equals to window size, we first check if it satisfies the conditions, then we advance the pointer i.

### 1298. Maximum Candies You Can Get from Boxes (***)
again it is a regular bfs problem.
we need to push all the found but un-opened boxes into queue, until we do not open any new boxes
```cpp
    int maxCandies(vector<int>& status, vector<int>& candies, vector<vector<int>>& keys, vector<vector<int>>& containedBoxes, vector<int>& initialBoxes) {
        //bfs
        queue<int> q;
        for(int t: initialBoxes) q.push(t);
        unordered_set<int> keys_owned;
        int ans=0;
        while(q.size()){
            int sz=q.size();
            bool found=0;
            while(sz--){
                int b=q.front();
                q.pop();
                if(status[b] || keys_owned.count(b)){
                    ans+=candies[b];
                    for(int k: keys[b]) keys_owned.insert(k);
                    for(int k: containedBoxes[b]) q.push(k);
                    found=1;
                }
                else q.push(b);
            }
            if(!found) break;
        }
        return ans;
    }
```	

## contest 167
### 1290. 1290. Convert Binary Number in a Linked List to Integer (*)
simple

### 1291. Sequential Digits (***)
simple backtracking
```cpp
    vector<int> sequentialDigits(int low, int high) {
        //backtrack
        string l=to_string(low),h=to_string(high);
        int m=l.size(),n=h.size();
        vector<int> ans;
        backtrack(ans,low,high,m,n,0,1);
        sort(ans.begin(),ans.end());
        return ans;
    }
    void backtrack(vector<int>& ans,int low,int high,int m,int n,int t,int ld){
        if(t>high) return;
        if(t>=low && t<=high){
            ans.push_back(t);
        }
        //first digit is 1
        for(int i=ld;i<=9;i++){ //try 
            int d=t%10;
            if(i==d+1 || d==0){
                t=t*10+i;
                backtrack(ans,low,high,m,n,t,d+1);
                t/=10;
            }
        }
    }
```

### 1292. Maximum Side Length of a Square with Sum Less than or Equal to Threshold	(****)
prefix sum for 2d using dp.
note: we need add 0 to 1d prefix, and one extra col, one extra row to 2d (I didn't fix the bug in the contest)
```cpp
    int maxSideLength(vector<vector<int>>& mat, int threshold) {
        //dp: get the rect sum 
        int m=mat.size(),n=mat[0].size();
        int ans=0;
        vector<vector<int>> dp(m+1,vector<int>(n+1)); //max side length
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                int side=0;
                dp[i][j]=mat[i-1][j-1]+dp[i][j-1]+dp[i-1][j]-dp[i-1][j-1];
                //check the lenght from 1 to i
                if(min(i,j)<ans) continue;
                for(int k=min(i,j);k>=0;k--){
                    int tsum=dp[i][j]-dp[i-k][j]-dp[i][j-k]+dp[i-k][j-k];
                    if(tsum<=threshold) {side=k;break;}
                }
                ans=max(ans,side);
            }
            
        }
        return ans;
    }
```	

### 1293. Shortest Path in a Grid with Obstacles Elimination (****)
regular bfs
one important observation: when number of obstacle > m-1+n-1, then we get the shortest length using dx+dy
this can be used in each step.
```cpp
    int shortestPath(vector<vector<int>>& grid, int k) {
        int m=grid.size(),n=grid[0].size();
        if(k>m+n-2) return m+n-2;
        queue<vector<int>> q;
        vector<bool> v(m*n*(k+1));
        int step=0;
        q.push({0,0,0});
        v[0]=1;
        int dir[][2]={{-1,0},{1,0},{0,-1},{0,1}};
        while(q.size()){
            int sz=q.size();
            while(sz--){
                auto t=q.front();
                q.pop();
                int x=t[0],y=t[1],cnt=t[2];
                if(x==m-1 && y==n-1 && cnt<=k) return step;
                //optimization: remaining steps from [x,y] to bott-right with k steps
                if(k-cnt>m-1-x+n-1-y) return step+(m-1-x+n-1-y);
                for(int d=0;d<4;d++){
                    int x0=x+dir[d][0],y0=y+dir[d][1];
                    if(x0<0||y0<0||x0>=m||y0>=n||
                       cnt+grid[x0][y0]>k||v[x0*n*(k+1)+y0*(k+1)+cnt+grid[x0][y0]])
                        continue;
                    int c=cnt+grid[x0][y0];
                    q.push({x0,y0,c}); //eliminate current obstacle
                    v[x0*n*(k+1)+y0*(k+1)+c]=1;
                }
            }
            step++;
            //cout<<step<<endl;
        }
        return -1;
    }
```
Important: we need use k+1 for the 3rd dimension since it allows 0 to k.

## biweek contest 15
### 1287. Element Appearing More Than 25% In Sorted Array (***)
in a sorted array, the >25% element will appear at 1/4, 2/4, 3/4 position
```cpp
    int findSpecialInteger(vector<int>& arr) {
        //element appear at 1/4,2/4,3/4
        int n=arr.size();
        int bound=(n+3)/4;
        if(num_elem(arr,arr[n/4])>bound) return arr[n/4] ;
        if(num_elem(arr,arr[n/2])>bound) return arr[n/2] ;
        return arr[n*3/4];
    }
    int num_elem(vector<int>& arr,int num){
        auto t=equal_range(arr.begin(),arr.end(),num);
        return distance(t.first,t.second);
    }
```
### 1288. Remove Covered Intervals (***)
sort the intervals according to the start. then we using the right max to remove covered intervals	
```cpp
    int removeCoveredIntervals(vector<vector<int>>& intervals) {
        //sorting with beginning
        int n=intervals.size();
        sort(intervals.begin(),intervals.end());
        int ans=0;
        int cur_end=intervals[0][1];
        for(int i=1;i<n;i++){
            if(intervals[i][1]>cur_end)
                cur_end=intervals[i][1];
            else ans++;
        }
        return n-ans;
    }
```
### 1286. Iterator for Combination	(**)
using backtracking to get all the combinations
```cpp
    string s;
    int cl;
    vector<string> vs;
    int ind;
    CombinationIterator(string characters, int combinationLength) {
        s=characters;
        cl=combinationLength;
        backtrack(s,0,"",vs);
        ind=0;
    }
    
    void backtrack(string& s,int start,string t,vector<string>& vs){
        if(t.length()==cl){
            vs.push_back(t);
            return;
        }
        for(int i=start;i<s.size();i++){
            t+=s[i];
            backtrack(s,i+1,t,vs);
            t.pop_back();
        }
    }
    string next() {
        return vs[ind++];  
    }
    
    bool hasNext() {
        return ind<vs.size();
    }
```
we can also get the iterator directly.

### 1289. Minimum Falling Path Sum II	(***)
the dp relation is apparent. It only involves with previous row's min and second min.
There is a O(N) method to find the min and 2nd min.
```cpp
    int minFallingPathSum(vector<vector<int>>& arr) {
        int m=arr.size(),n=arr[0].size();
        for(int i=1;i<m;i++){
            vector<int> min2=findmin2(arr[i-1]);
            //print(arr[i-1]);
            //print(min2);
            for(int j=0;j<n;j++){
                arr[i][j]+=(arr[i-1][j]==min2[0]?min2[1]:min2[0]);
            }
        }
        return *min_element(arr[m-1].begin(),arr[m-1].end());
    }
    vector<int> findmin2(vector<int>& num){
        int a=INT_MAX,b=INT_MAX;
        for(int t: num){
            if(t<a){
                b=a;
                a=t;
            }
            else if(t<b) b=t;
        }
        return {a,b};
    }
```

## contest 166
### 1281. Subtract the Product and Sum of Digits of an Integer (*)
simple
### 1282. Group the People Given the Group Size They Belong To (**)
greedy: sort the group size with index and then take by turn the elements
### 1283. Find the Smallest Divisor Given a Threshold (***)
binary search to find the min value.
a small trick: it needs to get the ceil division

```cpp
    int smallestDivisor(vector<int>& nums, int threshold) {
        //binary search to find the divisor
        int l=1,r=1e6;
        while(l<r){
            int m=l+(r-l)/2;
            if(getsum(nums,m)<=threshold) r=m;
            else l=m+1;
        }
        return l;
    }
    int getsum(vector<int>& nums,int m){
        int ans=0;
        for(int t: nums){
            ans+=(t+m-1)/m;
        }
        return ans;
    }
```

### 1284. Minimum Number of Flips to Convert Binary Matrix to Zero Matrix	(***)
a typical bfs to get the shortest distance
target is mxn '0's.
queue to store the matrix, and visited using the serialized matrix string.
```cpp
    int minFlips(vector<vector<int>>& mat) {
        //using bfs
        int m=mat.size(),n=mat[0].size();
        string target="000000000";
        target=target.substr(0,m*n);
        typedef vector<vector<int>> vvi;
        queue<vvi> q;
        unordered_set<string> v;
        q.push(mat);
        v.insert(mat2str(mat));
        int step=0;
        while(q.size()){
            int sz=q.size();
            while(sz--){
                vvi tm=q.front();
                q.pop();
                if(mat2str(tm)==target) return step;
                for(int i=0;i<m;i++){
                    for(int j=0;j<n;j++){
                        //choose i,j to flip
                        vvi ttm=tm;
                        ttm[i][j]^=1;
                        if(i) ttm[i-1][j]^=1;
                        if(i+1<m) ttm[i+1][j]^=1;
                        if(j) ttm[i][j-1]^=1;
                        if(j+1<n) ttm[i][j+1]^=1;
                        string s=mat2str(ttm);
                        if(v.count(s)) continue;
                        q.push(ttm);
                        v.insert(s);
                    }
                }
            }
            step++;
        }
        return -1;
    }
    string mat2str(vector<vector<int>>& m){
        string ans;
        for(int i=0;i<m.size();i++)
            for(int j=0;j<m[0].size();j++){
                ans+='0'+m[i][j];
            }
        return ans;
    }
```

## contest 165
### 1275. Find Winner on a Tic Tac Toe Game	(***)
hashmap or using array.
play1 set 1, player 2 set -1.
diagonal: r==c.
anti-diagonal: r+c==n-1
```cpp
    string tictactoe(vector<vector<int>>& moves) {
        int row[3]={0},col[3]={0},diag1=0,diag2=0;
        for(int i=0;i<moves.size();i++){
            int r=moves[i][0],c=moves[i][1];
            if(i%2==0){
                row[r]++;
                col[c]++;
                if(r==c) diag1++;
                if(r+c==2) diag2++;
            }
            else{
                row[r]--;
                col[c]--;
                if(r==c) diag1--;
                if(r+c==2) diag2--;
            }
            if(row[r]==3||col[c]==3||diag1==3||diag2==3) return "A";
            if(row[r]==-3||col[c]==-3||diag1==-3||diag2==-3) return "B";
        }
        if(moves.size()==9) return "Draw";
        return "Pending";
    }
```
### 1276. Number of Burgers with No Waste of Ingredients	 (*)
equation group to find integer solution, elementary math

### 1277. Count Square Submatrices with All Ones (***)
dp: using top left solution to build current solution
```cpp
    int countSquares(vector<vector<int>>& matrix) {
        //count left and top number of continuous 1s
        if(matrix.empty()) return 0;
        int m=matrix.size(),n=matrix[0].size();
        //vector<int> left(n),top(n),prerow(n);
        vector<vector<int>> dp(m,vector<int>(n)); //must be 2d, represent the side length
        int ans=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]){
                    dp[i][j]=min({i?dp[i-1][j]:0,j?dp[i][j-1]:0,i&&j?dp[i-1][j-1]:0})+1;
                    ans+=dp[i][j];
                }
            }
        }
        return ans;
    }
```
### 1278. Palindrome Partitioning III (*****)
subproblem dp[l,k] represents the min cost for string length l (from begining) with k pal-parts.
then dp[i,k]=min(dp[j, k-1]+cost(j,i),dp[i,k])
```cpp
    int palindromePartition(string s, int k) {
        //min cuts to make it palindrome?
        int n=s.size();
        vector<vector<int>> dp(n+1,vector<int>(k+1,n)); //min replace length of string with k parts
        dp[0][0]=0;
        for(int i=1;i<=n;i++){ //length 
            for(int j=1;j<=k;j++){ //number of pal groups
                for(int l=i-1;l>=0;l--) //start 
                {
                    int d=cost(s,l,i-1);
                    dp[i][j]=min(dp[l][j-1]+d,dp[i][j]);
                }
            }
        }
        return dp[n][k];
    }
    int cost(string s,int start,int end){
        int ans=0;
        while(start<end){
            if(s[start++]!=s[end--]) ans++;
        }
        return ans;
    }
```
complexity would be O(kn^3)

optimization: using memoization, we can reduce the cost(i,j) problem into another dp problem, the complexity would be O(kN^2)
dp[l,k] is the min cost for length l (starting from beginning) with k parts, dp1[i,j] is the cost to make s[i,j] to be one palindrome part.
dp1[i,j]=dp[i+1,j-1]+(s[i]!=s[j]).
```cpp
    int palindromePartition(string s, int k) {
        //two dp problem: dp1[l,k] cost for string from begin to l with k parts
        //dp2[i,j] cost for string s[i,j] as one parts
        int n=s.size();
        vector<vector<int>> dp(n+1,vector<int>(k+1,n)),dp1(n,vector<int>(n)); //min replace length of string with k parts
        dp[0][0]=0;
        for(int i=1;i<=n;i++){ //length 
            for(int j=1;j<=k;j++){ //number of pal groups
                for(int l=i-1;l>=0;l--) //start 
                {
                    dp1[l][i-1]=(l+1<i-1?dp1[l+1][i-2]:0)+(s[l]!=s[i-1]);
                    dp[i][j]=min(dp[l][j-1]+dp1[l][i-1],dp[i][j]);
                }
            }
        }
        return dp[n][k];
    }
```

## biweek contest 14
### 1271. Hexspeak
convert to hex if it only contains 1,0 ABCDEF
simple
### 1272. Remove Interval	(***)
O(N) one pass scan
- interval non-overlap
- interval overlap with the given interval.

```cpp
    vector<vector<int>> removeInterval(vector<vector<int>>& intervals, vector<int>& toBeRemoved) {
        vector<vector<int>> res;
        auto start = toBeRemoved[0], end = toBeRemoved[1];
        for (auto &v : intervals) {
            if (v[1] <= start || v[0] >= end) res.push_back(v);
            else {
                if (v[0] < start) res.push_back({v[0], start});
                if (v[1] > end) res.push_back({end, v[1]});
            }
        }
        return res;
    }
```	
	
### 1273. Delete Tree Nodes(****)
delete the subtree with sum of zero. return the number of nodes left.
the tree relation is given by parent and array.
approach: find the root and build the tree relation in data structure
postorder traversal and remove the subtree if sum=0. We need to get the sum and number of nodes at the same time.

```cpp
    int deleteTreeNodes(int nodes, vector<int>& parent, vector<int>& value) {
        int n=parent.size();
        vector<unordered_set<int>> tree(n);
        int root=0;
        for(int i=0;i<n;i++){
            if(parent[i]==-1) {root=i;continue;}
            tree[parent[i]].insert(i);
        }
        
        return dfs(root,tree,value)[0];
    }
    vector<int> dfs(int root,vector<unordered_set<int>>& tree,vector<int>& v){
        vector<int> res(2);
        for(int t: tree[root]){
            auto p=dfs(t,tree,v);
            res[0]+=p[0];
            res[1]+=p[1];
        }
        res[1]+=v[root];
        if(res[1]==0) res[0]=0;else res[0]++;
        return res;
    }
```	
### 1274. Number of Ships in a Rectangle(****)
this is similar to a quadtree problem. 
divide into subgrid if there are ships
make sure the division is non-overlapped.

```cpp
    int countShips(Sea sea, vector<int> topRight, vector<int> bottomLeft) {
        int ans=0;
        if(topRight[0]<bottomLeft[0] || topRight[1]<bottomLeft[1]) return 0;
        if(sea.hasShips(topRight,bottomLeft)){
            if(topRight==bottomLeft) return 1;
            vector<int> mid={(topRight[0]+bottomLeft[0])/2,(topRight[1]+bottomLeft[1])/2};
            ans+=countShips(sea,mid,bottomLeft)+
                countShips(sea,{topRight[0],mid[1]},{mid[0]+1,bottomLeft[1]})+
                countShips(sea,{mid[0],topRight[1]},{bottomLeft[0],mid[1]+1})+
                countShips(sea,topRight,{mid[0]+1,mid[1]+1});
        }
        return ans;
    }
```
base condition: 
bottom left, top right not forming a legal rectangle--->0
rectangle reduces to a point  ---> hasships

## contest 164
### 1266. Minimum Time Visiting All Points (****)
the critical observation is: the min time from point 1 to point 2 is the max(abs(dx),abs(dy))

### 1267. Count Servers that Communicate (***)
greedy: just count each row and each col's server number. and count them only when either of them >1

### 1268. Search Suggestions System (****)
output the top 3 matches (with duplicates)
using trie. trie node store the count of word, the word, leaf in the leaf node
alphabetical can be guaranteed by dfs of the trie.
```cpp
    struct TrieNode{
        TrieNode* child[26];
        bool leaf;
        int cnt;
        string w;
        TrieNode(){leaf=0;cnt=0;memset(child,0,26*sizeof(TrieNode*));}
    };
    TrieNode* root;
    void addWord(string w){
        TrieNode* p=root;
        for(char c: w){
            if(!p->child[c-'a']) p->child[c-'a']=new TrieNode();
            p=p->child[c-'a'];
        }
        p->leaf=1;
        p->w=w;
        p->cnt++;
    }
    TrieNode* searchPref(string pre){
        TrieNode* p=root;
        for(char c: pre){
            if(!p->child[c-'a']) return 0;
            p=p->child[c-'a'];
        }
        return p;
    }
    void dfs(TrieNode* p,vector<string>& vt){
        //vector<string> ans;
        //int cnt=0;
        if(!p || vt.size()>=3) return;
        if(p->leaf) {for(int i=0;i<p->cnt;i++) if(vt.size()<3) vt.push_back(p->w);}
        for(auto t: p->child){
            if(t){
                dfs(t,vt);
            }
        }
    }
    //search: matches the prefix and return the first 3 leaf
    vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {
        vector<vector<string>> ans;
        root=new TrieNode();
        for(auto w: products) addWord(w);
        string prefix;
        for(char c: searchWord){
            prefix+=c;
            TrieNode* p=searchPref(prefix);
            vector<string> vt;
            if(!p) ans.push_back({});
            else {dfs(p,vt),ans.push_back(vt);}
        }
        return ans;
    }
```
### 1269. Number of Ways to Stay in the Same Place After Some Steps	(****)
typical dp: the recurrence relation is apparent.
but there is a critical optimization: after k steps, it can only goes to a[k], the i>k elements are useless.
similar for several jump game problems. 
```cpp
    int numWays(int k, int n) {
        //+1,-1,0 and combination to give 0 using k choices, make sure the position shall be in 0 to n-1
        //dp[i,j] for step i at position j, answer is dp[k,0]
        //recurrence relation: dp[i,j]=dp[i-1,j-1]+dp[i-1,j]+dp[i-1,j+1] with j,j-1,j+1 in the range
        n=min(k,n);
        vector<vector<long>> dp(k+1,vector<long>(n));
        //base case: k=0
        int mod=1e9+7;
        //for(int i=0;i<n;i++) dp[0][i]=1;
        dp[0][0]=1;
        for(int i=1;i<=k;i++){
            for(int j=0;j<n;j++){
                dp[i][j]=dp[i-1][j];
                if(j>0) dp[i][j]+=dp[i-1][j-1];
                if(j<n-1) dp[i][j]+=dp[i-1][j+1];
                dp[i][j]%=mod;
            }
        }
        return dp[k][0];
    }
```	
	
## contest 163
### 1260. Shift 2D Grid (***)
idea: convert to 1d and rotate, then convert to 2d
note: c++ rotate is left rotate, this problem needs right rotate
```cpp
    vector<vector<int>> shiftGrid(vector<vector<int>>& grid, int k) {
        vector<int> t;
        int m=grid.size(),n=grid[0].size();
        for(int i=0;i<m;i++)
            for(int j=0;j<n;j++) t.push_back(grid[i][j]);
        //right rotate
        k%=m*n;
        k=m*n-k;
        rotate(t.begin(),t.begin()+k,t.end());
        //copy(t.begin(),t.end(),ostream_iterator<int>(cout," "));
        int cnt=0;
        for(int i=0;i<m;i++)
            for(int j=0;j<n;j++) grid[i][j]=t[cnt++];
        return grid;
    }
```	
### 1261. Find Elements in a Contaminated Binary Tree (***)
dfs with hashmap
```cpp
    unordered_set<int> ms;
    FindElements(TreeNode* root) {
        recover(root,0);
    }
    
    void recover(TreeNode* root,int v){
        if(!root) return;
        root->val=v;
        ms.insert(v);
        recover(root->left,2*v+1);
        recover(root->right,2*v+2);
    }
    bool find(int target) {
        return ms.count(target);
    }
```
### 1262. Greatest Sum Divisible by Three	(***)
idea: greedy approach: sort the array and get the sum, and remove the smallest element with rem=sum%3. 
if sum%3==1, remove first rem=1 or two with rem=2
if sum%3==2, remove first rem=2 or two with rem=1
the code is not very efficient
```cpp
    int maxSumDivThree(vector<int>& nums) {
        //mod will only has 0,1,2 and we choose from them
        //we only need store the 3 elememts in each
        sort(nums.begin(),nums.end());
        //copy(nums.begin(),nums.end(),ostream_iterator<int>(cout," "));
        vector<int> ori=nums;
        int tsum=accumulate(nums.begin(),nums.end(),0);
        //cout<<tsum;
        int sum0=0;
        for(int& i: nums) {
            i%=3;
            sum0+=i;
        }
        sum0%=3;
        if(sum0==0) return tsum;
        //==1, we can find the first 1 or the first 2 of 2
        //==2, we can find the first 2 or first two 1s
        if(sum0==1){
            int cnt2=0,sum2=0;
            for(int i=0;i<nums.size();i++){
                if(nums[i]==1) return cnt2==2?max(tsum-ori[i],tsum-sum2):tsum-ori[i];
                if(nums[i]==2 && cnt2<2) cnt2++,sum2+=ori[i];
                //if(cnt2>2) return tsum-sum2;
            }
            if(cnt2==2) return tsum-sum2;
            return 0;
        }
        if(sum0==2){
            int cnt2=0,sum2=0;
            for(int i=0;i<nums.size();i++){
                if(nums[i]==2) return cnt2==2?max(tsum-ori[i],tsum-sum2):tsum-ori[i];
                if(nums[i]==1 && cnt2<2) cnt2++,sum2+=ori[i];
                
            }
            if(cnt2==2) return tsum-sum2;
            return 0;
        }
        return 0;
    }
```	

### 1263. Minimum Moves to Move a Box to Their Target Location (*****)
two bfs problem

typical bfs problem for shortest distance.
The person shall be able to move to the location behind the box to make a move.
we can use dfs/bfs to check if person can move to desired location.
however dfs will get TLE since dfs is one direction forward until failure, hence dfs will use more time in average.

check if person can move a position is a conventional bfs.
check box can be moved to a position is a bit tricky, it needs both the person and box position, so we use a pair of position in the queue
and the visited shall also use combined information, I used the string combination of the two positions.

    int minPushBox(vector<vector<char>>& grid) {
        //bfs with person and box, the person can move in the free cells
        //person must be able to walk to the box.
        int m=grid.size(),n=grid[0].size();
        queue<pair<int,int>> q; //store the next valid box position: it shall store: player,box,
        unordered_set<string> v;
        int src=0,dst=0,player=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]=='S') {player=i*n+j;grid[i][j]='.';}
                if(grid[i][j]=='B') {src=i*n+j;grid[i][j]='.';}
                if(grid[i][j]=='T') {dst=i*n+j;grid[i][j]='.';}
            }
        }
        if(src==dst) return 0;
        q.push({src,player});
        int step=0;
        int dir[][4]={{-1,0},{1,0},{0,-1},{0,1}};
        while(q.size()){
            int sz=q.size();
            while(sz--){
                auto pos=q.front();
                q.pop();
                int box=pos.first,player=pos.second;
                if(box==dst) return step;
                int xb=box/n,yb=box%n;
                for(auto d: dir){
                    int x=xb+d[0],y=yb+d[1]; //new box position
                    int xp=xb-d[0],yp=yb-d[1];
                    if(x<0||y<0||x>=m||y>=n||grid[x][y]=='#') continue;
                    if(xp<0||yp<0||xp>=m||yp>=n||grid[xp][yp]=='#') continue;
                    string s=to_string(box)+","+to_string(xp*n+yp);//box pos+person pos
                    if(v.count(s)) continue;
                    if(can_access(grid,player,xp*n+yp,box)){
                        q.push({x*n+y,box});//make a push, box move to new, p moves to box
                        v.insert(s);
                    }
                }
            }
            step++;
        }
        return -1;
    }
    bool can_access(vector<vector<char>>& g,int src,int dst,int box){
        int m=g.size(),n=g[0].size();
        //bfs shall be better than dfs
        queue<int> q;
        vector<bool> v(m*n);
        q.push(src);
        v[src]=1;
        int dir[][2]={{-1,0},{1,0},{0,-1},{0,1}};
        g[box/n][box%n]='#';
        while(q.size()){
            int sz=q.size();
            while(sz--){
                int p=q.front();
                q.pop();
                if(p==dst) {g[box/n][box%n]='.';return 1;}
                int x0=p/n,y0=p%n;
                for(auto d: dir){
                    int x=x0+d[0],y=y0+d[1];
                    if(x<0||y<0||x>=m||y>=n||g[x][y]!='.'||v[x*n+y]) continue;
                    v[x*n+y]=1;
                    q.push(x*n+y);
                }
            }
        }
        g[box/n][box%n]='.';
        return 0;
    }
```
