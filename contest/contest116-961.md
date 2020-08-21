## contest 116
961. N-Repeated Element in Size 2N Array2
962. Maximum Width Ramp5
963. Minimum Area Rectangle II5
964. Least Operators to Express Number

### 961. N-Repeated Element in Size 2N Array.md

using hashset

```cpp
    int repeatedNTimes(vector<int>& A) {
        unordered_set<int> mp;
        for(int i=0;i<A.size();i++)
        {
            if(mp.count(A[i])) return A[i];
            mp.insert(A[i]);
        }
    }
```

### 962. Maximum Width Ramp.md

i<j and A[i]<=A[j], the ramp width=j-i.
Find the max ramp width

I came up several solutions:
1. bundle the index and sort the array, then using two pointer method
2. using decreasing stack
3. using lmin and rmax and uses binary search

All these solutions are O(nlogn)

1. two pointer method
i=0, j=1
- when index[j]>index[i], get the distance and j++ else i++
- when i==j, j++

2. decreasing stack
```cpp
int maxWidthRamp(vector<int>& A, int res = 0) {
  vector<pair<int, int>> v;
  for (int i = 0; i < A.size(); ++i) 
  {
    if (v.empty() || A[i]<v.back().first) v.push_back({ A[i], i });
    else 
      res = max(res, i - lower_bound(v.begin(), v.end(), make_pair( A[i], INT_MAX ), 
        greater<pair<int, int>>())->second);
  }
  return res;
}
```
Note: we do not need pop (which confused me and have to give up this method)

3. lmin and rmax
Get the lmin from left to right.
get the rmax from right to left. For each rmax, using binary search to find the rmax>=lmin
```cpp
    int maxWidthRamp(vector<int>& A) {
        //two pointers?
        int n=A.size();
        vector<int> lmin(A.size());
        lmin[0]=A[0];
        for(int i=1;i<A.size();i++) lmin[i]=min(lmin[i-1],A[i]);
        reverse(lmin.begin(),lmin.end());
        int rmax=A.back();
        int maxdiff=INT_MIN;
        for(int i=A.size()-1;i>=0;i--) //when size==2 does not work
        {
            rmax=max(rmax,A[i]);
            int ind=upper_bound(lmin.begin(),lmin.end(),rmax)-lmin.begin();
            ind--;
            if(rmax>=lmin[ind]) maxdiff=max(maxdiff,i-(n-1-ind));
        }
        return maxdiff==INT_MIN?0:maxdiff;
    }
```
Note: we do not need to reverse the lmin by using lower_bound with greater comparing function.




### 963. Minimum Area Rectangle II.md

given a list of points, find the min rectangle area. If no rectangle, return 0

Naive: (N^4)
all combinations of 4 points to see if it can combine a rectangle.
we need first determine:
- the 4 points can form a rectangle
- how to calculate the area from the 4 points
This involves some math and is not easy to figure out.
Better use vector or complex number.
suppose we have p0,p1,p2,p3 check if it is a rectangle.
vector p0p1, p0p2, p0p3: check if there exist perpendicular vectors. 
- Actually P3 is not needed. If we have p0p1 perpendicular to p0p2 and their amplitude is the same, we are able to find the other point.
- furthermore we only need two points to form a vector and its perpendicular vector is also known (two options: we can calculate from equations, these involves double and find in set, which is a mess.)
- hashset to store the vectors (but how to calculate hash function?) or we can use set
using above we can improve the efficiency greatly.

So from the idea to correct code there is still a long way to go.

https://leetcode.com/problems/minimum-area-rectangle-ii/discuss/208361/JAVA-O(n2)-using-Map
This is a much better approach:
It uses the vector as the diagnonal axis, the center and the distance combined and compose a string and use it as the key, the value is the two point pairs.
So all those vectors with the same center and length is now under the same key. (a rectangle does not require the two diagonal axes are perpendicular)

```cpp
    double minAreaFreeRect(vector<vector<int>>& points) {
        unordered_map<string,vector<vector<int>>> mp; //key vs diagonal axis (two points' index)
        int n=points.size();
        for(int i=0;i<n;i++)
        {
            for(int j=i+1;j<n;j++)
            {
                int dx=points[i][0]-points[j][0];
                int dy=points[i][1]-points[j][1];
                long dist2=dx*dx+dy*dy;
                double cx=(points[i][0]+points[j][0])*0.5;
                double cy=(points[i][1]+points[j][1])*0.5;
                string s=to_string(dist2)+": "+to_string(cx)+", "+to_string(cy);
                //cout<<s<<endl;
                mp[s].push_back({i,j});
            }
        }
        double min_area=1e15;
        for(auto it=mp.begin();it!=mp.end();it++)
        {
            if(it->second.size()<2) continue;
            //there could be multiple rectangle inside
            //now we have 4 points in diagonal, p0-p2 diagonal, p1-p3 diagonal
            vector<vector<int>>& vrect=it->second;
            for(int i=0;i<vrect.size();i++)
            {
                for(int j=i+1;j<vrect.size();j++)
                {
                    int p0=vrect[i][0],p2=vrect[i][1];
                    int p1=vrect[j][0],p3=vrect[j][1];
                    int dx=points[p0][0]-points[p1][0];
                    int dy=points[p0][1]-points[p1][1];
                    long long dist01=dx*dx+dy*dy;
                    dx=points[p0][0]-points[p3][0];
                    dy=points[p0][1]-points[p3][1];
                    long long dist03=dx*dx+dy*dy;
                    double area=sqrt(double(dist01)*dist03);
                    min_area=min(min_area,area);
                }
            }
        }
        return min_area==1e15?0:min_area;
    }
```

Note:
-  with same center and same distance, there could be multiple rectangles (imagine the 4 points on the circle and the diagnonal are the diameter), so we need to find the min inside it.


### 964. Least Operators to Express Number.md

Given a x, and use +-*/ to reach a target, with not using ().
Get the least number of operators

I have not get any solution on this. 
- / is rational divide and we can only get x/x
- any * is used to get x^n (using n *) and x/x can be considered x^0
- + is to combine the result for all the x^n
- - is a bit harder. 

A number can be composed sum(x^i) with 0 or 1 as the coefficient, which is the binary representation of an integer. 
for x-ary base, the coefficient could be from 0 to x-1.

It's obvious that the expression should be
C0*x^0 + C1*x^1 + ... + Cn*x^n
And there is only one way to get x^0 by x / x, and it's the only use for divide operation.
Then regard target as a x base number(for example, the 3 base form of 19 is 201)

So:
Consider about generating parts[i]*x^i, you can get it by two ways (you will never generate parts[i] * x^i by some expression other than x^i):
x^i + x^i + ... + x^i (parts[i] times)
x^(i+1) - x^i - x^i - ... - x^i (x - parts[i] times).

Thinking the problem using x-base problem is the key!

As a concrete example, say x = 5, target = 123. We either add 2 or subtract 3. This leaves us with a target of 120 or 125. If the target is 120, we can either add 5 or subtract 20, leaving us with a target of 100 or 125. If the target is 100, we can either add 25 or subtract 100, leaving us with a target of 125 or 0. If the target is 125, we subtract 125.

dp approach: dp[i, target]: i is the exponential, target is the target to solve.

we first calculate all the digits, add a 0 in the end since we may carry a flag (using subtract)
AiX^i: it needs Ai*i operators if using positive way
X^(i+1)-(X-Ai)X^i: if we process as negative,
If previous is pos, then it is i*Ai+pos[i-1]
if previous is neg, then it is i*Ai+neg[i-1]+i (there is a carry over)

If we use negative for current:
- we send a carry over to i+1
- current operators (x-Ai)*i
- if previous is positive, there is no carry over, (x-Ai)*i+pos[i-1]
- if previous is negative, there is a carry over, (x-Ai-1)*i+neg[i-1]

so the solution could be as below:
```cpp
    int leastOpsExpressTarget(int x, int target) {
        //dp: x-ary number to use least operations
        //n=sum(Ai*X^i) to use less operator need to compare x^(i+1) and x^i
        vector<int> digits;
        while(target) {digits.push_back(target%x);target/=x;}
        
        int n=digits.size();
        digits.push_back(0);//could add a carrier flag before the MSB
        vector<int> pos(n+1),neg(n+1);
        pos[0]=digits[0]*2;
        neg[0]=(x-digits[0])*2; //x^0=x/x use more operations
        for(int i=1;i<=n;i++)
        {
            //if use neg, we have a carrier flag to i
            //Aix^i: add Ai times, each with i multplication
            pos[i]=digits[i]*i+min(pos[i-1],neg[i-1]+i); 
            //AiX^i=x^(i+1)-(x-Ai)x^i
            neg[i]=min((x-digits[i])*i+pos[i-1],(x-digits[i]-1)*i+neg[i-1]);
        }
        return min(pos[n],neg[n])-1;
    }
```

Note:
- the boundary needs *2 since it can only be obtained x/x
- final result needs -1

