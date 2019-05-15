Interval

There are quite a few interval related problems.

986. Interval List Intersections
find the intersection of two list of intervals.

```cpp
    vector<vector<int>> intervalIntersection(vector<vector<int>>& A, vector<vector<int>>& B) {
        vector<vector<int>> ans;
        int i=0,j=0;
        int m=A.size(),n=B.size();
        while(i<m && j<n)
        {
            int head=max(A[i][0],B[j][0]);
            int tail=min(A[i][1],B[j][1]);
            if(A[i][1]>=head && A[i][1]<=tail) ans.push_back({head,tail}),i++;
            else if(B[j][1]>=head && B[j][1]<=tail) ans.push_back({head,tail}),j++;
            else //no overlap
            {
                if(A[i][1]<=tail) i++;
                else j++;
            }
        }
        return ans;
    }
```	

56. Merge Intervals

```cpp
vector<Interval> merge(vector<Interval>& ins) {
    if (ins.empty()) return vector<Interval>{};
    vector<Interval> res;
    sort(ins.begin(), ins.end(), [](Interval a, Interval b){return a.start < b.start;});
    res.push_back(ins[0]);
    for (int i = 1; i < ins.size(); i++) {
        if (res.back().end < ins[i].start) res.push_back(ins[i]);
        else
            res.back().end = max(res.back().end, ins[i].end);
    }
    return res;
}
```

57. Insert Interval
```cpp
    vector<Interval> insert(vector<Interval>& intervals, Interval newInterval) {
        vector<Interval> res;
        int n=intervals.size();
        //the input intervals are sorted
        int ind=int(lower_bound(intervals.begin(),intervals.end(),newInterval,cmp)-intervals.begin());//the start >= the new start
        //int ind1=int(upper_bound(intervals.begin(),intervals,.end(),Interval(newInterval.end,0))-intervals.begin()); //the end < start
        //merge the intervals from ind to ind1
        //cout<<ind;
        //if(ind==n) {res=intervals;res.push_back(newInterval);}
        //ind may be 0
        res.resize(ind);
        copy(intervals.begin(),intervals.begin()+ind,res.begin());
        
        if(ind==0)
        {
            res.push_back(newInterval);
        }
        else
        {
            //the new interval may overlap or not with interval[ind]
            if(newInterval.start>res.back().end) //no overlap
            {
                res.push_back(newInterval);
            }
            else //its end shall be replaced
            {
                res.back().end=max(res.back().end,newInterval.end);
            }
        }
        //keep adding or merging the remaining intervals
        for(int i=ind;i<n;i++)
        {
            if(res.back().end<intervals[i].start) res.push_back(intervals[i]);
            else res.back().end=max(res.back().end,intervals[i].end);
        }
        return res;
	}
```

435. Non-overlapping Intervals
Given a collection of intervals, find the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.
equivalently find the max number of non-overlapped intervals
```cpp
bool cmp(Interval& a,Interval& b) {return a.end<b.end;}
class Solution {
public:
    int eraseOverlapIntervals(vector<Interval>& intervals) {
        //if we sort the intervals according to start, the end is also sorted for non-overlapped cases
        //or the other problem: the largest number of steps 
        //greedy: always choose the interval with the ending smaller??
        if(intervals.size()==0) return 0;
        sort(intervals.begin(),intervals.end(),cmp);
        int ans=0;
        int cur_end=intervals[0].end;ans++;
        for(int i=0;i<intervals.size();i++)
        {
            //find the next interval using current ending, start>=current ending
            if(intervals[i].start>=cur_end) {cur_end=intervals[i].end;ans++;}
        }
        return intervals.size()-ans;
    }
};
```

436. Find Right Interval
Given a set of intervals, for each of the interval i, check if there exists an interval j whose start point is bigger than or equal to the end point of the interval i, which can be called that j is on the "right" of i.

For any interval i, you need to store the minimum interval j's index, which means that the interval j has the minimum start point to build the "right" relationship for interval i. If the interval j doesn't exist, store -1 for the interval i. Finally, you need output the stored value of each interval as an array.

```cpp
    vector<int> findRightInterval(vector<Interval>& intervals) {
        int n=intervals.size();
		vector<pair<Interval,int>> v(n);
		for(int i=0;i<n;i++) v[i]=make_pair(intervals[i],i);
		sort(v.begin(),v.end(),cmp);
		vector<int> ans(n,-1);
		for(int i=0;i<n;i++)
		{
			int ind=int(lower_bound(v.begin(),v.end(),make_pair(Interval(intervals[i].end,0),0),cmp)-v.begin());
			if(ind!=n) ans[i]=v[ind].second;
		}
		return ans;
    }
```

352. Data Stream as Disjoint Intervals
Given a data stream input of non-negative integers a1, a2, ..., an, ..., summarize the numbers seen so far as a list of disjoint intervals.

For example, suppose the integers from the data stream are 1, 3, 7, 2, 6, ..., then the summary will be:

[1, 1]
[1, 1], [3, 3]
[1, 1], [3, 3], [7, 7]
[1, 3], [7, 7]
[1, 3], [6, 7]
Follow up:
What if there are lots of merges and the number of disjoint intervals are small compared to the data stream's size?

```cpp
    struct Cmp{
        bool operator()(Interval a, Interval b){ return a.start < b.start; }
    };
    set<Interval, Cmp> st;

    void addNum(int val) {
        auto it = st.lower_bound(Interval(val, val));
        int start = val, end = val;
        if(it != st.begin() && (--it)->end+1 < val) it++;
        while(it != st.end() && val+1 >= it->start && val-1 <= it->end)
        {
            start = min(start, it->start);
            end = max(end, it->end);
            it = st.erase(it);
        }
        st.insert(it,Interval(start, end));
    }
    
    vector<Interval> getIntervals() {
        vector<Interval> result;
        for(auto val: st) result.push_back(val);
        return result;
    }

```

621. Task Scheduler
given a list of tasks and requirement, return the min number of intervals needed
somewhat similar to the playlist
choose the most frequent char and divide it into k interval with n period
and put other inside, the min time is (k-1)*n
however when n is smaller than the period, we need suppress non-necessary idle

```cpp
    int leastInterval(vector<char>& tasks, int n) {
        //same task can be only performed after n interval
        //idea: perform different task in a sequence
        //it largely depends on the number of instance of each task
        //the idea is always perform the task with the largest instance number (priority)
        //c++ priority-queue however does not support dynamic priority, need using other data structure
        //after the cooling period is exceeded, need to reorganize the data structure. Note we cannot do it dynamically
        int num_int_total=0;
        int numtask[26]={0};

        for(int i=0;i<tasks.size();i++) numtask[tasks[i]-'A']++;
        sort(numtask,numtask+26,greater<int>());
        int num_int=0;
        while(1)
        {
            num_int=0;
            for(int i=0;i<=n;i++)
            {
                if(numtask[i]>0)
                {
                    num_int++;
                    numtask[i]--;
                }
                else break;
            }
            if(num_int==0) break;
            num_int_total+=num_int;
            sort(numtask,numtask+26,greater<int>());
            if(numtask[0]) num_int_total+=(n-num_int+1);//add idle
        }
        
        return num_int_total;
    }
```

729. My Calendar I
Implement a MyCalendar class to store your events. A new event can be added if adding the event will not cause a double booking.

Your class will have the method, book(int start, int end). Formally, this represents a booking on the half open interval [start, end), the range of real numbers x such that start <= x < end.

A double booking happens when two events have some non-empty intersection (ie., there is some time that is common to both events.)

For each call to the method MyCalendar.book, return true if the event can be added to the calendar successfully without causing a double booking. Otherwise, return false and do not add the event to the calendar.

Your class will be called like this: MyCalendar cal = new MyCalendar(); MyCalendar.book(start, end)

double booking
```cpp
    set<pair<int, int>> books;

    bool book(int s, int e) {
        auto next = books.lower_bound({s, e}); // first element with key not go before k (i.e., either it is equivalent or goes after).
        if (next != books.end() && next->first < e) return false; // a existing book started within the new book (next)
        if (next != books.begin() && s < (--next)->second) return false; // new book started within a existing book (prev)
        books.insert({ s, e });
        return true;
    }
```

731. My Calendar II
triple booking

```cpp
class MyCalendar {
    vector<pair<int, int>> books;

    bool book(int start, int end) {
        for (pair<int, int> p : books)
            if (max(p.first, start) < min(end, p.second)) return false;
        books.push_back({start, end});
        return true;
    }
};

class MyCalendarTwo {
    vector<pair<int, int>> books;
public:
    bool book(int start, int end) {
        MyCalendar overlaps;
        for (pair<int, int> p : books) {
            if (max(p.first, start) < min(end, p.second)) { // overlap exist
                pair<int, int> overlapped = getOverlap(p.first, p.second, start, end);
                if (!overlaps.book(overlapped.first, overlapped.second)) return false; // overlaps overlapped
            }
        }
        books.push_back({ start, end });
        return true;
    }

    pair<int, int> getOverlap(int s0, int e0, int s1, int e1) {
        return { max(s0, s1), min(e0, e1)};
    }
};
```	
732. My Calendar III	
K booking
Implement a MyCalendarThree class to store your events. A new event can always be added.

Your class will have one method, book(int start, int end). Formally, this represents a booking on the half open interval [start, end), the range of real numbers x such that start <= x < end.

A K-booking happens when K events have some non-empty intersection (ie., there is some time that is common to all K events.)

For each call to the method MyCalendar.book, return an integer K representing the largest integer such that there exists a K-booking in the calendar.

Your class will be called like this: MyCalendarThree cal = new MyCalendarThree(); MyCalendarThree.book(start, end)

```cpp
class MyCalendarThree {
    struct Node{
        int val,cnt;
        Node *left,*right;
        Node(int v,int c):val(v),cnt(c){left=right=0;}
    };
    Node *root;
    int curt,maxcnt;
    Node* insert(Node* root,int key,int v)
    {
        if(!root) return new Node(key,v);
        if(key>root->val) root->right=insert(root->right,key,v);
        else if(key<root->val) root->left=insert(root->left,key,v);
        else {root->cnt+=v;}
        return root;
    }
    void count(Node* root)
    {
        if(!root) return;
        count(root->left);
        curt+=root->cnt;
        maxcnt=max(curt,maxcnt);
        count(root->right);
    }
public:
    MyCalendarThree() {
        maxcnt=curt=0;
        root=0;
    }
    
    int book(int start, int end) {
        root=insert(root,start,1);
        root=insert(root,end,-1);
        curt=maxcnt=0;
        count(root);
        return maxcnt;
    }
};
```
better and simpler:
This is to find the maximum number of concurrent ongoing event at any time.

We can log the start & end of each event on the timeline, each start add a new ongoing event at that time, each end terminate an ongoing event. Then we can scan the timeline to figure out the maximum number of ongoing event at any time.

The most intuitive data structure for timeline would be array, but the time spot we have could be very sparse, so we can use sorted map to simulate the time line to save space.

```cpp
class MyCalendarThree {
    map<int, int> timeline;
public:
    int book(int s, int e) {
        timeline[s]++; // 1 new event will be starting at [s]
        timeline[e]--; // 1 new event will be ending at [e];
        int ongoing = 0, k = 0;
        for (pair<int, int> t : timeline)
            k = max(k, ongoing += t.second);
        return k;
    }
};
```

### 495. Teemo Attacking
In LOL world, there is a hero called Teemo and his attacking can make his enemy Ashe be in poisoned condition. Now, given the Teemo's attacking ascending time series towards Ashe and the poisoning time duration per Teemo's attacking, you need to output the total time that Ashe is in poisoned condition.

You may assume that Teemo attacks at the very beginning of a specific time point, and makes Ashe be in poisoned condition immediately.

need subtract those overlaps

```cpp
    int findPoisonedDuration(vector<int>& timeSeries, int duration) {
        //timeseries point is the left side point, duration is the length of the line segment
        //the problem needs to accumulate the line. timeseries is sorted
        //brutal force solution
        int tp=0;
        int tend=0;
        for(int i=0;i<timeSeries.size();i++)
        {
            tp+=duration-(timeSeries[i]<tend)*(tend-timeSeries[i]);
            tend=timeSeries[i]+duration;
        }
        return tp;
    }
```	

