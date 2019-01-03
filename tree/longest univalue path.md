### Problem summary
find the length of the longest univalue in the tree

### idea
if the longest univalue path could be in the left or right, or cross the root
so it is three subproblem, left and right is same subproblem
crossing the root is different since it must equal to root val

### code
```cpp
    int longestUnivaluePath(TreeNode* root) {
        //if val cross left and right, 
        if(!root) return 0;
        int maxleft=longestUnivaluePath(root->left);
        int maxright=longestUnivaluePath(root->right);
        int maxlen=0;
        //if(root->left && root->right && root->left->val==root->val && root->right->val==root->val)
            maxlen=helper(root->left,root->val)+helper(root->right,root->val);
        return max(maxlen,max(maxleft,maxright));
    }
    int helper(TreeNode* root,int val)
    {
        if(!root || root->val!=val) return 0;
        if(root->val==val) return 1+max(helper(root->left,val),helper(root->right,val));
    }
```

### comment
- if add the commented if statement, the result will be incorrect since it may or may not include left or right.
