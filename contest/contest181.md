## contest 181

### 1389. Create Target Array in the Given Order
<em>
Given two arrays of integers nums and index. Your task is to create target array under the following rules:

Initially target array is empty.
From left to right read nums[i] and index[i], insert at index index[i] the value nums[i] in target array.
Repeat the previous step until there are no elements to read in nums and index.
Return the target array.

It is guaranteed that the insertion operations will be valid
</em>

- just simulate the array growing and insersion.

```cpp
    vector<int> createTargetArray(vector<int>& nums, vector<int>& index) {
        vector<int> ans;
        for(int i=0;i<nums.size();i++){
            ans.insert(ans.begin()+index[i],nums[i]);
        }
        return ans;
    }
```

### 1390. Four Divisors
<em>
Given an integer array nums, return the sum of divisors of the integers in that array that have exactly four divisors.

If there is no such integer in the array, return 0.
</em>

- 4 divisors, 1 and itself, so we only need find two divisors
- if it is a square number, then it will have odd number of divisors

```cpp
    int sumFourDivisors(vector<int>& nums) {
        //squares are not conisdered.
        int ans=0;
        for(int i: nums){
            int cnt=2;
            int i2=sqrt(i);
            if(i2*i2==i) continue;
            vector<int> vt;
            for(int j=2;j<=i2;j++){
                if(i%j==0) {
                    cnt+=2;
                    vt.push_back(j);
                    vt.push_back(i/j);
                }
                if(cnt>4) break;
            }
            if(cnt==4) {
                ans+=1+i+vt[0]+vt[1];
                //cout<<i<<" "<<vt[0]<<" "<<vt[1]<<endl;
            }
        }
        return ans;
    }
```

### 1391. Check if There is a Valid Path in a Grid
<em>
Given a m x n grid. Each cell of the grid represents a street. The street of grid[i][j] can be:
1 which means a street connecting the left cell and the right cell.
2 which means a street connecting the upper cell and the lower cell.
3 which means a street connecting the left cell and the lower cell.
4 which means a street connecting the right cell and the lower cell.
5 which means a street connecting the left cell and the upper cell.
6 which means a street connecting the right cell and the upper cell.

You will initially start at the street of the upper-left cell (0,0). A valid path in the grid is a path which starts from the upper left cell (0,0) and ends at the bottom-right cell (m - 1, n - 1). The path should only follow the streets.

Notice that you are not allowed to change any street.

Return true if there is a valid path in the grid or false otherwise.
</em>

dumb bfs approach:
- the road needs to be able to connect.
- put connected cells into queue.

```cpp
    bool hasValidPath(vector<vector<int>>& grid) {
        //bfs
        int m=grid.size(),n=grid[0].size();
        queue<int> q;
        vector<bool> v(m*n);
        q.push(0);
        v[0]=1;
        int dir[][2]={{-1,0},{1,0},{0,-1},{0,1}};
        while(q.size()){
            int sz=q.size();
            while(sz--){
                int p=q.front();
                q.pop();
                int x=p/n,y=p%n;
                if(x==m-1 && y==n-1) return 1;
                for(auto d: dir){
                    int x0=x+d[0],y0=y+d[1];
                    //check if the two cells are connected
                    if(x0<0 || y0<0 || x0>=m ||y0>=n || v[x0*n+y0]) continue;
                    bool valid=0;
                    if(grid[x][y]==1){
                        valid=(d[1]>0 && (grid[x0][y0]==1 || grid[x0][y0]==3 || grid[x0][y0]==5)) ||
                        (d[1]<0 && (grid[x0][y0]==1 || grid[x0][y0]==4 || grid[x0][y0]==6));
                    }
                    else if(grid[x][y]==2){
                        valid=(d[0]>0 && (grid[x0][y0]==2 || grid[x0][y0]==5 || grid[x0][y0]==6)) ||
                        (d[0]<0 && (grid[x0][y0]==2 || grid[x0][y0]==3 || grid[x0][y0]==4));
                    }
                    else if(grid[x][y]==3){
                        valid=(d[1]<0 && (grid[x0][y0]==1 || grid[x0][y0]==4 || grid[x0][y0]==6)) ||
                        (d[0]>0 && (grid[x0][y0]==2 || grid[x0][y0]==5 || grid[x0][y0]==6));
                    }
                    else if(grid[x][y]==4){
                        valid=(d[1]>0 && (grid[x0][y0]==1 || grid[x0][y0]==3 || grid[x0][y0]==5)) ||
                        (d[0]>0 && (grid[x0][y0]==2 || grid[x0][y0]==5 || grid[x0][y0]==6));
                    }
                    else if(grid[x][y]==5){
                        valid=(d[0]<0 && (grid[x0][y0]==2 || grid[x0][y0]==3 || grid[x0][y0]==4)) ||
                        (d[1]<0 && (grid[x0][y0]==1 || grid[x0][y0]==4 || grid[x0][y0]==6));
                    }
                    else {
                        valid=(d[0]<0 && (grid[x0][y0]==2 || grid[x0][y0]==3 || grid[x0][y0]==4)) ||
                        (d[1]>0 && (grid[x0][y0]==1 || grid[x0][y0]==3 || grid[x0][y0]==5));
                    }
                    
                    
                    if(valid){
                        q.push(x0*n+y0);
                        v[x0*n+y0]=1;
                    }
                }
            }
        }
        return 0;
    }
```	

upscaled bfs: we can divide each cell by 3x3 and then it is much easier.

smarter way: union-find, union the connected cells and check if top left and bottom right are in the same set.
lee215 use upscaled 2x2 approach + union-find.

### 1392. Longest Happy Prefix
<em>
A string is called a happy prefix if is a non-empty prefix which is also a suffix (excluding itself).

Given a string s. Return the longest happy prefix of s .

Return an empty string if no such prefix exists.
</em>

- rolling hash: treat the letter as a digit (base-26).
```cpp
    string longestPrefix(string s) {
        long h1 = 0, h2 = 0, mul = 1, base = 26, mod = 1e9 + 7, len = s.length(), ans = 0;
        for(int i = 0; i < len-1; i++) {
            h1 = (h1*base + (s[i]-'a' + 1))%mod;
            h2 = (h2 + (s[len-i-1]-'a'+1) * mul)%mod;
            mul = mul*base%mod;
            if(h1 == h2) ans = i + 1;
        }
        return s.substr(0, ans);
    }
```

	
	
