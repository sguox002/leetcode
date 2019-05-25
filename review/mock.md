Mocks

online 1

Design Hashmap
using vector of list for hashmap chaining.

```cpp
    typedef pair<int,int> pii;
    vector<list<pii>> hash;
    const int sz=1000;
    MyHashMap() {
        hash.resize(sz);
    }
    
    /** value will always be non-negative. */
    void put(int key, int value) {
        int ind=key%sz;
    
        bool found=0;
        for(auto &it: hash[ind])
            if(it.first==key) {it.second=value;found=1;}
        if(!found) hash[ind].push_back({key,value});

    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int ind=key%sz;
        for(auto it:hash[ind])
            if(it.first==key) return it.second;
        return -1;
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int ind=key%sz;
        for(auto it=hash[ind].begin();it!=hash[ind].end();it++)
            if(it->first==key) {hash[ind].erase(it);break;}
    }
```	

binary tree inorder traversal
iteration solution
```cpp
	vector<int> inorderTraversal(TreeNode* root) {
		stack<TreeNode*> st;
        vector<int> ans;
		if(!root) return ans;
		//left, node,right so we need push right,node,left
		if(root->right) st.push(root->right);
		st.push(root);
		while(st.size())
		{
			if(root->left) st.push(root->left);
			else root=st.top(),st.pop();
			ans.push_back(root->val);
			root=root->left;
		}
		return ans;
	}
```
left root right, so in stack we shall push right, root, left.

online 2:
palindrome number: trivial
add strings: trivial

online 3:
remove element: two pointer, trivial
combination sum:
typical backtracking:
```cpp
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        
        set<vector<int>> ms;
        dfs(candidates,{},ms,target);
        vector<vector<int>> ans={ms.begin(),ms.end()};
        return ans;
    }
    void dfs(vector<int>& num,vector<int> vt,set<vector<int>>& res,int target)
    {
        if(target<0) return;
        if(target==0) {sort(vt.begin(),vt.end());res.insert(vt);return;}
        for(int t: num)
        {
            vt.push_back(t);
            dfs(num,vt,res,target-t);
            vt.pop_back();
        }
    }
```
this use set to avoid duplicates, and sort the vector
but it is not the best:
```cpp
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        
        vector<vector<int>> ans;
        dfs(candidates,{},ans,target,0);
        return ans;
    }
    void dfs(vector<int>& num,vector<int> vt,vector<vector<int>>& res,int target,int ind)
    {
        if(target<0) return;
        if(target==0) {res.push_back(vt);return;}
        for(int i=ind;i<num.size();i++)
        {
            vt.push_back(num[i]);
            dfs(num,vt,res,target-num[i],i);
            vt.pop_back();
        }
    }
```	
this can eliminate the duplicates
for example [2,3,5] target=8
first search position 0: 2,2,2,2
we pop one 2: fail
we pop one 2: 2,2, fail
we pop one 2: 2+3+3=8
then we process to 3, and discarded all previous numbers, that's why we don't have duplicates
choose 2: we have 2+2+2+2, 2+3+3
choose 3: 3+5
choose 5: none

online 3:
find pivot index: left sum=right sum, trivial
linked list random node
using vector to store the list is easy, but does not satisfy the O(1) requirement
you cannot iterate the whole list since the list is very long
```cpp
    ListNode *head;
    default_random_engine rand_gen;
    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    Solution(ListNode* head) {
        this->head=head;
    }
    
    /** Returns a random node's value. */
    int getRandom() {
		ListNode* p = head;
		int k = 1;
		int v = p->val;
		while(p->next) {
			std::uniform_int_distribution<int> distribution(0,k);
			if(distribution(rand_gen)==0) {v = p->next->val;}
			p = p->next;
			k++;
		}
		return v;
    }
```	
this uses the c++17 random engine
use the low quality c++99 rand
```cpp
    ListNode* head;
    /** @param head The linked list's head. Note that the head is guanranteed to be not null, so it contains at least one node. */
    Solution(ListNode* head) {
        this->head = head;
    }
    
    /** Returns a random node's value. */
    int getRandom() {
        int res = head->val;
        ListNode* node = head->next;
        int i = 2;
        while(node){
            int j = rand()%i;
            if(j==0)
                res = node->val;
            i++;
            node = node->next;
        }
        return res;
    }
```
this is based on the math problem
for i numbers, a number chosen probability is 1/i
the total probability is 1/2*2/3*3/4*....*(n-1)/n=1/n.
Reservoir sampling is a family of randomized algorithms for randomly choosing a sample of {\displaystyle k} k items from a list {\displaystyle S} S containing {\displaystyle n} n items, where {\displaystyle n} n is either a very large or unknown number. Typically, {\displaystyle n} n is too large to fit the whole list into main memory
Suppose we see a sequence of items, one at a time. We want to keep ten items in memory, and we want them to be selected at random from the sequence. If we know the total number of items n, then the solution is easy: select 10 distinct indices i between 1 and n with equal probability, and keep the i-th elements. The problem is that we do not always know the exact n in advance. A possible solution is the following:

Keep the first ten items in memory.
When the i-th item arrives (for {\displaystyle i>10} {\displaystyle i>10}):
with probability {\displaystyle 10/i} {\displaystyle 10/i}, keep the new item (discard an old one, selecting which to replace at random, each with chance 1/10)
with probability {\displaystyle 1-10/i} {\displaystyle 1-10/i}, keep the old items (ignore the new one)
Thus,

when there are 10 items or fewer, each is kept with probability 1;
when there are 11 items, each of them is kept with probability 10/11; for the old items, that is (1)(1/11 + (10/11)(9/10)) = 1/11 + 9/11 = 10/11
In other words, the 10 old items are kept either if the new one is not selected 1/11 or the new one is selected to replace one of the other 9 items 10/11 × 9/10, and since "or" is represented with addition, we derive (1/11 + (10/11)(9/10)).
when there are 12 items, the twelfth item is kept with probability 10/12, and each of the previous 11 items are also kept with probability (10/11)(2/12 + (10/12)(9/10)) = (10/11)(11/12) = 10/12;
by induction, it is easy to prove that when there are n items, each item is kept with probability {\displaystyle 10/n} {\displaystyle 10/n}.

online 4:
search insert position
using upper bound

friend circles
union find
```cpp
    int findCircleNum(vector<vector<int>>& M) {
       int n=M.size();
        vector<int> parent(n),size(n,1);
        for(int i=0;i<n;i++) parent[i]=i;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(i==j || !M[i][j]) continue;
                if(M[i][j]) merge(parent,i,j,size);
            }
        }
        return accumulate(size.begin(),size.end(),0);
    }
    void merge(vector<int>& parent,int i,int j,vector<int>& size)
    {
        int pi=find_parent(parent,i);
        int pj=find_parent(parent,j);
        if(pi==pj) return;
        if(pi<pj) {parent[pj]=pi;size[pj]=0;}
        else {parent[pi]=pj,size[pi]=0;}
    }
    int find_parent(vector<int>&parent,int i)
    {
        while(parent[i]!=i) i=parent[i];
        return i;
    }
```

online 5:
reverse integer

array partition
min sum
	


Phone 1:
climbing stairs
simple dp

add digits: simple

onsite 1:
linked list cycle
use slow and fast
```cpp
    bool hasCycle(ListNode *head) {
        ListNode *slow=head,*fast=head;
        while(fast && fast->next)
        {
            slow=slow->next;
            fast=fast->next->next;
            if(fast==slow) return 1;
        }
        return 0;
    }
```

delete node in BST
find the root first
root left is empty, then promote the right
root right is empty, then promote the left
else swap the root with the right min
```cpp
    TreeNode* deleteNode(TreeNode* root, int key) {
        if(!root) return 0;
        
        if(root->val==key){ //swap the rightmost node in the left branch with root
            if(!root->left) return root->right;
            if(!root->right) return root->left;
            TreeNode* node=findmin(root->right);
            root->val=node->val;
            root->right=deleteNode(root->right,root->val);
            return root;
        }
        if(key<root->val) root->left=deleteNode(root->left,key);
        else root->right=deleteNode(root->right,key);
        return root;
    }
    TreeNode* findmin(TreeNode* root)
    {
        if(!root) return 0;
        while(root->left) root=root->left;
        return root;
    }
```

basic calculator II
approach: 
add + to the start and end
when we met a + - and can accumulate previous results and followed num add a sign
when we met a * / we perform the item with the followed number

```cpp
    int calculate(string s) {
        stringstream ss('+'+s+'+');
        int res=0;
        int n=0,prev=0;
        char op;
        
        while(ss>>op)
        {
            //ss>>n;
            switch(op)
            {
                case '+':
                case '-':
                    res+=prev; 
                    ss>>prev;
                    if(op=='-') prev*=-1;
                    break;
                case '*':
                    ss>>n;
                    prev*=n;
                    //res+=prev;
                    break;
                case '/':
                    ss>>n;
                    prev/=n;
                    //res+=prev;
                    break;
            }
        }
        return res;
    }
```

onsite 2:
Reverse Words in a String
simple using stringstream

Lowest Common Ancestor of a Binary Tree
find the node using bitset 
```cpp
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(!root) return 0;
        if(root==p || root==q) return root;
        int res1=contains(root->left,p,q);
        int res2=contains(root->right,p,q);
        if(res1 && res2) return root;
        if(res1) return lowestCommonAncestor(root->left,p,q);
        return lowestCommonAncestor(root->right,p,q);
    }
    int contains(TreeNode* root,TreeNode*p, TreeNode* q)
    {
        if(!root) return 0;
        int res=0;
        if(root==p) res|=1;
        if(root==q) res|=2;
        res|=contains(root->left,p,q);
        res|=contains(root->right,p,q);
        return res;
    }
```

Search a 2D Matrix II
row is sorted
col is sorted
simple.

lru cache
use a list and a hashmap of the value vs the list iterator
```cpp
    int maxsize;
    typedef pair<int,int> pii; //key value
    list<pii> lru;
    unordered_map<int,list<pii>::iterator> key2iter;
    
    LRUCache(int capacity) {
        maxsize=capacity;
    }
    
    int get(int key) {
        if(key2iter.count(key)){
            //move it to front
            lru.splice(lru.begin(),lru,key2iter[key]);
            key2iter[key]=lru.begin();
            return key2iter[key]->second;            
        }
        return -1;
    }
    
    void put(int key, int value) {
        if(key2iter.count(key))         //already have the key
        {
            lru.splice(lru.begin(),lru,key2iter[key]);
            key2iter[key]=lru.begin();
            lru.begin()->second=value;            
            return;
        }
        if(lru.size()>=maxsize) 
        {
            auto t=lru.back();
            lru.pop_back(); //remove oldest
            key2iter.erase(t.first);
        }
        lru.push_front({key,value});
        key2iter[key]=lru.begin();        
    }
```

onsite 3:
longest common prefix
simple

rotate image
use 2x2 to find the way
swap reverse diagonal elements
and then reverse rows

container with most water
two pointer
```cpp
    int maxArea(vector<int>& height) {
        //area=min(l,r)*(l-r)
        //to get area larger, we need shrink the range but get higher bars
        //using two pointers
        int n=height.size();
        int i=0,j=n-1;
        int minbar=min(height[0],height.back());
        int ans=minbar*(j-i);
        while(i<j)
        {
            if(height[i]<=minbar) i++;
            if(height[j]<=minbar) j--;
            minbar=min(height[i],height[j]);
            ans=max(ans,minbar*(j-i));
        }
        return ans;
    }
```

	

	


	
	



