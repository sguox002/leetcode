# bianry search

1. define the ranges and conditions

2. define the exit condition

3. invariant condition

also we need to know if we are getting the first or last answer. This will change the iteration loop termination condition

## easy

### 744. Find Smallest Letter Greater Than Target
letters are wraparound. 
approach 1: find upper_bound, if not found, return the first one.
approach 2: write our own upper_bound
may contain duplicates

Binary search: consider the following 3 things:

- using greater for the input, the left side is all false, and the right part is all true.
we are looking for the first true.
for the range [l,r], the l is false, and the right is true, ie. A[l]<=target, and A[r]>target.
l==r will terminate the loop and answer is l or r.

- invariant: we shall always keep the condition true.
A[m]>target: r=m (since m could be the answer)
else: l=m+1 since m is not the answer.

- two element: m=l+(r-l)/2 will always choose l, and we forward to l+1 and loop will exit. so no problem

And the code: 

```cpp
	char nextGreatestLetter(vector<char>& letters, char target) {
		int l=0,r=letters.size();
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(letters[m]>target) r=m;
			else l=m+1;
		}
		//when exit l==r so the answer is l or r.
		return letters[l%letters.size()];
	}
```

### 35. Search Insert Position
same as 744 but similar to lower_bound, but now we need find the position
we are looking for the first element satisfying A[m]>=target
using the condition: the left part are all false, and right part are all true.
so we shall keep this invariant.

1. [l,r] ge(l) is false and ge(r) is true. so we need keep l<r and exit condition would be l==r and answer is r.

2. A[m]>=target, ge(m) is true, and we shall forward to r=m (m is possible answer)
A[m]<target ge(m) is false, and we shall set l=m+1 (otherwise we will go to true and break the invariant)

3. two element: make sure there is no infinite loop.

```cpp
	int searchInsert(vector<int>& nums, int target) {
		int l=0,r=nums.size();
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(nums[m]==target) return m;
			if(nums[m]<target) l=m+1;
			else r=m;
		}
		return l;
	}
```
once we have duplicates, this solution will return any of the duplicates.

or a bit change: (better)
```cpp
	int searchInsert(vector<int>& nums, int target) {
		int l=0,r=nums.size();
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(nums[m]<target) l=m+1;
			else r=m;
		}
		return l;
	}
```
this shall work for duplicate numbers

### 852. Peak Index in a Mountain Array
left sorted, right reverse sorted
O(n): from either side to climb until get to the peak
binary search: we are looking for the peak: A[peak-1]<A[peak]>A[peak+1]

we still use the 3 principles for binary search:
1. peak in [l,r] means: A[l]<peak>A[r]. if we use A[m]>A[l] we will get: 1,1,1,...1,0,0,0....
so we are looking for the last true condition

2. A[m]<A[m+1] l=m+1
A[m-1]>A[m] r=m (since m is false but m-1 may be not)

3. using (2) l will stop at the last 1 and r will stop at the first 0 and loop shall terminate when l+1<r

```cpp
	int peakIndexInMountainArray(vector<int>& A) {
		int n=A.size(),l=0,r=n-1;
		while(l<r)  //change to while(l+1<r) also works.
		{
			int m=l+(r-l)/2;
			if(A[m]<A[m+1]) l=m+1;
			else if(A[m-1]>A[m]) r=m;
			else return m; //this is necessary to cover all cases. without this will fall into infinite loop
		}
		return -1;
	}
```
comment: not easy to implement the binary search.

### 704. Binary Search
classical to find a target's index in sorted array
approach 1: use lower_bound
approach 2: l, r. target shall be in the range [l,r] all the time.

3 principles:
1. the target is in the range [l,r] meaning that A[i]<=target, all left is true, and all right is false
we are shrinking the range to either [l,l+1] or [l,l]
In this case, since we are looking for exactly match, == means early termination

A[m]>target, it is in the left, r=m-1
A[m]<target, is is in the right l=m+1
A[m]==target, found

```cpp
	int search(vector<int>& nums, int target) {
		int l=0,r=nums.size()-1;
		while(l<=r)
		{
			int m=l+(r-l)/2;
			if(nums[m]==target) return m;
			if(nums[m]<target) r=m-1;
			else l=m+1;
		}
		return -1;
	}
```

### 374. Guess Number Higher or Lower
just replace the condition with predefined function
invariant: a[l]<=target<=a[r]

```cpp
    int guessNumber(int n) {
        int l=1,r=n;
		while(l<=r)
		{
			int m=l+(r-l)/2;
			int t=guess(m);
			if(t==0) return m;
			if(t<0) //the number is lower
				r=m-1;
			else l=m+1;
		}
		return l; //exit when l>r, not found
    }
```

### 367. Valid Perfect Square	
equiv. to find the square root x so x*x==N
x is positive number

1. range: l=1,r=n; 
2. we are searching for x*x==n which is exact match. so from 1 to n, left is all false, and right is all false. 
and our answer is the low.
3. when x*x<n, we need set l=m+1 (since it is false)
when x*x>n: we need set r=m-1 (since it is false)
4. exit condition when l>r (l==r need to be checked)

```cpp
	bool isPerfectSquare(int num) {
		int l=1,r=num;
		while(l<=r)
		{
			int m=l+(r-l)/2;
			long t=(long)m*m;
			if(t==num) return 1;
			if(t<num) l=m+1;
			else r=m-1;
		}
		return 0;
	}
```

### 441. Arranging Coins
to find the m so: n>=m*(m+1)/2. which is the lower_bound
answer in [l,r): l*(l+1)/2<=n<r*(r+1)/2
1. range from 1 to n. f(1),f(2)....f(m)=m*(m+1)/2
2. we are searching for the last f(m)<=n or equivalently the one before the first one f(m)>n
assuming we are using f(m)<=n for the last one:
f(m)<=n: lo=m; (m guarantee it is true)
f(m)>n: r=m-1 (m guarantee it is false)
3. iteration ends when l+1==r.
Note: n could be very large and high=n cannot guarantee that the condition is false. so there is a problem.
to guarantee the condition: we need make l=0 and r=n+1.
```cpp
    int arrangeCoins(int n) {
        long low = 0, high = (long)n+1;
        while (low+1 < high) {
            long mid = low + (high - low ) / 2;
            if ((mid + 1) * mid / 2.0 <= n) low = mid;
            else high = mid;
        }
        return low;
    }
```

The following code can handle 0, 1 no problem
since r=m-1 to go to the other end. (it can handle all 1 or all 0 cases)
	
```cpp
    int arrangeCoins(int n) {
        long low = 1, high = n;
        while (low < high) {
            long mid = low + (high - low + 1) / 2;
            if ((mid + 1) * mid / 2.0 <= n) low = mid;
            else high = mid - 1;
        }
        return high;
    }
```
above solution use [l,r) or [l,r-1] to find the last one which satisfies m*(m+1)<=n.
mid=l+(r-l+1)/2 is used to break the infinite loop
since l+(r-l)/2 always choose the first element in the group, add 1 to round instead of down.
test it using two element input to see if we fall into infinite loop

or
```cpp
    int arrangeCoins(int n) {
		int l=0,r=n;
		while(l<=r)
		{
			int m=l+(r-l)/2;
			long t=(long)m*(m+1)/2;
			if(t<=n) l=m+1;
			else r=m-1;
		}
		return l-1;
	}
```
above solution finds the one > n so the answer is l-1
i.e. m*(m+1)>n, and the one ahead it m*(m-1)<=n
so when <=n, we go ahead
when >n: we reduce it, but why is r=m-1?

### 69. Sqrt(x)
similar to the perfect square number. but now we are asking for the last <=
binary search to get y*y<=x
m*m<=x, l=m
m*m>x, r=m-1

invariant: answer is in the range [l,r). and exit l==r
```cpp
    int mySqrt(int x) {
		int l=0,r=x;
		while(l<r)
		{
			int m=l+((long)r-l+1)/2;
			long t=(long)m*m;
			if(t<=x) l=m;
			else r=m-1;
		}
		return l;
	}
```
to avoid overflow: when r=INT_MAX
exit condition l==r

### 278. First Bad Version
this is the get the first version return true
invariant: [l,r] where l is 0 and r is 1. getting the first one return 1.
exit condition: l==r 
```cpp
	int firstBadVersion(int n) {
		int l=1,r=n;
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(isBadVersion(m)) r=m;
			else l=m+1;
		}
		return r;
	}
```
will fall into infinite loop? l=1,r=2, m=1,
if true, r=1
if false, l=2 

### 349. Intersection of Two Arrays
input contains duplicates. not sorted.

approach 1: hashset is trivial O(n)
approach 2: sort the two arrays and use merge sort A>B A<B and A==B 3 cases
approach 3: sort the two arrays and use binary search (stl or by hand search)

not a good example since binary search is not as good as O(n) algorithm.

#### 167. Two Sum II - Input array is sorted
approach 1: put into hashmap and find target-node
approach 2: two pointer to shrink range
approach 3: binary search, iterate on all elements and do binary search for target-numbers. less efficient.

### 350. Intersection of Two Arrays II
contain duplicates and need output the duplicates
approach 1: hashmap to increase, and other to decrease
approach 2: sort both array and merge sort, not the best

```cpp
	vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
		sort(nums1.begin(),nums1.end());
		sort(nums2.begin(),nums2.end());
		int i=0,j=0;
		vector<int> ans;
		while(i<nums1.size() && j<nums2.size())
		{
			if(nums1[i]==nums2[j]) {ans.push_back(nums1[i]);i++,j++;}
			else if(nums1[i]<nums2[j]) i++;
			else j++;
		}
		return ans;
	}
```
sort complexity O(nlogn), merge sort O(n+m)

### 475. Heaters
given a list of house positions and a list of heater positions
find the min radius to cover all houses
sort the heaters and locate the heater for each house. get the smallest difference and get the global max


## medium

### 1011. Capacity To Ship Packages Within D Days
to get the min capacity to have all the weights shipped in D days
idea: 
1. the ship can carry one or more. The capacity is limited by the max element and the average Sum/D
2. the max capacity is limited by the total
3. we are searching for the capacity so we can divide the weights into D days
4. for different capacity we may have duplicate days. we are looking for the first capacity which satisfy the D days
for smaller capacity, it will get more days
for larger capacity, it will get less days
so the condition would be 0,0,0...1,1,..1,0,0....0. (wrong)
actually it shall be 0,0,0,...1,1,.....1 (all <=D is true)

```cpp
	int shipWithinDays(vector<int>& weights, int D) {
		int total=accumulate(weights.begin(),weights.end(),0);
		int mn=max((total+D/2)/D,*max_element(weights.begin(),weights.end()));
		int l=mn,r=total;
		while(l<r)
		{
			int m=l+(r-l)/2;
			int days=get_days(weights,m);
			if(days>D) l=m+1;
			//else if(days<D) r=m-1;//this shall be removed, check what the problem asks
			else r=m;
		}
		return l;
	}
	int get_days(vector<int>& weights,int cap)
	{
		//moving sum 
		int ndays=0,sum=0;
		for(int w: weights)
		{
			if(sum+w<=cap) sum+=w;
			else
			{
				ndays++;
				sum=w;
			}
		}
		if(sum) ndays++;
		return ndays;
	}
```
when reaching final [1,1], m=0, l=0 and will cause infinite loop so we need use < in the loop
check what the problem needs: it needs <=D days! so we need change to <=D r=m


### 230. Kth Smallest Element in a BST
approach 1: inorder traverse and count to O(K)
approach 2: binary search
idea:
1. count left nodes first
2. if k<num nodes in left, search in the left subtree
else search for k-1-nleft in the right subtree
best O(N), up to O(N^2) since node are repeatedly visited.
```cpp
    int kthSmallest(TreeNode* root, int k) {
        int nleft=num_nodes(root->left);
        if(nleft>=k) return kthSmallest(root->left,k);
        else if(k>nleft+1) return kthSmallest(root->right,k-1-nleft);
        return root->val;
    }
    int num_nodes(TreeNode* root)
    {
        if(!root) return 0;
        return 1+num_nodes(root->left)+num_nodes(root->right);
    }
```

### 981. Time Based Key-Value Store
store key, value, ts
get(key,time) to return the key value pair with ts<=time
multiple values: return the one with largest timestamp
use a unordered_map<string,map<int,string>> structure and binary search (upper_bound is easier)
sort by the timestamp. 
note 1: map has member upper_bound, cannot use algorithm upper_bound
note 2: if it is mp[key].begin() we need return ""


### 287. Find the Duplicate Number
count the number of numbers <=m
if cnt<=m, indicating m is larger (not in the smaller side) l=m+1
else m could be an answer so r=m

O(nlogn)

```cpp
    int findDuplicate(vector<int>& nums) {
        int l=0,r=nums.size();
        while(l<r)
        {
            int m=l+(r-l)/2;
            int cnt=0;
            for(int n: nums)
                if(n<=m) cnt++;
            if(cnt<=m) l=m+1;
            else r=m;
        }
        return l;
    }
```

O(N) algorithm treating this as a linked list, element value as the next pointer
try to detect a cycle and find the intersection node.

### 378. Kth Smallest Element in a Sorted Matrix	
rows and columns are sorted.
2d matrix
kth element in sorted order.

approach 1: merge sort 
approach 2: min heap using priority queue
first build a min heap from first row (store val,x,y)
pop the min, and push the next element in the same column.
the first min is (0,0), the 2nd min is (0,1) or (1,0), the 3rd is (0,2),(1,1),(1,2)

```cpp
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int  n=matrix.size();
        priority_queue<vector<int>> pq;
        for(int j=0;j<n;j++) pq.push({-matrix[0][j],0,j});
        for(int i=0;i<k-1;i++)
        {
            vector<int> t=pq.top();
            pq.pop();
            int x=t[1],y=t[2];
            if(x==n-1) continue;
            pq.push({-matrix[x+1][y],x+1,y});
        }
        return -pq.top()[0];
    }
```
this could be O(n^2logn) each insert into the pq, needs logn

binary search:
similar idea as 287
we count the number <=mid
count in a sorted row/col does not need traverse each number

```cpp
	int kthSmallest(vector<vector<int>>& matrix, int k) {
		int n=matrix.size();
		int l=matrix[0][0],r=matrix[n-1][n-1]+1;
		while(l<r)
		{
			int m=l+(r-l)/2;
			int cnt=0;
			//count number of elements <=mid
			int c=n-1;
			for(int r=0;r<n;r++)
			{
				while(c && matrix[r][c]>m) c--;
				cnt+=c+1; //always add 1 or more into it
			}
			if(cnt<k) l=m+1;
			else r=m;
		}
		return l;
	}
```	
fail with [[1,2],[3,3]]
2
need change while(c>=0 && ... to fix the bug.
how do we guarantee that lo is an element in the matrix??
assuming we found a m which satisfy the k, but m is not an element inside
will move r=m, and keep approaching the element. shrinking the range to 1

so does not matter the number of elements inside, but only matters the range (max-min)

### 392. Is Subsequence
check if s is subsequence of t
s is a short string <100
t is long string ~5e5

approach 1: two pointer, greedy, O(n+m)
```cpp
	bool isSubsequence(string s, string t) {
		int i=0,j=0;
		while(i<s.size() && j<t.size())
		{
			if(s[i]==t[j]) i++;
			j++;
		}
		return i==s.size();
	}
```

another approach:
build 26 char with all its index
binary search the index to see if we can match an index
this is good for a lot of incoming s
```cpp
	bool isSubsequence(string s, string t) {
		vector<vector<int>> mp(26);
		for(int i=0;i<t.size();i++)
		{
			int ind=t[i]-'a';
			mp[ind].push_back(i);
		}
		int start=-1;
		for(char c: s)
		{
			vector<int>& vt=mp[c-'a'];
			int ind=upper_bound(vt.begin(),vt.end(),start)-vt.begin();
			if(ind==vt.size()) return 0;
			start=vt[ind];//real index
		}
		return 1;
	}
```

### 911. Online Election
given input people, and time of vote
query the top candidate at time t.

if there is a tie, return the person who has the most recent vote.

approach:
maintain a data structure for each person, t, and number of votes
unordered_map<int,vector<int,int>>

```cpp
	unordered_map<int,vector<int>> mp; //person vs time
	TopVotedCandidate(vector<int> persons, vector<int> times) {
		for(int i=0;i<persons.size();i++)
			mp[persons[i]].push_back(times[i]);
	}
	int q(int t) {
        int recent,mostvoted=0;
		int ans=0;
		for(auto tt: mp)
		{
			vector<int>& vt=tt.second;
			int ind=upper_bound(vt.begin(),vt.end(),t)-vt.begin();
			if(mostvoted<=ind)
			{
				if(ind && mostvoted==ind && vt[ind-1]>recent)
				{
					recent=vt[ind-1];
					ans=tt.first;
				}
				if(mostvoted<ind)
				{
					mostvoted=ind;
					ans=tt.first;
					recent=vt[ind-1];
				}
			}
		}
		return ans;
    }
```
TLE: we shift the work all in the query and it is not reasonable. This function is frequently called.

Improve:
when building the hashmap we build at each time who is the leader. And query becomes really easy

```cpp
    map<int, int> m; //time vs leader
    TopVotedCandidate(vector<int> persons, vector<int> times) {
        int n = persons.size(), lead = -1;
        unordered_map<int, int> count; //person vs vote
        for (int i = 0; i < n; ++i) m[times[i]] = persons[i];
        for (auto it : m) {
            if (++count[it.second] >= count[lead])lead = it.second;
            m[it.first] = lead;
        }
    }

    int q(int t) {
        return (--m.upper_bound(t))-> second;
    }
```	
			
### 718. Maximum Length of Repeated Subarray
max length of subarray in two arrays
approach 1: sliding window match sliding length n+m, each window compare max(n,m)
approach 2: dp for longest common substring
approach 3: rolling hash, really hard to understand.
dp is the way to go
dp[i,j] is the longest common subarray for A1[0...i-1] and A2[0..j-1]
when a1[i]==a2[j] then dp[i+1,j+1]=dp[i,j]+1

```cpp
    int findLength(vector<int>& A, vector<int>& B) {
		int m=A.size(),n=B.size();
		vector<vector<int>> dp(m+1,vector<int>(n+1));
		int ans=0;
		//boundary: 0th row and 0th col
		//0th row: empty vs B, 0th col: A vs empty
		for(int i=1;i<=m;i++)
		{
			for(int j=1;j<=n;j++)
			{
				if(A[i-1]==B[j-1]) dp[i][j]=dp[i-1][j-1]+1;
				ans=max(ans,dp[i][j]);
			}
		}
        return ans;
    }
```	

### 875. Koko Eating Bananas
a list of bananas
eating speed K bananas/hour
if the pile has less than k bananas, it just finish it and does not eat more
You have H hours
return the min K so that it can eat all bananas

Some big piles it may need several hours to finish.

very similar to the ship capacity
min is the max of max element and average
max is the total sum
find the first one satisfying H hours

```cpp
    int minEatingSpeed(vector<int>& piles, int H) {
		int l=1,r=accumulate(piles.begin(),piles.end(),0l);
		while(l<r)
		{
			long m=l+(r-l)/2;
			int hrs=get_time(piles,m);
			if(hrs>k) l=m+1;
			else r=m;
		}
		return l;
    }
	int get_time(vector<int>& piles,int k)
	{
		int ans=0;
		for(int p: piles)
			ans+=p/k+(p%k!=0);
		return ans;
	}
```
pay attention to overflow

### 153. Find Minimum in Rotated Sorted Array
this is a very classic binary search problem.
the answer lies [0,n-1] inclusive

with duplicates or without duplicates
A[m-1]>A[m]<A[m+1].
1. initial boundary is [0, n-1]
2. how to shrink the range
the left will shift from 0 to the pivot and cannot move any more
the right will shift from n-1 to the pivot and cannot move any more
the pivot is a part of the right. so r=m
we can only compare with the right value, otherwise when left move to pivot and it compare with the right
the left will shift to the right and miss the range.

3. when to stop? l<r or l<=r? == the invariant does not hold we need use <.

```cpp
     int findMin(vector<int>& nums) {
		int l=0,r=nums.size()-1;
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(nums[m]<nums[r]) r=m; //<= also works since there is no duplicate
			else l=m+1;
		}
		return nums[l];
    }
```	

### 154. Find Minimum in Rotated Sorted Array II
with duplicates
the pivot must satisfy A[p-1]>A[p]<=A[p+1]
if all the same, any position is OK.

1. initial condition is [0, n-1]
2. similarly we need design how to shrink the range to the pivot
the special case A[m]==A[r], we need shrink the right by 1 

3. l<r when l==r break the loop

```cpp
	int findMin(vector<int>& nums) {
		int l=0,r=nums.size()-1;
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(nums[m]<nums[r]) r=m;
			else if(nums[m]>nums[r]) l=m+1;
			else r--;//nums[m]==nums[r], this same thing cannot be the solution
		}
		return nums[l];
	}
```







	




  


	
		

	

