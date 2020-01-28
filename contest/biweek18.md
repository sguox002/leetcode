## biweek 18
### 1331. Rank transform of an array
<em>Problem:

Given an array of integers arr, replace each element with its rank.

The rank represents how large the element is. The rank has the following rules:

Rank is an integer starting from 1.
The larger the element, the larger the rank. If two elements are equal, their rank must be the same.
Rank should be as small as possible.</em>

n up to 1e5.
intuition: cannot use O(N^2) algorithm. 
Approach: sort with index and then assign the id.

```cpp
    vector<int> arrayRankTransform(vector<int>& arr) {
        vector<vector<int>> vdata;
        for(int i=0;i<arr.size();i++){
            vdata.push_back({arr[i],i});
        }
        sort(vdata.begin(),vdata.end());
        vector<int> ans(arr.size());
        int id=0,prev=INT_MIN;
        for(auto t: vdata){
            if(t[0]!=prev){
                ans[t[1]]=++id;
                prev=t[0];
            }
            else ans[t[1]]=id;
        }
        return ans;
    }
```

### 1328. Break a Palindrome
<em>Problem:

Given a palindromic string palindrome, replace exactly one character by any lowercase English letter so that the string becomes the lexicographically smallest possible string that isn't a palindrome.

After doing so, return the final string.  If there is no way to do so, return the empty string.
</em>

Intuition:
- go only the half (avoid the mid element)
- find the first not a and replace it with a.
- otherwise replace the last one with b.
```cpp
    string breakPalindrome(string S) {
        int n = S.size();
        for (int i = 0; i < n / 2; ++i) {
            if (S[i] != 'a') {
                S[i] = 'a';
                return S;
            }
        }
        S[n - 1] = 'b';
        return n < 2 ? "" : S;
    }
```

### 1329. Sort the Matrix Diagonally	
<em>Problem:

Given a m * n matrix mat of integers, sort it diagonally in ascending order from the top-left to the bottom-right then return the sorted array.
</em>

Intuition: sort using map to put each diagonal with key i*n+j (with i or j to be 0)
```cpp
    vector<vector<int>> diagonalSort(vector<vector<int>>& mat) {
        //diagnonal: dx=dy
        unordered_map<int,multiset<int>> mp;
        int m=mat.size(),n=mat[0].size();
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                int x=i-min(i,j),y=j-min(i,j);
                mp[x*n+y].insert(mat[i][j]);
            }
        }
        for(auto t: mp){
            int ind=0;
            int x=t.first/n,y=t.first%n;
            for(int i: t.second) mat[x][y]=i,x++,y++;
        }
        return mat;
    }
```

### 1330. Reverse Subarray To Maximize Array Value
<em>Problem:

You are given an integer array nums. The value of this array is defined as the sum of |nums[i]-nums[i+1]| for all 0 <= i < nums.length-1.

You are allowed to select any subarray of the given array and reverse it. You can perform this operation only once.

Find maximum possible value of the final array.
</em>

n up to 3e4.
Intuition: O(N^2) may not pass the test.
Brutal force will TLE:
```cpp
    int maxValueAfterReverse(vector<int>& num) {
        //reverse a subarray only change the two end difference
        //assuming reverse [i,j], the orginal diff is sum(abs(A[i]-A[i-1]))
        //now 0,1,....i-1,j....i,j+1....
        //abs(A[j]-A[i-1])+abs(A[j+1]-A[i])-abs(A[i]-A[i-1])-abs(A[j+1]-A[j])
        //we are maxmizing this sum
        int sum=0,n=num.size();
        for(int i=1;i<num.size();i++){
            sum+=abs(num[i]-num[i-1]);
        }
        int maxdiff=0;
        for(int i=0;i<num.size();i++){
            for(int j=i+1;j<num.size();j++){
                maxdiff=max(maxdiff,(i?abs(num[j]-num[i-1]):0)
                            +(j+1<n?abs(num[j+1]-num[i]):0)
                            -(i?abs(num[i]-num[i-1]):0)
                            -(j+1<n?abs(num[j+1]-num[j]):0));
            }
        }
        return sum+maxdiff;
    }
```

We need improve this algorithm into O(N)
- now we know the changes are obtained using above solution.
- if we want to O(N) we can only process neigboring elements. then we need to know how to accumulate to get the reverse [i,j].
- to make it simple, we let a=num[i-1],b=num[i],c=num[j],d=num[j+1]. the minus part only involves with i or j. max(|c-a|+|d-b|-|b-a|-|d-c|)
- to maximize it, we need minimize |b-a|+|d-c| and maximize |c-a|+|d-b|.
- remove abs operator by using abs(a)=max(a,-a).
max(|c-a|+|d-b|)=max(max(c-a,a-c)+max(d-b,b-d))=max(max(c-a+d-b,c-a+b-d,a-c+b-d,a-c+d-b))
min(|b-a|+|d-c|)=min(max(b-a,a-b)+max(d-c,c-d))=min(max(b-a+d-c,b-a+c-d,a-b+d-c,a-b+c-d))
reorganize to make them clearer:
max(max(-a-b+c+d,-a+b+c-d,a+b-c-d,a-b-c+d))
min(max(-a+b-c+d,-a+b+c-d,a-b-c+d,a-b+c-d))
common items in both expression:
-a+b+c-d
a-b-c+d


Lee's solution on this:

total calculate the total sum of |A[i] - A[j]|.
res record the value the we can improve.

Assume the current pair is (a,b) = (A[i], A[i+1]).

If we reverse all element from A[0] to A[i],
we will improve abs(A[0] - b) - abs(a - b)

If we reverse all element from A[i+1] to A[n-1],
we will improve abs(A[n - 1] - a) - abs(a - b)

As we iterate the whole array,
We also record the maximum pair and the minimum pair.
We can break these two pair and reverse all element in the middle.
This will improve (max2 - min2) * 2


```cpp
    int maxValueAfterReverse(vector<int>& A) {
        int total = 0, res = 0, min2 = 123456, max2 = -123456, n = A.size();
        for (int i = 0; i < n - 1; ++i) {
            int a = A[i], b = A[i + 1];
            total += abs(a - b);
            res = max(res, abs(A[0] - b) - abs(a - b));
            res = max(res, abs(A[n - 1] - a) - abs(a - b));
            min2 = min(min2, max(a, b));
            max2 = max(max2, min(a, b));
        }
        return total + max(res, (max2 - min2) * 2);
    }
```
	

