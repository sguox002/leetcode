## contest 174
### 1337. The K Weakest Rows in a Matrix
<em>
Given a m * n matrix mat of ones (representing soldiers) and zeros (representing civilians), return the indexes of the k weakest rows in the matrix ordered from the weakest to the strongest.

A row i is weaker than row j, if the number of soldiers in row i is less than the number of soldiers in row j, or they have the same number of soldiers but i is less than j. Soldiers are always stand in the frontier of a row, that is, always ones may appear first and then zeros.
</em>

Approach: count each row number of 1s and sort.
```cpp
    vector<int> kWeakestRows(vector<vector<int>>& mat, int k) {
        if(mat.empty()) return {};
        int m=mat.size(),n=mat[0].size();
        vector<vector<int>> ans(m);
        for(int i=0;i<m;i++){
            int cnt=0;
            for(int j=0;j<n;j++) cnt+=mat[i][j];
            ans[i]={cnt,i};
        }
        stable_sort(ans.begin(),ans.end());
        //ans.resize(k);
        vector<int> res(k);
        for(int i=0;i<k;i++) res[i]=ans[i][1];
        return res;
    }
```
- use stable_sort to maintain its original order if they are tie.

### 1338. Reduce Array Size to The Half
<em>
Given an array arr.  You can choose a set of integers and remove all the occurrences of these integers in the array.

Return the minimum size of the set so that at least half of the integers of the array are removed.
</em>

Approach: using hashmap to get the histogram and then remove the most frequent ones
```cpp
    int minSetSize(vector<int>& arr) {
        int n=arr.size();
        //count and from max to min
        unordered_map<int,int> mp;
        for(int i: arr) mp[i]++;
        vector<int> cnt;
        for(auto t: mp) cnt.push_back(t.second);
        sort(cnt.begin(),cnt.end());
        int ans=0,t=0;
        for(int i=cnt.size()-1;i>=0;i--){
            t+=cnt[i]; ans++;
            if(t>=n/2) return ans;
        }
        return mp.size();
    }
```

### 1339. Maximum Product of Splitted Binary Tree
<em>
Given a binary tree root. Split the binary tree into two subtrees by removing 1 edge such that the product of the sums of the subtrees are maximized.
Since the answer may be too large, return it modulo 10^9 + 7.	
</em>

Intuition: when the a+b=sum and max(a*b) we shall make a b as close as possible or min(abs(a-b))
Approach: two pass postorder, first pass to get the total sum, second pass to get the mindiff.
```cpp
    int maxProduct(TreeNode* root) {
        //the difference of the two sum shall be minimized
        //postorder to get the sum of its left and right subtree
        int mindiff=INT_MAX;
        int tsum=sum(root);

        helper(root,tsum,mindiff);
        int a=(tsum+mindiff)/2,b=tsum-a;
        int mod=1e9+7;
        return (long)a*b%mod;
    }
    int sum(TreeNode* root){
        if(!root) return 0;
        return root->val+sum(root->left)+sum(root->right);
    }
    int helper(TreeNode* root,int tsum,int& mindiff){ //get the min diff
        if(!root) return 0;
        
        int left=helper(root->left,tsum,mindiff);
        int right=helper(root->right,tsum,mindiff);
        int sum=left+right+root->val;
        int diff=abs(2*sum-tsum);//
        mindiff=min(diff,mindiff);
        return sum;
    }
```	

### 1340. Jump Game V
<em>
Given an array of integers arr and an integer d. In one step you can jump from index i to index:

i + x where: i + x < arr.length and 0 < x <= d.
i - x where: i - x >= 0 and 0 < x <= d.
In addition, you can only jump from index i to index j if arr[i] > arr[j] and arr[i] > arr[k] for all indices k between i and j (More formally min(i, j) < k < max(i, j)).

You can choose any index of the array and start jumping. Return the maximum number of indices you can visit.

Notice that you can not jump outside of the array at any time.
</em>

Analysis:
equivalent: 
- jump from low to high
- at each index, the person has several options, and each option will lead to different subproblem, which is a typical dp problem.
- using top down + memoization:

```cpp
    int dp[1001] = {};
    int dfs(vector<int>& arr, int i, int d, int res = 1) {
        if (dp[i]) return dp[i];
        for (auto j = i + 1; j <= min(i + d, (int)arr.size() - 1) && arr[j] < arr[i]; ++j)
            res = max(res, 1 + dfs(arr, j, d));
        for (auto j = i - 1; j >= max(0, i - d) && arr[j] < arr[i]; --j)
            res = max(res, 1 + dfs(arr, j, d));
        return dp[i] = res;
    }
    int maxJumps(vector<int>& arr, int d, int res = 1) {
        for (auto i = 0; i < arr.size(); ++i)
            res = max(res, dfs(arr, i, d));
        return res;
    }
```

bottom up: we need first sort the array and then uses dp (start from highest) otherwise we will get wrong.
other solution can use bfs, dfs, but we need first apply the greedy sort algorithm, otherwise it will be wrong.


```cpp
    int maxJumps(vector<int>& arr, int d) {
        //typical dp
        int n=arr.size();
        int ans=0;
        vector<vector<int>> nums;
        for(int i=0;i<arr.size();i++){
            nums.push_back({arr[i],i});
        }
        sort(nums.begin(),nums.end());//jump from low to high
        vector<int> dp(n,1);
        for(int k=0;k<n;k++){
            int i=nums[k][1];
            for(int j=1;j<=d;j++){
                if(j+i>=n || arr[i]>=arr[j+i]) break;
                dp[i]=max(dp[i],dp[i+j]+1); //
            }
            for(int j=1;j<=d;j++){
                if(i-j<0 || arr[i]>=arr[i-j]) break;
                dp[i]=max(dp[i],dp[i-j]+1);
            }
            ans=max(ans,dp[i]);
        }
        return ans;
    }
```
This is wrong, why?
greedy: we just check smallest first. so we need still jump from high to low. 
change the >= to <= will fix the problem.

The top down approach applies to most dp problem and is the way to figure out the relation. Good problem and good practice.
	



 
