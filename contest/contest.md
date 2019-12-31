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
typical backtracking problem with extra complexity
first approach: O(N!) complexity
we collect all unique letters and repeat 0 to 9 with the help of hashmap.
however this will get TLE.
A common mistake I often made is: loop over the letters and loop over the digits
the recursive backtracking itself is a loop over the letters. So do not loop over it again.

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
it is easy to understand, however it tries a lot of unnecessary combinations.
for example when we choose the definition for the LSB of each words, the result bit is determined.
but this approach still tries all the combination.
More efficient way is to try col by col from the LSB.
This approach is more complicated since it is a two-backtracking problem:
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
	
## biweek contest 16
### 1299. Replace elemets with greatest element on right side (**)
simple, just do it from right to left and find the max and add to answer

### 1300. sum of mutated array closest to target (****)
first we can sort the array to make calculate the sum more efficient. (but not necessary)
second, it is a binary search problem.
we are actually looking for a value between 0 and max_element so that the sum==target.
when sum>=target, r=mid, else l=mid+1
This is pretty similar to find target in a sorted array using binary search. (the target could or not in the array)

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
 
  



