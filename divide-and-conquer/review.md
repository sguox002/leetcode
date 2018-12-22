Divide and conquer: divide the problem into several smaller sized similar problems and then assemble the results to get the final results.
Divide and conquer generally needs to consider two parts separately plus case where overlap the two cases. It generally make things much more complicated but will generally improve the efficiency from O(N) to O(logN)

169. Majority Element

find the element over [n/2] times.

- algorithm 1: sort and get the mid value.
- algorithm 2: voting: O(N) iterate over the array and remove different pairs, using count, when the same ++, otherwise --
- algorithm 3: divide and conquer, divide by half and get the majority element from left and right, choose the count larger. base case: the array only has one number.
- algorithm 4: hash table

53. Maximum Subarray

find the largest sum of the subarray
- dp: dp[i] is the max sum ending at i, then dp[i]=max(dp[i-1]+a[i],a[i])
- divide and conquer: 
the idea is accumulate to get the prefix sum and the max subarray sum is then the largest difference of the prefix sum.
for each subarray v1 and v2, we define the following:
  - l: the sum of the sub array with largest sum starting from the first element
  - m: the sum of the sub array with largest sum
  - r: the sum of the sub array with largest sum ending at the last element
  - s: the sum of the whole array
  then:
  l=max(v1.l,v1.s+v2.l) //appending the left to right
  m=max(v1.l+v2.r,max(v1.m,v2.m)) //subarray cross the v1 and v2, or in v1 or v2
  r=max(v2.r,v1.r+v2.s) //reverse direction
  s=v1.s+v2.s;
  base case: array of size=1.
  
932. Beautiful Array
a permutation of 1 to N, and there is no a[k]*2=a[i]+a[j] i<k<j
when we have a[k]*2!=a[i]+a[j]
-. we can add any number to it and get a new beautiful array
-. we can multiply a number to it and get a new beautiful array
-. a[i] is odd, a[j] is even, then a[k]*2 will satisfy the beatiful array.
instead of divide and conquer we use the reverse way to grow the array.
Assume we have a beautiful array:
A1=2*A-1 is also a beautiful array
A2=2*A is also a beautiful array
starting from a smallest beautiful array {1}.
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

241. Different Ways to Add Parentheses
a expression with numbers, + - *, by applying parentheses, we can get different results.
return all possible results.
The operators are the natural position for splitting the expression into left and right part and then we combine the left and right results.
```cpp
    unordered_map<string,vector<int>> memo;
    vector<int> diffWaysToCompute(string input) {
        if(input.size()==0) return vector<int>();
        vector<int> res;
        if(memo.count(input)) return memo[input];
        //divide the string into two parts and do the calculations
        for(int i=0;i<input.size();i++)
        {
            if(input[i]!='+' && input[i]!='-' && input[i]!='*') continue;

            vector<int> res1=diffWaysToCompute(input.substr(0,i)); 
            vector<int> res2=diffWaysToCompute(input.substr(i+1));
            //perform the operations between res1 and res2 using +-*
            for(int k=0;k<res1.size();k++)
            {
                for(int j=0;j<res2.size();j++)
                {
                    if(input[i]=='+') res.push_back(res1[k]+res2[j]);
                    else if(input[i]=='-') res.push_back(res1[k]-res2[j]);
                    else if(input[i]=='*') res.push_back(res1[k]*res2[j]);
                }
            }
            
        }
        if(res.empty()) res=vector<int>(1,stoi(input));
        memo[input]=res;
        return res; //when it has no results return the numbers
    }
```
- when there is no operator in the expression, we need return the number
- left and right combination produce l*r number of different results
- possibly get duplicate results but it is ok since we need different parentheses.

215. Kth Largest Element in an Array

return the kth largest element in its sorted order.
naive: sort and then get the k-th largest element. 


