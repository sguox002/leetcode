## biweek 35

### 1588. Sum of All Odd Length Subarrays
<em>Given an array of positive integers arr, calculate the sum of all possible odd-length subarrays.

A subarray is a contiguous subsequence of the array.

Return the sum of all odd-length subarrays of arr.</em>

approach 1: brutal force with each element as the end.
O(N^3)
```cpp
    int sumOddLengthSubarrays(vector<int>& arr) {
        int ans=0,n=arr.size();
        for(int i=0;i<arr.size();i++){
            for(int j=i;j>=0 && (j-i)%2==0;j-=2){
                for(int k=j;k<=i;k++) ans+=arr[k];
            }
        }
        return ans;
    }
```

optimization:
[1,4,2,5,3] length=5
[1]
[4]
[2]
[1,4,2]
[5]
[4,2,5]
[3]
[2,5,3]
[1,4,2,5,3]
1 appears 3 times
4 appears 4 times
2 appears 5 times
5 appears 3 times
3 appears 3 times

subarrays containing A[i]:
left has 0,1,2,...i, total i+1 choices
right has i,i+1...n-1 total n-i choices
total subarrays containing A[i]: (i+1)*(n-i)
total subarrays containing A[i] with odd length: ((i+1)*(n-i)+1)/2
O(N)

```cpp
    int sumOddLengthSubarrays(vector<int>& A) {
        int res = 0, n = A.size();
        for (int i = 0; i < n; ++i) {
            res += ((i + 1) * (n - i) + 1) / 2 * A[i];
        }
        return res;
    }
```

### 1589. Maximum Sum Obtained of Any Permutation
<em>
We have an array of integers, nums, and an array of requests where requests[i] = [starti, endi]. The ith request asks for the sum of nums[starti] + nums[starti + 1] + ... + nums[endi - 1] + nums[endi]. Both starti and endi are 0-indexed.

Return the maximum total sum of all requests among all permutations of nums.

Since the answer may be too large, return it modulo 109 + 7.
</em>

idea: get the histogram of all query ranges and greedily assign the larger value to the one with higher frequency

```cpp
    int maxSumRangeQuery(vector<int>& nums, vector<vector<int>>& requests) {
        int mod=1e9+7;
        //greedy: the highest freq components need to be bigger
        sort(begin(nums),end(nums));
        map<int,int> mp;
        for(auto r: requests){
            mp[r[0]]++;
            mp[r[1]+1]--;
        }
        int pre=0;
        vector<int> freq(nums.size());
        int i=0;
        for(auto t: mp){
            pre+=t.second;
            for(;i<t.first;i++) freq[i]=pre-t.second;
        }
        //sort according to the freq
        sort(begin(freq),end(freq));
        long ans=0,n=nums.size();
        for(int i=0;i<n;i++){
            ans+=(long)freq[i]*nums[i]%mod;
            ans%=mod;
        }
        return ans;
    }
```	

### 1590. Make Sum Divisible by P
<em>
Given an array of positive integers nums, remove the smallest subarray (possibly empty) such that the sum of the remaining elements is divisible by p. It is not allowed to remove the whole array.

Return the length of the smallest subarray that you need to remove, or -1 if it's impossible.

A subarray is defined as a contiguous block of elements in the array
</em>

idea:
the total sum %p=tsum, equivalently find the min subarray with sum%p=tsum.
so this is a hashmap problem similar to 2 sum

```cpp
    int minSubarray(vector<int>& nums, int p) {
        //tsum%p and find smallest subarray with psum%p==tsum%p
        int ans=nums.size();
        int tsum=0;
        for(int i: nums) tsum+=i,tsum%=p;
        if(tsum==0) return 0;
        int pre=0;
        unordered_map<int,int> mp; //prefix sum vs index
        mp[0]=-1;
        for(int i=0;i<nums.size();i++){
            if(nums[i]%p==tsum) return 1;
            pre+=nums[i],pre%=p;
            if(mp.count(pre-tsum)) ans=min(ans,i-mp[pre-tsum]);
            if(mp.count(pre+p-tsum)) ans=min(ans,i-mp[pre-tsum+p]);
            mp[pre]=i;
        }
        return ans==nums.size()?-1:ans;
    }
```

actually we do not need pre-tsum and pre+p-tsum but using (pre+p-tsum)%p equivalently.

### 1591. Strange Printer II
<em>
There is a strange printer with the following two special requirements:

On each turn, the printer will print a solid rectangular pattern of a single color on the grid. This will cover up the existing colors in the rectangle.
Once the printer has used a color for the above operation, the same color cannot be used again.
You are given a m x n matrix targetGrid, where targetGrid[row][col] is the color in the position (row, col) of the grid.

Return true if it is possible to print the matrix targetGrid, otherwise, return false.
</em>

This is actually straightforward.
we calculate each element's rectangle and print one by one and replace them with 0.
The sequence is we shall print the subrect containing only the element and 0.

```cpp
    bool isPrintable(vector<vector<int>>& targetGrid) {
        //the one with single element shall be printed first.
        //reverse and return to all 0.
        unordered_map<int,vector<int>> mp;
        int m=targetGrid.size(),n=targetGrid[0].size();
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                mp[targetGrid[i][j]].push_back(i*n+j);
            }
        }
        return valid(mp,targetGrid);
    }
    //we will eliminate one valid prints in one round
    bool valid(unordered_map<int,vector<int>>& mp,vector<vector<int>>& targetGrid){
        //start from smallest rect.
        //top left and bottom right
        int m=targetGrid.size(),n=targetGrid[0].size();
        bool ans=0;
        while(mp.size()){
            //print(targetGrid);
            int sz=mp.size();
            for(auto t: mp){
                bool found=1;
                int xmin=m,xmax=0,ymin=n,ymax=0;
                for(auto j: t.second){
                    int r=j/n,c=j%n;
                    xmin=min(xmin,r),xmax=max(xmax,r);
                    ymin=min(ymin,c),ymax=max(ymax,c);
                }
                for(int i=xmin;i<=xmax && found;i++){
                    for(int j=ymin;j<=ymax;j++){
                        if(targetGrid[i][j] && targetGrid[i][j]!=t.first) {
                            found=0;
                            break;
                        }   
                    }
                }
            
                if(found){
                    for(int i=xmin;i<=xmax;i++){
                        for(int j=ymin;j<=ymax;j++){
                            targetGrid[i][j]=0;
                        }
                    }
                    mp.erase(t.first);
                    break;
                }
            }
            if(mp.size()==sz) break;
        }
        return mp.empty();
    }
```	

	

	
