338. Counting Bits

given a number get all number of 1s in binary for each one

dp[i]=dp[i/2]+i&1

877. Stone Game

a list of numbers, picking from either end. who wins

we pick positive, the other pick negative, maximize the score. (for the other player it is the same)

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

sort the chain according to its start.

dp[i] is the number of longest chain for ith pair, it shall be the previous chain+1

343. Integer Break

make the product the largest.

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

copy all and paste two operations

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

and it is a knapsack problem without repetition
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

delete num[i] and earn points num[i] and the same time delete num[i]-1 and num[i]+1. max the points

this is house robber with duplicates (if there are a lot of value=nums[i]). So we use bucket sort (bucket distance is 1)

516. Longest Palindromic Subsequence

again can convert to edit distance. by reversing the string we are looking for the largest common subsequence.

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

if i is the root, we have 1 to i-1 in the left and i+1 to n in the right. both are subproblem.

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
no operation dp[i-1]
sell on ith day: dp[j-2]+price[i-2]-price[j-2]. why on jth day buy in, the profit is dp[j-2] (two days ago)
Note the j iteration shall from i. 

813. Largest Sum of Averages

partition the list into at most k parts and make the sum of average the max.

assuming dp[i,k] is the max sum of average for list len=i, and k groups, then we check every j to add one more group

dp[i][k]=max(dp[i][k],dp[j][k-1]+double(A[i]-A[j])/(i-j));

764. Largest Plus Sign

in four directions, using dp to get the largest radius (including itself)

then the larget radius is the min of the 4 directions

688. Knight Probability in Chessboard

at most k moves, the probability that the knight in the board

8 directions, out of board is 0, stay in board is 1

dp[k, i, j]=sum(dp[k-1,m,n)/8 m,n is the next 8 possible directions
we can effectively remove the k dimension
```cpp
    double knightProbability(int N, int K, int r, int c) {
        int moves[][2]={{-2,-1},{-2,1},{-1,2},{-1,-2},{1,-2},{1,2},{2,-1},{2,1}};
        vector<vector<double>> dp(N,vector<double>(N,1)); //inside the board is all 1
        for(int k=0;k<K;k++) //k steps, shall have a k dimension
        {
            vector<vector<double>> curr(N,vector<double>(N));//=dp;
            for(int i=0;i<N;i++)
            {
                for(int j=0;j<N;j++)
                {
                    for(int d=0;d<8;d++)
                    {
                        int ii=i+moves[d][0];
                        int jj=j+moves[d][1];
                        if(ii>=0 && ii<N && jj>=0 && jj<N) curr[i][j]+=dp[ii][jj]; //previous times dp
                    }
                }
            }
            dp=curr; //update dp, dp is actually previous state
        }
        return double(dp[r][c])/pow(8.0,K);
    }
```

698. Partition to K Equal Sum Subsets

knapsack without repetition, need take k-1 times and mark those already used

- sort in descending order will make it faster
- dfs to choose the elements

```cpp
   bool canPartitionKSubsets(vector<int>& nums, int k) {
        int sum=accumulate(nums.begin(),nums.end(),0);
        if(sum%k) return 0;
        int target=sum/k;
        multiset<int,greater<int>> myset(nums.begin(),nums.end(),greater<int>());
        multiset<int,greater<int>>::iterator it=myset.begin();
        if(*it>target) return 0;
        for(int i=0;i<k-1;i++) //the last loop is not needed
        {
            it=myset.begin();
            if(*it==target) {myset.erase(it);continue;}
            int t=target-*it;
            myset.erase(it);
            bool res=helper(myset,t);
            if(!res) return 0;
        }
        return 1;
    }
    bool helper(multiset<int,greater<int>>& mset,int target)
    {
        auto it=mset.find(target);
        if(it!=mset.end()) {mset.erase(it);return 1;}
        //fix one element and search for other
        for(it=mset.begin();it!=mset.end();it++)
        {
            if(*it>target) continue;
            int t=target-*it;
            int val=*it;
            mset.erase(it);
            if(helper(mset,t)) return 1;
            mset.insert(val); //cannot find it restore this value
        }
        return 0;
    }
```
300. Longest Increasing Subsequence

dp[i] is the max length subsequence ending at i.

if(nums[j]<nums[i]) dp[i]=max(dp[i],dp[j]+1);


279. Perfect Square

again knapsack problem with repetition, the selection is 1^2, 2^2, 3^2....n^2
```cpp
    int numSquares(int n) {
        int target=sqrt(n);
        vector<int> dp(n+1);
        for(int w=1;w<=n;w++)
        {
            dp[w]=INT_MAX;
            for(int i=1;i<=sqrt(w);i++)
            {
                dp[w]=min(dp[w],dp[w-i*i]+1);
            }
        }
        return dp[n];
    }
```
416. Partition Equal Subset Sum

knapsack without repetition. so iteration on elements shall be outside

then solve all the smaller problems
```cpp
       vector<vector<int>> dp(target+1,vector<int>(n+1));
        for(int i=1;i<=n;i++)
        {
            int wi=nums[i-1];
            for(int w=1;w<=target;w++)
            {
                dp[w][i]=dp[w][i-1]; //do not choose wi
                if(w>=wi) dp[w][i]=max(dp[w-wi][i-1]+wi,dp[w][i]); //choose wi
            }
        }
```

474. Ones and Zeroes

with m 0s and n 1s, get the max number of string in dictionary

knapsack with more complexity
- calculate each string's 0 and 1
- solve subproblem of i 0s and j 1s
- cannot repeat the string, so string iteration shall in the outer loop
```cpp
    int findMaxForm(vector<string>& strs, int m, int n) {
        int len=strs.size();
        vector<vector<int>>num10(len,vector<int>(2));
        for(int i=0;i<len;i++)
        {
            for(int j=0;j<strs[i].size();j++) num10[i][0]+=strs[i][j]-'0';
            num10[i][1]=strs[i].size()-num10[i][0];
        }
        vector<vector<vector<int>>> dp(len+1,vector<vector<int>>(m+1,vector<int>(n+1)));
        for(int i=1;i<=len;i++)
        {
            int ones=num10[i-1][0],zeros=num10[i-1][1];
            for(int j=0;j<=m;j++) //zeros
            {
                for(int k=0;k<=n;k++) //ones
                {
                    dp[i][j][k]=dp[i-1][j][k];
                    if(j>=zeros && k>=ones) dp[i][j][k]=max(dp[i][j][k],dp[i-1][j-zeros][k-ones]+1);
                }
            }
        }
        return dp[len][m][n];
    }
```
120. Triangle

min path sum

375. Guess Number Higher or Lower II

when guess wrong, you need pay that amount

min cost to ensure win

- minmax problem, get the max to ensure win and then get the min of all the max
- when we pick a number k, it splits into [left, k-1] and [k+1,right] the cost is num[k]+max(dp[left,k-1],dp[k+1,right])
- 1st dimension depends on k+1 and second dimension depends on k-1 so first using reverse iteration and 2nd use normal iteration
```cpp
    int getMoneyAmount(int n) {
        //pick a number k it always split it into two regions (1,k-1) (k+1,n)
        vector<vector<int>> dp(n+1,vector<int>(n+1));
        
        for(int r=2;r<=n;r++) //right
        {
            for(int l=r-1;l>0;l--) //left
            {
                int gmin=INT_MAX;        
                for(int k=l+1;k<r;k++) //choose k from {l+1,r-1} need at least 3 numbers
                {
                    int tmax=k+max(dp[l][k-1],dp[k+1][r]);
                    gmin=min(gmin,tmax);
                }
                dp[l][r]=gmin==INT_MAX?l:gmin;//in case of one element
            }
        }
        return dp[1][n];
    }
```

376. Wiggle Subsequence
return the max length of the wiggling subsequence

- by tracking the number of down and ups
- when num[i]>num[i-1], we increase up, up[i]=dn[i-1]+1 (since the two are dependent)
- when num[i]<num[i-1], we increase down
- equal case: no change
- final answer shall be the max of up and down ending at the last element

264. Ugly Number II

only has factor of 2,3,5, find the nth ugly number

iteratively find the next ugly number, it shall be the min of previous *2 *3 *5

use 3 pointers i: *2, j *3, k: *5 represents the current 3 min ugly number.
```cpp
    int nthUglyNumber(int n) {
        vector<int> dp(n);
        dp[0]=1;
        int i=0,j=0,k=0;
        for(int m=1;m<n;m++)
        {
            dp[m]=min(dp[i]*2,min(dp[j]*3,dp[k]*5));
            if(dp[m]==dp[i]*2) i++;
            if(dp[m]==dp[j]*3) j++;
            if(dp[m]==dp[k]*5) k++;
        }
        return dp[n-1];
    }
```
808. Soup Servings

- make things easy, divide by 25
- then options are, A4B0, A3B1, A2B2, A1B3
- A tends to be used up first, so when A is large, the probability is 1
- recursive is fine
```cpp
    double soupServings(int N) {
        int n=(N+24)/25;
        if(n>200) return 1.0;
        vector<vector<double>> dp(n+1,vector<double>(n+1));
        return helper(n,n,dp);
    }
    double helper(int m,int n,vector<vector<double>>& dp)
    {
        if(m<=0 && n>0) return 1.0;
        if(m<=0 && n<=0) return 0.5;
        if(m>0 && n<=0) return 0.0;
        if(dp[m][n]>0) return dp[m][n];
        return dp[m][n]=0.25*(helper(m-4,n,dp)+helper(m-3,n-1,dp)+helper(m-2,n-2,dp)+helper(m-1,n-3,dp));
    }
```

935. Knight Dialer

Get the number of different numbers given n presses.

just list next numbers for all given number.

this is similar to 688. knight probability

dp[k,i] is the number of different path using k presses, i the number ending at k times (or starting is also fine)
```cpp
    int knightDialer(int N) {
       vector<vector<int>> adj={{4,6},{8,6},{7,9},{4,8},{3,9,0},{},{1,7,0},{2,6},{1,3},{4,2}} ;
        int mod=1e9+7;
        vector<vector<int>> dp(N,vector<int>(10));
        for(int i=0;i<10;i++) dp[0][i]=1; //each digit can be one
        for(int k=1;k<N;k++)
        {
            for(int i=0;i<10;i++)
            {
                for(int j=0;j<adj[i].size();j++) 
                {
                    dp[k][i]+=dp[k-1][adj[i][j]];
                    dp[k][i]%=mod;
                }
            }
        }
        int ans=0;
        for(int i=0;i<10;i++) ans+=dp[N-1][i],ans%=mod;
        return ans;
    }
```

213. House Robber II

circular, this is two house robber 1 problem
```cpp
   int rob(vector<int>& nums) {
        int n=nums.size();
        if(n==0) return 0;
        if(n==1) return nums[0];
        return max(helper(nums,0,n-1),helper(nums,1,n));
    }
    int helper(vector<int>& nums, int l, int r)
    {
        vector<int> dp(r+1);
        //dp[i]=max(dp[i-1],dp[i-2]+a[i])
        //(l==r) return nums[l];
        dp[l]=nums[l],dp[l+1]=max(nums[l+1],nums[l]);
        for(int i=l+2;i<r;i++)
        {
            dp[i]=max(dp[i-1],dp[i-2]+nums[i]);
        }
        return dp[r-1];
    }
```

790. Domino and Tromino Tiling

we have I shape and L shape, to build 2*N tile, what is the number of combinations

this is actually hard.

we have two shapes ending: one is flat at the end and one is L shaped at the end
- g(i) represents the number of combinations ending with flat
- u(i) represents the number of combinations ending with L shape

```cpp
    int numTilings(int N) 
    {
        vector<long long> g(N+1),u(N+1);
        int mod=1000000007;
        g[0]=0; g[1]=1; g[2]=2;
        u[0]=0; u[1]=1; u[2]=2;
        
        for(int i=3;i<=N;i++)
        {
            u[i] = (u[i-1] + g[i-1]           )   %mod;
            g[i] = (g[i-1] + g[i-2] + 2*u[i-2])   %mod;
        }
        return g[N]%mod;
    }
```    
368. Largest Divisible Subset

find the largest subset which Si%Sj=0 (they are a part of geometric series)

dp with backtrace ability, we need keep the previous information.

dp with a bit complexity
```cpp
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        int n=nums.size();
        if(n==0) return vector<int>();
        sort(nums.begin(),nums.end());
        
        vector<int> dp(n,1); //l
        vector<int> prev(n,-1);
        for(int i=1;i<n;i++)
        {
            for(int j=i-1;j>=0;j--)
            {
                if(nums[i]%nums[j]==0) 
                {
                    dp[i]=max(dp[i],dp[j]+1);
                    if(dp[i]==dp[j]+1) prev[i]=j;
                }
            }
        }
        int ind=max_element(dp.begin(),dp.end())-dp.begin();
        vector<int> ans;
        while(ind>=0)
        {
            ans.push_back(nums[ind]);
            ind=prev[ind];
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
```
95. Unique Binary Search Trees II

dp with backtracking. combine with left and right
```cpp
    vector<TreeNode*> genTree(int start,int end)
    {
        vector<TreeNode*> ans;
        if(start>end) {ans.push_back(NULL);return ans;}
        //if(start==end) return vector<TreeNode*>(1,new TreeNode(start));
        for(int i=start;i<=end;i++)
        {
            vector<TreeNode*> left=genTree(start,i-1);
            vector<TreeNode*> right=genTree(i+1,end);
            //combine the left right, note left or right could be empty

            for(int l=0;l<left.size();l++)
            {
                for(int r=0;r<right.size();r++)
                {
                    TreeNode *root=new TreeNode(i);    
                    root->left=left[l];
                    root->right=right[r];
                    ans.push_back(root);
                }
            }
        }
        return ans;
    }
```    

139. Word Break

break the word into dictionary words

iterate and break the problem into a word + a smaller subproblem.

direct dp:

we mark the possible cut positions and when we iterate more, we check [j, i] if is also a word in the dict.
```cpp
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> ms(wordDict.begin(),wordDict.end());
        vector<bool> dp(s.size()+1);
        dp[0]=1; //empty s
        for(int i=1;i<=s.size();i++)
        {
            for(int j=i-1;j>=0;j--)
            {
                if(dp[j])
                {
                    if(ms.count(s.substr(j,i-j))) {dp[i]=1;break;}
                }
            }
        }
        return dp[s.size()];
    }
```

467. Unique Substrings in Wraparound String
- ending with different char guarantee the uniqueness.
- ending with a char, with a length, the substring is fixed (since it is always abcde...zabcd...z
```cpp
    int findSubstringInWraproundString(string p) {
        int n=p.size();
        vector<int> dp(26);
        int maxlen=1;
        for(int i=0;i<n;i++)
        {
            int ind=p[i]-'a';
            if(i&& (p[i]==p[i-1]+1 || p[i]+25==p[i-1])) maxlen++;
            else maxlen=1;
            dp[ind]=max(dp[ind],maxlen);
        }
        return accumulate(dp.begin(),dp.end(),0);
    }
```
801. Minimum Swaps To Make Sequences Increasing

two arrays, swap at the same position to make both array sorted

two cases:
- A[i]>A[i-1] && B[i]>B[i-1]: no swap, then i-1 no swap, swap then swap i-1
- A[i]>B[i-1] && B[i]>A[i-1]: no swap, then swap i-1. swap i, then no swap i-1
- Note: the two cases may overlap, so second case we need take the min
```cpp
    int minSwap(vector<int>& A, vector<int>& B) {
        int n=A.size();
        vector<int> swap(n,INT_MAX),no_swap(n,INT_MAX); 
        //swap(n) represent min swap when we swap n
        //no_swap(n) represent min swaps when we do not swap n
        swap[0]=1; //if we swap 0
        no_swap[0]=0;
        for(int i=1;i<n;i++)
        {
            if(A[i-1]<A[i] && B[i-1]<B[i]) //adding a[i] still sorted
            {
                //if we swap i, we also need swap i-1
                swap[i]=swap[i-1]+1;
                //if we do not swap i, we also not swap i-1
                no_swap[i]=no_swap[i-1];
            }
            if(A[i-1]<B[i] && B[i-1]<A[i]) //not sorted, or sorted (there is overlapping with previous condition)
            {
                //if we swap i, we shall not swap i-1
                swap[i]=min(swap[i],no_swap[i-1]+1);
                //if we not swap i, we need swap i-1
                no_swap[i]=min(no_swap[i],swap[i-1]);
            }
        }
        return min(swap[n-1],no_swap[n-1]);
    }
```
63. Unique Paths II-easy

673. Number of Longest Increasing Subsequence

solving two dp problem at the same time: find the longest subsequence, remembering the number of path to each of the longest subsequence
```cpp
    int findNumberOfLIS(vector<int>& nums) {
        int n=nums.size();
        if(n==0) return 0;
        vector<int> dp(n,1),cnt(n,1);//each number itself is a increasing subsequence
        //cnt is the count of largest subsequence, dp is the length
        for(int i=1;i<n;i++)
        {
            for(int j=i-1;j>=0;j--)
            {
                if(nums[i]>nums[j]) 
                {
                    //dp[i]=max(dp[i],dp[j]+1);
                    if(dp[i]==dp[j]+1) cnt[i]+=cnt[j]; //j is the max, this is important!!!
                    if(dp[i]<dp[j]+1) //the new max
                    {
                        dp[i]=dp[j]+1;
                        cnt[i]=cnt[j];//update the max
                    }
                }
            }
        }
        int maxlen=*max_element(dp.begin(),dp.end());
        int ans=0;
        for(int i=0;i<n;i++) if(dp[i]==maxlen) ans+=cnt[i];
        return ans;
    }
```    

787. Cheapest Flights Within K Stops

define dp[i,k] is the min cost from start to j with at most k stops

the key is we add one stop j: dp[j,k-1]+price[i,j] to minimize the cost
```cpp
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        vector<vector<int>> prices(n,vector<int>(n,INT_MAX)),dp(n,vector<int>(K+1,INT_MAX));
        for(int i=0;i<flights.size();i++) 
        {
            if(flights[i][0]==src) dp[flights[i][1]][0]=flights[i][2];
            prices[flights[i][0]][flights[i][1]]=flights[i][2];
        }
        for(int k=0;k<=K;k++) dp[src][k]=0;
        for(int k=1;k<=K;k++)
        {
            //add one stop based on previous k-1 stop
            for(int j=0;j<n;j++)
            {
                if(src==j) continue;
                for(int m=0;m<n;m++)
                {
                    if(dp[m][k-1]!=INT_MAX && prices[m][j]!=INT_MAX)
                    dp[j][k]=min(dp[j][k],dp[m][k-1]+prices[m][j]);
                }
            }
        }
        return dp[dst][K]==INT_MAX?-1:dp[dst][K];
    }
```    

898. Bitwise ORs of Subarrays

for all subarray, bit or of all elements, return number of possible results

for input ABC
A
A|B, B
A|B|C, B|C,C
so we take two set, one for final one for intermediate
```cpp
    int subarrayBitwiseORs(vector<int>& A) {
        unordered_set<int> ms,tmp;
        for(int i=0;i<A.size();i++)
        {
            if(i && A[i]==A[i-1]) continue;
            unordered_set<int> tt;
            for(auto it=tmp.begin();it!=tmp.end();it++)
                tt.insert(*it|A[i]);
            tt.insert(A[i]);
            tmp=tt;
            ms.insert(tmp.begin(),tmp.end());
        }
        return ms.size();
    }
```

221. Maximal Square

finding the max square with all 1s

similar to histogram but we uses dp

keep updating the height on each row.

keep updating its max range at j (left and right) using the height as the min height (this could be obtained using left max and right min dp process)

```cpp
    int maximalSquare(vector<vector<char>>& matrix) {
        if(matrix.size()==0) return 0;
        int m=matrix.size(),n=matrix[0].size();
        vector<int> left(n),right(n,n),height(n);
        int max_area=0;
        for(int i=0;i<m;i++)
        {
            int curr_left=0,curr_right=n;
            for(int j=0;j<n;j++) height[j]=(matrix[i][j]=='1')?(height[j]+1):0;
            for(int j=0;j<n;j++)
            {
                if(matrix[i][j]=='0') {curr_left=j+1;left[j]=0;}
                else left[j]=max(curr_left,left[j]);
            }
            for(int j=n-1;j>=0;j--)
            {
                if(matrix[i][j]=='0') {curr_right=j;right[j]=n;} //not inclusive
                else right[j]=min(right[j],curr_right);
            }
            for(int j=0;j<n;j++)
            {
                max_area=max(max_area,min(height[j],(right[j]-left[j])));
            }
        }
        return max_area*max_area;
    }
```

576. Out of Boundary Path

quite a few similar problems. using recursion
```cpp    
    int dp[50][50][51];
    Solution() {memset(dp,-1,sizeof(dp));}
    int findPaths(int m, int n, int N, int i, int j) {
        if(i<0 || i>=m || j<0 ||j>=n)  return 1;
        if(N<=0) return 0;
        if(dp[i][j][N]!=-1) return dp[i][j][N];
        int mod=1e9+7;
        dp[i][j][N]=findPaths(m,n,N-1,i-1,j)%mod;
        dp[i][j][N]+=findPaths(m,n,N-1,i+1,j)%mod;dp[i][j][N]%=mod;
        dp[i][j][N]+=findPaths(m,n,N-1,i,j-1)%mod;dp[i][j][N]%=mod;
        dp[i][j][N]+=findPaths(m,n,N-1,i,j+1)%mod;dp[i][j][N]%=mod;
        return dp[i][j][N];
    }
```

152. Maximum Product Subarray

largest product subarray

when a negative is involved, max becomes min, min becomes max
```cpp
    int maxProduct(vector<int>& nums) {
        //max times negative becomes min, min*negative becomes max
        int imax=nums[0],imin=nums[0];
        int ans=imax;
        for(int i=1;i<nums.size();i++)
        {
            if(nums[i]<0) swap(imax,imin);
            imax=max(imax*nums[i],nums[i]);
            imin=min(imin*nums[i],nums[i]);
            ans=max(ans,imax);
        }
        return ans;
    }
```
322. Coin Change-min number

classical knapsack with repetition: the amount shall be in outer loop
```cpp
    int coinChange(vector<int>& coins, int amount) {
        //knapsack with repetitive
        int n=coins.size();
        vector<int> dp(amount+1,amount+1);//min number for amount with number of different coins
        //since coins can be repeated, the coins shall be inside loop
        dp[0]=0;
        for(int w=1;w<=amount;w++)
        {
            for(int i=1;i<=coins.size();i++)
            {
                //choose or not choose
                int m=coins[i-1];
                if(w>=m) dp[w]=min(dp[w],dp[w-m]+1);
            }
        }
        return dp[amount]==amount+1?-1:dp[amount];
    }
 ```
 837. New 21 Game

this is like climbing stairs, there are more than two methods to reach a stair
 
 ```cpp
     double new21Game(int N, int K, int W) {
        //this is like climbing stairs. ith position can be obtained
        //dp[i]=sum(dp[j])/W j=i-1 to i-W
        if(N>=K+W || K==0) return 1.0;
        vector<double> dp(K+1); //dp[i] is the probability to reach i points
        dp[0]=1.0;
        //dp[i]=sum(dp[j])/W for j=i-1 to i-w
        double wsum=0;

        for(int i=1;i<=K;i++)
        {
            if(i<=W) dp[i]=wsum+=dp[i-1]/W;
            else {dp[i]=wsum+=(dp[i-1]-dp[i-W-1])/W;}
        }
        //copy(dp.begin(),dp.end(),ostream_iterator<double>(cout," "));
        double ans=0;
        for(int i=K-1;i>=max(K-W,0);i--)
        {
            int d=K-1-i;
            int len=min(W-d,N-K+1);
            ans+=len*dp[i]/W;
        }
        return ans;
    }
```

464. Can I Win

It is important to get the problem right: the two player adds to the same sum, and who reach the target first wins
number chosen cannot be reused.

- use bitset to indicate number used or not
- subtract target
- who reaches 0 wins
- if other wins, then we lose
- store solved solutions

```cpp
    bool canIWin(int m,int sum,int status)
    {
        if(sum<=0) return 0; //already to the dead end, but still did not win
        if(win.count(status)) return 1;
        if(lose.count(status)) return 0;
        for(int i=1;i<=m;i++)
        {
            int bit=1<<i;
            if(status & bit) continue; //already solved, need the solved results recorded
            bool res=canIWin(m,sum-i,status|bit);
            if(!res) //surely win, why use ! since it is the even times.
            {
                win.insert(status);
                return 1;
            }
        }
        lose.insert(status);//after all trials, cannot win
        return 0;
    }
```

5. Longest Palindromic Substring

just naive soluion

try all position and expand

523. Continuous Subarray Sum

target: multiples of K

accumulate sum (prefix sum). the prefix sum shall %k has the same value

91. Decode Ways

A-Z decode as 1-26
given a digit string, return number of decoding ways
only have two options:
- the number itself 
- the number combine with previous digit

