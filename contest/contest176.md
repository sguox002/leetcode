## contest 176

1346. Check If N and Its Double Exist
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

1347. Minimum Number of Steps to Make Two Strings Anagram
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

1348. Tweet Counts Per Frequency
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

1349. Maximum Students Taking Exam
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
				if(seats[i][j]=='.') s[i][j]=1;
			}
		}
		int ans=0;
		backtrack(s,dp,0,ans);
		return ans;
    }
	void backtrack(vector<bitset<8>>& s,vector<bitset<8>> state,int row,int& mx){
		if(row>=s.size()) {
			int sum=0;
			for(auto t: state) sum+=t.count();
			mx=max(mx,sum);
			return;
		}
		int n=s[0].size();
		if(row==0){ //there is no previous row
			int t=s[0].to_ulong();
			for(int i=0;i<1<<n;i++){ //try all combinations
				if(i&(~t)) continue;
				state[0]=bitset<8>(i);
				bool valid=1;
				for(int j=1;j<n;j++) {
					if(state[row][j] && state[row][j-1]){
						valid=0;break;
					}
				}
				if(valid) backtrack(s,state,row+1,mx);
			}
		}
		else{
			for(int i=0;i<n;i++){
				if(s[row][i] && (i?!state[row][i-1]:1) && 
				(i?!state[row-1][i-1]:1) && 
				(i+1<n?!state[row-1][i+1]:1)){
					state[row][i]=1;
				}
			}
			backtrack(s,state,row+1,mx);
		}

	}
```	
= this passed the example tests but failed with some cases
- the problem is for other rows we also need to try all combinations.

- dp: row by row.

