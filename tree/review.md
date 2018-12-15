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








