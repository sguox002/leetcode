# Tree
Generally tree is O(n) complexity (visiting each node just once).

## easy
easy tree problem generally involves only one recursive problem. Consider its subtree a similar problem and finally down to a single node.

## rating: *
### 589. N-ary Tree Preorder Traversal
recursive. visit root first, and then child.
Although we can use root+child vector, it is not very efficient. Use a helper function with the building vector is better.

```cpp
    vector<int> preorder(Node* root) {
        //node first
        vector<int> ans;
        helper(root,ans);
        return ans;
    }
    void helper(Node* root,vector<int>& ans)
    {
        if(!root) return;
        ans.push_back(root->val);
        for(auto t: root->children)
            helper(t,ans);
    }
```	
### 590. N-ary Tree Postorder Traversal: recursive
similar to 589
```cpp
    vector<int> postorder(Node* root) {
        vector<int> ans;
        helper(root,ans);
        return ans;
    }
    void helper(Node* root,vector<int>& ans)
    {
        if(!root) return;
        for(auto t: root->children)
            helper(t,ans);
        ans.push_back(root->val);
    }
```

### 700. Search in a Binary Search Tree
search in a bst is log(n)

```cpp
    TreeNode* searchBST(TreeNode* root, int val) {
        if(!root) return 0;
        if(val==root->val) return root;
        if(val>root->val) return searchBST(root->right,val);
        return searchBST(root->left,val);
    }
```	

### 559. Maximum Depth of N-ary Tree
depth=1+max(children depth), for the node, it is depth=1. (otherwise will be correct)
```cpp    
	int maxDepth(Node* root) {
        if(!root) return 0;
        int ans=1;
        for(auto t: root->children)
            ans=max(ans,1+maxDepth(t));
        return ans;
    }
```

### 872. Leaf-Similar Trees
recursively get the leaf and construct a string or array
from left to right, the inorder can form the sequence (dfs like search)
```cpp
    bool leafSimilar(TreeNode* root1, TreeNode* root2) {
        vector<int> v1,v2;
        inorder(root1,v1);
        inorder(root2,v2);
        return v1==v2;
    }
    void inorder(TreeNode* root,vector<int>& v)
    {
        if(!root) return;
        if(root->left) inorder(root->left,v);
        if(!root->left && !root->right) v.push_back(root->val);
        if(root->right) inorder(root->right,v);
    }
```

### 104. Maximum Depth of Binary Tree..same as 559
inorder traverse
```cpp
    int maxDepth(TreeNode* root) {
       //traverse to get the max depth 
        int maxh=0;
        inorder(root,1,maxh);
        return maxh;
    }
    void inorder(TreeNode* root,int h,int& maxh)
    {
        if(!root) return;
        
        inorder(root->left,h+1,maxh);
        maxh=max(h,maxh);
        inorder(root->right,h+1,maxh);
    }
```
more concise:
```cpp
    int maxDepth(TreeNode* root) {
       //traverse to get the max depth 
        if(!root) return 0;
        return 1+max(maxDepth(root->left),maxDepth(root->right));
    }
```
	
### 429. N-ary Tree Level Order Traversal: bfs
be sure to deal with null
```cpp
    vector<vector<int>> levelOrder(Node* root) {
        queue<Node*> q;
        vector<vector<int>> ans;
        if(!root) return ans;
        q.push(root);
        
        while(q.size())
        {
            int n=q.size();
            ans.push_back({});
            for(int i=0;i<n;i++)
            {
                Node* node=q.front();q.pop();
                ans.back().push_back(node->val);
                for(int j=0;j<node->children.size();j++) q.push(node->children[j]);
            }
        }
        return ans;
    }
```
We can also use inorder or any order traverse to form a depth vs nodes data structure and then get the answer.
```cpp
    vector<vector<int>> levelOrder(Node* root) {
        map<int,vector<int>> mp;
        inorder(root,0,mp);
        vector<vector<int>> ans;
        for(auto t: mp) ans.push_back(t.second);
        return ans;
    }
    void inorder(Node* root,int h,map<int,vector<int>>& mp)
    {
        if(!root) return;
        mp[h].push_back(root->val);
        for(auto t: root->children)
            inorder(t,h+1,mp);
    }
```
	
## rating: **	
### 617. Merge Two Binary Trees
recursive. reduce to left and right subtree and a single node.

```cpp
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if(!t1 && !t2) return 0;
        if(!t1) return t2;
        if(!t2) return t1;
        t1->val+=t2->val;
        t1->left=mergeTrees(t1->left,t2->left);
        t1->right=mergeTrees(t1->right,t2->right);
        return t1;
    }
```

### 965. Univalued binary tree
this involves the compare of two nodes: root with left, root with right. So the smallest problem is:
single node: true
node with one or two child: compare

```cpp
    bool isUnivalTree(TreeNode* root) {
        if(root->left)
        {
            if(root->val!=root->left->val || !isUnivalTree(root->left))
                return 0;
        }
        if(root->right)
        {
            if(root->val!=root->right->val || !isUnivalTree(root->right))
                return 0;
        }
        return 1;
    }
```	

### 637. Average of Levels in Binary Tree
similar to 429, using a map to record the level or level traversal (bfs)
```cpp
    vector<double> averageOfLevels(TreeNode* root) {
        unordered_map<int,vector<long long>> mp; //level vs sum,cnt pair
        preorder(root,0,mp);
        vector<double> ans(mp.size());
        for(auto t: mp)
            ans[t.first]=double(t.second[0])/t.second[1];
        return ans;
    }
    void preorder(TreeNode* root,int h,unordered_map<int,vector<long long>>& mp)
    {
        if(!root) return;
        if(mp.count(h)) mp[h][0]+=root->val,mp[h][1]++;
        else mp[h]={root->val,1};
        preorder(root->left,h+1,mp);
        preorder(root->right,h+1,mp);
    }
```	

### 226. Invert Binary Tree
swap the left and right
first swap the left sub and right sub, then swap the left and right
```cpp
    TreeNode* invertTree(TreeNode* root) {
        //use recursion to invert the tree
        if(!root) return 0;
        root->left=invertTree(root->left);
        root->right=invertTree(root->right);
        swap(root->left,root->right);
        return root;
    }
```

### 653. Two Sum IV - Input is a BST
traversal with set/hashset
```cpp
    bool findTarget(TreeNode* root, int k) {
        //traversal and keep a hashmap
        unordered_set<int> ms;
        return traverse(root,ms,k);
    }
    bool traverse(TreeNode* root,unordered_set<int>& ms,int k)
    {
        if(!root) return 0;
        if(ms.count(k-root->val)) return 1;
        ms.insert(root->val);
        return traverse(root->left,ms,k) || traverse(root->right,ms,k);
    }
```

### 606. Construct String from Binary Tree
need remove unnecessary (), preorder traversal
```cpp
    string tree2str(TreeNode* t) {
        string ans;
        if(!t) return ans;
        ans+=to_string(t->val);
        if(t->right || t->left) ans+="("+tree2str(t->left)+")"; //when 
        if(t->right) ans+="("+tree2str(t->right)+")";
        return ans;
    }
```

### 404. Sum of Left Leaves
two: find leaves, and must be left, so we can flag if it is left or right
or using root->left->left, root->right->left
```cpp
    int sumOfLeftLeaves(TreeNode* root) {
        if(!root) return 0;
        int ans=0;
        if(root->left && root->left->left==0 && root->left->right==0) ans+=root->left->val;
        ans+=sumOfLeftLeaves(root->left);
        ans+=sumOfLeftLeaves(root->right);
        return ans;
    }
```
	
### 100. Same Tree
compare its subtree and the root
```cpp
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(!p && !q) return 1;
        if(!p || !q) return 0;
        return p->val==q->val && isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
    }
```

### 107. Binary Tree Level Order Traversal II
from bottom up: we can still use up down and reverse the answer.
preorder: to get the root first and store in the vector
```cpp
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
       vector<vector<int>> ans;
        preorder(root,0,ans);
        reverse(ans.begin(),ans.end());
        return ans;
    }
    void preorder(TreeNode* root,int h,vector<vector<int>>& ans)
    {
        if(!root) return;
        if(ans.size()<h+1) ans.push_back({root->val});
        else ans[h].push_back(root->val);
        preorder(root->left,h+1,ans);
        preorder(root->right,h+1,ans);
    }
```	


## rating: ***

### 897. Increasing Order Search Tree
convert the inorder sequence into a only right tree
The leftmost node is now the root.
add a previous node, so the previous's right point to the current node.
current node's left point to null
this needs a bit more thinking.

```cpp
    TreeNode *pre=0,*hd=0;
    TreeNode* increasingBST(TreeNode* root) {
        inorder(root);
        return hd;
    }
    void inorder(TreeNode* root)
    {
        if(!root) return;
        inorder(root->left);
        if(!pre) hd=root;
        else {pre->right=root;}
        pre=root;
        root->left=0;
        inorder(root->right);
    }
```

### 669. Trim a Binary Search Tree
trim BST so value in range [L,R]
if node<L, then its left shall be trimed, go to right
if node>R, the its right shall be trimed, go to left
```cpp
    TreeNode* trimBST(TreeNode* root, int L, int R) {
        //bst: if root in range, trim left and right
        //if root<L, trim right (all left are out of range)
        //if root>R, trim left (all right are out of range)
        if(!root) return 0;
        if(root->val<L) return trimBST(root->right,L,R);
        else if(root->val>R) return trimBST(root->left,L,R);
        else
        {
            root->left=trimBST(root->left,L,R);
            root->right=trimBST(root->right,L,R);
        }
        return root;
    }
```

### 993. Cousins in Binary Tree
cousins have same depth and different parents
so one intuitive method is to get the height and the parent.
```cpp
    bool isCousins(TreeNode* root, int x, int y) {
        //find depth and its parent
        int hx=0,hy=0;
        TreeNode* xparent=get_depth(root,x,0,hx);
        TreeNode* yparent=get_depth(root,y,0,hy);
        return hx==hy && xparent!=yparent;
    }
    TreeNode* get_depth(TreeNode* root,int x,int depth,int& h)
    {
        if(!root) return 0;
        if((root->left && root->left->val==x) ||(root->right && root->right->val==x))
        {h=depth+1;return root;}
        TreeNode* left=get_depth(root->left,x,depth+1,h);
        if(left) return left;
        TreeNode* right=get_depth(root->right,x,depth+1,h);
        if(right) return right;
        return 0;
    }
```

or use bfs so we can make sure they are on the same level.

	
### 538. Convert BST to Greater Tree (**)
Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.
assuming we have in order array, the question is pretty simple: it is the postfix sum of all its behind numbers
trying a reverse in-order traverse and do the sum in place (postorder is not accurate)
```cpp
    TreeNode* convertBST(TreeNode* root) {
        int prev=INT_MIN;
        postorder(root,prev);
        return root;
    }
    void postorder(TreeNode* root,int& prev)
    {
        if(!root) return;
        postorder(root->right,prev);
        if(prev!=INT_MIN) root->val+=prev;
        prev=root->val;
        postorder(root->left,prev);
    }
```	

### 1022. Sum of Root To Leaf Binary Numbers
preorder traverse

```cpp
	int sumRootToLeaf(TreeNode* root) {
		int sum=0;
		helper(root,0,sum);
		return sum;
	}
	void helper(TreeNode* root,int sum,int& ans)
	{
		if(!root) return;
		sum=sum*2+root->val;
		if(!root->left && !root->right) ans+=sum;
		helper(root->left,sum,ans);
		helper(root->right,sum,ans);
	}
```
	
```cpp
    int sumRootToLeaf(TreeNode* root, int val = 0) {
        if (!root) return 0;
        val = (val * 2 + root->val);
        return root->left == root->right ? val : sumRootToLeaf(root->left, val) + sumRootToLeaf(root->right, val));
    }
```	

### 257. Binary Tree Paths
dfs with tracing
```cpp
		vector<string> s;
		string path;
		vector<string> binaryTreePaths(TreeNode* root) {
			if(!root) return s;
			DFS(root, path);
			return s;
		}
	
		void DFS(TreeNode *root, string path) 
        {
		    if(!root) return;
            path.append(to_string(root->val)+"->");
            if(!root->right && !root->left) 
            {
                path.pop_back(); //remove >
                path.pop_back(); //remove -
                s.push_back(path);
            }
            else 
            {
                DFS(root->left, path);
                DFS(root->right, path);
            }
		}
```
Note: we need pop twice to remove the left and right node to go back to the root.
or to avoid the removal of last two chars.
```cpp
    vector<string> binaryTreePaths(TreeNode* root) {
       vector<string> vs;
        string s;
        dfs(root,vs,s);
        return vs;
    }
    void dfs(TreeNode* root,vector<string>& vs,string s)
    {
        if(!root) return;
        if(!root->left && !root->right)
        {
            s+=to_string(root->val);
            vs.push_back(s);
            return; 
        }
        s+=to_string(root->val)+"->";
        dfs(root->left,vs,s);
        dfs(root->right,vs,s);
    }
```	

### 671. Second Minimum Node In a Binary Tree
Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly two or zero sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes.
Given such a binary tree, you need to output the second minimum value in the set made of all the nodes' value in the whole tree.
If no such second minimum value exists, output -1 instead.
traversal and put in a min heap. (does not use the fact provided in the question)
the root is the min, we just need to find the first one who is bigger than the root.
```cpp
    int findSecondMinimumValue(TreeNode* root) {
        if(!root) return -1;
        int ans=INT_MAX;
        traverse(root,root->val,ans);
        return ans==INT_MAX?-1:ans;
    }
    void traverse(TreeNode* root,int v,int& ans)
    {
        if(!root) return;
        if(root->val>v) ans=min(ans,root->val);
        traverse(root->left,v,ans);
        traverse(root->right,v,ans);
    }
```
	
### 235. Lowest Common Ancestor of a Binary Search Tree
R>node>L, the node is the parent
node>R, goes to left
node<L: goes to right
```cpp
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
       if(root->val>p->val && root->val>q->val) 
           return lowestCommonAncestor(root->left,p,q);
        if(root->val<p->val && root->val<q->val) 
           return lowestCommonAncestor(root->right,p,q);
        return root;
    }
```
	
### 101. Symmetric Tree (**)
note: it only needs left and right subtree under root to be symmetric
check if node is the same and left's left=right's right,  and left's right=right's left and this is recursively true
```cpp
    bool isSymmetric(TreeNode* root) {
        if(!root) return 1;
        return isSymmetric(root->left,root->right);
    }
    bool isSymmetric(TreeNode* root1,TreeNode* root2)
    {
        if(!root1 && !root2) return 1;
        if(!root1 || !root2) return 0;
        return root1->val==root2->val &&
            isSymmetric(root1->left,root2->right) &&
            isSymmetric(root1->right,root2->left);
    }
```


### 572. Subtree of Another Tree
check if s is a subtree of t.
node=node, left subtree=left subtree, right subtree=right subtree
again two recursive problem:
recursively check all subtree with t
another: recursively check two trees are the same
```cpp
    bool isSubtree(TreeNode* s, TreeNode* t) {
      if(!s && !t) return 1;
      if(!s && t) return 0;
      if(is_same(s,t)) return 1;
      return isSubtree(s->left,t) || isSubtree(s->right,t);
    }
    
    bool is_same(TreeNode *s, TreeNode* t)
    {
        if(!s && !t) return 1;
        if((!s && t) || (s&&!t)) return 0;
        if(s->val!=t->val) return 0;
        return is_same(s->left,t->left) && is_same(s->right,t->right);
    }
```

### 110. Balanced Binary Tree
check if tree left and right height difference <=1
one recursive: the height of a tree
```cpp
    bool isBalanced(TreeNode* root) {
       if(!root)  return 1;
        return abs(height(root->left)-height(root->right))<=1 &&
            isBalanced(root->left) && isBalanced(root->right);
    }
    int height(TreeNode* root)
    {
        if(!root) return 0;
        return max(height(root->left),height(root->right))+1;
    }
```	

### 501. Find Mode in Binary Search Tree
most frequency element, allows duplicate
left <=node, right>=node
in order traversal will give the number of duplicate number
using a map to record element vs frequency
```cpp
    vector<int> findMode(TreeNode* root) {
       int maxcnt=INT_MIN;
        unordered_map<int,int> mp;
        inorder(root,mp,maxcnt);
        vector<int> ans;
        for(auto it: mp)
            if(it.second==maxcnt) ans.push_back(it.first);
        return ans;
    }
    void inorder(TreeNode* root,unordered_map<int,int>& mp,int& maxcnt)
    {
        if(!root) return;
        inorder(root->left,mp,maxcnt);
        mp[root->val]++;
        maxcnt=max(maxcnt,mp[root->val]);
        inorder(root->right,mp,maxcnt);
    }
```
	
### 112. Path Sum
from root to leaf sum to target
dfs 
```cpp
    bool hasPathSum(TreeNode* root, int sum) {
        if(!root) return 0;
        sum-=root->val;
        if(!root->left && !root->right) return sum==0;
        if(root->left && hasPathSum(root->left,sum)) return 1;
        if(root->right && hasPathSum(root->right,sum)) return 1;
        return 0;
    }
```
	
### 111. Minimum Depth of Binary Tree
dfs to get the shortest path
```cpp
    int minDepth(TreeNode* root) {
        if(!root) return 0;
        int minh=INT_MAX;
        inorder(root,1,minh);
        return minh;
    }
    
    void inorder(TreeNode* root,int h,int& minh)
    {
        //if(!root) return;
        if(root->left) inorder(root->left,h+1,minh);
        if(!root->left && !root->right) minh=min(h,minh);
        if(root->right) inorder(root->right,h+1,minh);
    }
```
	
## rating ****	

### 108. Convert Sorted Array to Binary Search Tree
need to be a balanced tree. 
split the array in balanced way and recursively build the subtree
```cpp
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return helper(nums,0,nums.size()-1);
    }
    TreeNode* helper(vector<int>& nums,int l,int r)
    {
        if(l>r) return 0;
        int mid=(l+r)/2;
        TreeNode* root=new TreeNode(nums[mid]);
        root->left=helper(nums,l,mid-1);
        root->right=helper(nums,mid+1,r);
        return root;
    }
```
	
### 563. Binary Tree Tilt (**)
Given a binary tree, return the tilt of the whole tree.
The tilt of a tree node is defined as the absolute difference between the sum of all left subtree node values and the sum of all right subtree node values. Null node has tilt 0.
The tilt of the whole tree is defined as the sum of all nodes' tilt.
This is not so simple.
it involves: a subtree sum (it is a recursive problem)
tilt of a node
sum of all node's tilt
```cpp
    int findTilt(TreeNode* root) {
        //post-order traversal to get the tilt
        if(!root) return 0;
        int sum=0;
        sum+=findTilt(root->left)+findTilt(root->right);
        int l=0,r=0;
        if(root->left) l=treesum(root->left);//all left branch sum
        if(root->right) r=treesum(root->right);
        sum+=abs(l-r);
        return sum;
    }
    int treesum(TreeNode* root)
    {
        if(!root) return 0;
        int sum=0;
        sum=treesum(root->left)+treesum(root->right)+root->val;
        return sum;
    }
```

### 543. Diameter of Binary Tree (**)
diameter is the longest distance between two nodes
again this is not so straightforward.
- if two node need go through the root, ldepth+rdepth (a node's depth problem)
- if not go through root, choose lmax and rmax (a node's diameter subproblem)
```cpp
    int m_maxDia = 0;
    int diameterOfBinaryTree(TreeNode* root) {
        GetDepth(root);
        return m_maxDia;
    }
    int GetDepth(TreeNode* n)
    {
        if(!n) return 0;
         int left = GetDepth(n->left);
        int right = GetDepth(n->right);
        m_maxDia = max(m_maxDia, left + right);
        return max(left, right) + 1;
    }
```
	
### 437. Path Sum III (**)
this is called range sum. 
first we need use any node as the starting since it can be the range start. (recursively using all nodes)
second, for starting with a given node, we start from 0 and do dfs to search all path sum = target sum.
```cpp
    int pathSum(TreeNode* root, int sum) {
        //for each node do the dfs search to check the path sum
        //traversal all nodes, do dfs search
        if(!root) return 0;
        int cnt=dfs_sum(root,sum,0);
        cnt+=pathSum(root->left,sum);
        cnt+=pathSum(root->right,sum);
        return cnt;        
    }
    int dfs_sum(TreeNode* root,int sum,int prevsum) //inorder, preorder or post-order are all depth first search
    {
        if(!root) return 0;
        prevsum+=root->val;
        return (prevsum==sum)+dfs_sum(root->left,sum,prevsum)+dfs_sum(root->right,sum,prevsum);
    }
```    

## medium

### 654. Maximum Binary Tree

Given an integer array with no duplicates. A maximum tree building on this array is defined as follow:
The root is the maximum number in the array.
The left subtree is the maximum tree constructed from left part subarray divided by the maximum number.
The right subtree is the maximum tree constructed from right part subarray divided by the maximum number.
Construct the maximum tree by the given array and output the root node of this tree.

recursively find the max and divide into left and right
```cpp
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return helper(nums,0,nums.size());
    }
    TreeNode* helper(vector<int>& nums,int l,int r)
    {
        if(l>=r) return 0;
        if(l+1==r) return new TreeNode(nums[l]);
        int it=max_element(nums.begin()+l,nums.begin()+r)-nums.begin();
        TreeNode* root=new TreeNode(nums[it]);
        root->left=helper(nums,l,it);
        root->right=helper(nums,it+1,r);
        return root;
    }
```
	
### 701. Insert into a Binary Search Tree

the value > root, goes to right, otherwise goes to left
create a leaf node.
this shall be easy
```cpp
    TreeNode* insertIntoBST(TreeNode* root, int val) {
		if(!root) return new TreeNode(val);
		if(val<root->val) root->left=insertIntoBST(root->left,val);
		else root->right=insertIntoBST(root->right,val);
		return root;
    }
```	

### 1008. Construct Binary Search Tree from Preorder Traversal
preorder: root left right
root val >left root->val<right
```cpp
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        return helper(preorder,0,preorder.size());
    }
	TreeNode* helper(vector<int>& preorder,int l,int r)
	{
		if(l>=r) return 0;
		if(l+1==r) return new TreeNode(preorder[l]);
		TreeNode* root=new TreeNode(preorder[l]);
		int ind=upper_bound(preorder.begin()+l+1,preorder.begin()+r,preorder[l])-preorder.begin();
		root->left=helper(preorder,l+1,ind);
		root->right=helper(preorder,ind,r);
		return root;
		
	}
```	
note: we can use upper_bound since it satisfy 0,0,...1,1,1,...1 conditions.

### 814. Binary Tree Pruning

all 0 and 1, prune all subtree not containing 1
we can use the sum=0 or not 0 to prune the left or right.
or recursively prune the leaf nodes (this shall be the correct way)
post order traversal ( left and right branch first)
```cpp
    TreeNode* pruneTree(TreeNode* root) {
        if(!root) return 0;
		if(root->left)
		{
			if(!root->left->left && !root->left->right && !root->left->val) 
				root->left=0;
			else root->left=pruneTree(root->left);
		}
		if(root->right)
		{
			if(!root->right->left && !root->right->right && !root->right->val) 
				root->right=0;
			else root->right=pruneTree(root->right);
		}
		if(!root->val && !root->left && !root->right) return 0; //don't forget this to complete the postorder traversal
		return root;
    }
```	
### 894. All Possible Full Binary Trees (**)

all node is 0, return all possible full binary tree
for a node: we have 0 or 2 children until n node is exhausted.
again we have two recursion:
n nodes, i for left and n-i for right, which is a n combinations of subproblem
for each left m, and right n nodes, there are mxn combinations.
don't forget it shall have zero or two children.

```cpp
    vector<TreeNode*> allPossibleFBT(int N) {
		vector<TreeNode*> ans;
        if(N%2==0) return ans;
		if(!N) {ans.push_back({0});return ans;}
		if(N==1) ans.push_back({new TreeNode(0)});
		for(int i=1;i<N;i+=2) //left and right are all odd numbers
		{
			vector<TreeNode*> left=allPossibleFBT(i);
			vector<TreeNode*> right=allPossibleFBT(N-i-1);
			for(auto l: left)
			{
				for(auto r: right)
				{
					TreeNode* root=new TreeNode(0);
					root->left=l;
					root->right=r;
					ans.push_back(root);
				}
			}
		}
		return ans;
	}
```
1. even number of nodes are impossible
2. smallest solution is 1 node
	
### 979. Distribute Coins in Binary Tree	
tree has initial value of coins, number of moves to make each node has exactly one coins
approach: 
for example 3, 0, 0. we know the left node needs one from parent, and right node needs one from parent.
thus we need two moves.
two things: moves are the abs value, and the sum is not.
we can do post order traverse.

```cpp
    int distributeCoins(TreeNode* root) {
		int ans=0;
		helper(root,0,ans);
		return ans;
    }
	int helper(TreeNode* root,int& moves)
	{
		if(!root) return 0;
		int lsum=helper(root->left,moves);
		int rsum=helper(root->right,moves);
		int sum=lsum+rsum+root->val-1;
		moves+=abs(lsum)+abs(rsum);//from the child to current node
		return sum;
	}
```
The key idea is from bottom to top only count the moves from child to parent (does not count from node to parent)

### 951. Flip Equivalent Binary Trees

flip equivalent, also used for scrambling the string
flip left and right
left1=left2 and right1=right2
left1=right1 and right1=left2
finally down to a single node.

```cpp
    bool flipEquiv(TreeNode* root1, TreeNode* root2) {
        if(!root1 && !root2) return 1;
		if(!root1 || !root2) return 0;
		if(root1->val!=root2->val) return 0;
		return (flipEquiv(root1->left,root2->left) && flipEquiv(root1->right,root2->right)) ||
				(flipEquiv(root1->left,root2->right) && flipEquiv(root1->right,root2->left);
    }
```	
### 513. Find Bottom Left Tree Value

Given a binary tree, find the leftmost value in the last row of the tree.
It is easy to give the tree a x and y coordinate.
another: use the height, if left is taller, find it in left, otherwise in right
The last row has a depth of 1

### 515. Find Largest Value in Each Tree Row

bfs or hashmap using layer while traverse

### 889. Construct Binary Tree from Preorder and Postorder Traversal (**)

two arrays
pre: root left right
post: left right root
pick the first as the root from pre. then if we can split the array into left and right, each is then a subproblem
For two subarrays pre[a,b] and post[c,d], if we want to reconstruct a tree from them, we know that pre[a]==post[d] is the root node.

[root][......left......][...right..]  ---pre
[......left......][...right..][root]  ---post
pre[a+1] is the root node of the left subtree.
Find the index of pre[a+1] in post, then we know the left subtree should be constructed from pre[a+1, a+idx-c+1] and post[c, idx]

### 919. Complete Binary Tree Inserter

A complete binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible.
complete binary tree can use array, and each one has strict index 2*n+1 and 2*n+2 for parent node n

### 94. Binary Tree Inorder Traversal- iterative

left node right
put the node in stack and goes to left until no left, and then pop, go to the right

### 865. Smallest Subtree with all the Deepest Nodes

Return the node with the largest depth such that it contains all the deepest nodes in its subtree.
calculate each node's depth

### 508. Most Frequent Subtree Sum

a subtree sum=root+leftsum+rightsum
create a map

### 655. Print Binary Tree

a way to display the tree

### 144. Binary Tree Preorder Traversal-iterative

node, left, right
keep pushing the right into stack

### 684. Redundant Connection (**)

undirected graph
remove an edge to make it a tree
this is not so straightforward
using disjoint set to combine into n-ary tree.
if we found the vertices of an edge belongs to the same parent, the edge is redundant.
```cpp
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n=edges.size();
        vector<int> parents(2*n);
        for(int i=0;i<2*n;i++) parents[i]=i;
        for(int i=0;i<n;i++)
        {
            int f=edges[i][0],t=edges[i][1];
            if(find(parents,f)==find(parents,t)) return edges[i];
            else parents[find(parents,f)]=find(parents,t);
        }
        return vector<int>(2);
    }
    int find(vector<int>& parent,int f)
    {
        while(f!=parent[f]) f=parent[f];
        return f;
    }
```

### 230. Kth Smallest Element in a BST

inorder traversal

### 623. Add One Row to Tree

add a row of value at the given depth
bfs to find the row with the depth and replace the row and add those original as child

### 337. House robber III

dynamic with tree

### 102. Binary Tree Level Order Traversal

queue
or use left first and then right, with layer index, dfs is fine

### 199. Binary Tree Right Side View

basically it needs the rightmost node for each layer
bfs or dfs to store layer index and the last one in the layer

### 173. Binary Search Tree Iterator

next: next smallest
has_next:
next smallest is always the leftmost node, so using a stack to store those nodes in inorder sequence

### 449. Serialize and Deserialize BST

serialize to a string and deserialize the string into a BST
serialize output to a stringstream
deserialize input from a stringstream
use a # speical char for the null node which is more convenient
the following code uses preorder
```cpp
    void serialize(TreeNode* root, ostringstream& os)
    {
        if(root)
        {
            os<<root->val<<" ";
            serialize(root->left,os);
            serialize(root->right,os);
        }
        else os<<"# ";
    }
    TreeNode* deserialize(istringstream& is)
    {
        string s;
        is>>s;
        if(s=="#") return 0;
        TreeNode* p=new TreeNode(stoi(s));
        p->left=deserialize(is);
        p->right=deserialize(is);
        return p;
    }
```
### 863. All Nodes Distance K in Binary Tree

find all nodes with distance to k
one method: convert the tree to an undirectional graph and then using bfs

### 96. Unique Binary Search Trees

1 to N, how many unique BST
first 1 to n can be the root. Suppose i is the root
and it divides 1 to i-1 in the left and i+1 to n in the right. the total number is m=L*R
this is a dp problem: dp[i]=sum(dp[j-1]*dp[i-j]) j from 1 to i

### 652. Find Duplicate Subtrees

first we need to know how to judge two trees are equal
then we need compare each nodes (traverse)
The way: convert a tree into a string and store into a hashset or map. Then we traverse and check if the string already appeared.

### 129. Sum Root to Leaf Numbers

each path represents a number
dfs and build the number and sum on fly
```cpp
    void sumtree(TreeNode* root,long long num,long long & sum)
    {
        //root is valid
        num=num*10+root->val;
        if(root->left) sumtree(root->left,num,sum);//if root is leaf will call twice
        if(root->right) sumtree(root->right,num,sum);
        if(!root->left && !root->right) {sum+=num;return;}
    }
```
### 114. Flatten Binary Tree to Linked List

flatten the tree in-place (preorder)
first have the root, and connect its flattened left subtree and its right flattened subtree.
so it is recursive
```cpp
    void flatten(TreeNode* root) {
        //recursively flatten a tree using in-order traversal
        //flatten the left and the right, connect using root->left->right
        if(!root) return;
        flatten(root->left);
        flatten(root->right);
        TreeNode* right = root->right;
 
        if(root->left) 
        {

             // step 1: concatinate root with left flatten subtree
            root->right = root->left;
            root->left = 0; // set left to null

            // step 2: move to the end of new added flatten subtree
            while(root->right)
                root = root->right;

            // step 3: contatinate left flatten subtree with flatten right subtree	
            root->right = right;
        }   
    }
```

### 958 check completeness of a binary tree

level traversal and push all its child into queue including null. Then we found a null, make sure there is no more non-null nodes

### 103. Binary Tree Zigzag Level Order Traversal

level by level and then reverse all even layers

### 662. Maximum Width of Binary Tree

the left right largest difference
can use full binary tree array storage

### 450. Delete Node in a BST

there is standard operations on deleting a node
see some algorithm text coursera

Recursively find the node that has the same value as the key, while setting the left/right nodes equal to the returned subtree
Once the node is found, have to handle the below 4 cases
- node doesn't have left or right - return null
- node only has left subtree- return the left subtree
- node only has right subtree- return the right subtree
- node has both left and right - find the minimum value in the right subtree, set that value to the currently found node, then recursively delete the minimum value in the right subtree

### 113. Path Sum II

find the root to leaf path sum=target
dfs backtracking problem
```cpp
    void dfs(TreeNode* root,int sum,vector<int> path,vector<vector<int>>& res)
    {
        if(!root) return;
        if(!root->left && !root->right) //leaf node
        {
            if(sum==root->val) {path.push_back(sum);res.push_back(path);}
        }
        path.push_back(root->val);
        dfs(root->left,sum-root->val,path,res);
        dfs(root->right,sum-root->val,path,res);
    }

```

### 105. Construct Binary Tree from Preorder and Inorder Traversal

pre: root left right
in: left root right
get the root from the preorder and divide left and right
do it recursively

### 106. Construct Binary Tree from Inorder and Postorder Traversal

inorder: left root right
post: left right root
similarly get the root from post and then divide into left and right

### 116. Populating Next Right Pointers in Each Node

next: the next right node.
using the next we have a mechanism to do level traversal, and for each layer we directly update its children. next is used for this layer's traversal

next is null the layer is done

### 95. Unique Binary Search Trees II

instead get the number need get the tree
need to use the combination of left and right
```cpp
    void gentree(int l,int r,vector<TreeNode*>& res) //generate a bst number from l to r including r
    {
        if(l>r) {res.push_back(0);return;} //empty node
        if(l==r) {res.push_back(new TreeNode(l));return;} //leaf node
        for(int i=l;i<=r;i++)
        {
            vector<TreeNode*> vleft,vright;
            gentree(l,i-1,vleft); //left tree subproblem
            gentree(i+1,r,vright); //right tree subproblem
            for(int j=0;j<vleft.size();j++)
            {
                for(int k=0;k<vright.size();k++)
                {
                    TreeNode* root=new TreeNode(i);
                    root->left=vleft[j];
                    root->right=vright[k];
                    res.push_back(root);
                }
            }
        }
      }
```

### 236. Lowest Common Ancestor of a Binary Tree

two given nodes, not a BST
1. can we find the two nodes under root, that is the root
2. can we find the two nodes under left, subproblem on left
3. can we find the two nodes under right, subproblem on right

0: no p or q
1: p
2: q
3: p, q

```cpp
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        //traversal until the two nodes are in each side or one is the root node
        //preorder traversal: root first and then left and right branch
        if(!root) return 0;
        //check if root=p or q
        if(root==p || root==q) return root;
        int res1=contain_nodes(root->left,p,q);
        int res2=contain_nodes(root->right,p,q);
        if(res1 && res2) return root;
        if(res1) return lowestCommonAncestor(root->left,p,q);
        return lowestCommonAncestor(root->right,p,q);
    }
    int contain_nodes(TreeNode* root,TreeNode* p,TreeNode* q) //none: 0, p,1, q,2, p,q:4
    {
        if(!root) return 0;
        int res=0;
        if(root==p) res|=0x01;
        if(root==q) res|=0x02;
        res|=contain_nodes(root->left,p,q);
        res|=contain_nodes(root->right,p,q);
        return res;
    }
```
### 117. Populating Next Right Pointers in Each Node II

tree is not full, in-place

### 222. Count Complete Tree Nodes

a complete tree depth is the leftmost nodes
previous layers are full and contain 2^h-1 nodes
we can decide which side is not full
and one side is full and otherside is not full, and it is a sub problem

### 98. Validate Binary Search Tree

inorder traversal to see if it is sorted

### 145. Binary Tree Postorder Traversal-iterative

left right root
need push root to stack, right and then left. pop them and get post order.

### 297. Serialize and Deserialize Binary Tree

same as 449?

### 834. Sum of Distances in Tree

node i to all other nodes distance sum
think in a graph. 
dfs





