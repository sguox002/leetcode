## contest 189
### 1450. Number of Students Doing Homework at a Given Time
<em>
Given two integer arrays startTime and endTime and given an integer queryTime.

The ith student started doing their homework at the time startTime[i] and finished it at time endTime[i].

Return the number of students doing their homework at time queryTime. More formally, return the number of students where queryTime lays in the interval [startTime[i], endTime[i]] inclusive.
</em>

- since the length is <100, so we can use brutal force.
sort it, we need find the one with ending time >=querytime

- O(N): just loop over each interval and count those covering the time.

- map approach: startTime++, endTime-- (inclusive endTime+1--)
then do the prefix sum. For this we can just get each point value. 
This is good for multiple query. query is O(1)
but for this problem it is overkill.

```cpp
    int busyStudent(vector<int>& startTime, vector<int>& endTime, int queryTime) {
        map<int,int> mp;
        for(int i=0;i<startTime.size();i++){
            mp[startTime[i]]++;
            mp[endTime[i]+1]--;
        }
        int pre=0,prev=0;
        for(auto t: mp){
            if(queryTime>=prev && queryTime<t.first) return pre;
            pre+=t.second;
            
            prev=t.first;
        }
        return 0;
    }
```

### 1451. Rearrange Words in a Sentence
<em>
Given a sentence text (A sentence is a string of space-separated words) in the following format:

First letter is in upper case.
Each word in text are separated by a single space.
Your task is to rearrange the words in text such that all words are rearranged in an increasing order of their lengths. If two words have the same length, arrange them in their original order.

Return the new text following the format shown above.
</em>
	
Brutal force approach:
convert to vector of string and then sort by length
Note we need keep original order.
- use stable_sort

```cpp
    string arrangeWords(string text) {
        vector<string> vs;
        stringstream ss(text);
        string w;
        while(ss>>w){
            w[0]=tolower(w[0]);
            vs.push_back(w);
        }
        stable_sort(begin(vs),end(vs),[](const string& a,const string& b){
            return a.size()<b.size();
        });
        vs[0][0]=toupper(vs[0][0]);
        string ans;
        for(auto word: vs){
            ans+=word+" ";
        }
        ans.pop_back();
        return ans;
    }
```
- note for stable_sort, the const is required.
- if using sort, we need carry the index information.

### 1452. People Whose List of Favorite Companies Is Not a Subset of Another List
<em>
Given the array favoriteCompanies where favoriteCompanies[i] is the list of favorites companies for the ith person (indexed from 0).

Return the indices of people whose list of favorite companies is not a subset of any other list of favorites companies. You must return the indices in increasing order.
1 <= favoriteCompanies.length <= 100
1 <= favoriteCompanies[i].length <= 500
1 <= favoriteCompanies[i][j].length <= 20
All strings in favoriteCompanies[i] are distinct.
All lists of favorite companies are distinct, that is, If we sort alphabetically each list then favoriteCompanies[i] != favoriteCompanies[j].
All strings consist of lowercase English letters only. 
</em>

note using brutal force to check if one is another's subgroup O(N^2*M)
if using string, this requires another L.
This will get TLE if using string and set operations.

Optimization: using bit (or bool vector) to assign each person a bool vector.
```cpp
    vector<int> peopleIndexes(vector<vector<string>>& comp) {
        vector<int> ans;
        set<string> ms;
        for(auto t: comp){
            for(auto s: t) ms.insert(s);
        }
        unordered_map<string,int> mp;
        int i=0;
        for(auto t: ms) mp[t]=i++;
        vector<vector<bool>> v(comp.size(),vector<bool>(ms.size()));
        for(int i=0;i<comp.size();i++){
            for(int j=0;j<comp[i].size();j++) {
                string& s=comp[i][j];
                v[i][mp[s]]=1;
            }
        }
            
        //now just check the bools
        for(int i=0;i<comp.size();i++){
            bool valid=1;
            for(int j=0;j<comp.size();j++){
                if(i==j) continue;
                if(is_subset(v[i],v[j])) {
                    valid=0;
                    break;
                }
            }
            if(valid) ans.push_back(i);
        }
        return ans;
        //using bits to assign a number to it
    }
    bool is_subset(vector<bool>& a,vector<bool>& b){
        for(int i=0;i<a.size();i++){
            if(a[i] && !(a[i]&&b[i])) return 0; //
        }
        return 1;
    }
```

### 1453. Maximum Number of Darts Inside of a Circular Dartboard
<em>

You have a very large square wall and a circular dartboard placed on the wall. You have been challenged to throw darts into the board blindfolded. Darts thrown at the wall are represented as an array of points on a 2D plane. 

Return the maximum number of points that are within or lie on any circular dartboard of radius r.

</em>

The idea:
brutal force: check each pair of points as a straightline, and draw a circle with radius r. There is only two circles.
check how many points lies inside it.
why? that is the key: if we found the circle covering the most points, we can always shrink the circle to have two points on the circle.
maybe move the circle is more suitable without losing a single point.

- just pick one side circle is fine
- need use double and add an eps

```cpp
    int numPoints(vector<vector<int>>& A, int r) {
        int res = 1, n = A.size();
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int x1 = A[i][0], y1 = A[i][1];
                int x2 = A[j][0], y2 = A[j][1];
                double d = sqrt((x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2));
                if (d > r * 2) continue;
                double x0 = (x1 + x2) / 2.0 + (y2 - y1) * sqrt(r * r - d * d / 4) / d;
                double y0 = (y1 + y2) / 2.0 - (x2 - x1) * sqrt(r * r - d * d / 4) / d;
                int cnt = 0;
                for (vector<int>& point : A) {
                    double x = point[0], y = point[1];
                    if ((x - x0) * (x - x0) + (y - y0) * (y - y0) <= r * r + 0.00001) {
                        cnt++;
                    }
                }
                res = max(res, cnt);
            }
        }
        return res;
    }
```	
	


