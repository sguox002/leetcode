## contest 186

### 1422. Maximum Score After Splitting a String
<em>
Given a string s of zeros and ones, return the maximum score after splitting the string into two non-empty substrings (i.e. left substring and right substring).

The score after splitting a string is the number of zeros in the left substring plus the number of ones in the right substring.
</em>

suppose the input length=n, and we have m 1s and n-m 0s
we just count 1s in the left for example:
length i, j 1s, i-j 0s

```cpp
    int maxScore(string s) {
        int ans=0;
        int sum=0,n=s.size();
        for(char c: s) sum+=c=='1';
        int prefix=s[0]=='1';
        ans=max(ans,1+sum-2*prefix);
        for(int i=1;i<s.size()-1;i++){
            prefix+=s[i]=='1';
            ans=max(ans,i+1-prefix+sum-prefix);
        }
        return ans;
    }
```

### 1423. Maximum Points You Can Obtain from Cards	
<em>
There are several cards arranged in a row, and each card has an associated number of points The points are given in the integer array cardPoints.

In one step, you can take one card from the beginning or from the end of the row. You have to take exactly k cards.

Your score is the sum of the points of the cards you have taken.

Given the integer array cardPoints and the integer k, return the maximum score you can obtain.
</em>

Equivalently we are looking for the min window sum with window size n-k.
dp will TLE for this.

```cpp
    int maxScore(vector<int>& cardPoints, int k) {
        int sum=accumulate(begin(cardPoints),end(cardPoints),0);
        int wsum=0,n=cardPoints.size();
        int ans=INT_MAX;
        k=n-k;
        for(int i=0;i<n;i++){
            wsum+=cardPoints[i];
            if(i>=k-1) {
                if(i>=k) wsum-=cardPoints[i-k];
                ans=min(ans,wsum);
            }
        }
        return sum-ans;
    }
```	
be careful sliding window sum, k-1 and >k-1 two cases

### 1424. Diagonal Traverse II
<em>
Given a list of lists of integers, nums, return all elements of nums in diagonal order as shown in the below images.
</em>

approach 1: sort by r+c, O(nlogn)
```cpp
    vector<int> findDiagonalOrder(vector<vector<int>>& nums) {
        //diagonal i+j=const
        int n=nums.size();
        //sort the matrix in i+j order
        vector<vector<int>> mat;
        for(int i=0;i<n;i++){
            for(int j=0;j<nums[i].size();j++) 
                mat.push_back({i,j,nums[i][j]});
        }
        sort(begin(mat),end(mat),[](vector<int>& a,vector<int>& b){
            return a[0]+a[1]<b[0]+b[1] || (a[0]+a[1]==b[0]+b[1] && a[0]>b[0]);
        });
        vector<int> ans;
        for(auto t: mat) ans.push_back(t[2]);
        return ans;
    }
```
This one TLE

Approach 2: BFS
starting from i+j=0 to infinite
O(N): add its down first and then its right
we can use hashset as visited to avoid duplicates.
bfs: think it as tree
```python
    def findDiagonalOrder(self, nums: List[List[int]]) -> List[int]:
        ans = []
        m = len(nums)
        
        queue = collections.deque([(0, 0)])
        while queue:
            row, col = queue.popleft()
            ans.append(nums[row][col])
            # we only add the number at the bottom if we are at column 0
            if col == 0 and row + 1 < m:
                queue.append((row + 1, col))
            # add the number on the right
            if col + 1 < len(nums[row]):
                queue.append((row, col + 1))
            
        return an
```

### approach: hashmap
```cpp
vector<int> findDiagonalOrder(vector<vector<int>>& nums) {
        vector<int> answer;
        unordered_map<int, vector<int>> m;
        int maxKey = 0; // maximum key inserted into the map i.e. max value of i+j indices.
        
        for (int i=0; i<nums.size(); i++) {
            for (int j=0; j<nums[i].size(); j++) {
                m[i+j].push_back(nums[i][j]); // insert nums[i][j] in bucket (i+j).
                maxKey = max(maxKey, i+j); // 
            }
        }
        for (int i=0; i<= maxKey; i++) { // Each diagonal starting with sum 0 to sum maxKey.
            for (auto x = m[i].rbegin(); x != m[i].rend(); x++) { // print in reverse order.
                answer.push_back(*x); 
            }
        }
        
        return answer;
    }
```

### 1425. Constrained Subsequence Sum
<em>
Given an integer array nums and an integer k, return the maximum sum of a non-empty subsequence of that array such that for every two consecutive integers in the subsequence, nums[i] and nums[j], where i < j, the condition j - i <= k is satisfied.

A subsequence of an array is obtained by deleting some number of elements (can be zero) from the array, leaving the remaining elements in their original order.
</em>

It seems that is a knapsack dp problem. Let's try this approach first.
if we choose ith element, we need ensure previous chosen is >i-k.

let's try top down approach first
```cpp
    int constrainedSubsetSum(vector<int>& nums, int k) {
        int mx=*max_element(begin(nums),end(nums));
        if(mx<0) return mx;
        return helper(nums,-1,0,k);
    }
	int helper(vector<int>& nums,int prev,int start,int k){
		if(start>=nums.size()) return 0; 
		if(prev<0){ //no chosen yet
			return max({nums[start],helper(nums,prev,start+1,k),
			nums[start]+helper(nums,start,start+1,k)});
		}
		if(start>prev+k) return 0; //cannot add
		return max({nums[start],helper(nums,prev,start+1,k),
		nums[start]+helper(nums,start,start+1,k)});
	}
```	
complexity O(2^n)
this will pass the example tests.

add memoization will be able to reduce to O(N^2) [prev][start]
however since n is up to 10^5, the complexity is too high

We really need O(N) or O(nlogn) complexity.
or equivalently we need to find the min sum and the index difference >k.

from the above, we need a 1d dp approach
assuming dp[i] is the maximum sum ending with nums[i], then we choose the max among all dp[i]
for current item, we want to append to previous max with j+k<=i.
if current max is out of range, pop it.
we can use heap or pq to do it.

```cpp
    int constrainedSubsetSum(vector<int>& nums, int k) {
		int n=nums.size();
		vector<int> dp(n);
		int ans=INT_MIN;
		priority_queue<vector<int>> pq; //sum with index
        
		for(int i=0;i<n;i++){
			while(pq.size() && i>pq.top()[1]+k) pq.pop();
			if(pq.empty()) dp[i]=nums[i];
			else dp[i]=max(pq.top()[0],0)+nums[i];
			pq.push({dp[i],i});
			ans=max(ans,dp[i]);
		}
		return ans;
    }
```	
- note: if previous sum is <0, we start a new one. This is very similar to max subarray sum.


hence we see that we are searching for the max, sliding window k.
using deque we can reduce it to O(N)

deque+dp. Monotonic deque
```cpp
    int constrainedSubsetSum(vector<int>& nums, int k) {
		int n=nums.size();
		vector<int> dp(n);
		int ans=INT_MIN;
	    deque<int> dq; //store the dp in the dq    
		for(int i=0;i<n;i++){
			//front
			while(dq.size() && i>dq.front()+k) dq.pop_front();
			if(dq.empty()) dp[i]=nums[i];
			else dp[i]=max(dp[dq.front()],0)+nums[i];
			while(dq.size() && dp[i]>=dp[dq.back()]) dq.pop_back();
			dq.push_back(i);
			ans=max(ans,dp[i]);
		}
		return ans;
    }
```

