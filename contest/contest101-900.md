## contest 101

RLE Iterator4
Online Stock Span4
Numbers At Most N Given Digit Set7
Valid Permutations for DI Sequence

900. RLE Iterator
<em>
Write an iterator that iterates through a run-length encoded sequence.

The iterator is initialized by RLEIterator(int[] A), where A is a run-length encoding of some sequence.  More specifically, for all even i, A[i] tells us the number of times that the non-negative integer value A[i+1] is repeated in the sequence.

The iterator supports one function: next(int n), which exhausts the next n elements (n >= 1) and returns the last element exhausted in this way.  If there is no element left to exhaust, next returns -1 instead.

For example, we start with A = [3,8,0,9,2,5], which is a run-length encoding of the sequence [8,8,8,5,5].  This is because the sequence can be read as "three eights, zero nines, two fives".
</em>

approach: one array stores the value, one array stores the number (prefix sum).

```cpp
    vector<long long> v1;
    vector<int> vnum;
    long long cur;
    RLEIterator(vector<int> A) {
        cur=0;
        for(int i=0;i<A.size();i++)
        {
            if(i%2) vnum.push_back(A[i]);
            else v1.push_back(A[i]+(i==0?0:v1.back()));
        }
    }
    
    int next(int n) {
        cur+=n;
        //binary search to find the position
        auto it=lower_bound(v1.begin(),v1.end(),cur); //<=
        if(it==v1.end()) return -1;
        int ind=it-v1.begin();
        return vnum[ind];
    }
```

901. Online Stock Span
<em>
Write a class StockSpanner which collects daily price quotes for some stock, and returns the span of that stock's price for the current day.

The span of the stock's price today is defined as the maximum number of consecutive days (starting from today and going backwards) for which the price of the stock was less than or equal to today's price.

For example, if the price of a stock over the next 7 days were [100, 80, 60, 70, 60, 75, 85], then the stock spans would be [1, 1, 1, 2, 1, 4, 6].
</em>

monotonic stack

```cpp
    vector<int> prices;
    stack<int> st; //stack stores the index with decreasing order
    int days;
    StockSpanner() {
        days=0;
        st.push(0);
        prices.push_back(INT_MAX);
    }
    
    int next(int price) {
        prices.push_back(price);
        //if(st.empty()) {st.push(0);return 1;}
        //int last_day;
        while(st.size() && price>=prices[st.top()]) st.pop();
        days++;
        int ans;
        ans=days-st.top();
        st.push(days);
        return ans;
    }
```

902. Numbers At Most N Given Digit Set
<em>
You are given an array of digits, you can write numbers using each digits[i] as many times as we want.  For example, if digits = ['1','3','5'], we may write numbers such as '13', '551', and '1351315'.

Return the number of positive integers that can be generated that are less than or equal to a given integer n
</em>

Take N = 2563, D = {"1", "2", "6"} as an example,
The first loop handles the count of x, xx, xxx which x belongs to D. the sum is 3^1 + 3^2 + 3^3.
The second loop handles xxxx from left most digit.
For example,
count of 1xxx is 3^3
count of 21xx is 3^2
count of 22xx is 3^2

If the elements of D can compose entired N, answer + 1
Although it could be coded in a single loop, the logic would be unclear to me. I keep them seperated.

```cpp
class Solution {
public:
    int atMostNGivenDigitSet(vector<string>& D, int N) {
        string NS = to_string(N);
        int digit = NS.size(), dsize = D.size(), rtn = 0;
        
        for (int i = 1 ; i < digit ; ++i)
            rtn += pow(dsize, i);
        
        for (int i = 0 ; i < digit ; ++i) {
            bool hasSameNum = false;
            for (string &d : D) {
                if (d[0] < NS[i]) 
                    rtn += pow(dsize, digit - i - 1);
                else if (d[0] == NS[i]) 
                    hasSameNum = true;
            }
            if (!hasSameNum) return rtn;
        }               
        return rtn+1;
    }
};	
```

903. Valid Permutations for DI Sequence
<em>
We are given S, a length n string of characters from the set {'D', 'I'}. (These letters stand for "decreasing" and "increasing".)

A valid permutation is a permutation P[0], P[1], ..., P[n] of integers {0, 1, ..., n}, such that for all i:

If S[i] == 'D', then P[i] > P[i+1], and;
If S[i] == 'I', then P[i] < P[i+1].
How many valid permutations are there?  Since the answer may be large, return your answer modulo 10^9 + 7.
</em>

dp approach:

```cpp
    int numPermsDISequence(string S) {
        int mod=1e9+7;
        int n=S.length();
        vector<vector<int>> dp(n+1,vector<int>(n+1,0));
        //dp[i][j]: the number of permutation with length=i and end at number j
        for(int i=0;i<n+1;i++) dp[0][i]=1; //boundary: length=0
        for(int i=1;i<=n;i++)
        {
            for(int j=0;j<=i;j++)
            {
                if(S[i-1]=='D') //previous one shall be bigger
                {
                    for(int k=j;k<i;k++) {dp[i][j]+=dp[i-1][k];dp[i][j]%=mod;}
                }
                else //previous one shall be smaller
                {
                    for(int k=0;k<j;k++) {dp[i][j]+=dp[i-1][k];dp[i][j]%=mod;}
                }
            }
        }
        int ans=0;//length=n ending with all numbers.
        for(int i=0;i<n+1;i++) {ans+=dp[n][i];ans%=mod;}
        return ans;
    }
```
