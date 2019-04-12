medium problems are generally not straighforward and need some thinking and convert to simpler problems.

### 2. Add Two Numbers **
linked list, iterate on both list the same time (number in reversed order)

```cpp
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int cf=0;
        ListNode* root=0,*prev=0;
        while(l1 || l2 || cf)
        {
            int val=(l1?l1->val:0)+(l2?l2->val:0)+cf;
            cf=val/10;
            ListNode* p=new ListNode(val%10);
            if(l1) l1=l1->next;
            if(l2) l2=l2->next;
            if(!prev) root=p;else prev->next=p;
            prev=p;
        }
        return root;
    }
``` 
* keep adding node when l1 or l2 or cf 
*. keep the head, add a prev node.   

### 3. Longest Substring Without Repeating Characters ***
there are 1 or 2, ...chars similar problems, all can be done using a two pointer window
when a char appears already in the set, we advance to the char after it.
```cpp
    int lengthOfLongestSubstring(string s) {
        int i=0,j=0;
        int ans=0;
        unordered_set<int> ms;
        while(j<s.length())
        {
            int c=s[j];
            if(ms.count(c)) 
            {
                while(s[i]!=s[j]) {ms.erase(s[i]);i++;}
                ms.erase(s[i]);i++;
            }
            ms.insert(s[j]);
            ans=max(ans,j-i+1);
            j++;
        }
        return ans;
    }
```

### 5. Longest Palindromic Substring ***
at every position check two options i,i and i,i+1
```cpp
    string longestPalindrome(string s) {
        int ans=1;
        int start_ind=0;
        if(s.empty()) return "";
        for(int i=0;i<s.size()-1;i++)
        {
            check_pal(s,i,i,ans,start_ind);
            check_pal(s,i,i+1,ans,start_ind);
        }
        return s.substr(start_ind,ans);
    }
    
    void check_pal(string& s,int i,int j,int& len,int& ind)
    {
        while(i>=0 && j<s.size() && s[i]==s[j]) i--,j++;
        if(len<j-i-1) {len=j-i-1;ind=i+1;}
    }
```

### 6. ZigZag Conversion **
nothing special, if we notice the period is N+N-2
```cpp
    string convert(string s, int numRows) {
        if(numRows<2) return s;
       vector<string> vs(numRows);
        //0 to n-1, diag(i,i) and then repeat
        //due to the starting and ending, we can use the diag from n-2 to 1
        int i=0,j=0;
        int period=numRows+numRows-2;
        while(i<s.size())
        {
            int tind=i%period;
            if(tind<numRows) vs[tind]+=s[i];
            else
            {
                int t=tind-numRows;
                vs[numRows-2-t]+=s[i];
            }
            i++;
        }
        string zig;
        for(string& ss: vs) zig+=ss;
        return zig;
    }
```

### 8. String to Integer (atoi) **
trivial using stringstream

```cpp
    int myAtoi(string str) {
        stringstream ss(str);
        long long ans=0;
        ss>>ans;
        if(ans>INT_MAX) return INT_MAX;
        if(ans<INT_MIN) return INT_MIN;
        return ans;
    }
```
using long long is necessary to avoid overflow

### 11. Container With Most Water ****
two pointers
shrink the region, then need increase the height
two heights are bound by the smaller one.
```cpp
    int maxArea(vector<int>& height) {
        int i=0,j=height.size()-1;
        int maxarea=0;
        while(i<j)
        {
            int minh=min(height[i],height[j]);
            maxarea=max(maxarea,minh*(j-i));
            while(i<j && height[i]<=minh) i++;
            while(i<j && height[j]<=minh) j--;
            //cout<<i<<" "<<j<<" "<<maxarea<<endl;
        }
        return maxarea;
    }
 ```
 
 
### 12. Integer to Roman ***
it is not easy to use all the rules, but if we write all thousands, hundreds, tens and ones for all combination, this is really simple
sometimes, if the cases are not much, direct approach could be much simpler

```cpp
string intToRoman(int num) {
    string table[4][10] = {{"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"},
                           {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"},
                           {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"},
                           {"", "M", "MM", "MMM"}
                          };
    string result;
    int count = 0;
    while(num > 0){
        int temp = num % 10;
        result = table[count][temp] + result;
        num /= 10;
        count++;
    }
    return result;
}
````

### 15. 3Sum ***
for sorted array only
iterate on one and inside is a two pointer problem. This is classical extension to a 2sum problem.
O(N^2)

```cpp
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<vector<int>> ans;
        int n=nums.size();
        int i,j,k;
        for(i=0;i<n-2;i++)
        {
            if(i && nums[i]==nums[i-1]) continue;
            j=i+1;
            k=n-1;
            //now it is a two pointer problem
            while(j<k)
            {
                int t=nums[i]+nums[j]+nums[k];
                if(t>0) k--;
                else if(t<0) j++;
                else
                {
                    ans.push_back({nums[i],nums[j],nums[k]});
                    while(j<k && nums[j+1]==nums[j]) j++;
                    while(j<k && nums[k-1]==nums[k]) k--;
                    j++,k--;   
                }
            }
        }
        return ans;
    }
``` 
### 16. 3Sum Closest ****
similar to 15

```cpp
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(),nums.end());
        int i,j,k,n=nums.size();
        int mindiff=INT_MAX;
        int ans=0;
        for(i=0;i<n-2;i++)
        {
            j=i+1,k=n-1;
            while(j<k)
            {
                int t=nums[i]+nums[j]+nums[k];
                if(t<target) j++;
                else if(t>target) k--;
                else return target;
                if(mindiff>abs(t-target))
                {
                    mindiff=abs(t-target);
                    ans=t;
                }
            }
        }
        return ans;
    }
 ```
 
 ### 17. Letter Combinations of a Phone Number *****
 typical dfs+backtracking problem
 ```cpp
     vector<string> mp={"abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"} ;
    vector<string> letterCombinations(string digits) {
        vector<string> vs;
        if(digits.empty()) return vs;
        dfs(digits,vs,string(),0);
        return vs;
    }
    void dfs(string& digits,vector<string>& vs,string t,int start)
    {
        if(start==digits.size()) {vs.push_back(t);return;}
        int tind=digits[start]-'2';
        for(int i=0;i<mp[tind].size();i++)
        {
            t+=mp[tind][i];
            dfs(digits,vs,t,start+1);
            t.pop_back();
        }
    }
```

### 19. Remove Nth Node From End of List *****

1. the node could be the head, add a dummy
2. two passes, first pass to determine number of nodes, second pass to get the n-N node
3. one pass: fast pointer and slow pointer, let the fast point advance n nodes first and then slow and fast advance.
first create a dummy node to connect to the head and then process to the node before the selected node, 
delete it using slow->next=slow->next->next

```cpp
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy=new ListNode(0);
        dummy->next=head;
        ListNode *fast=dummy,*slow=dummy;
        for(int i=1;i<=n+1;i++) fast=fast->next;
        while(fast) fast=fast->next,slow=slow->next;
        slow->next=slow->next->next;
        return dummy->next;
    }
```

One pass generally use two pointers to create a condition which makes things easier.
writing a concise code also reflects clear thought and is important

### 22. Generate Parentheses *****
n pairs
dfs+ backtracking
the only constrains is number of ( >= number of ) at any position.
add a ( when cntleft<n
add a ) when cntright+1<=cntleft
```cpp
    vector<string> generateParenthesis(int n) {
        vector<string> vs;
        string ss;
        dfs(vs,ss,0,0,n);
        return vs;
    }
    void dfs(vector<string>& vs,string ss,int i,int j,int n)
    {
        if(i==n && j==n) {vs.push_back(ss);return;}
        if(i<n) {dfs(vs,ss+'(',i+1,j,n);}
        if(i>j) {dfs(vs,ss+')',i,j+1,n);}
    }
```    
backtracking or dfs needs clear thought, otherwise will be very complicated and buggy

### 24. Swap Nodes in Pairs ****
add a dummy
```cpp
    ListNode* swapPairs(ListNode* head) {
        if(!head) return head;
        ListNode* dummy=new ListNode(0);
        ListNode *next,*prev,*curr;
        curr=head;prev=dummy;next=head->next;
        prev->next=curr;
        while(next)
        {
            curr->next=next->next;
            prev->next=next;
            next->next=curr;
            //advance one pair
            prev=curr;
            curr=curr->next;
            if(curr) next=curr->next;
            else next=0;
        }
        return dummy->next;
    }
```    

### 29. Divide Two Integers ****
no * / can be used. Basically we need use +- to implement this.
the quotient could be very large so we need improve efficiency.
first double the divisor until close to n and get the quotient
then subtract the scale * divisor and repeat

```cpp
    int divide(int dividend, int divisor) {
        int s1=dividend>=0?1:-1;
        int s2=divisor>=0?1:-1;
        long long d1=(long long)s1*dividend;
        long long d2=(long long)s2*divisor;
        if(d2>d1) return 0;
        long long quotient=0;
        while(d2<=d1)
        {
            long long scale=get_scale(d1,d2);
            quotient+=scale;
            d1-=scale*d2;
        }
        long long ans=s1*s2*quotient;
        if(ans>INT_MAX || ans<INT_MIN) ans=INT_MAX;
        return ans;
    }
    long long get_scale(long long d1,long long d2)
    {
        long long scale=1;
        while(d2*2<=d1) {d2*=2;scale*=2;}
        return scale;
    }
```
to avoid overflow use long long for all intermediate

### 31. Next Permutation ****
this is a important algorithm implemented in std
from end search the first sorted element
for example 12354, next permutation is 12435
the step: 
find the first sorted position reversely, num[i-1]<num[i]
swap num[i-1] with the first one num[j]>num[i-1] reversely
reverse i to end

```cpp
    void nextPermutation(vector<int>& nums) {
        if(nums.size()<2) return;
        int i=nums.size()-1;
        while(i)
        {
            if(nums[i]>nums[i-1]) break;
            i--;
        }
        if(!i) {reverse(nums.begin(),nums.end());return;}
        //swap i-1 with the smallest >num[i-1]
        int j=nums.size()-1;
        while(nums[j]<=nums[i-1]) j--;
        swap(nums[i-1],nums[j]);
        reverse(nums.begin()+i,nums.end());
    }
```    

### 33. Search in Rotated Sorted Array *****
divide and conquer
first cut by half, the mid will either fall into a sorted segment or not sorted segment
sorted: regular binary search
not sorted: a reduced sub problem

```cpp
    int search(vector<int>& nums, int target) {
        if(nums.size()<1) return -1;
        if(nums.size()==1) return target==nums[0]?0:-1;
        return find(nums,0,nums.size()-1,target);
    }
    int find(vector<int>& nums,int l,int r,int target)
    {
        int mid=(l+r)/2;

        if(nums[mid]==target) return mid;
        if(l>=r) return -1;
        if(nums[mid]>=nums[l]) //[l..mid] is ordered, need >= since it may cover itself
        {
            if(nums[mid]>target) //it could be inside front or after
            {
                if(binary_search(nums.begin()+l,nums.begin()+mid,target))
                    return int(lower_bound(nums.begin()+l,nums.begin()+mid,target)-nums.begin());
            }
            return find(nums,mid+1,r,target);
        }
        else //[mid,r] is in order
        {
            if(nums[mid]<target) //the number is in the front rotated
            {
                if(binary_search(nums.begin()+mid+1,nums.end(),target))
                    return int(lower_bound(nums.begin()+mid+1,nums.end(),target)-nums.begin());
            }
            return find(nums,l,mid-1,target);
        }
        return -1;
    }
```
1. edge case: empty or less than 2 elements'
2. exit condition: found or l>=r failure
3. first decide mid in which segment
- mid in first segment, target<=num[mid], then target could be in this segment [l,mid] it could also in the other segment
- mid in second segment: target>num[mid], then it could be in range [l,r] or the other range


### 34. Find First and Last Position of Element in Sorted Array **
equal-range or lower_bound/upper_bound

### 43. Multiply string
the key is to store single digit product in a vector element
a[i]*b[j] store to res[i+j]
first reverse the input to satisfy the above condition

```cpp
    string multiply(string num1, string num2) {
        //single s[i]*s[j] will contribute to res[i+j] and res[i+j+1]
        //and then accumulate
        reverse(num1.begin(),num1.end());
        reverse(num2.begin(),num2.end());
        vector<int> res(num1.size()+num2.size());
        for(int i=0;i<num1.size();i++)
        {
            for(int j=0;j<num2.size();j++)
            {
                res[i+j]+=(num1[i]-'0')*(num2[j]-'0');
            }
        }
        int cf=0;
        string ans;
        for(int i=0;i<res.size();i++)
        {
            int d=res[i]+cf;
            ans+=(d%10)+'0';
            cf=d/10;
        }
        while(ans.size()>1 && ans.back()=='0') ans.pop_back();
        reverse(ans.begin(),ans.end());
        return ans;
    }
```	
note: need to remove all leading 0 except one 0.


### 48. Rotate Image
rotate 90 degrees
first transpose (row becomes col) and then reverse each row
transpose need to use swap.

```cpp
    void rotate(vector<vector<int>>& matrix) {
        for(int i=0;i<matrix.size();i++)
            for(int j=i+1;j<matrix[0].size();j++) swap(matrix[i][j],matrix[j][i]);
        for(int i=0;i<matrix.size();i++)
            reverse(matrix[i].begin(),matrix[i].end());
    }
```
O(N^2)

### 49. Group Anagrams	
if we sort each word, using hashmap
this shall be easy
```cpp
   vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>> mp;
        for(string s: strs)
        {
            string t=s;
            sort(s.begin(),s.end());
            mp[s].push_back(t);
        }
        vector<vector<string>> ans;
        for(auto t: mp) ans.push_back(t.second);
        return ans;
        
    }
```

### 50. Pow(x, n)	
x [-100, 100]
n: integer.

This first confused me. this is a divide and conquer problem
1. n<0 convert to (1/x)^-n
2. x^n convert to (x*x)^n/2

```cpp
    double myPow(double x, int n) {
        if(n==0) return 1;
        long long nn=n;
        if(n<0) {x=1/x;nn=-nn;}
        return nn%2?x*myPow(x*x,nn/2):myPow(x*x,nn/2);
    }
```

1. int negate will overflow so use long long
2. when we don't know what to do first try divide and conquer 

### 54. Spiral Matrix ****
right, down, left, up, everytime it rotates right 90 degrees
traverse right and increase rowBegin
traverse down and decrease colEnd
traverse left and decrease rowEnd
traverse up increase colStart

Actually this is not easily concluded.
```cpp
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if(matrix.empty()) return vector<int>();
        int m=matrix.size(),n=matrix[0].size();
        int row_start=0,row_end=m-1,col_start=0,col_end=n-1;
        vector<int> ans;
        while(row_start<=row_end && col_start<=col_end)
        {
            //right
            for(int i=col_start;i<=col_end;i++) ans.push_back(matrix[row_start][i]);
            row_start++;
            //down
            for(int i=row_start;i<=row_end;i++) ans.push_back(matrix[i][col_end]);
            col_end--;
            //left
            if(row_start<=row_end)
            {
            for(int i=col_end;i>=col_start;i--) ans.push_back(matrix[row_end][i]);
            row_end--;
            }
            //up
            if(col_start<=col_end)
            {
            for(int i=row_end;i>=row_start;i--) ans.push_back(matrix[i][col_start]);
            col_start++;
            }
        }
        return ans;
    }
```
Inside the while loop we shall guard each path so we won't add already visited nodes
quite condition: along that direction we have no steps to go
similar but better in implementation
```cpp
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if (matrix.empty()) return {};
        int m = matrix.size(), n = matrix[0].size();
        vector<int> spiral(m * n);
        int u = 0, d = m - 1, l = 0, r = n - 1, k = 0;
        while (true) {
            // up
            for (int col = l; col <= r; col++) spiral[k++] = matrix[u][col];
            if (++u > d) break;
            // right
            for (int row = u; row <= d; row++) spiral[k++] = matrix[row][r];
            if (--r < l) break;
            // down
            for (int col = r; col >= l; col--) spiral[k++] = matrix[d][col];
            if (--d < u) break;
            // left
            for (int row = d; row >= u; row--) spiral[k++] = matrix[row][l];
            if (++l > r) break;
        }
        return spiral;
    }
```

	
### 55. Jump Game *****
this is a good dp problem
subproblem: if it can reach ith node
It depends on previous node
first the node has to be reachable dp[j], second, i>j+num[j]

```cpp
    bool canJump(vector<int>& nums) {
        int n=nums.size();
        vector<bool> dp(n);
        dp[0]=1; //the first node is reachable
        for(int i=1;i<n;i++)
        {
            for(int j=i-1;j>=0;j--)
                if(dp[j] && j+nums[j]>=i) {dp[i]=1;break;}
        }
        return dp[n-1];
    }
```
O(N^2)

O(N) solution: we update the largest distance we can reach using current node.

```cpp
bool canJump(int A[], int n) {
    int i = 0;
    for (int reach = 0; i < n && i <= reach; ++i)
        reach = max(i + A[i], reach);
    return i == n;
}
```	
	
### 56. Merge Intervals
the first step is always sort for interval problem
we merge all the intervals if the start<the current ending
```cpp
    vector<Interval> merge(vector<Interval>& intervals) {
        if(intervals.size()==0) return {};
        sort(intervals.begin(),intervals.end(),cmp);
        vector<Interval> ans;
        ans.push_back(intervals[0]);
        for(auto t: intervals)
        {
            if(t.start<=ans.back().end) ans.back().end=max(ans.back().end,t.end);
            else {ans.push_back(t);}
        }
        return ans;
    }
```
to avoid missing the last interval, we either modify the last node in vector or add a new node into vector
note need use <= for overlap

### 59. Spiral Matrix II
similarly

```cpp
    vector<vector<int>> generateMatrix(int n) {
        int cnt=1;
        int l=0,r=n-1,u=0,d=n-1;
        vector<vector<int>> ans(n,vector<int>(n));
        while(cnt<=n*n)
        {
            for(int i=l;i<=r;i++) ans[u][i]=cnt++;
            if(++u>d) break;
            for(int i=u;i<=d;i++) ans[i][r]=cnt++;
            if(--r<l) break;
            for(int i=r;i>=l;i--) ans[d][i]=cnt++;
            if(--d<u) break;
            for(int i=d;i>=u;i--) ans[i][l]=cnt++;
            if(++l>r) break;
        }
        return ans;
    }
```
note in the while shall be cn<=n*n instead of cnt<n*n since we are using cnt++

### 60. Permutation Sequence	
constraints: n is [1,9] k<n! for 9: 9!=362880, which is relatively large
divide and conquer
the permutation
1+(234)
2+(134)
3+(124)
4+(123)

k=(n-1)!*c1+(n-1)!*c2+.....+Cn-1
Once we calculate these coefficient we can easily get the digits
```cpp
    string getPermutation(int n, int k) {
        string ans;
        vector<int> ms(n);
        for(int i=0;i<n;i++) ms[i]=i+1;
        k=k-1;
        while(ms.size())
        {
            int f=factorial(n-1);
            int d=k/f;
            ans+=ms[d]+'0';
            ms.erase(ms.begin()+d);
            k%=f;n--;
        }
        return ans;
    }
    int factorial(int n)
    {
        int ans=1;
        for(int i=2;i<=n;i++) ans*=i;
        return ans;
    }
```
note k shall minus 1 to get the correct order.

### 61. Rotate List	
rotate by k nodes to right
1. k shall be mod by the number of nodes
2. first pass to get the number of nodes, second pass do the rotation

add a dummy node to point to the head
add a tail node to point to the end

first traverse to the end to get the tail
connect the tail to the head
then traverse n-k steps to get the new head

```cpp
    ListNode* rotateRight(ListNode* head, int k) {
        if(!head) return 0;
        ListNode* dummy=new ListNode(0);
        ListNode *tail=head;
        dummy->next=head;
        int n=1;
        while(tail->next) {tail=tail->next;n++;}
        k%=n;
        if(k==0) return dummy->next;
        tail->next=head; //connect to the head

        for(int i=0;i<n-k;i++) tail=tail->next;
        dummy->next=tail->next;
        tail->next=0;
        
        return dummy->next;
    }
```
need pay attention to some details otherwise it is hard to get it correct


### 62. Unique Paths
dp[i,j]=dp[i-1,j]+dp[i,j-1]
dp[i,j]: unique paths arriving at i,j

the key is the boundary, we add an extra row and column set either dp[0][1] or dp[1][0]=1

```cpp
    int uniquePaths(int m, int n) {
        //dp[i,j]=dp[i-1,j]+dp[i,j-1]
        vector<vector<int>> dp(m+1,vector<int>(n+1));
        dp[0][1]=1;
        for(int i=1;i<=m;i++)
            for(int j=1;j<=n;j++) dp[i][j]=dp[i-1][j]+dp[i][j-1];
        return dp[m][n];
    }
```

### 63. Unique Paths II
dp
the path number is a Fib sequence and grows very quickly
m,n<=100

to avoid integer overflow try using long
```cpp
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        //dp[i,j]=dp[i-1,j]+dp[i,j-1]
        int m=obstacleGrid.size();
        if(m==0) return 0;
        int n=obstacleGrid[0].size();
        vector<vector<long>> dp(m+1,vector<long>(n+1));
        dp[0][1]=1;
        for(int i=1;i<=m;i++)
            for(int j=1;j<=n;j++) 
                if(!obstacleGrid[i-1][j-1]) dp[i][j]=dp[i-1][j]+dp[i][j-1];
        return dp[m][n];        
    }
```

### 64. Minimum Path Sum
dp:
dp[i,j]=min(dp[i-1,j],dp[i,j-1])+grid[i,j]
```cpp
    int minPathSum(vector<vector<int>>& grid) {
       if(grid.size()==0) return 0;
        int m=grid.size(),n=grid[0].size();
        vector<vector<int>> dp(m+1,vector<int>(n+1));
        for(int i=1;i<=m;i++)
            for(int j=1;j<=n;j++)
                dp[i][j]=min(dp[i-1][j],dp[i][j-1])+grid[i-1][j-1];
        return dp[m][n];
    }
```
why above is incorrect? since the boundary is incorrect. 
if we goes the 0th row, dp is the prefix sum, but using above it is the element itself.
except we set the outer layer to be a very large value (which is a common sense for min)
we initialize it to be int-max but we need reset dp[0][1] to be 0.
```cpp
    int minPathSum(vector<vector<int>>& grid) {
       if(grid.size()==0) return 0;
        int m=grid.size(),n=grid[0].size();
        vector<vector<int>> dp(m+1,vector<int>(n+1,INT_MAX));
        dp[0][1]=0;
        for(int i=1;i<=m;i++)
            for(int j=1;j<=n;j++)
                dp[i][j]=min(dp[i-1][j],dp[i][j-1])+grid[i-1][j-1];
        return dp[m][n];
    }
```	

### 71. Simplify Path
constraints:
1. must start with /
2. only need one single / any time
3. . and .. need to be removed
stack to save each level's directory name, but do not store the /
storing the slash makes the things complicated.

using stringstream to read with the / as the delim (so the / acts similarly as space)
```cpp
    string simplifyPath(string path) {
        stack<string> st;
        stringstream ss(path);
        string w;
        while(getline(ss,w,'/'))
        {
            if(w.empty()) continue;
            else if(w==".") continue;
            else if(w=="..") {if(st.size()) st.pop();}
            else st.push(w);
        }
        string ans;
        while(st.size()) {ans="/"+st.top()+ans;st.pop();}
        return ans.empty()?"/":ans;
    }
```

note .. if stack empty, ignore, otherwise pop, the two condition cannot be combined.

	
### 73. Set Matrix Zeroes
if an element is 0,set the row and col to be zero
using a hashmap to record 0's col and row
straightforward

```cpp
    void setZeroes(vector<vector<int>>& matrix) {
        unordered_set<int> row,col;
        for(int i=0;i<matrix.size();i++)
            for(int j=0;j<matrix[0].size();j++)
                if(!matrix[i][j]) row.insert(i),col.insert(j);
        for(int r: row) 
            for(int i=0;i<matrix[0].size();i++) matrix[r][i]=0;
        for(int c: col)
            for(int i=0;i<matrix.size();i++) matrix[i][c]=0;
    }
```

Can we use O(1) space?
generally this requires us to do it row by row or col by col
for example, we use has0 to indicate current row has 0
we iterate from left to right, check previous row if previous row has zero, we zero the element on the same col.
at the end we zeros the previous row

if current element is 0, we need set all previous element at the column to be 0

```cpp
    void setZeroes(vector<vector<int>>& matrix) {
        int curr=0,prev=0;
        int m=matrix.size(),n=matrix[0].size();
        for(int i=0;i<m;i++)
        {
            curr=0;
            for(int j=0;j<n;j++)
            {
                if(!matrix[i][j]) 
                {
                    curr=1;
                    for(int k=i-1;k>=0;k--) matrix[k][j]=0;
                }
                if(i && !matrix[i-1][j]) matrix[i][j]=0;
            }
            if(prev) for(int j=0;j<n;j++) matrix[i-1][j]=0;
            prev=curr;
        }
        if(prev) for(int j=0;j<n;j++) matrix[m-1][j]=0;
    }
```	

### 74. Search a 2D Matrix
row: sorted
first number is row is greater than the last number in previous row.
that means the matrix is sorted in 1d array (just like a c 2d array)
[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]

```cpp
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        //binary search
        if(matrix.size()==0) return 0;
        int m=matrix.size(),n=matrix[0].size();
        int l=0,r=m*n-1;
        while(l<=r)
        {
            int mid=l+(r-l)/2;
            int t=matrix[mid/n][mid%n];
            if(t==target) return 1;
            if(t<target) l=mid+1;
            else r=mid-1;
        }
        return 0;
    }
```

note while(l<=r) we need check l==r case

### 75. Sort Colors
in-place order red, white and blue
two pass: count r/w/b and then output the array

one pass algorithm
using two pointer:
zero: for the 0's element in the beginning
two: for the 2's element in the back

all 1s are in the middle

not so easy to write this:
```cpp
    void sortColors(vector<int>& nums) {
        int i=0,j=nums.size()-1;
        for(int k=0;k<=j;k++)
        {
            while(nums[k]==2 && k<j) swap(nums[k],nums[j--]);
            while(nums[k]==0 && k>i) swap(nums[k],nums[i++]);
        }
    }
```
The inner side while loop is necessary since we need keep swapping the element at k to its proper position
only advance to next char when previous elements are all set.

### 77. Combinations
Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.
note it needs combination, meaning that permutation does not matter
so the number of combination is C(n,k)
fix 1: choose k-1 from 2 to n
fix 2: choose k-2 from 3 to n
...
backtracking
iterate i from 1 to n
 iterate j=i+1 to n
   push the element
   solve k-1, i+1 problem
   pop the element

exit condition: k=0, we reach an answer, j>n we are out
```cpp
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ans;
        dfs(n,k,1,{},ans);
        return ans;
    }
    void dfs(int n,int k,int start,vector<int> t,vector<vector<int>>& ans)
    {
        if(k==0) {ans.push_back(t);return;}
        if(start>n) return; //need to be > since start from 1
        for(int i=start;i<=n;i++)
        {
            t.push_back(i);
            dfs(n,k-1,i+1,t,ans);
            t.pop_back();
        }
    }
```	
question: why the ranking is so low?
passing vector as value increase a lot of time. try as reference.
also largely decreases the memory requirement.

### 78. Subsets
subset is: each element has two choices: choose or not choose 2^n
for 2^n case we have multiple approaches
1. bit manipulations, from 0 to ffff
2. iterative,
[], 2^0
add 1: [],[1], 2^1
add 2: [],[1],[2],[1,2] 2^2
based on previous
3. backtrack:
the only difference: there is no exit condition, we can add the vector as result
```cpp
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> subs;
        vector<int> sub;
        subsets(nums, 0, sub, subs);
        return subs;
    }
    void subsets(vector<int>& nums, int i, vector<int>& sub, vector<vector<int>>& subs) {
        subs.push_back(sub);
        for (int j = i; j < nums.size(); j++) {
            sub.push_back(nums[j]);
            subsets(nums, j + 1, sub, subs);
            sub.pop_back();
        }
    }
```	
the intermediate vector can pass by reference or value. reference could be faster

### 79. Word Search *****

1. since we have multiple starting position, we can iterate on each element (if first one does not match it return immediately)
2. at each position do a dfs
dfs:
first if out of boundary or the char in the grid does not match the char in the string, return immediately
second, it should indicate success when we reach the string end
remember we need always have success or failure two exit.
if it matches, first mark it as visited by changing it to '*'
then search its 4 neigborings. Only one fail then try the other
after 4 directions dones, restore the last grid.

```cpp
    bool exist(vector<vector<char>>& board, string word) {
        int m=board.size();
        if(m==0) return 0;
        int n=board[0].size();
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(dfs(board,i,j,word,0)) return 1;
            }
        }
        return 0;
    }
    bool dfs(vector<vector<char>>& board,int i0,int j0,string word,int ind)
    {
        int m=board.size(),n=board[0].size();
        if(i0<0 || j0<0 || i0>=m || j0>=n || word[ind]!=board[i0][j0]) return 0;
        if(ind==word.size()-1) return 1;
        char c=board[i0][j0];
        board[i0][j0]='*';
        bool res=0;
        res=dfs(board,i0-1,j0,word,ind+1);
        if(!res) res=dfs(board,i0+1,j0,word,ind+1);
        if(!res) res=dfs(board,i0,j0-1,word,ind+1);
        if(!res) res=dfs(board,i0,j0+1,word,ind+1);
        board[i0][j0]=c;
        return res;
    }
```
one important thing to note: the backtracking will restore all its nodes when function returns. so next time we can still use it.

### 80. Remove Duplicates from Sorted Array II
duplicates appears at most twice, do it in-place
it is no easy to write the following simple code
```cpp    int removeDuplicates(vector<int>& nums) {
        if(nums.size()==0) return 0;
        int i=0,j=1;
        int cnt=1;
        while(j<nums.size())
        {
            if(nums[j]!=nums[j-1]) {nums[++i]=nums[j];cnt=1;}
            else
            {
                cnt++;
                if(cnt<3) nums[++i]=nums[j];
            }
            j++;
        }
        return i+1;
    }
```
1. ++i instead of i++, if not the first element is replaced
2. num[j] compare with previous char instead with i%period

this is even better
```cpp
int removeDuplicates(vector<int>& nums) {
    int i = 0;
    for (int n : nums)
        if (i < 2 || n > nums[i-2])
            nums[i++] = n;
    return i;
}
```

### 81. Search in Rotated Sorted Array II
the array allows duplicates

	
### 82. Remove Duplicates from Sorted List II
remove all nodes with same value
so for a node we need first peek its next to see if current node is duplicate
1. if there is no prev set, check next
2. if next is same with curr, it is duplicate, need to remove
3. if prev value is set, just compare curr with the value

### 91. Decode Ways
*. 0 can only combine with previous digit
*. combined must be in [1 26]
dp[i]: number of decoding ways for S[0..i]
dp[i]=Adp[i-1]+Bdp[i-2]
dp[i-1]: current digit itself as a decoding ways
dp[i-2]: current combined with previous as a decoding ways

```cpp
    int numDecodings(string s) {
        int n=s.size();
        if(n==0) return 0;
        vector<int> dp(n+1);
        dp[0]=1;
        dp[1]=s[0]!='0';
        for(int i=2;i<=n;i++)
        {
            if(s[i-1]!='0') dp[i]=dp[i-1];
            if(s[i-2]!='0' && (s[i-2]-'0')*10+s[i-1]-'0'<=26)
                dp[i]+=dp[i-2];
        }
        return dp[n];
    }
```
it is interesting:
1. empty string needs return 0
2. dp[0]=1. otherwise we cannot get correct result.
3. dp[1]=s[0]!='0'	

### 92. Reverse Linked List II
reverse m to n
```cpp
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        ListNode* dummy=new ListNode(0);
        dummy->next=head;
        ListNode* prev=dummy;
        int num_node=n-m+1;
        //stop in front of the node to be reversed m-1
        while(m-->1) {prev=head;head=head->next;}
        prev->next=reversen(head,num_node);
        return dummy->next;
    }
    ListNode* reversen(ListNode* head,int n)
    {
        if(n==0) return head;
        ListNode* prev=0;
        ListNode* next;
        ListNode* last=head;
        while(n--)
        {
            //cout<<n<<" "<<head->val<<endl;
            next=head->next;
            head->next=prev;
            prev=head;
            head=next;
        }
        last->next=next;
        return prev;
    }
```
some details
1. move to in front of the node to be reversed (m n start from 1)
2. reverse n: need keep first node and last node
3. dummy as the previous since the head may change

### 94. Binary tree inorder traversal
recursive is trivial
iterative just simulate the recursive approach using stack
iterative approach is needed for scaling purpose or stack overflow.

### 98. Validate BST
on the fly compare: inorder traversal and current node > prev
we have to set the initial prev to a value out of int range.
```cpp
    long prev=LONG_MIN;
    bool isValidBST(TreeNode* root) {
        if(!root) return 1;
        bool res=isValidBST(root->left);
        if(prev!=LONG_MIN) 
            res=res&&root->val>prev;
        prev=root->val;
        res=res&&isValidBST(root->right);
        return res;
    }
```	
to avoid using the long_min we can use prev as a node pointer which could be better to avoid this kind of bugs.

### 102. Binary Tree Level Order Traversal
trivial using queue
```cpp
    vector<vector<int>> levelOrder(TreeNode* root) {
        if(!root) return {};
        queue<TreeNode*> q;
        q.push(root);
        vector<vector<int>> ans;
        while(!q.empty())
        {
            int sz=q.size();
            ans.push_back({});
            for(int i=0;i<sz;i++)
            {
                TreeNode* p=q.front();q.pop();
                if(p->left) q.push(p->left);
                if(p->right) q.push(p->right);
                ans.back().push_back(p->val);
            }
        }
        return ans;
    }
```
can also use preorder traversal (preorder visit the node first) with a depth as the index

### 103. Binary Tree Zigzag Level Order Traversal
use 102 and then reverse every one row. or do the inverse on the fly since we already know the number of nodes in each layer

### 120. Triangle
min path sum
can do in-place if we do it from base to top. this is a dp.
```cpp
    int minimumTotal(vector<vector<int>>& triangle) {
        int n=triangle.size();
        for(int i=n-2;i>=0;i--)
        {
            for(int j=0;j<=i;j++) 
                triangle[i][j]+=min(triangle[i+1][j],triangle[i+1][j+1]);
        }
        return triangle[0][0];
    }
```	
### 113. Path Sum II
find all paths from root to leaf sum to target
similarly to 103, using preorder backtracking.
```cpp
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        vector<vector<int>> ans;
        preorder(root,sum,{},ans);
        return ans;
    }
    void preorder(TreeNode* root,int target,vector<int> vt,vector<vector<int>>& ans)
    {
        if(!root) return;
        vt.push_back(root->val);
        if(!root->left && !root->right && target==root->val) 
            ans.push_back(vt);
        preorder(root->left,target-root->val,vt,ans);
        //vt.pop_back();
        preorder(root->right,target-root->val,vt,ans);
        vt.pop_back();
    }
```
note: 
* we do not need vt.pop_back() at each recursive call, just pop the root.
* reference passing can improve the efficiency both time and storage
time 40ms->16ms
storage 39M->20M



### 129. Sum Root to Leaf Numbers
use preorder traversal
```cpp
    int sumNumbers(TreeNode* root) {
        int sum=0;
        preorder(root,0,sum);
        return sum;
    }
    void preorder(TreeNode* root,int num,int& sum)
    {
        if(!root) return;
        num=num*10+root->val;
        if(!root->left && !root->right) {sum+=num;}
        preorder(root->left,num,sum);
        preorder(root->right,num,sum);
    }
```
similar questions: find the root to leaf path, binary representation....
this is a dfs of the tree from left to right.

### 130. Surrounded Regions	
dfs, union-find problem
exclude all boundary cells
dfs: start from all boundary O cells and change to *
then all other change to x, * change to O

```cpp
void solve(vector<vector<char>>& board)
{
	if(board.size()==0) return;
	int m=board.size(),n=board[0].size();
	for(int i=0;i<m;i++) 
	{
		if(board[i][0]=='O') dfs(board,i,0);
		if(board[i][n-1]=='O') dfs(board,i,n-1);
	}
	for(int i=1;i<n-1;i++)
	{
		if(board[0][i]=='O') dfs(board,0,i);
		if(board[m-1][i]=='O') dfs(board,m-1,i);
	}
	for(int i=0;i<m;i++)
		for(int j=0;j<n;j++)
			board[i][j]=board[i][j]=='*'?'O':'X';
}
void dfs(vector<vector<char>>& board,int i,int j)
{
	if(i<0 || j<0 || i>=board.size() || j>=board[0].size() || board[i][j]!='O') return;
	board[i][j]='*';
	dfs(board,i-1,j);
	dfs(board,i+1,j);
	dfs(board,i,j-1);
	dfs(board,i,j+1);
}

```

### 137. Single Number II
Given a non-empty array of integers, every element appears three times except for one, which appears exactly once. Find that single one.
need O(n) time and O(1) storage
x^x=0
x^x^x=x
the unique one appears only once

### 151. Reverse words in a string
```cpp
    string reverseWords(string s) {
       string ans,w;
        
        for(char c: s)
        {
            if(c==' ')
            {
                if(w.size())
                {
                    if(ans.empty()) ans=w;
                    else ans=w+" "+ans;
                }
                w.clear();
            }
            else w+=c;
        }
        if(w.size()) if(ans.empty()) ans=w;else ans=w+" "+ans;
        return ans;
    }
```
solution is straightforward only remember the last word.
	
### 142. Linked List Cycle II
detect if we have a cycle. if yes find the starting point
use a slow fast pointer.
if there is not a cycle, the fast pointer will reach null.
if there is a cycle with C nodes, the starting line contains L nodes
the fast and slow nodes will meet in the cycle at a time t:
the fast pointer traverse 2*t=L+mC+x
the slow pointer traverse t=L+nC+x
so 2L+2nc+2x=L+mc+x-->x=(m-2n)C-L, m and n are non-negative integers
x+L=(m-2n)C means that from x position we walk L steps will meet the pointer starting from head walking L steps
and that is the solution!

```cpp
    ListNode *detectCycle(ListNode *head) {
        if(!head) return 0;
		ListNode *slow=head,*fast=head;
        bool iscycle=0;
		while(fast && fast->next)
		{
			fast=fast->next->next;
			slow=slow->next;
			if(fast==slow) {iscycle=1;break;}
		}
        if(!iscycle) return 0;
		slow=head;
		while(fast!=slow) fast=fast->next,slow=slow->next;
		return slow;
    }
```
	


	
### 165. Compare Version Numbers
the key is to convert the string into a int number each segment
if one string has no more segment, default it to be 0 (so to cover 1.0==1)
using stringstream to read
a small trick: replace the delim with space and use >> to read to avoid getline and stoi and read status check.

note: good() is not set when reaching eof (the last one)
eof(), fail(), bad() 

```cpp
    int compareVersion(string version1, string version2) {
		for(int i=0;i<version1.size();i++) if(version1[i]=='.') version1[i]=' ';
		for(int i=0;i<version2.size();i++) if(version2[i]=='.') version2[i]=' ';
		int v1=0,v2=0;
		stringstream s1(version1),s2(version2);
		while(1)
		{
			v1=0,v2=0;
			s1>>v1;
			s2>>v2;
			if(v1>v2) return 1;
			else if(v1<v2) return -1;
			else
			{
				if(s1.eof() && s2.eof()) return 0;
				continue;
			}
		}
        return 0;
    }
```	
note: ss>>v will not update v if read fail. so we need initialize it explicitly.

### 199. Binary Tree Right Side View
inorder traversal, or preorder, the right node is always the last
using this fact, we store map depth vs element.
this requires a map to vector conversion which needs extra storage.
to use a vector directly we need to visit the right node first. (out of order traversal?)

```cpp
     vector<int> rightSideView(TreeNode *root) {
        vector<int> res;
        recursion(root, 1, res);
        return res;
    }
    
    void recursion(TreeNode *root, int level, vector<int> &res)
    {
        if(root==NULL) return ;
        if(res.size()<level) res.push_back(root->val);
        recursion(root->right, level+1, res);
        recursion(root->left, level+1, res);
    }
```

	



### 238. Product of Array Except Self
two passes: 
one to get the left product before the element
2nd pass to get the right product behind the element
implementation:
use constant space. use the output array for the lprod and rprod
need to use long long for the rprod and lprod

```cpp
    vector<int> productExceptSelf(vector<int>& nums) {
        //left and right product and then times left and right
        int len=nums.size();
        long long left=1,right=1;
        vector<int> res(len,1);
        for(int i=1;i<len;i++)
        {
            left*=nums[i-1];
            res[i]*=left;
        }
        for(int i=len-2;i>=0;i--)
        {
            right*=nums[i+1];
            res[i]*=right;
        }
        return res;
    }
```	

### 567. Permutation in String
direct approach
get the hashmap of the pattern
moving to get the window hashmap
compare each window with the pattern hashmap

for 26 fixed letters, replace hashmap with vector can improve speed

```cpp
    bool checkInclusion(string s1, string s2) {
        //check the window's histogram
        if(s2.size()<s1.size()) return 0;
        vector<int> mp(26),mp1(26);
        for(char c: s1) mp[c-'a']++;
        int wlen=s1.size();
        
        for(int i=0;i<s2.size();i++)
        {
            if(i<wlen) mp1[s2[i]-'a']++;
            else 
            {
                mp1[s2[i-wlen]-'a']--,mp1[s2[i]-'a']++;
            }
            if(mp1==mp) return 1;
        }
        return 0;
    }
```
	


### 833. Find And Replace in String
need first sort the indexes
and then add a segment by segment
1. sort the indexes array with its index.
2. combine indexes array with source and target array, which needs more memory
3. we can also maintain a list of to be replaced and then replace from the right end.

```cpp
    string findReplaceString(string S, vector<int>& indexes, vector<string>& sources, vector<string>& targets) {
       int start=0;
        string ans;
        vector<vector<int>> mp;
        for(int i=0;i<indexes.size();i++) mp.push_back({indexes[i],i});
        sort(mp.begin(),mp.end());
        
        for(auto t: mp)
        {
            //int ind=indexes[i],len=sources[i].size();
            int ind=t[0],len=sources[t[1]].size();
            if(S.substr(ind,len)==sources[t[1]])
            {
                ans+=S.substr(start,ind-start)+targets[t[1]];
                start=ind+len;
            }
        }
        if(start<S.size()) ans+=S.substr(start);
        return ans;
    }
```
	
### 93. Restore IP Addresses
each number shall:
1. no leading zero
2. contains 1 to 3 digits
3. 4 numbers total

dfs to try all combinations
```cpp
    vector<string> restoreIpAddresses(string s) {
        vector<string> ans;
        if(s.size()<4) return ans;
        vector<string> num;
        dfs(s,num,4,0,ans);
        return ans;
    }
    void dfs(string& s,vector<string> ip,int k,int ind,vector<string>& ans)
    {
        if(k==0 && ind==s.size()) 
        {
            string w;
            for(int i=0;i<4;i++) w+=ip[i]+".";
            w.pop_back();
            ans.push_back(w);
            return;
        }
        if(k<=0 || ind>=s.size()) return;
        for(int i=ind;i<ind+3;i++)
        {
            string w=s.substr(ind,i-ind+1);
            //cout<<w<<endl;
            int n=stoi(w);
            if(n>255) continue;
            ip.push_back(w);
            dfs(s,ip,k-1,i+1,ans);
            ip.pop_back();
            if(s[ind]=='0') break;
        }
    }
```
It is very important to think clearly the exit condition: k<=0 || ind>=s.size() either one will terminate it
	
### 96. Unique Binary Search Trees
 number of unique BST store 1 to n
 1 to n can be the root
 when i is the root, 1 to i-1 must be on the left, i+1 to n is on the right, which are two subproblems
 this is a dp problem
 dp[i,n]=dp[i-1]*dp[n-i]
 dp[n]=sum(dp[i-1]*dp[n-i]) i=1 to n
 with dp[0]=1, dp[1]=1;
 the final answer is the sum of all the answers
 
 ```cpp
     int numTrees(int n) {
        vector<int> dp(n+1);
        dp[0]=1;
        dp[1]=1;
        for(int i=2;i<=n;i++)
        {
            for(int j=1;j<=i;j++) //j as the root
                dp[i]+=dp[j-1]*dp[i-j];
        }
        return dp[n];
    }
```	
 
 ### 95. Unique Binary Search Trees II
 dfs+backtracking to get all the trees
 but implentation is not simple
 assuming: left will return a vector of trees and right also
 then the combination is simple
 the recursive function shall return a vector of trees
 ```cpp
     vector<TreeNode*> generateTrees(int n) {
        if(!n) return {};
        vector<TreeNode*> ans=dfs(1,n);
        return ans;
    }
    vector<TreeNode*> dfs(int start,int end)
    {
        //if(start) return {};
        vector<TreeNode*> ans;
        if(start>end) {ans.push_back(NULL);return ans;}
        for(int i=start;i<=end;i++)
        {
            vector<TreeNode*> left=dfs(start,i-1);
            vector<TreeNode*> right=dfs(i+1,end);
                
            for(auto t1: left) //left or right could be empty
                for(auto t2: right)
                {
                    TreeNode* root=new TreeNode(i);
                    root->left=t1;
                    root->right=t2;
                    ans.push_back(root);
                }
        }
        return ans;
    }
```
Note:
1. start and end, since the subproblem is not limited as 1 to n
2. new the root shall be inside the left and right combination
3. The key: we have to add a null tree inside the vector when start>end so empty tree will be able to be included in the combination
which is in previous problem for zero node the number of tree is 1

### 105. Construct Binary Tree from Preorder and Inorder Traversal *****
preorder: root left right
in order: left root right
so we get the root from the first element in the preorder
and find the left and right in the inorder sequence

```cpp
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {

        return helper(preorder,0,preorder.size(),inorder,0,inorder.size());
    
    }

    TreeNode* helper(vector<int>& preorder,int i,int j,vector<int>& inorder,int ii,int jj)
    {
        // tree        8 4 5 3 7 3
        // preorder    8 [4 3 3 7] [5]
        // inorder     [3 3 4 7] 8 [5]

        // 每次从 preorder 头部取一个值 mid，作为树的根节点
        // 检查 mid 在 inorder 中 的位置，则 mid 前面部分将作为 树的左子树，右部分作为树的右子树

        if(i >= j || ii >= j)
            return NULL;

        int mid = preorder[i];
        auto f = find(inorder.begin() + ii,inorder.begin() + jj,mid);

        int dis = f - inorder.begin() - ii;

        TreeNode* root = new TreeNode(mid);
        root -> left = helper(preorder,i + 1,i + 1 + dis,inorder,ii,ii + dis);
        root -> right = helper(preorder,i + 1 + dis,j,inorder,ii + dis + 1,jj);
        return root;
    }
```	
The idea is simple but implementation has some difficulties.

### 106. Construct Binary Tree from Inorder and Postorder Traversal
inorder: left root right
postorder: left right root
so we get the root from the last one of the postorder, and then find the left and right in the inorder
```cpp
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
		int  n=inorder.size();
        return helper(0,n,0,n,inorder,postorder);
    }
	TreeNode* helper(int i,int j,int ii,int jj,vector<int>& in,vector<int>& post)
	{
		if(i>=j || ii>=jj) return 0; //exit condition
		int root=post[jj-1];
		auto t=find(in.begin()+i,in.begin()+j,root)-in.begin(); //find the root in inorder sequence
		int dist=t-i;
		TreeNode* p=new TreeNode(root);
		p->left=helper(i,t,ii,ii+dist,in,post);
		p->right=helper(t+1,j,ii+dist,jj-1,in,post);
		return p;
	}
```
Be sure to calculate the range of left and right correctly. It is just simple math


### 109. Convert Sorted List to Binary Search Tree
convert it to height balanced tree
find the middle and left becomes left branch and right becomes right branch
find middle can use fast/slow scheme

just do it similarly as vector with a head and tail pointer
```cpp
    TreeNode* sortedListToBST(ListNode* head) {
        if(!head) return 0;
        return toBST(head,0);
    }
	TreeNode* toBST(ListNode* head,ListNode* tail)
	{
		ListNode *slow=head,*fast=head;
		if(head==tail) return 0;
		while(fast!=tail && fast->next!=tail){
			slow=slow->next;fast=fast->next->next;}
		TreeNode* root=new TreeNode(slow->val);
		root->left=toBST(head,slow);
		root->right=toBST(slow->next,tail);
		return root;
	}
```

### 114. Flatten Binary Tree to Linked List
in-place
the node is arrange as preorder.
we can do it in preorder or reverse (postorder).
this is like the traverse into vector. similar

preorder, prev->right=curr
```cpp
	TreeNode* prev=0;
    void flatten(TreeNode* root) {
		if(!root) return;
		if(prev) 
        {
            prev->right=root;
		    prev->left=0;
        }
		prev=root;
		flatten(root->left);
		flatten(root->right);
    }
```	
Simply check will find bug:
first root 1, prev now is the root, when visiting the 2nd node, prev's right is destroyed but not yet visited.
how to fix it?
we shall do it in a reversed way (reversed preorder so the right branch is already visited)
```cpp
	TreeNode* prev=0;
    void flatten(TreeNode* root) {
		if(!root) return;
		flatten(root->right);
		flatten(root->left);
		if(prev)
		{
			root->right=prev;
			root->left=0;
		}
		prev=root;
    }

	
### 116. Populating Next Right Pointers in Each Node	
You are given a perfect binary tree where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.
Approach: level traverse and we can get its previous node
another fact: it is a perfect binary tree. we can set its child's next pointer.

```cpp
    Node* connect(Node* root) {
        if(!root) return 0;
		queue<Node*> dq;
		dq.push(root);
		while(dq.size())
		{
			int sz=dq.size();
			Node* prev=0;
			for(int i=0;i<sz;i++)
			{
				Node* t=dq.front();dq.pop();
				if(t->left) dq.push(t->left);
				if(t->right) dq.push(t->right);
				if(prev) prev->next=t;
				prev=t;
			}
		}
		return root;
    }
```	
constant space requirement: (above solution does not consider the fact of perfect binary tree)
the key observation is: 
parent:
parent->left->next=parent->right
parent->right->next=parent->next->left (parent->next shall be valid)

recursive or we can also have iterative version1

```cpp
	void connect(TreeLinkNode *root) {
    if(root == NULL) return;
        
    if(root->left) root->left->next = root->right;
    if(root->right && root->next ) root->right->next = root->next->left;

    connect(root->left);
    connect(root->right);
    }
```
Buggy: shall do right first before left.

### 117. Populating Next Right Pointers in Each Node II
not a full tree.
use the queue can solve it similarly
O(1) space requirement:

observation:
if parent has left and right, left->next=right
if parent has only left or right, we shall search for p->next which has a child
p->left or right->next=p->next....
```cpp
    Node* connect(Node* root) {
        if(!root) return 0;
		//it can have 0,1,2 child. 0 child: root is taken care by parent.
		Node* p=root->next;
		while(p && (!p->left && !p->right)) p=p->next;

		if(root->left && root->right) 
		{
			root->left->next=root->right;//right not processed yet.
			if(p) root->right->next=p->left?p->left:p->right;
		}
		else if(root->left || root->right)
		{
			if(p)
			{
				if(root->left) root->left->next=p->left?p->left:p->right;
				else root->right->next=p->left?p->left:p->right;
			}
		}
		connect(root->right);
		connect(root->left);
		return root;
    }
```
Note: we shall do right before left so right can point to left***

### 127. Word Ladder
dictionary contains a list of words
transform: change one letter
hit->hot->dot->dog->cog
        ->lot->log->cog
shortest transformation: bfs
consideration: possible cycle
how to find the word's neighbors: 
if we try to change each letter to * and compare with dictionary, this needs 26*L times

```cpp
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
		unordered_set<string> dict(wordList.begin(),wordList.end());
        if(dict.count(endWord)==0) return 0;
		unordered_set<string> visited;
		queue<string> q; 
		q.push(beginWord);
		int steps=1;
		while(q.size())
		{
			int sz=q.size();
			for(int i=0;i<sz;i++)
			{
				string s=q.front();q.pop();
				for(int j=0;j<s.size();j++)
				{
					char t=s[j];
					for(char c='a';c<='z';c++)
					{
						//if(c==t) continue;
						s[j]=c;
						if(s==endWord) return steps+1;
						if(dict.count(s) && !visited.count(s)){
							q.push(s);visited.insert(s);}
					}
                    s[j]=t;
				}
			}
			steps++;
		}
		return 0;
    }
```	
1. need consider the edge case first: the target word is not in dictionary
2. do not forget to restore the char.	

### 131. Palindrome Partitioning
dfs backtracking
at each char position we have different length of palindrome substring
```cpp
    vector<vector<string>> partition(string s) {
        vector<vector<int>> adj=build_list(s);
		vector<string> vt;
		vector<vector<string>> ans;
		dfs(s,0,adj,vt,ans);
		return ans;
    }
	
	void dfs(string& s,int ind,vector<vector<int>>& adj,vector<string>& vt,vector<vector<string>>& ans)
	{
		if(ind==s.size()) {ans.push_back(vt);return;}
		for(int i=0;i<adj[ind].size();i++)
		{
			int len=adj[ind][i];
			vt.push_back(s.substr(ind,len));
			dfs(s,ind+len,adj,vt,ans);
			vt.pop_back();
		}
	}
	vector<vector<int>> build_list(string s)
	{
		vector<vector<int>> ans(s.size(),vector<int>(1,1));
		int start=0,len=0;
		for(int i=0;i<s.size()-1;i++)
		{
			helper(s,i,i,start,len);
			//if(len>1) ans[start].push_back(len);
			for(int l=len;l>1;l-=2) ans[start++].push_back(l); 
			helper(s,i,i+1,start,len);
			//if(len>1) ans[start].push_back(len);
			for(int l=len;l>1;l-=2) ans[start++].push_back(l);
		}
		return ans;
	}
	void helper(string& s,int i,int j,int& start,int& len)
	{
		while(i>=0 && j<s.size() && s[i]==s[j]) i--,j++;
		start=i+1;
		len=j-i-1;
	}
```	

problem:
1. does not work for abba, for s[1] it did not find bb since it find abba
2. for a palindrome odd number cabac, it has 1,3,5 (max=5)
for even xybbyx it has 2,4,6
modified: the commented part has bugs.

### 133. Clone Graph
deep copy of the graph
each node contains a list of neighboring nodes
approach: a node is copied, but those pointers are invalidated

```cpp
    Node* cloneGraph(Node* node) {
        
    }
```	





	

	

	

