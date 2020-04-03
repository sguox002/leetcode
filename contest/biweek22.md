## biweek 22

### 1385. Find the Distance Value Between Two Arrays
<em>
Given two integer arrays arr1 and arr2, and the integer d, return the distance value between the two arrays.

The distance value is defined as the number of elements arr1[i] such that there is not any element arr2[j] where |arr1[i]-arr2[j]| <= d.
</em>

brutal force O(N^2)
```cpp
    int findTheDistanceValue(vector<int>& arr1, vector<int>& arr2, int d) {
        int ans=0;
        for(int i: arr1){
            bool valid=1;
            for(int j: arr2){
                if(abs(i-j)<=d) {valid=0;break;}
            }
            ans+=valid;
        }
        return ans;
    }
```
O(nlogn) sort the array first. and then search for a[i]+d and a[i]-d

### 1386. Cinema Seat Allocation
<em>
A cinema has n rows of seats, numbered from 1 to n and there are ten seats in each row, labelled from 1 to 10 as shown in the figure above.

Given the array reservedSeats containing the numbers of seats already reserved, for example, reservedSeats[i]=[3,8] means the seat located in row 3 and labelled with 8 is already reserved. 

Return the maximum number of four-person families you can allocate on the cinema seats. A four-person family occupies fours seats in one row, that are next to each other. Seats across an aisle (such as [3,3] and [3,4]) are not considered to be next to each other, however, It is permissible for the four-person family to be separated by an aisle, but in that case, exactly two people have to sit on each side of the aisle.
</em>

Approach: bit manipulation: 
- note we can process row by row.
- need to have as many as possible families. so we have 0,1,2 possible.
```cpp
int maxNumberOfFamilies(int n, vector<vector<int>>& reservedSeats)
{
    int ans = n*2;
    
    unordered_map<int, char> m;
    
    for (auto r : reservedSeats)
        if (r[1] > 1 && r[1] < 10)
            m[r[0]] |= 1<<(r[1]-2);
    
    for (auto seats : m)
    {  
        bool p1 = !(seats.second & 0b11110000);
        bool p2 = !(seats.second & 0b00111100);
        bool p3 = !(seats.second & 0b00001111);
        
        if (p1 && p3)
            continue;
        else if (p1 || p2 || p3)
            ans-=1;
        else
            ans-=2;
    }
    
    return ans;
}
```

### 1387. Sort Integers by The Power Value
<em>
The power of an integer x is defined as the number of steps needed to transform x into 1 using the following steps:

if x is even then x = x / 2
if x is odd then x = 3 * x + 1
For example, the power of x = 3 is 7 because 3 needs 7 steps to become 1 (3 --> 10 --> 5 --> 16 --> 8 --> 4 --> 2 --> 1).

Given three integers lo, hi and k. The task is to sort all integers in the interval [lo, hi] by the power value in ascending order, if two or more integers have the same power value sort them by ascending order.

Return the k-th integer in the range [lo, hi] sorted by the power value.

Notice that for any integer x (lo <= x <= hi) it is guaranteed that x will transform into 1 using these steps and that the power of x is will fit in 32 bit signed integer.
</em>

direct approach using hashmap similar to memoization or dp.
also, if the size is very large, we can use heap to maintain k elements only.

```cpp
    unordered_map<int,int> mp;
    int getKth(int lo, int hi, int k) {
        vector<vector<int>> vp;
        for(int i=lo;i<=hi;i++){
            vp.push_back({get_power(i),i});
        }
        sort(begin(vp),end(vp));
        return vp[k-1][1];
    }
    int get_power(int n){
        if(n==1) return 0;
        if(mp.count(n)) return mp[n];
        if(n%2){
            return mp[n]=1+get_power(3*n+1);
        }
        return mp[n]=1+get_power(n/2);
    }
```	

### 1388. Pizza With 3n Slices
<em>
There is a pizza with 3n slices of varying size, you and your friends will take slices of pizza as follows:

You will pick any pizza slice.
Your friend Alice will pick next slice in anti clockwise direction of your pick. 
Your friend Bob will pick next slice in clockwise direction of your pick.
Repeat until there are no more slices of pizzas.
Sizes of Pizza slices is represented by circular array slices in clockwise direction.

Return the maximum possible sum of slice sizes which you can have.
</em>

This problem is similar to 213. House Robber II
- You cannot pick 0 and n-1 at the same time.
- once you choose i, you cannot choose i-1 and i+1.
- the difference is you have to choose n/3 slices (the constrains), which makes it a 2d dp problem.

```cpp
    int maxSizeSlices(vector<int>& slices) {
        //same as 213. convert to two linear problems
        int n=slices.size();
        return max(dp(slices,0,n-2,n/3),dp(slices,1,n-1,n/3));
    }
    int dp(vector<int>& slices,int l,int r,int k){
        //choose i or not choose i
        int n=slices.size();
        vector<vector<int>> dp0(n+2,vector<int>(k+1));
        //choose i: dp0[i]=dp0[i-2]+slices[i]
        //not choose i: dp0[i]=dp0[i-1]
        for(int i=l;i<=r;i++){ //i is the index
            for(int j=1;j<=k;j++){
                dp0[i+2][j]=max(dp0[i][j-1]+slices[i],dp0[i+1][j]);
            }
        }
        return dp0[r+2][k];
    }
```
	

	
