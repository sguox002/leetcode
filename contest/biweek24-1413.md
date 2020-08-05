## biweek 24

### 1413. Minimum Value to Get Positive Step by Step Sum
<em>
Given an array of integers nums, you start with an initial positive value startValue.

In each iteration, you calculate the step by step sum of startValue plus elements in nums (from left to right).

Return the minimum positive value of startValue such that the step by step sum is never less than 1.
</em>

simple math and prefix sum
```cpp
    int minStartValue(vector<int>& nums) {
        int prefix=0;
        int ans=0;
        for(int i: nums){
            prefix+=i;
            ans=min(ans,prefix);
        }
        return max(1-ans,1);
    }
```

### 1414. Find the Minimum Number of Fibonacci Numbers Whose Sum Is K
<em>
Given the number k, return the minimum number of Fibonacci numbers whose sum is equal to k, whether a Fibonacci number could be used multiple times.

The Fibonacci numbers are defined as:

F1 = 1
F2 = 1
Fn = Fn-1 + Fn-2 , for n > 2.
It is guaranteed that for the given constraints we can always find such fibonacci numbers that sum k.
1 <= k <= 10^9
</em>

Intuition: greedy approach
always pick the largest one<=n and then reduce to a smaller subproblem

approach 1: precalculate the fib series (since k<1e9, this will not be long) and then find the largest one <=n

approach 2: recursive
```cpp
int findMinFibonacciNumbers(int k) {
    int f1 = 0, f2 = 1;
    while (f2 <= k) {
        swap(f1, f2);
        f2 += f1;
    }
    return 1 + (k == f1 ? 0 : findMinFibonacciNumbers(k - f1));
}
```

### 1415. The k-th Lexicographical String of All Happy Strings of Length n
<em>
A happy string is a string that:

consists only of letters of the set ['a', 'b', 'c'].
s[i] != s[i + 1] for all values of i from 1 to s.length - 1 (string is 1-indexed).
For example, strings "abc", "ac", "b" and "abcbabcbcb" are all happy strings and strings "aa", "baa" and "ababbc" are not happy strings.

Given two integers n and k, consider a list of all happy strings of length n sorted in lexicographical order.

Return the kth string of this list or return an empty string if there are less than k happy strings of length n.
</em>

backtrack approach:

```cpp
    string getHappyString(int n, int k) {
        //backtrack
        string ans;
        backtrack(n,k,"",ans);
        return ans;
    }
    void backtrack(int n,int& k,string t,string& ans){
        if(t.size()==n){
            if(--k==0) {ans=t;}
            return;
        }
        for(char c='a';c<='c';c++){
            if(t.size() && c==t.back()) continue;
            t+=c;
            backtrack(n,k,t,ans);
            t.pop_back();
        }
    }
```

### 1416. Restore The Array
<em>
A program was supposed to print an array of integers. The program forgot to print whitespaces and the array is printed as a string of digits and all we know is that all integers in the array were in the range [1, k] and there are no leading zeros in the array.

Given the string s and the integer k. There can be multiple ways to restore the array.

Return the number of possible array that can be printed as a string s using the mentioned program.

The number of ways could be very large so return it modulo 10^9 + 7
</em>
Intuition: dp problem, each number can attach to previous or start a new number.
```cpp
    int numberOfArrays(string s, int k) {
        //dp, current digit can be attached to previous or not
        int n=s.size();
        int mod=1e9+7;
        string sk=to_string(k);
        vector<long> dp(n+1);
        dp[0]=1;//empty string
        for(int i=1;i<=n;i++){ //length
            for(int j=i-1;j>=0 && i-j<=9;j--){ //attach to previous, j is th index
                if(j && s[j]=='0') continue; //cannot use it as leading char
                
                long num=stol(s.substr(j,i-j));
                if(num<=k) //add one combination, and it is same as previous
                    dp[i]+=dp[j],dp[i]%=mod;
                else break;
            }
        }
        return dp[n]%mod;
    }
```	


	
