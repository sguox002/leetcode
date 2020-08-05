1323. Maximum 69 Number
<em>Problem:

Given a positive integer num consisting only of digits 6 and 9.

Return the maximum number you can get by changing at most one digit (6 becomes 9, and 9 becomes 6).</em>

Pretty easy, find the first 6 and change it to 9.
```cpp
    int maximum69Number (int num) {
        //findd the first 6
        string s=to_string(num);
        int ind=s.find_first_of('6');
        if(ind!=string::npos) s[ind]='9';
        return stoi(s);
    }
```

1324. Print Words Vertically
<em>Problem:

Given a string s. Return all the words vertically in the same order in which they appear in s.
Words are returned as a list of strings, complete with spaces when is necessary. (Trailing spaces are not allowed).
Each word would be put on only one column and that in one column there will be only one word.</em>

Idea: need to add spaces in the front but not at the end.

```cpp
    vector<string> printVertically(string s) {
        vector<string> ans,vt;
        stringstream ss(s);
        string w;
        int cnt=0;
        while(ss>>w){
            int i=0;
            for(char c: w){
                if(ans.size()<=i) ans.push_back("");
                while(ans[i].size()<cnt) ans[i]+=' ';
                ans[i]+=c;
                i++;
            }
            cnt++;
        }
        return ans;
    }
```    

1325. Delete Leaves With a Given Value
<em>Problem:

Given a binary tree root and an integer target, delete all the leaf nodes with value target.

Note that once you delete a leaf node with value target, if it's parent node becomes a leaf node and has the value target, it should also be deleted (you need to continue doing that until you can't).
</em>
Idea: postorder traversal

```cpp
    TreeNode* removeLeafNodes(TreeNode* root, int target) {
        //postorder to remove
        if(!root) return 0;
        root->left=removeLeafNodes(root->left,target);
        root->right=removeLeafNodes(root->right,target);
        if(root->left==root->right && root->val==target) return 0;
        return root;
    }
```

1326. Minimum Number of Taps to Open to Water a Garden
<em>Problem:

There is a one-dimensional garden on the x-axis. The garden starts at the point 0 and ends at the point n. (i.e The length of the garden is n).

There are n + 1 taps located at points [0, 1, ..., n] in the garden.

Given an integer n and an integer array ranges of length n + 1 where ranges[i] (0-indexed) means the i-th tap can water the area [i - ranges[i], i + ranges[i]] if it was open.

Return the minimum number of taps that should be open to water the whole garden, If the garden cannot be watered return -1.
</em>

Idea:
Equivalent to the problem asking using min number of intervals to cover a given range.
we can use bfs, greedy or dp approach for this problem.
greedy: sort the intervals, and loop over each interval, and use the one which covers the start and extends to right the most.
```cpp
    int minTaps(int n, vector<int>& ranges) {
        //intervals: min number of intervals to cover [0,n]
        //greedy: choose the one covers 0 with largest right.
        vector<vector<int>> intervals;
        for(int i=0;i<ranges.size();i++){
            intervals.push_back({i-ranges[i],i+ranges[i]});
        }
        sort(intervals.begin(),intervals.end());
        int start=0,ans=1,right=0;
        for(int i=0;i<intervals.size();i++){
            if(intervals[i][0]<=start) right=max(right,intervals[i][1]);
            else{ //interval>start, check if 
                if(intervals[i][0]>right) return -1;
                start=right;
                right=max(right,intervals[i][1]);
                ans++;
            }
            if(right>=n) return ans;
        }
        return ans;        
    }
```

dp approach:
```cpp
    int minTaps(int n, vector<int>& A) {
        vector<int> dp(n + 1, n + 2);
        dp[0] = 0;
        for (int i = 0; i <= n; ++i)
            for (int j = max(i - A[i] + 1, 0); j <= min(i + A[i], n); ++j)
                dp[j] = min(dp[j], dp[max(0, i - A[i])] + 1);
        return dp[n]  < n + 2 ? dp[n] : -1;
    }
```    

 
