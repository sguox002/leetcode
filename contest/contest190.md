## contest 190

### 1455. Check If a Word Occurs As a Prefix of Any Word in a Sentence
<em>
Given a sentence that consists of some words separated by a single space, and a searchWord.

You have to check if searchWord is a prefix of any word in sentence.

Return the index of the word in sentence where searchWord is a prefix of this word (1-indexed).

If searchWord is a prefix of more than one word, return the index of the first word (minimum index). If there is no such word return -1.

A prefix of a string S is any leading contiguous substring of S.
</em>

straightforward. just check one by one.
```cpp
    int isPrefixOfWord(string sentence, string searchWord) {
        stringstream ss(sentence);
        vector<string> vw;
        string w;
        int n=searchWord.size();
        while(ss>>w){
            if(w.size()>=searchWord.size() && w.substr(0,n)==searchWord)
                return vw.size()+1;
            vw.push_back(w);
        }
        return -1;
    }
```

### 1456. Maximum Number of Vowels in a Substring of Given Length
<em>
Given a string s and an integer k.

Return the maximum number of vowel letters in any substring of s with length k.

Vowel letters in English are (a, e, i, o, u).
</em>

simple and straightforward
using sliding window

```cpp
    bool isvowel(char c){
        return c=='a'||c=='e'||c=='i'||c=='o'||c=='u';
    }
    int maxVowels(string s, int k) {
        //aeiou
        int cnt=0;
        int ans=0;
        int i=0;
        while(i<s.size()){
            cnt+=isvowel(s[i]);
            if(i>=k){
                cnt-=isvowel(s[i-k]);
            }
            if(i>=k-1) ans=max(ans,cnt);
            i++;
        }
        return ans;
    }
```

### 1457. Pseudo-Palindromic Paths in a Binary Tree
<em>
Given a binary tree where node values are digits from 1 to 9. A path in the binary tree is said to be pseudo-palindromic if at least one permutation of the node values in the path is a palindrome.

Return the number of pseudo-palindromic paths going from the root node to leaf nodes.
</em>

simple dfs, palindrome has at most one with odd number.

```cpp
    int pseudoPalindromicPaths (TreeNode* root) {
        //all except at most 1 appear odd times
        vector<int> cnt(10);
        return helper(root,cnt);
    }
    int helper(TreeNode* root,vector<int> cnt){
        if(!root) return 0;
        cnt[root->val]++;
        if(root->left==root->right) {return ispal(cnt);}
        return helper(root->left,cnt)+helper(root->right,cnt);
    }
    bool ispal(vector<int>& cnt){
        int odd=0;
        for(int i: cnt) odd+=i%2;
        return odd<=1;
    }
```

### 1458. Max Dot Product of Two Subsequences
<em>
Given two arrays nums1 and nums2.

Return the maximum dot product between non-empty subsequences of nums1 and nums2 with the same length.

A subsequence of a array is a new array which is formed from the original array by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, [2,3,5] is a subsequence of [1,2,3,4,5] while [1,5,3] is not).
</em>

typical dp. a good question.

- could be 3d, but it will TLE dp[i,j,k] means array length 1=i, array 2 length=j, and k pairs
- but we do not need k actually, we do not care how many pairs, we can just add a pair to previous if it is >0
- dp[i,j] reprsents the max dot product with array 1 length=i and array 2 length=j.
- there are several options:
* use i, but not use j, dp(i,j-1)
* use j, but not use i, dp(i-1,j)
* use i and j as a pair, A[i]*B[j]+dp[i-1,j-1], make sure A[i]*B[j]>0

```cpp
    int maxDotProduct(vector<int>& nums1, vector<int>& nums2) {
        int n = int(nums1.size()), m = int(nums2.size());
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, INT_MIN));
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= m; ++j) {
                dp[i][j] = max(dp[i][j], dp[i - 1][j]);
                dp[i][j] = max(dp[i][j], dp[i][j - 1]);
                dp[i][j] = max(dp[i][j], dp[i - 1][j - 1]);
                dp[i][j] = max(dp[i][j], max(dp[i - 1][j - 1], 0) + nums1[i - 1] * nums2[j - 1]);
            }
        }
        return dp[n][m];
    }
```

	


