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
