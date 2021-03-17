Optimization techniques 

In competitive programming, optimization is important to conquer the constraints or reduce the complexity by one order. Without this, you will most likely get TLE error even if you have the correct approach.

In leetcode, mostly they are marked as hard problems.

1787. Make the XOR of All Segments Equal to Zero

It may be not super hard to derive the 2d dp approach with O(1024^2*k)
dp[i,j] represents the min replacement for length i (prefix) array with xor=j.
dp[i,j]=min(dp[i,j],dp[i-1][j^x]+ceil(n/i)-freq(i,x)) j=0...1023

To reduce the complexity, we need to move the dp[i-1,j^x] out of the loop and it is feasible since it is the previous result.
- x is one of the element at i, then freq is not zero
- x is not in the set of x[i], then freq is 0. ceil(n/i) is const. and
dp[i,j]=min(dp[i,j],min(dp[i-1,j^x))+ceil(n/i))

```
    int minChanges(vector<int>& nums, int k) {
        int n=nums.size();
		vector<vector<int>> dp(k+1,vector<int>(1024,n));
        vector<vector<int>> v(k);
		//base case k=1
		int freq[k][1024];
        memset(freq,0,k*1024*sizeof(int));
		//precalculate the frequency
		for(int i=0;i<nums.size();i++){
			freq[i%k][nums[i]]++;
            v[i%k].push_back(nums[i]);
        }

        dp[0][0]=0;
        int premin=0; //premin=min(dp[i-1,l])
		for(int i=1;i<=k;i++){ //length of subarray
            int premin1=n+1;
			for(int j=0;j<1024;j++){ //xor=j
				for(int x: v[i-1]){ //choose one of the element at i%k
					dp[i][j]=min(dp[i][j],dp[i-1][x^j]+(n-i+k)/k-freq[i-1][x]);
				}
                dp[i][j]=min(dp[i][j],premin+(n-i+k)/k);
                premin1=min(premin1,dp[i][j]);
			}
            premin=premin1;
		}
		return dp[k][0];
    }
```	

1792. Maximum Average Pass Ratio
TLE when using pq with vector
vector is a data structure with variable lemgth and is less efficient than pair or array<int,n>

there are quite a few such questions in leetcode which the efficiency matters.






