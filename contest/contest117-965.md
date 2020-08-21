## contest 117
965. Univalued Binary Tree3
966. Numbers With Same Consecutive Differences5
967. Vowel Spellchecker6
968. Binary Tree Cameras


### 965. Univalued Binary Tree.md

check if the binary tree has one single value

there are quite a few methods such as using a hashset to traverse the tree
or use the root value to compare with all other node values

```cpp
    bool isUnivalTree(TreeNode* root) {
        if(!root) return 1;
        int val=root->val;
        return isSame(root,val);
    }
    bool isSame(TreeNode* root,int val)
    {
        if(!root) return 1;
        if(root->val!=val) return 0;
        return isSame(root->left,val) && isSame(root->right,val);
    }
```
### 966. Vowel Spellchecker.md

Given a word list and a query list
1. if exactly match, return the word
2. if ignore case match, return the first matched word in word list
3. if ignore the vowels match, return the first matched word in the word list

create a hashset for the word list
create a hashmap the lowered word vs the original word list
create a hashmap ignoring the vowels vs the original word list

```cpp
    vector<string> spellchecker(vector<string>& wordlist, vector<string>& queries) {
        unordered_map<string,string> mp_novow,mp_nocase; //word vs no vowels
        unordered_set<string> dict(wordlist.begin(),wordlist.end());
        char vow[]={'a','e','i','o','u'};
        unordered_set<char> vowels(vow,vow+6);
        for(int i=0;i<wordlist.size();i++)
        {
            //remove the vowels
            string s1,s2;
            for(int j=0;j<wordlist[i].size();j++)
            {
                char c=tolower(wordlist[i][j]);
                if(!vowels.count(c)) s1+=c;else s1+='*';
                s2+=c;
            }
            if(!mp_novow.count(s1))mp_novow[s1]=wordlist[i];//only need the first
            if(!mp_nocase.count(s2))mp_nocase[s2]=wordlist[i]; //only need the first
        }
        vector<string> ans(queries.size());
        for(int i=0;i<queries.size();i++)
        {
            if(dict.count(queries[i])) ans[i]=queries[i]; //exact match
            else //check if case match
            {
                string s1,s2;
                for(int j=0;j<queries[i].size();j++)
                {
                    char c=tolower(queries[i][j]);
                    s1+=c;
                    if(!vowels.count(c)) s2+=c;else s2+='*';
                }
                if(mp_nocase.count(s1)) ans[i]=mp_nocase[s1];
                else //case not match, check if no vowel match
                {
                    if(mp_novow.count(s2)) ans[i]=mp_novow[s2];//no vowels must also match position
                    else ans[i]="";
                }
            }
        }
        return ans;
    }
```

- could be shorter if we use functions to create noncased and novowels
- the first by ignoring the same keywords after.


### 967. Numbers With Same Consecutive Differences.md

Given digits 0 to 9, and N for a number with N digits, K for the absolute difference of consecuative digits
return all the numbers

This is a typical dfs problem. First choice is 1 to 9, the next is i+K and i-K

```cpp
    vector<int> numsSameConsecDiff(int N, int K) {
        //so this is a dfs problem
        vector<int> ans;
        if(N==1) return vector<int>({0,1,2,3,4,5,6,7,8,9});
        for(int i=1;i<10;i++)
            dfs(i,N-1,K,ans,0);
        return vector<int>(ans.begin(),ans.end());
    }
    void dfs(int start,int n,int K,vector<int>& ans,int res)
    {
        res=res*10+start;                
        if(n==0 ) {ans.push_back(res);return;}
     
        if(start+K<10) dfs(start+K,n-1,K,ans,res);
        if(K && start-K>=0) dfs(start-K,n-1,K,ans,res);
    }
```

- when we do the first round, leaving n-1
- when k=0, we need to avoid duplicates
- complexity O(9^N)

### 968. Binary Tree Cameras.md

a binary tree: a camera will cover its parent, itself and its immediate child
return the min number of camera needed

This is a dp problem similar to the house robber. 
Also it is a greedy problem.

Apply a recusion function dfs.
We use greedy algorithem here. We put camera at as higher level (closer to root) as possible. 
We use post-order DFS here. This would be O(n) time and O(h) space, where h is height of the tree.
The return value of DFS() has following meanings. 
- 0: there is no camera at this node, and it's not monitored by camera at either of its children, which means neither of child nodes has camera. 
- 1: there is no camera at this node; however, this node is monitored by at least 1 of its children, which means at least 1 of its children has camera. 
- 2: there is a camera at this node.

```cpp
class Solution {
public:
    int minCameraCover(TreeNode* root) {
        int sum=0;
        if(dfs(root,sum)==0)   sum++;// if root is not monitored, we place an additional camera here
        return sum;
    }
    
    int dfs(TreeNode * tr, int& sum){
        if(!tr) return 1;
        int l=dfs(tr->left,sum), r=dfs(tr->right,sum);
        if(l==0||r==0){// if at least 1 child is not monitored, we need to place a camera at current node 
            sum++;
            return 2;
        }else if(l==2||r==2){// if at least 1 child has camera, the current node is monitored. Thus, we don't need to place a camera here 
            return 1;
        }else{// if both children are monitored but have no camera, we don't need to place a camera here. We place the camera at its parent node at the higher level. 
            return 0;
        }
        return -1;// this return statement won't be triggered
    }
};

### comments
- This is hard and I cannot get the solution
