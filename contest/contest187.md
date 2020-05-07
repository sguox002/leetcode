## contest 187
### 1436. Destination City
<em>
You are given the array paths, where paths[i] = [cityAi, cityBi] means there exists a direct path going from cityAi to cityBi. Return the destination city, that is, the city without any path outgoing to another city.

It is guaranteed that the graph of paths forms a line without any loop, therefore, there will be exactly one destination city.
</em>

approach: count incoming and outcoming edges and the destination is the one with net negative incoming edges
```cpp
    string destCity(vector<vector<string>>& paths) {
        string ans;
        //all appear in-out except one
        unordered_map<string,int> mp;
        for(auto p: paths){
            mp[p[0]]++;
            mp[p[1]]--;
        }
        for(auto t: mp){
            if(t.second<0) return t.first;
        }
        return ans;
    }
```

other approach: using hashset, and check if the right one appears in the left.

### 1437. Check If All 1's Are at Least Length K Places Away
<em>
Given an array nums of 0s and 1s and an integer k, return True if all 1's are at least k places away from each other, otherwise return False.
</em>

approach:

check the distance of 1 to the previous 1.

```cpp
    bool kLengthApart(vector<int>& nums, int k) {
        int prev=-1;
        for(int i=0;i<nums.size();i++){
            if(prev<0 && nums[i]){
                prev=i;
            }
            else if(prev>=0 && nums[i]){
                if(i-prev-1<k) return 0;
                prev=i;
            }
        }
        return 1;
    }
```
O(N)

### 1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit
<em>
Given an array of integers nums and an integer limit, return the size of the longest continuous subarray such that the absolute difference between any two elements is less than or equal to limit.

In case there is no subarray satisfying the given condition return 0.
</em>

approach 1: deque+binary search O(nlogn)
using binary search to find the largest length. And the problem is equivalent to check 
if the array can satisfy the condition: for the window k, there is one subarray satisfy max-min<=limit
so the problem is to search for max and min in a sliding window.
```cpp
    int longestSubarray(vector<int>& nums, int limit) {
        //the max-min<=k
        int l=1,r=nums.size();
        while(l<r){
            int m=l+(r-l+1)/2;
            if(valid(nums,limit,m)) l=m;
            else r=m-1;
        }
        return l;
    }
    int valid(vector<int>& nums,int limit,int k){
        //using deque to find the max and min in the sliding window
        deque<int> dqmin,dqmax;
        for (int i=0; i<nums.size(); i++) {
            if (!dqmax.empty() && dqmax.front() == i-k) dqmax.pop_front();
            while (!dqmax.empty() && nums[dqmax.back()] < nums[i])
                dqmax.pop_back();
            dqmax.push_back(i);

            if (!dqmin.empty() && dqmin.front() == i-k) dqmin.pop_front();
            while (!dqmin.empty() && nums[dqmin.back()] > nums[i])
                dqmin.pop_back();
            dqmin.push_back(i);
            if (i>=k-1) {
                if(nums[dqmax.front()]-nums[dqmin.front()]<=limit) return 1;
            }
        }
        return 0;        
    }
```	
- uses two deque to find max and min separately. Can we use one to find max and min at the same time?
	
optimization:
- use two deque, but using sliding window O(n) with two pointers:
```cpp
    int longestSubarray(vector<int>& A, int limit) {
        deque<int> maxd, mind;
        int i = 0, j;
        for (j = 0; j < A.size(); ++j) {
            while (!maxd.empty() && A[j] > maxd.back()) maxd.pop_back();
            while (!mind.empty() && A[j] < mind.back()) mind.pop_back();
            maxd.push_back(A[j]);
            mind.push_back(A[j]);
            if (maxd.front() - mind.front() > limit) {
                if (maxd.front() == A[i]) maxd.pop_front();
                if (mind.front() == A[i]) mind.pop_front();
                ++i;
            }
        }
        return j - i;
    }
```	

- using multiset for max/min with sliding window using two pointer O(nlogn)
```cpp
    int longestSubarray(vector<int>& A, int limit) {
        int i = 0, j;
        multiset<int> m;
        for (j = 0; j < A.size(); ++j) {
            m.insert(A[j]);
            if (*m.rbegin() - *m.begin() > limit)
                m.erase(m.find(A[i++]));
        }
        return j - i;
    }
```

### 1439. Find the Kth Smallest Sum of a Matrix With Sorted Rows
<em>
You are given an m * n matrix, mat, and an integer k, which has its rows sorted in non-decreasing order.

You are allowed to choose exactly 1 element from each row to form an array. Return the Kth smallest array sum among all possible arrays.
[[1,3,11],[2,4,6]], k = 5, answer=7
</em>

This is a very good problem, let's exam the example first
[1,3,11]
[2,4,6]
1st: 1+2=3
then 
we replace 1 with 3: 3+2=5
we replace 2 with 4: 1+4=5
3+2->11+2=13
  |->3+4=7
1+4->3+4=7 (visited)
  |->1+6=7
  
It is a bfs but with the smallest to go first.
The trick is we shall combine the sum with all its elements into one node. 
This is a bit similar to dijkstra

```cpp
    int kthSmallest(vector<vector<int>>& mat, int k) {
        //store sum and all its index in pq
        priority_queue<vector<int>,vector<vector<int>>,greater<vector<int>>> pq;
        int sum=0;
        int m=mat.size(),n=mat[0].size();
        unordered_set<string> visited;
        vector<int> t(m+1);
        for(int i=0;i<m;i++){
            sum+=mat[i][0];  
            t[i+1]=i*n;
        } 
        t[0]=sum;
        pq.push(t);
        visited.insert(v2s(t));
        //do it like bfs!
        while(k>0){
            auto v=pq.top();
            pq.pop();
            sum=v[0];
            for(int i=1;i<v.size();i++){
                int r=v[i]/n,c=v[i]%n;
                if(c+1<n){
                    v[i]++;
                    if(visited.count(v2s(v))) {
                        v[i]--;
                        continue;
                    }
                    visited.insert(v2s(v));
                    v[0]+=mat[r][c+1]-mat[r][c];
                    pq.push(v);
                    v[0]-=mat[r][c+1]-mat[r][c];
                    v[i]--;
                }
            }
            k--;
        }
        return sum;
    }
    string v2s(vector<int>& t){
        string ans;
        for(int i=1;i<t.size();i++){
            ans+=to_string(t[i])+" ";
        }
        return ans;
    }
```
- I used index array to string for the visited.

complexity: 
we pop a node, then we need add n nodes in. total nodes will up to m*n.
priority_queue add is log time. 
O(m*n*log(m*n))

other approach:
- brutal force: get all combination sums and sort.




 
 

