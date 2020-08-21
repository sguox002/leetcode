## contest 118


969. Pancake Sorting.md

### Problem Summary
sorting the array by reverse the first k elements

### Approach
greedy: always move the biggest to the first and move to its sorted position
do not need to consider the special case when it is already in the first position
reverse to find the largest: the input is 1 to n, using this fact to make it more easier.

### code
```cpp
    vector<int> pancakeSort(vector<int> A) {
        vector<int> res;
        int x,i;
        for (x = A.size(); x > 0; --x) {
            for (i = 0; A[i] != x; ++i);
            reverse(A.begin(), A.begin() + i + 1);
            res.push_back(i + 1);
            reverse(A.begin(), A.begin() + x);
            res.push_back(x);
        }
        return res;
    }
```


970. Powerful Integers.md

### Problem Summary
given x and y, return all the number with x^i+y^j<=bound

### approach
just iterate all numbers
i<=log(bound-1)/log(x), j<=log(bound-1)/log(y)
edge case:
bound<2
x or y=1

### code
```cpp
    vector<int> powerfulIntegers(int x, int y, int bound) {
        if(bound<2) return vector<int>();
        if(x==1 && y==1) return vector<int>({2});
        if(x==1 || y==1) //bound-1=y^i
        {
            vector<int> ans;
            int xx=1;
            x=max(x,y);
            for(int i=0;i<=log(bound-1)/log(y);i++)
            {
                if(i) xx*=x;
                ans.push_back(xx+1);   
            }
            return ans;
        }
        int m=log(bound-1)/log(x),n=log(bound-1)/log(y);
        unordered_set<int> ans;
        int xx=1,yy=1;
        for(int i=0;i<=m;i++)
        {
            if(i) xx*=x;
            yy=1;
            for(int j=0;j<=n;j++)
            {
                if(j) yy*=y;
                if(xx+yy<=bound) ans.insert(xx+yy);
            }
        }
        return vector<int>(ans.begin(),ans.end());
    }
```

### comments
- we can break out when xx+yy>bound
- need to use hashset to remove duplicates

971. Flip Binary Tree To Match Preorder Traversal.md

### Problem Summary
given preorder traversal and an array of permutation. check if it is ok to flip left and right child to get the array

### approach
Match the array element by element using preorder traversal. If cannot match, try postorder traversal

### code
```cpp
    vector<int> flipMatchVoyage(TreeNode* root, vector<int>& voyage) {
        vector<int> ans;
        int res=helper(root,voyage,0,ans);
        if(!res) return vector<int>({-1});
        return ans;
    }
    int helper(TreeNode* root, vector<int>& voyage,int ind,vector<int>& ans)
    {
        if(!root) return ind;
        if(root->val!=voyage[ind]) {return 0;}
        //see if we need flip its left and right
        //switch or not switch
        int res=helper(root->left,voyage,ind+1,ans);
        
        if(!res) //first left failure, we try to switch
        {
            res=helper(root->right,voyage,ind+1,ans);
            if(!res) return 0;
            res=helper(root->left,voyage,res,ans);
            if(!res) return 0;
            ans.push_back(root->val);
        }
        else 
        {
            res=helper(root->right,voyage,res,ans);
            if(!res) return 0;
        }
        return res;
    }
```

972. Equal Rational Numbers.md

### Problem Summary
Given two strings representing two rational number, check if they are equal

### Approach
translate the number into a double with all the precision
and then compare using string or double.
0.9999999 and 1 will be missed if using string.

### code
```cpp
    bool isRationalEqual(string S, string T) {
        return f(S) == f(T);
    }

    double f(string S) {
        auto i = S.find("(");
        if (i != string::npos) {
            string base = S.substr(0, i);
            string rep = S.substr(i + 1, S.length() - i - 2);
            for (int j = 0; j < 20; ++j) base += rep;
            return stod(base);
        }
        return stod(S);
    }
```
