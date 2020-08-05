## contest 175
### 1346. Check If N and Its Double Exist
<em>
Given an array arr of integers, check if there exists two integers N and M such that N is the double of M ( i.e. N = 2 * M).

More formally check if there exists two indices i and j such that :

i != j
0 <= i, j < arr.length
arr[i] == 2 * arr[j]
</em>

Analysis: using hashmap, sort it. 
only check even numbers. 
for positives, check n/2
for negatives, check 2*n.

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

### 1347. Minimum Number of Steps to Make Two Strings Anagram
<em>
Given two equal-size strings s and t. In one step you can choose any character of t and replace it with another character.

Return the minimum number of steps to make t an anagram of s.

An Anagram of a string is a string that contains the same characters with a different (or the same) ordering.
</em>

Analysis:
- calculate the hist for s.
- subtract all occurrance of t 
- add all negatives
```cpp
    int minSteps(string s, string t) {
        vector<int> cnt(26);
        for(char c: s) cnt[c-'a']++;
        int ans=0;
        //need to replace all non-exist chars
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
This problem is simple but the contest test is a disaster. It judges the right to be wrong.

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
- using multiset for same id and same timestamp.
- using multiset actually is worse than vector since the distance is O(N) for multiset.

### 1349. Maximum Students Taking Exam
<em>
Given a m * n matrix seats  that represent seats distributions in a classroom. If a seat is broken, it is denoted by '#' character otherwise it is denoted by a '.' character.

Students can see the answers of those sitting next to the left, right, upper left and upper right, but he cannot see the answers of the student sitting directly in front or behind him. Return the maximum number of students that can take the exam together without any cheating being possible..

Students must be placed in seats in good condition.
Constraints:

seats contains only characters '.' and'#'.
m == seats.length
n == seats[i].length
1 <= m <= 8
1 <= n <= 8
</em>

My intuition on this is using backtracking, each valid seat can have an option to sit a person or not.
the complexity is O(2^n) n is up to 64. This will get TLE. and also it is not prooved that this will get the max solution.



	
 
 
