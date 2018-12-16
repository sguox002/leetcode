### easy
589. N-ary Tree Preorder Traversal-recursive
590. N-ary Tree Postorder Traversal: recursive
617. Merge Two Binary Trees: recursive
700. Search in a Binary Search Tree: recursive
559. Maximum Depth of N-ary Tree
depth=1+max(children depth), for the node, it is depth=1. (otherwise will be correct)
872. Leaf-Similar Trees
recursively get the leaf and construct a string
897. Increasing Order Search Tree
similar to convert the tree in order
669. Trim a Binary Search Tree
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
104. Maximum Depth of Binary Tree..same as 559
637. Average of Levels in Binary Tree
using a map to record the level or
level traversal
429. N-ary Tree Level Order Traversal: bfs
be sure to deal with null

226. Invert Binary Tree
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
653. Two Sum IV - Input is a BST
traversal with set/hashset
606. Construct String from Binary Tree
need remove unnecessary (), preorder traversal
538. Convert BST to Greater Tree (**)
Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.
assuming we have in order array, the question is pretty simple: it is the postfix sum of all its behind numbers
trying a reverse in-order traverse and do the sum in place

404. Sum of Left Leaves
two: find leaves, and must be left, so we can flag if it is left or right
or using root->left->left, root->right->left

100. Same Tree
compare its subtree and the root

108. Convert Sorted Array to Binary Search Tree
need to be a balanced tree. 
split the array in balanced way and recursively build the subtree

563. Binary Tree Tilt (**)
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
543. Diameter of Binary Tree (**)
diameter is the longest distance between two nodes
again this is not so straightforward.
- if two node need go through the root, ldepth+rdepth (a node's depth problem)
- if not go through root, choose lmax and rmax (a node's diameter subproblem)

107. Binary Tree Level Order Traversal II
from bottom up: we can still use up down and reverse

257. Binary Tree Paths
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
                path.pop_back();
                path.pop_back();
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

671. Second Minimum Node In a Binary Tree
Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly two or zero sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes.
Given such a binary tree, you need to output the second minimum value in the set made of all the nodes' value in the whole tree.
If no such second minimum value exists, output -1 instead.
traversal and put in a min heap. (does not use the fact provided in the question)
the root is the min, we just need to find the first one who is bigger than the root.

235. Lowest Common Ancestor of a Binary Search Tree
R>node>L, the node is the parent
node>R, goes to left
node<L: goes to right

101. Symmetric Tree (**)
note: it only needs left and right subtree under root to be symmetric
check if node is the same and left's left=right's right,  and left's right=right's left and this is recursively true

437. Path Sum III (**)
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

572. Subtree of Another Tree
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

110. Balanced Binary Tree
check if tree left and right height difference <=1
one recursive: the height of a tree

501. Find Mode in Binary Search Tree
most frequency element, allows duplicate
left <=node, right>=node
in order traversal will give the number of duplicate number
using a map to record element vs frequency

112. Path Sum
from root to leaf sum to target
dfs 

111. Minimum Depth of Binary Tree
dfs to get the shortest path

687. Longest Univalue Path
the path may or may not go through the root. (distance=number of edges)
again two recursive problem
- in the left subtree
- in the right subtree
- with the root involved: left=root=right, then add left side length and right side length
```cpp
    int longestUnivaluePath(TreeNode* root) {
        if(!root) return 0;
        int maxleft=0,maxright=0,maxlen=0;
        if(root->left) maxleft=longestUnivaluePath(root->left);//+(root->val==root->left->val);
        if(root->right) maxright=longestUnivaluePath(root->right);//+(root->val==root->right->val);
        if(root->left && root->right && root->val==root->left->val && root->val==root->right->val)
        maxlen=helper(root->left,root->val)+helper(root->right,root->val);
        return max(maxlen,max(maxleft,maxright));
    }
    int helper(TreeNode* root,int val)
    {
        if(!root || root->val!=val) return 0;
        //if(root->val==val) return 1;
        return 1+max(helper(root->left,val),helper(root->right,val));
    }
```






