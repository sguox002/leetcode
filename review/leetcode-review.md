# leetcode review

# Chapter 1. Tree

Generally tree is O(n) complexity (visiting each node just once).
O(N^2) is generally not the optimal method

height
width
complete tree
traversal

I put these problems from one star to 5 stars. one star problem is generally very straightforward and do not need to pay much attention
## easy
easy tree problem generally involves only one recursive problem. Consider its subtree a similar problem and finally down to a single node.

## 1.1 rating: *

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

### 100. Same Tree
compare its subtree and the root
```cpp
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(!p && !q) return 1;
        if(!p || !q) return 0;
        return p->val==q->val && isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
    }
```


## 1.2 rating: **	

which may need a little bit more thinking or with extra data structure to record information while traverse.

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


## 1.3 rating: ***

these problems need more thinking either structure reorganize or more subtle information collection while traverse
understanding the different order traversal thoroughly may help a lot. May need other kind of traversal sometimes. do not limit those 3 types.

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
preorder traverse, with value passing. We shall understand that the preorder traverse is basically dfs!!!

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
Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly two or zero sub-node. 
If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes.
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
Note: this is O(N*M) approach. a recursive problem involves another recursive problem.
generally reducing O(N^2) to linear using memoization is a direct effective way.
the memoization shall include s and t root node and results. hash table using s-val + t-val string is also OK.

other O(N) needs careful study to bring out the extra information while travese.


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
similarly this is O(N^2) again.

the second method is based on DFS. Instead of calling depth() explicitly for each child node, we return the height of the current node in DFS recursion. 
When the sub tree of the current node (inclusive) is balanced, the function dfsHeight() returns a non-negative value as the height. Otherwise -1 is returned. 
According to the leftHeight and rightHeight of the two children, the parent node could check if the sub tree is balanced, and decides its return value.
```cpp
	int dfsHeight (TreeNode *root) {
        if (root == NULL) return 0;
        
        int leftHeight = dfsHeight (root -> left);
        if (leftHeight == -1) return -1;
        int rightHeight = dfsHeight (root -> right);
        if (rightHeight == -1) return -1;
        
        if (abs(leftHeight - rightHeight) > 1)  return -1;
        return max (leftHeight, rightHeight) + 1;
    }
    bool isBalanced(TreeNode *root) {
        return dfsHeight (root) != -1;
    }
```	
In this bottom up approach, each node in the tree only need to be accessed once. Thus the time complexity is O(N), better than the first solution.

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
	
## 1.4 rating ****	
these problems generally has a not so clear recursive relation and need to be very logic.
generally while traverse, we shall collect more than one type of information, and then is very delicate.
	
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
again it is O(N^2)

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
again it is O(N^2)

-------------------------------------------------------------------------------------------------
## medium
-------------------------------------------------------------------------------------------------

## Rating ***

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

### 998. Maximum Binary Tree II
a maximum tree corresponds to a list of numbers. Now append a value to the list, return the new tree.
maximum tree: the root > its child
no duplicates

The value >root, then we need put all previous in its left
if the value < root, then we need goes to right (since the max is ahead of it and the right array belongs to the right tree)

```cpp
    TreeNode* insertIntoMaxTree(TreeNode* root, int val) {
        if(!root) return new TreeNode(val);
		if(root->val<val)
		{
			TreeNode *newnode=new TreeNode(val);
			newnode->left=root;
			return newnode;
		}
		root->right=insertIntoMaxTree(root->right,val);
		return root;
    }
```	
 
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

### 515. Find Largest Value in Each Tree Row

bfs or hashmap using layer while traverse
we don't need hashmap if using preorder traversal.

```cpp
    vector<int> largestValues(TreeNode* root) {
		vector<int> ans;
		preorder(root,1,ans);
		return ans;
    }
	void preorder(TreeNode* root,int h,vector<int>& ans)
	{
		if(!root) return;
		if(h>ans.size()) ans.push_back(root->val);
		else ans[h-1]=max(ans[h-1],root->val);
		preorder(root->left,h+1,ans);
		preorder(root->right,h+1,ans);
	}
```	
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

```cpp
    TreeNode* constructFromPrePost(vector<int>& pre, vector<int>& post) {
        return helper(pre,0,pre.size(),post,0,post.size());
    }
	TreeNode* helper(vector<int>& pre,int l1,int r1,vector<int>& post,int l2,int r2)
	{
		if(l1>=r1) return 0;
		if(l1+1==r1) return new TreeNode(pre[l1]);
		TreeNode* root=new TreeNode(pre[l1]);
		int ind=find(post.begin()+l2,post.begin()+r2-1,pre[l1+1])-post.begin();
		int len=ind-l2+1;
		root->left=helper(pre,l1+1,l1+len+1,post,l2,l2+len);
		root->right=helper(pre,l1+len+1,r1,post,l2+len,r2-1);
		return root;
	}
```
pay attention to the correct range. need make sure length is the same.

### 513. Find Bottom Left Tree Value

Given a binary tree, find the leftmost value in the last row of the tree.
It is easy to give the tree a x and y coordinate.
another: use the height, if left is taller, find it in left, otherwise in right
The last row has a depth of 1

the last row first value.
in order traverse with depth. Pay attention to the initial value so one node will work.

```cpp
    int findBottomLeftValue(TreeNode* root) {
		int ans=0,maxh=-1;
		inorder(root,0,ans,maxh);
		return ans;
    }
	void inorder(TreeNode* root,int h,int& ans,int& maxh)
	{
		if(!root) return;
		inorder(root->left,h+1,ans,maxh);
		if(h>maxh) {ans=root->val;maxh=h;}
		inorder(root->right,h+1,ans,maxh);
	}
```	

### 1026. Maximum Difference Between Node and Ancestor
equivalently max difference of current node with its parents
so peorder traverse will solve the problem
```cpp
	int maxAncestorDiff(TreeNode* root) {
		int maxdiff=0;
		int mn=root->val,mx=root->val;
		preorder(root,mn,mx,maxdiff);
		return maxdiff;
	}
	void preorder(TreeNode* root,int mn,int mx,int& maxdiff)
	{
		if(!root) return;
		maxdiff=max(maxdiff,max(root->val-mn,mx-root->val));
		mn=min(mn,root->val);
		mx=max(mx,root->val);
		preorder(root->left,mn,mx,maxdiff);
		preorder(root->right,mn,mx,maxdiff);
	}
```
note: root->val-mn cannot guarantee >0 better to add abs.

	
### 919. Complete Binary Tree Inserter

A complete binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible.
complete binary tree can use array, and each one has strict index 2*n+1 and 2*n+2 for parent node n

complete binary tree is implemented using array.
Store tree nodes to a list self.tree in bfs order.
Node tree[i] has left child tree[2 * i + 1] and tree[2 * i + 2]

So when insert the Nth node (0-indexed), we push it into the list.
we can find its parent tree[(N - 1) / 2] directly.

C++:
```cpp
    vector<TreeNode*> tree;
    CBTInserter(TreeNode* root) {
        tree.push_back(root);
        for(int i = 0; i < tree.size();++i) {
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


### 865. Smallest Subtree with all the Deepest Nodes

Return the node with the largest depth such that it contains all the deepest nodes in its subtree.
calculate each node's depth
approach:
left depth ==right depth root is the one
left<right, find the one in right
else find the one in left

It is very easy to write this O(n) into O(n^2). 
for example:
```cpp
	int depth(TreeNode *root) {
		return !root ? 0 : max(depth(root->left), depth(root->right)) + 1;
	}

	TreeNode* subtreeWithAllDeepest(TreeNode* root) {
		int d = depth(root->left) - depth(root->right);
		return !d ? root : subtreeWithAllDeepest(d > 0 ? root->left : root->right);
	}
```
with some memoization mechanism, the complexity could be reduced. (storing the depth for each node)

One pass O(n) may need some thinking:
make the depth reverse, ie the leaf node is depth 0 and node+1
use post-order traverse.

```cpp
    TreeNode* subtreeWithAllDeepest(TreeNode* root) {
        return helper(root).second;
    }
	pair<int,TreeNode*> helper(TreeNode* root)
	{
		if(!root) return {0,NULL};
		pair<int,TreeNode*> l=helper(root->left),r=helper(root->right);
		int d1=l.first,d2=r.first;
		int d=max(d1,d2)+1;
		TreeNode* ans;
		if(d1==d2) ans=root;
		else if(d1<d2) ans=r.second;
		else ans=l.second;
		return {d,ans};
	}
	
```
	
### 508. Most Frequent Subtree Sum

a subtree sum=root+leftsum+rightsum
create a map
post order traverse
```cpp
    vector<int> findFrequentTreeSum(TreeNode* root) {
        unordered_map<int,int> mp;
		helper(root,mp);
		vector<int> ans;
		int tmax=0;
		for(auto it: mp) tmax=max(tmax,it.second);
		for(auto it: mp) if(it.second==tmax) ans.push_back(it.first);
		return ans;
    }
	int helper(TreeNode* root,unordered_map<int,int>& mp)
	{
		if(!root) return 0;
		int lsum=helper(root->left,mp);
		int rsum=helper(root->right,mp);
		int sum=root->val+lsum+rsum;
		mp[sum]++;
        return sum;
	}
```	
### 655. Print Binary Tree

a way to display the tree
row number is the height of the given binary tree
column number is odd number
root is in the center of the two child

approach: we need calculate the left most node and rightmost node. (it asks to output a complete tree format)
and then each node's position would be determined.
Note each row actually shall leave all spaces for all previous nodes. 
h=1: 1
h=2: 2
h=3: 4
h=4: 8, total 2^4-1 nodes

this problem has the following sub problems
1. get the height
2. get the width (can be derived directly)
3. put the node value in its position


```cpp
    vector<vector<string>> printTree(TreeNode* root) {
		int h=height(root),w=width(root);
		vector<vector<string>> ans(h,vector<string>(w));
		helper(ans,root,0,0,w-1);
		return ans;
    }
	
	int height(TreeNode* root)
	{
		if(!root) return 0;
		return max(height(root->left),height(root->right))+1;
	}
	
	int width(TreeNode* root)
	{
		if(!root) return 0;
		return max(width(root->left),width(root->right))*2+1;
	}
	
	void helper(vector<vector<string>>& ans,TreeNode* root, int depth, int l, int r)
	{
		if(!root) return;
		int mid=l+(r-l)/2;
		ans[depth][mid]=to_string(root->val);
		helper(ans,root->left,depth+1,l,mid-1);
		helper(ans,root->right,depth+1,mid+1,r);
	}
```	

### 144. Binary Tree Preorder Traversal-iterative

node, left, right
keep pushing the right into stack.
this is to show off your programming skill.
```cpp
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
		vector<int> ans;
		if(!root) return ans;
		
		while(root)
		{
			ans.push_back(root->val);
			if(root->right) st.push(root->right);
			root=root->left;
			if(!root && st.size()) {root=st.top();st.pop();}
		}
		return ans;
    }
```
or more clear using below (it uses the stack push first pop last and is pretty)
```cpp	
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
		vector<int> ans;
		if(!root) return ans;
		st.push(root);
		while(st.size())
		{
			TreeNode* p=st.top();
			ans.push_back(p->val);
			if(p->right) st.push_back(p->right);
			if(p->left) st.push_back(p->left);
		}
		return ans;
	}
```


### 94. Binary Tree Inorder Traversal
inorder: left root right
keep pushing left until there is no left, then push its right
```cpp
	vector<int> inorderTraversal(TreeNode* root) {
		stack<TreeNode*> st;
        vector<int> ans;
		//push its right first, then node
		while(root || st.size())
		{
			while(root) {st.push(root);root=root->left;}
			if(st.size()) root=st.top(),st.pop();
			ans.push_back(root->val);
			root=root->right;
		}
		return ans;
    }
```	

### 145. Binary Tree Postorder Traversal
left right root
we can do root right left traversal and reverse which is equivalent.

```cpp
    vector<int> postorderTraversal(TreeNode *root) {
        stack<TreeNode*> nodeStack;
        vector<int> result;
        //base case
        if(root==NULL) return result;
        nodeStack.push(root);
		while(!nodeStack.empty())
		{
			TreeNode* node= nodeStack.top();  
			result.push_back(node->val);
			nodeStack.pop();
			if(node->left) nodeStack.push(node->left); //first push, last pop
			if(node->right) nodeStack.push(node->right);
		}
		 reverse(result.begin(),result.end());
		 return result;
    }
```	

### 684. Redundant Connection (**)

undirected graph. this shall not be in the category of tree, it shall be in graph or disjoint set.

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

inorder traversal or binary search

### 988. Smallest String Starting From Leaf
from leaf to root
we can start from root and reverse. using preorder traverse
```cpp
    string smallestFromLeaf(TreeNode* root) {
		string ans;
		helper(root,"",ans);
		return ans;
    }
	void helper(TreeNode* root,string t,string& ans)
	{
		if(!root) return;
		t+='a'+root->val;
		if(!root->left && !root->right)
		{
			reverse(t.begin(),t.end());
			if(ans.empty()) ans=t;
			else if(ans>t) ans=t;
		}
		helper(root->left,t,ans);
		helper(root->right,t,ans);
	}
	
```	
### 623. Add One Row to Tree

add a row of value at the given depth. root depth is 1. 
bfs to find the previous row with the depth and replace the row and add those original as child
left=parent->left
parent->left=new node(1)
parent->left->left=left
right treated the same.


```cpp
    void helper(TreeNode* root, int v, int d, int cd) {
        if (!root) return;
        
        if (cd == d) {
            TreeNode *savLeft = root->left;
            root->left = new TreeNode(v);
            root->left->left = savLeft;
            
            TreeNode *savRight = root->right;
            root->right = new TreeNode(v);
            root->right->right = savRight;
            return;
        }
        helper(root->left, v, d, cd + 1);
        helper(root->right, v, d, cd + 1);
    }
    TreeNode* addOneRow(TreeNode* root, int v, int d) {
        if (d == 1) {
            TreeNode *newRoot = new TreeNode(v);
            newRoot->left = root;
            return newRoot;
        }
        helper(root, v, d, 2);
        return root;
    }
```	

### 958. Check Completeness of a Binary Tree
after a null node is found, there is no more nodes in the queue.
the key is if we found a node we need push its left and right child, does not care if it is empty
bfs using queue.
```cpp
    bool isCompleteTree(TreeNode* root) {
        
    }
```	
### 337. House robber III

dynamic with tree

if we rob the root, then we need skip its left and right child (sub problem)
if we not rob the root, then we can rob the left and right
compare the two solutions and choose the max

```cpp
    int rob(TreeNode* root) {
        if(!root) return 0;
		int val=0;
		if(root->left) val+=rob(root->left->left)+rob(root->left->right);
		if(root->right) val+=rob(root->right->left)+rob(root->right->right);
		return max(val+root->val,rob(root->left)+rob(root->right));
    }
```	
this recursive solution TLE
There are a lot of overlaps.
to solve root: we need evaluate root->left, root->right, root->left->left,root->left->right,root->right->left,root->right->right
to evaluate root->left and we need to evaluate root->left->left and root->left->right again
we may need a hashmap for memoization.

```cpp
	unordered_map<TreeNode*,int> dp;
    int rob(TreeNode* root) {
        if(!root) return 0;
		if(dp.count(root)) return dp[root];
		int val=0;
		if(root->left) val+=rob(root->left->left)+rob(root->left->right);
		if(root->right) val+=rob(root->right->left)+rob(root->right->right);
		return dp[root]=max(val+root->val,rob(root->left)+rob(root->right));
    }
```	
### 102. Binary Tree Level Order Traversal

queue
or use left first and then right, with layer index, dfs is fine

### 199. Binary Tree Right Side View

basically it needs the rightmost node for each layer
bfs or dfs to store layer index and the last one in the layer
preorder traverse gives the desired sequence (not exact, root, right, left and right most node is visited first).

```cpp
     vector<int> rightSideView(TreeNode *root) {
        vector<int> res;
        recursion(root, 1, res);
        return res;
    }
    
    void recursion(TreeNode *root, int level, vector<int> &res)
    {
        if(root==NULL) return ;
        if(res.size()<level) res.push_back(root->val);
        recursion(root->right, level+1, res);
        recursion(root->left, level+1, res);
    }
```
	
### 173. Binary Search Tree Iterator

next: next smallest
has_next:
next smallest is always the leftmost node, so using a stack to store those nodes in inorder sequence

### 449. Serialize and Deserialize BST

serialize to a string and deserialize the string into a BST
serialize output to a stringstream using preorder
deserialize input from a stringstream using preorder
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
one method: convert the tree to an undirectional graph and then using bfs or dfs

```cpp
     vector<int> distanceK(TreeNode* root, TreeNode* target, int K) {
        unordered_map<int,vector<int>> adj;
		build_map(root,adj);
		queue<int> q;
		unordered_set<int> visited;
        q.push(target->val);
        visited.insert(target->val);
		int step=K;
		vector<int> ans;
        
		while(step && q.size())
		{
			int sz=q.size();
            for(int i=0;i<sz;i++)
			{
				int val=q.front();
				q.pop();
				vector<int>& vt=adj[val];
                //if(visited.count(val)) continue;
				for(int t: vt) 
                    if(!visited.count(t)) {
                        q.push(t);visited.insert(t);
                    }
			}
			step--;
		}
        //remaining in the queue are the answer
        while(q.size()) ans.push_back(q.front()),q.pop();
		return ans;
    }
	void build_map(TreeNode* root,unordered_map<int,vector<int>>& adj)
	{
		if(!root) return;
		if(root->left) {
			adj[root->val].push_back(root->left->val);
			adj[root->left->val].push_back(root->val);
		}
		if(root->right){
			adj[root->val].push_back(root->right->val);
			adj[root->right->val].push_back(root->val);
		}
		build_map(root->left,adj);
		build_map(root->right,adj);
	}
```
1. use TreeNode* instead of pointer (if duplicate allowed)
2. do not put too much inside the bfs loop, just exit when step becomes 0

### 971. Flip Binary Tree To Match Preorder Traversal
check if we can flip left right child to get the preorder traversal given.
return the flipped nodes if not possible return {-1}
approach: 
preorder: root, left, right.
root value not the same, false
left value not the same with array value, swap left and right
else go to left and right

```cpp
    vector<int> res;
    int i = 0;
    vector<int> flipMatchVoyage(TreeNode* root, vector<int>& v) {
        return dfs(root, v) ? res : vector<int>{-1};
    }

    bool dfs(TreeNode* node, vector<int>& v) {
        if (!node) return true;
        if (node->val != v[i++]) return false;
        if (node->left && node->left->val != v[i]) {
            res.push_back(node->val);
            return dfs(node->right, v) && dfs(node->left, v);
        }
        return dfs(node->left, v) && dfs(node->right, v);
    }
```
dfs is preorder traversal
need return the parent node. not the left/right.

### 96. Unique Binary Search Trees

1 to N, how many unique BST
first 1 to n can be the root. Suppose i is the root
and it divides 1 to i-1 in the left and i+1 to n in the right. the total number is m=L*R
this is a dp problem: dp[i]=sum(dp[j-1]*dp[i-j]) j from 1 to i

### 652. Find Duplicate Subtrees

first we need to know how to judge two trees are equal
then we need compare each nodes (traverse)
The way: convert a tree into a string and store into a hashset or map. Then we traverse and check if the string already appeared.
post order: the root is the last one
```cpp
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
		unordered_map<string,int> mp;
		vector<TreeNode*> ans;
		postorder(root,ans,mp);
		return ans;
    }
	string postorder(TreeNode* root,vector<TreeNode*>& ans,unordered_map<string,int>& mp)
	{
		if(!root) return "#";
		string s1=postorder(root->left,ans,mp);
		string s2=postorder(root->right,ans,mp);
		string s=s1+","+s2+","+to_string(root->val);
		if(mp[s]==1) ans.push_back(root);
		mp[s]++;
		return s;
	}
```	
To use a string, we need keep the null node in the string, otherwise we cannot differentiate the left/right.

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
can use full binary tree array storage (null node in the middle also counts)
We know that a binary tree can be represented by an array (assume the root begins from the position with index 1 in the array). 
If the index of a node is i, the indices of its two children are 2*i and 2*i + 1.
```cpp
    int widthOfBinaryTree(TreeNode* root) {
       //it can be calculated by the left most branch and the right most branch,  on the same layer
        //bfs search to get the left and rightmost nodes and get the max distance
        //use an array to store the left-most node number for each layer and then preorder traversal
        vector<int> lefts;
        return dfs(root,0,1,lefts);
    }
    int dfs(TreeNode* root, int level, int id,vector<int>& lefts) //get the width of the tree
    {
        if(!root) return 0;
        if(level>=lefts.size()) lefts.push_back(id); //the first non-empty node
        //cout<<level<<" "<<id<<" "<<lefts[level]<<endl;
        int d1=dfs(root->left,level+1,id*2,lefts);//max width of left subtree-
        int d2=dfs(root->right,level+1,id*2+1,lefts); //max width of right subtree
        return max(id-lefts[level]+1,max(d1,d2)); //current layer max width vs left right.
    }

```	

### 450. Delete Node in a BST

there is standard operations on deleting a node
see some algorithm text coursera

Recursively find the node that has the same value as the key, while setting the left/right nodes equal to the returned subtree
Once the node is found, have to handle the below 4 cases
- node doesn't have left or right - return null
- node only has left subtree- return the left subtree
- node only has right subtree- return the right subtree
- node has both left and right - find the minimum value in the right subtree, set that value to the currently found node, then recursively delete the minimum value in the right subtree
```cpp
    TreeNode* deleteNode(TreeNode* root, int key) {
		if(!root) return 0;
		if(key<root->val) root->left=deleteNode(root->left,key);
		else if(key>root->val) root->right=deleteNode(root->right,key);
		else //find the node
		{
			if(!root->left) return root->right;
			else if(!root->right) return root->left;
			TreeNode* minNode=findMin(root->right);
			root->val=minNode->val;
			root->right=deleteNode(root->right,root->val);
		}
		return root;
    }
	TreeNode* findMin(TreeNode* root)
	{
		while(root->left) root=root->left;
		return root;
	}
```
	
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
```cpp
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {

        return helper(preorder,0,preorder.size(),inorder,0,inorder.size());
    
    }

    TreeNode* helper(vector<int>& preorder,int i,int j,vector<int>& inorder,int ii,int jj)
    {
        // tree        8 4 5 3 7 3
        // preorder    8 [4 3 3 7] [5]
        // inorder     [3 3 4 7] 8 [5]

        // 每次从 preorder 头部取一个值 mid，作为树的根节点
        // 检查 mid 在 inorder 中 的位置，则 mid 前面部分将作为 树的左子树，右部分作为树的右子树

        if(i >= j || ii >= j)
            return NULL;

        int mid = preorder[i];
        auto f = find(inorder.begin() + ii,inorder.begin() + jj,mid);

        int dis = f - inorder.begin() - ii;

        TreeNode* root = new TreeNode(mid);
        root -> left = helper(preorder,i + 1,i + 1 + dis,inorder,ii,ii + dis);
        root -> right = helper(preorder,i + 1 + dis,j,inorder,ii + dis + 1,jj);
        return root;
    }
```
	
### 106. Construct Binary Tree from Inorder and Postorder Traversal

inorder: left root right
post: left right root
similarly get the root from post and then divide into left and right

### 116. Populating Next Right Pointers in Each Node

next: the next right node.
using the next we have a mechanism to do level traversal, and for each layer we directly update its children. next is used for this layer's traversal

next is null the layer is done
```cpp
    Node* connect(Node* root) {
        if(!root) return 0;
		queue<Node*> dq;
		dq.push(root);
		while(dq.size())
		{
			int sz=dq.size();
			Node* prev=0;
			for(int i=0;i<sz;i++)
			{
				Node* t=dq.front();dq.pop();
				if(t->left) dq.push(t->left);
				if(t->right) dq.push(t->right);
				if(prev) prev->next=t;
				prev=t;
			}
		}
		return root;
    }
```
	
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
```cpp
    Node* connect(Node* root) {
        if(!root) return 0;
        Node* p=root->next;
        while(p && (!p->left && !p->right)) p=p->next;        
		if(root->left && root->right) 
		{
			root->left->next=root->right;//right not processed yet.
			if(p) root->right->next=p->left?p->left:p->right;
		}
		else if(root->left || root->right)
		{
			if(p)
			{
				if(root->left) root->left->next=p->left?p->left:p->right;
				else root->right->next=p->left?p->left:p->right;
			}
		}
		connect(root->right);
        connect(root->left);
		return root;
    }
```
	
### 222. Count Complete Tree Nodes

a complete tree depth is the leftmost nodes
previous layers are full and contain 2^h-1 nodes
we can decide which side is not full
and one side is full and otherside is not full, and it is a sub problem

```cpp
    int countNodes(TreeNode* root) {
		if(!root) return 0;
        int h1=depth(root->left),h2=depth(root->right);
		if(h1>h2) return (1<<h2)+countNodes(root->left); //left>right, right is full
		return (1<<h1)+countNodes(root->right); //left is full
    }
	int depth(TreeNode* root)
	{
		if(!root) return 0;
		return 1+max(depth(root->left),depth(root->right));
	}
```

### 987. Vertical Order Traversal of a Binary Tree
vertical: left x-1 right x+1, depth is y
some nodes may share the same position
x,y,val, put into an array and sort using first x, then y and then val.

```cpp
    vector<vector<int>> verticalTraversal(TreeNode* root) {
		vector<vector<int>> ans,mp;
		preorder(root,0,0,mp);
		sort(mp.begin(),mp.end(),
		[](const vector<int>& a,const vector<int>& b)
		{
			return a[0]<b[0] || a[0]==b[0] && a[1]<b[1] || a[0]==b[0] && a[1]==b[1] && a[2]<b[2];
		});
		int prev=INT_MIN;
		for(auto t: mp)
		{
			if(t[0]!=prev) {ans.push_back({t[2]});prev=t[0];}
			else ans.back().push_back(t[2]);
		}
		return ans;
    }
	void preorder(TreeNode* root,int x,int y,vector<vector<int>>& mp)
	{
		if(!root) return;
		mp.push_back({x,y,root->val});
		preorder(root->left,x-1,y+1,mp);
		preorder(root->right,x+1,y+1,mp);
	}
```	
### 98. Validate Binary Search Tree

inorder traversal to see if it is sorted
```cpp
    long prev=LONG_MIN;
    bool isValidBST(TreeNode* root) {
        if(!root) return 1;
        bool res=isValidBST(root->left);
        if(prev!=LONG_MIN) 
            res=res&&root->val>prev;
        prev=root->val;
        res=res&&isValidBST(root->right);
        return res;
    }
```
	
### 145. Binary Tree Postorder Traversal-iterative

left right root
need push root to stack, right and then left. pop them and get post order.

### 297. Serialize and Deserialize Binary Tree

same as 449?


## Hard
### 834. Sum of Distances in Tree

node i to all other nodes distance sum
think in a graph. 
dfs
approach 1: iterate N nodes , each node using bfs or dfs and will get O(N^2) complexity, will TLE
approach 2: 
first we convert the tree into a graph using adjacent matrix.

suppose x and y are neighboring nodes, if we break it, we divide into two subtree X and Y.
ans(x)=x@X+y@Y+#(Y)
x@X: distance from x to all nodes in X
y@Y: distance from y to all nodes in Y
#(y): number of nodes in Y (since x to y repeat n times)

ans(y)=x@X+y@Y+#(X)
so ans(x)-ans(y)=#(Y)-#(X)

let us choose a node as the root.
we break it into two parts, one with the node inside, one is its subtree. (or the Y part), X is its child part.
count[node]: number of nodes in Y
stsum[node]: the sum of distance fron node to nodes in Y

These are two recursive problem (by repeating the scheme):
count(node)+=count(child)
stsum[node]+=stsum[child]+count[child]

then we use the neighboring relation ans(x)=ans(y)+#(Y)-#(X) to get the answer for node x from its neighboring node
#(Y)=N-#(X)


```cpp
    vector<int> sumOfDistancesInTree(int N, vector<vector<int>>& edges) {
        vector<vector<int>> adj(N);
		for(auto t: edges) adj[t[0]].push_back(t[1]),adj[t[1]].push_back(t[0]);
		vector<int> count(N,1),ans(N);
		dfs1(adj,0,-1,count,ans); //using node 0 as the node.
		dfs2(adj,0,-1,count,ans);
		return ans;
    }
	
	void dfs1(vector<vector<int>>& adj,int node,int parent,vector<int>& count,vector<int>& stsum)
	{
		for(int child: adj[node])
		{
			if(child!=parent)
			{
				dfs(adj,child,node,count,stsum);//first get its children, which is post order traversal
				count[node]+=count[child];
				stsum[node]+=stsum[child]+count[child];
			}
		}
	}
	
	void dfs2(vector<vector<int>>& adj,int node,int parent,vector<int>& count,vector<int>& stsum)
	{
		for(int child: adj[node])
		{
			if(child!=parent)
			{
				stsum[child]=stsum[node]+N-2*count[child]; //get its child's answer and go on deeper nodes
				dfs2(adj,child,node,count,stsum);
			}
		}
	}
	
```
Note count initialize to 1, since the node itself is not counted.

	
### 1028. Recover a Tree From Preorder Traversal
using stack to store the node and its depth. 
```cpp
    TreeNode* recoverFromPreorder(string S) {
        stack<pair<TreeNode*,int>> st;
        TreeNode* root=0;
        string w;
        int depth=0,prev_depth=0;
        S+='-';
        for(int i=0;i<S.size();i++)
        {
            char c=S[i];
            if(c=='-') 
            {
                if(w.size()) 
                {
                    int val=stoi(w);
                    TreeNode* t=new TreeNode(val);
                    if(st.empty()) root=t;
                    else
                    {
                        while(st.size() && st.top().second>depth) st.pop();
                        if(st.top().second==depth) {st.pop();st.top().first->right=t;}
                        else st.top().first->left=t;
                    }
                    st.push(make_pair(t,depth));
                    depth=0;
                    w="";
                }
                depth++;
            }
            else {w+=c;}
        }
        return root;
    }
```	

### 968. Binary Tree Cameras
camera can monitor its parent, itself and its immediate children
return the min number of cameras to cover all the nodes

see problem 337 house robber III.
they are similar in the fact that if we install a camera, its connected nodes are covered 
(in 337 if we rob the node, all its connected nodes cannot be robbed).

consider the root node with left and right subtree
consider the left subtree, there are 3 cases:
left covered: 
left not covered
left parent covered

also for right subtree:
right covered
right not covered
right parent covered

Greedy algorithm:
put cameras at as high level as possible, since it can cover more (children, parents and itself)
0: no camera at the root, and no camera at children (not monitored)
1: no camera at the root, and at least 1 child has camera, (monitored)
2: there is a camera at this node

```cpp
    int minCameraCover(TreeNode* root) {
		int sum=0;
		if(dfs(root,sum)==0) sum++;
		return sum;
    }
	
	int dfs(TreeNode* root,int& sum)
	{
		if(!root) return 1; //no camera, but covered
		int l=dfs(root->left,sum),r=dfs(root->right,sum);
		if(l==0 || r==0) //one or two children not monitored
		{
			sum++;return 2; //add a camera here
		}
		else if(l==2 || r==2) //one or two children has a camera
			return 1;
		else return 0; //both children are monitored but no camera. greedy choice, we are not place a camera here, but try to place on its parent
		return -1;
	}
```	

### 124. Binary Tree Maximum Path Sum
the path need contain at least one node
can contain the root or not. 

A path from start to end, goes up on the tree for 0 or more steps, then goes down for 0 or more steps. Once it goes down, it can't go up. Each path has a highest node, which is also the lowest common ancestor of all other nodes on the path.
A recursive method maxPathDown(TreeNode node) (1) computes the maximum path sum with highest node is the input node, update maximum if necessary (2) returns the maximum sum of the path that can be extended to input node's parent.

```cpp
	int maxPathSum(TreeNode* root)
	{
		int maxSum=INT_MIN;
		maxPathDown(root,maxSum);
		return maxSum;
	}
	
	int maxPathDown(TreeNode* root,int& max_sum)
	{
		if(!root) return 0;
		int left=max(0,maxPathDown(root->left,max_sum)); //starting from the node or continue
		int right=max(0,maxPathDown(root->right,max_sum));//starting from 
		max_sum=max(max_sum,left+right+root->val);
		return max(left,right)+root->val; //the root itself, or the left /right
	}
```

### 685. Redundant Connection II
now it is a directed graph
if one edge is removed, it becomes a tree.
return the last edge to remove.

```cpp
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        //use the fact that each node only has zero or one parent, and cannot be itself
        //note: when there is a cycle and two parents, need to remove the edge in the cycle
        //four cases: 1: loop, 2, two parents, 3, first loop then two parents 4. first two parents then loop
        unordered_map<int,int> parent;
        vector<int> twop(2,-1),loop(2,-1);
        for(int i=0;i<edges.size();i++)
        {
            int p=edges[i][0],c=edges[i][1];
            if(parent.count(c)) 
            {
                //we will first remove this edge and try later
                twop=edges[i];
                continue;
            }
            else parent[c]=p;
            if(iscycle(c,parent)) 
            {
                loop=edges[i];
                parent.erase(c);
            }
        }
        if(twop[0]<0) return loop;
        if(loop[0]<0) return twop;
        twop[0]=parent[twop[1]];
        return twop;
        
    }
    bool iscycle(int c,unordered_map<int,int>& parent)
    {
        //find its ultimate parent
        int p=c;
        while(parent.count(c)) {c=parent[c];if(c==p) break;}
        return p==c;
    }
```
	

# Chapter 2. Binary search

1. define the ranges and conditions

2. define the exit condition

3. invariant condition

also we need to know if we are getting the first or last answer. This will change the iteration loop termination condition

## easy

### 744. Find Smallest Letter Greater Than Target
letters are wraparound. 
approach 1: find upper_bound, if not found, return the first one.
approach 2: write our own upper_bound
may contain duplicates

Binary search: consider the following 3 things:

- using greater for the input, the left side is all false, and the right part is all true.
we are looking for the first true.
for the range [l,r], the l is false, and the right is true, ie. A[l]<=target, and A[r]>target.
l==r will terminate the loop and answer is l or r.

- invariant: we shall always keep the condition true.
A[m]>target: r=m (since m could be the answer)
else: l=m+1 since m is not the answer.

- two element: m=l+(r-l)/2 will always choose l, and we forward to l+1 and loop will exit. so no problem

And the code: 

```cpp
	char nextGreatestLetter(vector<char>& letters, char target) {
		int l=0,r=letters.size();
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(letters[m]>target) r=m;
			else l=m+1;
		}
		//when exit l==r so the answer is l or r.
		return letters[l%letters.size()];
	}
```

### 35. Search Insert Position
same as 744 but similar to lower_bound, but now we need find the position
we are looking for the first element satisfying A[m]>=target
using the condition: the left part are all false, and right part are all true.
so we shall keep this invariant.

1. [l,r] ge(l) is false and ge(r) is true. so we need keep l<r and exit condition would be l==r and answer is r.

2. A[m]>=target, ge(m) is true, and we shall forward to r=m (m is possible answer)
A[m]<target ge(m) is false, and we shall set l=m+1 (otherwise we will go to true and break the invariant)

3. two element: make sure there is no infinite loop.

```cpp
	int searchInsert(vector<int>& nums, int target) {
		int l=0,r=nums.size();
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(nums[m]==target) return m;
			if(nums[m]<target) l=m+1;
			else r=m;
		}
		return l;
	}
```
once we have duplicates, this solution will return any of the duplicates.

or a bit change: (better)
```cpp
	int searchInsert(vector<int>& nums, int target) {
		int l=0,r=nums.size();
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(nums[m]<target) l=m+1;
			else r=m;
		}
		return l;
	}
```
this shall work for duplicate numbers

### 852. Peak Index in a Mountain Array
left sorted, right reverse sorted
O(n): from either side to climb until get to the peak
binary search: we are looking for the peak: A[peak-1]<A[peak]>A[peak+1]

we still use the 3 principles for binary search:
1. peak in [l,r] means: A[l]<peak>A[r]. if we use A[m]>A[l] we will get: 1,1,1,...1,0,0,0....
so we are looking for the last true condition

2. A[m]<A[m+1] l=m+1
A[m-1]>A[m] r=m (since m is false but m-1 may be not)

3. using (2) l will stop at the last 1 and r will stop at the first 0 and loop shall terminate when l+1<r

```cpp
	int peakIndexInMountainArray(vector<int>& A) {
		int n=A.size(),l=0,r=n-1;
		while(l<r)  //change to while(l+1<r) also works.
		{
			int m=l+(r-l)/2;
			if(A[m]<A[m+1]) l=m+1;
			else if(A[m-1]>A[m]) r=m;
			else return m; //this is necessary to cover all cases. without this will fall into infinite loop
		}
		return -1;
	}
```
comment: not easy to implement the binary search.

### 704. Binary Search
classical to find a target's index in sorted array
approach 1: use lower_bound
approach 2: l, r. target shall be in the range [l,r] all the time.

3 principles:
1. the target is in the range [l,r] meaning that A[i]<=target, all left is true, and all right is false
we are shrinking the range to either [l,l+1] or [l,l]
In this case, since we are looking for exactly match, == means early termination

A[m]>target, it is in the left, r=m-1
A[m]<target, is is in the right l=m+1
A[m]==target, found

```cpp
	int search(vector<int>& nums, int target) {
		int l=0,r=nums.size()-1;
		while(l<=r)
		{
			int m=l+(r-l)/2;
			if(nums[m]==target) return m;
			if(nums[m]<target) r=m-1;
			else l=m+1;
		}
		return -1;
	}
```

### 374. Guess Number Higher or Lower
just replace the condition with predefined function
invariant: a[l]<=target<=a[r]

```cpp
    int guessNumber(int n) {
        int l=1,r=n;
		while(l<=r)
		{
			int m=l+(r-l)/2;
			int t=guess(m);
			if(t==0) return m;
			if(t<0) //the number is lower
				r=m-1;
			else l=m+1;
		}
		return l; //exit when l>r, not found
    }
```

### 367. Valid Perfect Square	
equiv. to find the square root x so x*x==N
x is positive number

1. range: l=1,r=n; 
2. we are searching for x*x==n which is exact match. so from 1 to n, left is all false, and right is all false. 
and our answer is the low.
3. when x*x<n, we need set l=m+1 (since it is false)
when x*x>n: we need set r=m-1 (since it is false)
4. exit condition when l>r (l==r need to be checked)

```cpp
	bool isPerfectSquare(int num) {
		int l=1,r=num;
		while(l<=r)
		{
			int m=l+(r-l)/2;
			long t=(long)m*m;
			if(t==num) return 1;
			if(t<num) l=m+1;
			else r=m-1;
		}
		return 0;
	}
```

### 441. Arranging Coins
to find the m so: n>=m*(m+1)/2. which is the lower_bound
answer in [l,r): l*(l+1)/2<=n<r*(r+1)/2
1. range from 1 to n. f(1),f(2)....f(m)=m*(m+1)/2
2. we are searching for the last f(m)<=n or equivalently the one before the first one f(m)>n
assuming we are using f(m)<=n for the last one:
f(m)<=n: lo=m; (m guarantee it is true)
f(m)>n: r=m-1 (m guarantee it is false)
3. iteration ends when l+1==r.
Note: n could be very large and high=n cannot guarantee that the condition is false. so there is a problem.
to guarantee the condition: we need make l=0 and r=n+1.
```cpp
    int arrangeCoins(int n) {
        long low = 0, high = (long)n+1;
        while (low+1 < high) {
            long mid = low + (high - low ) / 2;
            if ((mid + 1) * mid / 2.0 <= n) low = mid;
            else high = mid;
        }
        return low;
    }
```

The following code can handle 0, 1 no problem
since r=m-1 to go to the other end. (it can handle all 1 or all 0 cases)
	
```cpp
    int arrangeCoins(int n) {
        long low = 1, high = n;
        while (low < high) {
            long mid = low + (high - low + 1) / 2;
            if ((mid + 1) * mid / 2.0 <= n) low = mid;
            else high = mid - 1;
        }
        return high;
    }
```
above solution use [l,r) or [l,r-1] to find the last one which satisfies m*(m+1)<=n.
mid=l+(r-l+1)/2 is used to break the infinite loop
since l+(r-l)/2 always choose the first element in the group, add 1 to round instead of down.
test it using two element input to see if we fall into infinite loop

or
```cpp
    int arrangeCoins(int n) {
		int l=0,r=n;
		while(l<=r)
		{
			int m=l+(r-l)/2;
			long t=(long)m*(m+1)/2;
			if(t<=n) l=m+1;
			else r=m-1;
		}
		return l-1;
	}
```
above solution finds the one > n so the answer is l-1
i.e. m*(m+1)>n, and the one ahead it m*(m-1)<=n
so when <=n, we go ahead
when >n: we reduce it, but why is r=m-1?

### 69. Sqrt(x)
similar to the perfect square number. but now we are asking for the last <=
binary search to get y*y<=x
m*m<=x, l=m
m*m>x, r=m-1

invariant: answer is in the range [l,r). and exit l==r
```cpp
    int mySqrt(int x) {
		int l=0,r=x;
		while(l<r)
		{
			int m=l+((long)r-l+1)/2;
			long t=(long)m*m;
			if(t<=x) l=m;
			else r=m-1;
		}
		return l;
	}
```
to avoid overflow: when r=INT_MAX
exit condition l==r

### 278. First Bad Version
this is the get the first version return true
invariant: [l,r] where l is 0 and r is 1. getting the first one return 1.
exit condition: l==r 
```cpp
	int firstBadVersion(int n) {
		int l=1,r=n;
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(isBadVersion(m)) r=m;
			else l=m+1;
		}
		return r;
	}
```
will fall into infinite loop? l=1,r=2, m=1,
if true, r=1
if false, l=2 

### 349. Intersection of Two Arrays
input contains duplicates. not sorted.

approach 1: hashset is trivial O(n)
approach 2: sort the two arrays and use merge sort A>B A<B and A==B 3 cases
approach 3: sort the two arrays and use binary search (stl or by hand search)

not a good example since binary search is not as good as O(n) algorithm.

#### 167. Two Sum II - Input array is sorted
approach 1: put into hashmap and find target-node
approach 2: two pointer to shrink range
approach 3: binary search, iterate on all elements and do binary search for target-numbers. less efficient.

### 350. Intersection of Two Arrays II
contain duplicates and need output the duplicates
approach 1: hashmap to increase, and other to decrease
approach 2: sort both array and merge sort, not the best

```cpp
	vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
		sort(nums1.begin(),nums1.end());
		sort(nums2.begin(),nums2.end());
		int i=0,j=0;
		vector<int> ans;
		while(i<nums1.size() && j<nums2.size())
		{
			if(nums1[i]==nums2[j]) {ans.push_back(nums1[i]);i++,j++;}
			else if(nums1[i]<nums2[j]) i++;
			else j++;
		}
		return ans;
	}
```
sort complexity O(nlogn), merge sort O(n+m)

### 475. Heaters
given a list of house positions and a list of heater positions
find the min radius to cover all houses
sort the heaters and locate the heater for each house. get the smallest difference and get the global max


## medium

### 1011. Capacity To Ship Packages Within D Days
to get the min capacity to have all the weights shipped in D days
idea: 
1. the ship can carry one or more. The capacity is limited by the max element and the average Sum/D
2. the max capacity is limited by the total
3. we are searching for the capacity so we can divide the weights into D days
4. for different capacity we may have duplicate days. we are looking for the first capacity which satisfy the D days
for smaller capacity, it will get more days
for larger capacity, it will get less days
so the condition would be 0,0,0...1,1,..1,0,0....0. (wrong)
actually it shall be 0,0,0,...1,1,.....1 (all <=D is true)

```cpp
	int shipWithinDays(vector<int>& weights, int D) {
		int total=accumulate(weights.begin(),weights.end(),0);
		int mn=max((total+D/2)/D,*max_element(weights.begin(),weights.end()));
		int l=mn,r=total;
		while(l<r)
		{
			int m=l+(r-l)/2;
			int days=get_days(weights,m);
			if(days>D) l=m+1;
			//else if(days<D) r=m-1;//this shall be removed, check what the problem asks
			else r=m;
		}
		return l;
	}
	int get_days(vector<int>& weights,int cap)
	{
		//moving sum 
		int ndays=0,sum=0;
		for(int w: weights)
		{
			if(sum+w<=cap) sum+=w;
			else
			{
				ndays++;
				sum=w;
			}
		}
		if(sum) ndays++;
		return ndays;
	}
```
when reaching final [1,1], m=0, l=0 and will cause infinite loop so we need use < in the loop
check what the problem needs: it needs <=D days! so we need change to <=D r=m


### 230. Kth Smallest Element in a BST
approach 1: inorder traverse and count to O(K)
approach 2: binary search
idea:
1. count left nodes first
2. if k<num nodes in left, search in the left subtree
else search for k-1-nleft in the right subtree
best O(N), up to O(N^2) since node are repeatedly visited.
```cpp
    int kthSmallest(TreeNode* root, int k) {
        int nleft=num_nodes(root->left);
        if(nleft>=k) return kthSmallest(root->left,k);
        else if(k>nleft+1) return kthSmallest(root->right,k-1-nleft);
        return root->val;
    }
    int num_nodes(TreeNode* root)
    {
        if(!root) return 0;
        return 1+num_nodes(root->left)+num_nodes(root->right);
    }
```

### 981. Time Based Key-Value Store
store key, value, ts
get(key,time) to return the key value pair with ts<=time
multiple values: return the one with largest timestamp
use a unordered_map<string,map<int,string>> structure and binary search (upper_bound is easier)
sort by the timestamp. 
note 1: map has member upper_bound, cannot use algorithm upper_bound
note 2: if it is mp[key].begin() we need return ""

### 454. 4Sum II
4 lists, compute the number of tuples from each list to sum to 0
approach 1: A+B, C+D form a N^2 hashmap and then find opposite sum
this is really not a binary search problem. but if we sorted C and D, and we can get it also.
### 287. Find the Duplicate Number
count the number of numbers <=m
if cnt<=m, indicating m is larger (not in the smaller side) l=m+1
else m could be an answer so r=m

O(nlogn)

```cpp
    int findDuplicate(vector<int>& nums) {
        int l=0,r=nums.size();
        while(l<r)
        {
            int m=l+(r-l)/2;
            int cnt=0;
            for(int n: nums)
                if(n<=m) cnt++;
            if(cnt<=m) l=m+1;
            else r=m;
        }
        return l;
    }
```

O(N) algorithm treating this as a linked list, element value as the next pointer
try to detect a cycle and find the intersection node.

### 378. Kth Smallest Element in a Sorted Matrix	
rows and columns are sorted.
2d matrix
kth element in sorted order.

approach 1: merge sort 
approach 2: min heap using priority queue
first build a min heap from first row (store val,x,y)
pop the min, and push the next element in the same column.
the first min is (0,0), the 2nd min is (0,1) or (1,0), the 3rd is (0,2),(1,1),(1,2)

```cpp
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int  n=matrix.size();
        priority_queue<vector<int>> pq;
        for(int j=0;j<n;j++) pq.push({-matrix[0][j],0,j});
        for(int i=0;i<k-1;i++)
        {
            vector<int> t=pq.top();
            pq.pop();
            int x=t[1],y=t[2];
            if(x==n-1) continue;
            pq.push({-matrix[x+1][y],x+1,y});
        }
        return -pq.top()[0];
    }
```
this could be O(n^2logn) each insert into the pq, needs logn

binary search:
similar idea as 287
we count the number <=mid
count in a sorted row/col does not need traverse each number

```cpp
	int kthSmallest(vector<vector<int>>& matrix, int k) {
		int n=matrix.size();
		int l=matrix[0][0],r=matrix[n-1][n-1]+1;
		while(l<r)
		{
			int m=l+(r-l)/2;
			int cnt=0;
			//count number of elements <=mid
			int c=n-1;
			for(int r=0;r<n;r++)
			{
				while(c && matrix[r][c]>m) c--;
				cnt+=c+1; //always add 1 or more into it
			}
			if(cnt<k) l=m+1;
			else r=m;
		}
		return l;
	}
```	
fail with [[1,2],[3,3]]
2
need change while(c>=0 && ... to fix the bug.
how do we guarantee that lo is an element in the matrix??
assuming we found a m which satisfy the k, but m is not an element inside
will move r=m, and keep approaching the element. shrinking the range to 1

so does not matter the number of elements inside, but only matters the range (max-min)

### 392. Is Subsequence
check if s is subsequence of t
s is a short string <100
t is long string ~5e5

approach 1: two pointer, greedy, O(n+m)
```cpp
	bool isSubsequence(string s, string t) {
		int i=0,j=0;
		while(i<s.size() && j<t.size())
		{
			if(s[i]==t[j]) i++;
			j++;
		}
		return i==s.size();
	}
```

another approach:
build 26 char with all its index
binary search the index to see if we can match an index
this is good for a lot of incoming s
```cpp
	bool isSubsequence(string s, string t) {
		vector<vector<int>> mp(26);
		for(int i=0;i<t.size();i++)
		{
			int ind=t[i]-'a';
			mp[ind].push_back(i);
		}
		int start=-1;
		for(char c: s)
		{
			vector<int>& vt=mp[c-'a'];
			int ind=upper_bound(vt.begin(),vt.end(),start)-vt.begin();
			if(ind==vt.size()) return 0;
			start=vt[ind];//real index
		}
		return 1;
	}
```

### 911. Online Election
given input people, and time of vote
query the top candidate at time t.

if there is a tie, return the person who has the most recent vote.

approach:
maintain a data structure for each person, t, and number of votes
unordered_map<int,vector<int,int>>

```cpp
	unordered_map<int,vector<int>> mp; //person vs time
	TopVotedCandidate(vector<int> persons, vector<int> times) {
		for(int i=0;i<persons.size();i++)
			mp[persons[i]].push_back(times[i]);
	}
	int q(int t) {
        int recent,mostvoted=0;
		int ans=0;
		for(auto tt: mp)
		{
			vector<int>& vt=tt.second;
			int ind=upper_bound(vt.begin(),vt.end(),t)-vt.begin();
			if(mostvoted<=ind)
			{
				if(ind && mostvoted==ind && vt[ind-1]>recent)
				{
					recent=vt[ind-1];
					ans=tt.first;
				}
				if(mostvoted<ind)
				{
					mostvoted=ind;
					ans=tt.first;
					recent=vt[ind-1];
				}
			}
		}
		return ans;
    }
```
TLE: we shift the work all in the query and it is not reasonable. This function is frequently called.

Improve:
when building the hashmap we build at each time who is the leader. And query becomes really easy

```cpp
    map<int, int> m; //time vs leader
    TopVotedCandidate(vector<int> persons, vector<int> times) {
        int n = persons.size(), lead = -1;
        unordered_map<int, int> count; //person vs vote
        for (int i = 0; i < n; ++i) m[times[i]] = persons[i];
        for (auto it : m) {
            if (++count[it.second] >= count[lead])lead = it.second;
            m[it.first] = lead;
        }
    }

    int q(int t) {
        return (--m.upper_bound(t))-> second;
    }
```	
			
### 718. Maximum Length of Repeated Subarray
max length of subarray in two arrays
approach 1: sliding window match sliding length n+m, each window compare max(n,m)
approach 2: dp for longest common substring
approach 3: rolling hash, really hard to understand.
dp is the way to go
dp[i,j] is the longest common subarray for A1[0...i-1] and A2[0..j-1]
when a1[i]==a2[j] then dp[i+1,j+1]=dp[i,j]+1

```cpp
    int findLength(vector<int>& A, vector<int>& B) {
		int m=A.size(),n=B.size();
		vector<vector<int>> dp(m+1,vector<int>(n+1));
		int ans=0;
		//boundary: 0th row and 0th col
		//0th row: empty vs B, 0th col: A vs empty
		for(int i=1;i<=m;i++)
		{
			for(int j=1;j<=n;j++)
			{
				if(A[i-1]==B[j-1]) dp[i][j]=dp[i-1][j-1]+1;
				ans=max(ans,dp[i][j]);
			}
		}
        return ans;
    }
```	

### 875. Koko Eating Bananas
a list of bananas
eating speed K bananas/hour
if the pile has less than k bananas, it just finish it and does not eat more
You have H hours
return the min K so that it can eat all bananas

Some big piles it may need several hours to finish.

very similar to the ship capacity
min is the max of max element and average
max is the total sum
find the first one satisfying H hours

```cpp
    int minEatingSpeed(vector<int>& piles, int H) {
		int l=1,r=accumulate(piles.begin(),piles.end(),0l);
		while(l<r)
		{
			long m=l+(r-l)/2;
			int hrs=get_time(piles,m);
			if(hrs>k) l=m+1;
			else r=m;
		}
		return l;
    }
	int get_time(vector<int>& piles,int k)
	{
		int ans=0;
		for(int p: piles)
			ans+=p/k+(p%k!=0);
		return ans;
	}
```
pay attention to overflow

### 153. Find Minimum in Rotated Sorted Array
this is a very classic binary search problem.
the answer lies [0,n-1] inclusive

with duplicates or without duplicates
A[m-1]>A[m]<A[m+1].
1. initial boundary is [0, n-1]
2. how to shrink the range
the left will shift from 0 to the pivot and cannot move any more
the right will shift from n-1 to the pivot and cannot move any more
the pivot is a part of the right. so r=m
we can only compare with the right value, otherwise when left move to pivot and it compare with the right
the left will shift to the right and miss the range.

3. when to stop? l<r or l<=r? == the invariant does not hold we need use <.

```cpp
     int findMin(vector<int>& nums) {
		int l=0,r=nums.size()-1;
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(nums[m]<nums[r]) r=m; //<= also works since there is no duplicate
			else l=m+1;
		}
		return nums[l];
    }
```	

### 154. Find Minimum in Rotated Sorted Array II
with duplicates
the pivot must satisfy A[p-1]>A[p]<=A[p+1]
if all the same, any position is OK.

1. initial condition is [0, n-1]
2. similarly we need design how to shrink the range to the pivot
the special case A[m]==A[r], we need shrink the right by 1 

3. l<r when l==r break the loop

```cpp
	int findMin(vector<int>& nums) {
		int l=0,r=nums.size()-1;
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(nums[m]<nums[r]) r=m;
			else if(nums[m]>nums[r]) l=m+1;
			else r--;//nums[m]==nums[r], this same thing cannot be the solution
		}
		return nums[l];
	}
```


### 528. Random Pick with Weight
w[i] is the weight of index i
randomly pick the index proporitional to the weight
the range is [0,sum]. rand()%(sum+1) will fall into this range.
if we accumulate the weight, and we can use binary search to find the number.

```cpp
	vector<long> prefix;
    Solution(vector<int>& w) {
		prefix.resize(w.size());
		prefix[0]=w[0];
        for(int i=1;i<w.size();i++)
			prefix[i]=prefix[i-1]+w[i];
    }
    
    int pickIndex() {
        int r=rand()%prefix.back();//+prefix[0];
		int ind=upper_bound(prefix.begin(),prefix.end(),r)-prefix.begin();
		return ind;
    }
```	
the random is from 0 to sum-1 so upper bound is fine

### 436. Find Right Interval
for each interval, find the minimum right interval's index. If there is no right interval return -1
right: start>=previous end, minimum is the min of the start position
approach: store the start position and index and sort. then use each interval's end point to binary search its lower_bound

```cpp
    vector<int> findRightInterval(vector<Interval>& intervals) {
        map<int, int> hash;
        vector<int> res;
        int n = intervals.size();
        for (int i = 0; i < n; ++i)
            hash[intervals[i].start] = i;
        for (auto in : intervals) {
            auto itr = hash.lower_bound(in.end);
            if (itr == hash.end()) res.push_back(-1);
            else res.push_back(itr->second);
        }
        return res;
    }
```
we can use map since there are no duplicate start point

### 162. Find Peak Element	
good example on binary search
1. answer lies [0, n-1] since both ends are negative infinity with A[l]>A[l-1] and A[r]>A[r+1]
2. how to shrink the range
if A[m]>A[m-1], we keep the left valid if we move l to m.
if A[m]>A[m+1], we keep the right valid if we move r to m.
from previous experience we know if we use two different stands, we may miss the correct range.
A[m]>A[m+1] r=m
else (A[m]<=A[m+1]) l=m+1
3. that is from left side we will reach l=peak so break when l==r
```cpp
    int findPeakElement(vector<int>& nums) {
        int l=0,r=nums.size()-1;
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(nums[m]>nums[m+1]) r=m;
			else l=m+1;
		}
		return l;
	}
```	
    
### 74. Search a 2D Matrix
the matrix is sorted in c++ storage
efficiently convert it to a 1D problem O(logN^2)
```cpp
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.empty()) return 0;
        int m=matrix.size(),n=matrix[0].size();
        int l=0,r=m*n-1;
        while(l<=r)
        {
            int mid=l+(r-l)/2;
            int row=mid/n,col=mid%n;
            int t=matrix[row][col];
            if(t==target) return 1;
            if(t<target) l=mid+1;
            else r=mid-1;
        }
        return 0;
    }
```
often to forget that r is defined and again define r for row, very hard to debug!

	
### 240. Search a 2D Matrix II	
row is sorted, col is sorted

check if a target exist

from bottom left to top right, use 2d binary search
for example search 12 in matrix O(m+n)

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
18->10->13->6->9->16->12
```cpp
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
		int m=matrix.size(),n=matrix[0].size();
		int i=m-1,j=0;
		while(i>=0 && j<n)
		{
			if(matrix[i][j]==target) return 1;
			if(matrix[i][j]<target) j++;
			else i--;
		}
		return 0;
	}
```

### 300. Longest Increasing Subsequence
approach 1: dp
dp[i] is the longest increasing subsequence ending with a[i]
dp[i]=max(dp[i],dp[j]+1) if a[i]>a[j]
```cpp
    int lengthOfLIS(vector<int>& nums) {
		int n=nums.size();
		vector<int> dp(n,1);
		int ans=1;
		for(int i=1;i<n;i++)
		{
			for(int j=i-1;j>=0;j--)
				if(nums[i]>nums[j]) dp[i]=max(dp[i],dp[j]+1);
			ans=max(ans,dp[i]);
		}
		return ans;
    }
```
O(N^2)
approach 2: binary search
need build a new data structure so that we can search
maintain a tail array. if it is larger than all tails, add to the back
else update the tail
for example [4,5,6,3]
add 4, [4]
add 5, [4,5]
add 5, [4,5,6]
add 3, [3,5,6]
  
```cpp
int lengthOfLIS(vector<int>& nums) {
    vector<int> res;
    for(int i=0; i<nums.size(); i++) {
        auto it = lower_bound(res.begin(), res.end(), nums[i]); //>=
        if(it==res.end()) res.push_back(nums[i]);
        else *it = nums[i];
    }
    return res.size();
}
```

### 658. Find K Closest Elements *****
sorted array given x and k find the elements k closest to x. (k is the number)
if there is a tie choose the smaller one

1. the answer is contiguous suppose it is a[i] to a[i+k-1]. this is similar to find one of the closest element.

2. binary search i

3. if x-a[mid]>a[mid+k]-x it means a[mid+1] to a[mid+k] is better, so we need have left=mid+1

```cpp
    vector<int> findClosestElements(vector<int>& A, int k, int x) {
        int left = 0, right = A.size() - k;
        while (left < right) {
            int mid = (left + right) / 2;
            if (x - A[mid] > A[mid + k] - x)
                left = mid + 1;
            else
                right = mid;
        }
        return vector<int>(A.begin() + left, A.begin() + left + k);
    }
```

### 497. Random Point in Non-overlapping Rectangles
a list of axis-aligned rectangle.
randomly and uniformly point in the covered area
rects are non-overlapping

x is bounded by  xmin and xmax, but segmented
y is also bounded by ymin and ymax, but segmented.

approach: 
1. randomly choose a rect, (using area and sum of all areas
2. randomly choose a point in the rect

```cpp
    vector<vector<int>> rects;
    
    Solution(vector<vector<int>> rects) : rects(rects) {
    }
    
    vector<int> pick() {
        int sum_area = 0;
        vector<int> selected;
        
        /* Step 1 - select a random rectangle considering the area of it. */
        for (auto r : rects) {
            /*
             * What we need to be aware of here is that the input may contain
             * lines that are not rectangles. For example, [1, 2, 1, 5], [3, 2, 3, -2].
             * 
             * So, we work around it by adding +1 here. It does not affect
             * the final result of reservoir sampling.
             */
            int area = (r[2] - r[0] + 1) * (r[3] - r[1] + 1);
            sum_area += area;
            
            if (rand() % sum_area < area)
                selected = r;
        }
        
        /* Step 2 - select a random (x, y) coordinate within the selected rectangle. */
        int x = rand() % (selected[2] - selected[0] + 1) + selected[0];
        int y = rand() % (selected[3] - selected[1] + 1) + selected[1];
        
        return { x, y };
    }
```

### 275. H-Index II	
citations in sorted order
each citation is the citation of a researcher's paper.
H-index: if the research has N papers, h of them have >=h citations, and N-h<=h
[0,1,3,5,6] h index is 3
[0,1,2,4,5,6] h index is also 3

this is to find a number to break into two parts. all left <=h and all right<=h.
if there are multiple h, choose the max one.


we are searching pattern 0 0...1,1...1,0,0,... the last true
what defines a true: if we choose a[m]: h=n-a[m], 

do not misunderstand: max h, minimize the right part.

to find the last true:
when right side is false, we move to next r=m
left side even it is 1 we need to move to right, l=m+1
```cpp
    int hIndex(vector<int>& citations) {
        int n=citations.size(),l=0,r=n-1;
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(citations[m]>n-m) r=m; //
			else l=m+1;
		}
		return n-l;
	}
```
for example [0,1,3,5,6]
m=2, c[m]=3, 3>5-2? false, l->3, r=4
l=3, 5-(3-1)=3

for example [0,1,2,4,5,6]
m=2, c[m]=2, 2>6-2 false, l=3,r=5
m=4, c[m]=5, 5>6-4 true, l=3 r=4
m=3, c[m]=4, 4>6-3 true, l=3, r=3
return l=3 6-(3-1)=4 wrong!

as we see we are asking for the right first true position.

```cpp
    int hIndex(vector<int>& citations) {
        if (citations.empty()) return 0;
        int , n = citations.size(),l = 0, r = min(n, citations.back());
        while (l < r) 
		{
            int m = l + (r - l + 1) /2; 
            if (citations[n-m] >= m) l = m; 
            else r = m-1;
        }
        return r;
    }
```

### 209. Minimum Size Subarray Sum
Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead

approach 1:
prefix sum will form a sorted array. then we can use two pointer to do a sliding window

```cpp
    int minSubArrayLen(int s, vector<int>& nums) {
		for(int i=1;i<nums.size();i++) nums[i]+=nums[i-1];
		int i=0,j=1;
		int ans=INT_MAX;
		while(j<nums.size())
		{
			if(nums[j]-nums[i]<s) j++;
			else
			{
				ans=min(ans,j-i);
				i++;
			}
		}
		return ans==INT_MAX?0:ans;
    }
```
Note above is buggy since it does not cover the first index. (i,j].
for example [1,2,3,4,5] and s=15. adding a zero ahead can solve the problem
or we can do it on the fly by moving the old head out

approach 2:
for each prefix, find the prefix+s (lower_bound) and get the min distance

### 34. Find First and Last Position of Element in Sorted Array
that is stl::equal_range
the pattern is 0,0...0,1,..1,0,...0
we need to find the first 1 and last 1

approach 1: use equal_range or lower_bound /upper_bound
approach 2: use binary search
if A[m]>target we need r=m-1 so that we can move to 1
A[m]<target, we need l=m+1 so that we can move to 1
we will stop:
the right goes to the first 1
the left goes to the last 1
that is two binary search problem

```cpp
    vector<int> searchRange(vector<int>& nums, int target) {
		int n=nums.size(),l=0,r=n-1;
		int left,right;
		while(l<r) //find the last one ==target
		{
			int m=l+(r-l)/2;
			if(nums[m]<=target) l=m+1;
			else r=m;
		}
		right=l-1;
		
		l=0,r=n-1;
		while(l<r) //find the first one ==target
		{
			int m=l+(r-l)/2;
			if(nums[m]<target) l=m+1;
			else r=m-1;
		}
		left=l;
		return {left,right};
    }
```	
buggy

correct:
```cpp
vector<int> searchRange(int A[], int n, int target) {
    int i = 0, j = n - 1;
    vector<int> ret(2, -1);
    // Search for the left one, the first true
    while (i < j)
    {
        int mid = (i + j) /2;
        if (A[mid] < target) i = mid + 1;
        else j = mid;
    }
    if (A[i]!=target) return ret;
    else ret[0] = i;
    
    // Search for the right one
    j = n-1;  // We don't have to set i to 0 the second time.
    while (i < j)
    {
        int mid = (i + j) /2 + 1;	// Make mid biased to the right
        if (A[mid] > target) j = mid - 1;  
        else i = mid;				// So that this won't make the search range stuck.
    }
    ret[1] = j;
    return ret; 
}
```

very intersting. the mid can be biased to left or right by adding 1.


### 33. Search in Rotated Sorted Array
the target either on the left or right side
common mistake: compare with a[l] or a[r] to determine side. L will get into right side.
for example sorted array or shrink to the pivot.

approach 1: find the pivot point and then search in sorted array
approach 2: directly binary search

a[m]>A[0], it is on the left side
else m on the left side

a[m]>a[0]:
 target>a[m]: l=m+1
 else r=m //target could be on left or right
else
	target<=a[m]: r=m
	else //target could be on left or right
 
```cpp
	int search(vector<int>& A,int target)
	{
		int n=A.size(),l=0,r=n-1;
        if(!n) return -1;
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(A[m]==target) return m;
			if(A[l]<=A[m]) //left side sorted
			{
				if(target>=A[l] && target<A[m]) //target in [l,m)
					r=m-1;
				else //target in (m,r]
					l=m+1;//why m-1 since target!=A[m]
			}
			else //m in right side
			{
				if(target>A[m] && target<=A[r]) //target in (m,r]
					l=m+1;
				else
					r=m-1; //target in [l,m)
			}
		}
		return A[l]==target?l:-1;
	}
```

note target==A[m] is already removed , so r=m-1	
if we set r=m we will fall into infinite loop.

### 81. Search in Rotated Sorted Array II
with duplicates
approach 1: find the pivot and search in sorted array

```cpp
    bool search(vector<int>& nums, int target) {
		int n=nums.size(),l=0,r=n-1;
		if(!n) return 0;
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(nums[m]>nums[l]) //[l,m] is sorted
				l=m+1; 
			else if(nums[m]<nums[l]) //m is behind the pivot
				r=m;
			else //nums[m]==nums[l]
				l++; //l is not the pivot
		}
		int pivot=l;
		//now search in the sorted array
		l=0,r=n-1;
		while(l<=r)
		{
			int m=l+(r-l)/2;
			int rm=(m+pivot)%n;
			int t=nums[rm];
			if(t==target) return rm;
			
			if(t>target) r=m-1;
			else l=m+1;
		}
		return -1;
    }
```
buggy: why we cannot use nums[l] to compare with [m] 
since the m is left biased, and for two element it will compare with itself

so change to compare with r.
but keeps r-- is also problematic. it will miss the range.
1,1,1,1,1,2,1, do like this the answer is skipped.
it is problematic.

If we split the array with mi into [lo, mi] and [mi, hi]. If [lo, mi] is not sorted, since we detect [lo, mi] is not sorted by nums[lo] > nums[mi] so nums[lo] cannot be min, min must be within (lo, mi]. If [mi, hi] is not sorted, min must be within (mi, hi] - since we detect [mi, hi] is not sorted by nums[mi] > nums[hi], nums[mi] cannot be min. If they are both sorted, nums[lo] is the min.
There are 4 kinds of relationship among num[lo], nums[mi], nums[hi]

nums[lo] <= nums[mi] <= nums[hi], min is nums[lo]
nums[lo] > nums[mi] <= nums[hi], (lo, mi] is not sorted, min is inside
nums[lo] <= nums[mi] > nums[hi], (mi, hi] is not sorted, min is inside
nums[lo] > nums[mi] > nums[hi], impossible

correct version to find the pivot with duplicates
```cpp
    bool search(vector<int>& nums, int target) {
		int n=nums.size(),l=0,r=n-1;
		if(!n) return 0;
		while(l<r)
		{
			int m=l+(r-l)/2;
            if(nums[l]<nums[r]) break;
            if (nums[m] > nums[r])  
                l = m + 1;
            else if (nums[m] < nums[l]) 
                r = m;
            else
            {  // nums[lo] <= nums[mi] <= nums[hi] 
                if(nums[l]==nums[l+1]) l++;
                if(nums[r]==nums[r-1]) r--;
            }
		}
		int pivot=l;
        //cout<<pivot;
		//now search in the sorted array
		l=0,r=n-1;
		while(l<=r)
		{
			int m=l+(r-l)/2;
			int rm=(m+pivot)%n;
			int t=nums[rm];
			if(t==target) return 1;
			
			if(t>target) r=m-1;
			else l=m+1;
		}
		return 0;
    }
```
*. first, only l++ and r-- when tere are duplicates l=l+1 and r=r-1
*. second, do not use while since it jump the judge l<r
*. when nums[l]<nums[r] the l to r is sorted shall exit early

approach2: similar to the non-duplicated one
```cpp
    bool search(vector<int>& A, int target) {
		int n=A.size(),lo=0,hi=n-1;
        if(!n) return 0;
        int mid = 0;
        while(lo<hi){
          mid=(lo+hi)/2;
          if(A[mid]==target) return true;
          if(A[mid]>A[hi]){
              if(A[mid]>target && A[lo] <= target) hi = mid;
              else lo = mid + 1;
          }else if(A[mid] < A[hi]){
              if(A[mid]<target && A[hi] >= target) lo = mid + 1;
              else hi = mid;
          }else{
              hi--;//this could not be the answer so it is safe.
          }
          
    }
    return A[lo] == target ? true : false;
  }
```
  
## hard
### 778. Swim in Rising Water
nxn grid from (0,0) to (n-1,n-1). grid[i,j] is the elevation at (i,j). (consider it is a wall)
you can go to any 4 directions at no time only when the elevation is <=current elevation.
at time t: depth of water is t everywhere.
return the least time to get to bottom right.

Analysis: at time t, there shall exist a path from top left to bottom right.
binary search the first true condition
0,0,...0,1,1....1
find a path can use dfs
range: min and max of the elevation.
```cpp
    int swimInWater(vector<vector<int>>& grid) {
        int n=grid.size(),l=0,r=n*n-1;
        while(l<r)
        {
            int m=l+(r-l)/2;
            if(dfs(grid,m,0,0)) r=m;
            else l=m+1;
            cout<<m<<endl;
        }
        return l;
    }
    bool dfs(vector<vector<int>>& grid,int d,int i,int j)
    {
        int n=grid.size();
        if(i<0 || j<0 ||i>=n ||j>=n) return 0;
        
        if(grid[i][j]>d) return 0;
        if(i==n-1 && j==n-1) return 1;
        int t=grid[i][j];
        grid[i][j]=INT_MAX;
        bool res=dfs(grid,d,i-1,j)||dfs(grid,d,i+1,j)||dfs(grid,d,i,j-1)||dfs(grid,d,i,j+1);
        //grid[i][j]=t;//once restored, the dead trials are retried and cause infinite loop
		
        return res;
    }
```	
dfs will enter into infinite loop why?
fix: use a curr=grid, and restore it after a search

### 410. Split Array Largest Sum
Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.
so m parts we can get multiple solutions
equivently we can give the target sum and divide into parts
min largest sum is the largest single element

convert to equivalent problem. 
```cpp
    int splitArray(vector<int>& nums, int m) {
        long l=*max_element(nums.begin(),nums.end());
        long r=accumulate(nums.begin(),nums.end(),0l);
        while(l<r)
        {
            long mid=l+(r-l)/2;
            int nparts=get_numparts(nums,mid);
            
            if(nparts>m) l=mid+1; //too small need increase
            else r=mid;
        }
        return l;
    }
    int get_numparts(vector<int>& nums,long target)
    {
        int ans=0;
        long sum=0;
        for(int i: nums)
        {
            if(sum+i>target) {sum=0;ans++;}
            sum+=i;
        }
        return ans+(sum!=0);
    }
```

668. Kth Smallest Number in Multiplication Table
this is sorted matrix, row sorted and col sorted.
height mxn
similar to 378. only difference is mxn instead of nxn.
378 has two approaches:
1. priority queue (min heap and adding current min's next larger element into it.
multiplication table has a lot of duplicates.
2. binary search. 

multiplication table at M(i,j)=(i+1)*(j+1)
so the matrix is symmetric. 

similarly we can use binary search. 
given the mid value, we count number of elements <=mid
for a value mid, we may have duplicates. 
so when cnt>=k it may be an answer r=m


```cpp
    int findKthNumber(int m, int n, int k) {
		int l=1,r=m*n;
		while(l<r)
		{
			int mid=l+(r-l)/2;
			int cnt=cntle(mid);
			if(cnt>=k) r=m;
			else l=m+1;
		}
		return l;
    }
	int cntle(int mid,int m,int n) //count numbers <=mid
	{
		int ans=0;
		for(int i=1;i<=m;i++)
			ans+=min(mid/i,n);
		return ans;
	}

```	

### 786. K-th Smallest Prime Fraction
a list of prime numbers
for every p<q in the list forms the fraction. return the kth fraction.

similar to multiplication table, but now forms a upper matrix of division table.

every row is sorted from right to left.
every col is sorted from top to down.

define the search range: min is 1/max, max is 2nd_max/max we can use 1.
do we use double? bad idea, we need calculate all the combinations. and store in a matrix
we can use double m and m*denominator to find the divisor

```cpp
    vector<int> kthSmallestPrimeFraction(vector<int>& A, int K) {
		double l=0, r=1;
		int n=A.size();
		int p,q;
		while(l<r)
		{
			double m=l+(r-l)/2;
			int cnt=cntle(A,m,p,q);
            //cout<<m<<":"<<cnt<<endl;
			if(cnt>K) r=m;
			else if(cnt<K) l=m;
			else return {p,q};
		}
        return {p,q};
    }
	int cntle(vector<int>& A,double v,int&p,int& q)
	{
		int ans=0;
		int n=A.size();
		p=0,q=1;
		for(int i=0;i<n-1;i++) //upper triangle
		{
			for(int j=i+1;j<n;j++)
				if(A[i]<=A[j]*v) //A[i]/A[j]<=v
				{
					ans+=n-j;//ith line starts from i+1
					if(p*A[j]<q*A[i]) {p=A[i],q=A[j];} //p/q<A[i]/A[j]
					break;
				}
		}
		return ans;
	}
```

### 719. Find K-th Smallest Pair Distance
	
Given an integer array, return the k-th smallest distance among all the pairs. The distance of a pair (A, B) is defined as the absolute difference between A and B.
approach 1: find all pair's distance and put in pq
sort the input, then we have a very similar upper matrix as 785, only difference is subtraction

the distance is sorted in row.
range 0 to max-min
```
    int smallestDistancePair(vector<int>& nums, int k) {
        sort(nums.begin(),nums.end());
		int n=nums.size(),l=0,r=nums[n-1]-nums[0];
		while(l<r)
		{
			int m=l+(r-l)/2;
			int cnt=cntle(nums,m);
			if(cnt>=k) r=m;
			else l=m+1;
		}
		return l;
	}
	int cntle(vector<int>& nums,int v)
	{
		int ans=0;
		for(int i=0;i<nums.size()-1;i++)
		{
			for(int j=nums.size()-1;j>i;j--)
				if(nums[j]-nums[i]<=v) {ans+=j-i;break;}
		}
		return ans;
	}
```

### 793. Preimage Size of Factorial Zeroes Function
k up to 1e9
0 is depends on the number of factor of 5.
1x5,2x5,3x5,4x5,5x5, once we have 5^n will has extra 5.
for n the number of 5 is n/5+n/5^2+n/5^3+....

```cpp
	int preimageSizeFZF(int K)
	{
		long l=0,r=5l*(K+1);
		while(l<r)
		{
			long m=l+(r-l)/2;
			int cnt=num5_factor(m);
			if(cnt>=K) r=m;
			else l=m+1;
		}
		int lower=l;
		l=0,r=5l*(K+1);
		while(l<=r)
		{
			long m=l+(r-l)/2;
			int cnt=num5_factor(m);
			//we are looking for the lower range and upper range
			if(cnt>K) r=m-1;
			else l=m+1;
		}
		return r-lower+1;
	}
	long num5_factor(long n)
	{
		long ans=0;
		while(n)
		{
			ans+=n/5;
			n/=5;
		}
		return ans;
	}
```
1. upper limit is 5*(k+1) this has more zeros than required
2. need use long
3. upper bound search need use <= and l=m+1, r=m-1 to reach the target.
4. also find the first k and k-1 also gives the same result.

### 363. Max Sum of Rectangle No Larger Than K
2d matrix, find the max sum of a rectangle in the matrix with sum<=k.

### 483. Smallest Good Base
find the smallest base for k so that it can be represented all as 1

apparently for n, n-1 satisfy the condition.
it asks for the smallest.

binary search: we have a base and to see if n can be represented as 1111...

n=x^m+x^m-1+.....+1=(x^(m+1)-1)/(x-1)

n-1=x*(x^m-1+.....1)
(n-1)/x can be represented as 111...
n-x^m=(n-1)/x->
nx-x^(m+1)=n-1->
x^(m+1)=nx-n+1->
x^(m+1)-1=n*(x-1)->
[x^(m+1)-1]/(x-1)=n (actually this is a exponential series and do not need derivation)

search base 2 to n-1
x^m-1=n(x-1)
n(x-1)>x^m-1 x is too large (m is variable)
n(x-1)<x^m-1 x is too small (m is a variable)

so we do not need to check all x, but only the factor of n-1
for example 13
candidates are 12's factors, 12, 6, 4, 3, 2
12: 11
6: not possible
4: not possible 
3: 9+3+1 
2: not possible
x^(m-1)<n<x^m 
iterate from the largest possible m to smallest m.
when m is fixed. we can binary search the answer.

```cpp
    string smallestGoodBase(string n) {
		long num=stol(n);
		for(int m=log(num+1)/log(2);m>2;m--)
		{
			long l=pow(num,1.0/m);
			long r=pow(num,1.0/(m-1));
			while(l<=r)
			{
				long mid=l+(r-l)/2;
				long t=0;
				for(int i=0;i<m;i++) t=t*mid+1;
				if(num==t) return to_string(mid);
				if(num<t) r=mid-1;
				else l=mid+1;
			}
		}
		return to_string(num-1);
    }
```

### 862. Shortest Subarray with Sum at Least K	
if not found return -1
sum>=K
it may contain negative, K may be also negative
1. K is negative, the max element >= K 
2. K is positive, but numbers include negatives
can be approached using binary search
given a length, can we get a sum >=K? using prefix sum.
note: if answer is not in the range, we cannot find it.
if answer is not inside, all false. will go to l=n+1

```cpp
    int shortestSubarray(vector<int>& A, int K) {
		int tmax=*max_element(A.begin(),A.end());
        if(tmax>=K) return 1; 
        if(K<=0) return -1;
        
        A.insert(A.begin(),0);
        for(int i=1;i<A.size();i++) A[i]+=A[i-1];
		int l=1,r=A.size();
		while(l<r)
		{
			int m=l+(r-l)/2;
			bool t=sumge(A,m,K);
			if(t) r=m;
			else l=m+1;
            cout<<l<<" "<<r<<endl;
		}
		return l<A.size()?l:-1;
	}
	
	bool sumge(vector<int>& prefix,int m,int target)
	{
		bool t=0;
		for(int i=m;i<prefix.size();i++)
			if(prefix[i]-prefix[i-m]>=target) {t=1;break;}
		return t;
	}
```

this strategy has problem:
since it cannot return true for a continuous parts so we cannot guarantee the answer.
ie there is no monotonically property.
we shall only search those monotonically increasing part.

sliding window approach.
for all positives, we can increase the ending pointer until it satisfy sum>=K. and then increase the first pointer
with negatives inside, this is not valid, since when sum<K increase the first pointer is also an option
using a deque:
assuming we store the prefix sum,
when prefix < the deque back, remove the back, so we maintain a increasing sequence
when prefix -front >=K we pop front and compare the distance.
```cpp
    int shortestSubarray(vector<int> A, int K) {
        int N = A.size(), res = N + 1;
        vector<int> B(N + 1, 0);
        for (int i = 0; i < N; i++) B[i + 1] = B[i] + A[i];
        deque<int> d;
        for (int i = 0; i < N + 1; i++) {
            while (d.size() > 0 && B[i] - B[d.front()] >= K)
                res = min(res, i - d.front()), d.pop_front();
            while (d.size() > 0 && B[i] <= B[d.back()]) d.pop_back();
            d.push_back(i);
        }
        return res <= N ? res : -1;
    }
```
which is similar to those stack problems .

https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/discuss/204290/Monotonic-Queue-Summary

### 174. Dungeon Game	
dp solution
can also using binary search since it gives 0,0,0.1,1,1,pattern given the initial health
negatives: losing health
positives: adding health
from top left to bottom right
the range is 0 to -sum(negatives)

### 4. Median of Two Sorted Arrays
approach 1: merge sort and return the median O(n+m)
approach 2: binary search
the median will separate smaller and larger into equal parts
may contain duplicates, we can count >= at the same time.
we can count.
1. first find the target element in the lists to separate less = (n+m-1)/2
```cpp
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n=nums1.size(),m=nums2.size();
		int less
    }

suppose we cut the A at i and B at j
left: A[0]....A[i-1], right: A[i]....A[n-1]
left: B[0]....B[j-1], right: B[j]....A[m-1]
A[i-1]<=B[j]
B[j-1]<=A[i]
median: [max(left)+min(right)]/2
i+j=n-i+m-j (even)
i+j=n-i+m-j+1 (odd, put the number in the left side)

searching i in (0,n) where j=(m+n+1)/2-i (this works for even and odd)
j==0 || i==n || B[j-1]<=A[i] &&
i==0 || j==m || A[i-1]<=B[j]

binary search:
j>0 && i<m && B[j-1]>A[i]: i is too small
i>- && j<n && A[i-1]>B[j]: i is too large

### 927. Three Equal Parts
approach 1: ignore leading and trailing zeros. divide the 1s into 3 parts.
O(n) solution

### 710. Random Pick with Blacklist
given a blacklist in the range [0,N), random shall not include these numbers
approach 1: effectively reduce the length when rand()%M
when searching the correct index, (after a blacklist number inserted, all below needs add 1)

approach 2: remap the blacklist to the tail of the numbers

# Chapter 3. dynamic programming

### 338. Counting Bits<br/>
given a number get all number of 1s in binary for each one.<br/> dp[i]=dp[i/2]+i&1

### 877. Stone Game<br/>
a list of numbers, picking from either end. who wins<br/>
we pick positive, the other pick negative, maximize the score. (for the other player it is the same)<br/>

```cpp
    int helper(vector<int>& piles,vector<vector<int>>& dp,int l,int r)
    {
        if(l>r) return 0;
        if(dp[l][r]!=INT_MAX) return dp[l][r];
        dp[l][r]=max(piles[l]-helper(piles,dp,l+1,r),piles[r]-helper(piles,dp,l,r-1));
        return dp[l][r];
    }
```

### 931. Minimum Falling Path Sum<br/>
element + min(previous row closest three sum)

### 647. Palindromic Substrings<br/>
dp[i,j] is the number of p-string for S(i...j)<br/>
self+dp[i,j-1]+dp[i+1,j]-dp[i+1,j-1]<br/>
iterate i reverse, j normal<br/>

### 413. Arithmetic Slices<br/>
dp[i] means the number of arithmetic slices ending with A[i]<br/>
dp[i]=dp[i-1]+1 if it satisfy arithmetic<br/>
answer is the accumulation<br/>

### 712. Minimum ASCII Delete Sum for Two Strings<br/>
edit distance dp[i,j]

### 714. Best Time to Buy and Sell Stock with Transaction Fee<br/>
dp[i]=max(dp[i-1],dp[j]+price[i]-price[j]-fee) for j=0 to i-1<br/>
sell or not sell on ith day<br/>

### 646. Maximum Length of Pair Chain<br/>
sort the chain according to its start.<br/>
dp[i] is the number of longest chain for ith pair, it shall be the previous chain+1

### 343. Integer Break<br/>
make the product the largest.<br/>
j*(i-j) vs j*dp[i-j], break into j and i-j two numbers<br/>

### 638. Shopping Offers<br/>
better using recursive for this

### 357. Count Numbers with Unique Digits<br/>
dp[i] is the number from 10^(i-1) 10^i<br/>
first digit: 1-9<br/>
second digit: 0-9 but cannot use previous digit, 9<br/>
9*9*8*7.....

### 486. Predict the Winner<br/>
same as 877, but we can use direct dp max(num[l]-dp[l+1,r],num[r]-dp[l,r-1])<br/>
l: reverse iteration, r normal iteration<br/>

### 650. 2 Keys Keyboard<br/>
copy all and paste two operations<br/>
dp[i]=dp[j]+i/j if i%j==0. paste j for i/j times, find the max j.<br/>

### 392. Is Subsequence<br/>
dp: by deleting char to see if we can reach to the other string<br/>
greedy: always match the first char using two pointers

### 62. Unique Paths<br/>
easy

### 494. Target Sum<br/>
using + and - to reach the target sum. how many ways?<br/>
negative sum N and positive sum P
- P+N=target
- P-N=total so P=(S+T)/2
it reduces to get the number of ways to reach a new target sum.<br/>
and it is a knapsack problem without repetition<br/>

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

### 740. Delete and Earn<br/>
delete num[i] and earn points num[i] and the same time delete num[i]-1 and num[i]+1. max the points<br/>
this is house robber with duplicates (if there are a lot of value=nums[i]). So we use bucket sort (bucket distance is 1)<br/>

### 516. Longest Palindromic Subsequence<br/>
again can convert to edit distance. by reversing the string we are looking for the largest common subsequence.<br/>

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

### 64. Minimum Path Sum<br/>
from top left to bottom right<br/>
dp[i][j]=min(dp[i-1][j],dp[i][j-1])+grid[i][j];<br/>

### 96. Unique Binary Search Trees<br/>
using node 1 to n, get the number of unique BST<br/>
if i is the root, we have 1 to i-1 in the left and i+1 to n in the right. both are subproblem.<br/>
The number is dp[i]=sum(dp[j-1]*dp[i-j]) j from 1 to i<br/>
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

### 377. Combination Sum IV<br/>
give a target, get the number of combinations that sums to the target<br/>
knapsack with repetition: for smaller target, we shall iterate over all the numbers.<br/>
Note: it also includes climbing stairs, if we have multiple methods to reach i, we need sum them up.<br/>

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

### 718. Maximum Length of Repeated Subarray<br/>
larget common substr<br/>

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

### 309. Best Time to Buy and Sell Stock with Cooldown<br/>
after sell, you have to rest one day before buy stock<br/>
on ith day:<br/>
if we sell, the profit is dp[j-2]+price[i-2]-price[j-2] (buy at jth day, at least two days before ith day)<br/>
if we not sell, the profit is dp[i-1]<br/>
note: price[i-2] and price[j-2] is the price at ith and jth day (since we add two guarding elements)<br/>
dp[j-2] is the profit before we buy at jth day (sell at two days ago)<br/>

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

### 813. Largest Sum of Averages<br/>
partition the list into at most k parts and make the sum of average the max.<br/>
assuming dp[i,k] is the max sum of average for list len=i, and k groups, then we check every j to add one more group<br/>
dp[i][k]=max(dp[i][k],dp[j][k-1]+double(A[i]-A[j])/(i-j));

### 764. Largest Plus Sign<br/>
in four directions, using dp to get the largest radius (including itself)<br/>
then the larget radius is the min of the 4 directions<br/>

### 688. Knight Probability in Chessboard<br/>
at most k moves, the probability that the knight in the board<br/>
8 directions, out of board is 0, stay in board is 1<br/>
dp[k, i, j]=sum(dp[k-1,m,n)/8 m,n is the next 8 possible directions<br/>
we can effectively remove the k dimension<br/>

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

### 698. Partition to K Equal Sum Subsets<br/>
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
### 300. Longest Increasing Subsequence<br/>
dp[i] is the max length subsequence ending at i.<br/>
if(nums[j]<nums[i]) dp[i]=max(dp[i],dp[j]+1);<br/>

### 279. Perfect Square<br/>
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
### 416. Partition Equal Subset Sum<br/>
knapsack without repetition. so iteration on elements shall be outside<br/>
then solve all the smaller problems<br/>
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

### 474. Ones and Zeroes<br/>
with m 0s and n 1s, get the max number of string in dictionary<br/>
knapsack with more complexity<br/>
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

### 120. Triangle<br/>
min path sum

### 375. Guess Number Higher or Lower II<br/>
when guess wrong, you need pay that amount<br/>
min cost to ensure win<br/>
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

### 376. Wiggle Subsequence<br/>
return the max length of the wiggling subsequence<br/>
- by tracking the number of down and ups
- when num[i]>num[i-1], we increase up, up[i]=dn[i-1]+1 (since the two are dependent)
- when num[i]<num[i-1], we increase down
- equal case: no change
- final answer shall be the max of up and down ending at the last element

### 264. Ugly Number II<br/>
only has factor of 2,3,5, find the nth ugly number<br/>
iteratively find the next ugly number, it shall be the min of previous *2 *3 *5<br/>
use 3 pointers i: *2, j *3, k: *5 represents the current 3 min ugly number.<br/>
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
### 808. Soup Servings<br/>
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

### 935. Knight Dialer<br/>
Get the number of different numbers given n presses.<br/>
just list next numbers for all given number.<br/>
this is similar to 688. knight probability<br/>
dp[k,i] is the number of different path using k presses, i the number ending at k times (or starting is also fine)<br/>

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

### 213. House Robber II<br/>
circular, this is two house robber 1 problem<br/>
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

### 790. Domino and Tromino Tiling<br/>
we have I shape and L shape, to build 2*N tile, what is the number of combinations<br/>
this is actually hard.<br/>
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
 
### 368. Largest Divisible Subset<br/>
find the largest subset which Si%Sj=0 (they are a part of geometric series)<br/>
dp with backtrace ability, we need keep the previous information.<br/>
dp with a bit complexity<br/>
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

### 95. Unique Binary Search Trees II<br/>
dp. combine with left and right
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

### 139. Word Break<br/>
break the word into dictionary words<br/>
iterate and break the problem into a word + a smaller subproblem.<br/>
direct dp:<br/>
we mark the possible cut positions and when we iterate more, we check [j, i] if is also a word in the dict.<br/>
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

### 467. Unique Substrings in Wraparound String<br/>
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
### 801. Minimum Swaps To Make Sequences Increasing<br/>
two arrays, swap at the same position to make both array sorted<br/>
two cases:<br/>
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
### 63. Unique Paths II-easy

### 673. Number of Longest Increasing Subsequence<br/>
solving two dp problem at the same time: find the longest subsequence, remembering the number of path to each of the longest subsequence<br/>
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

### 787. Cheapest Flights Within K Stops<br/>
define dp[i,k] is the min cost from start to j with at most k stops<br/>
the key is we add one stop j: dp[j,k-1]+price[i,j] to minimize the cost<br/>

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

### 898. Bitwise ORs of Subarrays<br/>
for all subarray, bit or of all elements, return number of possible results<br/>
for input ABC<br/>
A<br/>
A|B, B<br/>
A|B|C, B|C,C<br/>
so we take two set, one for final one for intermediate<br/>

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

### 221. Maximal Square<br/>
finding the max square with all 1s<br/>
similar to histogram but we uses dp<br/>
keep updating the height on each row.<br/>
keep updating its max range at j (left and right) using the height as the min height (this could be obtained using left max and right min dp process)<br/>

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

### 576. Out of Boundary Path<br/>
quite a few similar problems. using recursion<br/>
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

### 152. Maximum Product Subarray<br/>
largest product subarray<br/>
when a negative is involved, max becomes min, min becomes max<br/>
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

### 322. Coin Change-min number<br/>
classical knapsack with repetition: the amount shall be in outer loop<br/>
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
 
### 837. New 21 Game<br/>
this is like climbing stairs, there are more than two methods to reach a stair<br/>
 
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

### 464. Can I Win<br/>
It is important to get the problem right: the two player adds to the same sum, and who reach the target first wins
number chosen cannot be reused.<br/>
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

### 5. Longest Palindromic Substring<br/>
just naive soluion<br/>
try all position and expand<br/>

### 523. Continuous Subarray Sum<br/>
target: multiples of K<br/>
accumulate sum (prefix sum). the prefix sum shall %k has the same value<br/>

### 91. Decode Ways<br/>
A-Z decode as 1-26<br/>
given a digit string, return number of decoding ways<br/>
only have two options:
- the number itself 
- the number combine with previous digit

## hard

### 10. Regular Expression Matching
##### problem summary
'.' matches any single char and 
'*' matches 0 or more of preceding char

##### idea
1. two string direct dp problem, dp[i, j] represents if s[0...i-1] matches p[0...j-1]
2. p[j-1] is letter or ., dp[i, j]=dp[i-1, j-1]&&(s[i-1]==p[j-1]||p[j-1]=='.')
3. p[j-1]=='*', assuming previous char is a, then a*
  - matches 0 char, previous char is skipped, a* matches empty, dp[i,j]=dp[i,j-2] (s(0...i-1) matches p(0...j-3). Note: we are using previous solution for current solution!)
  - matches 1 char, then dp[i,j]=(s[i-1]==p[j-2] || p[j-2]=='.') && dp[i,j-1] (a* counts as one a, s(0..i-1) matches p[0..j-2])
  - matches more than 1 char, a* represents multiple a, dp[i,j]=(s[i-1]==p[j-2] || p[j-2]=='.') && dp[i-1,j] (depends s(0...i-2) matches p(0...j-1)). Actually this case is included in match 1 char.
4. boundary condition
  - dp[0,0]=1, empty vs empty
  - p empty, s non-empty, all false, col 0 shall not be included in loop.
  - s empty, p non-empty, must have .* letter* to match 0 char dp[0,i]=dp[0, i-2] && p[j-1]=='*'
5. extra complexity: it involves p[j-2] and dp[j-2], which indicates that row 1 needs to be included in boundary. A common approach is: we do not involve the boundary except col 0, but using the recurrence relation to process the boundary. This is much simpler in some cases when the boundary is not so straightforward.

##### code
```cpp
    bool isMatch(string s, string p) {
        int m=s.size(),n=p.size();
        vector<vector<bool>> dp(m+1,vector<bool>(n+1));
        dp[0][0]=1;
        //p is empty, s non-empty all false is straightforward dp[i][0]=0
        //put other boundary in loops
        for(int i=0;i<=m;i++)
        {
            for(int j=1;j<=n;j++)
            {
                if(p[j-1]!='*') dp[i][j]=i && dp[i-1][j-1] && (s[i-1]==p[j-1] || p[j-1]=='.');
                else dp[i][j]=(j>1 &&dp[i][j-2]) 
                    || (i && j>1 && dp[i][j-1] && (s[i-1]==p[j-2] || p[j-2]=='.'))
                    || (i && j>1 && dp[i-1][j] && (s[i-1]==p[j-2] || p[j-2]=='.'));
            }
        }
        return dp[m][n];
    }
  ```
  
  ##### comments
  1. when boundary is hard to write, include it in the loop
  2. a* matches empty, then it means previous solution shall be s(0..i-1) matches p(0..j-3), which is dp[i, j-2]
  3. a* matches 1, then it means previous solution is s(0..i-2) matches p(0..j-1) which is dp[i-1,j]
  4. the last condition can be skipped.

 ### 115. Distinct Subsequences.md
 ##### problem Summary
return the number of distinct subsequence of S which is equal to t.

##### idea
this is two string compare with only deletion in S is allowed,
The number of distinct subsequence is similar to climbing stairs.

if s[i-1]!=t[j-1], dp[i,j]=d[i-1,j] where we need skip this char
else we have two choices use this char or not: if we use dp[i,j]=dp[i-1,j-1]
if we do not use dp[i,j]=dp[i-1,j]

This is also similar to walking in a 2d matrix.

##### code
```cpp
    int numDistinct(string s, string t) {
        //dp edit distance
        int m=s.size(),n=t.size();
        vector<vector<int>> dp(m+1,vector<int>(n+1));
        //dp[i,j] represent number of subsequence for s(0...i) vs t[0..j]
        //allowing only deletion from s
        //boundary: dp[0,0]=1
        for(int i=0;i<=m;i++) dp[i][0]=1; //when t is empty, need delete all s
        for(int i=1;i<=m;i++)
        {
            for(int j=1;j<=n;j++)
            {
                if(s[i-1]!=t[j-1]) dp[i][j]=dp[i-1][j]; //need delete this char, i proceed one
                else dp[i][j]=dp[i-1][j-1]+dp[i-1][j]; //delete: i proceed one, keep: dp[i-1][j-1]
            }
        }
        return dp[m][n];
    }
```

##### comments
- the key is when we delete char i-1, why it is dp[i-1,j] instead of dp[i-1,j-1]?

### 132. Palindrome Partitioning II.md
##### problem summary
find the min number of cut to make s a list of palindrome strings

##### idea
assuming dp[i] is the number of min cut at position i, previous cut position is at j
s[i..j] has to be a palindrome string then
dp[i]=min(dp[i],dp[j]+1) for all dp[j]>0 && s[i..j] is palindrome

fixing the end is kind of clumsy for palindrome problem, if we consider i as the center and grows both end:
1. odd length with center at i and with radius j, s[i-j, i+j]
2. even length with center at i+0.5 and with radius j, s[i-j+1,i+j]

dp[i+j+1]=min(dp[i+j+1],dp[i-j]+1) odd
dp[i+j+1]=min(dp[i+j+1],dp[i-j+1]+1) even

Boundary condition:
max number of cuts for s with i length s[i]=i-1 (by cutting into a single characters)

##### code
```cpp
    int minCut(string s) {
        int n=s.size();
        vector<int> dp(n+1);
        for(int i=0;i<=n;i++) dp[i]=i-1;
        int j=0;
        for(int i=0;i<n;i++)
        {
            //odd expanding
            j=0;while(i-j>=0 && i+j<n && s[i+j]==s[i-j]) dp[i+j+1]=min(dp[i+j+1],dp[i-j]+1),j++;
            //even expanding
            j=0;while(i-j+1>=0 && i+j<n  && s[i+j]==s[i-j+1]) dp[i+j+1]=min(dp[i+j+1],dp[i-j+1]+1),j++;
        }
        return dp[n];
    }
```

##### comments
- dp[0]=-1 is necessary since dp[1] has to be 0 for one char. 



### 140. Word Break II.md
##### problem Summary
Given a string, and a list of words in dictionary, return all possible combinations using words in dictionary

##### idea
since we need combine words by words, using recursive + memoization is simpler. 
We use the front as the sub problem and the ending as the solved, which makes combination easier.
It uses the last word (may have different) recursively added.
for example "catsanddog" dict=["cat","cats","and","sand","dog"]
dog + sub problem catsand
and + sub problem cats -> cats and dog
sand + sub problem cat -> cat sand dog


```cpp
    unordered_map<string, vector<string>> m;

    vector<string> combine(string word, vector<string> prev)
    {
        for(int i=0;i<prev.size();++i)
            prev[i]+=" "+word;
        return prev;
    }

    vector<string> wordBreak(string s, vector<string>& wordDict) 
    {
        unordered_set<string> dict(wordDict.begin(),wordDict.end());
        return wordBreak(s,dict);
    }    

    vector<string> wordBreak(string s, unordered_set<string>& dict) 
    {
        if(m.count(s)) return m[s]; //take from memory
        vector<string> result;
        if(dict.count(s)){result.push_back(s);} //a whole string is a word
        for(int i=s.size()-1;i>=0;i--)
        {
            string word=s.substr(i);
            if(dict.count(word))
            {
                string rem=s.substr(0,i);
                vector<string> prev=combine(word,wordBreak(rem,dict));
                result.insert(result.end(),prev.begin(), prev.end());
            }
        }
        m[s]=result; //memorize
        return result;
    }
```    

##### comments
1. the code is hard to understand. The prev is the solution for the front subproblem which returns a list of words (already combined), so we just append the later word into each of it. It uses the sub-problem: the string return a list of combined words (doing the cut and combination the same time)
2. for the above example:
dog + subproblem(catsand)
subproblem catsand returns a list:
"cats and"
"cat sand"
and then we append the dog which get the final answer.
3. when the subproblem cannot produce a combination, the result is empty and combine will also produce empty (which is a trick here)

### 174. Dungeon Game.md
##### problem Summary
positive: add power
negative: reduce power
power need >=1 to be alive

#### Approach
reverse traverse

##### code
```cpp
    int calculateMinimumHP(vector<vector<int>>& dungeon) {
        //this shall be done in reverse order from bottom-right to top-left
        int m=dungeon.size(),n=dungeon[0].size();
        vector<vector<int>> dp(m+1,vector<int>(n+1,INT_MAX));
        //add a right, bottom cell as 1 so that we can apply +1 to the min
        dp[m][n-1]=dp[m-1][n]=1;
        for(int i=m-1;i>=0;i--)
        {
            for(int j=n-1;j>=0;j--)
            {
                int t=min(dp[i+1][j],dp[i][j+1])-dungeon[i][j];
                dp[i][j]=t<=0?1:t; //we need at least add 1 to it
            }
        }
        return dp[0][0];
        
    }
```

##### comment
- need initialize to be int-max
- initial to be 1 so that they can enter the two cells

### 312. Burst Balloons.md
##### problem Summary
Given a list of balloons with number on it. If a balloon is burst, point num[l] * num * num[r] will be added. (l,r is the adjacent left and right balloon)
Ask: to return max point we can get

##### idea
1. add a guardian to avoid boundary. add 1 to left and 1 to right
2. every time we burst a balloon, it depends on the left and right index (we do not want to alter the array and it will be a big mess). It is naturally to use left and right in the dp solutions
3. Once we want to burst balloon i, its points will be nums[i]*nums[l]*nums[r]. And it leaves two parts l to i-1 and i+1 to r. (Between l and r there are multiple elements, we are just assuming after some bursting, l and r becomes adjacent). This is similar to a reverse process. When we solve l, i, r, the previous problem (l, i-1) and (i+1, r) have all be solved. In another word, those balloons are all bursted already.
4. so the recurrence dp[l, r]=max(num[i] * num[l] * num[r]+dp[l,i-1]+dp[i+1,l]), i from l to r

##### code
```cpp
    int maxCoins(vector<int>& nums) {
        nums.insert(nums.begin(),1);
        nums.push_back(1);
        int n=nums.size();
        vector<vector<int>> dp(n,vector<int>(n));
        return helper(nums,1,n-2,dp);
    }
    int helper(vector<int>& nums,int s,int e,vector<vector<int>>& dp)
    {
        if(s>e) return 0;
        if(dp[s][e]>0) return dp[s][e];
        for(int i=s;i<=e;i++)
        {
            dp[s][e]=max(dp[s][e],nums[i]*nums[s-1]*nums[e+1]+helper(nums,s,i-1,dp)+helper(nums,i+1,e,dp));
        }
        return dp[s][e];
    }
```

#### comments
1. start,end are all possible balloon to burst, its left and right are its adjacent. So it is num[i]*num[s-1]*num[e+1]
2. terminate condition then is s>e (s==e is allowed for a single element left)
3. dp[s,e] is exactly a subproblem from start to end (a continuous partial array)
4. These details reflect the correct understanding of the method and shall pay special attention.



### 321. Create Maximum Number.md
#### problem summary
from two arrays with numbers 0-9, create k digits which is the max number.

#### idea:

try all combinations: i+j=k when get i digits from num1, and j digits from num2 and then merge the two.

Get k digits from one array: leave the k-1 digits alone, and find the max digit from 0 to n-k. And then search for the 2nd max after the previous max position.

When merge two arrays, one point need attention: when two elements are the same, we need compare lexicographically the arrays behind them.

For example, 2,1.. and 2,3... If we choose 2 from the 2nd array, then next would 2,1... merge with 3.... and 3 will be chosen and we get 2,3. On the contrary, if we choose 2 from the first array, we then need merge 1... and 2,3..., then we get 2,2 which is not correct.

#### code
```cpp
    vector<int> maxNumber(vector<int>& nums1, vector<int>& nums2, int k) {
        int m=nums1.size(),n=nums2.size();
        //cout<<m<<" "<<n<<" "<<k<<endl;
        if(k>=m+n)  //just merge the two arrays
            return merge(nums1,nums2);
        //try all possible combinations, i from array 1, k-i from array 2
        //i range is [max(m-k,0),min(k,m)], other array range is [max(n-k,0),min(k,n)]
        vector<int> ans(k);
        for(int i=0;i<=min(m,k);i++)
        {
            if(k-i>nums2.size()) continue;
            vector<int> a=maxNumber(nums1,i);
            vector<int> b=maxNumber(nums2,k-i);
            vector<int> c=merge(a,b);
            if(lexicographical_compare(ans.begin(),ans.end(),c.begin(),c.end())) ans=c;
        }
        return ans;
    }
    
    vector<int> maxNumber(vector<int>& v,int k) //get k digits from 1 array
    {
        int n=v.size();
        if(k>=n) return v;
        if(k==0) return vector<int>();
        vector<int> ans(k);
        //greedy choice, the leftmost digit is the max from i+[0,n-(k-1))
        int i=0;
        for(int j=1;j<=k;j++) //repeat k times
        {
            auto it=max_element(v.begin()+i,v.begin()+n-(k-j));
            i=int(it-v.begin())+1; //now the new pointer
            ans[j-1]=*it;
        }
        return ans;
    }
    vector<int> merge(vector<int>& v1,vector<int>& v2)
    {
        int i=0,j=0,k=0;
        vector<int> ans(v1.size()+v2.size());
        while(i<v1.size() && j<v2.size())
        {
            if(v1[i]>v2[j]) {ans[k++]=v1[i++];}
            else if(v1[i]<v2[j]) {ans[k++]=v2[j++];}
            else //two number is equal, choose the one behind is larger
            {
                if(lexicographical_compare(v1.begin()+i,v1.end(),v2.begin()+j,v2.end())) //v1<v2
                {ans[k++]=v2[j++];}
                else {ans[k++]=v1[i++];}
            }
        }
        if(i<v1.size()) copy(v1.begin()+i,v1.end(),ans.begin()+k);
        if(j<v2.size()) copy(v2.begin()+j,v2.end(),ans.begin()+k);
        return ans;
    }
```

#### comments
- this is a typical greedy choice problem. The key is if we want k digits from array, we leave space for the unsolved k-1 digits and greedy choose the max.
- lexicographical_compare is useful for string and array (do not need same length)
- merge two array

### 321. Create Maximum Number.md
#### problem summary
from two arrays with numbers 0-9, create k digits which is the max number.

#### idea:

try all combinations: i+j=k when get i digits from num1, and j digits from num2 and then merge the two.

Get k digits from one array: leave the k-1 digits alone, and find the max digit from 0 to n-k. And then search for the 2nd max after the previous max position.

When merge two arrays, one point need attention: when two elements are the same, we need compare lexicographically the arrays behind them.

For example, 2,1.. and 2,3... If we choose 2 from the 2nd array, then next would 2,1... merge with 3.... and 3 will be chosen and we get 2,3. On the contrary, if we choose 2 from the first array, we then need merge 1... and 2,3..., then we get 2,2 which is not correct.

#### code
```cpp
    vector<int> maxNumber(vector<int>& nums1, vector<int>& nums2, int k) {
        int m=nums1.size(),n=nums2.size();
        //cout<<m<<" "<<n<<" "<<k<<endl;
        if(k>=m+n)  //just merge the two arrays
            return merge(nums1,nums2);
        //try all possible combinations, i from array 1, k-i from array 2
        //i range is [max(m-k,0),min(k,m)], other array range is [max(n-k,0),min(k,n)]
        vector<int> ans(k);
        for(int i=0;i<=min(m,k);i++)
        {
            if(k-i>nums2.size()) continue;
            vector<int> a=maxNumber(nums1,i);
            vector<int> b=maxNumber(nums2,k-i);
            vector<int> c=merge(a,b);
            if(lexicographical_compare(ans.begin(),ans.end(),c.begin(),c.end())) ans=c;
        }
        return ans;
    }
    
    vector<int> maxNumber(vector<int>& v,int k) //get k digits from 1 array
    {
        int n=v.size();
        if(k>=n) return v;
        if(k==0) return vector<int>();
        vector<int> ans(k);
        //greedy choice, the leftmost digit is the max from i+[0,n-(k-1))
        int i=0;
        for(int j=1;j<=k;j++) //repeat k times
        {
            auto it=max_element(v.begin()+i,v.begin()+n-(k-j));
            i=int(it-v.begin())+1; //now the new pointer
            ans[j-1]=*it;
        }
        return ans;
    }
    vector<int> merge(vector<int>& v1,vector<int>& v2)
    {
        int i=0,j=0,k=0;
        vector<int> ans(v1.size()+v2.size());
        while(i<v1.size() && j<v2.size())
        {
            if(v1[i]>v2[j]) {ans[k++]=v1[i++];}
            else if(v1[i]<v2[j]) {ans[k++]=v2[j++];}
            else //two number is equal, choose the one behind is larger
            {
                if(lexicographical_compare(v1.begin()+i,v1.end(),v2.begin()+j,v2.end())) //v1<v2
                {ans[k++]=v2[j++];}
                else {ans[k++]=v1[i++];}
            }
        }
        if(i<v1.size()) copy(v1.begin()+i,v1.end(),ans.begin()+k);
        if(j<v2.size()) copy(v2.begin()+j,v2.end(),ans.begin()+k);
        return ans;
    }
```

#### comments
- this is a typical greedy choice problem. The key is if we want k digits from array, we leave space for the unsolved k-1 digits and greedy choose the max.
- lexicographical_compare is useful for string and array (do not need same length)
- merge two array

### 354. Russian Doll Envelopes.md
#### problem Summary
Given a list of envelopes with width and height. What is the max number of envelops we can russian doll (one put inside another if width and height are both smaller. Rotation is not allowed.

Input: [[5,4],[6,4],[6,7],[2,3]]
Output: 3 

Explanation: The maximum number of envelopes you can Russian doll is 3 ([2,3] => [5,4] => [6,7]).

#### analysis
It is natural to compare each evelope with all others. We can sort the envelpes so that larger one only need to compare with previous smaller ones. (save by half)

2d sort: first sort by width, if width is the same, sort by height.

dp[i] represents the largest russian doll ending at i, dp[i]=max(dp[i],dp[j]+1) if j can fit into i

The answer is max(dp[i]).

boundary condition:
each envelope itself counts 1: dp[i]=1;

This problem seems not that hard.

#### Implementation
```cpp
bool cmp(pair<int,int> a,pair<int,int> b) {return a.first<b.first || (a.first==b.first && a.second<b.second);}
class Solution {
public:
    bool canfit(pair<int,int> a,pair<int,int> b) {return a.first<b.first && a.second<b.second;}
    int maxEnvelopes(vector<pair<int, int>>& envelopes) {
        int n=envelopes.size();
        if(n==0) return 0;
        sort(envelopes.begin(),envelopes.end(),cmp);
        vector<int> dp(n,1); //dp[i] largest number ending at i (i is the outer envelope)
        for(int i=1;i<n;i++)
        {
            for(int j=i-1;j>=0;j--)
            {
                if(canfit(envelopes[j],envelopes[i])) dp[i]=max(dp[i],dp[j]+1);
            }
        }
        return *max_element(dp.begin(),dp.end());
    }
};
```

#### comments:
- boundary is critical for the correctness.




### 403. Frog Jump.md
#### problem summary
A list of stone at different positions in ascending order and first stone is always 0, and first jump is always 1.
frog can only jump on the stones
if previous jump is k, next jump can only be k+1, k, or k-1 steps.
Ask: check if the frog can reach the end.

#### idea
seems like a dfs problem. At every stone, we have 3 options, k, k+1, k-1. If none can reach next stone, we are done here and return 0.
suppose we take k step to reach stone pos[i]:
next is subproblem 

canCross(stones, i, k+1), 
canCross(stones, i, k), 
canCross(stones,i, k-1)

#### code
```cpp
    unordered_map<int,bool> mp;
    bool canCross(vector<int>& stones) {
        //dfs
        return helper(stones,0,1);// || helper(stones,1,2);
    }
    bool helper(vector<int>& stones,int ind,int nextstep) //step is previous step used
    {
        if(nextstep<=0) return 0;
        int key=ind | (nextstep<<11);
        if(mp.count(key)) return mp[key];
        if(stones[ind]+nextstep==stones.back()) return 1;
        int next=stones[ind]+nextstep;
        int it=lower_bound(stones.begin()+ind,stones.end(),next)-stones.begin();
        
        if(it<stones.size() && stones[it]==next) 
            return mp[key]=helper(stones,it,nextstep+1) || helper(stones,it,nextstep) || helper(stones,it,nextstep-1);
        return 0;
    }
```

#### comment
- dfs has to be with memoization since dfs is O(2^n)
- use nextstep is easier.

### 410. Split Array Largest Sum.md
#### problem summary
Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.

Note:
If n is the length of array, assume the following constraints are satisfied:

1 ≤ n ≤ 1000
1 ≤ m ≤ min(50, n)

#### analysis
1. the min sum is the max element in the array, the largest sum is the total sum of the array
2. accumulate ths sum, and array is sorted, segment sum is easily calculated using a[i]-a[j]
3. use binary search until we reach m segments, the idea is:
- use the mid=(lbound+ubound)/2
- if more than m part is needed, mid value is too small, lbound=mid+1
- if less than m part is obtained, mid value is too large, ubound=mid-1
- find how many part is easier given the max sum target. Just iterate and cut. We can consider this is greedy.

#### Implementation
```cpp
    int splitArray(vector<int>& nums, int m) {
        //the sum is between the max and the sum of all elements
        //use the mid value to split and use the greedy algorithm to form split arrays
        //if more than m can be obtained, the mid value is smaller, 
        //if less than m can be obtained, the mid value is larger
        
        int lbound=*max_element(nums.begin(),nums.end());
        int ubound=accumulate(nums.begin(),nums.end(),0);
        if(m<=1) return ubound;
        if(m>=nums.size()) return lbound;
        for(int i=1;i<nums.size();i++) nums[i]+=nums[i-1];//accumulate sum, and they are sorted
        int mid,maxsum;
        int numseg=0;
        while(lbound<=ubound)
        {
            mid=(lbound+ubound)/2;
            numseg=calcNumSeg(nums,mid,maxsum);
            if(numseg>m) {lbound=mid+1;}
            else {ubound=mid-1;}
        }
        return lbound;//maxsum;
    }
    int calcNumSeg(vector<int>& nums,int midval,int& maxsum)
    {
        int cnt=0,i=0,prevsum=0;
        maxsum=0;
        while(i<nums.size())
        {
            int ind=int(upper_bound(nums.begin()+i,nums.end(),midval+prevsum)-nums.begin());
            if(ind!=nums.size()) 
            {
                maxsum=max(maxsum,nums[ind-1]-prevsum);
                i=ind;
                prevsum=nums[ind-1];
            }
            else 
            {
                maxsum=max(maxsum,nums[ind-1]-prevsum);
                i=ind;
            }
            cnt++;
        }
        return cnt;
    }
```

### 44. Wildcard Matching.md
#### problem summary
given input string s and match pattern p, ? matches any single char, * matches 0 or more chars.
Check if p matches s.

#### idea
1. two string problem is a 2d dp problem. assuming dp[i, j] represents if s(0..i-1) matches p(0...j-1)
2. when add p[j], it could be a letter, a ? or a *
  - a letter: dp[i, j]=dp[i-1, j-1]&& s[i]==p[j]
  - a ?, always match dp[i, j]=dp[i-1, j-1]
  - a *, 
    - matches 0 char, dp[i, j]=dp[i-1, j]
    - matches 1 or more chars, dp[i, j]=dp[i,j-1] (s can advance one, but p cannot)
    - so when p[j-1]=='*' dp[i,j]=dp[i-1,j]||dp[i, j-1]
3. boundary condition
  -. dp[0, 0]: empty vs empty always true
  -. p empty, s non-empty, no match
  -. s empty, p non-empty, then p can only contains *
    
#### code
```cpp
    bool isMatch(string s, string p) {
        int m=s.size(),n=p.size();
        vector<vector<bool>> dp(m+1,vector<bool>(n+1));
        dp[0][0]=1;
        for(int i=1;i<=n;i++) dp[0][i]=dp[0][i-1] && p[i-1]=='*';
        for(int i=1;i<=m;i++)
        {
            for(int j=1;j<=n;j++)
            {
                if(p[j-1]!='*') dp[i][j]=dp[i-1][j-1] && (s[i-1]==p[j-1] || p[j-1]=='?');
                else dp[i][j]=dp[i-1][j]||dp[i][j-1];
            }
        }
        return dp[m][n];
    }
```

#### comments
  - complexity O(m*n)
  - one subtle point: when * matches 0, we need advance p, when * matches 1 or more char, s need advance, p cannot (leaving it for latter match).
  - direct dp for two string problem. 
  - similar problem regular expression matching
  
### 446. Arithmetic Slices II - Subsequence.md
#### problem summary
arithmetic subsequence: len>=3 and differene is the same
ask: number of arithmetic subsequence

#### idea
Suppose we are adding A[i] to A[0]...A[i-1]. It could form arithmetic subsequence ending with A[i]
1,3,5: 1 sub
1,3,5,7: 2 subs
1,3,5,7,9:
1,3,5: ending with 5, difference is 2
3,5,7
1,3,5,7: ending with 7, difference is 2
5,7,9
3,5,7,9
1,3,5,7,9: ending with 9, difference is 2
1,5,9: ending with 9, difference is 4

so adding A[i]:
1. it form a new arithmetic subsequence with difference d, dp[i][d]=1, 
2. it extends an existent array with difference d, dp[i][d]=sum(dp[j][d]+1), for all j<i when j has d
  - Why? since we may have duplicate numbers and they count.
  - what about starting from two elements? 
  for example 1,2,3, for 2, d=1. dp[2][1]=1, 
  for 3, we get dp[3][1]=1+1, dp[3][2]=1+0. But we only have 1 subsequence. (so when adding to results, we can just add the dp[j][d].
3. the final answer is the sum of all endings
4. since d can be negative range from INT_MIN to INT_MAX, using a hashmap is more suitable

boundary condition
all zero

#### code
```cpp
    int numberOfArithmeticSlices(vector<int>& A) {
        if(A.empty()) return 0;
        vector<unordered_map<int,int>> dp(A.size());
        int res=0;
        for(int i=1;i<A.size();i++)
        {
            for(int j=i-1;j>=0;j--)
            {
                long long d=(long long)A[i]-A[j];
                if(d<=INT_MIN || d>=INT_MAX) continue;
                dp[i][d]++;
                if(dp[j].count(d)) 
                {
                    dp[i][d]+=dp[j][d];
                    res+=dp[j][d];//reason we add dp[j][d]: we need to get rid of those two-element slices
                }
            }
            
        }
        return res;
    }
```

#### comment
1. using long long for difference is necessary to avoid overflow
2. we add dp[j][d] to make sure we are only counting len>=3 subsequences. (1 element dp is 0, 2 element dp is 1, 3 element dp is 2)

### 466. Count The Repetitions.md
#### problem summary
define S=[s,n] repeat s n times and get S
given s1 and s2, S1=[s1,n1], S2=[s2,n2]
find the max M so that [S2,M] is a subsequence of S1
n1 is [0 1e6] and n2 is [1 1e6]
s1 and s2 max length is 100

#### analysis
the repeated string is up to 10^8. Even O(N) algorithm may get TLE.
if character in s2 is not in s1, then answer is 0

Not a typical dp problem but more likely a greedy problem.

Brutal force: two pointers 
find the repeating pattern of s2 in S1.
What is a repeating cycle? 
The first or last character position of s2 matching position in S1 (repeating segment and its inside index)
When an identical inside index is found, we know the cycle length is the segment!

When we reach the ending char of s2, we reached one segment, record the position in S1 and number of segment of s2 in a hashmap with the key i%s1.length().
When a repeat pattern is found, i%s1.length appeared in the hashmap, then we can safely skip all the repeating pattern until to the remaining part.

```cpp
    int getMaxRepetitions(string s1, int n1, string s2, int n2) {
        //greedy choice to find the repeat pattern
        int i=0,j=0; //i is for s1 and j is for s2
        int nseg2=0,nseg1=0;
        int m=s1.size(),n=s2.size();
        if(m*n1<n*n2) return 0;
        if(!contains(s1,s2)) return 0;
        unordered_map<int,vector<int>> start_indx; //key is starting inside a seg
        //value0: the repeat number of s2, value 1: the index in S1
        while(i<n1*m)
        {
            while(s1[i%m]!=s2[j%n]) i++;
            if(j%n==n-1) //first char
            {
                nseg2++;
                if(start_indx.count(i%m)) //have found repeat pattern
                {
                    int T=i-start_indx[i%m][0];//the repeat period
                    int R=nseg2-start_indx[i%m][1]; //the repeat s2 segment
                    int nremain=(n1*m-i-1)/T;
                    
                    i+=nremain*T;
                    j+=nremain*R*n;
                    //cout<<T<<" "<<R<<" "<<nremain<<": "<<i<<" "<<j;
                    nseg2+=nremain*R;                    
                }
                else start_indx[i%m]=vector<int>({i,nseg2});
            }
            i++,j++;
        }
        return nseg2/n2;
    }
    bool contains(string s1,string s2)
    {
        unordered_set<char> cs1(s1.begin(),s1.end());
        unordered_set<char> cs2(s2.begin(),s2.end());
        for(auto it=cs2.begin();it!=cs2.end();it++)
            if(cs1.count(*it)==0) return 0;
        return 1;
    }
```

#### comments
- the beginning and ending part shall be treated iteratively
- the mid part is the repeating part and can be skipped
- some apparent non-match shall be pruned first, including length problem and s2 is not a subset of s1.
- I tried to find an equation but failed many times, and has to switch to above solution

### 472. Concatenated Words.md
#### problem Summary
Given a list of words, return all the words in the list which can be combined by other words in the list

#### analysis
1. Apparently we shall first sort the words according to its length, so we only need to check its front words.

2. if a word is found in it, then it is split into left and right sub-problem.

3. so recursion + memoization could be a solution. Memoization shall record what string is already processed.

4. using a hashset is good for searching.

#### Implementation
without memoization
```cpp
    vector<string> findAllConcatenatedWordsInADict(vector<string>& words) {
        sort(words.begin(),words.end(),cmp);
        unordered_set<string> dict;
        vector<string> ans;
        unordered_map<string,bool> dp;
        for(int i=0;i<words.size();i++)
        {
            if(canCombine(words[i],dict,dp)) ans.push_back(words[i]);
            dict.insert(words[i]);
        }
        return ans;
    }
    //check if a word can be combined using words from a dictionary
    bool canCombine(string w,unordered_set<string>& dict,unordered_map<string,bool>& dp)
    {
        if(dict.empty()) return 0;
        //if(dp.count(w)) return dp[w];
        if(dict.count(w)) return dp[w]=1;
        
        for(int i=1;i<w.size();i++)
        {
            if(dict.count(w.substr(0,i)))
            {
                if(canCombine(w.substr(i),dict,dp))
                {
                    return dp[w]=1;
                }
            }
        }
        return dp[w]=0;
    }
```
Note using the recorded result is disabled. Some bugs are with the memoization.

Partial DP solution: dp approach is just used for a single word.
The idea is similar to the word break, dp[i] represents if i is good cut position
```cpp
    bool canCombine(string& word,unordered_set<string>& dict)
    {
        if(dict.empty()) return 0;
        vector<bool> dp(word.length()+1);//dp[i]: [0...i-1] substr can be combined
        dp[0]=1;//always can form by an empty string
        for(int i=1;i<=word.length();i++)
        {
            for(int j=0;j<i;j++)
            {
                if(!dp[j]) continue;//previous one is not a word
                string t=word.substr(j,i-j);//note substr 2nd is the length
                if(dict.count(t)) {dp[i]=1;break;} //cannot search the whole set!
            }
        }
        return dp[word.length()];
    }
```

The two solutions runs almost the same time.

#### comments


### 514. Freedom Trail.md
#### problem Summary
On a ring, there is a string. Initially the 0th character is on 12:00 direction. Given a keyword, we need in turn to rotate the letter to 12:00 direction and select it.
Ask: the minimum steps (rotation + selection)

#### idea
1. selection can be ignored since each letter requires exactly one selection.
2. support clockwise and anti-clockwise rotation. Greedy choice will not work to choose the min distance of clockwise and counter-clockwise since it may affect the following rotations
3. correct way is to choose all possible positions and solving the remaining subproblem using its position as the reference point. This is a dfs approach.
4. For convenience we maintain a hashmap with the key as the char and list of positions as its value.

#### code
```cpp
    int findRotateSteps(string ring, string key) {
        //dfs
        int n=ring.size(),m=key.size();
        vector<vector<int>> mp(26);
        for(int i=0;i<ring.size();i++) mp[ring[i]-'a'].push_back(i);
        vector<vector<int>> dp(n,vector<int>(m));
        int minSteps=minsteps(mp,key,0,vector<int>(1,0),n,dp);
        return minSteps+key.length();
    }
    
    int minsteps(vector<vector<int>>& mp,string& key,int start,vector<int> prev,int n,vector<vector<int>>& dp)
    {
        if(start==key.length()) {return 0;}
        if(dp[prev.back()][start]) return dp[prev.back()][start];
        vector<int>& v=mp[key[start]-'a'];
        int sum=INT_MAX;
        for(int i=0;i<v.size();i++)
        {
            int d=abs(v[i]-prev.back());
            d=min(d,n-d);
            prev.push_back(v[i]);
            sum=min(sum,d+minsteps(mp,key,start+1,prev,n,dp));
            prev.pop_back();
        }
        dp[prev.back()][start]=sum;
        return sum;
    }
```

### DP approach:
  dp approach shall use reverse thinking, using the key from right to left since the first status (0th char at 0th position) is known.
  dp[i, j]: represents the min number of moves for subproblem: ring position at i and spell the key 0...j-1. The final answer is dp[0,0].
  When we solved the subproblem i to n-1, and leaving the ring position at i.
  dp[i, j]=min(dp[v[k]][j+1]+d) d is the distance from v[k] to i, and v[k] is the position for key[j].
  
  
#### comments
- previous position is the list of previous position, initialized as 0, since 0th char is at 0 move position
- start is the key position





### 517. Super Washing Machines.md
#### problem Summary
a list of washing machine with some clothes inside, each move choose any number of machine and passing one clothes to machines on its left or right side.

Ask: the min number of move to make each machine the same load

#### idea
target is the average load. We can elmininate those illegal cases. and then subtract the average. The new target will be 0.
A machine with positive value can only pass out clothes.
1. from left to right, we shall give all its load to right to make it 0 (does not matter it is negative or positive). number of move is abs(value). That is like accumulate.

2. the least steps we need to eventually finish this process is determined by the peak of abs(cnt) and the max of "gain/lose" array
refer to:
https://leetcode.com/problems/super-washing-machines/discuss/99185/Super-Short-and-Easy-Java-O(n)-Solution


#### code
```cpp
    int findMinMoves(vector<int>& machines) {
        int n=machines.size();
        int avg=accumulate(machines.begin(),machines.end(),0);
        if(avg%n) return -1;
        avg/=n;
        for(int i=0;i<n;i++) machines[i]-=avg;
        int max0=0,cnt=0;
        for(int i=0;i<n;i++)
        {
            cnt+=machines[i];
            max0=max(max0,max(abs(cnt),machines[i]));
        }
        return max0;
    }
```

#### comments
1. the machine with largest load shall give away all of it. That is one bound
2. at ith position, the prefix sum is another bound: which shall pass out or pass in to it. That is another bound.
3. be sure to include max0 self in the max function, otherwise it will only use current (local) max. 

### 546. Remove Boxes.md
#### problem Summary
Given a list of numbers (different colors), every time you can choose to remove continuous same number (box with same color). If you remove k boxes, you get point k^2.
Return the max point you can get.

#### idea
for example [1, 3, 2, 2, 2, 3, 4, 3, 1]
You can remove 2 first, and let the 3 connected together, 9 points
1,3,3,4,3,1, remove the 4, get 1
1,3,3,3,1 remove the 3, get 9
1,1 remove 1, get 4 total: 23

assuming dp[i,j] is the max points you can get from i to j (inclusive). And the final answer would be dp[0, n-1].

we get the group a[i] to a[i+k], assuming the k+1 elements are the same color, then we have two choices:

    group them and get the score and delete them
    leave them for a while and process other first and hope to have a longer sequence and higher scores.

For the first case, the score is (k+1)^2+subproblem(i+k+1,j)
for the second case, assuming we find mth element==nums[i] and want to combine with a[i, i+k], then we need solve two subproblem first [i+k, m-1] with no previous same char and [m, j] with previous k+1 same char.

Thus the dp needs to add another dimension k, dp[i][j][k] defines the max score we get from the sequence i to j (inclusive) with number of same color box ahead.

dp[i][j][k]=max((k+1)^2+sub(i+k+1,j,0), sub(i+k+1,m-1,0), sub(m, j, k+1))

#### code
```cpp
    int removeBoxes(vector<int> boxes) {
        int n = boxes.size();
        vector<vector<vector<int>>> dp(n,vector<vector<int>>(n,vector<int>(n)));
        return removeBoxesSub(boxes, 0, n - 1, 0, dp);
    }

    int removeBoxesSub(vector<int>& boxes, int i, int j, int k, vector<vector<vector<int>>>& dp) 
    {
        if (i > j) return 0;
        if (dp[i][j][k] > 0) return dp[i][j][k];
        for (; i + 1 <= j && boxes[i + 1] == boxes[i]; i++, k++); 
        // optimization: all boxes of the same color counted continuously from the first box should be grouped together
        int res = (k + 1) * (k + 1) + removeBoxesSub(boxes, i + 1, j, 0, dp);
        for (int m = i + 1; m <= j; m++) {
            if (boxes[i] == boxes[m]) {
                res = max(res, removeBoxesSub(boxes, i + 1, m - 1, 0, dp) + removeBoxesSub(boxes, m, j, k + 1, dp));
            }
        }
        dp[i][j][k] = res;
        return res;
    }
```

#### comment
- recursive + memoization is more straightforward.
- similar problem: 664 strange printer

### 552. Student Attendance Record II.md
#### problem summary
Given string length n, A: Absent, P: Present, L: Late. A rewardable record is: no more than one A, or more than two continuous L
Return the number of records

#### idea
This is a typical DP problem.
Best approach here: https://leetcode.com/problems/student-attendance-record-ii/discuss/101634/Python-DP-with-explanation

1. there is no A
ending with:
P: dp[i]=dp[i-1]
PL: dp[i]=dp[i-2]
PLL: dp[i]=dp[i-3]

2. With A.
A can be in any position from 0 to j. When A is at j, it divides the string into two strings with no A case:

left from 0 to i-1: dp(i)

right from i+1 to n-1: dp(n-1-i)

sum(dp(left)*dp(right)) j=0 to i

dp[i]: represents the number of strings without A

boundary: dp[0]=1, 
dp[1]=2, P, L
dp[2]=4: PP, PL,LP,LL

#### code
```cpp
    int checkRecord(int n) {
        if(n==0) return 1;
        if(n==1) return 3;
        vector<int> dp(n+1);//dp: number of strings without A
        dp[0]=1;
        dp[1]=2;
        dp[2]=4;
        int mod=1e9+7;
        for(int i=3;i<=n;i++) dp[i]=((long long)dp[i-1]+dp[i-2]+dp[i-3])%mod;
        int result=dp[n];
        
        for(int i=0;i<=n;i++)
        {    
            result+=((long long)dp[i]*dp[n-1-i])%mod;
            result%=mod;
        }
        return result;
    }
```

#### comments
-. dp[i] is the number of strings with length i, without A

-. need use long long for intermediate results to avoid overflow

### 600. Non-negative Integers without Consecutive Ones.md
#### problem Summary
Given n, from 0 to n, return the number of integers whose binary contains no consecuative 1s

#### idea
1. convert n to binary, it will have m bits
2. can we solve problem when n=11111..1 with m bits case? 
Assuming dp[i] is the number of valid string with length i.
  - if previous bit is 1, then we can only add 0 dp[i][0]=dp[i-1][1]
  - if previous bit is 0, then we can add 1 or 0 dp[i][0]=dp[i-1][0], dp[i][1]=dp[i-1][1]
  - above is awkward, if we think in another way, 
  if we define a[i] as the number of valid strings ending with 0, b[i] is the string ending with 1
  we can add 0 no matter previous: 
  a[i]=a[i-1]+b[i-1], 
  we can add 1 only when previous is 0:
  b[i]=a[i-1]
3. subtract all over counted integers when number>n
-. when binary of N appears 11, we just break, since all next smaller
-. when binary of N appears 00, over count those ending with 1

#### code
```cpp
    int findIntegers(int num) {
        string s;
        while(num) {s+=num%2+'0';num/=2;}
        reverse(s.begin(),s.end());//MSB is at 0
        int m=s.length();
        vector<int> dp0(m),dp1(m);
        dp0[0]=1,dp1[0]=1;
        for(int i=1;i<m;i++)
        {
            dp0[i]=dp0[i-1]+dp1[i-1];
            dp1[i]=dp0[i-1];
        }
        int result=dp0[m-1]+dp1[m-1];
        for(int i=1;i<m;i++)
        {
            if(s[i]=='1' && s[i-1]=='1') break;
            if(s[i]=='0' && s[i-1]=='0') result-=dp1[m-i-1];
        }
        return result;
    }
```

#### comments
1. the string of n is in human preference with MSB at the first element in string
2. when previous of n is 00, we need minus dp1[m-i-1] (those > this number is ith bit is 1)
3. if we use LSB at 0, it is easier to understand

  
### 629. K Inverse Pairs Array.md
#### problem summary
given 1 to n, get the number of combinations that having exactly k inverse pairs

#### ideas
assuming we solved problems for [1, i-1], adding i to exisiting condition:
i can be anywhere, for example adding 5 to [1,4]
5xxxx: adding 4 inversion pairs (i-1)
x5xxx: adding 3 (i-2)
xx5xx: adding 2 (i-3)
xxx5x: adding 1 (i-4)
xxxx5: adding 0 (i-5)
we have 5 ways to reach to a specific number of inverse.

dp[i,k] represents the number of permutation with k inverse pairs
according to above observations
if we put n as the last number then all the k inverse pair should come from the first n-1 numbers
if we put n as the second last number then there's 1 inverse pair involves n so the rest k-1 comes from the first n-1 numbers
...
if we put n as the first number then there's n-1 inverse pairs involve n so the rest k-(n-1) comes from the first n-1 numbers

dp[i,k]=dp[i-1,k]+dp[i-1,k-1]+dp[i-1,k-2]+dp[i-1,k-3].....+dp[i-1,k-(i-1)]
using above equation:
dp[i,k-1]=dp[i-1,k-1]+dp[i-1,k-2]+....dp[i-1,k-i]
subtract the two:
dp[i,k]=dp[i-1,k]+dp[i,k-1]-dp[i-1,k-i]

boundary condition:
i=0, there is no inverse pair dp[0][k]=0;
k=0, only sorted array supports dp[i][0]=1
dp[i,0]=1

#### code
```cpp
    int kInversePairs(int n, int k) {
        if (k > n*(n-1)/2 || k < 0) return 0;
        if (k == 0 || k == n*(n-1)/2) return 1;        
        vector<vector<int>> dp(n+1,vector<int>(k+1));
        for(int i=0;i<=n;i++) dp[i][0]=1;
        int mod=1e9+7;
        for(int i=1;i<=n;i++)
        {
            for(int j=1;j<=k;j++) ###
            {
                dp[i][j]=(dp[i-1][j]+dp[i][j-1])%mod;
                if(j-i>=0) dp[i][j]-=dp[i-1][j-i];
                dp[i][j]=(dp[i][j]+mod)%mod;
            }
        }
        return dp[n][k];
    }
```

#### comment
- why dp[0,0]=1?


### 664. Strange Printer.md
#### problem summary
Printer each move prints a list of same characters. Next print will be on the previous print, covering those under.
Given a string, get the min number of moves

#### idea
1. the number of same char does not matter (different from previous problem on remove boxes which is depending on k)
2. when same char is separated, either we connect them or print differently. That is why it is the same as removing boxes.

We don't need the k dimension in this case, since number of char does not matter.
3. this is similar to a series of overlapped segments, greedy choice will not work.

#### code
```cpp
    int strangePrinter(string s) {
        if(s.length()<1) return 0;
        string ss;
        //reduce the string first to avoid time or space TLE
        char c=s[0];
        ss+=s[0];
        for(int i=1;i<s.length();i++) if(s[i]!=c) {c=s[i];ss+=s[i];}
        s=ss;
        int n=s.length();
        vector<vector<int>> dp(n,vector<int>(n));
        return helper(s,dp,0,n-1);
    }
    int helper(string& s,vector<vector<int>>& dp,int i,int j)
    {
        if(i>j) return 0;
        if(i==j) return 1;
        if(dp[i][j]) return dp[i][j];
        //k is the number of same char
        int res=1+helper(s,dp,i+1,j);//no char attached
        for(int m=i+1;m<=j;m++)
        {
            if(s[m]==s[i])
                res=min(res,helper(s,dp,i+1,m-1)+helper(s,dp,m,j));
        }
        dp[i][j]=res;
        return res;
    }
```

#### comments
- please refer to remove boxes, which is almost the same.

### 689. Maximum Sum of 3 Non-Overlapping Subarrays.md
#### problem Summary
Given a array of length n, and a number k, find 3 non-overlap region with k elements each, make the sum largest.
Ask: get the starting index of the 3 segment. If there is tie, choose the smaller index (lexigraphically smaller one)

#### idea
1. There are 3 segments, left, right and mid. left range limits <n-2*k, right range limits >2*k
2. using dp to update the k window sum for left from left to right (dp[i] is the k-window sum max for 0 to i-1)
3. using dp to update the k window sum for right from right to left (dp[i] is the k window sum max for i to n)
4. iterate on all mid position to get the max
5. find the leftmost index satisfying the max

#### Implementation
```cpp
    vector<int> maxSumOfThreeSubarrays(vector<int>& nums, int k) {
        //get the partial sum first
        int n=nums.size();
        vector<int> psum(n-k+1);
        psum[0]=accumulate(nums.begin(),nums.begin()+k,0);
        for(int i=1;i<n-k+1;i++) psum[i]=psum[i-1]+nums[i+k-1]-nums[i-1];
        
        vector<int> leftmax(n-k+1),rightmax(n-k+1);
        int tmax=INT_MIN;for(int i=0;i<n-k+1;i++) tmax=leftmax[i]=max(tmax,psum[i]);
        tmax=INT_MIN;for(int i=n-k;i>=0;i--) tmax=rightmax[i]=max(tmax,psum[i]);

        int globalmax=INT_MIN;
        int mid=0,left=0,right=0;
        for(int i=k;i<psum.size()-k;i++) //mid can from k to n-k
        {
            int l=i-k,r=i+k; 
            int localmax=leftmax[i-k]+psum[i]+rightmax[i+k];
            if(globalmax<localmax) //use < so equal max will not be counted in
            {
                globalmax=localmax;
                mid=i;left=i-k;right=i+k;
            }
        }
        
        for(int i=left-1;i>=0;i--) if(leftmax[i]==leftmax[left]) left--;else break;
        for(int i=right+1;i<n-k+1;i++) if(rightmax[i]==rightmax[right]) right++;else break;
        return vector<int>({left,mid,right});
    }
```

### 730. Count Different Palindromic Subsequences.md
#### problem Summary
Given a string of length n, find the number of different Palindrome subsequences, string has only a,b,c,d
Attention: it asks for **subsequences**, not substring

#### ideas
1. it is easy to extend to 26 chars

2. a palindrome string can be from i to j. It is naturally use a start, end pair, or a start, length pair to indicate a palindrome string.

3. dp natural thinking: we start from the (i,len) subproblem and extend to see if we can solve bigger problem
assuming we add a char to s[i, i+len-1] (we use xxxx to indicat the string which is palindrome):

4. if we define dp[i,len,x] as the number of different pal-subsequence starting at i, with length=len, with start/end char =x

if s[i]!='x', we can ignore (remove) first char, dp[i,len,x]=dp[i+1,len-1,x]
else if s[j]!='x', we can ignore (remove) last char, dp[i,len,x]=dp[i,len-1,x] (the head is x but tail is not)

if both are x: 
dp[i,len,x]=dp[i+1,len-2,'a']+dp[i+1,len-2,'b']+dp[i+1,len-2,'c']+dp[i+1,len-2,'d']+2

why?
  - we are adding one x to the head and one x to the tail, which makes xa..ax, xb..bx, xc..cx, xd..dx all different pal-subsequence. Since we are making the length increased by 2, and they are all different.
  
  - +2: we can add x and xx into it since we add two x into previous solution, and we at least have length>=3
  
for example: we have aabaa, the subsequence start and end with a:

a,aa,aaa,aaaa,aba,aabaa

when add a to head and tail, they become:

aaa,aaaa,aaaaaa,aabaa,aaabaaa

and we add a and aa into it.

5. since it only involves len-2, len-1 and len, we only need 3 matrices.

6. The final answer is the sum of start=0, len=n, and char=a, b, c, d

#### Implementation
```cpp
    int countPalindromicSubsequences(string S) {
       int n=S.length();
        int mod=1e9+7;
        //dp[i][len][c]: represents starting at i, with length=len start and ending with c
        vector<vector<int>> dp0(n,vector<int>(4)),dp1(n,vector<int>(4)),dp2(n,vector<int>(4));
        //dp0:len, dp1: len-1, dp2: len-2
        for(int len=1;len<=n;len++)
        {
            for(int i=0;i+len<=n;i++)
            {
                for(int j=0;j<4;j++)
                {
                    dp0[i][j]=0;
                    if(len==1) {dp0[i][j]=(S[i]=='a'+j);continue;}
                    if(S[i]!='a'+j) dp0[i][j]=dp1[i+1][j];//dp[i][len][c]=dp[i+1][len-1][c]
                    else if(S[i+len-1]!='a'+j) dp0[i][j]=dp1[i][j];//dp[i,len,c]=dp[i,len-1,c]
                    else //both ==x
                    {
                        dp0[i][j]=2;
                        if(len>2) for(int k=0;k<4;k++) {dp0[i][j]+=dp2[i+1][k];dp0[i][j]%=mod;} //dp[i+1,len-2,k]
                    }
                    dp0[i][j]%=mod;
                }
            }
            //len increase
            dp2=dp1;
            dp1=dp0;
        }
        //final answer is sum(dp[0,n,c])
        return accumulate(dp0[0].begin(),dp0[0].end(),0LL)%mod;
    }
```

#### comments
- it needs subsequence, not substring, this is very important to the understanding of the algorithm
- need special treat len=1 case
- need special treat len==2 case when add two char (empty)
- when an iteration on len is done, we need update len-1->len-2, len->len-1
- every time len shall be initialized since we reuse the matrix. that is why dp2[i][x]=0 is needed. Attention shall be paid to this.



### 741. cherry pickup.md
#### problem Summary
This is a pretty hard DP problem.

matrix: 0 empty, 1 cherry -1: thorn

You need go roundtrip from top left to bottom right and back to top left and get the max cherry.

### Approach:

- intuitively way that maximizes the first pass and changes the optimal path and then finds the second pass optimal path will not work. Since this will only maximize the first pass and the global optimal is not guaranteed.

- From top left to bottom right is equivalent to from bottom right to top left

- The correct approach is to try the two passes simultaneously and make the two passes optimal. The only constraint is: the two passes cannot pick up the same cherry twice.

- for a matrix n x n, one trip takes 2N-1 steps. We can try all possible locations for two passes for each step and this is the key point. i.e., the first pass goes to (i,j) and second pass goes to (p,q) and i+j=p+q=steps. The only constraint is (i, j)=(p, q). The cherry picked up at these two locations are grid[i, j]+grid[p, q].

- From previous position to current (i,j) and (p,q), the previous combination could be the following: (i-1, j, p-1, q, k-1) (i-1, j, p, q-1, k-1) (i, j-1, p-1, q, k-1) (i, j-1, p, q-1, k-1). k is the number of steps.

- So the recurrence relation is dp(i, j, p, q, k)=max(dp(i-1, j, p-1, q, k-1), dp(i-1, j, p, q-1, k-1), dp(i, j-1, p-1, q, k-1), dp(i, j-1, p, q-1, k-1))+grid(i, j)+grid(p,q).

- Since i and j are associated, also p and q are associated, dp shall not use i and j, but we need use i and p, or j and q. (the x coordinate for two positions or y coordinates for the two passes).

    - dp(i-1, j, p-1, q, k-1) reduced to dp(i-1, p-1, k-1)

    - dp(i-1, j, p, q-1, k-1) reduced to dp(i-1, p, k-1)

    - dp(i, j-1, p-1, q, k-1) reduced to dp(i, p-1, k-1)

    - dp(i, j-1, p, q-1, k-1) reduced to dp(i, p, k-1)
    
Since only k-1 iteration is involved, we may not need the 3rd dimension, but extra care is needed, generally reverse iteration is required to avoid using updated values.

And finally we reached the solution:

Attention:

Since we reduce the 3d problem into 2d problem we need do reverse iteration to use n-1 values

when grid[x][y]<0, we need set dp[x1][x2]=-1 it is necessary since it is dp[x1][x2][n]! and dp[x1][x2][n-1] may be >0 but dp[x1][x2][n] may be <0. Need to keep updating.

two legs can cross the same position, but can only pick the cherry once.

#### code
```cpp
    int cherryPickup(vector<vector<int>>& grid) {
        int n=grid.size();
        vector<vector<int>> dp(n,vector<int>(n,-1));
        dp[0][0]=grid[0][0];
        for(int nstep=1;nstep<2*n-1;nstep++)
        {
            for(int x1=n-1;x1>=0;x1--)
            {
                for(int x2=n-1;x2>=0;x2--) //can share the same position but cannot pick twice
                {
                    int y1=nstep-x1,y2=nstep-x2;
                    if(y1<0 || y2<0 ||y1>=n || y2>=n) continue;
                    if(grid[x1][y1]<0 || grid[x2][y2]<0) {dp[x1][x2]=-1;continue;}
                    int delta=grid[x1][y1];
                    if(x1!=x2) delta+=grid[x2][y2];
                    int best=-1;
                    if(x1 && x2 && dp[x1-1][x2-1]>=0) best=max(best,dp[x1-1][x2-1]+delta);
                    if(y1 && x2 && dp[x1][x2-1]>=0) best=max(best,dp[x1][x2-1]+delta);
                    if(x1 && y2 && dp[x1-1][x2]>=0) best=max(best,dp[x1-1][x2]+delta);
                    if(y1 && y2 && dp[x1][x2]>=0) best=max(best,dp[x1][x2]+delta); 
                    dp[x1][x2]=best; 
                }
            }
        }
        return dp[n-1][n-1]==-1?0:dp[n-1][n-1];
    }

```

### Attention:
- this is a 3d problem with space reduced to 2d, especial care needs attention. one is the reverse iteration, one is setting dp to be -1 when there is a thorn at either position
- cannot pick the same cherry
- initialize to -1 to mark. do not have to be int_min which makes things more complicated.
- complexity O(N^3)

### 787. Cheapest Flights Within K Stops.md
#### problem Summary

from src to dst, find the cheapest cost using at most k stops

#### idea

- since src is fixed, we only need calculate src to all other airport min cost and the answer is src to dst. Why we care about other dest, that is the nature of dp. We solve our problem from other solved problems.

- No need 3D space. Assuming dp[j, k] is the min cost from src to j using at most k stops.

- The key idea using dp approach is:

when we have k-1 stops, we add one more stop to check if the cost will be relaxed.

assuming the added airport is m, then dp[j, k]=min(dp[j, k], dp[m, k-1]+prices[m, j])

Boundary condition:

    1. src to src is 0
    2. src to other station directly is given in flights
    
#### code
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
 #### comments on the code
 - we build a matrix on the prices, which may have other way to just use the flights, but it is not important
 - boundary: src to src is 0, src to other is given
 - there are quite a few similar problems with at most k times, they are all approached with similar idea: compare add one time on previous k-1 or not add. dp[k-1], dp[k-1]+one transaction.
 - based on above conclusion, there is another equivalent coding:
 ```cpp
         for(int k=1;k<=K;k++)
        {
            //add one stop based on previous k-1 stop
            for(int j=0;j<n;j++)
            {
                if(src==j) continue;
                dp[j][k]=dp[j][k-1]; //not add one stop
                for(int m=0;m<n;m++)
                {
                    if(dp[m][k-1]!=INT_MAX && prices[m][j]!=INT_MAX)
                    dp[j][k]=min(dp[j][k-1],dp[m][k-1]+prices[m][j]);
                }
            }
        }
```        

### 818. Race Car.md
#### problem summary
Initial position 0 and speed +1, and given a target>0
Instruction A: position+=speed, speed*=2;
Instruction R: position no change, speed=+1 if negative, -1 if positive
Ask: minimum length of instructios

#### idea
Greedy choice 1: accelerate until passing target 1+2+4+8+...+2^n>=target and then reverse back. Remaining is a subproblem with reduced target distance.
Greedy choice 2: acceleate and stop before the target, 1+2+4+...+2^n<=target, and then reverse back and go for some steps and reverse back again. Remaining is a subproblem with reduced target distance

choose the min of the two

1+2+4+..+2^n=2^(n+1)-1, total n instructions
n+dp(2^(n+1)-target)+1 +1: represent the R

Reverse before target is more complicated. We need get away from the target for some steps and then reverse back.
n+dp(target-2^n+2^m)+1, +2 represent two time of reverse. The number of m shall <n (we shall make the distance smaller)
stop when it is 0

#### code
```cpp
    int dp[10001];
    int racecar(int t) {
        if (dp[t] > 0) return dp[t];
        int n = floor(log2(t)) + 1, res;
        if (1 << n == t + 1) dp[t] = n;
        else {
            dp[t] = racecar((1 << n) - 1 - t) + n + 1;
            for (int m = 0; m < n - 1; ++m)
                dp[t] = min(dp[t], racecar(t - (1 << (n - 1)) + (1 << m)) + n + m + 1);
        }
        return dp[t];
    }
```

#### comments
- 1 to 2^n only n accelerations
- recursive with memoization is needed to avoid repeative solving same problems.

### 847. Shortest Path Visiting All Nodes.md
#### problem summary
Given a graph with n nodes, the array gives the connected nodes to ith node. Graph is undirected. number of nodes <=12
Ask: get the min length to visit all nodes

#### analysis
1. We shall try all nodes as the starting node.
2. using bitset as the visited for a limited number of nodes
3. shortest path problem generally uses bfs. Here we shall solve n node bfs
4. The key idea: when we add a node's children through some paths (indicated by the status) makes the length shorter. Then we need relax the length to the child

BFS procedure for one starting node:
-. push the node in the queue, set the bit
-. iterate on its children, relax the distance (initialize to max)
-. push the children node into queue
-. until queue is empty

for n nodes the bfs is similar. Just push all starting nodes in the queue. The later starting nodes keeps on relaxing the distances.
May worth a try to test if run n separate bfs the same.

The final answer is starting nodes is min for all i and status is 0b1111111..1.

dp[i,status] represents starting node i, status: all nodes visited.

#### code
```cpp
    struct State{
        int mask,source;
        State(int m,int s):mask(m),source(s){}
    };
    int shortestPathLength(vector<vector<int>>& graph) {
        int m=graph.size();
        int len=1<<m;
        vector<vector<int>> dp(m,vector<int>(len,m*m));
        queue<State> qs;
        for(int i=0;i<m;i++) 
        {
            dp[i][1<<i]=0; //self to self distance is 0
            qs.push(State(1<<i,i)); //try all nodes as starting
        }
        while(!qs.empty()) //until no node is in the queue, which means no node can make it closer
        {
            State state=qs.front();qs.pop();
            for(int next:graph[state.source]) //connected nodes
            {
                int nextmask=state.mask|(1<<next);
                if(dp[next][nextmask]>dp[state.source][state.mask]+1) //if passing its parent node can be closer
                {
                    dp[next][nextmask]=dp[state.source][state.mask]+1;
                    qs.push(State(nextmask,next));//bfs next layer
                }
            }
        }
        //shortest path 
        int ans=INT_MAX;
        for(int i=0;i<m;i++) ans=min(ans,dp[i][(1<<m)-1]);
        return ans;
    }
```

#### comment



### 85. Maximal Rectangle.md
#### problem Summary
Given a matrix mxn, return the max area of rectangle with all 1 inside

#### idea
This can be approached using the max rectangle in histogram using stack.
However we are now using dp method

Dp method can still process row by row, but repeating the 1d problem, which is pretty similar to the histogram problem.

when we add a row matrix[i]:

individual column height: height[j]=0 if m[i,j]=0, height[j]++ if m[i,j]=1

If we know the width for height[j] and then we can calculate the area. (the height is the min from left to right)

needs update row by row

left: left[j]=max(left[j],curr_left) curr_left is the left for current row

right: right[j]=min(right[j],curr_right), curr_right is the right for current row

init left as 0, and right as n.
Since left and right are updated row by row, and we guarantee the height[j] with width (right[j]-left[j]) is a rectangle with all 1.

#### code
```cpp
    int maximalRectangle(vector<vector<char>>& matrix) {
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
                max_area=max(max_area,height[j]*(right[j]-left[j]));
            }
        }
        return max_area;
    }
```

#### comment
1. when meet 0, set left[j]=0 or right[j]=n is necessary since next row update left[j] or right[j] will be bound by previous row. 
2. similar problem maximal square can also be solved using similar code, except the area shall be the min of height and width





### 87. Scramble String.md
#### problem summary
partition a string into left and right recursively.
choose any node and swap its left right branch
The leaf is a scrambled string of original

Check if a new string is a scramble string of s

#### analysis
partition at i, left s[0,i) and right [i,n)
swap left and right: left [i,n) and right [0,i)
we need recursively check if they are scrambled.
left vs left and right vs right
s1[0,i) vs s2[0,i) && s1[i,n) vs s2[i,n)
left vs right and right vs left
s1[0,i) vs s2[n-i,i) && s1[i,n) vs s2[0,n-i)

#### code
```cpp
    bool isScramble(string s1, string s2) {
        if(s1==s2) return 1;
        if(s1.length()!=s2.length()) return 0;
        int n=s1.length();
        //check if the histogram the same
        vector<int> cnt(26);
        for(int i=0;i<s1.length();i++)
        {
            cnt[s1[i]-'a']++;
            cnt[s2[i]-'a']--;
        }
        for(int i=0;i<26;i++) if(cnt[i]) return 0;
        
        for(int i=1;i<=n-1;i++) //possible partition position
        {
            if(isScramble(s1.substr(0,i),s2.substr(0,i)) && isScramble(s1.substr(i),s2.substr(i))) return 1;
            if(isScramble(s1.substr(0,i),s2.substr(n-i)) && isScramble(s1.substr(i),s2.substr(0,n-i))) return 1;
        }
        return 0;
    }
```

#### comment
- the iteration i is actually the length of the left string, make sure it reaches n-1
- prune: s1==s2, length not match, histogram match
- check similar problem 951 Flip Equivalent Binary Trees
It uses a tree
```cpp
    bool flipEquiv(TreeNode* root1, TreeNode* root2) {
        if(!root1 && !root2) return 1; //both null
        else if(!root1 || !root2) return 0;
        if(root1->val!=root2->val) return 0;
        if(flipEquiv(root1->left,root2->left) && flipEquiv(root1->right,root2->right)) return 1;
        if(flipEquiv(root1->left,root2->right) && flipEquiv(root1->right,root2->left)) return 1;
        return 0;
    }
```    
### 871. Minimum Number of Refueling Stops.md
#### problem summary
given initial milage, a list of station at different position with adding milage. Find the min number of refueling station to reach target.

#### idea
This is similar to the super egg drop problem.

By solving an equivalent dp problem: what is the max distance using a number of refuelings

assuming dp[i] is the max distance using i refueling stations

dp[i]=max(dp[i], dp[i-1]+gas[j]) with constrains dp[i-1]>=pos[j] (You have to be able to reach pos[j] to add the gas[j])

#### code
```cpp
    int minRefuelStops(int target, int startFuel, vector<vector<int>>& stations) {
        int n=stations.size();
        vector<int> dp(n+1); //the largest miles at each station
        dp[0]=startFuel;
        for(int i=0;i<n;i++) //iterate on all stations
        {
            int pos=stations[i][0],gas=stations[i][1];
            for(int j=i;j>=0;j--)
            {
                if(dp[j]>=pos) dp[j+1]=max(dp[j+1],dp[j]+gas);  //previous station needs update
            }
        }
        for(int i=0;i<=n;i++) if(dp[i]>=target) return i;
        return -1;
    }
 ```
 
 #### comments
 - need iterate all stations and cannot early exit since each station may contribute to the max distance

 ### 879. Profitable Schemes.md
 #### problem summary
Given G people, a list of profit with person required, and a target profit P. If person is used for one project, then it cannot be used for another one.

Ask: the number of profitable scheme

#### idea
First impression: this is a knapsack problem without repetition. All profitable scheme with total profit>=P. Or all profit >=P can be counted as P.

knapsack dp[i,p]: with number of items and weight p. Since each item can be chosen or not chosen, we iterate the item for all w.
There is another factor: the number of people, making it a 3D dp problem: 
dp[i, p, g]: i: number of items, p: target profit, g: number of people 

Assuming dp[i,p,g] is the number of combinations for using i projects, target profit p, and number of people g.
when we add ith element into it: dp[i,p+pi,g+gi]+=dp[i-1,p,g] (there is no min/max compare since it asks for the number of combinations)

i only involves with i-1, we can safely remove the first dimension but need to use reverse iteration to avoid using updated values

The final answer:
sum of all dp[P,g] 

Boundary condition:
dp[0,0]=1, there is one way to reach 0 and 0 people

#### code
```cpp
   int profitableSchemes(int G, int P, vector<int>& group, vector<int>& profit) {
        int n=group.size();
        vector<vector<int>> dp(P+1,vector<int>(G+1)); //dp is the number of combinations with p, g
        //reduced 3d to 2D
       dp[0][0]=1;
        int mod=1e9+7;
        for(int i=0;i<n;i++)
        {
            int p=profit[i],g=group[i];
            for(int pp=P;pp>=0;pp--)
            {
                for(int gg=G-g;gg>=0;gg--)
                {
                    int p1=pp+p;
                    if(p1>P) p1=P;
                    dp[p1][g+gg]=(dp[p1][g+gg]+dp[pp][gg])%mod;
                }
            }
        }
        return accumulate(dp[P].begin(),dp[P].end(),0LL)%mod;
    }
```

#### comments
- there is multiple way to reach p, g, the total method shall be sum of them, similar to climbing stairs

### 887. Super Egg Drop.md
#### problem summary
given k eggs, and floor 1 to N, there is a floor F 0<=F<<N, when you drop the egg on >F the egg will break. Get the min number of moves required to determine F

#### idea
Since F could be any number, this is a minmax problem, ie. get the min of all the max eggs to guarantee the finding of F.

If we drop egg at floor j, there are two options:

egg does not breaks, then F>j the problem is a smaller problem with k eggs, and N-j floors

egg breaks, the F<j, we are solving problem with k-1 eggs and j-1 floors

so dp(k, n)=min(dp[k,n],1+max(dp[k-1, j-1], dp[k, n-j]) where k is the number of eggs, n is the number of floor, and j is the floor from 1 to n.

The final answer is dp[k,N] with k eggs and N floors

bounary condition:
0 eggs we cannot determine anything, dp[0, i]=0;
i eggs, 0 floor, min number of eggs is also 0. k=0, N=0 is actually not needed. 
1 eggs, i floor, need i moves
i eggs, 1 floor, need 1 moves

And we get the code

```cpp
    int superEggDrop(int K, int N) {
        //
        vector<vector<int>> dp(K+1,vector<int>(N+1));
        for(int i=1;i<=K;i++) dp[i][1]=1;
        for(int i=1;i<=N;i++) dp[1][i]=i;
        for(int k=2;k<=K;k++)
        {
            for(int n=2;n<=N;n++)
            {
                dp[k][n]=INT_MAX;
                for(int j=1;j<=n;j++)
                    dp[k][n]=min(dp[k][n],1+max(dp[k-1][j-1],dp[k][n-j]));
            }
        }
        return dp[K][N];
    }
```
The complexity is O(KN^2), which needs further optimization.
Possible optimization: dp[k-1][j-1] increase with j, and dp[k][N-j] decreases with j. The min of the max of the two can be found using binary search, which reduce the complexity to O(KNlogN)

Inspired @lee215, we can solve an equivalent dp problem: assuming dp[k,m] as using k eggs, m moves, what is the max number of floors we can reach:

if egg breaks: we can check max floor dp[k-1,m-1] <current floor

if egg does not break: we can check max floor dp[k, m-1] > current floor (using current floor as the base 0)

When N floors are checked, F is found. dp[k, m]=N and we find the min m.

dp[k,m]=dp[k-1,m-1]+dp[k,m-1]+1 (the combined below and above number of floors)

Why the above equation: 
dp[k-1, m-1] using m-1 moves and k-1 eggs, the max number of floors we can check (lower than current)
dp[k, m-1] using m-1 moves and k eggs, the max number of floors we can check (higher than current)
+1: the floor we currently checked.

```cpp
    int superEggDrop(int K, int N) {
        //
        vector<vector<int>> dp(K+1,vector<int>(N+1));
        //dp[k,n] now is the max number of floors checked using k eggs and n moves
        for(int m=1;m<=N;m++)
        {
            for(int k=1;k<=K;k++)
                dp[k][m]=dp[k][m-1]+dp[k-1][m-1]+1;
            if(dp[K][m]>=N) return m;
        }
    }
```

Above solution can be reduced to 1D since it only involves with m-1. Please attention if using 1D, need to reverse iterate on k since we don't want to use the updated dp[k-1][m] to replace dp[k-1][m-1].

### 903. Valid Permutations for DI Sequence.md
#### problem Summary
D: decrease A[i]>A[i+1], I: increase, A[i]<A[i+1]
Given numbers from 0 to n and DI string of length=n
Ask: number of valid permutations

#### idea
let dp[i,j] represents number of valid permutation with string length i (given number 0 to i) and ending number j
when D: previous number shall be bigger, i.e, j+1 to n, then dp[i,j]=sum(dp[i-1,k]) k=j..i-1 
when I: previous number shall be smaller, i.e from 0 to j-1 then dp[i,j]=sum(dp[i-1,k]) k=0...j-1

final answer: i=n, ending with 0 to n

boundary condition: It only needs previous row values, so the 0th row shall be provided.
i=0, dp[0,0]=1
Actually this is a lower triangle, we do not need to calculate the whole row, they are not used.

#### code
```cpp
    int numPermsDISequence(string S) {
        int n=S.length();
        vector<vector<int>> dp(n+1,vector<int>(n+1));
        //dp[i,j] represents len=i, ending with j (j=0 to i)
        dp[0][0]=1;
        //for(int i=0;i<=n;i++) dp[0][i]=1;
        int mod=1e9+7;
        for(int i=1;i<=n;i++)
        {
            for(int j=0;j<=i;j++)
            {
                if(S[i-1]=='D') //previous is larger
                {
                    for(int k=j;k<i;k++) {dp[i][j]+=dp[i-1][k];dp[i][j]%=mod;}
                }
                else for(int k=0;k<j;k++) {dp[i][j]+=dp[i-1][k];dp[i][j]%=mod;}
            }
        }
        //print(dp);
        return accumulate(dp[n].begin(),dp[n].end(),0LL)%mod;
    }
```

#### comment
1. when D, **why sum from j to i-1 instead from j+1 to i**? Because for (0...i-1) cases, k can only change from 0 to i-1. for (0..i) case, j can from 0 to i. The j is used already in the previous solution.

So, why start with j, not j + 1, since the sequence is decreasing to j?
Thought Experiment: 
In the sequence with length of i-1, the largest number in this sequence should be i-1. 
However, when we are dealing with length i and end with j, the previous sequence has already another j and we should also add i to the sequence. What we can do is, add one to all those numbers greater than or equal to j. This operation will make the largest number to be i without breaking the sequence property, also, it will free the j so that we can use it at the end of the sequence. 

### 920. Number of Music Playlists.md
#### problem Summary
Given N different songs, create a playlist with L songs, requiring:

each song play at least one time

same song can be played only if K other songs has played
0 <= K < N <= L <= 100

Ask: number of possible playlist (mod 1e9+7)

#### analysis
seems no clue?

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

#### comments
- has to use long long for the type since dp[n,l]+= will overflow and we have no way to avoid that
- make sure n>K is added otherwise it will add a negative value
- The main difficulty of the problem is the thinking process or the dp methodology.

### 940. Distinct Subsequences II.md
#### problem summary
Given a string, count all distinct subsequence

#### idea
for example aba
a: ending with a
ab,b: ending with b
ba,aba: ending with a (a is duplicate and shall be removed)

Since the number of subsequence ending with same char could be very large, it is not suitable to store them in a data structure, hence we need find a way to get rid of those duplicates.

When we add a char to the end, we append a char to all previous string ending with from a to z. This is the key observation.
we can use an array of 26 to record the number.

adding all numbers together and the letter itself dp[c]=sum(dp(a to z))+1

#### code
```cpp
    int distinctSubseqII(string S) {
        vector<int> dp(26); //store the number of strings ending with a char a to z
        int mod=1e9+7;
        for(int i=0;i<S.size();i++)
        {
            int ind=S[i]-'a';
            long long t=0;
            for(int j=0;j<26;j++) {t+=dp[j];t%=mod;}
            dp[ind]=t+1; //the letter itself
        }
        return accumulate(dp.begin(),dp.end(),0LL)%mod;
    }
```

#### comments
- be sure to use long long to avoid overflow.
- use 26 char array and update the table is a common practice for string dp problem


### 97. Interleaving String.md
#### problem summary: 
check if s1 and s2 interleaves to s3

#### Approach
This is similar to a mxn matrix which we can find a path of s3

dp[i, j] represents s1[0...i-1] with s2[0...j-1] can interleave to s3[0...i+j-1]

when s1[i-1]==s3[i+j-1] we may choose s1[i-1], previous solution is dp[i-1, j]

when s2[j-1]==s3[i+j-1], we may choose s2[j-1], previous solution is dp[i, j-1]

when both s1[i-1] and s2[j-1] equals s3[i+j-1] we may choose either of them.

So, the recurrence relation is:

dp[i, j]=(dp[i-1, j] && s1[i-1]==s3[i+j-1]) || (dp[i, j-1] && s2[j-1]==s3[i+j-1])

Boundary condition:
0th row: when s1 is empty, dp[0,i] depends previous dp[0, i-1] and current char if the same

0th col: when s2 is empty, dp[i,0] depends previous dp[i-1,0] and current char if the same

#### code
```cpp

    bool isInterleave(string s1, string s2, string s3) {
        int n1=s1.size(),n2=s2.size();
        if(s3.length() != n1+n2) return 0;
        if(n1==0) return s3==s2;
        if(n2==0) return s3==s1;
        vector<vector<bool>> dp(n1+1,vector<bool>(n2+1));
        dp[0][0]=1; //empty vs empty
        //boundary condition
        for(int i=1;i<=n1;i++) dp[i][0]=dp[i-1][0] && (s1[i-1]==s3[i-1]); //j=0, s1 compare with s3
        for(int j=1;j<=n2;j++) dp[0][j]=dp[0][j-1] && (s2[j-1]==s3[j-1]); //i=0: s2 compare with s3
        for(int i=1; i<=n1; i++)
        {
            for(int j=1; j<=n2; j++)
            {
                dp[i][j] = (dp[i-1][j] && s1[i-1] == s3[i+j-1] ) || (dp[i][j-1] && s2[j-1] == s3[i+j-1] );
            }
        }   
        return dp[n1][n2];
    }
    
```

#### Attention:
- this is a direct dp problem, which uses dp(i, j) directly for the string s1 with len i and s2 with string j and s3 with len i+j
- deal with special case: which is easy
- boundary condition: easy but if incorrectly specified, will get wrong results
- complexity O(N^2)

# Chapter 4. Greedy

Greedy:
choose a first move and proof it is a safe move which is consistent to a global optimal solution. Then the problem is reduced to a smaller sized subproblem.
Generally sort will improve the efficiency.

example: 
- n digits to form a largest number, each time choose the largest number from remaining digits
- min refueling stations: refill at the farthest reachable station
- min group of children with age difference <=1, sort first and use segment to cover the leftmost.
- fractional knapsack: sort with the value/weight, and choose as much as possible the current highest value.

If your first move is not safe, the solution is incorrect.

944. Delete Columns to Make Sorted
just count the unsorted column. To improve efficiency, we just compare neigboring char, O(NM)
```cpp
    int minDeletionSize(vector<string>& A) {
        //using dp: adding one char, if the column is sorted, it is the length of dp[i-1]
        int m=A.size(),n=A[0].size(); //m is row, n is col
        int ans=0;
        for(int i=0;i<n;i++)
        {
            for(int j=1;j<m;j++) 
                if(A[j][i]<A[j-1][i]) {ans++;break;}
        }
        return ans;
    }
```

122. Best Time to Buy and Sell Stock II
as many as transactions.
Greedy: just compare with previous price, if profit >0 then make it.
```cpp
    int maxProfit(vector<int>& prices) {
        int profit=0;
        for(int i=1;i<prices.size();i++)
        {
            profit+=max(prices[i]-prices[i-1],0);
        }
        return profit;
    }
```
A transaction is defined as buying at Prices[X] and selling at Prices[Y],

the profit of the transaction
= Prices[Y] - Prices[X] 
= Prices[Y] - Prices[Y-1] +
   Prices[Y-1] - Prices[Y-2] ...
    ....
   Prices[X+1] - Prices[X] 
= D[Y] + D[Y-1] + ... + D[X+1]
= sum of D from X+1 to Y

860. Lemonade Change
greedy: always gives changes of the large bill first. 

saving the change for 5, 10, and 20 separately.

455. Assign Cookies
given a list of children greedy size, and a list of candy size, return the number of satisifcation (each child one candy)
greedy choice: bucket sort or using map to sort both the greedy and the size. Greedy choice: assign the candy to the child who first meet the requirement.
```cpp
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        //use two pointers
        int j=0;
        for(int i=0;i<s.size() && j<g.size();i++)
            if(g[j]<=s[i]) {j++;}
        return j;
    }
```
using map for bucket sorting is slower and more complicated, two pointer is more efficient.

874. Walking Robot Simulation
starting from (0,0), -2: turn left, -1: turn right, x (1,9) forward nsteps. with obstacle, then stop before the obstacle.
get the largest distance from the origin.

each step we can calculate its position and then we get the largest distance

we have 4 directions: -x, +x, -y, +y which can be represented as (-1,0) (1,0) (0,-1) (0,1)
turn right: rotate -90 degrees: 
turn left: rotate 90 degrees: 
we can use rotation matrix for the direction changes.
how to easily judge if the obstacle is on the way?
can use a string to include the x and y coordinate and move step by step and stop when we meet an obstacle.


921. Minimum Add to Make Parentheses Valid
cannot just count the ( and ) for example )(. The ( shall have a ) followed. The ) shall have a preceding (. We use a stack for this, removing all those paired.
```cpp
    int minAddToMakeValid(string S) {
        //to make it balanced, the () shall be the same amount
        //one we have a ) we need have a balanced in its front
        //once we have a ( we need have a balanced in the behind
        stack<char> st;
        int ans=0;
        for(int i=0;i<S.size();i++)
        {
            if(S[i]=='(') st.push(S[i]);
            else 
            {
                if(st.size()) st.pop();
                else ans++; 
            }
        }
        return ans+st.size();
    }
```

using two pointers for the two conditions will save the extra storage using stack


861. Score After Flipping Matrix
a move choose any row or column and toggle its value.
Each row represents a binary number, return the largest sum of all these numbers.
greedy choice: 
the MSB shall all toggles to 1, by toggling the row
the non-MSB shall have more 1s than 0s. toggling columns

763. Partition Labels
split the string so that each char only appear in one part.
straightforward:
record each char's start and ending position and then merge the segments if they overlap.
efficient one:
don't use hashmap but use array for each char and record its ending index. Merging the segments only matters the ending.
so use two passes:
```cpp
    vector<int> partitionLabels(string S) {
        vector<int> charIdx(26, 0);
        for(int i = 0; i < S.size(); i++){
            charIdx[S[i]-'a'] = i;
        }
        
        vector<int> results;
        
        int maxIdx = -1, lastIdx = 0;
        for(int i = 0; i < S.size(); i++){
            maxIdx = max(maxIdx, charIdx[S[i]-'a']);
            if(i == maxIdx) {
                results.push_back(maxIdx - lastIdx + 1);
                lastIdx = i+1;
            }
        }
        return results;
    }
```

406. Queue Reconstruction by Height
each person has a height and number of person taller or equal to him in front of him.
reconstruct the queue
sort the people in descend order but the rank in ascending order, so the same height people is in order. Then insert each lower person.
This is a greedy choice.
for example: [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
sort as: [7,0], [7,1], [6,1],[5,0],[5,2],[4,4]
each step:
[7,0]
[7,0], [7,1]
[7,0],[6,1],[7,1]
[5,0],[7,0],[6,1],[7,1]
[5,0],[7,0],[6,1],[5,2],[7,1]
[5,0],[7,0],[6,1],[5,2],[4,4],[7,1]
sorting is the key to make sure all member inside the array are taller than current one.
```cpp
bool cmp(pair<int,int>& a,pair<int,int>& b) {return a.first>b.first || (a.first==b.first && a.second<b.second);}
    vector<pair<int, int>> reconstructQueue(vector<pair<int, int>>& people) {
        //reverse: we sort the highest person first, they are ordered
        //then the next, 
        sort(people.begin(),people.end(),cmp); //descending order
        vector<pair<int,int>> vs;
        for(int i=0;i<people.size();i++)
        {
            vs.insert(vs.begin()+people[i].second,people[i]);
            for(int i=0;i<vs.size();i++) cout<<"("<<vs[i].first<<" "<<vs[i].second<<") ";cout<<endl;
        }
        return vs;
    }
```

714. Best Time to Buy and Sell Stock with Transaction Fee
see dp solution
dp[i]=max(dp[i-1],dp[j]+price[i]-price[j]-fee) j=0 to i-1
dp[i]=max(dp[i-1],price[i]+max(dp[j]-price[j]-fee)) j=0 to i-1 and this can be updated iteratively and reducing the complexity from O(N^2) to O(N)
```cpp
    int maxProfit(vector<int>& prices, int fee) {
        int n=prices.size();
        vector<int> dp(n);//dp is the max profit ending at ith day
        int tmax=INT_MIN;
        for(int i=1;i<n;i++)
        {
            tmax=max(tmax,dp[i-1]-prices[i-1]-fee);
            dp[i]=max(dp[i-1],prices[i]+tmax);
        }
        return dp[n-1];
    }
```    
greedy choice:
when current price>previous price+fee, perform a transaction, is this a safe move?</br>
a<b<c<d, and b>a+fee, d>c+fee, then b-a-fee+d-c-fee ==? d-a-fee not really. so this approach is incorrect.</br>
for example fee is 1, 4,6,8,10, we get 3 using first approach, 5 for 2nd approach.</br>
we buy at 4, sell at 6 (equiv sell at 5), buy at 6 sell at 8 (equiv sell at 7). In this case we cannot start a new transaction.</br>
for example 4,6,3,8. buy at 4 sell at 6, profit is 1, buy at 3 sell at 8, profit is 4, total profit is 5. </br>
for example 4,6,5,8. buy at 4 sell at 6, profit is 1, buy at 5 sell at 8, profit is 2, total profit is 3. </br>
so what is the safe move?</br>
we shall tolerate some variation, only when price[i]<price[i-1]-fee we can sell at i-1 and buy at i. (i.e the dip shall be deep enough to cover the transaction fee, otherwise it is not worthful). Once we start a transaction, we reduce the problem into a smaller subproblem with the new minimum, which is a typical greedy approach.</br>

```cpp
    int maxProfit(vector<int>& prices, int fee) {
        //greedy solution
        //only when price[i]<price[i-1]-fee we can start buy at ith day
        int n=prices.size();
        if(n<2) return 0;
        int minimum=prices[0];
        int maxprof=0;
        for(int i=1;i<n;i++)
        {
            if(prices[i]<minimum) //prices[i-1]-fee
                minimum=prices[i];
            else if(prices[i]>minimum+fee)
            {
                maxprof+=prices[i]-minimum-fee;
                minimum=prices[i]-fee;
            }
        }
        return maxprof;
    }
```
The greedy is not so understandable, and most of greedy can be solved using dp approach.

392. Is Subsequence
check if s is subsequence of t.</br>
dp approach: by deleting chars from t to see if we can ontain s</br>
greedy: choose the first matching char in t using two pointers</br>
```cpp
    bool isSubsequence(string s, string t) {
        //use two pointer
        int i=0,j=0;
        while(i<s.length() && j<t.length())
        {
            while(t[j]!=s[i])
            {
                j++;
                if(j==t.length()) return 0;
            }
            i++,j++;
        }
        return i==s.length() && j<=t.length();
    }
```

452. Minimum Number of Arrows to Burst Balloons
provided the start coordinate and end coordinate of each balloon along x-axis. Arrows are shot up and burst all the balloons along its path. Return the min number of arrows to burst all the balloons.</br>
this is to find the overlaps.</br>
Approach: sort the segments according to its end. If current segment > current end, then we need start a new overlap</br>
what position should we pick each time? We should shoot as to the right as possible, because since balloons are sorted, this gives you the best chance to take down more balloons. </br>
ie. greedy choice is to choose the left most segment ending point and shoot all those overlapping balloons.</br>
this problem is similar to those overlapping interval problems such as double booking, triple booking et al.</br>

```cpp
    int findMinArrowShots(vector<pair<int, int>>& points) {
        if(points.size()<2) return points.size();
        sort(points.begin(),points.end(),cmp);
        int ans=0;
        int cur_end=points[0].second;ans++;
        for(int i=1;i<points.size();i++)
        {
            if(points[i].first>cur_end) {cur_end=points[i].second;ans++;}
        }
        return ans;
    }
```

621. Task Scheduler
same task needs at least n intervals (adding idle). </br>
return the number of intervals to complete all tasks </br>
greedy approach: try to take one from each set of task and add zero or more idles for this to satisfy n intervals. Always take one task from current max task. 
Assuming the highest frequency is K, then we divide it into k chunks. each chunk will fill one task from each set. (k-1)*(n+1)

First count the number of occurrences of each element.
Let the max frequency seen be M for element E
We can schedule the first M-1 occurrences of E, each E will be followed by at least N CPU cycles in between successive schedules of E
Total CPU cycles after scheduling M-1 occurrences of E = (M-1) * (N + 1) // 1 comes for the CPU cycle for E itself
Now schedule the final round of tasks. We will need at least 1 CPU cycle of the last occurrence of E. If there are multiple tasks with frequency M, they will all need 1 more cycle.
Run through the frequency dictionary and for every element which has frequency == M, add 1 cycle to result.
If we have more number of tasks than the max slots we need as computed above we will return the length of the tasks array as we need at least those many CPU cycles.

For the last point I think you should explain more clearly.

Support we have AAABBBCCDDEF n = 2

No matter how we arrange, we at least have the following schedule:

A cool cool A cool cool A
In this way, we could replace the first 'cool' with B. then it becomes.

A B cool A B cool A B

With the remain C, we could
A B C A B C A B
for the def,
A B C (D) E A B C (D) F
we insert at the position of end of each period of A B C. then we could ensure that there would be no collision

```cpp
    int leastInterval(vector<char>& tasks, int n) {
        unordered_map<char,int>mp;
        int count = 0;
        for(auto e : tasks)
        {
            mp[e]++;
            count = max(count, mp[e]);
        }
        
        int ans = (count-1)*(n+1);
        for(auto e : mp) if(e.second == count) ans++;
        return max((int)tasks.size(), ans);
    }
```

881. Boats to Save People
given a list of person's weight, each boat has a weight limit and 2 person limit. And it is guaranteed that the boat can carry one person.
return the min number of boats required.
greedy: sort the weight and use two pointers. If the higher weight + lower weight exceeds, only moves the higher end. The lower weight needs to be waited to be together with other person.
```cpp
    int numRescueBoats(vector<int>& people, int limit) {
        sort(people.begin(),people.end());
        int i=0,j=people.size()-1;
        int total=0;
        while(i<=j)
        {
            if(people[i]+people[j]<=limit) {i++;}
            j--;
            total++;
        }
        return total;
    }
```

435. Non-overlapping Intervals
given a collection of overlapping intervals, return the min number of intervals to remove to make them non-overlapping.
Actually, the problem is the same as "Given a collection of intervals, find the maximum number of intervals that are non-overlapping." (the classic Greedy problem: Interval Scheduling). With the solution to that problem, guess how do we get the minimum number of intervals to remove? : )

Sorting Interval.end in ascending order is O(nlogn), then traverse intervals array to get the maximum number of non-overlapping intervals is O(n). Total is O(nlogn).

```cpp
    int eraseOverlapIntervals(vector<Interval>& intervals) {
        if(intervals.size()==0) return 0;
        sort(intervals.begin(),intervals.end(),cmp);
        int ans=0;
        int cur_end=intervals[0].end;ans++;
        for(int i=0;i<intervals.size();i++)
        {
            if(intervals[i].start>=cur_end) {cur_end=intervals[i].end;ans++;}
        }
        return intervals.size()-ans;
    }
```

738. Monotone Increasing Digits
Given a non-negative integer N, find the largest number that is less than or equal to N with monotone increasing digits.
(digits are sorted)
greedy: reversely find the inversion. if found an inversion we reduce the previous by 1, thus to propagate the changes forward.

example: 3421->3411->3311->3399
example: 3431->3421->3321->3399
example: 144267->144267->144267->143267->133267->13999
```cpp
    int monotoneIncreasingDigits(int N) {
        string n_str = to_string(N);
        int marker = n_str.size();
        for(int i = n_str.size()-1; i > 0; i --) {
            if(n_str[i] < n_str[i-1]) {
                marker = i;
                n_str[i-1]--;// = n_str[i-1]-1;
            }
        }
        for(int i = marker; i < n_str.size(); i ++) n_str[i] = '9';
        return stoi(n_str);
    }
```

870. Advantage Shuffle
Given two arrays A and B of equal size, the advantage of A with respect to B is the number of indices i for which A[i] > B[i].

Return any permutation of A that maximizes its advantage with respect to B.
Greedy solution: if there is an element in A >B[i], then choose the smallest one. 
运用田忌赛马的思路，如果A中有相应的higher元素，使用最小的higher元素。否则使用A中最小的元素 :)

767. Reorganize String
neighboring characters are not the same.
if the highest freq char appears more than half of the length, we cannot fulfill the requirement.
Otherwise we divide into k chunks by appending other chars into the chunk.
c++ using priority_queue by removing the highest freq one and followed by the 2nd highest priority one. -1 and put back. A good example to use PQ for this.

```cpp
    string reorganizeString(string S) {
        priority_queue<pair<int, char>> pq;
        int map[26] = { 0 };
        
        for (auto c : S) 
        {
            if (++map[c-'a'] > (S.size() + 1)/2)
                return "";
        }
        
        for (int i = 0; i < 26; ++i) 
        {
            if (map[i]) pq.push({map[i], i + 'a'});
        }
        
        string ans;
        while(!pq.empty()) 
        {
            pair<int, char> p1, p2;
            p1 = pq.top(); pq.pop();
            ans.push_back(p1.second);
            if (!pq.empty()) 
            {
                p2 = pq.top(); pq.pop();
                ans.push_back(p2.second);
                if (--p2.first) pq.push(p2);
            }
            if (--p1.first) pq.push(p1);
        }
        return ans;
    }
```    

659. Split Array into Consecutive Subsequences

You are given an integer array sorted in ascending order (may contain duplicates), you need to split them into several subsequences, where each subsequences consist of at least 3 consecutive integers. Return whether you can make such a split.

for each card, either it can be appended to existent, or start a new group or it is a single

```cpp
    bool isPossible(vector<int>& nums) {
        unordered_map<int, int> freq, need;
        for (int num : nums) ++freq[num];
        for (int num : nums) {
            if (freq[num] == 0) continue;
            else if (need[num] > 0) {
                --need[num];
                ++need[num + 1];
            } else if (freq[num + 1] > 0 && freq[num + 2] > 0) {
                --freq[num + 1];
                --freq[num + 2];
                ++need[num + 3];
            } else return false;
            --freq[num];
        }
        return true;
    }
```

948. Bag of Tokens
given a list of tokens with power[i]. We put the token down, losing the power and gain 1 point. We put the token up, losing one point and gain power[i].
What the max point we can get with initial power P and 0 points.
Greedy choice: losing min power to get 1 point and lost 1 point to get the max power.
sort the power and uses two pointers
```cpp
    int bagOfTokensScore(vector<int>& tokens, int P) {
        //greedy problem: lose min power for point, lose point for max power
        if(tokens.size()==0) return 0;
        if(tokens.size()==1) return P>tokens[0];
        sort(tokens.begin(),tokens.end());
        int n=tokens.size();
        int i=0,j=n-1;
        if(P<tokens[0]) return 0;
        int maxpoint=0;
        int points=0,power=P;
        while(i<j)
        {
            while(i<=j && power>=tokens[i]) 
            {
                points++;power-=tokens[i];
                maxpoint=max(maxpoint,points);i++;
            }
            if(i<j && points) {points--;power+=tokens[j];j--;}
        }
        return maxpoint;
    }
```

649. Dota2 Senate
two parties. according order, each senate can ban a senator from other party, predict who is the winner.
greedy: ban its next member from other party since otherwise it will ban a member from your party.
using two queues: if the member exercises its right, move it to the end of the queue.
```cpp
string predictPartyVictory(string senate) {
        queue<int> q1, q2;
        int n = senate.length();
        for(int i = 0; i<n; i++)
            (senate[i] == 'R')?q1.push(i):q2.push(i);
        while(!q1.empty() && !q2.empty()){
            int r_index = q1.front(), d_index = q2.front();
            q1.pop(), q2.pop();
            (r_index < d_index)?q1.push(r_index + n):q2.push(d_index + n);
        }
        return (q1.size() > q2.size())? "Radiant" : "Dire";
    }
```

376. Wiggle Subsequence
greedy or dp: the first number could be smaller or larger number.
```cpp
    int wiggleMaxLength(vector<int>& nums) {
        if(nums.size()==0) return 0;
        int up=1,dn=1;
        for(int i=1;i<nums.size();i++)
        {
            if(nums[i]>nums[i-1]) up=dn+1;
            if(nums[i]<nums[i-1]) dn=up+1;
            //cout<<nums[i]<<"\t"<<up<<" "<<dn<<endl;
        }
        return max(up,dn);
    }
```

842. Split Array into Fibonacci Sequence
given a string of digits, return the Fib series if not able to return empty string
each integer fits in a 32 bit

The first two number choice determines the whole string. 
first number can choose up to 10 digits
second number can choose up to 10 digits
and this is a somewhat greedy choice.

backtracking algorithm
```cpp
    vector<int> splitIntoFibonacci(string S) {
        vector<int> nums;
        backtrack(S, 0, nums);
        return nums;
    }
    bool backtrack(string &S, int start, vector<int> &nums){        
        int n = S.size();
        if(start >= n && nums.size()>=3) return 1;
        int maxSplitSize = (S[start]=='0') ? 1 : 10;
        for(int i=1; i<=maxSplitSize && start+i<=S.size(); i++)
        {
            long long num = stoll(S.substr(start, i));
            if(num > INT_MAX) return 0;
            int sz = nums.size();
            if(sz >= 2 && nums[sz-1]+nums[sz-2] < num) return 0;
            if(sz<=1 || nums[sz-1]+nums[sz-2]==num)
            {
                nums.push_back(num);
                if(backtrack(S, start+i, nums)) return 1;
                nums.pop_back();                
            }
        }
        return 0;
    }
```
- num>INT_MAX, return 0, since adding more chars will be meaningless
- sz>=2 && nums[sz-1]+nums[sz-2]<num why? same reason: if the num is too large, we don't continue since it is wasting time.

134. Gas Station
a circular circuit with each station gas[i] and cost[i], you are starting at 0. cost[i] is the cost from i to i+1 clockwise
-. flatten the circle by copying the array
-. net gas[i]-cost[i]
-. accumulate a N-window sum. Cannot have any station with accumulate sum to be <0

55. Jump Game
given a list of max jump steps at each position, check if we can reach the end

greedy vs dp:
dp is a bottom down, greedy is top down. greedy choose local optimal solution and reach global
dp makes optimal solution based on previous smaller subproblem optimal solution.

Looking from the end and at each point ahead checking the best possible way to reach the end
```cpp
    bool canJump(vector<int>& nums) {
        int n = nums.size();
        vector<bool> jump(n,false);
        jump[n-1]=true;
        
        for(int i=n-2;i>=0;i--)
        {
            for(int j=0;j<=nums[i] && i+j<n;j++)
            {
                if(jump[i+j]==true) 
                {
                    jump[i]=true; 
                    break;
                }
            }
        }
        
        return jump[0];
    }
```
greedy:
```cpp
    bool canJump(vector<int>& nums) {
      int n = nums.size(), farest = 0;
      for(int i = 0;i < n; i++)
      {
        if(farest < i) return false;
        farest = max(i + nums[i], farest);
      }
      
      return true;
    }
```

955. Delete Columns to Make Sorted II
make each row sorted. Once the column is removed, replace it with same char *, and continue to compare.

402. Remove K Digits
to make the number the smallest.
for example 1432219, k=3
greedy choice: remove the first peak digit from left to right
we can use a stack to find the peak digit to make it O(n)
note: the accepted submission cannot process string with no peaks correctly. (if there is no peak, it will not delete any digits)

```cpp
string removeKdigits(string num, int k) {
        string res;
        int keep = num.size() - k;
        for (int i=0; i<num.size(); i++) {
            while (res.size()>0 && res.back()>num[i] && k>0) {
                res.pop_back();
                k--;
            }
            res.push_back(num[i]);
        }
        res.erase(keep, string::npos);
        
        // trim leading zeros
        int s = 0;
        while (s<(int)res.size()-1 && res[s]=='0')  s++;
        res.erase(0, s);
        
        return res=="" ? "0" : res;
    }
```

910. Smallest Range II
given an array, each element can be add K or minus K, return the smallest difference between the max and min
greedy: sort the array, the left side always +k, and the right side always -k. we iterate each element as the left/right and get the min.

or it is equivalent to add 0 or 2K to each element. 
sort is the key step.

135. Candy.md
### Problem Summary
There are N children standing in a line. Each child is assigned a rating value.

You are giving candies to these children subjected to the following requirements:

Each child must have at least one candy.
Children with a higher rating get more candies than their neighbors.
What is the minimum candies you must give?

### Analysis
example [1,0,2] you need given 2,1,2 candies to each.
- each one at least have one candy, initialize to 1
- left to right scan, if rating > previous one, we need add one candy
- right to left scan, if rating > previous one (behind it), we need check if we need add one.

thus we guarantee the left and right side. this is more likely a dp solution

```cpp
    int candy(vector<int>& ratings) {
        int ans=0;
        int n=ratings.size();
        vector<int> can(n,1);
        //from left to right
        for(int i=1;i<n;i++) if(ratings[i]>ratings[i-1]) can[i]=can[i-1]+1;
        for(int i=n-2;i>=0;i--) if(ratings[i]>ratings[i+1]) can[i]=max(can[i],can[i+1]+1);
       
        return accumulate(can.begin(),can.end(),0);
    }
```    
316. Remove Duplicate Letters.md
### Problem Summary
remove duplicate letters from string so that every char occurs only once. return the smallest string remaining

### Analysis

naive approach:
We can safely store the information in a map of char vs its index list.
find the first smallest char who has larger chars followed, and this char is used and removed from the map.
reduce the problem to smaller size.
once the char is chosen, all previous index shall be removed.
```cpp
    string removeDuplicateLetters(string s) {
        //create a map for each character
        map<char,vector<int>> freq;
        for(int i=0;i<s.size();i++) freq[s[i]].push_back(i);
//greedy choice: the leftmost char shall have an index smaller than all other larger char's max index, 
//this letter shall be as small as possible
        string ans;
        auto it=freq.begin();
        bool found=0;
        int nchar=freq.size();
        while(ans.size()<nchar)
        {
            found=1;
            for(auto it1=freq.begin();it1!=freq.end();it1++)
            {
                if(it1==it) continue;
                if(it->second[0]>it1->second.back()) {it++;found=0;break;}
            }
            if(found) 
            {
                int ind=it->second[0];
                ans+=it->first;
                freq.erase(it);
                for(auto it1=freq.begin();it1!=freq.end();it1++) //remove all index smaller than ind
                {
                    auto it2=lower_bound(it1->second.begin(),it1->second.end(),ind);
                    it1->second.erase(it1->second.begin(),it2);
                }
                it=freq.begin();
            }
        }
        return ans;
    }
```    

Optimization
 If we think about this problem intuitively, you would sort of go from the beginning of the string and start removing one if there is still the same character left and a smaller character is after it.  Given "bcabc", when you see a 'b', keep it and continue with the search, then keep the following 'c', then we see an 'a'. Now we get a chance to get a smaller lexi order, you can check if after 'a', there is still 'b' and 'c' or not. We indeed have them and "abc" will be our result.
```java
public String removeDuplicateLetters(String s) {
    Stack<Character> stack = new Stack<>();
    int[] count = new int[26];
    char[] arr = s.toCharArray();
    for(char c : arr) count[c-'a']++;
    
    boolean[] visited = new boolean[26];
    for(char c : arr) 
    {
        count[c-'a']--;
        if(visited[c-'a']) continue;
        
        while(!stack.isEmpty() && stack.peek() > c && count[stack.peek()-'a'] > 0) 
        {
            visited[stack.peek()-'a'] = false;
            stack.pop();
        }
        stack.push(c);
        visited[c-'a'] = true;
    }
    StringBuilder sb = new StringBuilder();
    for(char c : stack) {sb.append(c); }
    return sb.toString();
}
```

321. Create Maximum Number.md
### Problem Summary
Given two arrays of length m and n with digits 0-9 representing two numbers. Create the maximum number of length k <= m + n from digits of the two. The relative order of the digits from the same array must be preserved. Return an array of the k digits.

### Approach
- try all combination i+j=k which take i digits from array 1 and j digits from array 2
- reduces to problem get n digits from one array
- merge sort to get the max number

```cpp
    vector<int> maxNumber(vector<int>& nums1, vector<int>& nums2, int k) {
        int m=nums1.size(),n=nums2.size();
        //cout<<m<<" "<<n<<" "<<k<<endl;
        if(k>=m+n)  //just merge the two arrays
            return merge(nums1,nums2);
        //try all possible combinations, i from array 1, k-i from array 2
        //i range is [max(m-k,0),min(k,m)], other array range is [max(n-k,0),min(k,n)]
        vector<int> ans(k);
        for(int i=0;i<=min(m,k);i++)
        {
            if(k-i>nums2.size()) continue;
            vector<int> a=maxNumber(nums1,i);
            vector<int> b=maxNumber(nums2,k-i);
            vector<int> c=merge(a,b);
            if(lexicographical_compare(ans.begin(),ans.end(),c.begin(),c.end())) ans=c;
        }
        return ans;
    }
    
    vector<int> maxNumber(vector<int>& v,int k) //get k digits from 1 array
    {
        int n=v.size();
        if(k>=n) return v;
        if(k==0) return vector<int>();
        vector<int> ans(k);
        //greedy choice, the leftmost digit is the max from i+[0,n-(k-1))
        int i=0;
        for(int j=1;j<=k;j++) //repeat k times
        {
            auto it=max_element(v.begin()+i,v.begin()+n-(k-j));
            i=int(it-v.begin())+1; //now the new pointer
            ans[j-1]=*it;
        }
        return ans;
    }
    vector<int> merge(vector<int>& v1,vector<int>& v2)
    {
        int i=0,j=0,k=0;
        vector<int> ans(v1.size()+v2.size());
        while(i<v1.size() && j<v2.size())
        {
            if(v1[i]>v2[j]) {ans[k++]=v1[i++];}
            else if(v1[i]<v2[j]) {ans[k++]=v2[j++];}
            else //two number is equal, choose the one behind is larger
            {
                if(lexicographical_compare(v1.begin()+i,v1.end(),v2.begin()+j,v2.end())) //v1<v2
                   ans[k++]=v2[j++];
                else ans[k++]=v1[i++];
            }
        }
        if(i<v1.size()) copy(v1.begin()+i,v1.end(),ans.begin()+k);
        if(j<v2.size()) copy(v2.begin()+j,v2.end(),ans.begin()+k);
        return ans;
    }
```    

330. Patching Array.md
### Problem Summary
Given N and a number array, check the min numbers to be added into the array to get the sum from 1 to n
input array is sorted

example:
nums = [1, 2, 4, 13, 43] and n = 100
1 covers 1
1,2: covers 1,2,3
1,2,4: covers 1,2,3,4,5,6,7
missing 8, add 8, it can covers up to 15
1,2,4,8,13 covers up to 28 (13+15)
missing 29, add 29
1,2,4,8,13,29, covers up to 28+29=57
1,2,4,8,13,29,43, covers up to 100

so if previous m element can sum to [1,M]. If M+1 is not covered, we shall add it

```cpp
    int minPatches(vector<int>& nums, int n) {
        //the fastest way to go to n without leaving out any number
        //if the smaller subproblem  solves from 1 to m, then add a number m+1, it solves the problem from 1 to 2m+1
        //the greedy choice is the smaller subproblem's sum+1
         long miss = 1, added = 0, i = 0;
         while (miss <= n) 
         {
            if (i < nums.size() && nums[i] <= miss) 
            {
                miss += nums[i++];
            } 
            else 
            {
                miss += miss;
                added++;
            }
        }
        return added;
    }
```    

45. Jump Game II.md
### Problem Summary
Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

### Analysis
- starting at the first step i=0, max range is num[0]. Which covers the node from 1 to num[0]
- we shall check the next max range using all those nodes
- the first one > n-1 is the min level

```cpp
    int jump(vector<int>& nums) {
    int n=nums.size();
	 if(n<2)return 0;
	 int level=0,currentMax=0,i=0,nextMax=0;

	 while(currentMax-i+1>0)
     {		//nodes count of current level>0
		 level++;
		 for(;i<=currentMax;i++)
         {	//traverse current level , and update the max reach of next level
			nextMax=max(nextMax,nums[i]+i);
			if(nextMax>=n-1)return level;   // if last element is in level+1,  then the min jump=level 
		 }
		 currentMax=nextMax;
	 }
	 return 0;
 }        
 ```
 
502. IPO.md
### Problem Summary
Given k projects, each project has a profit Pi and cost Wi, Initial capital is W. 
Pick up at most k projects to maximize the profit, return the max profit.

### Approach
- find the projects which requires cost<=current capital (using sort/binary search or minheap: pq or multiset)
- choose the max profit project first (using priority-queue)

```cpp
bool cmp(pair<int,int> a,pair<int,int> b) {return a.first<b.first;}
class Solution {
public:
    int findMaximizedCapital(int k, int W, vector<int>& Profits, vector<int>& Capital) {
        vector<pair<int,int>> vs;
        for(int i=0;i<Profits.size();i++) 
            if(Profits[i]) vs.push_back(make_pair(Capital[i],Profits[i]));
        priority_queue<int> pq;
        sort(vs.begin(),vs.end(),cmp);
        int ind=0,nproj=0,w=W;
        while(nproj<k)
        {
            auto it=upper_bound(vs.begin()+ind,vs.end(),make_pair(w,0),cmp);
            if(it==vs.begin()) break;
            for(auto it1=vs.begin()+ind;it1!=it;it1++) pq.push(it1->second);
            if(!pq.empty()){w+=pq.top();pq.pop();}
            ind=int(it-vs.begin());
            nproj++;
        }
        return w;
    }
};
```
630. Course Schedule III.md
### Problem Summary
Given a list of courses (t,d): t: duration day, d: closing date
return max number of courses taken

### Analysis
greedy: always choose the shorter course
approach: sort the courses according to its closing date. push the course duration time into priority-queue. When the finish time > closing date, we pop out the longest course in the pq.

### code
```cpp
bool cmp(vector<int>& a,vector<int>& b) {return a[1]<b[1] || (a[1]==b[1] && a[0]<b[0]);}
class Solution {
public:
    int scheduleCourse(vector<vector<int>>& courses) {
        sort(courses.begin(),courses.end(),cmp);
        priority_queue<int> pq;
        int n=courses.size();
        int s=0; //s is the current time
        for(int i=0;i<n;i++)
        {
            s+=courses[i][0]; //if we select this course, advance the time
            pq.push(courses[i][0]);
            if(s>courses[i][1]) //out of closing date, try to remove the longest 
            {s-=pq.top();pq.pop();} //
        }
        return pq.size();
    }
};
```
757. Set Intersection Size At Least Two.md
### Problem Summary
An integer interval [a, b] (for integers a < b) is a set of all consecutive integers from a to b, including a and b.

Find the minimum size of a set S such that for every integer interval A in intervals, the intersection of S with A has size at least 2.

### Analysis
- we can sort the intervals according to its end
- the segment shall cover the first segment for at least two element to the end, the last segment from the start.
- iterate all segments: 
  (1) If there is no number in this interval being chosen before, we pick up 2 biggest number in this interval. (the biggest number have the most possibility to be used by next interval)
  (2) If there is one number in this interval being chosen before, we pick up the biggest number in this interval.
  (3) If there are already two numbers in this interval being chosen before, we can skip this interval since the requirement has been fulfilled.

```cpp
    int intersectionSizeTwo(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [](vector<int>& a, vector<int>& b) 
        {
            return a[1] < b[1] || (a[1] == b[1] && a[0] > b[0]); 
        });
        int n = intervals.size(), ans = 0, p1 = -1, p2 = -1;
        for (int i = 0; i < n; i++) 
        {
            if (intervals[i][0] <= p1) continue;// current p1, p2 works for intervals[i]
            if (intervals[i][0] > p2) // Neither of p1, p2 works for intervals[i]
            {
                ans += 2;
                p2 = intervals[i][1];// replace p1, p2 by ending numbers
                p1 = p2-1;
            }
            else //only p2 works: start in the range [p1,p2]
            {
                ans++;
                p1 = p2;
                p2 = intervals[i][1];
            }
        }
        return ans;
    }
```

Note: the problem asks for a set not a single interval, for example, [[1,2],[9,10]] it will return [1,2,9,10] the length is 4

765. Couples Holding Hands.md
### Problem Summary
N couples labelled as 0,1,....2*N-1, couples shall sit together. Return the min number of swaps

### Approach
union find using cyclic permutation based on group theory can be applied to find the connected groups (the value and the index)
key observations:
- if we divide each number by 2, we get the group id for all couples
- the final state: position 2i and 2i+1 must have the same group id
- assume a group has i and j, i!=j, then we need swap:
  1. keep i, and swap j with the next i (this at least fix i)
  2. keep j, and swap i with the next j (this is equivalent to above)
  3. thus every time we fix the current group (also may fix some group behind)
we get the following greedy solution:

```cpp
    int minSwapsCouples(vector<int>& row) {
        for(int i=0;i<row.size();i++) row[i]/=2; //making couple the same number
        int cnt=0;
        for(int i=0;i<row.size();i+=2)
        {
            if(row[i]!=row[i+1]) 
            {
                auto it=find(row.begin()+i+2,row.end(),row[i]);
                swap(*it,row[i+1]);
                cnt++;
            }
        }
        return cnt;
    }
```

- the complexity is O(N^2) since every time we need to search for the next
- O(N): create a index array to record each number's position and we exactly know each group's partner and we swap that. Then we do not need to use find. Be careful when we swap two nodes, the index array shall also be swapped.
```java
function minSwapsCouples(row) {
  const pos = {};
  for (let i = 0; i < row.length; i++) {
    pos[row[i]] = i;
  }

  let count = 0;
  for (let i = 1; i < row.length; i += 2) {
    while ((row[i]^1) !== row[i-1]) {
      let idx = pos[row[i]^1]^1;
      [row[i], row[idx]] = [row[idx], row[i]];
      count++;
    }
  }
  return count;
}
```

927. Three Equal Parts.md
### Problem Summary
Given an array A of 0s and 1s, divide the array into 3 non-empty parts such that all of these parts represent the same binary value.
return the index i and j. If cannot split, return -1,-1

### Analysis
This problem is clear and straightforward.

- leading zero does not contribute to the value
- so we only care about 1. the number of 1 and 1's position
- divide into 3 parts and check if the three parts are the same (with only a const index difference)
- the last part's trailing zero shall also present in the first two parts

### code
```cpp
    vector<int> threeEqualParts(vector<int>& A) {
      vector<int> ones;
        for(int i=0;i<A.size();i++) if(A[i]) ones.push_back(i);
        if(ones.size()%3) return vector<int>({-1,-1});
        if(ones.size()==0) return vector<int>({1,A.size()-1});
        int len=ones.size()/3;
        //check if the three parts has the same pattern
        for(int i=0;i<len;i++)
        {
            if(ones[i]-ones[0]!=ones[i+len]-ones[len] || ones[i]-ones[0]!=ones[i+2*len]-ones[2*len])
                return vector<int>({-1,-1});
        }
        //the pattern is the same, we need check number of zeros
        int nzeros3=A.size()-1-ones.back();
        int nzeros2=ones[2*len]-ones[2*len-1]-1;
        int nzeros1=ones[len]-ones[len-1]-1;
   
        if(nzeros1>=nzeros3 && nzeros2>=nzeros3)
        {
            return vector<int>({ones[len-1]+nzeros3,ones[2*len-1]+nzeros3+1});
        }
        return vector<int>({-1,-1});
    }
```    

936. Stamping The Sequence.md
### Problem Summary
Given a stamp string and a target string, return the stamping move sequence.
for example, stamp="abc", target="ababc", stamp at 0 and then stamp at 2

### approach
The key is to reverse the process: we first match the whole stamp which would be the last stamp sequence and change them to ***

```cpp
    vector<int> movesToStamp(string stamp, string target) {
        //reverse operation: matched then change it to ***
        //until we change the target string into *****
        //note we can only match one end if it is covered
        int n=target.length();
        string final(n,'*');
        vector<int> ans;
        while(target!=final)
        {
            int ind=match_change(target,stamp);
            if(ind==-1) return vector<int>();
            ans.push_back(ind);
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
    int match_change(string& target,string stamp)
    {
        //find the first matching and return
        //at least has one non * char inside, * matches any char
        bool matched=0;
        for(int i=0;i<target.size();i++)
        {
            int cnt_match=0;
            int j=0;
            for(j=0;j<stamp.size();j++)
            {
                if(target[i+j]=='*') continue;
                if(target[i+j]==stamp[j]) cnt_match++;
                else break;
            }
            if(j==stamp.size()&& cnt_match) 
            {
                for(j=0;j<stamp.size();j++) target[i+j]='*';
                return i;
            }
        }
        return -1; //no matching
    }
```

# Chapter 6 Math
### part 1-easy
942. DI String Match
return any permutation matching the DI string
one solution: We track high (h = N - 1) and low (l = 0) numbers within [0 ... N - 1]. When we encounter 'I', we insert the current low number and increase it. With 'D', we insert the current high number and decrease it. In the end, h == l, so we insert that last number to complete the premutation.
this is a greedy solution or two pointers

728. Self Dividing Numbers
A self-dividing number is a number that is divisible by every digit it contains.
just using brutal force

883. Projection Area of 3D Shapes
cube projection to xy, yz, xz plane and return the total area
```cpp
    int projectionArea(vector<vector<int>>& grid) {
        int xy=0,xz=0,yz=0;
        //xy we only care if it's z is 0 or not
        //xz we only care each y direction's max z: along row
        //yz we only care each x direction's max z: along column
        int n=grid.size();
        int total=0;
        vector<int> maxx(n,INT_MIN),maxy(n,INT_MIN);
        for(int i=0;i<n;i++) //row
        {
            for(int j=0;j<n;j++) //column
            {
                total+=(grid[i][j]>0); //xy plane
                maxx[i]=max(maxx[i],grid[i][j]); //xz: projection y
                maxy[j]=max(maxy[j],grid[i][j]); //yz: projection x
            }
            total+=maxx[i];
        }
        total+=accumulate(maxy.begin(),maxy.end(),0);
        return total;
    }
```    

908. Smallest Range I
Given an array A of integers, for each integer A[i] we may choose any x with -K <= x <= K, and add x to A[i].

After this process, we have some array B.

Return the smallest possible difference between the maximum value of B and the minimum value of B.
math: max and min, we can max pull down by 2K. if the difference>0, then the min difference is max-min-2K

868. Binary Gap
find the largest distance of 1 in its binary representation
straightforward

892. Surface Area of 3D Shapes
surface area of 3d shape
calculate the stacked cube surface area
minus its neighboring (i-1,j) and (i,j-1)
```cpp
    int surfaceArea(vector<vector<int>> grid) {
        int res = 0, n = grid.size();
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j]) res += grid[i][j] * 4 + 2;
                if (i) res -= min(grid[i][j], grid[i - 1][j]) * 2;
                if (j) res -= min(grid[i][j], grid[i][j - 1]) * 2;
            }
        }
        return res;
    }
```

812. Largest Triangle Area
the area of a triangle using 3 points: the area formula also works for other shape (polygon)
```cpp
    double largestTriangleArea(vector<vector<int>>& points) {
        int area=0,max_area=0;
        int m=points.size();
        for(int i=0;i<m;i++)
        {
            for(int j=i+1;j<m;j++)
            {
                for(int k=j+1;k<m;k++)
                {
                    area=points[i][0]*points[j][1]-points[j][0]*points[i][1];
                    area+=points[j][0]*points[k][1]-points[k][0]*points[j][1];
                    area+=points[k][0]*points[i][1]-points[i][0]*points[k][1];
                    if(max_area<abs(area)) max_area=abs(area);
                }    
            }
        }
        return 0.5*max_area;
    }
```    

258. Add Digits
Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.
do it in O(1)
The problem, widely known as digit root problem, has a congruence formula:

https://en.wikipedia.org/wiki/Digital_root#Congruence_formula
For base b (decimal case b = 10), the digit root of an integer is:

dr(n) = 0 if n == 0
dr(n) = (b-1) if n != 0 and n % (b-1) == 0
dr(n) = n mod (b-1) if n % (b-1) != 0
or

dr(n) = 1 + (n - 1) % 9

13. Roman to Integer
a hashmap of the letter to the number
if its right>left, then we subtract the left
else we add the left

171. Excel Sheet Column Number
note we shall add 1

453. Minimum Moves to Equal Array Elements
Given a non-empty integer array of size n, find the minimum number of moves required to make all array elements equal, where a move is incrementing n - 1 elements by 1.

sum + m * (n - 1) = x * n
x = minNum + m

598. Range Addition II
        //seems to find the most overlapped region
        //since it always covers the (0,0), we only need track the right bottom point
        //it is the min of all the x and min of all the y
        
268. Missing Number
0 to n, one is missing.
xor 1 to n and identical one goes to 0

628. Maximum Product of Three Numbers
sort the array. It may contains negatives.
sort the whole array which is O(nlogn)
just find the min 2 and max 3 which is O(n)
```java
    public int maximumProduct(int[] nums) {
        int max1 = Integer.MIN_VALUE, max2 = Integer.MIN_VALUE, max3 = Integer.MIN_VALUE, min1 = Integer.MAX_VALUE, min2 = Integer.MAX_VALUE;
        for (int n : nums) {
            if (n > max1) {
                max3 = max2;
                max2 = max1;
                max1 = n;
            } else if (n > max2) {
                max3 = max2;
                max2 = n;
            } else if (n > max3) {
                max3 = n;
            }

            if (n < min1) {
                min2 = min1;
                min1 = n;
            } else if (n < min2) {
                min2 = n;
            }
        }
        return Math.max(max1*max2*max3, max1*min1*min2);
    }
```

836. Rectangle Overlap
        //x1-x2 interval and x3-x4 interval shall overlap
        //y1-y2 interval and y3-y4 interval shall overlap
rec1[0]<rec2[2] && rec2[0]<rec1[2] && rec1[1]<rec2[3] && rec2[1]<rec1[3];

202. Happy Number
A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.
detect a cycle using hashset

415. Add Strings
reverse and add

231. Power of Two
n&(n-1)

326. Power of Three
3^k is factor of 3^max 

9. Palindrome Number
convert to string and reverse to see if equal

66. Plus One
MSB is at the front, so we add from the end

263. Ugly Number
only has 2,3,5 factor
recursive or iterative dividing 2,3,5 until is 1

645. Set Mismatch
1 to n, one number is changed to another number. find the missing number
The idea is based on:
(1 ^ 2 ^ 3 ^ .. ^ n) ^ (1 ^ 2 ^ 3 ^ .. ^ n) = 0
Suppose we change 'a' to 'b', then all but 'a' and 'b' are XORed exactly 2 times. The result is then
0 ^ a ^ b ^ b ^ b = a ^ b
Let c = a ^ b, if we can find 'b' which appears 2 times in the original array, 'a' can be easily calculated by a = c ^ b.

367. Valid Perfect Square
1. n=k*k
2. binary search
3. a square number=1+3+5+...

441. Arranging Coins
n coins to arrange it as 1,2,3,4....
m(m+1)/2<=n

67. Add Binary
reverse and add

172. Factorial Trailing Zeroes
n/2 2s n/5 5s so we need count number of 5s

914. X of a Kind in a Deck of Cards
count card number for each number and find the gcd>1
gcd for a list of number is the gcd of the gcd with the numnber

507. Perfect Number
We define the Perfect Number is a positive integer that is equal to the sum of all its positive divisors except itself.
brutal force: up to sqrt(n)
be sure if i==num/i and it counts twice

633. Sum of Square Numbers
check if a number is sum of two squares
a^2+b^2=c, we just search one from 1 to sqrt(c/2) for a and once a is fixed we can find b

949. Largest Time for Given Digits
need to consider the time as a whole. just use all the permutation and get the largest valid time

754. Reach a Number
You are standing at position 0 on an infinite number line. There is a goal at position target.

On each move, you can either go left or right. During the n-th move (starting from 1), you take n steps.

Return the minimum number of steps required to reach the destination.
very similar to the dp problem of racing car

Step 0: Get positive target value (step to get negative target is the same as to get positive value due to symmetry).
Step 1: Find the smallest step that the summation from 1 to step just exceeds or equalstarget.
Step 2: Find the difference between sum and target. The goal is to get rid of the difference to reach target. For ith move, if we switch the right move to the left, the change in summation will be 2*i less. Now the difference between sum and target has to be an even number in order for the math to check out.
Step 2.1: If the difference value is even, we can return the current step.
Step 2.2: If the difference value is odd, we need to increase the step until the difference is even (at most 2 more steps needed).
Eg:
target = 5
Step 0: target = 5.
Step 1: sum = 1 + 2 + 3 = 6 > 5, step = 3.
Step 2: Difference = 6 - 5 = 1. Since the difference is an odd value, we will not reach the target by switching any right move to the left. So we increase our step.
Step 2.2: We need to increase step by 2 to get an even difference (i.e. 1 + 2 + 3 + 4 + 5 = 15, now step = 5, difference = 15 - 5 = 10). Now that we have an even difference, we can simply switch any move to the left (i.e. change + to -) as long as the summation of the changed value equals to half of the difference. We can switch 1 and 4 or 2 and 3 or 5.
```java
    public int reachNumber(int target) {
        target = Math.abs(target);
        int step = 0;
        int sum = 0;
        while (sum < target) {
            step++;
            sum += step;
        }
        while ((sum - target) % 2 != 0) {
            step++;
            sum += step;
        }
        return step;
    }
```

69. Sqrt(x)
binary search

400. Nth Digit
find the nth digit for 1,2,3,....
one digit: 1-9: 9
two digit: 10-99: 90
three digits: 100-999: 900
accumulate these numbers and using binary search to find the number with nth digit

168. Excel Sheet Column Title
pay attention to starting index 0

204. Count Primes
cross out all even numbers
cross out all multiples
this is a typical algorithm
```cpp
    int countPrimes(int n) {
        //matlab implementation on primes
        if(n<3) return 0;
        vector<bool> primes(n/2,1); //only store odd numbers, the number=2*k+1
        //note primes[0]=2 instead of 1
        for(int i=3;i<=sqrt(n);i+=2) //only odd factors
        {
            if(primes[(i-1)/2]) 
                for(int j=i*i;j<n;j+=2*i) {primes[(j-1)/2]=0;}
        }
        return accumulate(primes.begin(),primes.end(),0);
    }
```
7. Reverse Integer
using string, note there is a sign

### medium


535. Encode and Decode TinyURL
using 0-9 a-z A-Z to form a tinyURL
tinyURL includes 6 chars from above

using hashmap: 
tiny url vs real url
id vs tinyURL
everytime we increase the id when a new url comes

537. Complex Number Multiplication
read the real and imag and perform multiplication 
read using stringstream

885. Spiral Matrix III
starting from (r0,c0), walking clockwise
change direction and change step
```cpp
    vector<vector<int>> spiralMatrixIII(int R, int C, int r, int c) {
        vector<vector<int>> res = {{r, c}};
        int x = 0, y = 1, tmp;
        for (int n = 0; res.size() < R * C; n++) {
            for (int i = 0; i < n / 2 + 1; i++) {
                r += x, c += y;
                if (0 <= r && r < R && 0 <= c && c < C)
                    res.push_back({r, c});
            }
            tmp = x, x = y, y = -tmp;
        }
        return res;
    }
```

877. Stone Game
even number of piles, total sum is odd. taking from either end.
who will win
can use dp
sum(piles(even)) < sum(piles[odd]), pick all odd
sum(piles(even)) > sum(piles[odd]), pick all even
always win

553. Optimal Division
perform float division. by adding parenthesis to reach max result
X1/X2/X3/../Xn will always be equal to (X1/X2) * Y, no matter how you place parentheses. i.e no matter how you place parentheses, X1 always goes to the numerator and X2 always goes to the denominator. Hence you just need to maximize Y. And Y is maximized when it is equal to X3 *..*Xn. So the answer is always X1/(X2/X3/../Xn) = (X1 *X3 *..*Xn)/X2

413. Arithmetic Slices
see dp

789. Escape The Ghosts
you and ghost can move 4 directions and move simultaneously. see if you can escape and reach the target position
Let's say you always take the shortest route to reach the target because if you go a longer or a more tortuous route, I believe the ghost has a better chance of getting you.
Denote your starting point A, ghost's starting point B, the target point C.
For a ghost to intercept you, there has to be some point D on AC such that AD = DB. Fix D. By triangle inequality, AC = AD + DC = DB + DC >= BC. What that means is if the ghost can intercept you in the middle, it can actually reach the target at least as early as you do. So wherever the ghost starts at (and wherever the interception point is), its best chance of getting you is going directly to the target and waiting there rather than intercepting you in the middle.

462. Minimum Moves to Equal Array Elements II
a move is to add +/-1 to an element
min moves to make the elements equal.
choose the median and make every number equal to median
Suppose you have two endpoints A and B, when you want to find a point C that has minimum sum of distance between AC and BC, the point C will always between A and B. Draw a graph and you will understand it. Lets keep moving forward. After we locating the point C between A and B, we can define that
dis(AC) = c - a; dis(CB) = b - c;
sum = dis(AC) + dis(CB) = b - a.
b - a will be a constant value, given specific b and a. Thus there will be no difference between points among A and B.

In this problem, we set two boundaries, saying i and j, and we move the i and j to do the computation.

```java
    public int minMoves2(int[] nums) {
        Arrays.sort(nums);
        int i = 0, j = nums.length-1;
        int count = 0;
        while(i < j){
            count += nums[j]-nums[i];
            i++;
            j--;
        }
        return count;
    }
```

858. Mirror Reflection
reflection by expanding the rect in 2 directions and problem becomes a simple gcd problem

781. Rabbits in Forest
In a forest, each rabbit has some color. Some subset of rabbits (possibly all of them) tell you how many other rabbits have the same color as them. Those answers are placed in an array.

Return the minimum number of rabbits that could be in the forest.
For rabits with same colors, the answers must be the same. However, when the total amount of that same answer exceeds  'that answer + 1', there must be a new color. (say [3,3,3,3,3], the first four 3s indicates 4 rabits with the same color. The 5th 3 must represent a new color that contains 4 other rabits). We only calculate the amount of rabits with the same color once. Hashmap is used to record the frequency of the same answers. Once it exceeds the range, we clear the frequency and calculate again.

If x+1 rabbits have same color, then we get x+1 rabbits who all answer x.
now n rabbits answer x.
If n % (x + 1) == 0, we need n / (x + 1) groups of x + 1 rabbits.
If n % (x + 1) != 0, we need n / (x + 1) + 1 groups of x + 1 rabbits.

the number of groups is math.ceil(n / (x + 1)) and it equals to (n + x) / (x + 1) , which is more elegant.

672. Bulb Switcher II
flips all lights
flips all odd lights
flip all even lights
flip all 3k+1 lights
here are three important observations:

For any operation, only odd or even matters, i.e. 0 or 1. Two same operations equal no operation.
The first 3 operations can be reduced to 1 or 0 operation. For example, flip all + flip even = flip odd. So the result of the first 3 operations is the same as either 1 operation or original.
The solution for n > 3 is the same as n = 3.
For example, 1 0 0 ....., I use 0 and 1 to represent off and on.
The state of 2nd digit indicates even flip; The state of 3rd digit indicates odd flip; And the state difference of 1st and 3rd digits indicates 3k+1 flip.
In summary, the question can be simplified as m <= 3, n <= 3. I am sure you can figure out the rest easily.

12. Integer to Roman
```cpp
    string intToRoman(int num) {
        string s;
        char digit[4]={0}; //store the digits in decimal
        char roman[]={'I','V','X','L','C','D','M'};
        int val[]={1,5,10,50,100,500,1000};

        //start from the msb digit
        int start_indx=6; //1000
        for(int i=1000;i>=1;i/=10,start_indx-=2)
        {
            int digit=num/i;
            if(!digit) continue;
            if(digit<4) s.append(digit,roman[start_indx]);
            else if(digit<5) s=s+roman[start_indx]+roman[start_indx+1];
            else if(digit<9) {s+=roman[start_indx+1];s.append(digit-5,roman[start_indx]);}
            else s=s+roman[start_indx]+roman[start_indx+2];
            num-=digit*i;
        }
        return s;
    }
```

869. Reordered Power of 2
store the digits in sorted order string
and then we iterate all 2^n to get same string
check if the two string equal

343. Integer Break
see dp

357. Count Numbers with Unique Digits
from 0 to 10^n get the number of unique digits
9*9*8*7*6...

592. Fraction Addition and Subtraction
need to find the lcm for all demom
final results need find the gcd

423. Reconstruct Original Digits from English
first count those unique letters in each number
after they decided, minus it from other

319. Bulb Switcher
n bulbs from 1 to n, from 2 to n toggle all its multiples. Number of lights left.
A bulb ends up on iff it is switched an odd number of times.

Call them bulb 1 to bulb n. Bulb i is switched in round d if and only if d divides i. So bulb i ends up on if and only if it has an odd number of divisors.

Divisors come in pairs, like i=12 has divisors 1 and 12, 2 and 6, and 3 and 4. Except when i is a square, like 36 has divisors 1 and 36, 2 and 18, 3 and 12, 4 and 9, and double divisor 6. So bulb i ends up on if and only if i is a square.

So just count the square numbers.

313. Super Ugly Number
just replace k pointers using dp

593. Valid Square
4 points if form a square
C(4,2) to calculate the side length, 4 same and 2 same

using hashset to count only two

279. Perfect Squares
see dp for knapsack

640. Solve the Equation
find the = and move all x items to left and non-x items to right
ax=b

670. Maximum Swap
a greedy choice
find the right max and swap with the first smaller.

963. Minimum Area Rectangle II
any four points
using the side as diagnonal and use the center point and the side length as the key to form a hashmap
these points are all on a circle.

775. Global and Local Inversions
if global==local
only local inversion exists. so the i-A[i] difference cannot be more than 1

223. Rectangle Area
two rect overlap
```cpp
     int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        int left=max(A,E);
        int right=min(C,G);
        int top=max(B,F);
        int bott=min(D,H);
        int w=right>left?right-left:0; //extreme case right-left will be wrong!
        int h=bott>top?bott-top:0;
        int area=w*h;
        if(area<0) area=0;
        int tarea=(C-A)*(D-B)+(G-E)*(H-F);
        return tarea-area;
    }
```

372. Super Pow
Your task is to calculate a^b mod 1337 where a is a positive integer and b is an extremely large positive integer given in the form of an array.
```cpp
/*One knowledge: ab % k = (a%k)(b%k)%k
Since the power here is an array, we'd better handle it digit by digit.
One observation:
a^1234567 % k = (a^1234560 % k) * (a^7 % k) % k = (a^123456 % k)^10 % k * (a^7 % k) % k
Looks complicated? Let me put it other way:
Suppose f(a, b) calculates a^b % k; Then translate above formula to using f :
f(a,1234567) = f(a, 1234560) * f(a, 7) % k = f(f(a, 123456),10) * f(a,7)%k;
Implementation of this idea: divide and conque
*/
class Solution {
    const int base = 1337;
    int powmod(int a, int k) //a^k mod 1337 where 0 <= k <= 10
    {
        a %= base;
        int result = 1;
        for (int i = 0; i < k; ++i)
            result = (result * a) % base;
        return result;
    }
public:
    int superPow(int a, vector<int>& b) {
        if (b.empty()) return 1;
        int last_digit = b.back();
        b.pop_back();
        return powmod(superPow(a, b), 10) * powmod(a, last_digit) % base;
    }
};
```

264. Ugly Number II
see dp

396. Rotate Function
```cpp
    int maxRotateFunction(vector<int>& A) {
        //this is the inner product of two vectors
        //derived from the recursive relation F(k)-F(k-1)=Sum(A)-nA(n-k)
        int sum=0,allsum=0;
        int n=A.size();
        for(int i=0;i<n;i++) {sum+=A[i];allsum+=i*A[i];}//F(0) and sum(A)
        int maxsum=allsum;
        for(int i=1;i<n;i++)
        {
            allsum+=sum-n*A[n-i];
            if(allsum>maxsum) maxsum=allsum;
        }
        return maxsum;
    }
```

368. Largest Divisible Subset
subset numbers all satisfy Si%Sj=0
dp with backtrace

60. Permutation Sequence
given number 1 to n, find its kth permutation
for n elements, it has n! permutations
we have n on the first, n-1 on the second, so it is reduced to n*sub(n-1)
we can locate the position for n, n-1, n-2....for each digit

397. Integer Replacement
if n is even n/=2
if n is odd n++ or n--
repeat until n=1, return the min step

binary 01: we subtract 1->00
binary 11: we add 1->00
so we can reduce by half sooner

2. Add Two Numbers
linked list

43. Multiply Strings
naive approach using hand multiplication

794. Valid Tic-Tac-Toe State
Players take turns placing characters into empty squares (" ").
The first player always places "X" characters, while the second player always places "O" characters.
"X" and "O" characters are always placed into empty squares, never filled ones.
The game ends when there are 3 of the same (non-empty) character filling any row, column, or diagonal.
The game also ends if all squares are non-empty.
No more moves can be played if the game is over.

so
When turns is 1, X moved. When turns is 0, O moved. rows stores the number of X or O in each row. cols stores the number of X or O in each column. diag stores the number of X or O in diagonal. antidiag stores the number of X or O in antidiagonal. When any of the value gets to 3, it means X wins. When any of the value gets to -3, it means O wins.

When X wins, O cannot move anymore, so turns must be 1. When O wins, X cannot move anymore, so turns must be 0. Finally, when we return, turns must be either 0 or 1, and X and O cannot win at same time.

365. Water and Jug Problem.
jug x and y, to measure z. we can only use x and y and x+y or x-y

```cpp
    bool canMeasureWater(int x, int y, int z) {
        //number theory: coprime: can get any number
        //has a gcd, then it can only get any number of N*gcd
        //for example 5,3: 5-3=2, 5-(3-2)=1
        if(z==x+y || z==x || z==y) return 1;
        if(!x ||!y) return 0;
        int g=gcd(x,y);
        return z%g==0 && z<=x+y;
    }
    int gcd(int a,int b)
    {
        //using euclid's theory
        if(!b) return a;
        return gcd(b,a%b);
    }
```

50. Pow(x, n)
divide and conque, x^n=x^2*x^(n/2)

523. Continuous Subarray Sum
target sum multiple of k
prefix sum and put the %k the same value together

910. Smallest Range II
add +k or -k
sort it first and then greedy

166. Fraction to Recurring Decimal
need to find the repeat pattern

866. Prime Palindrome
find the smallest prime palindrome number >=N
All palindrome with even digits is multiple of 11.
So among them, 11 is the only one prime
if (8 <= N <= 11) return 11
For other, we consider only palindrome with odd dights.

reduce the number digits by half.

29. Divide Two Integers
no division
use subtraction. to speed up the subtraction we can double the divisor

8. String to Integer (atoi)
```cpp
    int myAtoi(string str) {
      long long res=0; //shall have a large container
	  int sign=1;
      bool found_sign=0;
      bool found_digit=0;
	  for(int i=0;i<str.size();i++)
	  {
		  char c=str[i];
		  if(c=='-') {if(!found_sign) {sign=-1;found_sign=1;}else break;}
		  else if(c=='+' ) {if(!found_sign) {sign=1;found_sign=1;}else break;}
		  else if(c>='0' && c<='9') {found_digit=1;res=res*10+c-'0';if(res>INT_MAX) break;}
		  else if(c==' ' || c=='\t') if(!found_digit && !found_sign) continue;else break;
		  else break;
	  }
	  //overflow error dealing
	  //if(sign*res>INT_MAX || sign*res<INT_MIN) return 0;
	  if(res<0) if(sign>0) return INT_MAX;else return INT_MIN;
	  if(sign*res>INT_MAX) return INT_MAX;
	  if(sign*res<INT_MIN) return INT_MIN;
	  return sign*res;
    }
```


899. Orderly Queue
in each move we move one of the char in the first k char to the end
return the smallest string

Remind what bubble sort eventually is: swap pairs

So, you have a buffer of at least 2 when K>1
you can put them back into the queue in different order: swap!

So, K>1 equals bubble sort

```cpp
    string orderlyQueue(string S, int K) {
        if (K>1){
            sort(S.begin(),S.end());
            return S;
        }
        string minS=S;
        for (int i=0;i<S.size();++i){
            S=S.substr(1)+S.substr(0,1);
            minS=min(S,minS);
        }
        return minS;
    }
```

753. Cracking the Safe
return the string to guarantee to open the box (keeping matching the password)
password is n digit using 0 to k-1 digits

need to cover all permutations. and try to maximize the overlap between the permutations.
we can reuse the last n-1 digits and add a new digit at the end. If we find a path reaching all nodes, that is the shortest string.
The total combination is k^n
dfs
```cpp
string crackSafe(int n, int k) {
        string ans = string(n, '0');
        unordered_set<string> visited;
        visited.insert(ans);
        
        for(int i = 0;i<pow(k,n);i++){
            string prev = ans.substr(ans.size()-n+1,n-1);
            for(int j = k-1;j>=0;j--){
                string now = prev + to_string(j);
                if(!visited.count(now)){
                    visited.insert(now);
                    ans += to_string(j);
                    break;
                }
            }
        }
        return ans;
    }
```
The solution prunes early using backward iteration on k. 
when we start from forward, there will be a circle preventing us going on.
???

810. Chalkboard XOR Game


# Chapter 7 Divide and conquer
Divide and conquer: divide the problem into several smaller sized similar problems and then assemble the results to get the final results.
Divide and conquer generally needs to consider two parts separately plus case where overlap the two cases. It generally make things much more complicated but will generally improve the efficiency from O(N) to O(logN)

169. Majority Element

find the element over [n/2] times.

- algorithm 1: sort and get the mid value.
- algorithm 2: voting: O(N) iterate over the array and remove different pairs, using count, when the same ++, otherwise --
- algorithm 3: divide and conquer, divide by half and get the majority element from left and right, choose the count larger. base case: the array only has one number.
- algorithm 4: hash table

53. Maximum Subarray

find the largest sum of the subarray
- dp: dp[i] is the max sum ending at i, then dp[i]=max(dp[i-1]+a[i],a[i])
- divide and conquer: 
the idea is accumulate to get the prefix sum and the max subarray sum is then the largest difference of the prefix sum.
for each subarray v1 and v2, we define the following:
  - l: the sum of the sub array with largest sum starting from the first element
  - m: the sum of the sub array with largest sum
  - r: the sum of the sub array with largest sum ending at the last element
  - s: the sum of the whole array
  then:
  l=max(v1.l,v1.s+v2.l) //appending the left to right
  m=max(v1.l+v2.r,max(v1.m,v2.m)) //subarray cross the v1 and v2, or in v1 or v2
  r=max(v2.r,v1.r+v2.s) //reverse direction
  s=v1.s+v2.s;
  base case: array of size=1.
  
932. Beautiful Array
a permutation of 1 to N, and there is no a[k]*2=a[i]+a[j] i<k<j
when we have a[k]*2!=a[i]+a[j]
-. we can add any number to it and get a new beautiful array
-. we can multiply a number to it and get a new beautiful array
-. a[i] is odd, a[j] is even, then a[k]*2 will satisfy the beatiful array.
instead of divide and conquer we use the reverse way to grow the array.
Assume we have a beautiful array:
A1=2*A-1 is also a beautiful array
A2=2*A is also a beautiful array
starting from a smallest beautiful array {1}.
```cpp
    vector<int> beautifulArray(int N) {
        vector<int> res = {1};
        while (res.size() < N) {
            vector<int> tmp;
            for (int i : res) if (i * 2 - 1 <= N) tmp.push_back(i * 2 - 1);
            for (int i : res) if (i * 2 <= N) tmp.push_back(i * 2);
            res = tmp;
        }
        return res;
    }
```

241. Different Ways to Add Parentheses
a expression with numbers, + - *, by applying parentheses, we can get different results.
return all possible results.
The operators are the natural position for splitting the expression into left and right part and then we combine the left and right results.
```cpp
    unordered_map<string,vector<int>> memo;
    vector<int> diffWaysToCompute(string input) {
        if(input.size()==0) return vector<int>();
        vector<int> res;
        if(memo.count(input)) return memo[input];
        //divide the string into two parts and do the calculations
        for(int i=0;i<input.size();i++)
        {
            if(input[i]!='+' && input[i]!='-' && input[i]!='*') continue;

            vector<int> res1=diffWaysToCompute(input.substr(0,i)); 
            vector<int> res2=diffWaysToCompute(input.substr(i+1));
            //perform the operations between res1 and res2 using +-*
            for(int k=0;k<res1.size();k++)
            {
                for(int j=0;j<res2.size();j++)
                {
                    if(input[i]=='+') res.push_back(res1[k]+res2[j]);
                    else if(input[i]=='-') res.push_back(res1[k]-res2[j]);
                    else if(input[i]=='*') res.push_back(res1[k]*res2[j]);
                }
            }
            
        }
        if(res.empty()) res=vector<int>(1,stoi(input));
        memo[input]=res;
        return res; //when it has no results return the numbers
    }
```
- when there is no operator in the expression, we need return the number
- left and right combination produce l*r number of different results
- possibly get duplicate results but it is ok since we need different parentheses.

215. Kth Largest Element in an Array

return the kth largest element in its sorted order.
- naive: sort and then get the k-th largest element. sorting/multiset/maxheap/priority-queue O(nlogn)
- improve: only store k elements in maxheap or priority-queue, so sorting is more efficient O(nlogK)
- parital sorting: using c++ nth_element
```cpp
    int findKthLargest(vector<int>& nums, int k) {
        nth_element(nums.begin(), nums.begin() + k - 1, nums.end(), greater<int>());
        return nums[k - 1];
    }
```
- divide and conquer using the same idea as qsort, randomly pick the pivot and separate the array into two parts.
Quicksort
In quicksort, in each iteration, we need to select a pivot and then partition the array into roughly two halves:

  - Elements larger than or equal to the pivot;
  - Elements smaller than or equal to the pivot.
First let's do an example with the array [3, 2, 1, 5, 4, 6] in the problem statement. Let's assume in each time we select the leftmost element to be the pivot, in this case, 3. We then use it to partition the array, which results in [5, 6, 4, 3, 1, 2]. Now 3 is in the 3rd (0-based) position and we know that it is the 4th (1-based) largest element. So, have you noticed how partition is related to this problem?

In the above example, now we know that 3 is the 4th largest element. If we are asked to find the 2nd largest element, it should be to the left of 3. If we are asked to find the 5th largest element, it should be to the right of 3. So, in the average sense, the problem is reduced to approximately half of its original size, giving the recursion T(n) = T(n/2) + O(n) in which O(n) is the time for partition. This recursion, once solved, gives T(n) = O(n) and thus we have a linear time solution. Note that since we only need to consider one half of the array, the time complexity is O(n). If we need to consider both the two halves of the array, like quicksort, then the recursion will be T(n) = 2T(n/2) + O(n) and the complexity will be O(nlogn).

Of course, O(n) is the average/best time complexity. In the worst case, the recursion may become T(n) = T(n - 1) + O(n) and the complexity will be O(n^2).

The algorithm is as follows.

  - Initialize left to be 0 and right to be nums.size() - 1;
  - Partition the array, if the pivot is at the k-1-th position, return it (we are done);
  - If the pivot is right to the k-1-th position, update right to be the left neighbor of the pivot;
  - Else update left to be the right neighbor of the pivot.
  - Repeat 2.
  
 The complexity analysis of the divide and conquer is valuable.
 
 240. Search a 2D Matrix II
 each row sorted, each column sorted
 Thinking the matrix a tree with root at the left bottom corner. Then we know how to go.
 
 312. Burst Balloons
 see dp, it also relates with divide and conquer
 
 903. Valid Permutations for DI Sequence
 D: decrease, I: increase
 using 0 to N for the n-len string. Get the number of permutation.
 dp[i,j]: i the length, j: ending number of j
 see dp
 
 514. Freedom Trail
 see dp
 
 315. Count of Smaller Numbers After Self
 convert the array into a BST with duplicate numbers, and each node also contains the sum of its child
 ```cpp
 struct Node{
    Node *left,*right;
    int dup,sum_left,val;
    Node(int v,int s):val(v),sum_left(s),dup(1){left=right=0;}
};
class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        vector<int> result(nums.size());
        Node* root=0;
        for(int i=nums.size()-1;i>=0;i--)
        {
            root=insert_tree(nums[i],root,result,i,0);
        }
        return result;
    }
    
    Node* insert_tree(int num,Node*root,vector<int>& result,int ind,int presum)
    {
        if(!root) {root=new Node(num,0);result[ind]=presum;}
        else if(num==root->val) {root->dup++;result[ind]=presum+root->sum_left;}
        else if(num<root->val) {root->sum_left++;root->left=insert_tree(num,root->left,result,ind,presum);}
        else root->right=insert_tree(num,root->right,result,ind,presum+root->sum_left+root->dup);
        return root;
    }
};
```

23. Merge k Sorted Lists
using a minheap or a priority-queue

282. Expression Add Operators
given a string of digits from 0 to 9. add + - * to reach the target.
return all possibility of combinations

dfs: try +-* at each position and split into two half and evaluate the results

327. Count of Range Sum 
 Lower and upper bound, count the number of range sums in this range.
 - naive: prefix sum and check each difference O(N^2)
 - divide and conquer: divide the array at m, then we have three cases
 1. range sum in left array
 2. range sum in right array
 3. range sum with first index in left array and 2nd index in right array
 1 and 2 are sub-problem and shall be solved using recursion. 
 how to approach 3?
 - we can do prefix sum for right and postfix sum for left and add together we get the subarray sum. Again this is O(N^2)
 - an important fact of problem 3: the index i<j is automatically guaranteed and we do not need to keep the ordering of left and right array. We can sort (merge sort) to find ->O(nlogn)
j is the first index satisfy sums[j] - sums[i] > upper and
k is the first index satisfy sums[k] - sums[i] >= lower.
Then the number of sums in [lower, upper] is j-k

218. The Skyline Problem
given a list of buildings: (x1,x2, y): means from x1 to x2 it is height y.
return the outer skyline
naive: check each building and updates the height (keep the max). when the x-axis is long, this could be a lot.
improvement: we can just record the turning point value
https://briangordon.github.io/2014/08/the-skyline-problem.html
that we’re able to scan through the critical points and consider only the “active” set of rectangles at each critical point, an interesting opportunity presents itself. Our current solution can be written as:

for each critical point c  c.y gets the height of the tallest rectangle over c
This is no longer obviously O(n2). If we can somehow calculate the height of the tallest rectangle over c in faster than O(n) time, we have beaten our O(n2) algorithm. Fortunately, we know about a data structure which can keep track of an active set of integer-keyed objects and return the highest one in O(logn) time: a heap.

Our final solution, then, in O(nlogn) time, is as follows. First, sort the critical points. Then scan across the critical points from left to right. When we encounter the left edge of a rectangle, we add that rectangle to the heap with its height as the key. When we encounter the right edge of a rectangle, we remove that rectangle from the heap. (This requires keeping external pointers into the heap.) Finally, any time we encounter a critical point, after updating the heap we set the height of that critical point to the value peeked from the top of the heap.

# Chapter 8. Linked List
876. Middle of the Linked List
traverse twice

206. Reverse Linked List
curr, prev, next to reverse the list
```cpp
    ListNode* reverseList(ListNode* head) {
        ListNode *curr,*prev;
        curr=head;
        prev=0;
        while(curr) //current node is not null
        {
            ListNode *next=curr->next;
            curr->next=prev; //reverse it
            prev=curr;
            curr=next;
        }
        return prev;
    }
```

237. Delete Node in a Linked List
replace current node with its next

21. Merge Two Sorted Lists
just trick to manage the next pointer

83. Remove Duplicates from Sorted List
remove a node
using delete must generated using new

203. Remove Linked List Elements
remove the head first if necessary and keep the new head
note for freeing the nodes

141. Linked List Cycle
classical. If there is a cycle, using a fast and a slow pointer to traverse the list and they will meet.

234. Palindrome Linked List
O(1) space and O(n) complexity
using a fast and slow pointer to get the mid, and then reverse the second half
then use two pointer to traverse each half list.

160. Intersection of Two Linked Lists
pointer 1 traverse A and then B
pointer 2 traverse B and then A
the two pointers meet at the intersection

707. Design Linked List
support function
get
addHead
AddTail
delHead
delTail
addIndex
DelIndex

we can keep the head and tail and size for convenience

817. Linked List Components
G is a subset array of linked list L, if connected in L then it is considered as a group.
Return the number of groups in G
the order in G does not matter. so we use hash table.
```cpp
    int numComponents(ListNode* head, vector<int>& G) {
        unordered_set<int> myset(G.begin(),G.end());
        int grp=0,cnt=0;
        while(head)
        {
            if(myset.count(head->val)) cnt++;
            else {if(cnt) {grp++;cnt=0;}}
            head=head->next;
        }
        if(cnt) grp++;
        return grp;
    }
```

445. Add Two Numbers II
reverse the both lists so they are aligned on the LSB.
```cpp
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *r1,*r2;
        r1=reverseList(l1);
        r2=reverseList(l2);
        //print(r1);
        //print(r2);
        ListNode *newh=0;
        int i=0,j=0;
        int cr=0;//carrier
        ListNode *p1=r1,*p2=r2,*p=newh,*prev=0;
        while(p1 || p2)
        {
            int t=(p1?p1->val:0)+(p2?p2->val:0)+cr;
            cr=t/10;
            t-=cr*10;
            if(!p) p=newh=new ListNode(t);
            else p=new ListNode(t);
            if(prev) prev->next=p;
            prev=p;
            if(p1) {p1=p1->next;}
            if(p2) {p2=p2->next;}
        }
        if(cr) {p=new ListNode(cr);prev->next=p;}
        
        newh=reverseList(newh);
        return newh;
    }
    ListNode* reverseList(ListNode* root)
    {
        ListNode* p=root,*prev=0,*next;
        while(p)
        {
            next=p->next; //save its next
            p->next=prev; //reverse its next
            prev=p;//current node becomes prev
            p=next; //go to next node
        }
        return prev;//p will become empty
    }
```

725. Split Linked List in Parts
given k, split the array into k parts with consecuative parts difference <=1 and previous part >= later parts
get the number of nodes and divide by k and the mod assigned to first several parts
using a 2d loop 

328. Odd Even Linked List
use two pointer to alternate, odd even odd even, odd's next is even, even's next is odd
```cpp
    ListNode* oddEvenList(ListNode* head) {
        if(!head) return 0;
        ListNode* even_head=head->next; //saved for the first even node
        ListNode* odd_node=head;
        ListNode* even_node=even_head;
        //involves two nodes, odd, even and separate them into two lists
        while(odd_node->next && even_node->next) //current node and next node is not empty
        {
            //ListNode* temp=odd_node->next;
            odd_node->next=even_node->next;
            odd_node=odd_node->next;
            if(odd_node)
            {
                even_node->next=odd_node->next; //in current loop it is not able to determine event_node's next
                even_node=even_node->next;
            }
            
        }
        //connect two lists
        odd_node->next=even_head;
        return head;
        
    }
```

24. Swap Nodes in Pairs
const space, must swap nodes instead of values
```cpp
    ListNode* swapPairs(ListNode* head) {
        ListNode *ptr=head;
        ListNode *curr=head,*next,*nextnext,*prev=0;
        if(head && head->next) head=head->next;
        while(ptr && ptr->next)
        {
            //swap ptr and ptr->next
            curr=ptr;
            next=ptr->next;
            nextnext=ptr->next->next;

            curr->next->next=curr;
			curr->next=nextnext;
			curr=next; //note the second time pair switch is incorrect!!! the previous node's next shall be modified
            if(prev) prev->next=curr;
            
            //advance to next next node
			prev=curr->next;
            ptr=nextnext; //this invalidates above assignment!
        }
        return head;
    }
```

109. Convert Sorted List to Binary Search Tree
balance tree
traverse and store the value and recursively build the tree (divide by half)
```cpp
    TreeNode* sortedListToBST(ListNode* head) {
        vector<int> nums;
        while(head){nums.push_back(head->val);head=head->next;}
        int n=nums.size();
        if(!n) return 0;
        int left=0,right=n-1;
        int mid=(left+right+1)/2;
        //each root shall be the mid value of the current section
        TreeNode* root=new TreeNode(nums[mid]);
        insert(root,nums,0,mid-1);
        insert(root,nums,mid+1,right);
        return root;
    }
    void insert(TreeNode* root,vector<int>& nums,int l,int r)
    {
        if(r<l) return;//exit condition
        int mid=(l+r+1)/2;
        int val=nums[mid];
        TreeNode *node=new TreeNode(val);
        if(val<=root->val) 
        {
            root->left=node;
            insert(root->left,nums,l,mid-1);
            insert(root->left,nums,mid+1,r);
        }
        else 
        {
            root->right=node;
            insert(root->right,nums,l,mid-1);
            insert(root->right,nums,mid+1,r);
            
        }
    }
```

430. Flatten a Multilevel Doubly Linked List
if it has a child, flatten it
```cpp
    Node* flatten(Node* head)
    {
        Node* last=0;
        return helper(head,last);
    }
    Node* helper(Node* head,Node*& last) {
        if(!head) return 0;
        Node* node=head;
        while(node)
        {
            //cout<<node->val<<endl;
            if(node->child)
            {
                Node* next=node->next;//save the next
                node->next=helper(node->child,last);
                node->next->prev=node;
                node->child=0;
                last->next=next;
                if(next) next->prev=last;
                node=last;
            }
            if(!node->next) last=node;
            node=node->next;
            
        }
        return head;
    }
```
we get the next and last pointer of the child and then connect

147. Insertion Sort List
insertion sort using const space. cannot use multiset
Keep a sorted partial list (head) and start from the second node (head -> next), each time when we see a node with val smaller than its previous node, we scan from the head and find the position that the node should be inserted. Since a node may be inserted before head, we create a dummy head that points to head.

86. Partition List
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.
You should preserve the original relative order of the nodes in each of the two partitions.
use one list to get the smaller than x and one list to get the >=x and then connect the two lists

19. Remove Nth Node From End of List
using two pointers, one pointer advance n first and then other pointer advance the same time, when the first pointer reaches the end, the other pointer reaches the nth node from the end

92. Reverse Linked List II
Reverse a linked list from position m to n. Do it in one-pass.
Note: 1 ≤ m ≤ n ≤ length of list.
traverse to mth node, and then reverse n and connect the first and last.

```cpp
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        //first traverse and then reverse the one and then connect the reversed 
        int cnt=1;
        ListNode *curr=head,*prev=0,*next=0;
        while(cnt<m) {prev=curr,curr=curr->next;cnt++;}
        ListNode* nhead=reversen(curr,n-m+1,next);
        if(prev) prev->next=nhead;
        curr->next=next;
        return prev?head:nhead;
    }
    //need return the new head and the next node
    ListNode* reversen(ListNode* head,int n,ListNode*& next)
    {
        int cnt=0;
        ListNode *curr,*prev;
        prev=0;curr=head;
        while(cnt<n)
        {
            //cout<<curr->val<<endl;
            next=curr->next;
            curr->next=prev;
            prev=curr;
            curr=next;
            cnt++;
            
        }
        return prev;
    }
```    

148. Sort List
Sort a linked list in O(n log n) time using constant space complexity
split in half and solve the two smaller subproblem and then merge sort.
```cpp
    ListNode* sortList(ListNode* head) {
        //split list into a single node and then use merge sort
        //stop condition!
        if(!head || !head->next) return head;
        int len=0;
        ListNode* p=head;
        while(p) {len++;p=p->next;}
        ListNode* right=split(head,len/2);
        head=sortList(head);
        right=sortList(right);
        head=mergeSort(head,right);
        return head;
    }
    ListNode* split(ListNode* head,int split_len)
    {
        if(!split_len) return head;
        ListNode* p=head;
        int cnt=0;
        while(cnt<split_len-1) {p=p->next;cnt++;}
        ListNode* newhead=p->next;
        p->next=0;
        return newhead;
    }
    ListNode* mergeSort(ListNode* l1,ListNode* l2)
    {
        ListNode *p1=l1,*p2=l2,*newhead=0,*prev=0;
        while(p1 && p2)
        {
            if(p1->val<p2->val) 
            {
                if(!newhead) newhead=p1;
                if(prev) prev->next=p1;
                prev=p1;
                p1=p1->next;
            }
            else 
            {
                if(!newhead) newhead=p2;
                if(prev) prev->next=p2;
                prev=p2;
                p2=p2->next;
            }
        }
        if(p1) {if(prev) prev->next=p1;}
        else {if(prev) prev->next=p2;}
        return newhead;
    }
```

82. Remove Duplicates from Sorted List II
Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.
since the head may be removed, we add a dummy node to point to the head.
when the node val is the same we skip them
```cpp
    ListNode* deleteDuplicates(ListNode* head) {
        if(!head) return 0;
        ListNode *pre,*post;
        ListNode* dummy=new ListNode(INT_MIN);
        dummy->next=head;
        pre=dummy;
        post=head->next;
        ListNode *p=head;
        while(p)
        {
            while(post && post->val==p->val) post=post->next;
            if(post!=p->next) //contain more than one node
            {
                //delete all nodes between pre and post
                pre->next=post;
            }
            else  pre=p;//new pre updated only when no deletion
            p=post;//new p
            if(post) post=post->next;//new post
        }
        return dummy->next;
    }
```

142. Linked List Cycle II
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

Note: Do not modify the linked list.
use a slow and a fast pointer to detect if there is a cycle
if there is a cycle, starting another pointer at the head and it will meet the slow pointer at the entry.
It is proved using math:
2t=L+P+mC, for fast
t=L+P+nC, for slow
then t=(m-n)C=L+P+nC so we get L+P=(m-2n)C, so the two will meet at L.
```cpp
ListNode *detectCycle(ListNode *head) {
    if (head == NULL || head->next == NULL)
        return NULL;
    
    ListNode *slow  = head;
    ListNode *fast  = head;
    ListNode *entry = head;
    
    while (fast->next && fast->next->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) {                      // there is a cycle
            while(slow != entry) {               // found the entry location
                slow  = slow->next;
                entry = entry->next;
            }
            return entry;
        }
    }
    return NULL;                                 // there has no cycle
}
```

2. Add Two Numbers
number stored in reverse order in list.

143. Reorder List
split the list in half, and reverse the 2nd half and then merge one by one
```cpp
    void reorderList(ListNode* head) {
        //divide the list by half, reverse the second one and merge
        int len=0;
        ListNode* p=head;
        while(p) {len++;p=p->next;}
        ListNode* right=split(head,len/2);
        right=reverseList(right);
        //merge two lists in-place
        p=0;
        ListNode *p1=head,*p2=right;
        ListNode *next1,*next2;
        while(p1&&p2) 
        {
            next1=p1->next; //save its next node
            next2=p2->next; //save its next node
            p=p1;//p1 is now changed the next pointer
            p->next=p2;//
            p->next->next=next1; //this is very important to point to right nodes
            p1=next1;
            p2=next2;
            p=p->next;
        }
        if(p1) p->next=p1;
        if(p2) p->next=p2;
        return;
    }
    ListNode* split(ListNode* head,int split_len)
    {
        if(!split_len) return head;
        ListNode* p=head;
        int cnt=0;
        while(cnt<split_len-1) {p=p->next;cnt++;}
        ListNode* newhead=p->next;
        p->next=0;
        return newhead;
    }

   ListNode* reverseList(ListNode* root)
    {
        ListNode* p=root,*prev=0,*next;
        while(p)
        {
            next=p->next; //save its next
            p->next=prev; //reverse its next
            prev=p;//current node becomes prev
            p=next; //go to next node
        }
        return prev;//p will become empty
    }
```

61. Rotate List
Given a linked list, rotate the list to the right by k places, where k is non-negative.
get the list length, and k%len and then find the new head and connect the 2nd half with the first half.

138. Copy List with Random Pointer
need hashmap to map the pointer to index

25. Reverse Nodes in k-Group
k<2 no change
head will be changed, add a dummy node
reverse in place of n nodes (it needs get the first and the last)
pre, curr, next.

23. Merge k Sorted Lists
using priority_queue or multiset to merge the lists.
```cpp
    ListNode* mergeKLists(vector<ListNode*>& lists) {
       //use a min heap to do the sorting
        //a heap can be built using set or make_heap
        multiset<ListNode*,cmp1> myset;//(lists.begin(),lists.end());
        for(auto it=lists.begin();it!=lists.end();it++)
        {
            if(*it) myset.insert(*it);
        }
        
        //print(myset);
        ListNode* newhead=0,*prev=0,*p;
        while(!myset.empty())
        {
            if(!newhead) newhead=*myset.begin();
            p=*myset.begin();
            if(prev) prev->next=p;
            prev=p;
            myset.erase(myset.begin());
            if(p&&p->next) myset.insert(p->next);
        }
        return newhead;
    }
```
it first stores the starting elements into pq or multiset, then pop the smallest and advance its next and push into it.

# Chapter 9 Array
### Easy
### 1. two sum
trivial using hashmap O(n)
sort and use two pointers O(nlogn)

```cpp
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> ms;
        for(int i=0;i<nums.size();i++)
        {
            int n=nums[i];
            if(ms.count(target-n)) return vector<int>({ms[target-n],i});
            ms[n]=i;
        }
        return vector<int>();
    }
```	
905. sort array by parity
832. flipping image: stl reverse
561. Array Partition I: min pair sum, sort
922. Sort Array By Parity II
867. Transpose Matrix: A[i,j]=a[j,i]


### Medium
950. Reveal Cards In Increasing Order
reverse build the cards

442. Find All Duplicates in an Array
mark the number seen as negative, then those not marked is duplicates (will marked twice)

695. Max Area of Island
dfs or bfs from any nodes
dfs: remember to mark it visited.

238. Product of Array Except Self
left product and right product, two direction methodology

495. Teemo Attacking
need subtract those overlaps

667. Beautiful Arrangement II
1,k+1,2,k,3,k-1,4,k-2....

565. Array Nesting
use visited array, since every nested array will form a cycle. if a node is visited, it is not needed
769. Max Chunks To Make Sorted
split and sort, then connect and the whole array is sorted, return the max chunks
compare with the sorted array. If the two has same hash, then it is a segment
216. Combination Sum III
1-9, k numbers combination sum to n
dfs with backtrace. don't forget to pop out the solved node
78. Subsets
power subsets corresponds to n-bit from 0 to 1111...1
835. Image Overlap
max overlap
714. Best Time to Buy and Sell Stock with Transaction Fee
dp
900. RLE Iterator
count of numbers, number...
use a pointer to record the current head

926. Flip String to Monotone Increasing
min number of flips
if choose i as the mid point, we need flip all 1 in front, and all zeros behind
so left to right scan to get number of 1s
right to left scan to get number of 0s

48. Rotate Image
swap and reverse

729. My Calendar I
double booking
we need a sorted array of intervals and using binary search to find the one to add
731. My Calendar II
tripe booking
The key is to use double booking to create overlaps and to see the new added one overlaps with current overlap
732. My Calendar III: k booking
if we mark the start a new event, and end minus an event, then accumulate all until the ending is the k event. If we choose the max, we can iterate until the end.
segment tree: build a tree with cnt, start +1, end -1, the same idea
```cpp
class MyCalendarThree {
    map<int, int> timeline;
public:
    int book(int s, int e) {
        timeline[s]++; // 1 new event will be starting at [s]
        timeline[e]--; // 1 new event will be ending at [e];
        int ongoing = 0, k = 0;
        for (pair<int, int> t : timeline)
            k = max(k, ongoing += t.second);
        return k;
    }
};
```
62. Unique Paths-dp problem
39. Combination Sum
no duplicates, all combinations sum to target, number can be reused
dfs with backtracking
```cpp
    void helper(vector<int>& nums,int target,vector<vector<int>>& res,vector<int> tmp,int pos)
    {
        if(target==0) {res.push_back(tmp);return;}
        if(target<0) {return;}
        //solved solution?
        for(int i=pos;i<nums.size();i++)
        {
            //tmp.clear();
            tmp.push_back(nums[i]);
            helper(nums,target-nums[i],res,tmp,i);//since it allows duplicates, use i here
            tmp.pop_back();
        }
    }
```
40. Combination Sum II
each number can only be used once
```cpp
    void helper(vector<int>& nums,int target,vector<vector<int>>& res,vector<int> tmp,int pos)
    {
        if(target==0) {res.push_back(tmp);return;}
        if(target<0) {return;}
        //solved solution?
        for(int i=pos;i<nums.size();i++)
        {
            //tmp.clear();
            if(i && nums[i]==nums[i-1]&&i>pos) continue;
            tmp.push_back(nums[i]);
            helper(nums,target-nums[i],res,tmp,i+1);//since it does not allows duplicates, use i here
            tmp.pop_back();
        }
    }
```
216. Combination Sum III
use 1 to 9, and k numbers to sum to n
```cpp
void backtrack(vector<vector<int>> &result, vector<int> &path, int start, int k, int target)
{
    if(target==0 && k==0) //we reach a good solution
    {
        result.push_back(path);
        return;
    }
    for(int i=start; i<=10-k && i<=target; i++)
    {
        path.push_back(i);
        backtrack(result,path,i+1,k-1,target-i);//reduce a smaller problem k-1
        path.pop_back(); //after trial, go back to another
    }
}
```
377. Combination Sum IV
get the number of combinations. number can be repeated
knapsack with repetition

64. Minimum Path Sum
in matrix, dp problem,
59. Spiral Matrix II
fill the matrix from 1 to n^2
need to get: how many loops, and each loop how to iterate
621. Task Scheduler
given a list of tasks and requirement, return the min number of intervals needed
somewhat similar to the playlist
choose the most frequent char and divide it into k interval with n period
and put other inside, the min time is (k-1)*n
however when n is smaller than the period, we need suppress non-necessary idle

718. Maximum Length of Repeated Subarray
straightforward: sliding window to find max overlap
however, dp is the right way to find the largest common substr

611. Valid Triangle Number
a+b>c
sort first and then fix two a and b, find c

873. Length of Longest Fibonacci Subsequence
dp mixed with array tech, fixed i, i+1, and derive i+2

153. Find Minimum in Rotated Sorted Array
binary search

795. Number of Subarrays with Bounded Maximum
subarrays: for example 1,2,3 is a subarray, then it has [1],[1,2],[2],[2,3],[3],[1,2,3] total 6=1+2+3
using two pointers: 

289. Game of Life
state change at the same time: generally needs another copy for changes, but we can use bit in original to do in-place

11. Container With Most Water
this is a stack problem.
principle: 
each element must be pushed and poped once
increasing or decreasing order: depending on the question. 
For this question, when a lower bar comes, it shall be considered as the highest bar to contain water if using it as the right bar. So we need keep the stack increasing.

560. Subarray Sum Equals K
accumulate sum and find cumsum-k if exist
using hashmap

915. Partition Array into Disjoint Intervals
two parts, so that all elements in left < all elements in right
left and right problem: two directions to get the max

945. Minimum Increment to Make Array Unique
sort and add, greedy

870. Advantage Shuffle
Given two arrays A and B of equal size, the advantage of A with respect to B is the number of indices i for which A[i] > B[i].
Return any permutation of A that maximizes its advantage with respect to B.
greedy: if there is higher, use the smallest, if no, use the smallest.
sort it in multiset, if chosen then we remove it

162. Find Peak Element
num[i]>nums[i-1] && nums[i]>nums[i+1] naively by adding two padding INT_MIN
linear scan: only need compare nums[i]>nums[i+1] and that is the peak. (since nums[i-1]<nums[i] is the reverse of nums[i]>nums[i+1].)
binary search: we reduce the problem size by finding a mid, and mid+1
linear scan can find all local peaks, binary search can just find one.

792. Number of Matching Subsequences
find number of words in dict is subsequence of S
making a hashmap of s (the char vs the list of its index, can also use 26 char vector). Then greedy choose the first matching one

670. Maximum Swap
Given a non-negative integer, you could swap two digits at most once to get the maximum valued number. Return the maximum valued number you could get.
greedy: ith digit compare with its behind max. if less, swap

80. Remove Duplicates from Sorted Array II
inplace remove duplicates, allowing at most 2 duplicates. sorted.
two pointers: a[i]>a[j-2] then add it in

73. Set Matrix Zeroes
Inplace set its row and col to be zero.
just collect all zero's row and columns and set them to be zeros

105. Construct Binary Tree from Preorder and Inorder Traversal
in order: left node right
preorder: node left right
so we can get the root from the preorder first, and split the left and right from in-order
and then recursively solve the subproblems.

120. Triangle
min path sum, dp as min path sum.

106. Construct Binary Tree from Inorder and Postorder Traversal
similar to 105

775. Global and Local Inversions
i<j, A[i]>A[j] calls global inversion, j=i+1 is called local inversion
check if local inversion == global inversion. The array is 0-N-1
Apparently we only have local inversion, that means if we compare with 0,1,...N-1 and we only see local inversions.

713. Subarray Product Less Than K
similar to sum, if product >k then remove the oldest one.

74. Search a 2D Matrix
row sorted. next row > previous row
2d search using the property. start from the bottom left

228. Summary Ranges
sorted array, summary of ranges, merge into ranges

56. Merge Intervals
classical: sort the interval, and iterate and merge always to the back, and update the back

825. Friends Of Appropriate Ages

209. Minimum Size Subarray Sum
Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.
common practice: when sum>=s we keep shrinking the range from the left side

63. Unique Paths II-dp
34. Find First and Last Position of Element in Sorted Array-equal range
33. Search in Rotated Sorted Array-binary search
954. Array of Doubled Pairs
if we can arrange it 2*A[i]=A[i+1]
using multiset to store negative and positive separately
229. Majority Element II
find all elements appearing >n/3 times
hashmap

55. Jump Game
array number is the max steps you can jump, find if we can reach the last.
max steps we can reach at i, and also we must be able to reach i (reach>=i)

79. Word Search
dfs at any starting position. we need mark visited position first and then restore

31. Next Permutation
the next bigger permutation. from right to left, choose the first smaller one with larger one behind it, swap
918. Maximum Sum Circular Subarray
two cases: if the max is inside, and if the min is inside
907. Sum of Subarray Minimums
for each element we find a window which A[i] is the min.


724. Find Pivot Index.md
### Problem Summary
its left and right sum are equal
find the first index

### idea
it is finding the first target with tsum-2*prefixsum

### code
```cpp
    int pivotIndex(vector<int>& nums) {
        int psum=0;
        int tsum=0;
        tsum=accumulate(nums.begin(),nums.end(),0);
        for(int i=0;i<nums.size();i++)
        {
            if(tsum-nums[i]==2*psum) return i;
            psum+=nums[i];
        }
        return -1;
    }
```

### comment
- we can try lsum and rsum from both end and it is more complicated and shall consider all edge cases and match the index.

# Chapter 10 Stack
682. Baseball Game
simple

496. Next Greater Element I
for each number is array 1 find its next greater element in its right. If there is none, return -1.
this is a good practice for stack, keeping the stack decreasing order. 
why? since we need to find the right bigger one. When the new number comes, we know it is the answer and we do not need it in the stack any more.
O(n) since each element is pushed and poped once.
```cpp
    vector<int> nextGreaterElement(vector<int>& findNums, vector<int>& nums) {
        int n=nums.size();
        stack<int> st;
        unordered_map<int,int> mp; //mp the number with its next greater
        for(int i=0;i<n;i++)
        {
            while(!st.empty() && st.top()<nums[i])
            {
                mp[st.top()]=nums[i];
                st.pop();
            }
            st.push(nums[i]);
        }
        vector<int> ans(findNums.size(),-1);
        for(int i=0;i<findNums.size();i++)
        {
            if(mp.count(findNums[i])) ans[i]=mp[findNums[i]];
        }
        return ans;
    }
```
Note:
1. keep popping when the top < nums[i] since previous may all have it as the right
2. every element shall be pushed once
3. it is regular practice using stack comparing in the front or behind in an array
4. stack is generally O(n) complexity and other means generally takes O(N^2)

503. Next Greater Element II
note this is a circular buffer. 
We first push the array in reverse order:
So in the stack we have:
An-1, An-2, ....A1, A0, where A0 is on the top. Thus each element has n-1 elements on its right side.
This is basically the idea of doubling the array to simulate the circular buffer.
complexity O(2*n)
```cpp
    vector<int> nextGreaterElements(vector<int>& nums) {
        int n=nums.size();
        stack<int> st;
        vector<int> res(n,-1);
        for(int i=n-1;i>=0;i--) st.push(nums[i]);
        for(int i=n-1;i>=0;i--)
        {
            while(!st.empty() && st.top()<=nums[i]) st.pop();
            if(!st.empty()) res[i]=st.top();
            st.push(nums[i]);
        }
        return res;
    }
```

844. Backspace String Compare
simple
232. Implement Queue using Stacks
simple
225. Implement Stack using Queues
20. Valid Parentheses
use stack or vector, no difference

155. Min Stack
need a min vector

921. Minimum Add to Make Parentheses Valid
simple, removing paired in stack. keep recording extra )

946. Validate Stack Sequences
simulating the stack push and pop

739. Daily Temperatures
classical stack, need to get how many days after the temperature is higher.
When higher, we need pop all previous lower temperatures, so the stack is in decreasing order.
```cpp
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> st;
        int n=temperatures.size(),j;
        vector<int> ans(n);
        for(int i=0;i<n;i++)
        {
            while(!st.empty() && temperatures[i]>temperatures[j=st.top()])
            {
                ans[j]=i-j;
                st.pop();
            }
            st.push(i);
        }
        return ans;
    }
```
856. Score of Parentheses
() has 1 score, AB has A+B where A and B are number of balanced pairs, (A) is 2*A
this is not so straightforward however.
Use the stack to keep the score:
when ( push 0
when ) check the top: if it is not zero, then double it, else +1
then update top()+current score (for connecting paired parenthese)
```cpp
    int scoreOfParentheses(string S) {
        stack<int> st;
        st.push(0);
        for(int i=0;i<S.size();i++)
        {
            if(S[i]=='(') st.push(0);
            else
            {
                int score=st.top();
                st.pop();
                if(score==0) score=1;
                else score*=2;
                st.top()+=score;
            }
        }
        return st.top();
    }
```
Note: it is important to first push 0 in the stack, otherwise it will be wrong.

94. Binary Tree Inorder Traversal
iteration using stack: left, root, right, so we need to put the node into the stack first and then goes to left, and then pop to process the root and then goes to right by following similar steps.

144. Binary Tree Preorder Traversal
root left right, so we process root first, then we push its right and process its left

636. Exclusive Time of Functions
- get the id, start/end time
- when next start, accumulate the time
- when another function starts, the previous function suspends and the time shall be subtracted from other function.
```cpp
    vector<int> exclusiveTime(int n, vector<string>& logs) {
        //when it starts pushed in the stack, when it ends, pop it up and store in array
        vector<int> ans(n);
        stack<pair<int,int>> st; //id, time
        for(int i=0;i<logs.size();i++)
        {
            int ind=logs[i].find_first_of(':');
            int id=stoi(logs[i].substr(0,ind));
            int ind1=logs[i].find_first_of(':',ind+1);
            if(ind1-ind==6) //start
            {
                int stime=stoi(logs[i].substr(ind1+1));
                st.push(make_pair(id,stime));
            }
            else //endtime
            {
                int etime=stoi(logs[i].substr(ind1+1));
                int dtime=etime-st.top().second+1;
                ans[id]+=dtime;
                st.pop();
                if(!st.empty()) ans[st.top().first]-=dtime;//equivalent to add the time to the start
            }
        }
        return ans;
    }
```
The key here is: we subtract the time used by other function from the start on the stack top.

341. Flatten Nested List Iterator
This problem is very confusing.
The class NestedInteger: it could be a single integer, or a list of Nested Integers for example 5, [5], [4,[5,[6]]
Needs to build an iterator class for a vector of nested integer.
NestedInteger:
isInteger: the nested integer is a single integer
getInteger: only works for single integer NestedInteger
getList: only works for non-single integer nested integer (since a nestedInteger is also a vector of nestedInteger

The iterator shall:
- be able to navigate the vector
- when the node is non-single integer node, we need expand the iterator.
- using the stack to pop the composite node iterator and replace with its inside iterator ( a list of iterator)
- using begin and end, then we do not need save iterator for each element

173. Binary Search Tree Iterator
in order traversal.
starting from root, we need push all its left
when pop one, we need push its right
```cpp
class BSTIterator {
    //use a stack to keep all the parent nodes
    stack<TreeNode*> s;
    void update(TreeNode *root)
    {
        while(root){s.push(root);root=root->left;}
    }
public:
    BSTIterator(TreeNode *root) {
        update(root);
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !s.empty();
    }

    /** @return the next smallest number */
    int next() {
        //get the top of the stack, it is always the leftmost node
        TreeNode *node=s.top();s.pop();
        update(node->right);
        return node->val;
    }
};
```
901. Online Stock Span
stock span is defined as: from today, number of consecuative days  (previous) prices<=today's price.
Typical stack problem since we need old data.
When prices is higher, we need pop out those smaller ones, so we are maintaining a decreasing stack.

394. Decode String
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef"
natural thinking: we push the [] in stack and then we evaluate the most innerside first. Each string inside [] is also a subproblem. We need to assemble to get a bigger problem.
The complexity is the inner string keeps expanding.
A often used method is: when we meet the first [, we leave the remaining as a subproblem. So expanding is always the latter part

```cpp
    string helper(int& pos, string s) {
        int num=0;
        string word = "";
        for(;pos<s.size(); pos++) 
        {
            char cur = s[pos];
            if(cur == '[') 
            {
                string curStr = helper(++pos, s);
                for(;num>0;num--) word += curStr;
            } 
            else if (cur >= '0' && cur <='9') 
                num = num*10 + cur - '0';
            else if (cur == ']') 
                return word;
            else     // Normal characters
                word += cur;
        }
        return word;
    }
```

103. Binary Tree Zigzag Level Order Traversal
level order traversal and then reverse all even rows (without stack)
using stack to reverse can also be fine (this is an important function of stack)

331. Verify Preorder Serialization of a Binary Tree
preorder: root, left, right. Null is represented as #
method 1: when we see a non-null node, it must add two nodes. When we see # it just add itself. (A null root will has only one #)
We can count non-null nodes and +2, adding any node -1, the whole shall be 0
method 2: using stack O(n), push each once and pop once. 
When we meet a #, keep poping until it is not #. Then pop the non-null node. That means the node # # is replace with a #, indicating the node is fully explored and absorbed as a null node #. At the end, each node shall be explored and leaving a # only in the stack.

"9,3,4,#,#,12,#,#,2,#,6,#,#"

stack status

char   stack
'9':   '9'  
'3':   '3','9'
'4':   '4','3','9'
'#':   '#','4','3','9'
'#':   '#','3','9'
'12':  'n', '#', '3','9'
'#':   '#','1', '#', '3','9'
'#':   '#','3','9' -> '#','9'
'2':   '2', '#','9'
'#':   '#', '2', '#','9'
'6':   '6', '#', '2','#','9'
'#':   '#', '6', '#', '2','#','9'
'#':   '#', '2','#','9' -> '#','9' -> '#'
Stack is hard to understand!!!

735. Asteroid Collision
Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.
The right ones trying to go right, and the left ones trying to go left.
O(n) solution: push into the stack, only when the new comer is negative will collide
- at the end, all the negative star has to be on the left, and all the positive star has to be on the right.
- from the left, a negative star will pass through if no positive star on the left;
- keep track of all the positive stars moving to the right, the right most one will be the 1st confront the challenge of any future negative star.
- if it survives, keep going, otherwise, any past positive star will be exposed to the challenge, by being popped out of the stack.

```cpp
    vector<int> asteroidCollision(vector<int>& asteroids) {
        vector<int> ret;
        if (asteroids.size() == 0) {
            return ret;
        }
        
        stack<int> st;
        for (int i = 0; i < asteroids.size(); i++) 
        {
            int curr = asteroids[i];
            bool skip = false;
            while (!st.empty() && (st.top() > 0 && curr < 0)) 
            {
                int t1 = st.top(); st.pop();
                int t2 = asteroids[i];
                if (t1 + t2 == 0) 
                {
                    skip = true;
                    break;
                } 
                else if (abs(t1) > abs(t2)) curr = t1;
                else curr = t2;
            }
            if (!skip) st.push(curr);
        }
        
        while (!st.empty()) 
        {
            ret.emplace(ret.begin(), st.top());
            st.pop();
        }
        
        return ret;
    }
```

853. Car Fleet
cars running along the same lane at different speed and different positions, when catched up, the two cars bumps together to one fleet. At the destination, get the number of fleets.
method 1: calculate the time for each car to reach the destination. Sort it according to the positions. Reversely check if ith car needs less time than i+1 car, then the two cars merges to a fleet.
method 2: stack. We put the farthest car into stack, when the next car comes and uses less time, pop the old one. The remaining in the stack will be the number of fleets. (almost the same as method 1 and stack is not really necessary)

385. Mini Parser
deserialize the nested integers.
```cpp
    NestedInteger deserialize(istringstream &in) {
        int number;
        if (in >> number)  return NestedInteger(number);
        in.clear();//clear error flag (not a number)
        in.get();//get a char
        NestedInteger list;
        while (in.peek() != ']') 
        {
            list.add(deserialize(in));
            if (in.peek() == ',')
                in.get();
        }
        in.get();
        return list;
    }
```
150. Evaluate Reverse Polish Notation
The operator follows the two numbers.
["4", "13", "5", "/", "+"] means: 4+(13/5)
using  a stack: 
if seen an operator, pop the two numbers, calculate it and push back for further evalulation
```cpp
   int evalRPN(vector<string>& tokens) {
        stack<string> st;
        st.push(tokens[0]);
        int i=1;
        int ans;
        while(i<tokens.size())
        {
            if(tokens[i]=="+" || tokens[i]=="-" || tokens[i]=="*" || tokens[i]=="/")
            {
                int b=stoi(st.top());st.pop();
                int a=stoi(st.top());st.pop();
                if(tokens[i]=="+") st.push(to_string(a+b));
                else if(tokens[i]=="-") st.push(to_string(a-b));
                else if(tokens[i]=="*") st.push(to_string(a*b));
                else if(tokens[i]=="/") st.push(to_string(a/b));
            }
            else st.push(tokens[i]);
            i++;
        }
        return stoi(st.top());
    }
```
71. Simplify Path
. current dir
.. previous dir
using a stack

456. 132 Pattern (**)

this is a classical stack problem. It needs looking for a peak like feature (i<j<k && Ai<A[k]<A[j]).
Suppose we want to find a 123 sequence with s1 < s2 < s3, we just need to find s3, followed by s2 and s1. Now if we want to find a 132 sequence with s1 < s3 < s2, we need to switch up the order of searching. we want to first find s2, followed by s3, then s1.
More precisely, we keep track of highest value of s3 for each valid (s2 > s3) pair while searching for a valid s1 candidate to the left. Once we encounter any number on the left that is smaller than the largest s3 we have seen so far, we know we found a valid sequence, since s1 < s3 implies s1 < s2.

```cpp
    bool find132pattern(vector<int>& nums) {
        //from right side to left and keep in stack
        stack<int> st;
        int s3=INT_MIN;
        for(int i=nums.size()-1;i>=0;i--)
        {
            if(nums[i]<s3) return 1;
            else while(st.size() && nums[i]>st.top())
            {
                s3=st.top();st.pop();
            }
            st.push(nums[i]);
        }
        return 0;
    }
```
Note: the program gets the max s2 and s3 (s2>s3) pairs from right to left. 

402. Remove K Digits
Given a non-negative integer num represented as a string, remove k digits from the number so that the new number is the smallest possible.
greedy: remove the first peak digit for k times
stack: keeping the stack increasing order, when a smaller one comes, pop previous until we got k

907. Sum of Subarray Minimums
Given an array of integers A, find the sum of min(B), where B ranges over every (contiguous) subarray of A.
method 1: since each number could be the min, we need just calculate the window for each number and get the sum (left*right*A[i])
```cpp
    int sumSubarrayMins(vector<int>& A) {
        int mod=1e9+7;
        //each element can appear many times
        //for element i, we have a window size which A[i] is the minimum
        vector<int> win(A.size());
        int ans=0;
        for(int i=0;i<A.size();i++)
        {
            int l=i,r=i;
            while(A[r+1]>A[i] && r<A.size()-1) r++;
            while(l>0 && A[l-1]>=A[i]) l--;
            ans+=((((r-i+1)*(i-l+1))%mod)*A[i])%mod;
            ans%=mod;
        }
        return ans;
    }
```    
method 2: this is another classical stack using monotone increasing sequence
We push the number in stack, the elements in stack are all smaller than it. When a number is larger just pushed in, when a number smaller, we need pop all larger ones and we get its left. 

880. Decoded String at Index
The string keeps repeating and growing, another problem on this.
cannot hold the string for only finding the kth char.


# Chapter 11 String
890. Find and Replace Pattern
permuation of pattern and match
we can transform both the pattern and the word into same abc.., the first char is a and second appeared is b...
and then check if they are equal

537. Complex Number Multiplication
just using stringstream to read real and imag correctly

791. Custom Sort String
s is sorted using some custom sort, we need sort string t
using hashmap to arrange the sorted chars first

553. Optimal Division
The second one is always under, others are above, so there is only one answer

647. Palindromic Substrings--dp
count the number of pal-substring
dp[i][j]=self+dp[i][j-1]+dp[i+1][j]-dp[i+1][j-1];

856. Score of Parentheses
using stack
or dp: dp[i] represents the score 
when (, the score is 0

22. Generate Parentheses
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses
recursive build: has n left ( and n right ). The principle is any time left>=right
```cpp
    void build(vector<string>& vs,int n,int lcount,int rcount,string s)
    {
        if(s.size()==2*n) {vs.push_back(s);return;}
        if(lcount<n) build(vs,n,lcount+1,rcount,s+"(");
        if(lcount>rcount) build(vs,n,lcount,rcount+1,s+")");
    }
```
12. Integer to Roman
<4
4
6-8
need to know the rules

539. Minimum Time Difference
that is a wrap. sort it for integer, and the last one and the first one (make it a circle) negative +24*60

583. Delete Operation for Two Strings-dp
916. Word Subsets
the words need include all the chars in the dictionary including number of occurance
hash

833. Find And Replace in String
record the operations
and build the string from end to head (where the index does not change)

49. Group Anagrams
using hashmap

816. Ambiguous Coordinates
all spaces,'.' and ',' are removed. return all possible combinations
this is recursive, we iteratively split the string into half, and then add decimal at different positions for each of them
and then combine the left and right

831. Masking Personal Information
masking the email and phone numnber

809. Expressive Words
letters are repeated more than needed (at least 3 same char)
use two pointer 


767. Reorganize String
arrange the most frequent first. and then less frequent

17. Letter Combinations of a Phone Number
return all combinations of letters

848. Shifting Letters
shift the first number of chars by a[i] times.
easy it is accum sum for each letter

842. Split Array into Fibonacci Sequence
observation: the first two number uniquely determines the whole array. number has no leading zero
the idea is:
first try to choose the first number, 1 digit, 2 digits...,.....
then choose the second number: 1 digits, 2 digits..... and then we check if we can exhaust the string
when there is 0 number, find num1
when there is 1 number, find num2
when there is 2 or more number, find the last two sum

need a start index, and current index

522. Longest Uncommon Subsequence II
defined: a subsequence of one string and is not subsequence of any other string. need return the length
intuition: 
sort in size. long subsequence cannot be subsequence of short strings
check if a str is subsequence of another string: using two pointer
the most important: the subsequence shall be one of the words. since if subsequence cannot be common, adding some more char is also not common.

227. Basic Calculator II
supports + - * /
simple syntax processing: 
if * and / we need to read the followed number to complete calculation
if + and - we need get the left and right results before do the calculation
better using stack to store the results

385. Mini Parser
[] is an object, [[]] nested. the basic is the object. recursive for nested

678. Valid Parenthesis String
* can be left, right, or empty.
rule: at any position, left ( is always >= right ).
recursive: count left and right, and index of current char, treat * as left right or empty
```cpp
    bool is_validstring(const string& s,int i,int nleft,int nright)
    {
        if(nleft<nright) return 0;
        if(i==s.length())
        {
            return nleft==nright;
        }
        if(s[i]=='(') return is_validstring(s,i+1,nleft+1,nright);
        if(s[i]==')') return is_validstring(s,i+1,nleft,nright+1);
        else //use it as left, right or nothing
        {
            return is_validstring(s,i+1,nleft,nright) || is_validstring(s,i+1,nleft+1,nright) || is_validstring(s,i+1,nleft,nright+1);
        }
    }
```
93. Restore IP Addresses
ip is unsigned int8, no leading zero
each number has 1 to 3 digits
all 4 numbers adds to total length of string
(leading zero can be eliminated by converting back to string)

6. ZigZag Conversion
accumulate vector string can connect the strings

722. Remove Comments
remove c++ comments
regular expression: 
vector of a string to a string using copy to stringstream.

556. Next Greater Element III
using the same digits
next permutation or greedy to swap the first smaller with first larger (reverse direction)

43. Multiply Strings
simple
71. Simplify Path
. current dir
.. upper dir
use a stack or vector to record the actual path

5. Longest Palindromic Substring--dp
3. Longest Substring Without Repeating Characters
for all substring ending at i. the substring without duplicate. two pointer.

165. Compare Version Numbers
note string compare is not sufficient since it allows leading zeros. we need convert to int and then convert to string.
read from a string using some delim and copy back to another output stringstream. getline can accept delim

    string sentence = "And I feel fine...";
    istringstream iss(sentence);
    copy(istream_iterator<string>(iss),
         istream_iterator<string>(),
         ostream_iterator<string>(cout, "\n"));
       
91. Decode Ways--dp
468. Validate IP Address
validate ipv4 or ipv6

151. Reverse Words in a String
common trick using stringstream to convert to vector

8. String to Integer (atoi)

214. Shortest Palindrome.md
### problem summary
by adding min number of chars in the front to make the string palindrome

### approach
The problem is to find the longest palindrome string starting with 0.
However brutal force TLE for all aaaaa since we have to repeat each char until the longest

The smartest approach using recursive
we use two pointers i and j from both end. if j goes to end, the whole string is a palindrome string.
that is a reverse compare. Otherwise, we divide the String by j, and get mid = s.substring(0, j) and suffix.

```cpp
    string shortestPalindrome(string s) {
        //find the longest palindrome in s and then use it as the base to add
        //the longest must start with the first char
        int n=s.size();
        int j = 0;
        for (int i = s.length() - 1; i >= 0; i--) 
        {
            if (s[i] == s[j]) { j += 1; }
        }
        if (j == s.length()) { return s; }
        string suffix=s.substr(j);
        string rss=suffix;
        reverse(rss.begin(),rss.end());
        return rss+shortestPalindrome(s.substr(0,j))+suffix;
    }
```    

# Chapter 12 dfs
dfs is generally recursive, reducing to similar subproblems
generally involves push and pop a new path
generally uses coloring to indicate visited nodes
dfs/bfs/disjoint set sometimes can be all used for similar problems

559. Maximum Depth of N-ary Tree
max depth is 1+maxdepth(children)
bfs is even faster

872. Leaf-Similar Trees
dfs all tree first left and then right, combined to a string or an array

897. Increasing Order Search Tree
inorder traversal of the tree and arrange the nodes in a linked list.
naive solution is trivial
inplace is more meaningful
connection is left->root->right. we need the last node to connect.
first subproblem: the left connects the root as the last node, modify the root's left=NULL. and its right shall be the subproblem for the right subtree.
it returns the new root, 
```cpp
    TreeNode* increasingBST(TreeNode* root, TreeNode* tail = NULL) {
        if (!root) return tail;
        TreeNode* res = increasingBST(root->left, root);
        root->left = NULL;
        root->right = increasingBST(root->right, tail);
        return res;
    }
```

104. Maximum Depth of Binary Tree
same as 559, but we only have left and right child

690. Employee Importance
employee's importance=itself's importance+all its subordinate's importance.
this is a n-tary tree with its sum.
first build a tree map, and then use recursion:
importance=self+sum(child's importance), child's importance is a subproblem

100. Same Tree
check left sub and right sub equal and root equal

733. Flood Fill
typical dfs
```cpp
    void dfs(vector<vector<int>>& image,int sr,int sc,int color,int newColor)
    {
        int m=image.size(),n=image[0].size();
        if(sr<0 || sc<0 || sr>=m || sc>=n) return;
        if(image[sr][sc]==color)
        {
            image[sr][sc]=newColor;
            dfs(image,sr-1,sc,color,newColor);
            dfs(image,sr+1,sc,color,newColor);
            dfs(image,sr,sc-1,color,newColor);
            dfs(image,sr,sc+1,color,newColor);
        }
    }
```
Note: need first change the color before processing the subproblem, otherwise, it will go into infinite loop

108. Convert Sorted Array to Binary Search Tree
convert the sorted array into balanced binary tree
choose the mid number as the root, then solve left and right subtree recursively

257. Binary Tree Paths
output the path in a string
classical dfs: when get a node, add into the string, dfs the left, pop out the node, dfs the right
```cpp
    void dfs(TreeNode* root, string path, vector<string>& vs)
    {
        if(!root) return;
        if(!root->left && !root->right) 
        {
            path+=to_string(root->val);
            vs.push_back(path);
            return;
        }
        path+=to_string(root->val)+"->";
        dfs(root->left,path,vs);
        dfs(root->right,path,vs);
    }
```
Note:
- need process the leaf node to add the answer (cannot use null node for this since it will give wrong path)
- use value passing for path, so we do not need to push and pop the string

101. Symmetric Tree
recursively check whether l->left equals r->right and l->right equals r->left.

110. Balanced Binary Tree
recursively check if the left sub height vs right sub height difference is <=1

743. Network Delay Time
times[i] = (u, v, w), from node u to node v, takes travel time w
from node K, what is the time required for all nodes to receive it.
max time of all min time from K to other nodes.
this could be a dp, dijkstra, bellman-ford problem. Trying to relax edges from k to all other nodes.
dfs: from k to all other node, calculate the min path sum.
dijkstra: using priority_queue or multiset (min heap) to try the smallest first to relax the edge
```cpp
    typedef pair<int,int> pii;
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<pii> g[n + 1];
        for (const auto& t : times) g[t[0]].push_back(make_pair(t[1], t[2]));
        const int inf = 1e9;
        vector<int> dist(n + 1, inf);
        dist[k] = 0;//k to k
        priority_queue<pii, vector<pii>, greater<pii> > pq;
        pq.push(make_pair(0, k));//distance, destination
        int u, v, w;
		vector<bool> visited(n + 1);
        while (!pq.empty()) 
        {
            pii p = pq.top(); pq.pop();
            u = p.second;
			if (visited[u]) continue;
            for (auto& to : g[u]) 
            {
                v = to.first, w = to.second;
                if (dist[v] > dist[u] + w) 
                {
                    dist[v] = dist[u] + w;
                    pq.push(make_pair(dist[v], v));
                }
            }
			visited[u] = 1;
        }
        int ans = *max_element(dist.begin() + 1, dist.end());
        return ans == inf ? -1 : ans;
    }
```
the dijkstra:
- form the graph
- maintain a min distance from k to all other nodes (from k to k is 0)
- priority-queue: using the distance for compare, min heap (try smallest distance first)
- use the other nodes to relax the edge and get the min dist.
- when the edge can be relaxed, add the updated distance back to priority-queue
- stop when no more distance can be relaxed.
- this is not an easy problem.

112. Path Sum
check if the tree has a root to leaf path sum=target
dfs or recursive. reduce to sub problem with target-root on left sub or right sub

111. Minimum Depth of Binary Tree
similar for max depth 1+min(left height,right height)

959. Regions Cut By Slashes
upsample and do dfs
need at least upsample to 3x3, otherwise connected region will be separated.

841. Keys and Rooms
N rooms, and each room has a list of keys to open other rooms
check if we can open all rooms,initially at room 0
that is dfs, we need check all possible keys and see if all rooms are visited.
bfs is also ok. 
use set or other mechanism to record visited.

513. Find Bottom Left Tree Value
Given a binary tree, find the leftmost value in the last row of the tree
if right height is bigger, find in right subtree
else find in left subtree

515. Find Largest Value in Each Tree Row
can use hash map for each layer and get the max using traversal (any order)

695. Max Area of Island
dfs and count the number and mark it as different color
dfs is much simpler than bfs.
The following code is used for finding the area of one connected region

```cpp
    int dfs(vector<vector<int>>& grid,int i,int j)
    {
        int m=grid.size(),n=grid[0].size();
        if(i<0 || j<0 || i>=m || j>=n) return 0;
        if(grid[i][j]!=1) return 0;
        grid[i][j]=2;
       return dfs(grid,i-1,j)+dfs(grid,i+1,j)+dfs(grid,i,j-1)+dfs(grid,i,j+1)+1;
    }
```    

547. Friend Circles
disjoint set could be perfect for this
if M[i,j]==1 then we merge i and j

for dfs: if M(i,j)==1 then we need dfs from i and dfs from j. the two are a group. That means we shall iterate on the person instead of the edges, hence coloring on the matrix is not good. instead we shall use the person if visited.

The relation matrix is symmetric. and can we only need to use half? The answer is no, since when we found i relates with j, j could relates with people ahead of i. so We need iterate on all people.
```cpp
    int findCircleNum(vector<vector<int>>& M) {
        int num_cycle=0;
        vector<bool> visited(M.size());
        for(int i=0;i<M.size();i++) //for each person
        {
            if(!visited[i]) dfs(M,i,visited),num_cycle++;
        }
        return num_cycle;
    }
    void dfs(vector<vector<int>>& M,int node,vector<bool>& visited)
    {
        for(int i=0;i<M.size();i++) //shall start from 0 instead of from node
        {
            if(M[node][i] && !visited[i]) visited[i]=1,dfs(M,i,visited);
        }
    }
```

529. Minesweeper
given a minesweeper and a click on (i,j) return the next status
if click on M, then change to x, game over
if click on E, change to B and reveal all its adjacent empty cell. If the empty cell has mines adjacent, change to the number of mines.
adjacent is 8 directions.
dfs with 8 directions

```cpp
    void reveal(vector<vector<char>>& board, int x, int y){
         int dir[][2]={{0,1},{0,-1},{1,0},{-1,0},{1,1},{1,-1},{-1,1},{-1,-1}};
        int m=board.size(),n=board[0].size();
        if(board[x][y] == 'E')
        {
            //search 8 adjacent squares
            int count = 0;
            for(int i=0;i<8;i++)
            {
                int tx=x+dir[i][0],ty=y+dir[i][1];
                if(tx>=0 && tx<m && ty>=0 && ty<n && board[tx][ty]=='M') count++;
            }
            if(count>0) board[x][y] = '0'+count;
            else
            {
                board[x][y] = 'B';
                for(int i=0;i<8;i++)
                {
                    int tx=x+dir[i][0],ty=y+dir[i][1];
                    if(tx>=0 && tx<m && ty>=0 && ty<n) reveal(board,tx,ty);
                }
            }
        }
    }
```

947. Most Stones Removed with Same Row or Columns
stones share same row or column are  in a connected group. 
we can only remove a stone when stone has >1 stones in a group, leaving one stone at each group. 
So the max number of stones we can remove is N-num_groups
disjoint set is perfect for this.
let's try dfs way.
we can use dfs with coloring
Note: the input is the coordinates for the n stones. The coordinate could be much larger than n. So build a grid is pretty naive.
```cpp
    int removeStones(vector<vector<int>>& stones) {
        int num_groups=0;
        int n=stones.size();
        vector<bool> visited(n);
        for(int i=0;i<n;i++)
        {
            if(!visited[i]) dfs(stones,i,visited),num_groups++;
        }
        return n-num_groups;
    }
    
    void dfs(vector<vector<int>>& stones,int i,vector<bool>& visited)
    {
        visited[i]=1;
        for(int j=0;j<stones.size();j++)
        {
            if(visited[j]) continue;
            if(stones[i][0]==stones[j][0] || stones[i][1]==stones[j][1])
            {
                dfs(stones,j,visited);
            }
        }
    }
```
The above solution is much slower than using disjoint set (140ms vs 40ms). Since we use a loop to find all those nodes sharing coordinates, this could be reduced using a hashmap.

756. Pyramid Transition Matrix
It is very hard to understand the question. Let me explain:
Given a list of allowed triplets, the pyramid is built layer by layer, each triangle (upper layer node with its adjacent lower layer two nodes) must be in the allowed list. 
The question is: given the bottom layer and the allowed list, can we build the pyramid? (all the way to the top).

So the solution is:
get the possible string for upper layer and recursively solve each problem.
we can build a map with two chars with the other char. Building all kinds of combinations from lower layer can use dfs.

638. Shopping Offers
this is more like a dp or knapsack problem

337. House Robber III
dp problem

199. Binary Tree Right Side View
bfs the last one is the anwer

851. Loud and Rich
N people
richer: richer(x,y) means x is richer than y, only a subset is provided.
quite[x] is the quiteness of person x
find the person with money >= person x and is the loudest. 
for n people, that is n problems.
example: [[1,0],[2,1],[3,1],[3,7],[4,3],[5,3],[6,3]], quiet = [3,2,5,4,6,1,7,0]
for person 0:
who is richer than him? 1
who is richer than 1? 2,3
who is richer than 2,3? 4,5,6
7 is less rich than 3 but not sure if 7 is richer than 1, so the anwer is 5 (who is loudest)

just build a map for each person with a list of richer person. and do dfs.
```cpp
    unordered_map<int, vector<int>> richer2;
    vector<int> res;
    vector<int> loudAndRich(vector<vector<int>>& richer, vector<int>& quiet) {
        for (auto v : richer) richer2[v[1]].push_back(v[0]);
        res = vector<int> (quiet.size(), -1);
        for (int i = 0; i < quiet.size(); i++) dfs(i, quiet);
        return res;
    }

    int dfs(int i, vector<int>& quiet) {
        if (res[i] >= 0) return res[i];
        res[i] = i;
        for (int j : richer2[i]) if (quiet[res[i]] > quiet[dfs(j, quiet)]) res[i] = res[j];
        return res[i];
    }
```

494. Target Sum
with + and - for each number, find number of ways to get target sum S.
can be a dp or recursion with memoization
Note the S shall be in the range of [-m, m] where m is the accumulate sum.
each number has +/- two selections, the complexity would be O(2^N) without the memoization to avoid repeatedly solving overlapped subproblems.

```cpp
    int findTargetSumWays(vector<int>& nums, int S) {
        int n=nums.size();
        int m=accumulate(nums.begin(),nums.end(),0);
        if(m<abs(S)) return 0;
        vector<unordered_map<int,int>> status(n);
        return helper(nums,S,0,status,m);
    }
    int helper(vector<int>& nums,int target,int cur_pos,vector<unordered_map<int,int>>& status,int offset)
    {
        if(cur_pos==nums.size()) return target?0:1;
        //(status[cur_pos][target+offs]>=0) return status[cur_pos][target+offset];
        if(status[cur_pos].count(target+offset)) return status[cur_pos][target+offset];
        int res=0;
        res+=helper(nums,target-nums[cur_pos],cur_pos+1,status,offset);//it overflows target+/-num
        res+=helper(nums,target+nums[cur_pos],cur_pos+1,status,offset);
        status[cur_pos][target+offset]=res;
        return res;
    }
```    

863. All Nodes Distance K in Binary Tree
given a node find all nodes in distance K
One method: build a graph and bfs. To build the graph, the tree parent needs to be passed in recursive traversal.

394. Decode String
stack, recursion problem.
The subproblem is:
the first non-repeated string
the repeat number [
the to be repeated string (subproblem since it may contain [] again)
]

This effectively avoid the problem to do it first in the stack and expand the string which make the problem really complicated.
```cpp
    string decodeString(const string& s, int& i) {
        string res;
        while (i < s.length() && s[i] != ']') 
        {
            if (!isdigit(s[i])) res += s[i++];
            else 
            {
                int n = 0;
                while (i < s.length() && isdigit(s[i])) n = n * 10 + s[i++] - '0';
                i++; // '['
                string t = decodeString(s, i);
                i++; // ']'
                while (n-- > 0) res += t;
            }
        }
        return res;
    }
```

934. Shortest Bridge
two islands, the shortest bridge (4 direction connection)
dfs to two parties and calculate the mutual distance and get the min
note the distance is not sqrt distance

802. Find Eventual Safe States
given a directed graph, starting from some node if we can reach a terminal node (not a cycle)
the node is called safe node, return all the safe nodes.
so: a node start, it must goes to a terminal for all paths. If any path fails (cycle) it is not a safe node.
if the path contains any non-safe node, it is not safe either.

visited can also use coloring, they are equivalent, but coloring is more efficient.
```cpp
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        //dfs to see if it can walk to a terminal (all paths)
        vector<int> ans;
        vector<int> safenodes(graph.size(),-1);
        for(int i=0;i<graph.size();i++)
        {
            unordered_set<int> visited;
            if(isSafeNode(graph,i,visited,safenodes)) ans.push_back(i);
        }
        return ans;
    }
    
    bool isSafeNode(vector<vector<int>>& graph,int start,unordered_set<int> visited,vector<int>& safenodes)
    {
        //all paths reaches to terminal. If one case fail, then fail
        //failed case: we had a cycle
        //we need add memoization 
        if(safenodes[start]>-1) return safenodes[start];
        if(graph[start].size()==0) {return 1;} //cannot mark it as safe if only one path is safe
        if(visited.count(start)) {return 0;} //
        for(int i=0;i<graph[start].size();i++)
        {
            visited.insert(start);
            if(!isSafeNode(graph,graph[start][i],visited,safenodes)) 
            {
                //all visited nodes shall mark as not safe
                safenodes[start]=0;
                return 0;
            }
            visited.erase(start);
        }
        safenodes[start]=1;
        return 1;
    }
```

785. Is Graph Bipartite?
edge only occurs between different parties but not inside.
two coloring: a->b we color a as 0 and b as 1, then b->c we color c as 1... if we found no conflicts it is two parites.

0: Haven't been colored yet.
1: Blue.
-1: Red.
For each node,

If it hasn't been colored, use a color to color it. Then use the other color to color all its adjacent nodes (DFS).
If it has been colored, check if the current color is the same as the color that is going to be used to color it.
```cpp
    bool isBipartite(vector<vector<int>>& graph) {
        //use coloring 0: not color, 1, -1
        int n=graph.size(); //num of nodes
        vector<int> color(n); 
        for(int i=0;i<n;i++)
        {
            if(!color[i] && !dfs(graph,i,color,1)) return 0;
        }
        return 1;
    }
    bool dfs(vector<vector<int>>& graph,int i,vector<int>& color,int nc)
    {
        if(color[i]) return color[i]==nc;
        color[i]=nc;
        for(int j=0;j<graph[i].size();j++)
        {
            if(!dfs(graph,graph[i][j],color,-nc)) return 0; //adjacent reverse color
        }
        return 1;
    }
```

491. Increasing Subsequences
typical backtracking problem

```cpp
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> myv;
        helper(nums,0,ans,myv);//no need loop here, often made mistakes here

        return ans;
    }
    
    void helper(vector<int>& nums,int ind,vector<vector<int>>& res,vector<int>& v)
    {
        if(v.size()>1) res.push_back(v);
        unordered_set<int> visited;
        for(int i=ind;i<nums.size();i++)
        {
            if((!v.size() || nums[i]>=v.back()) && !visited.count(nums[i]))
            {
                v.push_back(nums[i]);
                helper(nums,i+1,res,v);//i+1 instead ind+1
                v.pop_back();
                visited.insert(nums[i]);
            }
        }
    }
```
use reference to push and pop the last element and try new path

129. Sum Root to Leaf Numbers
each path represents a number and add all path
backtracking
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
using value passing so no need to push/pop

114. Flatten Binary Tree to Linked List
arrange the tree in preorder
inplace: we can do different order.
we have two subproblem, flatten the left and flatten the right and then connect using the root as root->left->right
The linked list generally uses the reverse sequence right->left->root
ie. we first flatten the right subtree, then left subtree, finally the root, root->right connects the right first and now the root becomes the top node. The top node is null first.
```cpp
    void flatten(TreeNode* root) {
        TreeNode* prev=0;
        helper(root,prev);
    }
    void helper(TreeNode* root, TreeNode*& prev)
    {
        if(!root) return;
        helper(root->right,prev);
        helper(root->left,prev);
        root->right=prev;
        root->left=0;
        prev=root;
    }
```
we have to use reference passing since the change of prev shall carry to the next step.

886. Possible Bipartition
disliked person cannot be in the same group
use coloring. three state 0: not assigned, 1/-1
build the input to ajacent matrix.

```cpp
    bool possibleBipartition(int N, vector<vector<int>>& dislikes) {
        vector<int> color(N);
        vector<vector<int>> adj(N);
        for(int i=0;i<dislikes.size();i++)
        {
            adj[dislikes[i][0]-1].push_back(dislikes[i][1]-1);
            adj[dislikes[i][1]-1].push_back(dislikes[i][0]-1);
        }
        for(int i=0;i<N;i++)
        {
            if(!color[i] && !dfs(adj,i,color,1)) return 0;
        }
        return 1;
    }
    bool dfs(vector<vector<int>>& adj,int i,vector<int>& color,int nc)
    {
        if(color[i]) return nc==color[i];
        color[i]=nc;
        for(int j=0;j<adj[i].size();j++) //adj[i] could be empty
        {
            int node=adj[i][j];
            if(color[node] && color[node]!=-nc) return 0;
            if(!color[node] && !dfs(adj,node,color,-nc)) return 0;
        }
        return 1;
    }
```
Note: the precheck color[node]!=-nc is critical: since the following condition only checks non-set colors. if already set, we need check if it is the color we expected. otherwise it is 0. If it is same color, we need continue checking (cannot return)

200. Number of Islands
disjoint set also ok
4 direction. use coloring and dfs
```cpp
    int numIslands(vector<vector<char>>& grid) {
        int ans=0;
        for(int i=0;i<grid.size();i++)
        {
            for(int j=0;j<grid[0].size();j++)
            {
                if(grid[i][j]=='1') dfs(grid,i,j),ans++;
            }
        }
        return ans;
    }
    void dfs(vector<vector<char>>& grid,int i,int j)
    {
        if(i<0 ||j<0 ||i>=grid.size()||j>=grid[0].size()) return;
        if(grid[i][j]=='1')
        {
            grid[i][j]='2';
            dfs(grid,i-1,j);
            dfs(grid,i+1,j);
            dfs(grid,i,j-1);
            dfs(grid,i,j+1);
        }
    }
```

473. Matchsticks to Square
using sticks with different length to form a square
You have to use all the sticks once

first calculate the side length, which is the target sum.
dfs to get the target
```cpp
    bool dfs(vector<int>& sticks,int target,int start_ind,unordered_set<int>& visited)
    {
        if(target==0) return 1;
        if(target<0) return 0;
        for(int i=start_ind;i<sticks.size();i++)
        {
            if(visited.count(i)) continue;
            visited.insert(i);
            if(dfs(sticks,target-sticks[i],i+1,visited)) return 1;
            visited.erase(i); //put them back if failed
        }
        return 0;
    }
```
if path fail, we need pop back the node visited.

542. 01 Matrix
find the nearest distance to 0 for each cell
bfs for each non zero cell.

576. Out of Boundary Paths
recursive or dp

332. Reconstruct Itinerary
The nice thing about DFS is it tries a path, and if that's wrong (i.e. path does not lead to solution), DFS goes one step back and tries another path. It continues to do so until we've found the correct path (which leads to the solution). You need to always bear this nice feature in mind when utilizing DFS to solve problems.

In this problem, the path we are going to find is an itinerary which:

uses all tickets to travel among airports
preferably in ascending lexical order of airport code
Keep in mind that requirement 1 must be satisfied before we consider 2. If we always choose the airport with the smallest lexical order, this would lead to a perfectly lexical-ordered itinerary, but pay attention that when doing so, there can be a "dead end" somewhere in the tickets such that we are not able visit all airports (or we can't use all our tickets), which is bad because it fails to satisfy requirement 1 of this problem. Thus we need to take a step back and try other possible airports, which might not give us a perfectly ordered solution, but will use all tickets and cover all airports.

Thus it's natural to think about the "backtracking" feature of DFS. We start by building a graph and then sorting vertices in the adjacency list so that when we traverse the graph later, we can guarantee the lexical order of the itinerary can be as good as possible. When we have generated an itinerary, we check if we have used all our airline tickets. If not, we revert the change and try another ticket. We keep trying until we have used all our tickets.

good to know what backtracking is: try a solution, if failure, abandom it and try next.

Adjacent matrix: always sort the nodes (using multiset can sort it automatically)
```cpp
    bool dfs(unordered_map<string,multiset<string>>& mp,string node,multiset<pair<string,string>>& to_visit,vector<string>& ans)
    {
        if(to_visit.size()==0) return 1;//visited all
        if(mp[node].size()==0) return 0;//dead end
        
        for(auto it=mp[node].begin();it!=mp[node].end();it++)
        {
            string next=*it;
            pair<string,string> p=make_pair(node,next);
            auto it1=to_visit.find(p);
            if(it1!=to_visit.end())
            {
                to_visit.erase(it1);//remove this edge
                ans.push_back(next);
                if(dfs(mp,next,to_visit,ans)) {return 1;}
                to_visit.insert(p);//put back this edge if failed
                ans.pop_back();
            }
        }
        return 0;
    }
```    

133. Clone Graph
when clone the graph, those pointers are invalidated and they shall point to new nodes
mapping pointers to id, id to pointer then we can rebuild the relation

98. Validate Binary Search Tree
straightforward: in order traversal and see if they are sorted. Or we do not need extra space for this.
must be surrounded by x, boundary o does not count.
union find, by connecting all boundary cells to a parent.
dfs: we only need to find those regions attached to the 4 boundaries and they shall not change. other nodes all change to x

753. Cracking the Safe
n boxes with n digits password using k digits only (from 0 to k-1)
the min length of input to guarantee opening it. (auto match the password)
the string shall contain all possible combinations of the k digits
and we shall combine these combinations with the largest common. 123, 231 shall be combined as 1231
Note each digit can be repeated: so the total combination is k^n (k max is 10, n max is 4, k^n max is 4096)
greedy choice: 
two digits: 00, 01, 11, 10-->00110. every time only the last n-1 digits are reserved and append a new digit in reversed way, ie. from k-1 to 0
if we try from 0 to k-1: 00, 01, 10, 11-->001011, which is 6, not the smallest one. 
if a cycle is seen, continue next.

# Chapter 12 Hash table

# Chapter 13 heap

# Chapter 14 Disjoint set

# Chapter 15 Backtracking

# Chapter 16 Trie

# Chapter 17 BFS

# Chapter 18 Queue

# Chapter 19 Graph







