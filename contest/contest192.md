## contest 192

### 1470. Shuffle the Array
<em>
Given the array nums consisting of 2n elements in the form [x1,x2,...,xn,y1,y2,...,yn].

Return the array in the form [x1,y1,x2,y2,...,xn,yn].</em>

too simple


### 1471. The k Strongest Values in an Array
<em>
Median is the middle value in an ordered integer list. More formally, if the length of the list is n, the median is the element in position ((n - 1) / 2) in the sorted list (0-indexed).

 value arr[i] is said to be stronger than a value arr[j] if |arr[i] - m| > |arr[j] - m| where m is the median of the array.
If |arr[i] - m| == |arr[j] - m|, then arr[i] is said to be stronger than arr[j] if arr[i] > arr[j].
</em>

approach: sort first to get the median and then sort using the median difference

```cpp
    struct comp{
        int m;
        comp(int k):m(k){}
        bool operator()(int a,int b){
            return abs(a-m)>abs(b-m) || (abs(a-m)==abs(b-m) && a>b);
        }
    };
    vector<int> getStrongest(vector<int>& arr, int k) {
        sort(begin(arr),end(arr));
        int n=arr.size();
        int m=arr[(n-1)/2];
        sort(begin(arr),end(arr),comp(m));
        return {arr.begin(),arr.begin()+k};
    }
```

Approach 2: after first sort, we do not need to do second sort, it is the max and min element, similar to sorted array square
using two pointer

### 1472. Design Browser History
<em>
You have a browser of one tab where you start on the homepage and you can visit another url, get back in the history number of steps or move forward in the history number of steps.

Implement the BrowserHistory class:

BrowserHistory(string homepage) Initializes the object with the homepage of the browser.
void visit(string url) visits url from the current page. It clears up all the forward history.
string back(int steps) Move steps back in history. If you can only return x steps in the history and steps > x, you will return only x steps. Return the current url after moving back in history at most steps.
string forward(int steps) Move steps forward in history. If you can only forward x steps in the history and steps > x, you will forward only x steps. Return the current url after forwarding in history at most steps.
</em>

The only need to pay attention to is that visit need to clear all forward sites

```cpp
    vector<string> hist;
    int cur;
    BrowserHistory(string homepage) {
        hist.push_back(homepage);
        cur=0;
    }
    
    void visit(string url) {
        while(hist.size()>cur+1) hist.pop_back();
        hist.push_back(url);
        cur=hist.size()-1;
    }
    
    string back(int steps) {
        if(cur-steps<0) cur=0;
        else cur-=steps;
        return hist[cur];
    }
    
    string forward(int steps) {
        if(cur+steps>=hist.size()-1) cur=hist.size()-1;
        else cur+=steps;
        return hist[cur];
    }
```

### 1473. Paint House III
<em>
There is a row of m houses in a small city, each house must be painted with one of the n colors (labeled from 1 to n), some houses that has been painted last summer should not be painted again.

A neighborhood is a maximal group of continuous houses that are painted with the same color. (For example: houses = [1,2,2,3,3,2,1,1] contains 5 neighborhoods  [{1}, {2,2}, {3,3}, {2}, {1,1}]).

Given an array houses, an m * n matrix cost and an integer target where:

houses[i]: is the color of the house i, 0 if the house is not painted yet.
cost[i][j]: is the cost of paint the house i with the color j+1.
Return the minimum cost of painting all the remaining houses in such a way that there are exactly target neighborhoods, if not possible return -1.
</em>
dp, not hard, but I messed it up for the base case.
- initial add a color out of range.
- when k==0 we need continue to end of list (k<0 is the end of recursion)
- I use long to avoid int overflow.

```cpp
    int minCost(vector<int>& houses, vector<vector<int>>& cost, int m, int n, int target) {
        for(int& i: houses) i=i-1;
        vector<vector<vector<long>>> dp(m,vector<vector<long>>(target+1,vector<long>(n+1,-1)));
        long ans=helper(houses,cost,0,n,target,dp);
        return ans>=INT_MAX?-1:ans;
    }
    long helper(vector<int>& houses,vector<vector<int>>& cost,int start,int prev,int k,vector<vector<vector<long>>>& dp){
        if(k<0) return INT_MAX;
        if(start>=houses.size()) return k==0?0:INT_MAX;//k>0 //k>0
        if(dp[start][k][prev]>=0) return dp[start][k][prev];
        
        int m=cost.size(),n=cost[0].size(); //m: number of houses, n: number of colors
        long ans=INT_MAX;
        if(houses[start]>=0){ //painted
            return dp[start][k][prev]=helper(houses,cost,start+1,houses[start],k-(houses[start]!=prev),dp); //
        }
        else{ //not painted
            for(int c=0;c<n;c++){ //paint house start with color c
                ans=min(ans,cost[start][c]+helper(houses,cost,start+1,c,k-(c!=prev),dp));
            }
        }
        return dp[start][k][prev]=ans;
    }
```	
	


