1. define the ranges and conditions
2. define the exit condition
3. invariant condition

### 744. Find Smallest Letter Greater Than Target
upper_bound

invariant: answer is in [l,r), r shall include the n instead of n-1
answer is r. 
terminal condition l<r cannot be l<=r
A[m]>target: r=m (m could be the range)
A[m]<=target: l=m+1 (to guarantee target>A[l])

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
stl lower_bound
problem assumes no duplicates
if there are duplicates, we need to find the last one i.e a[l]<=a[m]<a[r]
the answer is [0,n] inclusive.
shrink the range and answer is l.
a[m]==target done
A[m]<target (not condition of A[m]<A[r]): l=m+1
A[m]>target: r=m

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

### 367. Valid Perfect Square	
equiv. to find the square root x so x*x==N
x is positive number

l=1,r=n; 
target in [l,r], which satisfy A[l]<=target<=A[r]
exit condition: l>r 
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

### 374. Guess Number Higher or Lower
just replace the condition with predefined function
invariant: a[l]<=target<=a[r]

```cpp
int guess(int num);
class Solution {
public:
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
};
```

### 441. Arranging Coins
to find the m so: n>=m*(m+1)/2. which is the lower_bound
answer in [l,r): l*(l+1)/2<=n<r*(r+1)/2
the answer is l
exit condition: l>=r 
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

so 

### 475. Heaters
given a list of house positions and a list of heater positions
find the min radius to cover all houses
sort the heaters and locate the heater for each house. get the smallest difference and get the global max


### 69. Sqrt(x)
binary search to get y*y<=x
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


	
	

	
