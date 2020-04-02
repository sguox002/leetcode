## biweek 21
### 1370. Increasing Decreasing String
<em>
Given a string s. You should re-order the string using the following algorithm:

Pick the smallest character from s and append it to the result.
Pick the smallest character from s which is greater than the last appended character to the result and append it.
Repeat step 2 until you cannot pick more characters.
Pick the largest character from s and append it to the result.
Pick the largest character from s which is smaller than the last appended character to the result and append it.
Repeat step 5 until you cannot pick more characters.
Repeat the steps from 1 to 6 until you pick all characters from s.
In each step, If the smallest or the largest character appears more than once you can choose any occurrence and append it to the result.

Return the result string after sorting s with this algorithm.
</em>

Approach:

use a hashmap and do the simulation

```cpp
    string sortString(string s) {
        vector<int> cnt(26);
        for(char c: s) cnt[c-'a']++;
        string res;
        while(res.size()!=s.size()){
            for(int i=0;i<26;i++){
                if(cnt[i]) res+='a'+i,cnt[i]--;
            }
            for(int i=25;i>=0;i--){
                if(cnt[i]) res+='a'+i,cnt[i]--;
            }
        }
        return res;        
    }
```

### 1371. Find the Longest Substring Containing Vowels in Even Counts
<em>
Given the string s, return the size of the longest substring containing each vowel an even number of times. That is, 'a', 'e', 'i', 'o', and 'u' must appear an even number of times.
</em>

Approach:
- the exact count does not matter. use xor to get 0
- sliding window will not make O(N)
- we can use bit operations, aeiou corresponds to bit 0 to bit 4.
	- we can record the status in hashmap. 
	- keep the smallest index with the same status. and then we can get the longest substring.
	- only has 32 status, so we can use vector to speed up.

```cpp
	int findTheLongestSubstring(string s) {
		unordered_map<int,int> mp; //status vs smallest index
		int cnt=0,ans=0;
		mp[0]=-1; 
		for(int i=0;i<s.size();i++){
			char c=s[i];
			if(c=='a') cnt^=1;
			else if(c=='e') cnt^=2;
			else if(c=='i') cnt^=4;
			else if(c=='o') cnt^=8;
			else if(c=='u') cnt^=16;
			if(mp.count(cnt)) ans=max(ans,i-mp[cnt]);
			else mp[cnt]=i;
		}
		return ans;
	}
```

### 1372. Longest ZigZag Path in a Binary Tree
<em>
Given a binary tree root, a ZigZag path for a binary tree is defined as follow:

Choose any node in the binary tree and a direction (right or left).
If the current direction is right then move to the right child of the current node otherwise move to the left child.
Change the direction from right to left or right to left.
Repeat the second and third step until you can't move in the tree.
Zigzag length is defined as the number of nodes visited - 1. (A single node has a length of 0).

Return the longest ZigZag path contained in that tree.
</em>

Approach:

post order traversal
- it could be in the left or right subtree or connect the root itself.

```cpp
    int longestZigZag(TreeNode* root) {
        //post order and update the max path
        int ans=0;
        auto t=helper(root,ans);
        return ans;
    }
    vector<int> helper(TreeNode* root,int& ans){
        if(!root) return {0,0};
        auto left=helper(root->left,ans);
        auto right=helper(root->right,ans);
        ans=max({ans,left[1],right[0]}); //update the ans
        return {1+left[1],1+right[0]};//include the root
    }
```
	
### 1373. Maximum Sum BST in Binary Tree
<em>
Given a binary tree root, the task is to return the maximum sum of all keys of any sub-tree which is also a Binary Search Tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
</em>

Approach:
- post order traversal
- left and right shall be BST, get the lmax,rmin, sum

```cpp
    int maxSumBST(TreeNode* root) {
        //postorder get the sum, lmax,rmin
        //BST: lmax<root<rmin
        int ans=0;
        //int lmax=-1e5,rmin=1e5;
        helper(root,ans);
        return ans;
    }
    //return sum and if it is a bst
    vector<int> helper(TreeNode* root,int& ans){
        if(!root) return {1,0,-100000,100000}; //it is a bst
        int sum=0,lmax=-100000,rmin=100000;
        auto left=helper(root->left,ans);
        auto right=helper(root->right,ans);
        bool isbst=left[0] && right[0] && root->val>left[2] && root->val<right[3];
        if(isbst){
            sum=root->val+left[1]+right[1];
            ans=max(ans,sum);
            lmax=max(root->val,lmax);
            rmin=min(root->val,rmin);
        }
        return {isbst,sum,lmax,rmin};
    }
```
	
