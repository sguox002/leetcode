338. Counting Bits
given a number get all number of 1s in binary for each one
dp[i]=dp[i/2]+i&1

877. Stone Game
a list of numbers, picking from either end. who wins
we pick positive, the other pick negative, maximize the score
```cpp
    int helper(vector<int>& piles,vector<vector<int>>& dp,int l,int r)
    {
        if(l>r) return 0;
        if(dp[l][r]!=INT_MAX) return dp[l][r];
        dp[l][r]=max(piles[l]-helper(piles,dp,l+1,r),piles[r]-helper(piles,dp,l,r-1));
        return dp[l][r];
    }
```
931. Minimum Falling Path Sum
element + min(previous row closest three sum)

647. Palindromic Substrings
dp[i,j] is the number of p-string for S(i...j)
self+dp[i,j-1]+dp[i+1,j]-dp[i+1,j-1]
iterate i reverse, j normal

413. Arithmetic Slices
dp[i] means the number of arithmetic slices ending with A[i]
dp[i]=dp[i-1]+1 if it satisfy arithmetic
answer is the accumulation

712. Minimum ASCII Delete Sum for Two Strings
edit distance dp[i,j]

714. Best Time to Buy and Sell Stock with Transaction Fee
dp[i]=max(dp[i-1],dp[j]+price[i]-price[j]-fee) for j=0 to i-1
sell or not sell on ith day

646. Maximum Length of Pair Chain
//dp[i] is the number of longest chain for ith pair, it shall be the previous chain+1

343. Integer Break
make the product the largest
j*(i-j) vs j*dp[i-j], break into j and i-j two numbers

638. Shopping Offers
better using recursive for this

357. Count Numbers with Unique Digits
dp[i] is the number from 10^(i-1) 10^i
first digit: 1-9
second digit: 0-9 but cannot use previous digit, 9
9*9*8*7.....

486. Predict the Winner
same as 877, but we can use direct dp max(num[l]-dp[l+1,r],num[r]-dp[l,r-1])
l: reverse iteration, r normal iteration

650. 2 Keys Keyboard
copy all and paste
dp[i]=dp[j]+i/j if i%j==0. paste j for i/j times, find the max j.

392. Is Subsequence
dp: by deleting char to see if we can reach to the other string
greedy: always match the first char using two pointers

62. Unique Paths
easy

494. Target Sum
using + and - to reach the target sum. how many ways?
negative sum N and positive sum P
P+N=target
P-N=total so P=(S+T)/2
it reduces to get the number of ways to reach a new target sum.
a knapsack problem without repetition
```cpp
    int findTargetSumWays(vector<int>& nums, int S) {
        //P-N=T, P+N=S P=(S+T)/2 T is the total
        int T=accumulate(nums.begin(),nums.end(),0);
        if((T+S)%2) return 0;
        int target=(T+S)/2;//T+S must be even
        if(target>T) return 0;
        int n=nums.size();
        vector<vector<int>> dp(n+1,vector<int>(target+1));
        //boundary: 0th row, using 0 element to reach target, no way
        //0th col: using elements to reach target 0, can only choose empty
        //for(int i=0;i<=n;i++) dp[i][0]=1;
        dp[0][0]=1; //other are all 0
        for(int i=1;i<=n;i++) //iterate over elements
        {
            for(int j=0;j<=target;j++) //j>=nums[i]
            {
                dp[i][j]=dp[i-1][j];
                if(j>=nums[i-1]) dp[i][j]+=dp[i-1][j-nums[i-1]];
            }
        }
        return dp[n][target];
        
    }
```

740. Delete and Earn
delete num[i] and earn points num[i] and the same time delete num[i]-1 and num[i]+1
max the points
this is house robber with duplicates (if there are a lot of value=nums[i]). So we use bucket sort (bucket distance is 1)

516. Longest Palindromic Subsequence
again can convert to edit distance. by reversing the string we are looking for the largest common subsequence
```cpp
    int longestPalindromeSubseq(string s) {
        string rs=s;
        reverse(rs.begin(),rs.end());
        int n=s.size();
        //align the two strings with min deletion
        vector<vector<int>> dp(n+1,vector<int>(n+1));
        for(int i=0;i<=n;i++) dp[0][i]=dp[i][0]=i;
        //dp(i,j) represents the s0(0...i-1) and s1(0...j-1)
        for(int i=1;i<=n;i++)
        {
            for(int j=1;j<=n;j++)
            {
                int t=min(dp[i-1][j],dp[i][j-1])+1; //delete a char from s0 or s1
                //mismatch or match
                if(s[i-1]==rs[j-1]) dp[i][j]=dp[i-1][j-1];
                else dp[i][j]=min(dp[i-1][j-1]+2,t);//delete a char from s0 and s1
            }
        }
        return (2*n-dp[n][n])/2;
    }
```
64. Minimum Path Sum
from top left to bottom right
dp[i][j]=min(dp[i-1][j],dp[i][j-1])+grid[i][j];

96. Unique Binary Search Trees
using node 1 to n, get the number of unique BST
if i is the root, we have 1 to i-1 in the left and i+1 to n in the right. both are subproblem
The number is dp[i]=sum(dp[j-1]*dp[i-j]) j from 1 to i
```cpp
    int numTrees(int n) {
        vector<int> dp(n+1);
        //dp[i] is the number of bst with i nodes
        dp[0]=1;dp[1]=1;
        for(int i=2;i<=n;i++)
        {
            for(int j=1;j<=i;j++) //j is the root node
            {
                dp[i]+=dp[j-1]*dp[i-j];
            }
        }
        return dp[n];
    }
```
377. Combination Sum IV
give a target, get the number of combinations that sums to the target
knapsack with repetition: for smaller target, we shall iterate over all the numbers.
Note: it also includes climbing stairs, if we have multiple methods to reach i, we need sum them up.

```cpp
    int combinationSum4(vector<int>& nums, int target) {
        //typical dp knapsack problem
        vector<int> dp(target+1);
        dp[0]=1; //when target is 0, empty choice
        for(int i=1;i<=target;i++)
        {
            for(int j=0;j<nums.size();j++)
            {
                if(i>=nums[j]) dp[i]+=dp[i-nums[j]];
            }
        }
        return dp[target];
    }
```    

718. Maximum Length of Repeated Subarray
larget common substr
```cpp
    int findLength(vector<int>& A, vector<int>& B) {
        int m=A.size(), n=B.size();
        vector<vector<int>> dp(m+1,vector<int>(n+1));
        int res=INT_MIN;
        for(int i=1;i<=m;i++)
        {
            for(int j=1;j<=n;j++)
            {
                if(A[i-1]==B[j-1]) dp[i][j]=dp[i-1][j-1]+1;
                res=max(res,dp[i][j]);
            }
        }
        return res;
    }
```

309. Best Time to Buy and Sell Stock with Cooldown
after sell, you have to rest one day before buy stock
on ith day,
if we sell, the profit is dp[j-2]+price[i-2]-price[j-2] (buy at jth day, at least two days before ith day)
if we not sell, the profit is dp[i-1]
note: price[i-2] and price[j-2] is the price at ith and jth day (since we add two guarding elements)
dp[j-2] is the profit before we buy at jth day (sell at two days ago)

```cpp
    int maxProfit(vector<int>& prices) {
        int n=prices.size();
        if(n<2) return 0;
        vector<int> dp(n+2); //dp[i] is the max profit on ith day, adding two guarding
        if(prices[1]>prices[0]) dp[3]=prices[1]-prices[0];
        for(int i=4;i<n+2;i++)
        {
            for(int j=i;j>=2;j--)
            {
                dp[i]=max(dp[i],max(dp[i-1],dp[j-2]+prices[i-2]-prices[j-2]));
            }
        }
        return dp[n+1];
    }
```
Note the j iteration shall from i. 









