## contest 119
973. K Closest Points to Origin3
974. Largest Perimeter Triangle4
975. Subarray Sums Divisible by K6
976. Odd Even Jump


973. K Closest Points to Origin.md

### Problem Summary
Given a series of points in 2d plane return the list of k points closest to origin

### approach
using a priority_queue of size k, keeping popping out the max

### code
```cpp
    vector<vector<int>> kClosest(vector<vector<int>>& points, int K) {
        priority_queue<pair<long,int>> pq;
        for(int i=0;i<points.size();i++)
        {
            long d=points[i][0]*points[i][0]+points[i][1]*points[i][1];
            pq.push(make_pair(d,i));
            if(pq.size()>K) pq.pop();
        }
        vector<vector<int>> ans;
        while(pq.size()) ans.push_back(points[pq.top().second]),pq.pop();
        reverse(ans.begin(),ans.end());
        return ans;
    }
```

974. Subarray Sums Divisible by K.md

### Problem summary
Given an array A of integers, return the number of (contiguous, non-empty) subarrays that have a sum divisible by K.

### Approach
This could be done using dp or simple approach using hashmap
prefix sum and mod by K, the same results will have [i,j] to be a section

### code
```cpp
    int subarraysDivByK(vector<int>& A, int K) {
        A[0]=(A[0]%K+K)%K;
        for(int i=1;i<A.size();i++) 
        {
            A[i]+=A[i-1];
            A[i]=(A[i]%K+K)%K;
        }
        sort(A.begin(),A.end());
        
        int ans=0,cnt=0;
        for(int i=0;i<A.size();i++)
        {
            if(A[i]==0) ans++;
            if(i && A[i]==A[i-1]) cnt++,ans+=cnt;
            else cnt=0;
        }
        return ans;
    }
```    

### comment
- there are negatives, so first %K and +K and mod to convert to [0,k-1]


976. Largest Perimeter Triangle.md

### Problem Summary
Given a list of side length, find the triangle with max perimeter

### approach
a triangle must satisfy: a+b>c (a-b<c is equivalent)
sort in descending order, then we find the first a[i]<a[i+1]+a[i+2].

### code
```cpp
    int largestPerimeter(vector<int>& A) {
       //a triangle: a+b>c a-b<c
        sort(A.begin(),A.end(),greater<int>());
        for(int i=0;i<A.size()-2;i++)
        {
            if(A[i]<A[i+1]+A[i+2]) return A[i]+A[i+1]+A[i+2];
        }
        return 0;
    }
```

975. Odd Even Jump.md

### Problem summary
Given a list of numbers, you can start at some indices and make a series of jump:
odd jump: you jump to smallest index which is >= current
even jump: you jump to smallest index which is <=current
An index is good if you can jump to the end. 
return all the good indices.

### Approach
If not good to do it in one direction, what about the reverse direction?
Actually it is much easier, take a small example:
[10,13,12,14,15]
take 15: it is already the end, odd=1, even=1, +1
take 14: 1 step to end, odd=1, even=0, +1
take 12: 1 step to 14, odd=1, even=0, +1
take 13: odd you have to first jump to 14, but it is even jump, even has to jump to 12, odd=0, even=0
take 10: odd jump to 13, even jump: no legal, but 13 is not a good jump.

So it is a dp: 
we can put the visited nodes into a sorted order and then binary search to find the next number to jump for odd and even jump and check if it is legal.

There is one thing I missed: it also requires the first jump is odd, second jump is even!!!!!


```cpp
    int oddEvenJumps(vector<int>& A) {
        int n  = A.size(), res = 1;
        vector<int> higher(n), lower(n);
        higher[n - 1] = lower[n - 1] = 1;
        map<int, int> map;
        map[A[n - 1]] = n - 1;
        for (int i = n - 2; i >= 0; --i) {
            auto hi = map.lower_bound(A[i]), lo = map.upper_bound(A[i]);
            if (hi != map.end()) higher[i] = lower[hi->second];
            if (lo != map.begin()) lower[i] = higher[(--lo)->second];
            if (higher[i]) res++;
            map[A[i]] = i;
        }
        return res;
    }
```

