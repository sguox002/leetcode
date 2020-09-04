## contest 108

929. Unique Email Addresses - hashmap
930 Binary Subarrays With Sum - sliding window
931 Minimum Falling Path Sum - dp
932 Beautiful Array - divide and conquer, greedy

### 929. Unique Email Addresses
<em>
Every email consists of a local name and a domain name, separated by the @ sign.

For example, in alice@leetcode.com, alice is the local name, and leetcode.com is the domain name.

Besides lowercase letters, these emails may contain '.'s or '+'s.

If you add periods ('.') between some characters in the local name part of an email address, mail sent there will be forwarded to the same address without dots in the local name.  For example, "alice.z@leetcode.com" and "alicez@leetcode.com" forward to the same email address.  (Note that this rule does not apply for domain names.)

If you add a plus ('+') in the local name, everything after the first plus sign will be ignored. This allows certain emails to be filtered, for example m.y+name@email.com will be forwarded to my@email.com.  (Again, this rule does not apply for domain names.)

It is possible to use both of these rules at the same time.

Given a list of emails, we send one email to each address in the list.  How many different addresses actually receive mails? 
</em>

```cpp
    int numUniqueEmails(vector<string>& emails) {
        unordered_set<string> acct;
        for(string s: emails){
            int ind=s.find_first_of('@');
            string user=s.substr(0,ind);
            string domain=s.substr(ind);
            string trueuser;
            for(char c: user){
                if(c=='.') continue;
                if(c=='+') break;
                trueuser+=c;
            }
            acct.insert(trueuser+domain);
        }
        return acct.size();
    }
```

### 930. Binary Subarrays With Sum
<em>
In an array A of 0s and 1s, how many non-empty subarrays have sum S?
</em>
sliding window. left 0 and right 0 will add to the subarray.
- approach 1: get the 1's indices and work on the new array
- using prefix sum and hashmap
```cpp
    int numSubarraysWithSum(vector<int>& A, int S) {
        unordered_map<int, int> c({{0, 1}});
        int psum = 0, res = 0;
        for (int i : A) {
            psum += i;
            res += c[psum - S];
            c[psum]++;
        }
        return res;
    }
```

sliding window:
we accumulate the sum, when it is >S, we advance the left pointer.

```cpp
    int numSubarraysWithSum(vector<int>& A, int S) {
        return atMost(A, S) - atMost(A, S - 1);
    }

    int atMost(vector<int>& A, int S) {
        if (S < 0) return 0;
        int res = 0, i = 0, n = A.size();
        for (int j = 0; j < n; j++) {
            S -= A[j];
            while (S < 0)
                S += A[i++];
            res += j - i + 1;
        }
        return res;
    }
```
The best of this: do not need complicated logic, but simple sliding window.

### 931. Minimum Falling Path Sum
<em>
Given a square array of integers A, we want the minimum sum of a falling path through A.

A falling path starts at any element in the first row, and chooses one element from each row.  The next row's choice must be in a column that is different from the previous row's column by at most one.
</em>

simple dp:

```cpp
    int minFallingPathSum(vector<vector<int>>& A) {
        //use the matrix A for dp
        int n=A.size();
        for(int i=1;i<n;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(j>0 && j<n-1)
                    A[i][j]+=min(min(A[i-1][j-1],A[i-1][j]),A[i-1][j+1]);
                else if(j==0) A[i][j]+=min(A[i-1][j],A[i-1][j+1]);
                else A[i][j]+=min(A[i-1][j],A[i-1][j-1]);
            }
        }
        //print(A);
        int ans=A[n-1][0];
        for(int i=0;i<n;i++) ans=min(ans,A[n-1][i]);
        return ans;
    }
```

### 932. Beautiful Array
<em>
For some fixed N, an array A is beautiful if it is a permutation of the integers 1, 2, ..., N, such that:

For every i < j, there is no k with i < k < j such that A[k] * 2 = A[i] + A[j].

Given N, return any beautiful array A.  (It is guaranteed that one exists.)
</em>

thinking process:
- if A[i] is odd, A[j] is even, then A[i]+A[j] is odd, then no A[k]*2=A[i]+A[j]
- if A is beautiful array, 2*A is also a beautiful array-->this we can expand the range. 
for example odd=[1,3,5],even=[2,4,6], or equivalently odd=2i-1,even=2i. we only need to make i beautiful array
len=1: [1]
len=2: [1,2] 2i-1,2i i=1
len=4: [1,3,2,4], 2i-1, 2i, i=1,2
len=8: [1,5,3,7,2,6,4,8]
....

divide and conquer.

```cpp
    vector<int> beautifulArray(int N) {
        vector<int> res = {1};
        while (res.size() < N) {
            vector<int> tmp;
            for (int i : res) if (i * 2 - 1 <= N) tmp.push_back(i * 2 - 1);
            for (int i : res) if (i * 2 <= N) tmp.push_back(i * 2);
            res = tmp;
        }
        return res;
    }
```

	
	




