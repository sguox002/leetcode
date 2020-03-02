## contest 178

### 1365. How Many Numbers Are Smaller Than the Current Number
<em>
Given the array nums, for each nums[i] find out how many numbers in the array are smaller than it. That is, for each nums[i] you have to count the number of valid j's such that j != i and nums[j] < nums[i].

Return the answer in an array.
</em>

Since length is small <500, brutal force is sufficient.
Otherwise, we need maintain a hashmap the value vs the indices.
- brutal force: O(N^2)
- sort and then binary search O(nlogn)
- count sort (since the numbers are limited <100)


### 1366. Rank Teams by Votes
<em>
In a special ranking system, each voter gives a rank from highest to lowest to all teams participated in the competition.

The ordering of teams is decided by who received the most position-one votes. If two or more teams tie in the first position, we consider the second position to resolve the conflict, if they tie again, we continue this process until the ties are resolved. If two or more teams are still tied after considering all positions, we rank them alphabetically based on their team letter.

Given an array of strings votes which is the votes of all voters in the ranking systems. Sort all teams according to the ranking system described above.

Return a string of all teams sorted by the ranking system.
</em>

- two dimensional data.
- count each character along each column
- sort by column and finally by the name index

```cpp
    string rankTeams(vector<string>& votes) {
        int m=votes.size(),n=votes[0].size();
        vector<vector<int>> cnt(26,vector<int>(n)); //
        string ans;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++) cnt[votes[i][j]-'A'][j]++;
        }
        vector<vector<int>> v;
        for(int i=0;i<26;i++){
            int tsum=accumulate(cnt[i].begin(),cnt[i].end(),0);
            if(tsum) {cnt[i].push_back(i);v.push_back(cnt[i]);}
        }
        
        sort(v.begin(),v.end(),[](vector<int>& a,vector<int>& b){
           bool ans=0;
            for(int i=0;i<a.size()-1;i++){
                if(a[i]==b[i]) continue;
                return a[i]>b[i];
            }
            return a.back()<b.back(); //
        });
        for(auto t: v) ans+='A'+t.back();
        return ans;
    }
```

### 1367. Linked List in Binary Tree
<em>
Given a binary tree root and a linked list with head as the first node. 

Return True if all the elements in the linked list starting from the head correspond to some downward path connected in the binary tree otherwise return False.

In this context downward path means a path that starts at some node and goes downwards.
</em>

idea:

- first dfs to get the nodes which has the value of the linklist head.
- then dfs from those nodes to find the list.
```cpp
    TreeNode* find(TreeNode* root,int val,vector<TreeNode*>& vnode){
        if(!root) return 0;
        if(root->val==val) {vnode.push_back(root);}
        auto left=find(root->left,val,vnode);
        auto right=find(root->right,val,vnode);
        return left?left:right;
    }
    bool isSubPath(ListNode* head, TreeNode* root) {
        //dfs pattern match?
        //find the head and match the root and then go dfs
        vector<TreeNode*> rt;
        find(root,head->val,rt);
        bool ans=0;
        for(auto r: rt){
            if(dfs(r,head)) return 1;
        }
        return 0;
    }
    bool dfs(TreeNode* root,ListNode* head){
        if(!head) return 1;
        if(!root) return 0;
        if(root->val!=head->val) return 0;
        bool ans=dfs(root->left,head->next)||dfs(root->right,head->next);
        return ans;
    }
```	
Lee215 solution:
```cpp
    bool isSubPath(ListNode* head, TreeNode* root) {
        if (!head) return true;
        if (!root) return false;
        return dfs(head, root) || isSubPath(head, root->left) || isSubPath(head, root->right);
    }

    bool dfs(ListNode* head, TreeNode* root) {
        if (!head) return true;
        if (!root) return false;
        return head->val == root->val && (dfs(head->next, root->left) || dfs(head->next, root->right));
    }
```
idea is: if root matches, then check it. otherwise, restart with its left and right.

### 1368. Minimum Cost to Make at Least One Valid Path in a Grid
<em>
Given a m x n grid. Each cell of the grid has a sign pointing to the next cell you should visit if you are currently in this cell. The sign of grid[i][j] can be:
1 which means go to the cell to the right. (i.e go from grid[i][j] to grid[i][j + 1])
2 which means go to the cell to the left. (i.e go from grid[i][j] to grid[i][j - 1])
3 which means go to the lower cell. (i.e go from grid[i][j] to grid[i + 1][j])
4 which means go to the upper cell. (i.e go from grid[i][j] to grid[i - 1][j])
Notice that there could be some invalid signs on the cells of the grid which points outside the grid.

You will initially start at the upper left cell (0,0). A valid path in the grid is a path which starts from the upper left cell (0,0) and ends at the bottom-right cell (m - 1, n - 1) following the signs on the grid. The valid path doesn't have to be the shortest.

You can modify the sign on a cell with cost = 1. You can modify the sign on a cell one time only.

Return the minimum cost to make the grid have at least one valid path.

 </em>
 
 This is a very good question on bfs.
 first, the problem can be converted to equivalent bfs problem.
 if we can go from current cell to next cell, the cost is 0
 if we cannot go from current cell to next cell, the cost is 1.
 Equivalent problem is then find the shortest cost path from (0,0) to (m-1,n-1)
 
 To apply bfs, we need build layers using cost, from 0 to 1, 2.....
 but one direction queue is hard to implement this.
 We can use deque, the same layer is in the front, the cost+1 is added to the back.
 
 ```cpp
     int minCost(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        deque<pair<int, int>> q{{0, 0}};  // for the pair, the first element is the cell position, the second is the path cost to this cell
        int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        unordered_set<int> visited;
        
        int res = 0;
        while(!q.empty())
        {
            auto t = q.front(); 
            q.pop_front();

            int curi = t.first / n, curj = t.first % n;
            //v.insert return a pair of iterator and value, indicating if insert success.
            if (visited.insert(t.first).second)  // If we have never visited this node, then we have the shortest path to this node
                res = t.second;
            
            if (curi == m-1 && curj == n-1)
                return res;
            
            for (auto dir: dirs)
            {
                int x = curi + dir[0];
                int y = curj + dir[1];
                int pos = x * n + y;
                if (x<0 || x>=m || y<0 || y>=n || visited.count(pos)) continue;
                
                int cost;
                if (grid[curi][curj] == 1 && dir[0] == 0 && dir[1] == 1) cost = 0;
                else if (grid[curi][curj] == 2 && dir[0] == 0 && dir[1] == -1) cost = 0;
                else if (grid[curi][curj] == 3 && dir[0] == 1 && dir[1] == 0) cost = 0;
                else if (grid[curi][curj] == 4 && dir[0] == -1 && dir[1] == 0) cost = 0;
                else cost = 1;
                
                if (cost == 1)
                    q.push_back({pos, t.second + cost});
                else
                    q.push_front({pos, t.second + cost});
            }
        }
        return res;
    }
```
//note we only update the cost at the first time of visit.	
	

