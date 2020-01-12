### 1313. Decompress Run-Length Encoded List (*)
problem description is not very clear, however very simple:
```cpp
    vector<int> decompressRLElist(vector<int>& nums) {
        vector<int> ans;
        for(int i=0;i<nums.size();i+=2){
            int a=nums[i],b=nums[i+1];
            while(a--) ans.push_back(b);
        }
        return ans;
    }
```

### 1314. Matrix Block Sum (*)
brutal force is fine. however we can use dp to get the prefix sum and then get block matrix sum
```cpp
    vector<vector<int>> matrixBlockSum(vector<vector<int>>& mat, int K) {
        if(mat.empty()) return {};
        int m=mat.size(),n=mat[0].size();
        vector<vector<int>> ans(m,vector<int>(n));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                for(int k=-K;k<=K;k++){
                    for(int l=-K;l<=K;l++){
                        if(i+k<0||i+k>=m||j+l<0||j+l>=n) continue;
                        ans[i][j]+=mat[i+k][j+l];
                    }
                }
            }
        }
        return ans;
    }
```

### 1315. Sum of Nodes with Even-Valued Grandparent (*)
preorder traversal, simple
```cpp
    int sumEvenGrandparent(TreeNode* root) {
        //preorder
        int ans=0;
        preorder(root,0,0,ans);
        return ans;
    }
    void preorder(TreeNode* root,TreeNode* p,TreeNode* pp,int& ans){
        if(!root) return;
        if(pp && pp->val%2==0) ans+=root->val;
        preorder(root->left,root,p,ans);
        preorder(root->right,root,p,ans);
    }
```

### 1316. Distinct Echo Substrings (**)
problem is not well described. It asks for substring which is AA format.
sliding window is fine 
but rolling hash is another option.
```cpp
    int distinctEchoSubstrings(string text) {
        //substring which is repeat.
        //we can use a window to see
        //rolling hash?
        //problem is not clear, it means two identical parts
        unordered_set<string> ms;
        for(int i=0;i<text.size();i++){
            for(int j=i;2*j+1-i<text.size();j++){
                //check substr(i,j-i+1)
                if(text[i]!=text[j+1]) continue;
                if(text.substr(i,j-i+1)==text.substr(j+1,j-i+1))
                    ms.insert(text.substr(i,j-i+1));
            }
        }
        return ms.size();
    }
```
- note if do not check the first char will get TLE since it avoids the substr construction work.
This contest is simple, but problem is not clear in Q1 and Q4.



