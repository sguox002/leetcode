## biweek 4
### 1118. Number of Days in a Month (**)
simply check if it is a lunar year

a lunar year: divisible by 4, and if divisible by 100 and 400

### 1119. Remove Vowels from a String (*)
simple

### 1120. Maximum Average Subtree (***)
idea: the average of a subtree requires the sum and num of nodes. traversal and get the two, calculate the average and max average on the fly.
```cpp
    double maximumAverageSubtree(TreeNode* root) {
        double ans=0;
        int num=0;
        dfs(root,num,ans);
        return ans;
    }
    pair<int,int> dfs(TreeNode* root,int num,double& ans){
        if(!root) return {0,0};
        auto left=dfs(root->left,num,ans);
        auto right=dfs(root->right,num,ans);
        num=1+left.second+right.second;
        int sum=(left.first+right.first+root->val);
        double leftavg=left.first*1.0/left.second;
        double rightavg=right.first*1.0/right.second;
        double curr=sum*1.0/num;
        ans=max({ans,leftavg,rightavg,curr});
        return {sum,num};
    }
```	

### 1121. Divide Array Into Increasing Sequences (***)
<em>Problem: 

Given a non-decreasing array of positive integers nums and an integer K, find out if this array can be divided into one or more disjoint increasing subsequences of length at least K.
</em>
Idea:

hashmap to get the histogram. The most frequent one determines at least how many groups we will have.
- maxfreq*K<n no solution
- maxfreq=1 only one
- greedy: we need use the max freq and reduce by 1, stop when all same maxfreq is reduced and cnt>=k.
- using a pq to reduce the maxfreq by 1 and then put back
```cpp
    struct comp{
        bool operator()(pair<int,int> a,pair<int,int> b){
            return a.second<b.second || (a.second==b.second && a.first<b.first);
        }
    };
    bool canDivideIntoSubsequences(vector<int>& nums, int K) {
        int n=nums.size();
        map<int,int> mp;
        int maxdup=0;
        for(int i: nums) {mp[i]++;maxdup=max(maxdup,mp[i]);}
        if(maxdup*K>n) return 0;
        if(maxdup==1) return 1;
        //we need to divide the array into at least maxdup parts
        //we first need to take one from the most frequent one until the top
        priority_queue<pair<int,int>,vector<pair<int,int>>,comp> pq(mp.begin(),mp.end());
        while(pq.size()){
            auto p=pq.top();
            int maxfreq=p.second;
            int cnt=0;
            vector<pair<int,int>> vp;
            while(pq.size() && (cnt<K || pq.top().second==maxfreq)){ //same freq shall be used all
                if(pq.top().second>1)
                    vp.push_back({pq.top().first,pq.top().second-1});
                //cout<<pq.top().first<<" ";
                cnt++;
                pq.pop();
            }
            if(cnt<K) return 0;
            for(auto t: vp) pq.push(t);
        }
        return 1;
    }
```	