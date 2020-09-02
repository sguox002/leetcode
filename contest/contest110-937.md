## contest 110

Reorder Data in Log Files4
Range Sum of BST4
Minimum Area Rectangle5
Distinct Subsequences II7

937. Reorder Data in Log Files
<em>
You have an array of logs.  Each log is a space delimited string of words.

For each log, the first word in each log is an alphanumeric identifier.  Then, either:

Each word after the identifier will consist only of lowercase letters, or;
Each word after the identifier will consist only of digits.
We will call these two varieties of logs letter-logs and digit-logs.  It is guaranteed that each log has at least one word after its identifier.

Reorder the logs so that all of the letter-logs come before any digit-log.  The letter-logs are ordered lexicographically ignoring identifier, with the identifier used in case of ties.  The digit-logs should be put in their original order.

Return the final order of the logs.
</em>

approach:
- hashmap

```cpp
    vector<string> reorderLogFiles(vector<string>& logs) {
        map<string,set<string>> ms;
        vector<string> dlogs;
        for(string l: logs){
            int ind=l.find_first_of(' ');
            //while(l[ind]==' ') ind++;
            ind++;
            if(isdigit(l[ind])){
                dlogs.push_back(l);
            }
            else ms[l.substr(ind)].insert(l.substr(0,ind));
        }
        vector<string> ans;
        for(auto t: ms) 
            for(auto s: t.second) ans.push_back(s+t.first);
        for(auto t: dlogs) ans.push_back(t);
        return ans;
    }
```

note need use multimap to pass new tests.

938. Range Sum of BST
<em>
Given the root node of a binary search tree, return the sum of values of all nodes with value between L and R (inclusive).

The binary search tree is guaranteed to have unique values.	
</em>

```cpp
    int rangeSumBST(TreeNode* root, int L, int R) {
        int sum=0;
        inorder(root,sum, L, R);
        return sum;
    }
    void inorder(TreeNode* root, int& sum,int L, int R)
    {
        if(!root) return;
        inorder(root->left,sum,L,R);
        if(root->val>=L && root->val<=R) sum+=root->val;
        inorder(root->right,sum,L,R);
    }
```

O(N) but we shall have O(H):
```cpp
    int rangeSumBST(TreeNode* root, int L, int R) {
        if(!root) return 0;
        if(root->val<L) return rangeSumBST(root->right,L,R);
        if(root->val>R) return rangeSumBST(root->left,L,R);
        return root->val+rangeSumBST(root->left,L,R)+rangeSumBST(root->right,L,R);
    }
```

939. Minimum Area Rectangle
<em>
Given a set of points in the xy-plane, determine the minimum area of a rectangle formed from these points, with sides parallel to the x and y axes.

If there isn't any rectangle, return 0.
</em>

divide and conquer

```cpp
    int minAreaRect(vector<vector<int>>& points) {
        map<int,set<int>> xy;
        for(auto p:points){
            xy[p[0]].insert(p[1]);
        }
        auto it=++xy.begin();
        int minarea=INT_MAX;
        for(;it!=xy.end();it++){ //find rect with previous coordinates
            for(auto it1=xy.begin();it1!=it;it1++){
                //find the intersection of two sets
                set<int> intersection;
                for(auto i: it->second){
                    if(it1->second.count(i)) intersection.insert(i);
                }
                if(intersection.size()<2) continue;
                int mindy=INT_MAX;
                auto iter=intersection.begin(),iter1=++intersection.begin();
                for(;iter1!=intersection.end();iter++,iter1++){
                    mindy=min(mindy,(*iter1)-(*iter));
                }
                //cout<<mindy;
                if(mindy<INT_MAX)
                minarea=min(minarea,mindy*(it->first-it1->first));
            }
        }
        return minarea==INT_MAX?0:minarea;
    }
```

940. Distinct Subsequences II
<em>
Given a string S, count the number of distinct, non-empty subsequences of S .

Since the result may be large, return the answer modulo 10^9 + 7.
</em>

dp: ending with different char.
```cpp
    int distinctSubseqII(string S) {
        int endwith[26]={0};
        int mod=1e9+7;
        for(int i=0;i<S.length();i++)
        {
            int t=0;
            for(int j=0;j<26;j++)
                t+=endwith[j], t%=mod;
            endwith[S[i]-'a']=t+1;
            //endwith[S[i]-'a']=accumulate(endwith,endwith+26,1LL)%mod;
        }
            
        return accumulate(endwith,endwith+26,0LL)%mod;
    }
```	
	

