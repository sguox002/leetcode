## biweek 19

### 1342. Number of Steps to Reduce a Number to Zero
<em>
Given a non-negative integer num, return the number of steps to reduce it to zero. If the current number is even, you have to divide it by 2, otherwise, you have to subtract 1 from it.</em>

Really simple:
```cpp
    int numberOfSteps (int num) {
        int ans=0;
        while(num){
            if(num%2==0) num/=2;
            else num--;
            ans++;
        }
        return ans;
    }
```

### 1343. Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold
<em>
Given an array of integers arr and two integers k and threshold.

Return the number of sub-arrays of size k and average greater than or equal to threshold.
</em>

classical sliding window:
```cpp
    int numOfSubarrays(vector<int>& arr, int k, int threshold) {
        int target=k*threshold;
        //moving average
        int i=0,sum=0,ans=0;
        while(i<arr.size()){
            sum+=arr[i];
            if(i>=k){
                sum-=arr[i-k];
            }
            if(i>=k-1 && sum>=target) ans++;
            i++;
        }
        return ans;
    }
```

### 1344. Angle Between Hands of a Clock
<em>
Given two numbers, hour and minutes. Return the smaller angle (in sexagesimal units) formed between the hour and the minute hand.
</em>
Simple: get the min and hr angle position and get the difference.
if <0 we add 360. then check if it >180, choose the other one.

```cpp
    double angleClock(int hour, int minutes) {
        //a min 360/60 degree, a hr: 360/12=
        int min_angle=minutes*6;
        double hr_angle=hour*30+minutes/60.0*30;
        double ans=hr_angle-min_angle;
        if(ans<0)  ans+=360;
        if(ans>180) ans=360-ans;
        return ans;
    }
```

### 1345. Jump Game IV
<em>
Given an array of integers arr, you are initially positioned at the first index of the array.

In one step you can jump from index i to index:

i + 1 where: i + 1 < arr.length.
i - 1 where: i - 1 >= 0.
j where: arr[i] == arr[j] and i != j.
Return the minimum number of steps to reach the last index of the array.

Notice that you can not jump outside of the array at any time.
</em>

typical bfs problem with hashmap

```cpp
    int minJumps(vector<int>& arr) {
        //bfs
        unordered_map<int,vector<int>> mp;
        for(int i=arr.size()-1;i>=0;i--) mp[arr[i]].push_back(i);
        int n=arr.size();
       
        queue<int> q;
        vector<bool> v(n);
        q.push(0),v[0]=1;
        int step=0;
        while(q.size()){
            int sz=q.size();
            while(sz--){
                int cur=q.front();
                q.pop();
                if(cur==n-1) return step;
                if(cur+1<n && !v[cur+1]){
                    q.push(cur+1),v[cur+1]=1;
                }
                if(cur-1>=0 && !v[cur-1]){
                    q.push(cur-1),v[cur-1]=1;
                }
                for(int i: mp[arr[cur]]){ //this could be very lengthy and will cause TLE
                    if(i==cur || v[i]) continue;
                    q.push(i),v[i]=1;
                }
                mp.erase(arr[cur]);
            }
            step++;
        }
        return -1;
    }
```

- the key for the problem is the mp.erase(arr[cur]) otherwise it will have to repeatedly check and will cause TLE.

	
