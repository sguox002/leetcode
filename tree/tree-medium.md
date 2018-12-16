654. Maximum Binary Tree
Given an integer array with no duplicates. A maximum tree building on this array is defined as follow:
The root is the maximum number in the array.
The left subtree is the maximum tree constructed from left part subarray divided by the maximum number.
The right subtree is the maximum tree constructed from right part subarray divided by the maximum number.
Construct the maximum tree by the given array and output the root node of this tree.

recursively find the max and divide into left and right

701. Insert into a Binary Search Tree
the value > root, goes to right, otherwise goes to left
create a leaf node.
this shall be easy

814. Binary Tree Pruning
all 0 and 1, prune all subtree not containing 1
we can use the sum=0 or not 0 to prune the left or right.
or recursively prune the leaf nodes

894. All Possible Full Binary Trees (**)
all node is 0, return all possible full binary tree
for a node: we have 0 or 2 children until n node is exhausted.
again we have two recursion:
n nodes, i for left and n-i for right, which is a n combinations of subproblem
for each left m, and right n nodes, there are mxn combinations

951. Flip Equivalent Binary Trees
flip equivalent, also used for scrambling the string
flip left and right
left1=left2 and right1=right2
left1=right1 and right1=left2

513. Find Bottom Left Tree Value
Given a binary tree, find the leftmost value in the last row of the tree.
It is easy to give the tree a x and y coordinate.
another: use the height, if left is taller, find it in left, otherwise in right
The last row has a depth of 1

515. Find Largest Value in Each Tree Row
bfs or hashmap using layer while traverse

889. Construct Binary Tree from Preorder and Postorder Traversal (**)
two arrays
pre: root left right
post: left right root
pick the first as the root from pre. then if we can split the array into left and right, each is then a subproblem
For two subarrays pre[a,b] and post[c,d], if we want to reconstruct a tree from them, we know that pre[a]==post[d] is the root node.

[root][......left......][...right..]  ---pre
[......left......][...right..][root]  ---post
pre[a+1] is the root node of the left subtree.
Find the index of pre[a+1] in post, then we know the left subtree should be constructed from pre[a+1, a+idx-c+1] and post[c, idx]

919. Complete Binary Tree Inserter
A complete binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible.
complete binary tree can use array, and each one has strict index 2*n+1 and 2*n+2 for parent node n


