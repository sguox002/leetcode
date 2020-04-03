## contest 181

### 1380. Lucky Numbers in a Matrix
<em>
Given a m * n matrix of distinct numbers, return all lucky numbers in the matrix in any order.

A lucky number is an element of the matrix such that it is the minimum element in its row and maximum in its column.
</em>

- we can use number directly since there is no duplicates
- using hashmap

```cpp
    vector<int> luckyNumbers (vector<vector<int>>& matrix) {
        vector<int> ans;
        unordered_set<int> mn;
        int m=matrix.size(),n=matrix[0].size();
        for(int i=0;i<matrix.size();i++){
            int ind=*min_element(begin(matrix[i]),end(matrix[i]));//-begin(matrix[i]);
            mn.insert(ind);
            //cout<<matrix[i][ind]<<endl;
        }
        for(int i=0;i<matrix[0].size();i++){
            int ind=0;
            int max0=matrix[0][i];
            for(int j=1;j<matrix.size();j++){
                if(matrix[j][i]>max0) {
                    max0=matrix[j][i];
                    ind=i+j*n;
                }
            }   
            //cout<<max0<<endl;
            if(mn.count(max0)) ans.push_back(max0);
                        
        }
        return ans;
    }
```

### 1381. Design a Stack With Increment Operation
<em>
Design a stack which supports the following operations.

Implement the CustomStack class:

- CustomStack(int maxSize) Initializes the object with maxSize which is the maximum number of elements in the stack or do nothing if the stack reached the maxSize.
- void push(int x) Adds x to the top of the stack if the stack hasn't reached the maxSize.
- int pop() Pops and returns the top of stack or -1 if the stack is empty.
void inc(int k, int val) Increments the bottom k elements of the stack by val. If there are less than k elements in the stack, just increment all the elements in the stack.
</em>	

use two stack approach, not optimized:

```cpp
    stack<int> st1,st2;
    int size;
    CustomStack(int maxSize) {
        size=maxSize;
    }
    
    void push(int x) {
        if(st1.size()<size){
            st1.push(x);
        }
    }
    
    int pop() {
        if(st1.size()){
            int x=st1.top();
            st1.pop();
            return x;
        }
        return -1;
    }
    
    void increment(int k, int val) {
        while(st1.size()){
            int x=st1.top();
            st1.pop();
            st2.push(x);
        }
        while(st2.size() && k--){
            st1.push(st2.top()+val);
            st2.pop();
        }
        while(st2.size()) {
            st1.push(st2.top());
            st2.pop();
        }
    }
```

### 1382. Balance a Binary Search Tree
<em>
Given a binary search tree, return a balanced binary search tree with the same node values.

A binary search tree is balanced if and only if the depth of the two subtrees of every node never differ by more than 1.

If there is more than one answer, return any of them.
</em>

- reoganize the tree
- convert to sorted array and then convert to balanced BST.

```cpp
    TreeNode* balanceBST(TreeNode* root) {
        //put in an array and recursively balance it
        vector<int> arr;
        inorder(root,arr);
        int n=arr.size();
        return build(arr,0,n);
    }
    void inorder(TreeNode* root,vector<int>& arr){
        if(!root) return;
        inorder(root->left,arr);
        arr.push_back(root->val);
        inorder(root->right,arr);
    }
    TreeNode* build(vector<int>& arr,int l,int r){
        if(l>=r) return 0;
        int m=(l+r)/2;
        TreeNode* root=new TreeNode(arr[m]);
        
        root->left=build(arr,l,m);
        root->right=build(arr,m+1,r);
        return root;
    }
```

### 1383. Maximum Performance of a Team
<em>
There are n engineers numbered from 1 to n and two arrays: speed and efficiency, where speed[i] and efficiency[i] represent the speed and efficiency for the i-th engineer respectively. Return the maximum performance of a team composed of at most k engineers, since the answer can be a huge number, return this modulo 10^9 + 7.

The performance of a team is the sum of their engineers' speeds multiplied by the minimum efficiency among their engineers. 
</em>

- this is a very good question.
- bind the speed and efficiency.
- sort according to efficiency in decreasing order, so we know next will be lower efficiency person
- add k person into heap (sorted by speed)
- add one person into heap and pop the one with min speed.
- the key part: we need to get the max every time we add a person to the min heap.

```cpp
    int maxPerformance(int n, vector<int>& speed, vector<int>& efficiency, int k) {
        priority_queue<int, vector<int>, greater<int>> heap;
        vector<vector<int>> worker;
        vector<int> tmp(2,0);
        for (int i = 0; i < n; i++) {
            tmp[0] = speed[i];
            tmp[1] = efficiency[i];
            worker.push_back(tmp);
        }
        sort(worker.begin(), worker.end(), compare);
        long res = 0;
        long total = 0;
        long minE;
        for (int i = 0; i < k; i++) {
            total += worker[i][0];
            minE = worker[i][1];
            res = max(res, total*minE);
            heap.push(worker[i][0]);
        }
        for (int i = k; i < n; i++) {
            if (worker[i][0] > heap.top()) {
                total += (-heap.top()+worker[i][0]);
                minE = worker[i][1];
                res = max(res, total*minE);
                heap.pop();
                heap.push(worker[i][0]);
            }
        }
        return (int)(res%1000000007);
    }
```	

