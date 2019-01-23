Greedy:
choose a first move and proof it is a safe move which is consistent to a global optimal solution. Then the problem is reduced to a smaller sized subproblem.
Generally sort will improve the efficiency.

example: 
- n digits to form a largest number, each time choose the largest number from remaining digits
- min refueling stations: refill at the farthest reachable station
- min group of children with age difference <=1, sort first and use segment to cover the leftmost.
- fractional knapsack: sort with the value/weight, and choose as much as possible the current highest value.

If your first move is not safe, the solution is incorrect.

944. Delete Columns to Make Sorted
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

122. Best Time to Buy and Sell Stock II
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

860. Lemonade Change
greedy: always gives changes of the large bill first. 

saving the change for 5, 10, and 20 separately.

455. Assign Cookies
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

874. Walking Robot Simulation
starting from (0,0), -2: turn left, -1: turn right, x (1,9) forward nsteps. with obstacle, then stop before the obstacle.
get the largest distance from the origin.

each step we can calculate its position and then we get the largest distance

we have 4 directions: -x, +x, -y, +y which can be represented as (-1,0) (1,0) (0,-1) (0,1)
turn right: rotate -90 degrees: 
turn left: rotate 90 degrees: 
we can use rotation matrix for the direction changes.
how to easily judge if the obstacle is on the way?
can use a string to include the x and y coordinate and move step by step and stop when we meet an obstacle.


921. Minimum Add to Make Parentheses Valid
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


861. Score After Flipping Matrix
a move choose any row or column and toggle its value.
Each row represents a binary number, return the largest sum of all these numbers.
greedy choice: 
the MSB shall all toggles to 1, by toggling the row
the non-MSB shall have more 1s than 0s. toggling columns

763. Partition Labels
split the string so that each char only appear in one part.
straightforward:
record each char's start and ending position and then merge the segments if they overlap.
efficient one:
don't use hashmap but use array for each char and record its ending index. Merging the segments only matters the ending.
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

406. Queue Reconstruction by Height
each person has a height and number of person taller or equal to him in front of him.
reconstruct the queue
sort the people in descend order but the rank in ascending order, so the same height people is in order. Then insert each lower person.
This is a greedy choice.
for example: [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
sort as: [7,0], [7,1], [6,1],[5,0],[5,2],[4,4]
each step:
[7,0]
[7,0], [7,1]
[7,0],[6,1],[7,1]
[5,0],[7,0],[6,1],[7,1]
[5,0],[7,0],[6,1],[5,2],[7,1]
[5,0],[7,0],[6,1],[5,2],[4,4],[7,1]
sorting is the key to make sure all member inside the array are taller than current one.
```cpp
bool cmp(pair<int,int>& a,pair<int,int>& b) {return a.first>b.first || (a.first==b.first && a.second<b.second);}
    vector<pair<int, int>> reconstructQueue(vector<pair<int, int>>& people) {
        //reverse: we sort the highest person first, they are ordered
        //then the next, 
        sort(people.begin(),people.end(),cmp); //descending order
        vector<pair<int,int>> vs;
        for(int i=0;i<people.size();i++)
        {
            vs.insert(vs.begin()+people[i].second,people[i]);
            for(int i=0;i<vs.size();i++) cout<<"("<<vs[i].first<<" "<<vs[i].second<<") ";cout<<endl;
        }
        return vs;
    }
```

714. Best Time to Buy and Sell Stock with Transaction Fee
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
we shall tolerate some variation, only when price[i]<price[i-1]-fee we can sell at i-1 and buy at i. (i.e the dip shall be deep enough to cover the transaction fee, otherwise it is not worthful). Once we start a transaction, we reduce the problem into a smaller subproblem with the new minimum, which is a typical greedy approach.</br>

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

392. Is Subsequence
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

452. Minimum Number of Arrows to Burst Balloons
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

621. Task Scheduler
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
        unordered_map<char,int>mp;
        int count = 0;
        for(auto e : tasks)
        {
            mp[e]++;
            count = max(count, mp[e]);
        }
        
        int ans = (count-1)*(n+1);
        for(auto e : mp) if(e.second == count) ans++;
        return max((int)tasks.size(), ans);
    }
```

881. Boats to Save People
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

435. Non-overlapping Intervals
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

738. Monotone Increasing Digits
Given a non-negative integer N, find the largest number that is less than or equal to N with monotone increasing digits.
(digits are sorted)
greedy: reversely find the inversion. if found an inversion we reduce the previous by 1, thus to propagate the changes forward.

example: 3421->3411->3311->3399
example: 3431->3421->3321->3399
example: 144267->144267->144267->143267->133267->13999
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

870. Advantage Shuffle
Given two arrays A and B of equal size, the advantage of A with respect to B is the number of indices i for which A[i] > B[i].

Return any permutation of A that maximizes its advantage with respect to B.
Greedy solution: if there is an element in A >B[i], then choose the smallest one. 
运用田忌赛马的思路，如果A中有相应的higher元素，使用最小的higher元素。否则使用A中最小的元素 :)

767. Reorganize String
neighboring characters are not the same.
if the highest freq char appears more than half of the length, we cannot fulfill the requirement.
Otherwise we divide into k chunks by appending other chars into the chunk.
c++ using priority_queue by removing the highest freq one and followed by the 2nd highest priority one. -1 and put back. A good example to use PQ for this.

```cpp
    string reorganizeString(string S) {
        priority_queue<pair<int, char>> pq;
        int map[26] = { 0 };
        
        for (auto c : S) 
        {
            if (++map[c-'a'] > (S.size() + 1)/2)
                return "";
        }
        
        for (int i = 0; i < 26; ++i) 
        {
            if (map[i]) pq.push({map[i], i + 'a'});
        }
        
        string ans;
        while(!pq.empty()) 
        {
            pair<int, char> p1, p2;
            p1 = pq.top(); pq.pop();
            ans.push_back(p1.second);
            if (!pq.empty()) 
            {
                p2 = pq.top(); pq.pop();
                ans.push_back(p2.second);
                if (--p2.first) pq.push(p2);
            }
            if (--p1.first) pq.push(p1);
        }
        return ans;
    }
```    

659. Split Array into Consecutive Subsequences

You are given an integer array sorted in ascending order (may contain duplicates), you need to split them into several subsequences, where each subsequences consist of at least 3 consecutive integers. Return whether you can make such a split.

for each card, either it can be appended to existent, or start a new group or it is a single

```cpp
    bool isPossible(vector<int>& nums) {
        unordered_map<int, int> freq, need;
        for (int num : nums) ++freq[num];
        for (int num : nums) {
            if (freq[num] == 0) continue;
            else if (need[num] > 0) {
                --need[num];
                ++need[num + 1];
            } else if (freq[num + 1] > 0 && freq[num + 2] > 0) {
                --freq[num + 1];
                --freq[num + 2];
                ++need[num + 3];
            } else return false;
            --freq[num];
        }
        return true;
    }
```

948. Bag of Tokens
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

649. Dota2 Senate
two parties. according order, each senate can ban a senator from other party, predict who is the winner.
greedy: ban its next member from other party since otherwise it will ban a member from your party.
using two queues: if the member exercises its right, move it to the end of the queue.
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

376. Wiggle Subsequence
greedy or dp: the first number could be smaller or larger number.
```cpp
    int wiggleMaxLength(vector<int>& nums) {
        if(nums.size()==0) return 0;
        int up=1,dn=1;
        for(int i=1;i<nums.size();i++)
        {
            if(nums[i]>nums[i-1]) up=dn+1;
            if(nums[i]<nums[i-1]) dn=up+1;
            //cout<<nums[i]<<"\t"<<up<<" "<<dn<<endl;
        }
        return max(up,dn);
    }
```

842. Split Array into Fibonacci Sequence
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





