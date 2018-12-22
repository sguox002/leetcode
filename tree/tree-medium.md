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

94. Binary Tree Inorder Traversal- iterative

left node right
put the node in stack and goes to left until no left, and then pop, go to the right

865. Smallest Subtree with all the Deepest Nodes

Return the node with the largest depth such that it contains all the deepest nodes in its subtree.
calculate each node's depth

508. Most Frequent Subtree Sum

a subtree sum=root+leftsum+rightsum
create a map

655. Print Binary Tree

a way to display the tree

144. Binary Tree Preorder Traversal-iterative

node, left, right
keep pushing the right into stack

684. Redundant Connection (**)

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

230. Kth Smallest Element in a BST

inorder traversal

623. Add One Row to Tree

add a row of value at the given depth
bfs to find the row with the depth and replace the row and add those original as child

337. House robber III

dynamic with tree

102. Binary Tree Level Order Traversal

queue
or use left first and then right, with layer index, dfs is fine

199. Binary Tree Right Side View

basically it needs the rightmost node for each layer
bfs or dfs to store layer index and the last one in the layer

173. Binary Search Tree Iterator

next: next smallest
has_next:
next smallest is always the leftmost node, so using a stack to store those nodes in inorder sequence

449. Serialize and Deserialize BST

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
863. All Nodes Distance K in Binary Tree

find all nodes with distance to k
one method: convert the tree to an undirectional graph and then using bfs

96. Unique Binary Search Trees

1 to N, how many unique BST
first 1 to n can be the root. Suppose i is the root
and it divides 1 to i-1 in the left and i+1 to n in the right. the total number is m=L*R
this is a dp problem: dp[i]=sum(dp[j-1]*dp[i-j]) j from 1 to i

652. Find Duplicate Subtrees

first we need to know how to judge two trees are equal
then we need compare each nodes (traverse)
The way: convert a tree into a string and store into a hashset or map. Then we traverse and check if the string already appeared.

129. Sum Root to Leaf Numbers

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
114. Flatten Binary Tree to Linked List

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

958 check completeness of a binary tree

level traversal and push all its child into queue including null. Then we found a null, make sure there is no more non-null nodes

103. Binary Tree Zigzag Level Order Traversal

level by level and then reverse all even layers

662. Maximum Width of Binary Tree

the left right largest difference
can use full binary tree array storage

450. Delete Node in a BST

there is standard operations on deleting a node
see some algorithm text coursera

Recursively find the node that has the same value as the key, while setting the left/right nodes equal to the returned subtree
Once the node is found, have to handle the below 4 cases
- node doesn't have left or right - return null
- node only has left subtree- return the left subtree
- node only has right subtree- return the right subtree
- node has both left and right - find the minimum value in the right subtree, set that value to the currently found node, then recursively delete the minimum value in the right subtree

113. Path Sum II

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

105. Construct Binary Tree from Preorder and Inorder Traversal

pre: root left right
in: left root right
get the root from the preorder and divide left and right
do it recursively

106. Construct Binary Tree from Inorder and Postorder Traversal

inorder: left root right
post: left right root
similarly get the root from post and then divide into left and right

116. Populating Next Right Pointers in Each Node

next: the next right node.
using the next we have a mechanism to do level traversal, and for each layer we directly update its children. next is used for this layer's traversal

next is null the layer is done

95. Unique Binary Search Trees II

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

236. Lowest Common Ancestor of a Binary Tree

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
117. Populating Next Right Pointers in Each Node II

tree is not full, in-place

222. Count Complete Tree Nodes

a complete tree depth is the leftmost nodes
previous layers are full and contain 2^h-1 nodes
we can decide which side is not full
and one side is full and otherside is not full, and it is a sub problem

98. Validate Binary Search Tree

inorder traversal to see if it is sorted

145. Binary Tree Postorder Traversal-iterative

left right root
need push root to stack, right and then left. pop them and get post order.

297. Serialize and Deserialize Binary Tree

same as 449?

834. Sum of Distances in Tree

node i to all other nodes distance sum
think in a graph. 
dfs

