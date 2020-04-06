## contest 183 4/4/2020

### 1403. Minimum Subsequence in Non-Increasing Order
<em>
Given the array nums, obtain a subsequence of the array whose sum of elements is strictly greater than the sum of the non included elements in such subsequence. 

If there are multiple solutions, return the subsequence with minimum size and if there still exist multiple solutions, return the subsequence with the maximum total sum of all its elements. A subsequence of an array can be obtained by erasing some (possibly zero) elements from the array. 

Note that the solution with the given constraints is guaranteed to be unique. Also return the answer sorted in non-increasing order.
</em>

straightforward: sort and try largest first

```cpp
    vector<int> minSubsequence(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int tsum=accumulate(begin(nums),end(nums),0);
        int sum=0;
        vector<int> ans;
        for(int i=nums.size()-1;i>=0;i--){
            sum+=nums[i];
            ans.push_back(nums[i]);
            if(sum>tsum-sum) return ans;
        }
        return ans;
    }
```

### 1404. Number of Steps to Reduce a Number in Binary Representation to One

<em>
Given a number s in their binary representation. Return the number of steps to reduce it to 1 under the following rules:

If the current number is even, you have to divide it by 2.

If the current number is odd, you have to add 1 to it.

It's guaranteed that you can always reach to one for all testcases.
</em>

brutal force: string operations.

```cpp
    int numSteps(string s) {
        int ans=0;
        //number would be very large.
        while(s.size()>1){
            if(s.back()=='1') inc(s);
            else s.pop_back();
            ans++;
            //cout<<s<<endl;
        }
        if(s[0]=='0') ans++;
        return ans;
    }
    void inc(string& s){
        int cf=1;
        int i=s.size()-1;
        while(i>=0 && cf){
            int d=cf^(s[i]-'0');
            cf=cf&(s[i]-'0');
            s[i]='0'+d;
            i--;
        }
        if(cf) s="1"+s;
        //return s;
    }
```

smart way: do not have to simulate, but use a cf:
- if last bit is 0, discard it
- if last bit is 1, discard it but cf=1 to bring forward
- this optimize to O(N)

```cpp
int numSteps(string &s) {
    int res = 0, carry = 0;
    for (auto i = s.size() - 1; i > 0; --i) {
        ++res;
        if (s[i] - '0' + carry == 1) {
            carry = 1;
            ++res;
        }
    }
    return res + carry;
}
```

### 1405. Longest Happy String
<em>

A string is called happy if it does not have any of the strings 'aaa', 'bbb' or 'ccc' as a substring.

Given three integers a, b and c, return any string s, which satisfies following conditions:

s is happy and longest possible.
s contains at most a occurrences of the letter 'a', at most b occurrences of the letter 'b' and at most c occurrences of the letter 'c'.
s will only contain 'a', 'b' and 'c' letters.
If there is no such string s return the empty string ""

</em>

Approach:

- priority_queue: try to use the most frequent one first
- if the second one is one or zero less, we have to use 2 of it
otherwise we use one of it ( to make sure we have longest string and no violation)

```cpp
    string longestDiverseString(int a, int b, int c) {
        //always use the max one first two
        priority_queue<vector<int>> pq;
        if(a) pq.push({a,0});
        if(b) pq.push({b,1});
        if(c) pq.push({c,2});
        string s;
        while(pq.size()){
            auto t=pq.top();
            pq.pop();
            s.append(min(t[0],2),'a'+t[1]);
            
            if(pq.size()){
                auto t1=pq.top();
                pq.pop();
                //next time it may get 3 if it is max
                int next=0;
                if(t1[0]+1>=t[0]) next=min(t1[0],2);
                else next=min(t1[0],1);
                s.append(next,'a'+t1[1]);//to make sure it is smaller
                t[0]-=min(t[0],2);
                t1[0]-=next;
                if(t1[0]) pq.push(t1);
                if(t[0]) pq.push(t);
            }
        }
        return s;
    }
```

without using pq:
```cpp
string longestDiverseString(int a, int b, int c, char aa = 'a', char bb = 'b', char cc = 'c') {
    if (a < b)
        return longestDiverseString(b, a, c, bb, aa, cc);
    if (b < c)
        return longestDiverseString(a, c, b, aa, cc, bb);
    if (b == 0)
        return string(min(2, a), aa);
    auto use_a = min(2, a), use_b = a - use_a >= b ? 1 : 0; 
    return string(use_a, aa) +  string(use_b, bb) + 
		longestDiverseString(a - use_a, b - use_b, c, aa, bb, cc);
}
```

### 1406. Stone Game III

<em>

Alice and Bob continue their games with piles of stones. There are several stones arranged in a row, and each stone has an associated value which is an integer given in the array stoneValue.

Alice and Bob take turns, with Alice starting first. On each player's turn, that player can take 1, 2 or 3 stones from the first remaining stones in the row.

The score of each player is the sum of values of the stones taken. The score of each player is 0 initially.

The objective of the game is to end with the highest score, and the winner is the player with the highest score and there could be a tie. The game continues until all the stones have been taken.

Assume Alice and Bob play optimally.

Return "Alice" if Alice will win, "Bob" if Bob will win or "Tie" if they end the game with the same score.
</em>


Approach:

- dynamic programming
- 	use bob's score as negative and accumulate the scores.

```cpp
    string stoneGameIII(vector<int>& stoneValue) {
        //dp: both plays optimally
        int n=stoneValue.size();
        vector<int> mem(n,INT_MIN);
        int ans=dp(stoneValue,0,mem);
        if(ans>0) return "Alice";
        else if(ans<0) return "Bob";
        return "Tie";
    }
    int dp(vector<int>& stone,int start,vector<int>& mem){
        int ans=INT_MIN;
        if(start>=stone.size()) return 0;
        if(mem[start]>INT_MIN) return mem[start];
        //choose 1,2,3
        int sum=0;
        for(int i=start;i<min(start+3,(int)stone.size());i++){
            sum+=stone[i];
            ans=max(ans,sum-dp(stone,i+1,mem));
        }
        return mem[start]=ans;
    }
```	
- it is essential to initialize as INT_MIN instead of 0.
	

