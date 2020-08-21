## contest 125

997. Find the Town Judge.md

In a town, there are N people labelled from 1 to N.  There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

The town judge trusts nobody.
Everybody (except for the town judge) trusts the town judge.
There is exactly one person that satisfies properties 1 and 2.
You are given trust, an array of pairs trust[i] = [a, b] representing that the person labelled a trusts the person labelled b.

If the town judge exists and can be identified, return the label of the town judge.  Otherwise, return -1.

### Approach
Be sure to pay attention to special case

```cpp
    int findJudge(int N, vector<vector<int>>& trust) {
        unordered_map<int,int> mp1,mp2;
        for(int i=0;i<trust.size();i++) 
        {
            mp1[trust[i][0]]++;//he trust
            mp2[trust[i][1]]++; //he is trusted
        }
        for(auto it=mp2.begin();it!=mp2.end();it++)
        {
            if(it->second==N-1 && mp1.count(it->first)==0) return it->first;
        }
        if(N==1) return 1;
        return -1;
    }
```

N=1 is the special case. no trust and no trusted.

998. Maximum Binary Tree II.md

We are given the root node of a maximum tree: a tree where every node has a value greater than any other value in its subtree.

Just as in the previous problem, the given tree was constructed from an list A (root = Construct(A)) recursively with the following Construct(A) routine:

If A is empty, return null.
Otherwise, let A[i] be the largest element of A.  Create a root node with value A[i].
The left child of root will be Construct([A[0], A[1], ..., A[i-1]])
The right child of root will be Construct([A[i+1], A[i+2], ..., A[A.length - 1]])
Return root.
Note that we were not given A directly, only a root node root = Construct(A).

Suppose B is a copy of A with the value val appended to it.  It is guaranteed that B has unique values.

Return Construct(B).

### Approach
1. Note the value appended to B, and shall always goes to the right branch.

A not so simple solution:
```cpp
    TreeNode* insertIntoMaxTree(TreeNode* root, int val) {
        if(val>root->val) 
        {
            TreeNode* p=new TreeNode(val);
            p->left=root;
            return p;
        }
        helper(NULL,root,val,1);
        return root;
    }
    void helper(TreeNode* parent,TreeNode* root,int val,int lr)
    {
        if(!root) //leaf node
        {
            TreeNode* p=new TreeNode(val);
            if(lr==0) parent->left=p;
            else parent->right=p;
            
            return;
        }
        if(root->val<val) //need to replace the current node
        {
            TreeNode* p=new TreeNode(val);
            if(lr==0) parent->left=p;
            else parent->right=p;
            p->left=root;
            return;
        }
        if(root->val>val) //prefer the larger branch, can pick any branch
        {
            helper(root,root->right,val,1);
        }
    }
```

There are better solution which yields shorter and more intuitive solution.
1. when the value>root value, node->left=root and return node
2. when the value<root value, goes to right branch. reduced to a subproblem
```cpp
    TreeNode* insertIntoMaxTree(TreeNode* root, int val) {
        if(!root) return new TreeNode(val);
        if(val>root->val) 
        {
            TreeNode* node=new TreeNode(val);
            node->left=root;
            return node;
        }
        root->right=insertIntoMaxTree(root->right,val);
        return root;
    }
```


999. Available Captures for Rook.md

On an 8 x 8 chessboard, there is one white rook.  There also may be empty squares, white bishops, and black pawns.  These are given as characters 'R', '.', 'B', and 'p' respectively. Uppercase characters represent white pieces, and lowercase characters represent black pieces.

The rook moves as in the rules of Chess: it chooses one of four cardinal directions (north, east, west, and south), then moves in that direction until it chooses to stop, reaches the edge of the board, or captures an opposite colored pawn by moving to the same square it occupies.  Also, rooks cannot move into the same square as other friendly bishops.

Return the number of pawns the rook can capture in one move.

Simple:
```cpp
    int numRookCaptures(vector<vector<char>>& board) {
        //check 4 directions to 
        int ans=0;
        int r=0,c=0;
        for(int i=0;i<8;i++)
        {
            for(int j=0;j<8;j++) if(board[i][j]=='R') {r=i;c=j;break;}
        }
        
        if(c) for(int i=c-1;i>=0;i--) if(board[r][i]!='.') {if(board[r][i]=='p') ans++;break;}//left
        if(c<7)for(int i=c+1;i<8;i++) if(board[r][i]!='.') {if(board[r][i]=='p') ans++;break;}//right
        if(r) for(int i=r-1;i>=0;i--) if(board[i][c]!='.') {if(board[i][c]=='p') ans++;break;}//up
        if(r<7) for(int i=r+1;i<8;i++) if(board[i][c]!='.') {if(board[i][c]=='p') ans++;break;}//right
        return ans;
    }
```    

1000. Minimum Cost to Merge Stones.md

### Problem Summary
There are N piles of stones arranged in a row.  The i-th pile has stones[i] stones.

A move consists of merging exactly K consecutive piles into one pile, and the cost of this move is equal to the total number of stones in these K piles.

Find the minimum cost to merge all piles of stones into one pile.  If it is impossible, return -1.

### Analysis
1. K>1. other K has no solution
2. each time k piles reduce to 1 pile. so the number of pile must be able to divide by k-1
3. this is very similar to the burst balloon problem which is a typical dp problem.
assuming we choose ith element as the start, k is the length, it divides the left and right part. 
However, the subproblem needs cross the mid element. 
Similarly to the solution in burst balloon, we can think it in reversed way:
assuming starting i and ending j elements (some elements are already used) are the last action to merge

dp[l][i] represents the minimal cost to merge the piles in interval [i,i+l), here, we merge piles as much as possible. 
So, when l < 1+(K-1), we don't merge any piles, so dp[i][i+l] = 0; 
when 1+(K-1) <= l < 1+2(K - 1), we merge once; 
when 1+2(K-1) <= l < 1+3(K-1), we merge twice, and so on so forth.
Let's see for a certain interval length l, how can we get dp[l][i]. 
After all mergings, if we consider the leftmost pile in interval [i,i+l), then the status of this pile will show as below:

No merging happens in this pile, so it contains 1 original pile, let k = 1, then the cost is dp[k][i]+dp[l-k][i+k].
One merging happens in this pile, so it contains 1+(K-1) original piles, let k = 1+(K-1), then the cost is dp[k][i]+dp[l-k][i+k].
Two mergings happen in this pile, so it contains 1+2(K-1) original piles, let k = 1+2(K-1), then the cost is dp[k][i]+dp[l-k][i+k].
.......
When (l-1) mod (K-1)==0, we can see all piles in interval [i,i+l) can be finally merged into one pile, and the cost of the last merging is sum(stones[j]) for j in [i,i+l), regardless of the merging choices before the last one. And this "last cost" happens if and only if (l-1) mod (K-1)==0

```cpp
int mergeStones(vector<int>& stones, int K)
{
    int N = (int)stones.size();
    if((N - 1) % (K - 1)) return -1;
    
    vector<int> sum(N + 1, 0);
    for(int i = 0; i < N; i++) sum[i + 1] = sum[i] + stones[i];
    
    vector<vector<int> > dp(N + 1, vector<int>(N, 0));
    for(int l = K; l <= N; l++) //length
        for(int i = 0; i + l <= N; i++) //start position
        {
            dp[l][i] = 10000;
            for(int k = 1; k < l; k += K - 1) //
                dp[l][i] = min(dp[l][i], dp[k][i] + dp[l - k][i + k]);
            if((l - 1) % (K - 1) == 0) dp[l][i] += sum[i + l] - sum[i];
        }
    return dp[N][0];
}
```

The solution is hard to comprehend. k=1, 1+(K-1), 1+2*(K-1)....., we are looking for the min cost among all these possible choices:
dp[1+m*(k-1)][i] means the cost for the case where the left part can be merged to one pile at i (l-1)%(k-1)==0
dp[l-k][i+k] is the cost for the right part solution (merge to the left pile at i+k)
dp[l][i] then is the last merge where i to 1+m*(K-1) are all original piles (which is similar to the approach used in the burst balloon)
(we can also try the right piles to merge)

why we add the (l-1)%(K-1)==0 case? i.e l=1+m*(K-1) (the above loop does not finish the last merge) and the last merge is fixed.
so the approach is considering the subproblem starting at i with length l as the last step of merging.


