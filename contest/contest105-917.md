## contest 105

Reverse Only Letters4
Maximum Sum Circular Subarray6
Complete Binary Tree Inserter7
Number of Music Playlists

917. Reverse Only Letters
<em>
Given a string S, return the "reversed" string where all characters that are not a letter stay in the same place, and all letters reverse their positions.
</em>

simple two pointer from both end

```cpp
    string reverseOnlyLetters(string S) {
      int left=0,right=S.length()-1;
      while(left<right)
      {
        while(!isalpha(S[left])) left++;
         while(!isalpha(S[right])) right--;
          if(left>=right) break;
          swap(S[left],S[right]);
          left++,right--;
      }
        return S;
    }
```

918. Maximum Sum Circular Subarray
<em>
Given a circular array C of integers represented by A, find the maximum possible sum of a non-empty subarray of C.

Here, a circular array means the end of the array connects to the beginning of the array.  (Formally, C[i] = A[i] when 0 <= i < A.length, and C[i+A.length] = C[i] when i >= 0.)

Also, a subarray may only include each element of the fixed buffer A at most once.  (Formally, for a subarray C[i], C[i+1], ..., C[j], there does not exist i <= k1, k2 <= j with k1 % A.length = k2 % A.length.)

 
</em>

get the max and min at the same time, and the answer would be max(tsum-min,max) 

```cpp
    int maxSubarraySumCircular(vector<int>& A) {
        if (A.empty()) return 0;

        int n = A.size();
        int tot = 0, ma = A[0], last_ma = 0, mi = A[0], last_mi = 0;
        for (int i = 0; i < n; i++) {
            tot += A[i];
            ma = max(ma, last_ma = A[i] + max(0, last_ma));
            mi = min(mi, last_mi = A[i] + min(0, last_mi));
        }
        return ma < 0 ? ma : max(ma, tot - mi);
    }
```

919. Complete Binary Tree Inserter
<em>
A complete binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible.

Write a data structure CBTInserter that is initialized with a complete binary tree and supports the following operations:

CBTInserter(TreeNode root) initializes the data structure on a given tree with head node root;
CBTInserter.insert(int v) will insert a TreeNode into the tree with value node.val = v so that the tree remains complete, and returns the value of the parent of the inserted TreeNode;
CBTInserter.get_root() will return the head node of the tree.
</em>

using vector to store complete binary tree.
```cpp
    vector<TreeNode*> tree;
    CBTInserter(TreeNode* root) {
        tree.push_back(root);
        for(int i = 0; i < tree.size();++i) 
        {
            if (tree[i]->left) tree.push_back(tree[i]->left);
            if (tree[i]->right) tree.push_back(tree[i]->right);        
        }
    }
    
    int insert(int v) {
        int N = tree.size();
        TreeNode* node = new TreeNode(v);
        tree.push_back(node);
        if (N % 2)
            tree[(N - 1) / 2]->left = node;
        else
            tree[(N - 1) / 2]->right = node;
        return tree[(N - 1) / 2]->val;        
    }
    
    TreeNode* get_root() {
        return tree[0];
    }
```

920. Number of Music Playlists
<em>
Your music player contains N different songs and she wants to listen to L (not necessarily different) songs during your trip.  You create a playlist so that:

Every song is played at least once
A song can only be played again only if K other songs have been played
Return the number of possible playlists.  As the answer can be very large, return it modulo 10^9 + 7.
0<=K<N<=L<=100
</em>

dp approach:
It is natural to define the problem as dp(n,l,k) with n songs, l in list, with k constraints.

We need to find how we can build the solution from sub-problems. That is the essence of DP technique.

if only N-1 songs are used in l-1 songs, then the last song can be anyone (not appeared yet)
dp[n,l,k]=n*dp[n-1,l-1,k]

if N songs are used in l-1 songs, then the previous k songs cannot be placed at the list end, which we have N-K choices
dp[n,l,k]=(n-k)*dp[n,l-1,k]

K dimension can be removed.

The answer is the sum of the two: dp[n,l,k]=n*dp[n-1, l-1, k]+(n-k)*dp[n, l-1, k], which is simplified to:

dp[n,l]=dp[n-1,l-1] * n+dp[n,l-1] * (n-k). 

Update only depends on left and up cell.

And the final answer is dp[N,L]

**Boundary condition:**
n=0, there is only 0 playlist dp[0,l]=0 but dp[0,0]=1 (empty vs empty)

since l>=n, we are only updating the upper triangle, and dp[n,n] is the boundary.

dp[n,n]=n! (when n songs and list is also n, any permutation is fine)

#### Implementation
```cpp
    int numMusicPlaylists(int N, int L, int K) {
        vector<vector<long long>> dp(N+1,vector<long long>(L+1));
        dp[0][0]=1;
        int mod=1e9+7;
        for(int i=1;i<=N;i++) dp[i][i]=(dp[i-1][i-1]*i)%mod;
        for(int n=1;n<=N;n++)
        {
            for(int l=n+1;l<=L;l++)
            {
                dp[n][l]=(n*dp[n-1][l-1])%mod;
                if(n>K) dp[n][l]+=((n-K)*dp[n][l-1])%mod;
                dp[n][l]%=mod;
            }
        }
        return dp[N][L];
    }
```
