## contest 122

985. Sum of Even Numbers After Queries.md

We have an array A of integers, and an array queries of queries.

For the i-th query val = queries[i][0], index = queries[i][1], we add val to A[index].  Then, the answer to the i-th query is the sum of the even values of A.

(Here, the given index = queries[i][1] is a 0-based index, and each query permanently modifies the array A.)

Return the answer to all queries.  Your answer array should have answer[i] as the answer to the i-th query.

 
 Approach
 Sraightforward: 
 
 ```cpp
     vector<int> sumEvenAfterQueries(vector<int>& A, vector<vector<int>>& queries) {
        int sum=0;
        vector<int> ans(queries.size());
        for(int i=0;i<A.size();i++) if(A[i]%2==0) sum+=A[i];
        for(int i=0;i<queries.size();i++)
        {
            int t=A[queries[i][1]]+queries[i][0];
            int ind=queries[i][1];
            if(t%2==0) //even
            {
                if(A[ind]%2) sum+=t;
                else sum+=queries[i][0];
            }
            else //odd
            {
                if(A[ind]%2==0) sum-=A[ind];
            }
            A[ind]=t;
            ans[i]=sum;
        }
        return ans;
    }
```    
986. Interval List Intersections.md

### Problem Summary
to return the intersection of two sorted list of intervals

### Approach
two pointers

```cpp
    vector<Interval> intervalIntersection(vector<Interval>& A, vector<Interval>& B) {
        int m=A.size(),n=B.size();
        vector<Interval> ans;
        int i=0,j=0;//two pointers
        while(i<m && j<n) //either is done
        {
            while(A[i].end<B[j].start) i++;
            while(B[j].end<A[i].start) j++;
            if(i<m && j<n)
            {
              int start=max(A[i].start,B[j].start);
              int end=min(A[i].end,B[j].end);
              if(start<=end) ans.push_back(Interval(start,end));
              if(A[i].end<B[j].end) i++;
              else if(A[i].end>B[j].end) j++;
              else i++,j++;
            }
        }
        return ans;
    }
```    

the above solution: 
1. make sure i, j not over bound
2. make sure start<end

987. Vertical Order Traversal of a Binary Tree.md

Return the vertical order traversal

The only thing: there will be node overlaps, and they shall be sorted

Direct Approach
traversal and get the x, y and val, and then sort

```cpp
struct node {
    int x,y,val;
    node(int x1,int y1,int v):x(x1),y(y1),val(v){}
};
bool cmp(node& a,node& b) {return a.x<b.x || (a.x==b.x && a.y<b.y) || (a.x==b.x && a.y==b.y && a.val<b.val);}
class Solution {
public:
    vector<node> vn;
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        inorder(root,0,0);
        sort(vn.begin(),vn.end(),cmp);
        vector<vector<int>> ans;
        int prev=INT_MIN;
        for(int i=0;i<vn.size();i++)
        {
            if(vn[i].x!=prev) {ans.push_back({vn[i].val});prev=vn[i].x;}
            else ans.back().push_back(vn[i].val);
        }
        return ans;
        
    }
    void inorder(TreeNode* root,int x,int y)
    {
        if(!root) return;
        inorder(root->left,x-1,y+1);
        vn.push_back(node(x,y,root->val));
        inorder(root->right,x+1,y+1);
    }
};
```
988. Smallest String Starting From Leaf.md

### Problem Summary
return the smallest string from leaf to root

### Approach
from root to leaf and dfs to get the string

```cpp
    string smallestFromLeaf(TreeNode* root) {
        //dfs to get the string
        string ans,t;
        dfs(root,ans,t);
        return ans;
    }
    void dfs(TreeNode* root,string& ans,string t)
    {
        t+=root->val+'a';
        if(root->left) dfs(root->left,ans,t);
        if(root->right) dfs(root->right,ans,t);
        if(!root->left && !root->right)
        {
            reverse(t.begin(),t.end());
            if(ans.size())ans=min(ans,t);else ans=t;
        }
    }
```    