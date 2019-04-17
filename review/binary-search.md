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



	

