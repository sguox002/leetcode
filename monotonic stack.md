## Monotonic stack or deque

monotonic stack/deque is often used to implement O(N) algorithm (generally need O(N^2) otherwise).

when need push/pop from both ends, we need use monotonic deque.

this topic is generally hard, and not intuitive. It is critical to understand why we do this.

monotonic stack problem is mostly previous/next smaller/larger problems.

### Next smaller/larger problems
find the right first element which smaller or greater than current element.
- for next smaller: use monotonic increasing stack. When iterating to the right, if current element is smaller than stack top, then we get the first smaller elements for the stack top.
- for next greater: use monotnic decreasing stack and similar as above.
- generally we store index in the stack.
- each element is pushed once and poped once.

more similar problems:

[- 496	Next Greater Element I    		64.7%	Easy](https://leetcode.com/problems/next-greater-element-i/)
[- 503	Next Greater Element II    		57.5%	Medium	](https://leetcode.com/problems/next-greater-element-ii/)
[- 1019	Next Greater Node In Linked List    		58.2%	Medium](https://leetcode.com/problems/next-greater-node-in-linked-list/)
[- 739	Daily Temperatures    		64.1%	Medium	](https://leetcode.com/problems/daily-temperatures/)
[- 901	Online Stock Span    		60.9%	Medium	](https://leetcode.com/problems/online-stock-span/)
[- 1475	Final Prices With a Special Discount in a Shop    		75.0%	Easy](https://leetcode.com/problems/final-prices-with-a-special-discount-in-a-shop/)

example: 
[- 503	Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)
find next greater element in circular array.
	- idea: we first double the array to unwrap the circular array and then apply monotonic decreasing stack.

```
    vector<int> nextGreaterElements(vector<int>& nums) {
        stack<int> st; //store the index
        //monotonic decreasing
        int n=nums.size();
        for(int i=0;i<n;i++) nums.push_back(nums[i]);
        vector<int> ans(n,-1);
        for(int i=0;i<nums.size();i++){
            while(st.size() && nums[i]>nums[st.top()]){
                ans[st.top()%n]=nums[i%n];
                st.pop();
            }
            st.push(i);
        }
        return ans;
    }
```	

### Previous smaller/larger problem.
this is to find the left first elment which is smaller/larger than current element.
This can be converted to equivalent next larger/smaller problem.
why we need this? It can be applied in search the valid subarray to have the left and right the first previous smaller/larger and right first smaller/larger.
previus smaller equiv to next larger.
previous larger equiv to next smaller.

### Find the range for previous and next smaller/larger.
This can be solved using sliding window O(N^2).
- for finding previous and next smaller: 
using monotonic increasing stack, when we iterate to the right, and the element is smaller than stack top, then we update all the stack elements > current (current element is the first smaller). Also when the current element is able to push into the stack, we find the previous smaller for the current element.

example:
[907	Sum of Subarray Minimums    		33.3%	Medium](https://leetcode.com/problems/sum-of-subarray-minimums/)
return the sum of all subarray minimum.
idea: each element can be the minimum for some subarray. If we can find the next smaller and previous smaller for each element, then we get the subarray length for each element as the minimum.

```
    int sumSubarrayMins(vector<int>& A) {
        stack<int> st;
        int n=A.size();
        vector<int> left(n),right(n);//max length
        for(int i=0;i<n;i++) {
            left[i]=i+1;
            right[i]=n-i;
        }
        //get previous less and next less
        //increasing order
        for(int i=0;i<A.size();i++){
            while(st.size() && A[i]<A[st.top()]){
                int tp=st.top();st.pop();
                right[tp]=i-tp;
            }
            if(st.size()) left[i]=i-st.top(); //else no change
            
            st.push(i);
        }
        long ans=0;
        int mod=1e9+7;
        for(int i=0;i<A.size();i++){
            ans+=(long)left[i]*right[i]*A[i]%mod;
            ans%=mod;
        }
        return ans;
    }
```

similar problem:
[1793. Maximum Score of a Good Subarray](https://leetcode.com/problems/maximum-score-of-a-good-subarray/)

more monotonic stack problems:

[- 334	Increasing Triplet Subsequence    		39.9%	Medium	](https://leetcode.com/problems/increasing-triplet-subsequence/)
check if exist i<j<k and A[i]<A[j]<A[k]
idea: A[i]<A[j] shall be as small as possible so that A[k] can be easier found.
from left to right, keep elements in monotonic increasing stack, If current element < stack top, the bigger elements in stack are popped, so the top is kept as smallest as possible. When stack has >=2 elements, and current element > stack top, the pattern is found.

[- 456	132 Pattern    		30.5%	Medium](https://leetcode.com/problems/132-pattern/)
i<j<k and A[i]<A[k]<A[j]
similar to 334: A[i] shall be kept as small as possible, A[k] shall be as large as possible.
if we do from left to right, it is hard to keep A[i] smallest and A[j] largest.
if we do right from left, we want to pop those smaller elements, which is a monotonic decreasing stack.

[- 42	Trapping Rain Water    		50.1%	Hard](https://leetcode.com/problems/trapping-rain-water/)
rain water can only be contained by a smaller bar with left and right higher bar.
so this is equivalent to find previous and next greater element.

[- 84	Largest Rectangle in Histogram    		36.0%	Hard](https://leetcode.com/problems/largest-rectangle-in-histogram/)
the rectangle shall have all bars >=the smallest bar.
so it is equivalent to find the next and previous smaller bar. using each element as the shortest bar to find the rectangle area.

[- 85	Maximal Rectangle    		38.7%	Hard](https://leetcode.com/problems/maximal-rectangle/)
based on 84.
[- 1063	Number of Valid Subarrays    		71.7%	Hard	](https://leetcode.com/problems/number-of-valid-subarrays/)
[- 1081	Smallest Subsequence of Distinct Characters    		53.3%	Medium	](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/)
[- 316	Remove Duplicate Letters    		38.5%	Medium](https://leetcode.com/problems/remove-duplicate-letters/)
[- 1130	Minimum Cost Tree From Leaf Values    		67.1%	Medium	](https://leetcode.com/problems/minimum-cost-tree-from-leaf-values/)

## Monotonic increasing/decreasing deque

monotonic deque is often combined with sliding window with min/max.
general approach: 
- the front stores the min or max, and we add new elements to the back and keep the deque in increasing (max problem) or decreasing (min problem).
- pop old front elements when it is out of window.
- each element is pushed and popped once (either front or back)
- store index instead of values in the deque.

[- 239	Sliding Window Maximum    		44.1%	Hard	](https://leetcode.com/problems/sliding-window-maximum/)
```
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
	
[- 862	Shortest Subarray with Sum at Least K    		24.9%	Hard](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/)
with negatives.
	- get the prefix sum
	- range sum = prefix[i]-prefix[j] with j<i.
	- keep visited prefix sum in decreasing deque. pop all those front elements satisfying the condtion. (they are not needed since right elements will not be able to get shorter using these old values)
	- new elements can pop those larger than it since the old larger values cannot contribute shorter.

Please upvote it if it is helpful so that more people can see it. Thanks
