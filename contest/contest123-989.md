## contest 123

989. Add to Array-Form of Integer.md

### Problem Summary
For a non-negative integer X, the array-form of X is an array of its digits in left to right order.  For example, if X = 1231, then the array form is [1,2,3,1].

Given the array-form A of a non-negative integer X, return the array-form of the integer X+K.

### Solution
Easy
convert both x and k to array form and then add from right to left

```cpp
    vector<int> addToArrayForm(vector<int>& A, int K) {
        int cf=0;
        string s=to_string(K);
        int n=A.size(),m=s.size();
        vector<int> ans;
        for(int i=n-1,j=m-1;i>=0||j>=0;i--,j--)
        {
            int t=(i>=0?A[i]:0)+cf+((j>=0)?s[j]-'0':0);
            ans.push_back(t%10);
            cf=t/10;
        }
        
        if(cf) ans.push_back(cf);
        reverse(ans.begin(),ans.end());
        return ans;
    }
```

Comments
- right to left and then reverse the result
- final may add one digit

990. Satisfiability of Equality Equations.md

### Problem Summary
Given an array equations of strings that represent relationships between variables, each string equations[i] has length 4 and takes one of two different forms: "a==b" or "a!=b".  Here, a and b are lowercase letters (not necessarily different) that represent one-letter variable names.

Return true if and only if it is possible to assign integers to variable names so as to satisfy all the given equations.

 ### Approach
 first collect all equals and connect them
 and then using the non-equals to verify
 disjoint set
 
 ### code
 ```cpp
     bool equationsPossible(vector<string>& equations) {
       //using disjoint set
        vector<int> parent(26);
        for(int i=0;i<26;i++) parent[i]=i;
        for(int i=0;i<equations.size();i++)
        {
            if(equations[i][1]=='=')
            {
                int t0=equations[i][0]-'a',t1=equations[i][3]-'a';
                merge(t0,t1,parent);
            }
        }
        for(int i=0;i<equations.size();i++)
        {
            if(equations[i][1]=='!')
            {
                int t0=equations[i][0]-'a',t1=equations[i][3]-'a';
                if(get_parent(t0,parent)==get_parent(t1,parent)) return 0;
            }
        }
        return 1;
    }
    void merge(int i,int j,vector<int>& parent)
    {
        int pi=get_parent(i,parent);
        int pj=get_parent(j,parent);
        if(pi==pj) return;
        if(pi<pj) {parent[pj]=pi;}
        else {parent[pi]=pj;}
    }
    int get_parent(int i,vector<int>& parent)
    {
        while(parent[i]!=i) i=parent[i];
        return i;
    }
```


991. Broken Calculator.md

### Problem Summary
On a broken calculator that has a number showing on its display, we can perform two operations:

Double: Multiply the number on the display by 2, or;
Decrement: Subtract 1 from the number on the display.
Initially, the calculator is displaying the number X.

Return the minimum number of operations needed to display the number Y.

### Analysis
This is a greedy choice.
If Y is odd, the last step is dec 1
If Y is even, the last step is x2
when y<x only dec is allowed

Decrease the problem Y to X, which is a typical greedy problem.

```cpp
    int brokenCalc(int X, int Y) {
        //every step we can choose -1 or *2
        //greedy solution: 
        //if Y is even, divide by 2, if Y is odd add 1
        int res = 0;
        while (Y > X) {
            Y = Y % 2 > 0 ? Y + 1 : Y / 2;
            res++;
        }
        return res + X - Y;        
    }
```



992. Subarrays with K Different Integers.md

### Problem Summary
Given an array A of positive integers, call a (contiguous, not necessarily distinct) subarray of A good if the number of different integers in that subarray is exactly K.

(For example, [1,2,3,1,2] has 3 different integers: 1, 2, and 3.)

Return the number of good subarrays of A.

### Brutal force
O(N^2), and this will TLE.
we use two pointer i and j to defines a region.
```cpp
    int subarraysWithKDistinct(vector<int>& A, int K) {
        int ans=0;
        for(int i=0;i<A.size();i++)
        {
            unordered_map<int,int> mp;
            for(int j=i;j<A.size();j++)
            {
                mp[A[j]]++;
                if(mp.size()==K) ans++;
                if(mp.size()>K) break;
            }
        }
        return ans;
    }
```

O(N) solution.
Above solution tries every regions. The key observations:
when we had a region with k unique elements inside
- each time we add a new element into the group, it has two cases:
  - it does not increase the unique element, this case it will add multiple regions (ending with the new element)
  - it increase the unique element by 1 and we shall erase the region head element from the group
  
we define three pointers
i: the leftmost index for the k unique element group
j: the left index for the smallest k unique element group
k: the ending index of the group

when adding the new element:
if it does not increase element, add j-i+1 regions, need update j and the map
if it increase 1, then j and i shall be all set to j+1, update i and j and the map

```cpp
    int subarraysWithKDistinct(vector<int>& A, int K) {
        int ans=0;
        int i=0,j=0,k=0;//use 3 pointers, i, j for the two left i<j
        unordered_map<int,int> mp;
        while(k<A.size())
        {
            mp[A[k]]++;
            if(mp.size()>K) {i=j+1;mp.erase(A[j]);}
            if(j<i) j=i;
            if(mp.size()==K) 
            {
                while(mp[A[j]]>1) {mp[A[j]]--;if(mp[A[j]]==0) mp.erase(A[j]);j++;}
                ans+=j-i+1;
            }
            k++;
        }
        return ans;
    }
```
 
 
