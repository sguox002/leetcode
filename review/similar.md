common techniques

1. find min or max, 2nd min 2nd max
O(n) one pass to find the top one two or three max/min

2. find majority using voting algorithm
O(n) to find the majority elements.
>1/2, one pass
>1/3, could have two candidate, need another pass verify
>1/4, could have three candiates, need another pass to verify.

3. from left to right, right to left, both end
problems to compare with left and right
problems leftmax, right max
problems using two pointers

4. two pointer, 3 pointer, 4 pointer..
reduce one order of complexity.

5. parition into several groups

6. x-base problems including negatives

7. linked list operations

8. permuation problems

9. prime factorization

10. modular problems

11. duplicates

12. gray code: reflected

13. cyclic linking problems

14. tree O(n) 

15. subproblems: recursive, dp, divide and conquer

16. union-find

17. dfs

18. bfs

19. shortest distance

20 traveling salesman problems

21. knapsack problems

22. stack problems

24. trie problems

25 kadane 

26 binary search

27. greedy

28 bit operations

29 random

30 graph

31 sliding window

32 monotonic stack/deque/vector

33. backtracking

34. puzzles

### gray code
- gray code: every time it changes only one bit
- can used to build all numbers using shortest length
- other methods to change one bit only will incur early cycles

89. generate gray code
- i^(i>>1)
- build from 0,1 using the definition to reflect.
- backtracking.
```cpp
	vector<int> grayCode(int n){
		vector<int> ans;
		ans.push_back(0);
		int len=1<<n;
		for(int i=0;i<len;i++){
			int sz=ans.size();
			for(int k=sz-1;k>=0;k--){ //reflect and double.
				ans.push_back(ans[k]|1<<i);
			}
		}
		return ans;
	}
```	

717. one bit or two bit char
- from left to end. if start 1, we add 2 steps, otherwise add one steps
- reverse from right to left: 100, one char, 

753. cracking the safe
use k-base numbers. password is n-digit
shortest length to guarantee the crack password.
It can be considered having k^n-1 nodes and visiting all nodes in shortest path (exactly once).
following the gray code:
```cpp
    string crackSafe(int n, int k) {
		string ans = string(n, '0');
		unordered_set<string> visited;
		visited.insert(ans);

		for(int i = 0;i<pow(k,n);i++)
		{
			string prev = ans.substr(ans.size()-n+1,n-1);
			for(int j = k-1;j>=0;j--) //reflect and attach
			{
				string now = prev + to_string(j);
				if(!visited.count(now))
				{
					visited.insert(now);
					ans += to_string(j);
					break;
				}       
			}
		}
		return ans;
    }
```
neighboring string has n-1 in common, so connected string is in minimal length.


### shortest distance problem
shortest distance:
- dfs to get all possible path and find the min
- dp or recursive
- bfs
- dijkstra
- bellman
- travel salesman
- two pointer
- sliding window

943. find the shortest superstring
n strings, they have suffix prefix in common. connect them in shortest length.
- generate nxn matrix, m[i,j] is the min length connecting i to j (edge)
- problem becomes traveling salesman problem: visiting all nodes exactly once with shortest path
- regular practice
	- using n bits to indicate if a node is visited
	- there are 2^n status (dp, set the distance to be INF)
	- loop over all status. dp[i,j]: i status, j nodes
	- loop over all nodes
	- visit the nodes: its own length
	- loop over all nodes to relax dp[i,j] using other node k
	- status=0xffff are for all nodes visited.
	- need restore the path using parent node.
	
1092. Shortest Common Supersequence
this needs sequence
the idea is to find the lcs and place only one copy of it.
lcs of two strings are common dp problem.

820. Short Encoding of Words	
- sort the words according to length (descending) since only longer words will contain small words
- store all suffix into hashset.
- if a word is seen in the suffix, the word is hidden.

864. Shortest Path to Get All Keys
this allows go back and forth.
bfs also need 1<<K status. dp[m,n,1<<K]
-. visited and queue
- queue contain status and i,j.
```cpp
int shortestPathAllKeys(vector<string>& grid) {
    int m=grid.size(), n=m?grid[0].size():0;
    if(!m || !n) return 0;
    int path=0, K=0;
    vector<int> dirs={0,-1,0,1,0};
    vector<vector<vector<bool>>> visited(m,vector<vector<bool>>(n,vector<bool>(64,0))); //at most 6 keys, using bitmap 111111
    queue<pair<int,int>> q; //<postion, hold keys mapping, status vs i*n+j.
	//get number of keys and start position
    for(int i=0;i<m;i++){
        for(int j=0;j<n;j++){
            if(grid[i][j]=='@'){
                q.push({i*n+j,0});
                visited[i][j][0]=1;                    
            }
            if(grid[i][j]>='A' && grid[i][j]<='F') K++; //total alpha number
        }
    }
	
    while(!q.empty()){
        int size=q.size();
        for(int i=0;i<size;i++){
			auto z=q.front();
			q.pop();
            int a=z.first/n, b=z.first%n;
            int carry=z.second; //the status
			
            if(carry==((1<<K)-1)) return path; //if all keys hold, just return 
            
			for(int j=0;j<4;j++){
                int x=a+dirs[j], y=b+dirs[j+1], k=carry;
				//out of boundary or the wall
				if(x<0 || x>=m || y<0 || y>=n || grid[x][y]=='#') continue;
                //get the key.
				if(grid[x][y]>='a' && grid[x][y]<='f'){
                    k=carry|(1<<(grid[x][y]-'a')); //update hold keys
                }
                else if(grid[x][y]>='A' && grid[x][y]<='F'){ //if we can go over the locker
                    if(!(carry & (1<<(grid[x][y]-'A')))) continue;
                }
                if(!visited[x][y][k]){
                    visited[x][y][k]=1;
                    q.push({x*n+y,k});
               }                
            }
        }
        path++;
    }
    return -1;
}
```
847. Shortest Path Visiting All Nodes
given an undirected graph, visiting all nodes (can start from any node) and revisit
bfs:
we store the source node and the status (nodes reachable from the source)
we have 2^n status.
- first round, all nodes into the queue with distance 0, other distance are inf.
- dp[n,1<<n]
- pop all previous layers node
- using its child to relax the distance 
- if relaxed push the layer into queue for next layer
- final answer is the status of 0xfff...
```cpp
    struct State{
        int mask,source;
        State(int m,int s):mask(m),source(s){}
    };
    int shortestPathLength(vector<vector<int>>& graph) {
        int m=graph.size();
        int len=1<<m;
        vector<vector<int>> dp(m,vector<int>(len,INT_MAX));
        queue<State> qs;
        for(int i=0;i<m;i++) 
        {
            dp[i][1<<i]=0; //self to self distance is 0
            qs.push(State(1<<i,i));
        }
        while(!qs.empty())
        {
            State state=qs.front();
            //cout<<state.source<<" "<<hex<<state.mask<<endl;
            qs.pop();
            for(int next:graph[state.source]) //connected nodes
            {- 
                int nextmask=state.mask|(1<<next);
                if(dp[next][nextmask]>dp[state.source][state.mask]+1) //passing this node is closer
                {
                    dp[next][nextmask]=dp[state.source][state.mask]+1;
                    qs.push(State(nextmask,next));
                }
            }
        }
        //shortest path 
        int ans=INT_MAX;
        for(int i=0;i<m;i++) ans=min(ans,dp[i][(1<<m)-1]);
        return ans;
    }
````

214. Shortest Palindrome
by only adding in the front.
equivalent to: find the longest prefix palindrome.
- direct approach will TLE for long test cases for example all 'a'. (reverse check the prefix)	
- more efficient: 
	- the longest pal shall in the range [0,i]
	- every time we reduce the size.
	- two pointer: one from left one from right
	- if s[i]==s[j] left advance by 1
The proof of correction is that: Say the string was a perfect palindrome, ii would be incremented nn times. Had there been other characters at the end, ii would still be incremented by the size of the palindrome. Hence, even though there is a chance that the range \text{[0,i)}[0,i) is not always tight, it is ensured that it will always contain the longest palindrome from the beginning.
i.e the prefix shall contain the letters, and we advance, it may contain more, but the longest pal is always inside it.
```cpp
    string shortestPalindrome(string s) {
        //find the longest palindrome in s and then use it as the base to add
        //the longest must start with the first char
        int n=s.size();
        int j = 0;
        for (int i = s.length() - 1; i >= 0; i--) 
        {
            if (s[i] == s[j]) { j += 1; }
        }
        if (j == s.length()) { return s; }
        string suffix=s.substr(j);
        string rss=suffix;
        reverse(rss.begin(),rss.end());
        return rss+shortestPalindrome(s.substr(0,j))+suffix;
    }
```

934. Shortest Bridge
two islands
first find the two islands and include all points
and find the min distance brutal force O(M*N)

821. Shortest Distance to a Character
two pass: first left to right to find the closest distance to the char
second pass to find the closest to the right char
choose the min

similar: 135. Candy
two pass to get left and right.

1091. Shortest Path in Binary Matrix
0/1 matrix, 8 directions
1 is obstacles
from top left to bottom right
bfs
```cpp
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        //bfs
        int n=grid.size();
        if(n<2) return n;
        vector<int> visited(n*n);
        if(grid[0][0]||grid[n-1][n-1]) return -1;
        queue<int> q;
        q.push(0);visited[0]=1;
        int step=1;
        int dir[][2]={{-1,0},{1,0},{0,-1},{0,1},{-1,-1},{1,1},{1,-1},{-1,1}};
        while(q.size())
        {
            int sz=q.size();
            while(sz--){
                int t=q.front();q.pop();
                if(t==n*n-1) return step;
                int i=t/n,j=t%n;
                for(auto v: dir){
                    int x=i+v[0],y=j+v[1];
                    if(x<0 ||y<0 ||x>=n || y>=n || grid[x][y] || visited[x*n+y]) continue;
                    //cout<<x<<" "<<y<<endl;
                    visited[x*n+y]=1;
                    q.push(x*n+y);
                }
            }
            step++;
        }
        return -1;
    }
```

# monotonic queue	
209. Minimum Size Subarray Sum
non-negative array, subarray sum>=K.
- cumsum is sorted for non-negative input.
- use hashmap or binary search to get the previous position
O(nlogn)

sliding window using two pointers: O(n)
- if sum<K, advance j.
- while sum>=k, advance i and subtract the out of window elements from it.

862. Shortest Subarray with Sum at Least K
include negatives.
a modified sliding window using deque.
montonic queue
LC84. Largest Rectangle in Histogram
LC239. Sliding Window Maximum
LC739. Daily Temperatures
LC862. Shortest Subarray with Sum at Least K
LC901. Online Stock Span
LC907. Sum of Subarray Minimums
it maintains a queue/stack/deque/array in increasing/decreasing order
- we are maintaining a prefix sum array. sum can be easily calculated using two element diff.
- the deque maintains the indices
- always push in the back, but can pop both ends.
- the deque from the front to back is in increasing order (front is the smallest)
- when a new element come in, first check with the front:
	- while prefix[i]>=k+front, update the min size, pop it (back will have a shorter size)
	- while back>=prefix[i] popping the back
	- add prefix[i] in the back.
	
```cpp
    int shortestSubarray(vector<int> A, int K) {
        int N = A.size(), res = N + 1;
        vector<int> B(N + 1, 0);
        for (int i = 0; i < N; i++) B[i + 1] += B[i] + A[i];
        deque<int> d;
        for (int i = 0; i < N + 1; i++) {
            while (d.size() > 0 && B[i] - B[d.front()] >= K)
                res = min(res, i - d.front()), d.pop_front();
            while (d.size() > 0 && B[i] <= B[d.back()]) d.pop_back();
            d.push_back(i);
        }
        return res <= N ? res : -1;
    }
```
a bit about deque:
- deque is segment continuous
- front is the first element, back is the last element
for example: [1,2,3,4,-3,-2,0,3,5], with k=4	
the deque from front to back:
0 
0 1 
0 1 2 
2 3 
4 
5 
6 
7 
7 8 
9 

239. Sliding Window Maximum
also works for minimum
- deque stores the index, front: the left pointer, back: the right pointer
- deque from front to back: elements in decreasing order. the front is the max.
- if the front is the one out of window, pop it.
- while current > back, pop back
- add back the current.

```cpp
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        //monotonic array using deque
        vector<int> res;
        deque<int> dq;
        for(int i=0;i<nums.size();i++)
        {
            if(!dq.empty() && dq.front()==i-k) dq.pop_front();
            while(!dq.empty() && nums[dq.back()]<nums[i]) dq.pop_back(); //remove all elements less than it
            dq.push_back(i);
            if(i>=k-1) res.push_back(nums[dq.front()]);
        }
        return res;
    }
```	

480. Sliding Window Median
- we need use two heap: left use max heap, right use min heap.
- first k numbers, push to left and then half of them push to right. k is odd, left has one more.
- the element moving out of the window: only when it is the top, remove it, otherwise record it in hashmap
- add the new element in left or right
- rebalance the left and right.
- remove those on top and shall remove.
- 
```cpp
    vector<double> medianSlidingWindow(vector<int>& nums, int k) {
        vector<double> medians;
        unordered_map<int, int> hash;                          // count numbers to be deleted
        priority_queue<int, vector<int>> left;                // heap on the bottom
        priority_queue<int, vector<int>, greater<int>> right;  // heap on the top
        
        int i = 0;
        
        // Initialize the heaps
        while (i < k)  { left.push(nums[i++]); }
        for (int count = k/2; count > 0; --count) {
            right.push(left.top()); left.pop();
        }
        
        while (true) {
            // Get median
            if (k % 2) medians.push_back(left.top());
            else medians.push_back( ((double)left.top() + right.top()) / 2 );
            
            if (i == nums.size()) break;
            int m = nums[i-k], n = nums[i++], balance = 0;
            
            // What happens to the number m that is moving out of the window
            if (m <= left.top())  { --balance;  if (m == left.top()) left.pop(); else ++hash[m]; }
            else                   { ++balance;  if (m == right.top()) right.pop(); else ++hash[m]; }
            
            // Insert the new number n that enters the window
            if (!left.empty() && n <= left.top())  { ++balance; left.push(n); }
            else                                     { --balance; right.push(n); }
            
            // Rebalance the bottom and top heaps
            if      (balance < 0)  { left.push(right.top()); right.pop(); }
            else if (balance > 0)  { right.push(left.top()); left.pop(); }
            
            // Remove numbers that should be discarded at the top of the two heaps
            while (!left.empty() && hash[left.top()])  { --hash[left.top()]; left.pop(); }
            while (!right.empty() && hash[right.top()])  { --hash[right.top()]; right.pop(); }
        }
        
        return medians;
    }
```	
	

295. Find Median from Data Stream
expanding array median.

```cpp
    priority_queue<int,vector<int>,less<int>> left;
    priority_queue<int,vector<int>,greater<int>> right;
    double med;
    int total;
    MedianFinder() {
        total=0;
    }
    
    void addNum(int num) {
        total++;
        if(total==1) {med=num;return;}
        
        if(total%2)//total is odd, one element is in and one element is out
        {
            if(num<left.top()) {left.push(num);med=left.top();left.pop();}
            else if(num>right.top()) {right.push(num);med=right.top();right.pop();}
            else med=num;//lmax<=num<=lmin
        }
        else //total is even, the new num and med shall be inside left or right
        {
            if(num<med) {left.push(num);right.push(med);}
            else {left.push(med);right.push(num);}
            med=(left.top()+right.top())/2.0;
        }
    }
    
    double findMedian() {
        return med;
    }
```
	
# knapsack problems
use or not, cannot reuse
can use repeatedly
partition: several groups
dp or backtracking








