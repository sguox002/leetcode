## contest 177

### 1351. Count Negative Numbers in a Sorted Matrix
<em>Given a m * n matrix grid which is sorted in non-increasing order both row-wise and column-wise. 

Return the number of negative numbers in grid.
m,n<100
</em>

Intution:
100x100 is not large, brutal force is sufficient.
```cpp
    int countNegatives(vector<vector<int>>& grid) {
        int ans=0;
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                ans+=grid[i][j]<0;
            }
        }
        return ans;
    }
```
binary search (for decreasing array)
```cpp
    int countNegatives(vector<vector<int>>& grid) {
        int ans=0;
		for(auto r: grid){
			auto it=upper_bound(r.rbegin(),r.rend(),-1)-r.rbegin();
			ans+=it;
		}
        return ans;
    }
```

### 1352. Product of the Last K Number
<em>
Implement the class ProductOfNumbers that supports two methods:

1. add(int num)

Adds the number num to the back of the current list of numbers.
2. getProduct(int k)

Returns the product of the last k numbers in the current list.
You can assume that always the current list has at least k numbers.
At any time, the product of any contiguous sequence of numbers will fit into a single 32-bit integer without overflowing.
</em>
Intuition:
- brutal force simple.
- prefix product and then using division
- prefix if there is 0 inside, it will break the property. The trick is reinitinialize the prefix array.
```cpp
    vector<int> A = {1};
    void add(int a) {
        if (a)
            A.push_back(A.back() * a);
        else
            A = {1};
    }

    int getProduct(int k) {
        return k < A.size() ? A.back() / A[A.size() - k - 1]  : 0;
    }
```
The prefix solution is 10 times faster than the brutal force.

### 1353. Maximum Number of Events That Can Be Attended
<em>
Given an array of events where events[i] = [startDayi, endDayi]. Every event i starts at startDayi and ends at endDayi.

You can attend an event i at any day d where startTimei <= d <= endTimei. Notice that you can only attend one event at any time d.

Return the maximum number of events you can attend.	
</em>

Intuition:
- only sort by start or ending is not sufficient to solve the problem
- greedy approach: for day i, we choose the event with earliest ending date from all those event starting<=i.
- so we first sort by starting date, and then use min heap to find the earliest ending date event.
- once we choose a date, we need pop all those events which does not meet our criteria.

```cpp
    struct comp{
        bool operator()(vector<int>& a,vector<int>& b){
            return a[1]>b[1] || (a[1]==b[1] && a[0]>b[0]);
        }
    };
    int maxEvents(vector<vector<int>>& events) {
        int ans=0;
        //sort by the beginning time
        sort(events.begin(),events.end());
        //print(events);
        //add and sort by ending time
        priority_queue<vector<int>,vector<vector<int>>,comp> pq;
        //check all days and choose the one ending first.
        int i=0;
        int start=events[i][0];
        while(i<events.size()){
            while(i<events.size() && events[i][0]<=start) pq.push(events[i++]);
            if(pq.size()){
                auto e=pq.top();
                
                pq.pop();
                if(e[1]>=start) ans++;
                //cout<<start<<" "<<e[0]<<" "<<e[1]<<" "<<ans<<endl;
                start++;
                while(pq.size() && pq.top()[1]<start) pq.pop();
            }
            else start++; 
        }
        while(pq.size()){
            auto e=pq.top();
            pq.pop();
            if(e[1]>=start) ans++;
            start++;
        }
        return ans;
    }
```	
- the code could be more concise.

### 1354. Construct Target Array With Multiple Sums
<em>
Given an array of integers target. From a starting array, A consisting of all 1's, you may perform the following procedure :

let x be the sum of all elements currently in your array.
choose index i, such that 0 <= i < target.size and set the value of A at index i to x.
You may repeat this procedure as many times as needed.
Return True if it is possible to construct the target array from A otherwise return False.
</em>

Intution:
- goes reverse way from the current array to all ones array
- the max element is the previous sum: sum+sum-a[i]=cur_sum. Using pq to find the max.
- there is a case where we add to the same element a lot of times, and we shall optimize this, otherwise it will TLE.
- for the above case:
assume t=cur_sum, s=sum, 2*s-a[i]=t, a[i]=2*s-t
next step, a[i]->s, s->t, a[i]'=2*a[i]-s=2*(2*s-t)-s=3*s-2*t=2s-t+(s-t)
next step, a[i]'->s, a[i]->t, a[i]''=2*a[i]'-a[i]=2*(3s-2t)-2s+t=4s-3t=2s-t+2(s-t)
....
this goes to: (2s-t)%(s-t)

```cpp
    bool isPossible(vector<int>& target) {
        priority_queue<long> pq;
        long s(0);
		// push each element into max-heap, also sum all elements
        for (const int& i: target) {
            pq.push(i);
            s += i;
        }
        
        while (pq.top() != 1) { // until every element in heap is 1
			// we pop the largest from heap
            long t = pq.top();
            pq.pop();
			
			// (special case) by induction, [n, 1] -> [n-1, 1] -> ... -> [2, 1] -> [1, 1] is always possible
			if (s - t == 1) return true;
			// if it cannot be formed by other numbers in heap, return false
            if (s-t<1 || (2 * t - s) % (s - t) <= 0) return false;
			
			// otherwise, we push the previous number (possibly from many steps before / accelerate by module)
            pq.push((2 * t - s) % (s - t));
			// update current sum
            s += (2 * t - s) % (s - t) - t;
        }
        
        return true;
    }
```	
complexity O(nlogn)

The max drops to 1 exponentially, using max_element will be O(N)
```cpp
    bool isPossible(vector<int>& t) {
        auto s = accumulate(begin(t), end(t), (long long)0);
        auto i = max_element(begin(t), end(t)) - begin(t);
        while (s != 1 && t[i] > s / 2) {
            s -= t[i];
            if(s<1) return 0;
            t[i] = t[i] % s;
            s += t[i];
            i = max_element(begin(t), end(t)) - begin(t);
        }
        return s == 1 || s == t.size();
    }
```	


