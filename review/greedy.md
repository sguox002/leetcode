# Chapter 4. Greedy

Greedy:
choose a first move and proof it is a safe move which is consistent to a global optimal solution. Then the problem is reduced to a smaller sized subproblem.
Generally sort will improve the efficiency.

example: 
- n digits to form a largest number, each time choose the largest number from remaining digits
- min refueling stations: refill at the farthest reachable station
- min group of children with age difference <=1, sort first and use segment to cover the leftmost.
- fractional knapsack: sort with the value/weight, and choose as much as possible the current highest value.

If your first move is not safe, the solution is incorrect.

### 1029. Two city scheduling  ***
this is a math problem
A[i]+B[j]<A[j]+B[i]->A[i]-A[j]<B[i]-B[j]
sorted using the difference


### 944. Delete Columns to Make Sorted
just count the unsorted column. To improve efficiency, we just compare neigboring char, O(NM)
```cpp
    int minDeletionSize(vector<string>& A) {
        //using dp: adding one char, if the column is sorted, it is the length of dp[i-1]
        int m=A.size(),n=A[0].size(); //m is row, n is col
        int ans=0;
        for(int i=0;i<n;i++)
        {
            for(int j=1;j<m;j++) 
                if(A[j][i]<A[j-1][i]) {ans++;break;}
        }
        return ans;
    }
```

### 955. Delete Columns to Make Sorted II
make rows are sorted. Once the column is removed, replace it with same char *, and continue to compare.
do not misunderstand: it needs the strings (row represents a word) to be sorted.
```cpp
    int minDeletionSize(vector<string>& A) {
        //previous col is sorted then is over
        int ans=0;
        int m=A.size(),n=A[0].size();
        for(int j=0;j<n;j++)
        {
            bool need_next=0;
            int i=1;
            for(;i<m;i++)
            {
                if(A[i].substr(0,j+1)<A[i-1].substr(0,j+1)) {ans++;break;}
                else if(A[i].substr(0,j)==A[i-1].substr(0,j)) need_next=1;
            }
            if(i==m && !need_next) return ans; //sorted
            if(i<m) for(int k=0;k<m;k++) A[k][j]='*';
        }
        return ans;
    }
```	
if for the previous j columns not sorted, then we replace the jth column with *
if there is string ==, then we need check further to see if they are sorted.

### 122. Best Time to Buy and Sell Stock II
as many as transactions.
Greedy: just compare with previous price, if profit >0 then make it.
```cpp
    int maxProfit(vector<int>& prices) {
        int profit=0;
        for(int i=1;i<prices.size();i++)
        {
            profit+=max(prices[i]-prices[i-1],0);
        }
        return profit;
    }
```
A transaction is defined as buying at Prices[X] and selling at Prices[Y],

the profit of the transaction
= Prices[Y] - Prices[X] 
= Prices[Y] - Prices[Y-1] +
   Prices[Y-1] - Prices[Y-2] ...
    ....
   Prices[X+1] - Prices[X] 
= D[Y] + D[Y-1] + ... + D[X+1]
= sum of D from X+1 to Y

### 714. Best Time to Buy and Sell Stock with Transaction Fee ***
see dp solution
dp[i]=max(dp[i-1],dp[j]+price[i]-price[j]-fee) j=0 to i-1
dp[i]=max(dp[i-1],price[i]+max(dp[j]-price[j]-fee)) j=0 to i-1 and this can be updated iteratively and reducing the complexity from O(N^2) to O(N)
```cpp
    int maxProfit(vector<int>& prices, int fee) {
        int n=prices.size();
        vector<int> dp(n);//dp is the max profit ending at ith day
        int tmax=INT_MIN;
        for(int i=1;i<n;i++)
        {
            tmax=max(tmax,dp[i-1]-prices[i-1]-fee);
            dp[i]=max(dp[i-1],prices[i]+tmax);
        }
        return dp[n-1];
    }
```    
greedy choice:
when current price>previous price+fee, perform a transaction, is this a safe move?</br>
a<b<c<d, and b>a+fee, d>c+fee, then b-a-fee+d-c-fee ==? d-a-fee not really. so this approach is incorrect.</br>
for example fee is 1, 4,6,8,10, we get 3 using first approach, 5 for 2nd approach.</br>
we buy at 4, sell at 6 (equiv sell at 5), buy at 6 sell at 8 (equiv sell at 7). In this case we cannot start a new transaction.</br>
for example 4,6,3,8. buy at 4 sell at 6, profit is 1, buy at 3 sell at 8, profit is 4, total profit is 5. </br>
for example 4,6,5,8. buy at 4 sell at 6, profit is 1, buy at 5 sell at 8, profit is 2, total profit is 3. </br>
so what is the safe move?</br>
we shall tolerate some variation, only when price[i]<price[i-1]-fee we can sell at i-1 and buy at i. 
(i.e the dip shall be deep enough to cover the transaction fee, otherwise it is not worthful). 
Once we start a transaction, we reduce the problem into a smaller subproblem with the new minimum, 
which is a typical greedy approach.</br>

```cpp
    int maxProfit(vector<int>& prices, int fee) {
        //greedy solution
        //only when price[i]<price[i-1]-fee we can start buy at ith day
        int n=prices.size();
        if(n<2) return 0;
        int minimum=prices[0];
        int maxprof=0;
        for(int i=1;i<n;i++)
        {
            if(prices[i]<minimum) //prices[i-1]-fee
                minimum=prices[i];
            else if(prices[i]>minimum+fee)
            {
                maxprof+=prices[i]-minimum-fee;
                minimum=prices[i]-fee;
            }
        }
        return maxprof;
    }
```
The greedy is not so understandable, and most of greedy can be solved using dp approach.


### 860. Lemonade Change
greedy: always gives changes of the large bill first. 

saving the change for 5, 10, and 20 separately.
```cpp
    bool lemonadeChange(vector<int>& bills) {
        int num5=0,num10=0;
        for(int i=0;i<bills.size();i++)
        {
            if(bills[i]==5) num5++;
            else if(bills[i]==10)
            {
                if(!num5) return 0;
                num10++,num5--;
            }
            else
            {
                if(num10 && num5) 
                {
                    num10--,num5--;
                }
                else if(num5>2) num5-=3;
                else return 0;
            }
        }
        return 1;
    }
```

### 1005. Maximize Sum Of Array After K Negations **
convert as many as negatives into positives
if still have k left, we need compare the largest negative and smallest positive
```cpp
    int largestSumAfterKNegations(vector<int>& A, int K) {
        sort(A.begin(),A.end());
        int i=0;
        while(K)
        {
            if(A[i]<0) {A[i]*=-1;i++,K--;}
            else break;
        }
        if(K%2)
        {    
            if(i==0 || A[i]<A[i-1]) A[i]*=-1;
            else A[i-1]*=-1;
        }
        return accumulate(A.begin(),A.end(),0);
    }
```	
note i==0 to avoid i-1 overflow
	
### 455. Assign Cookies
given a list of children greedy size, and a list of candy size, return the number of satisifcation (each child one candy)
greedy choice: bucket sort or using map to sort both the greedy and the size. Greedy choice: assign the candy to the child who first meet the requirement.
```cpp
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        //use two pointers
        int j=0;
        for(int i=0;i<s.size() && j<g.size();i++)
            if(g[j]<=s[i]) {j++;}
        return j;
    }
```
using map for bucket sorting is slower and more complicated, two pointer is more efficient.

### 874. Walking Robot Simulation **
starting from (0,0), -2: turn left, -1: turn right, x (1,9) forward nsteps. with obstacle, then stop before the obstacle.
get the largest distance from the origin.

each step we can calculate its position and then we get the largest distance

we have 4 directions: -x, +x, -y, +y which can be represented as (-1,0) (1,0) (0,-1) (0,1)
turn right: rotate -90 degrees: 
turn left: rotate 90 degrees: 
we can use rotation matrix for the direction changes.
how to easily judge if the obstacle is on the way?
can use a string to include the x and y coordinate and move step by step and stop when we meet an obstacle.
```cpp
    int robotSim(vector<int>& commands, vector<vector<int>>& obstacles) {
        unordered_set<string> obs;
        for(auto o:obstacles) obs.insert(to_string(o[0])+","+to_string(o[1]));
        int x=0,y=0,dx=0,dy=1;
        int maxdist=0;
        for(int i: commands)
        {
            switch(i)
            {
                case -2: swap(dx,dy);dx*=-1;break;
                case -1: swap(dx,dy);dy*=-1;break;
                default:
                    for(int j=0;j<i;j++)
                    {
                        int xx=x+dx,yy=y+dy;
                        if(obs.count(to_string(xx)+","+to_string(yy))) break;
                        x=xx,y=yy;
                        maxdist=max(maxdist,x*x+y*y);
                    }
            }
        }
        return maxdist;
    }
```

### 763. Partition Labels ***
split the string so that each char only appear in one part.
straightforward:
record each char's start and ending position and then merge the segments if they overlap.

efficient one:

don't use hashmap but use array for each char and record its ending index. Merging the segments only matters the ending.
**when we see a char we update the max index when the index == max index, we reach a segment.
Note: this is a very useful interval merging method. **

so use two passes:
```cpp
    vector<int> partitionLabels(string S) {
        vector<int> charIdx(26, 0);
        for(int i = 0; i < S.size(); i++){
            charIdx[S[i]-'a'] = i;
        }
        
        vector<int> results;
        
        int maxIdx = -1, lastIdx = 0;
        for(int i = 0; i < S.size(); i++){
            maxIdx = max(maxIdx, charIdx[S[i]-'a']);
            if(i == maxIdx) {
                results.push_back(maxIdx - lastIdx + 1);
                lastIdx = i+1;
            }
        }
        return results;
    }
```

### 921. Minimum Add to Make Parentheses Valid ***
cannot just count the ( and ) for example )(. The ( shall have a ) followed. The ) shall have a preceding (. We use a stack for this, removing all those paired.
```cpp
    int minAddToMakeValid(string S) {
        //to make it balanced, the () shall be the same amount
        //one we have a ) we need have a balanced in its front
        //once we have a ( we need have a balanced in the behind
        stack<char> st;
        int ans=0;
        for(int i=0;i<S.size();i++)
        {
            if(S[i]=='(') st.push(S[i]);
            else 
            {
                if(st.size()) st.pop();
                else ans++; 
            }
        }
        return ans+st.size();
    }
```

using two pointers for the two conditions will save the extra storage using stack
more efficient method: any time we shall keep the count to be>=0

```cpp
    int minAddToMakeValid(string S) {
        //anytime the left >=right and final must be 0
        int ans=0;
        int cnt=0;
        for(char c: S)
        {
            if(c=='(') cnt++;
            else cnt--;
            if(cnt<0) ans++,cnt++;
        }
        ans+=cnt;
        return ans;
    }
```

### 861. Score After Flipping Matrix ***
a move choose any row or column and toggle its value.
Each row represents a binary number, return the largest sum of all these numbers.
greedy choice: 
the MSB shall all toggles to 1, by toggling the row
the non-MSB shall have more 1s than 0s. toggling columns
the observation is critical

implementation can have a straightforward one
but we can just add all column's bit and it is the same but more concise.

```cpp
    int matrixScore(vector<vector<int>> A) {
        int M = A.size(), N = A[0].size(), res = (1 << (N - 1)) * M;
        for (int j = 1; j < N; j++) {
            int cur = 0;
            for (int i = 0; i < M; i++) cur += A[i][j] == A[i][0];
            res += max(cur, M - cur) * (1 << (N - j - 1));
        }
        return res;
    }
```
	
### 406. Queue Reconstruction by Height ***
each person has a height and number of person taller or equal to him in front of him.
reconstruct the queue
sort the people in descend order but the rank in descending order, so the same height people is in order. Then insert each lower person.
This is a greedy choice.
for example: [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
sort as: [7,0], [7,1], [6,1],[5,0],[5,2],[4,4]
each step:
[7,0]
[7,0], [7,1]
[7,0],[6,1],[7,1]
[5,0],[7,0],[6,1],[7,1]
[5,0],[7,0],[5,2],[6,1],[7,1]
[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]
sorting is the key to make sure all member inside the array are taller than current one.
```cpp
bool cmp(vector<int> a, vector<int> b) {return a[0]>b[0] || (a[0]==b[0] && a[1]<b[1]);}
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(),people.end(),cmp);
        vector<vector<int>> ans;
        for(auto t: people)
        {
            ans.insert(ans.begin()+t[1],t);
        }
        return ans;
    }
};
```


### 1007. Minimum Domino Rotations For Equal Row
swap a vs b to make a has the same number
we only have two choice: 
a=a[0] or a=b[0]
we need check one by one to see if we have a[0] or b[0] in each column
```cpp
    int minDominoRotations(vector<int>& A, vector<int>& B) {
      //we need count 1 to 6 index in each row
        vector<vector<int>> inda(6,vector<int>()),indb(6,vector<int>());
        int n=A.size();
        for(int i=0;i<A.size();i++)
        {
            inda[A[i]-1].push_back(i);
            indb[B[i]-1].push_back(i);
        }
        int minstep=INT_MAX;
        for(int i=0;i<6;i++)
        {
            unordered_set<int> ms(inda[i].begin(),inda[i].end());;
            for(int j=0;j<indb[i].size();j++) ms.insert(indb[i][j]);
            //cout<<i<<" "<<ms.size()<<endl;
            if(ms.size()==n) minstep=min(minstep,n-max((int)inda[i].size(),(int)indb[i].size()));
        }
        return minstep==INT_MAX?-1:minstep;
    }
```	
Solution 2
Try A[0]
Try B[0]
return -1
One observation is that, if A[0] works, no need to check B[0].
Because if both A[0] and B[0] exist in all dominoes,
the result will be the same.

```cpp
    int minDominoRotations(vector<int>& A, vector<int>& B) {
        int n = A.size();
        for (int i = 0, a = 0, b = 0; i < n && (A[i] == A[0] || B[i] == A[0]); ++i) {
            if (A[i] != A[0]) a++;
            if (B[i] != A[0]) b++;
            if (i == n - 1) return min(a, b);
        }
        for (int i = 0, a = 0, b = 0; i < n && (A[i] == B[0] || B[i] == B[0]); ++i) {
            if (A[i] != B[0]) a++;
            if (B[i] != B[0]) b++;
            if (i == n - 1) return min(a, b);
        }
        return -1;
    }
```
	


### 392. Is Subsequence
check if s is subsequence of t.</br>
dp approach: by deleting chars from t to see if we can ontain s</br>
greedy: choose the first matching char in t using two pointers</br>
```cpp
    bool isSubsequence(string s, string t) {
        //use two pointer
        int i=0,j=0;
        while(i<s.length() && j<t.length())
        {
            while(t[j]!=s[i])
            {
                j++;
                if(j==t.length()) return 0;
            }
            i++,j++;
        }
        return i==s.length() && j<=t.length();
    }
```

### 452. Minimum Number of Arrows to Burst Balloons
provided the start coordinate and end coordinate of each balloon along x-axis. Arrows are shot up and burst all the balloons along its path. Return the min number of arrows to burst all the balloons.</br>
this is to find the overlaps.</br>
Approach: sort the segments according to its end. If current segment > current end, then we need start a new overlap</br>
what position should we pick each time? We should shoot as to the right as possible, because since balloons are sorted, this gives you the best chance to take down more balloons. </br>
ie. greedy choice is to choose the left most segment ending point and shoot all those overlapping balloons.</br>
this problem is similar to those overlapping interval problems such as double booking, triple booking et al.</br>

```cpp
    int findMinArrowShots(vector<pair<int, int>>& points) {
        if(points.size()<2) return points.size();
        sort(points.begin(),points.end(),cmp);
        int ans=0;
        int cur_end=points[0].second;ans++;
        for(int i=1;i<points.size();i++)
        {
            if(points[i].first>cur_end) {cur_end=points[i].second;ans++;}
        }
        return ans;
    }
```

### 881. Boats to Save People
given a list of person's weight, each boat has a weight limit and 2 person limit. And it is guaranteed that the boat can carry one person.
return the min number of boats required.
greedy: sort the weight and use two pointers. If the higher weight + lower weight exceeds, only moves the higher end. The lower weight needs to be waited to be together with other person.
```cpp
    int numRescueBoats(vector<int>& people, int limit) {
        sort(people.begin(),people.end());
        int i=0,j=people.size()-1;
        int total=0;
        while(i<=j)
        {
            if(people[i]+people[j]<=limit) {i++;}
            j--;
            total++;
        }
        return total;
    }
```

### 435. Non-overlapping Intervals
given a collection of overlapping intervals, return the min number of intervals to remove to make them non-overlapping.
Actually, the problem is the same as "Given a collection of intervals, find the maximum number of intervals that are non-overlapping." (the classic Greedy problem: Interval Scheduling). With the solution to that problem, guess how do we get the minimum number of intervals to remove? : )

Sorting Interval.end in ascending order is O(nlogn), then traverse intervals array to get the maximum number of non-overlapping intervals is O(n). Total is O(nlogn).

```cpp
    int eraseOverlapIntervals(vector<Interval>& intervals) {
        if(intervals.size()==0) return 0;
        sort(intervals.begin(),intervals.end(),cmp);
        int ans=0;
        int cur_end=intervals[0].end;ans++;
        for(int i=0;i<intervals.size();i++)
        {
            if(intervals[i].start>=cur_end) {cur_end=intervals[i].end;ans++;}
        }
        return intervals.size()-ans;
    }
```

### 738. Monotone Increasing Digits
Given a non-negative integer N, find the largest number that is less than or equal to N with monotone increasing digits.
(digits are sorted)
greedy: reversely find the inversion. if found an inversion we reduce the previous by 1, thus to propagate the changes forward.

example: 3421->3411->3311->3399
example: 3431->3421->3321->3399
example: 144267->144267->144267->143267->133267->13999
example: 1100->1100->1000->0000->999
```cpp
    int monotoneIncreasingDigits(int N) {
        string n_str = to_string(N);
        int marker = n_str.size();
        for(int i = n_str.size()-1; i > 0; i --) {
            if(n_str[i] < n_str[i-1]) {
                marker = i;
                n_str[i-1]--;// = n_str[i-1]-1;
            }
        }
        for(int i = marker; i < n_str.size(); i ++) n_str[i] = '9';
        return stoi(n_str);
    }
```

### 870. Advantage Shuffle
Given two arrays A and B of equal size, the advantage of A with respect to B is the number of indices i for which A[i] > B[i].

Return any permutation of A that maximizes its advantage with respect to B.
Greedy solution: if there is an element in A >B[i], then choose the smallest one. 
运用田忌赛马的思路，如果A中有相应的higher元素，使用最小的higher元素。否则使用A中最小的元素 :)
using a multset
```cpp
    vector<int> advantageCount(vector<int>& A, vector<int>& B) {
        multiset<int> ms(A.begin(),A.end());
        vector<int> ans;
        //if we cannot beat we choose the min available for it
        //if we can beat we choose the min > B[i]
        for(int t: B)
        {
            auto it=upper_bound(ms.begin(),ms.end(),t);
            if(it==ms.end()) {
                ans.push_back(*ms.begin());
                ms.erase(ms.begin());
            }
            else
            {
                ans.push_back(*it);
                ms.erase(it);
            }
        }
        return ans;
    }
```
but got TLE
with some optimization (do not check upperbound every time
```cpp
	vector<int> advantageCount(vector<int>& A, vector<int>& B) {
	  multiset<int> s(begin(A), end(A));
	  for (auto i = 0; i < B.size(); ++i) {
		auto p = *s.rbegin() <= B[i] ? s.begin() : s.upper_bound(B[i]);
		A[i] = *p;
		s.erase(p);
	  }
	  return A;
	}
```	

### 767. Reorganize String
neighboring characters are not the same.
if the highest freq char appears more than half of the length, we cannot fulfill the requirement.
Otherwise we divide into k chunks by appending other chars into the chunk.
c++ using priority_queue by removing the highest freq one and followed by the 2nd highest priority one. -1 and put back. A good example to use PQ for this.

```cpp
    string reorganizeString(string S) {
        vector<int> mp(26);
        for(char c: S) {
            mp[c-'a']++;
            if(mp[c-'a']>(S.size()+1)/2) return "";
        }
        priority_queue<pair<int,char>> pq;
        for(int i=0;i<26;i++) if(mp[i]) pq.push({mp[i],i+'a'});
        string ans;
        while(pq.size())
        {
            auto t=pq.top();
            pq.pop();
            ans+=t.second;
            if(pq.size())
            {
                auto t1=pq.top();
                pq.pop();
                ans+=t1.second;
                if(--t1.first) pq.push(t1);
            }
            if(--t.first) pq.push(t);
        }
        return ans;
    }
```    

### 621. Task Scheduler
same task needs at least n intervals (adding idle). </br>
return the number of intervals to complete all tasks </br>
greedy approach: try to take one from each set of task and add zero or more idles for this to satisfy n intervals. Always take one task from current max task. 
Assuming the highest frequency is K, then we divide it into k chunks. each chunk will fill one task from each set. (k-1)*(n+1)

First count the number of occurrences of each element.
Let the max frequency seen be M for element E
We can schedule the first M-1 occurrences of E, each E will be followed by at least N CPU cycles in between successive schedules of E
Total CPU cycles after scheduling M-1 occurrences of E = (M-1) * (N + 1) // 1 comes for the CPU cycle for E itself
Now schedule the final round of tasks. We will need at least 1 CPU cycle of the last occurrence of E. If there are multiple tasks with frequency M, they will all need 1 more cycle.
Run through the frequency dictionary and for every element which has frequency == M, add 1 cycle to result.
If we have more number of tasks than the max slots we need as computed above we will return the length of the tasks array as we need at least those many CPU cycles.

For the last point I think you should explain more clearly.

Support we have AAABBBCCDDEF n = 2

No matter how we arrange, we at least have the following schedule:

A cool cool A cool cool A
In this way, we could replace the first 'cool' with B. then it becomes.

A B cool A B cool A B

With the remain C, we could
A B C A B C A B
for the def,
A B C (D) E A B C (D) F
we insert at the position of end of each period of A B C. then we could ensure that there would be no collision

```cpp
    int leastInterval(vector<char>& tasks, int n) {
        vector<int> mp(26);
        int mostfreq=0;
        for(char c: tasks) {mp[c-'A']++;mostfreq=max(mostfreq,mp[c-'A']);}
        int ans=(mostfreq-1)*(n+1);
        for(int i=0;i<26;i++) if(mp[i]==mostfreq) ans++;
        return max((int)tasks.size(),ans);
    }
```

### 659. Split Array into Consecutive Subsequences

You are given an integer array sorted in ascending order (may contain duplicates), you need to split them into several subsequences, where each subsequences consist of at least 3 consecutive integers. Return whether you can make such a split.

for each card, either it can be appended to existent, or start a new group or it is a single

```cpp
    bool isPossible(vector<int>& nums) {
        unordered_map<int,int> cnt,need;
        for(int i: nums) cnt[i]++;
        for(int i: nums)
        {
            if(cnt[i]==0) continue;
            if(need[i]) //we are in need of i
            {
                need[i]--;
                need[i+1]++;
            }
            else if(cnt[i+1] && cnt[i+2]) //start a new group
            {
                cnt[i+1]--,cnt[i+2]--;
                need[i+3]++;
            }
            else return 0; //neither
            cnt[i]--;
        }
        return 1;
    }
```
uses two hashmap: 
freq is the counter for each number.
need is the counter for needed number for current consecuative subsequence


### 948. Bag of Tokens
given a list of tokens with power[i]. We put the token down, losing the power and gain 1 point. We put the token up, losing one point and gain power[i].
What the max point we can get with initial power P and 0 points.
Greedy choice: losing min power to get 1 point and lost 1 point to get the max power.
sort the power and uses two pointers
```cpp
    int bagOfTokensScore(vector<int>& tokens, int P) {
        //greedy problem: lose min power for point, lose point for max power
        if(tokens.size()==0) return 0;
        if(tokens.size()==1) return P>tokens[0];
        sort(tokens.begin(),tokens.end());
        int n=tokens.size();
        int i=0,j=n-1;
        if(P<tokens[0]) return 0;
        int maxpoint=0;
        int points=0,power=P;
        while(i<j)
        {
            while(i<=j && power>=tokens[i]) 
            {
                points++;power-=tokens[i];
                maxpoint=max(maxpoint,points);i++;
            }
            if(i<j && points) {points--;power+=tokens[j];j--;}
        }
        return maxpoint;
    }
```

### 649. Dota2 Senate
two parties. according order, each senate can ban a senator from other party, predict who is the winner.
greedy: ban its next member from other party since otherwise it will ban a member from your party.
using two queues: if the member exercises its right, move it to the end of the queue. banned member are removed from queue

```cpp
	string predictPartyVictory(string senate) {
        queue<int> q1, q2;
        int n = senate.length();
        for(int i = 0; i<n; i++)
            (senate[i] == 'R')?q1.push(i):q2.push(i);
        while(!q1.empty() && !q2.empty()){
            int r_index = q1.front(), d_index = q2.front();
            q1.pop(), q2.pop();
            (r_index < d_index)?q1.push(r_index + n):q2.push(d_index + n);
        }
        return (q1.size() > q2.size())? "Radiant" : "Dire";
    }
```
note: why push_back with +n? (make sure its index is greater than n and it can live to other rounds)
this is the key step.

### 376. Wiggle Subsequence
greedy or dp: the first number could be smaller or larger number.
use dp[n][2] or down[n]/.up[n] to record the wiggle subsequence length
```java
    public int wiggleMaxLength(int[] nums) {
        
        if( nums.length == 0 ) return 0;
        
        int[] up = new int[nums.length];
        int[] down = new int[nums.length];
        
        up[0] = 1;
        down[0] = 1;
        
        for(int i = 1 ; i < nums.length; i++){
            if( nums[i] > nums[i-1] ){
                up[i] = down[i-1]+1;
                down[i] = down[i-1];
            }else if( nums[i] < nums[i-1]){
                down[i] = up[i-1]+1;
                up[i] = up[i-1];
            }else{
                down[i] = down[i-1];
                up[i] = up[i-1];
            }
        }
        
        return Math.max(down[nums.length-1],up[nums.length-1]);
    }
```

reducing to O(1)

```cpp
    int wiggleMaxLength(vector<int>& nums) {
        if(nums.size()==0) return 0;
        int up=1,dn=1;
        for(int i=1;i<nums.size();i++)
        {
            if(nums[i]==nums[i-1]) continue;
            if(nums[i]>nums[i-1]) up=dn+1;
            else dn=up+1;
        }
        return max(up,dn);
    }
```

### 991. Broken Calculator
On a broken calculator that has a number showing on its display, we can perform two operations:

Double: Multiply the number on the display by 2, or;
Decrement: Subtract 1 from the number on the display.
Initially, the calculator is displaying the number X.

Return the minimum number of operations needed to display the number Y.
seen in math
from Y to x is easier.
Y<X we can only use -1.
```cpp
    int brokenCalc(int X, int Y) {
        //every step we can choose -1 or *2
        //greedy solution: 
        //if Y is even, divide by 2, if Y is odd add 1
        int res = 0;
        while (Y > X) {
            Y = Y % 2 > 0 ? Y + 1 : Y / 2;
            res++;
        }
        return res + X - Y;        
    }
```

### 842. Split Array into Fibonacci Sequence
given a string of digits, return the Fib series if not able to return empty string
each integer fits in a 32 bit

The first two number choice determines the whole string. 
first number can choose up to 10 digits
second number can choose up to 10 digits
and this is a somewhat greedy choice.

backtracking algorithm
```cpp
    vector<int> splitIntoFibonacci(string S) {
        vector<int> nums;
        backtrack(S, 0, nums);
        return nums;
    }
    bool backtrack(string &S, int start, vector<int> &nums){        
        int n = S.size();
        if(start >= n && nums.size()>=3) return 1;
        int maxSplitSize = (S[start]=='0') ? 1 : 10;
        for(int i=1; i<=maxSplitSize && start+i<=S.size(); i++)
        {
            long long num = stoll(S.substr(start, i));
            if(num > INT_MAX) return 0;
            int sz = nums.size();
            if(sz >= 2 && nums[sz-1]+nums[sz-2] < num) return 0;
            if(sz<=1 || nums[sz-1]+nums[sz-2]==num)
            {
                nums.push_back(num);
                if(backtrack(S, start+i, nums)) return 1;
                nums.pop_back();                
            }
        }
        return 0;
    }
```
- num>INT_MAX, return 0, since adding more chars will be meaningless
- sz>=2 && nums[sz-1]+nums[sz-2]<num why? same reason: if the num is too large, we don't continue since it is wasting time.

### 134. Gas Station
a circular circuit with each station gas[i] and cost[i], you are starting at 0. cost[i] is the cost from i to i+1 clockwise
-. flatten the circle by copying the array
-. net gas[i]-cost[i]
-. accumulate a N-window sum. Cannot have any station with accumulate sum to be <0

first we need prove:
if the total net gas>=0 there is a solution
sum(gas[i]-cost[i])>=0 for i=0 to n-1
sum from 0 to j is min, then j+1 is the start station (math proof is simple)

```cpp
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n = gas.size();
        int total(0), subsum(INT_MAX), start(0);
        for(int i = 0; i < n; ++i){
            total += gas[i] - cost[i];
            if(total < subsum) {
                subsum = total;
                start = i + 1;
            }
        }
        return (total < 0) ?  -1 : (start%n); 
    }
```	
note start=i+1 and start%n

### 55. Jump Game
given a list of max jump steps at each position, check if we can reach the end

greedy vs dp:
dp is a bottom down, greedy is top down. greedy choose local optimal solution and reach global
dp makes optimal solution based on previous smaller subproblem optimal solution.

Looking from the end and at each point ahead checking the best possible way to reach the end
```cpp
    bool canJump(vector<int>& nums) {
        int n = nums.size();
        vector<bool> jump(n,false);
        jump[n-1]=true;
        
        for(int i=n-2;i>=0;i--)
        {
            for(int j=0;j<=nums[i] && i+j<n;j++)
            {
                if(jump[i+j]==true) 
                {
                    jump[i]=true; 
                    break;
                }
            }
        }
        
        return jump[0];
    }
```
greedy:
```cpp
    bool canJump(vector<int>& nums) {
      int n = nums.size(), farest = 0;
      for(int i = 0;i < n; i++)
      {
        if(farest < i) return false;
        farest = max(i + nums[i], farest);
      }
      
      return true;
    }
or
    bool canJump(vector<int>& nums) {
        //to jump from ith position, you first need to be able to jump here
        int reach=0;
        for(int i=0;i<nums.size() && i<=reach;i++)
            reach=max(reach,i+nums[i]);
        return reach>=nums.size()-1;
    }

```

### 45. Jump Game II.md
### Problem Summary
Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

### Analysis
- starting at the first step i=0, max range is num[0]. Which covers the node from 1 to num[0]
- we shall check the next max range using all those nodes
- the first one > n-1 is the min level
does it sound like a bfs? yes

We can also use dp for a O(N^2) approach.

```cpp
    int jump(vector<int>& nums) {
        if(nums.size()<2) return 0;
        int cur_max=0,next_max=0;
        int i=0,ans=0;
        while(cur_max>=i)
        {
            ans++;
            for(;i<=cur_max;i++)
                next_max=max(next_max,nums[i]+i);
            cur_max=next_max;
            if(cur_max>=nums.size()-1) return ans;
        }
        return 0;
    }
 ```
 note: n==1 is a corner case, need 0 steps.
 
 
### 402. Remove K Digits
to make the number the smallest.
for example 1432219, k=3
greedy choice: remove the first peak digit from left to right (left is most important for numbers)
for example 1329 remove 3 becomes 129, remove 9 becomes 132
for example 1239 remove 9 to become 123 (the largest)

we can use a stack to find the peak digit to make it O(n)
note: the accepted submission cannot process string with no peaks correctly. (if there is no peak, it will not delete any digits)

```cpp
	string removeKdigits(string num, int k) {
        string res;
        int keep = num.size() - k;
        for (int i=0; i<num.size(); i++) {
            while (res.size()>0 && res.back()>num[i] && k>0) {
                res.pop_back();
                k--;
            }
            res.push_back(num[i]);
        }
        res.erase(keep, string::npos);//otherwise removing the rightmost digits
        
        // trim leading zeros
        int s = 0;
        while (s<(int)res.size()-1 && res[s]=='0')  s++;
        res.erase(0, s);
        
        return res=="" ? "0" : res;
    }
```
string acts as a stack. using a stack to find a pattern is regular practice see the stack.
leading zero may occur due to digits removed and 0 is left. for example 10200
first erase peaks from left to right
then remove the right remaining
note: while is correct. if we change while to if, this will not be correct since the peak is missed.


### 910. Smallest Range II
given an array, each element can be add K or minus K, return the smallest difference between the max and min
greedy: sort the array, the left side always +k, and the right side always -k. we iterate each element as the left/right and get the min.
the left one could become max, and the right could become min

or it is equivalent to add 0 to right or 2K to left. 
sort is the key step.
```cpp
    int smallestRangeII(vector<int> A, int K) {
        sort(A.begin(), A.end());
        int n = A.size(), mx = A[n - 1], mn = A[0], res = mx - mn;
        for (int i = 0; i < n - 1; ++i) {
            mx = max(mx, A[i] + 2 * K);
            mn = min(A[i + 1], A[0] + 2 * K);//add 0 or 2*K
            res = min(res, mx - mn);
        }
        return res;
    }
```	
1. add all 0 to each element, mx-mn
2. the leftmost add 2*K and rightmost add 0


### 135. Candy.md
### Problem Summary
There are N children standing in a line. Each child is assigned a rating value.

You are giving candies to these children subjected to the following requirements:

Each child must have at least one candy.
Children with a higher rating get more candies than their neighbors.
What is the minimum candies you must give?

### Analysis
example [1,0,2] you need given 2,1,2 candies to each.
- each one at least have one candy, initialize to 1
- left to right scan, if rating > previous one, we need add one candy
- right to left scan, if rating > previous one (behind it), we need check if we need add one.

thus we guarantee the left and right side. this is more likely a dp solution

```cpp
    int candy(vector<int>& ratings) {
        int ans=0;
        int n=ratings.size();
        vector<int> can(n,1);
        //from left to right
        for(int i=1;i<n;i++) if(ratings[i]>ratings[i-1]) can[i]=can[i-1]+1;
        for(int i=n-2;i>=0;i--) if(ratings[i]>ratings[i+1]) can[i]=max(can[i],can[i+1]+1);
       
        return accumulate(can.begin(),can.end(),0);
    }
```    
### 316. Remove Duplicate Letters.md
### Problem Summary
remove duplicate letters from string so that every char occurs only once. return the smallest string remaining

### Analysis

naive approach:
We can safely store the information in a map of char vs its index list.
find the first smallest char who has larger chars followed, and this char is used and removed from the map.
reduce the problem to smaller size.
once the char is chosen, all previous index shall be removed.
```cpp
    string removeDuplicateLetters(string s) {
        //create a map for each character
        map<char,vector<int>> freq;
        for(int i=0;i<s.size();i++) freq[s[i]].push_back(i);
//greedy choice: the leftmost char shall have an index smaller than all other larger char's max index, 
//this letter shall be as small as possible
        string ans;
        auto it=freq.begin();
        bool found=0;
        int nchar=freq.size();
        while(ans.size()<nchar)
        {
            found=1;
            for(auto it1=freq.begin();it1!=freq.end();it1++)
            {
                if(it1==it) continue;
                if(it->second[0]>it1->second.back()) {it++;found=0;break;}
            }
            if(found) 
            {
                int ind=it->second[0];
                ans+=it->first;
                freq.erase(it);
                for(auto it1=freq.begin();it1!=freq.end();it1++) //remove all index smaller than ind
                {
                    auto it2=lower_bound(it1->second.begin(),it1->second.end(),ind);
                    it1->second.erase(it1->second.begin(),it2);
                }
                it=freq.begin();
            }
        }
        return ans;
    }
```    

best algorithm: each time we find the leftmost (smallest) until there is a char missing, then we choose the letter
removing all other instance of this char and form a new string for the subproblem.
each time O(n)
```cpp
    string removeDuplicateLetters(string s) {
        if(s.empty()) return "";
        vector<int> cnt(26);
        for(char c: s) cnt[c-'a']++;
        char leftmost=s[0];
        int ind=0;
        for(int i=0;i<s.size();i++)
        {
            if(s[i]<leftmost) {leftmost=s[i],ind=i;}
            if(--cnt[s[i]-'a']==0) break;
        }
        //erase all instance of leftmost in the right substr
        string ns;
        for(int i=ind+1;i<s.size();i++) if(s[i]!=leftmost) ns+=s[i];
        return leftmost+removeDuplicateLetters(ns);
    }
```

### 321. Create Maximum Number.md
### Problem Summary
Given two arrays of length m and n with digits 0-9 representing two numbers. Create the maximum number of length k <= m + n from digits of the two. The relative order of the digits from the same array must be preserved. Return an array of the k digits.

### Approach
- try all combination i+j=k which take i digits from array 1 and j digits from array 2
- reduces to problem get n digits from one array
- merge sort to get the max number

```cpp
    vector<int> maxNumber(vector<int>& nums1, vector<int>& nums2, int k) {
        int m=nums1.size(),n=nums2.size();
        //cout<<m<<" "<<n<<" "<<k<<endl;
        if(k>=m+n)  //just merge the two arrays
            return merge(nums1,nums2);
        //try all possible combinations, i from array 1, k-i from array 2
        //i range is [max(m-k,0),min(k,m)], other array range is [max(n-k,0),min(k,n)]
        vector<int> ans(k);
        for(int i=0;i<=min(m,k);i++)
        {
            if(k-i>nums2.size()) continue;
            vector<int> a=maxNumber(nums1,i);
            vector<int> b=maxNumber(nums2,k-i);
            vector<int> c=merge(a,b);
            if(lexicographical_compare(ans.begin(),ans.end(),c.begin(),c.end())) ans=c;
        }
        return ans;
    }
    
    vector<int> maxNumber(vector<int>& v,int k) //get k digits from 1 array
    {
        int n=v.size();
        if(k>=n) return v;
        if(k==0) return vector<int>();
        vector<int> ans(k);
        //greedy choice, the leftmost digit is the max from i+[0,n-(k-1))
        int i=0;
        for(int j=1;j<=k;j++) //repeat k times
        {
            auto it=max_element(v.begin()+i,v.begin()+n-(k-j));
            i=int(it-v.begin())+1; //now the new pointer
            ans[j-1]=*it;
        }
        return ans;
    }
    vector<int> merge(vector<int>& v1,vector<int>& v2)
    {
        int i=0,j=0,k=0;
        vector<int> ans(v1.size()+v2.size());
        while(i<v1.size() && j<v2.size())
        {
            if(v1[i]>v2[j]) {ans[k++]=v1[i++];}
            else if(v1[i]<v2[j]) {ans[k++]=v2[j++];}
            else //two number is equal, choose the one behind is larger
            {
                if(lexicographical_compare(v1.begin()+i,v1.end(),v2.begin()+j,v2.end())) //v1<v2
                   ans[k++]=v2[j++];
                else ans[k++]=v1[i++];
            }
        }
        if(i<v1.size()) copy(v1.begin()+i,v1.end(),ans.begin()+k);
        if(j<v2.size()) copy(v2.begin()+j,v2.end(),ans.begin()+k);
        return ans;
    }
```    

### 330. Patching Array.md
### Problem Summary
Given N and a number array, check the min numbers to be added into the array to get the sum from 1 to n
input array is sorted

example:
nums = [1, 2, 4, 13, 43] and n = 100
### 1 covers 1
### 1,2: covers 1,2,3
### 1,2,4: covers 1,2,3,4,5,6,7
missing 8, add 8, it can covers up to 15
### 1,2,4,8,13 covers up to 28 (13+15)
missing 29, add 29
### 1,2,4,8,13,29, covers up to 28+29=57
### 1,2,4,8,13,29,43, covers up to 100

so if previous m element can sum to [1,M]. If M+1 is not covered, we shall add it

```cpp
    int minPatches(vector<int>& nums, int n) {
        //the fastest way to go to n without leaving out any number
        //if the smaller subproblem  solves from 1 to m, then add a number m+1, it solves the problem from 1 to 2m+1
        //the greedy choice is the smaller subproblem's sum+1
         long miss = 1, added = 0, i = 0;
         while (miss <= n) 
         {
            if (i < nums.size() && nums[i] <= miss) 
            {
                miss += nums[i++];
            } 
            else 
            {
                miss += miss;
                added++;
            }
        }
        return added;
    }
```    


### 502. IPO.md
### Problem Summary
Given k projects, each project has a profit Pi and cost Wi, Initial capital is W. 
Pick up at most k projects to maximize the profit, return the max profit.

### Approach
- find the projects which requires cost<=current capital (using sort/binary search or minheap: pq or multiset)
- choose the max profit project first (using priority-queue)

```cpp
bool cmp(pair<int,int> a,pair<int,int> b) {return a.first<b.first;}
class Solution {
public:
    int findMaximizedCapital(int k, int W, vector<int>& Profits, vector<int>& Capital) {
        vector<pair<int,int>> vs;
        for(int i=0;i<Profits.size();i++) 
            if(Profits[i]) vs.push_back(make_pair(Capital[i],Profits[i]));
        priority_queue<int> pq;
        sort(vs.begin(),vs.end(),cmp);
        int ind=0,nproj=0,w=W;
        while(nproj<k)
        {
            auto it=upper_bound(vs.begin()+ind,vs.end(),make_pair(w,0),cmp);
            if(it==vs.begin()) break;
            for(auto it1=vs.begin()+ind;it1!=it;it1++) pq.push(it1->second);
            if(!pq.empty()){w+=pq.top();pq.pop();}
            ind=int(it-vs.begin());
            nproj++;
        }
        return w;
    }
};
```
### 630. Course Schedule III.md
### Problem Summary
Given a list of courses (t,d): t: duration day, d: closing date
return max number of courses taken

### Analysis
greedy: always choose the shorter course
approach: sort the courses according to its closing date. push the course duration time into priority-queue. When the finish time > closing date, we pop out the longest course in the pq.

### code
```cpp
bool cmp(vector<int>& a,vector<int>& b) {return a[1]<b[1] || (a[1]==b[1] && a[0]<b[0]);}
class Solution {
public:
    int scheduleCourse(vector<vector<int>>& courses) {
        sort(courses.begin(),courses.end(),cmp);
        priority_queue<int> pq;
        int n=courses.size();
        int s=0; //s is the current time
        for(int i=0;i<n;i++)
        {
            s+=courses[i][0]; //if we select this course, advance the time
            pq.push(courses[i][0]);
            if(s>courses[i][1]) //out of closing date, try to remove the longest 
            {s-=pq.top();pq.pop();} //
        }
        return pq.size();
    }
};
```
### 757. Set Intersection Size At Least Two.md
### Problem Summary
An integer interval [a, b] (for integers a < b) is a set of all consecutive integers from a to b, including a and b.

Find the minimum size of a set S such that for every integer interval A in intervals, the intersection of S with A has size at least 2.

### Analysis
- we can sort the intervals according to its end
- the segment shall cover the first segment for at least two element to the end, the last segment from the start.
- iterate all segments: 
  (1) If there is no number in this interval being chosen before, we pick up 2 biggest number in this interval. (the biggest number have the most possibility to be used by next interval)
  (2) If there is one number in this interval being chosen before, we pick up the biggest number in this interval.
  (3) If there are already two numbers in this interval being chosen before, we can skip this interval since the requirement has been fulfilled.

```cpp
    int intersectionSizeTwo(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [](vector<int>& a, vector<int>& b) 
        {
            return a[1] < b[1] || (a[1] == b[1] && a[0] > b[0]); 
        });
        int n = intervals.size(), ans = 0, p1 = -1, p2 = -1;
        for (int i = 0; i < n; i++) 
        {
            if (intervals[i][0] <= p1) continue;// current p1, p2 works for intervals[i]
            if (intervals[i][0] > p2) // Neither of p1, p2 works for intervals[i]
            {
                ans += 2;
                p2 = intervals[i][1];// replace p1, p2 by ending numbers
                p1 = p2-1;
            }
            else //only p2 works: start in the range [p1,p2]
            {
                ans++;
                p1 = p2;
                p2 = intervals[i][1];
            }
        }
        return ans;
    }
```

Note: the problem asks for a set not a single interval, for example, [[1,2],[9,10]] it will return [1,2,9,10] the length is 4

### 765. Couples Holding Hands.md
### Problem Summary
N couples labelled as 0,1,....2*N-1, couples shall sit together. Return the min number of swaps

### Approach
union find using cyclic permutation based on group theory can be applied to find the connected groups (the value and the index)
key observations:
- if we divide each number by 2, we get the group id for all couples
- the final state: position 2i and 2i+1 must have the same group id
- assume a group has i and j, i!=j, then we need swap:
  1. keep i, and swap j with the next i (this at least fix i)
  2. keep j, and swap i with the next j (this is equivalent to above)
  3. thus every time we fix the current group (also may fix some group behind)
we get the following greedy solution:

```cpp
    int minSwapsCouples(vector<int>& row) {
        for(int i=0;i<row.size();i++) row[i]/=2; //making couple the same number
        int cnt=0;
        for(int i=0;i<row.size();i+=2)
        {
            if(row[i]!=row[i+1]) 
            {
                auto it=find(row.begin()+i+2,row.end(),row[i]);
                swap(*it,row[i+1]);
                cnt++;
            }
        }
        return cnt;
    }
```

- the complexity is O(N^2) since every time we need to search for the next
- O(N): create a index array to record each number's position and we exactly know each group's partner and we swap that. Then we do not need to use find. Be careful when we swap two nodes, the index array shall also be swapped.
```java
function minSwapsCouples(row) {
  const pos = {};
  for (let i = 0; i < row.length; i++) {
    pos[row[i]] = i;
  }

  let count = 0;
  for (let i = 1; i < row.length; i += 2) {
    while ((row[i]^1) !== row[i-1]) {
      let idx = pos[row[i]^1]^1;
      [row[i], row[idx]] = [row[idx], row[i]];
      count++;
    }
  }
  return count;
}
```

### 927. Three Equal Parts.md
### Problem Summary
Given an array A of 0s and 1s, divide the array into 3 non-empty parts such that all of these parts represent the same binary value.
return the index i and j. If cannot split, return -1,-1

### Analysis
This problem is clear and straightforward.

- leading zero does not contribute to the value
- so we only care about 1. the number of 1 and 1's position
- divide into 3 parts and check if the three parts are the same (with only a const index difference)
- the last part's trailing zero shall also present in the first two parts

### code
```cpp
    vector<int> threeEqualParts(vector<int>& A) {
      vector<int> ones;
        for(int i=0;i<A.size();i++) if(A[i]) ones.push_back(i);
        if(ones.size()%3) return vector<int>({-1,-1});
        if(ones.size()==0) return vector<int>({1,A.size()-1});
        int len=ones.size()/3;
        //check if the three parts has the same pattern
        for(int i=0;i<len;i++)
        {
            if(ones[i]-ones[0]!=ones[i+len]-ones[len] || ones[i]-ones[0]!=ones[i+2*len]-ones[2*len])
                return vector<int>({-1,-1});
        }
        //the pattern is the same, we need check number of zeros
        int nzeros3=A.size()-1-ones.back();
        int nzeros2=ones[2*len]-ones[2*len-1]-1;
        int nzeros1=ones[len]-ones[len-1]-1;
   
        if(nzeros1>=nzeros3 && nzeros2>=nzeros3)
        {
            return vector<int>({ones[len-1]+nzeros3,ones[2*len-1]+nzeros3+1});
        }
        return vector<int>({-1,-1});
    }
```    

### 936. Stamping The Sequence.md
### Problem Summary
Given a stamp string and a target string, return the stamping move sequence.
for example, stamp="abc", target="ababc", stamp at 0 and then stamp at 2

### approach
The key is to reverse the process: we first match the whole stamp which would be the last stamp sequence and change them to ***

```cpp
    vector<int> movesToStamp(string stamp, string target) {
        //reverse operation: matched then change it to ***
        //until we change the target string into *****
        //note we can only match one end if it is covered
        int n=target.length();
        string final(n,'*');
        vector<int> ans;
        while(target!=final)
        {
            int ind=match_change(target,stamp);
            if(ind==-1) return vector<int>();
            ans.push_back(ind);
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
    int match_change(string& target,string stamp)
    {
        //find the first matching and return
        //at least has one non * char inside, * matches any char
        bool matched=0;
        for(int i=0;i<target.size();i++)
        {
            int cnt_match=0;
            int j=0;
            for(j=0;j<stamp.size();j++)
            {
                if(target[i+j]=='*') continue;
                if(target[i+j]==stamp[j]) cnt_match++;
                else break;
            }
            if(j==stamp.size()&& cnt_match) 
            {
                for(j=0;j<stamp.size();j++) target[i+j]='*';
                return i;
            }
        }
        return -1; //no matching
    }
```

### 984. String Without AAA or BBB
just one way to build the string
A=0: b
B=0: a
A==B: ab
A>B: aab
A<B: abb


```cpp
    string strWithout3a3b(int A, int B) {
        if(A == 0) return string(B, 'b');
        else if(B == 0) return string(A, 'a');
        else if(A == B) return "ab" + strWithout3a3b(A - 1, B - 1);
        else if(A > B) return "aab" + strWithout3a3b(A - 2, B - 1);
        else return strWithout3a3b(A - 1, B - 2) + "abb";
    }
```	
for example 1a and 3b, we always need keep the single a or b with the remaining substring
"abb"+strWithout3a3b(A-1,B-2) is not correct.


