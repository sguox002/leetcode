## contest 176

### 1346. Check If N and Its Double Exist
<em>
Given an array arr of integers, check if there exists two integers N and M such that N is the double of M ( i.e. N = 2 * M).

More formally check if there exists two indices i and j such that :

i != j
0 <= i, j < arr.length
arr[i] == 2 * arr[j]
</em>
```cpp
    bool checkIfExist(vector<int>& arr) {
        sort(arr.begin(),arr.end(),greater<int>());
        unordered_set<int> ms;
        for(int i: arr){
            if(ms.count(i*2) || (ms.count(i/2) && i%2==0)) return 1;
            ms.insert(i);
        }
        return 0;
    }
```
- sort is not needed actually.

### 1347. Minimum Number of Steps to Make Two Strings Anagram
<em>
Given two equal-size strings s and t. In one step you can choose any character of t and replace it with another character.

Return the minimum number of steps to make t an anagram of s.

An Anagram of a string is a string that contains the same characters with a different (or the same) ordering.	
</em>

using hashmap and one add one subtract. The sum will be 0, and positive or negative sum is the answer.
```cpp
    int minSteps(string s, string t) {
        vector<int> cnt(26);
		int ans=0;    
		for(char c: s) cnt[c-'a']++;
        for(char c: t) cnt[c-'a']--;
        for(int i: cnt){
            if(i<0) ans+=-i;
        }
        return ans;
    }
```

### 1348. Tweet Counts Per Frequency
<em>
Implement the class TweetCounts that supports two methods:

1. recordTweet(string tweetName, int time)

Stores the tweetName at the recorded time (in seconds).
2. getTweetCountsPerFrequency(string freq, string tweetName, int startTime, int endTime)

Returns the total number of occurrences for the given tweetName per minute, hour, or day (depending on freq) starting from the startTime (in seconds) and ending at the endTime (in seconds).
freq is always minute, hour or day, representing the time interval to get the total number of occurrences for the given tweetName.
The first time interval always starts from the startTime, so the time intervals are [startTime, startTime + delta*1>,  [startTime + delta*1, startTime + delta*2>, [startTime + delta*2, startTime + delta*3>, ... , [startTime + delta*i, min(startTime + delta*(i+1), endTime + 1)> for some non-negative number i and delta (which depends on freq).  
</em>

note there is same name, same time, so use multimap
```cpp
    unordered_map<string,multiset<int>> mp;
    TweetCounts() {
        
    }
    
    void recordTweet(string tweetName, int time) {
        mp[tweetName].insert(time);
    }
    
    vector<int> getTweetCountsPerFrequency(string freq, string tweetName, int startTime, int endTime) {
        vector<int> ans;
        //from start time to end time using the frequency
        int dt=0;
        if(freq=="minute") dt=60;
        else if(freq=="hour") dt=3600;
        else dt=3600*24;
        for(int t=startTime;t<=endTime;t+=dt){
            auto it1=mp[tweetName].lower_bound(t); //>=t
            auto it2=mp[tweetName].lower_bound(min(t+dt,endTime+1));//>t+dt
            ans.push_back(distance(it1,it2));
        }
        return ans;
    }
```

### 1349. Maximum Students Taking Exam
<em>
Given a m * n matrix seats  that represent seats distributions in a classroom. If a seat is broken, it is denoted by '#' character otherwise it is denoted by a '.' character.

Students can see the answers of those sitting next to the left, right, upper left and upper right, but he cannot see the answers of the student sitting directly in front or behind him. Return the maximum number of students that can take the exam together without any cheating being possible..

Students must be placed in seats in good condition.
m,n<=8
</em>
- brutal force using bit for each empty set and remove those invalid and get the max. very high complexity!
- backtrack, try all the valid positions and sit a person (with a lot of repeated results)
```cpp
    int maxStudents(vector<vector<char>>& seats) {
		int m=seats.size(),n=seats[0].size();
        vector<bitset<8>> s(m),dp(m);
		for(int i=0;i<m;i++){
			for(int j=0;j<n;j++){
				if(seats[i][j]=='.') s[i][j]=1; //MSB
			}
		}
		int ans=0;
		backtrack(s,dp,0,ans);
		return ans;
    }
    bool valid(bitset<8>& a,bitset<8>& b,bitset<8>& mask,int nbit){
        bool ans=1;
        //note bitset 0 is the MSB
        for(int i=0;i<8;i++){
            if(!mask[i] && a[i]) {ans=0;break;} 
            if(a[i] && ((i?a[i-1]:0) || (i?b[i-1]:0) || (i+1<8?b[i+1]:0))) {
                ans=0;break;
            }
        }
        return ans;
    }
	void backtrack(vector<bitset<8>>& mask,vector<bitset<8>> state,int row,int& mx){
		if(row>=mask.size()) {
			int sum=0;
			for(auto t: state) sum+=t.count();
            mx=max(mx,sum);
			return;
		}
		int n=mask[0].size();

        for(int i=0;i<(1<<n);i++){ //try all combinations
            bitset<8> t(i<<(8-n)),pre(0);
            if(row) pre=state[row-1];
            if(valid(t,pre,mask[row],n)) {
                state[row]=t;
                //cout<<t.to_string()<<endl;
                backtrack(mask,state,row+1,mx);
            }
        }
	}
```	
- backtrack tries all kind of valid row and this is exponential and will TLE for some cases.
- backtrack solves some repeat cases and that is why it will TLE. For example for i-1 row with a given combination, row i may have same combination which is solved before.
- pay attention to bitset, bit0 is the MSB.
- backtrack is the base for dp solution (which is also brutal force and tries all kinds of combination
- applies memoization to backtracking.
* need to modify the backtracking code
* only input row and previous status
* do not use bitset since it is not good to represent in the table.

top down+ memoization based on the backtracking solution:

what to put into memory: 
- the state, the row, and the max. dp[i,j]: i is the number of rows, j is the state, dp[i,j] is the max student to seat for i rows.
- we do not need all those previous state, we only need current row state.
```cpp
    vector<vector<int>> dp;
    int maxStudents(vector<vector<char>>& seats) {
		int m=seats.size(),n=seats[0].size();
        vector<int> s(m),state(m+1);
		for(int i=0;i<m;i++){
			for(int j=0;j<n;j++){
				if(seats[i][j]=='.') s[i]+=1<<j; //LSB
			}
		}
        dp=vector<vector<int>>(m+1,vector<int>(1<<n,-1));
        //int mx=0;
		backtrack(s,state,1,n);
		return dp[0][0];
    }
    bool valid(bitset<8>& a,bitset<8>& b,bitset<8> mask,int nbit){
        bool ans=1;
        //note bitset 0 is the MSB
        for(int i=0;i<8;i++){
            if(!mask[i] && a[i]) {ans=0;break;} 
            if(a[i] && ((i?a[i-1]:0) || (i?b[i-1]:0) || (i+1<8?b[i+1]:0))) {
                ans=0;break;
            }
        }
        return ans;
    }
	int backtrack(vector<int>& mask,vector<int> state,int row,int n){
		if(row>mask.size()) {
			/*int sum=0;
			for(auto t: state) sum+=bitset<8>(t).count();
			return sum;*/
            return 0;
		}
		//int n=mask[0].size();
        int mx=0;
        int ind=state[row-1];
        if(dp[row-1][ind]>=0) return dp[row-1][ind];
        for(int i=0;i<(1<<n);i++){ //try all combinations
            bitset<8> t(i),pre(0);
            if(row) pre=state[row-1];
            if(valid(t,pre,bitset<8>(mask[row-1]),n)) {
                state[row]=i;
                //cout<<t.to_string()<<endl;
                mx=max(mx,(int)t.count()+backtrack(mask,state,row+1,n));
            }
        }
        return dp[row-1][ind]=mx;
	}
```
- add one extra row before row 0
- answer is then dp[0][0]
- get the max from all its combination and store it.


- dp: row by row.

