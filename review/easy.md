### 1. two sum
trivial using hashmap
O(n)

### 7. Reverse integer
using stack is trivial
can do it without data structure.
a regular trick
```cpp
    int reverse(int x) {
        if(!x) return 0;
        long long ans=0;
        while(x)
        {
            ans*=10;
            int d=x%10;
            ans+=d;
            x/=10;
        }
        if(ans>INT_MAX || ans<INT_MIN) return 0;
        return ans;
    }
```
O(logn)

### 9. Palindrome Number
two pointer, trivial


### 13. Roman Integer

we can also from other direction
```cpp
int romanToInt(string s) 
{
    unordered_map<char, int> T = { { 'I' , 1 },
                                   { 'V' , 5 },
                                   { 'X' , 10 },
                                   { 'L' , 50 },
                                   { 'C' , 100 },
                                   { 'D' , 500 },
                                   { 'M' , 1000 } };
                                   
   int sum = T[s.back()];
   for (int i = s.length() - 2; i >= 0; --i) 
   {
       if (T[s[i]] < T[s[i + 1]])
           sum -= T[s[i]];
       else
           sum += T[s[i]];
   }
   return sum;
}
```
1. way to initialize hashmap in c++

2. reverse direction iteration: if current number < previous number then subtract it

### 14. Longest Common Prefix
trivial, 

### 20. Valid Parentheses
using stack and check if stack is empty when done

### 21. Merge Two Sorted Lists
using two pointer, regular practice
```cpp
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* dummy=new ListNode(0);
        ListNode* head=dummy;
        while(l1 && l2)
        {
            if(l1->val<l2->val)
            {
                head->next=l1;
                l1=l1->next;
            }
            else
            {
                head->next=l2;
                l2=l2->next;
            }
            head=head->next;
        }
        if(l1) head->next=l1;
        else head->next=l2;
        return dummy->next;
    }
```

### 26. Remove Duplicates from Sorted Array	
in place algorithm using two pointer

### 27. Remove Element
in place, two pointer
```cpp
    int removeElement(vector<int>& nums, int val) {
        int i=0,j=0;
        while(j<nums.size())
        {
            if(nums[j]!=val) nums[i++]=nums[j];
            j++;
        }
        return i;
    }
```

### 28. Implement strStr()	
brutal force or KMP

### 35. Search Insert Position
sorted array using lower_bound >= the target O(logn)


### 38. count and say
Constraints
N: [1,30]
1->11
2->12
11->21
That means we count the number of the digit
```cpp
    string countAndSay(int n) {
        string ans="1";
        string s="1";
        for(int i=2;i<=n;i++)
        {
            int j=0,k=0;
            char c=s[0];
            ans.clear();
            while(j<s.size())
            {
                while(j<s.size() && s[j]==s[k]) j++;
                ans+=to_string(j-k)+s[k];
                k=j;
            }
            s=ans;
        }
        return ans;
}
```
while in a while loop generally introduce bugs, we can rewrite:

```cpp
            int j=0,k=0;
            while(k<s.size())
            {
                if(s[j]!=s[k]) {t+=to_string(k-j)+s[j];j=k;}
                k++;
            }
            if(j<k) t+=to_string(k-j)+s[j];
```
			
Trap:
1.	While loop need make sure j<s.size() to avoid overflow
2.	k-j is the number of same digit. This is a two pointer approach
3.  don't forget the last section.

### 53. max subarray
Subarray sum
Constraints: 
has negatives
number of elements >=1
Accumulate the sum, when the sum is negative we need restart from 0
Since negative will make the sum smaller

Cases:
Empty
One element
All negative
```cpp
    int maxSubArray(vector<int>& nums) {
        if(nums.size()==0) return 0;
        int maxsum=INT_MIN;
        int maxelem=*max_element(nums.begin(),nums.end());
        if(maxelem<0) return maxelem;
        int tsum=0;
        for(int i=0;i<nums.size();i++)
        {
            if(tsum<0) tsum=0;
            tsum+=nums[i];
            maxsum=max(maxsum,tsum);
        }
        return maxsum;
}
```

Divide and conquer or dp solution
Don’t need consider those edge cases
Dp[i]=A[i]+(dp[i-1]>0?dp[i-1]:0)

Complexity O(n)

### 58. length of last word
```cpp
    int lengthOfLastWord(string s) {
      stringstream ss(s)  ;
        string word;
        while(ss) ss>>word;
        return word.length();
    }
```	
Nothing special

### 66. plus one
Digits in array format
Corner case:
999
[]

    vector<int> plusOne(vector<int>& digits) {
       int cf=1;
        int n=digits.size();
        for(int i=n-1;i>=0;i--)
        {
            int t=digits[i]+cf;
            digits[i]=t%10;
            cf=t/10;
        }
        if(cf) digits.insert(digits.begin(),cf);
        return digits;
    }
```
Digits.size()-1 for empty may get a large number and runtime error

### 67. add binary
Only note we need reverse add
Edge case:
One is empty, n-1 will be invalid
```cpp
    string addBinary(string a, string b) {
        string s;
        int m=a.size(),n=b.size();
        int i=m-1,j=n-1;
        int cf=0;
        while(i>=0 || j>=0)
        {
            int t=(i>=0?a[i]-'0':0)+(j>=0?b[j]-'0':0)+cf;
            s+=t%2+'0';
            cf=t/2;
            i--,j--;
        }
        if(cf) s+='0'+cf;
        reverse(s.begin(),s.end());
        return s;
    }
```

### 69 sqrt(x)
Find i*i<=x and (i+1)^2<x
We can use binary search
Lower boundary is 0
Higher boundary is x

Note: binary search generally has following problem:
1.	cannot exit (infinite loop)
2.	miss the correct result due to incorrect range given
In our case:
L==r: we found the correct answer
L+1==r, we found the i*i<x the closest one

Also x*x may get overflow
Test edge case
9
Large number
1: cannot pass
```cpp
    int mySqrt(int x) {
        if(x<2) return x;
        int l=0,r=x;
        while(l+1<r)
        {
            int mid=(l+r)/2;
            long long t=(long long)mid*mid;
            if(t==x) return mid;
            if(t<x) l=mid;
            else r=mid;
        }
        return l;
	}
```
We can also use / to avoid using long long
why we use l=mid and r=mid?
when mid*mid<x, mid still could be answer, so we cannot skip
mid*mid>x, mid will not be an answer so r=mid-1 (but if we use this, will get wrong results since it will miss correct result)

when we let l=mid, we will fall into infinite loop when r=l+1, mid=(2l+1)/2=l and this keeps forever.
so while(l+1<r) to keep the mid changes

### 70. climbing stairs
Dp
dp[1]=1
dp[2]=2 //2 steps, 2 1 step two method
dp[n]=dp[n-1]+dp[n-2]

edge case: n=0,1,2
```cpp
    int climbStairs(int n) {
        if(!n) return 1;
        if(n==1) return 1;
        if(n==2) return 2;
        vector<int> dp(n);
        dp[0]=1;
        dp[1]=2;
        for(int i=2;i<n;i++)
            dp[i]=dp[i-1]+dp[i-2];
        return dp[n-1];
    }
```

83. Remove Duplicates from Sorted List
- The head will not change so we do not need the dummy
- we can keep the val and found the next different node
-edge case: empty list, one list….
```cpp
    ListNode* deleteDuplicates(ListNode* head) {
        if(!head) return head;
       ListNode *curr,*next;
        curr=head;
        next=curr->next;
        int val=curr->val;
        while(next)
        {
            while(next && next->val==val) next=next->next;
            curr->next=next;
            curr=next;
            if(next) next=next->next;
            if(curr) val=curr->val;
        }
        return head;
   }
```
Traps:
1.	empty list
2.	check if the pointer is null to avoid runtime error

### 88. Merge Sorted Array
Constraints:
Need to merge into nums1 instead create a new array

Approach
Not so straightforward
Use two pointers
When an element in num2 to insert, we append it to nums1, and then swap
Forward is hard
But reverse merging is much easier since the space is reserved
```cpp
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i=m-1,j=n-1,k=m+n-1;
        while(i>=0 && j>=0)
        {
            if(nums1[i]>nums2[j]) {nums1[k--]=nums1[i--];}
            else nums1[k--]=nums2[j--];
        }
        while(j>=0) nums1[k--]=nums2[j--];
	}
```

### 100. Same Tree
```cpp
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(!p && !q) return 1;
        if(!p || !q) return 0;
        return p->val==q->val && isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
    }
```

Both empty, return 1
One is empty, return 0
We shall have two exit condition

### 101. Symmetric Tree
Not so straightforward
Consider a node with left and right child
It is symmetric: left->val==right->val
We need have two subtree compare
Subproblem Left->left vs right->right
Subproblem Left->right vs right->left
```cpp
    bool isSymmetric(TreeNode* root) {
        if(!root) return 1;
        return isSymmetric(root->left,root->right);
    }
    bool isSymmetric(TreeNode* root1,TreeNode* root2)
    {
        if(!root1 && !root2) return 1;
        if(!root1 || !root2) return 0;
        return root1->val==root2->val &&
            isSymmetric(root1->left,root2->right) &&
            isSymmetric(root1->right,root2->left);
    }
```

### 104. Maximum Depth of Binary Tree
```cpp
    int maxDepth(TreeNode* root) {
       //traverse to get the max depth 
        int maxh=0;
        inorder(root,1,maxh);
        return maxh;
    }
    void inorder(TreeNode* root,int h,int& maxh)
    {
        if(!root) return;
        
        inorder(root->left,h+1,maxh);
        maxh=max(h,maxh);
        inorder(root->right,h+1,maxh);
    }
```

### 107. Binary Tree Level Order Traversal II
```cpp   
   vector<vector<int>> levelOrderBottom(TreeNode* root) {
       vector<vector<int>> ans;
        preorder(root,0,ans);
        reverse(ans.begin(),ans.end());
        return ans;
    }
    void preorder(TreeNode* root,int h,vector<vector<int>>& ans)
    {
        if(!root) return;
        if(ans.size()<h+1) ans.push_back({root->val});
        else ans[h].push_back(root->val);
        preorder(root->left,h+1,ans);
        preorder(root->right,h+1,ans);
    }

### 108. Convert Sorted Array to Binary Search Tree
Convert to height balanced tree
Every time we need use the median for the root
```cpp
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return helper(nums,0,nums.size()-1);
    }
    TreeNode* helper(vector<int>& nums,int l,int r)
    {
        if(l>r) return 0;
        int mid=(l+r)/2;
        TreeNode* root=new TreeNode(nums[mid]);
        root->left=helper(nums,l,mid-1);
        root->right=helper(nums,mid+1,r);
        return root;
    }
```

### 110. Balanced Binary Tree
Check if the depth difference is <=1
isBalanced: we need compare left vs right
each subtree we do similar things
so we have two problem:
check the depth of a tree
check if balanced
This is typical and not so straightforward
```cpp
    bool isBalanced(TreeNode* root) {
       if(!root)  return 1;
        return abs(height(root->left)-height(root->right))<=1 &&
            isBalanced(root->left) && isBalanced(root->right);
    }
    int height(TreeNode* root)
    {
        if(!root) return 0;
        return max(height(root->left),height(root->right))+1;
    }
```

### 111. Minimum Depth of Binary Tree
```cpp    
	int minDepth(TreeNode* root) {
        if(!root) return 0;
        int minh=INT_MAX;
        inorder(root,1,minh);
        return minh;
    }
    
    void inorder(TreeNode* root,int h,int& minh)
    {
        //if(!root) return;
        if(root->left) inorder(root->left,h+1,minh);
        if(!root->left && !root->right) minh=min(h,minh);
        if(root->right) inorder(root->right,h+1,minh);
    }
```
//minh only can be evaluated when it is a leaf node.

### 112. Path Sum
Root to leaf node
Check if it has such a path
Edge case: empty tree return false
```cpp
    bool hasPathSum(TreeNode* root, int sum) {
        if(!root) return 0;
        sum-=root->val;
        if(!root->left && !root->right) return sum==0;
        if(root->left && hasPathSum(root->left,sum)) return 1;
        if(root->right && hasPathSum(root->right,sum)) return 1;
        return 0;
    }
```
Using preorder traversal, to visit the node first so all path are visited

### 118. Pascal's Triangle
Straightforward if we use a vector<vector<int>>
```cpp
    vector<vector<int>> generate(int numRows) {
        if(!numRows) return vector<vector<int>>();
        vector<vector<int>> ans(numRows);
        for(int i=0;i<numRows;i++)
        {
            ans[i].push_back(1);
            if(i)
            {
                for(int j=1;j<i;j++) ans[i].push_back(ans[i-1][j]+ans[i-1][j-1]);
                ans[i].push_back(1);
            }
        }
        return ans;
    }
```

### 119. Pascal's Triangle II
Only get the kth row
Constrains: using only O(k) memory
Note we only use previous line memory in previous example
So we can reduce it from 2d to 1d
Since we need use old value we need iterate in reverse order
```cpp
    vector<int> getRow(int rowIndex) {
        vector<int> A(rowIndex+1, 0);
        A[0] = 1;
        for(int i=1; i<rowIndex+1; i++)
            for(int j=i; j>=1; j--)
                A[j] += A[j-1];
        return A;
    }
```


### 121. Best Time to Buy and Sell Stock
At most one transaction
Need get the previous min and current max
Prices[i]-min(prices[j] j from i-1 to 0
```cpp
    int maxProfit(vector<int>& prices) {
        if(prices.size()<2) return 0;
       int min_price=prices[0],max_price=INT_MIN; 
        int ans=0;
        for(int i=1;i<prices.size();i++)
        {
            ans=max(ans,max(prices[i]-min_price,0));
            min_price=min(min_price,prices[i]);
       }
        return ans;
	}
```

### 122. Best Time to Buy and Sell Stock II
Perform any times of transaction
```cpp    
	int maxProfit(vector<int>& prices) {
       //accumulate the gain
        if(prices.size()<2) return 0;
        int ans=0;
        for(int i=1;i<prices.size();i++)
            ans+=max(prices[i]-prices[i-1],0);
        return ans;
    }   
```

### 125. Valid Palindrome
Two pointer, ignore non-alpha numeric chars
```cpp    
	bool isPalindrome(string s) {
        if(s.empty()) return 1;
        int i=0,j=s.size()-1;
        while(i<j)
        {
            while(i<j && !isalnum(s[i])) i++;
            while(i<j && !isalnum(s[j])) j--;
            if(tolower(s[i])!=tolower(s[j])) return 0;
            i++,j--;
        }
        return 1;
    }
```

Trap: 
empty string, i<j while skipping the chars
need to know isalnum function


### 136. Single Number
```cpp    
	int singleNumber(vector<int>& nums) {
        int ans=0;
        for(int n: nums) ans^=n;
        return ans;
    }
```

x^x=0

### 141. Linked List Cycle
Use a fast and a slow. The two pointer must meet somewhere in the cycle
```cpp
    bool hasCycle(ListNode *head) {
        ListNode *fast=head,*slow=head;
        //while(fast && slow)
        while(fast && fast->next)
        {
            fast=fast->next->next;
            slow=slow->next;
            if(fast==slow) return 1;
        }
        return 0;
    }
```

Trap:
Use fast and fast->next not null for the condition. Otherwise will cause runtime error

### 155. Min Stack
A stack and retrieve min in const time
Need another data structure
Push update min
Pop also need update min
So we need also a stack to keep the min values, a global can be used or not needed
```cpp
    stack<int> st,stmin;
    int min0;
    MinStack() {
        min0=INT_MAX;
    }
    
    void push(int x) {
        st.push(x);
        min0=min(min0,x);
        stmin.push(min0);
    }
    
    void pop() {
        if(st.size())
        {
            st.pop();
            stmin.pop();
            if(st.size()) min0=stmin.top();//need update min0
            else min0=INT_MAX;
         }
    }
    
    int top() {
        return st.top();
    }
    
    int getMin() {
        return stmin.top();
    }
```

Traps:
When stack empty we need initialize the min0
Min0 shall be update when push and pop

### 160. Intersection of Two Linked Lists
Assume before the intersection each length is a and b
Common length is c
For list 1: a+c
For list 2: b+c
When the first finished one, wait for the other to finish, then we know the difference of a and b, then we let the first finished one to walk a-b steps and we reach the intersection
Wrong: we shall wait for the longer one to walk first for lena-lenb and then walk the same time.
An even smarter approach: when A reaches the end, switch to B, when B reaches the end, switch to A and then the two will meet together.

### 167. Two Sum II - Input array is sorted
two pointer O(N)

### 168. Excel Sheet Column Title
starting index is 1
```cpp
    string convertToTitle(int n) {
        string s;
        while(n) {s+=(n-1)%26+'A';n=(n-1)/26;}
        reverse(s.begin(),s.end());
        return s;
    }
```
	
### 169. Majority Element
using hashmap to count is trivial O(n) in space and time
sort O(nlogn) and O(1)
there is a voting algorithm which is used for finding the majority element. O(n) and O(1)
wiki:
In its simplest form, the algorithm finds a majority element, if there is one: that is, an element that occurs repeatedly for more than half of the elements of the input. However, if there is no majority, the algorithm will not detect that fact, and will still output one of the elements. A version of the algorithm that makes a second pass through the data can be used to verify that the element found in the first pass really is a majority.

The algorithm will not, in general, find the mode of a sequence (an element that has the most repetitions) unless the number of repetitions is large enough for the mode to be a majority. It is not possible for a streaming algorithm to find the most frequent element in less than linear space, when the number of repetitions can be smal

```cpp
    int majorityElement(vector<int>& nums) {
        int major=nums[0],cnt=0;
        for(int n: nums)
        {
            if(n==major) cnt++;
            else 
            {
                cnt--;
                if(cnt<=0) {major=n;cnt=1;}
            }
        }
        return major;
    }
```
Note: when cnt->0 need reset it to 1

### 229. Majority Element II
similarly we can use voting algorithm
since there could be one or two candidates, we need add two counters. Second pass is required to make sure they satisify
```cpp
vector<int> majorityElement(vector<int>& nums) {
    int cnt1 = 0, cnt2 = 0, a=0, b=1;
    
    for(auto n: nums){
        if (a==n) cnt1++;
        else if (b==n) cnt2++;
        else if (cnt1==0){
            a = n;
            cnt1 = 1;
        }
        else if (cnt2 == 0){
            b = n;
            cnt2 = 1;
        }
        else{
            cnt1--;
            cnt2--;
        }
    }
    
    cnt1 = cnt2 = 0;
    for(auto n: nums){
        if (n==a)   cnt1++;
        else if (n==b)  cnt2++;
    }
    
    vector<int> res;
    if (cnt1 > nums.size()/3)   res.push_back(a);
    if (cnt2 > nums.size()/3)   res.push_back(b);
    return res;
}
```

### 171. Excel Sheet Column Number
base 26
```cpp
    int titleToNumber(string s) {
        int res=0;
        for(int i=0;i<s.size();i++)
        {
            res*=26;
            res+=s[i]-'A'+1;
        }
        return res;
    }
```
	
### 189. Rotate Array, rating 4

approach 1: double the array and return begin()+k for n elements

approach 2: k to end and then followed by 0 to k

approach 3: in-place O(1) memory approach

brutal force: each element's position is known (i+k)%n. we need store the original value to temp

a[0] to a[n-k-1] shift to a[k]-a[n-1]

a[n-k] to a[n-1] move to a[0]-a[k-1]

approach 4:
first reverse first n-k elements

2nd reverse the last k elements

reverse the whole
most easy to implement

```cpp
    void rotate(vector<int>& nums, int k) {
        int n=nums.size();
        k%=n;
        reverse(nums.begin(),nums.begin()+n-k);
        reverse(nums.begin()+n-k,nums.end());
        reverse(nums.begin(),nums.end());
    }
```
traps: need make k<n by k%=n

### 190. Reverse Bits

1. don't misunderstand the question. It ask reverse, not 1 to 0 and 0 to 1

2. it needs the trailing and leading 0s to fill 32 bit

3. reverse the bits by first double and then add. (useful trick)

```cpp    
    uint32_t reverseBits(uint32_t n) {
        uint32_t ans=0;
        for(int i=0;i<32;i++)
        {
            ans*=2;
            int t=n%2;
            n/=2;
            ans+=t;
        }
        return ans;
    }
```

be sure to double first so that the bit31 is not doubled.

### 191. Number of 1 Bits
trivial, loop or bitset

### 198. House Robber
dp[i]=max(dp[i-1],dp[i-2]+a[i])
boundary: 
dp[0]=0
dp[1]=a[1]

```cpp
    int rob(vector<int>& nums) {
       int n=nums.size();
        if(n<1) return 0;
        vector<int> dp(n+1);
        dp[1]=nums[0];
        for(int i=2;i<=n;i++)
            dp[i]=max(dp[i-1],dp[i-2]+nums[i-1]);
        return dp[n];
    }
```
edge case: n==0 will runtime error

### 202. Happy Number
trivial: detect the cycle or 1
```cpp
    bool isHappy(int n) {
        unordered_set<int> ms;
        while(n!=1)
        {
            if(ms.count(n)) return 0;
            ms.insert(n);
            string s=to_string(n);
            n=0;
            for(char c: s) n+=(c-'0')*(c-'0');
        }
        return 1;
    }
```

### 203. Remove Linked List Elements
practice removing a node
add a dummy node to avoid head change
subtle: 
when a node is to be removed, previous no change
else prev=curr, curr=curr->next

```cpp
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummy=new ListNode(0);
        ListNode *prev,*curr;
        dummy->next=head;
        prev=dummy;
        curr=head;
        while(curr)
        {
            if(curr->val==val)
                prev->next=curr->next;
            else prev=curr;
            curr=curr->next;
        }
        return dummy->next;
    }
```

### 204. Count Primes
basic math problem: by marking down all its multiples and remaining are all primes. need extra storage to store the flags
n could be very large, pay attention to overflow problem (even i is int, i*i could overflow)
```cpp
    int countPrimes(int n) {
        //range (2 to n-1) counting all 
        if(n<2) return 0;
        vector<bool> flag(n,1);
        flag[0]=flag[1]=0;
        int ans=0;
        for(int i=2;i<n;i++)
        {
            if(flag[i])
            {
                for(long long j=(long long)i*i;j<n;j+=i) flag[j]=0;
                ans++;
            }
        }
        return ans;
    }
```

### 205. Isomorphic Strings
we just map both to abc order. This is a useful trick to have a common mapping
```cpp
    bool isIsomorphic(string s, string t) {
        char c='a';
        unordered_map<char,char> mp1;
        string s1;
        for(char tt: s){
            if(mp1.count(tt)) s1+=mp1[tt];
            else {s1+=c;mp1[tt]=c++;}
        }
        string s2;
        c='a';
        mp1.clear();
        for(char tt: t){
            if(mp1.count(tt)) s2+=mp1[tt];
            else {s2+=c;mp1[tt]=c++;}
        }
        return s1==s2;
    }
```
### 206 reverse linked list
reverse can be done recursively or iteratively
```cpp
    ListNode* reverseList(ListNode* head) {
        ListNode* tail=head;
        while(tail && tail->next) tail=tail->next;
        while(head && head!=tail)
        {
            ListNode* tmp=head->next;
            head->next=tail->next;
            tail->next=head;
            head=tmp;
        }
        return tail;
    }
```
Much easier, intuitive recursive method and concise code:
Key: reverse remaining linked list headed at head.next first, then operate on head

initial:
1 -> 2 -> 3 -> 4 -> 5

after reverseList(2):
1 -> 2 <- 3 <- 4 <- 5
     |
    null
	
after operate on 1:
null <- 1 <- 2 <- 3 <- 4 <- 5
Code:

```cpp
class Solution {
    public ListNode reverseList(ListNode head) {
        // base case
        if(head == null || head.next == null) return head;
        
        ListNode newHead = reverseList(head.next);
        
        head.next.next = head;
        head.next = null;

        return newHead;
    }
}
```

another approach is more straightforward by using a prev node. and reverse the curr->next to prev. final answer is the prev. One pass.

### 217. Contains Duplicate
naive: i and j, O(N^2)
sort: O(nlogn)
hash: O(n)
```cpp
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> ms;
        for(int n: nums)
        {
            if(ms.count(n)) return 1;
            ms.insert(n);
        }
        return 0;
    }
```
### 219. Contains Duplicate II
index difference shall be <=k
```cpp
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int,int> mp;
        for(int i=0;i<nums.size();i++)
        {
            if(mp.count(nums[i]) && i-mp[nums[i]]<=k) return 1;
            mp[nums[i]]=i;
        }
        return 0;
    }
```

### 225. Implement Stack using Queues
only support push to the back and pop to the front
when we push an element we need pop all front and add back to it

### 226. Invert Binary Tree
invert root's left and right and also its subtree
```cpp
    TreeNode* invertTree(TreeNode* root) {
        if(!root) return 0;
        TreeNode* p=invertTree(root->left);
        root->left=invertTree(root->right);
        root->right=p;
        return root;
    }
```
scalable version: recursive not good for large tree
using stack for iterative
using queue for bfs

### 231. Power of Two
check if it is power of 2
power of 2 in binary is 10000000
x-1 is 0111111
so x&(x-1)==0
```cpp
    bool isPowerOfTwo(int n) {
        if(n<=0) return 0;
        return (n&(n-1))==0;
    }
```
possible bugs: 
& is lower than ==, be sure to add ()
edge case n<=0, =0 will satisfy the condition too

power of 2:
1. num>0
2. num & num-1 ==0

power of 3
- num>0
- keep divide 3 until go to 1

power of 4:
- num>0
- even bit is 1 ( num & num-1==0 and num&0x555555 to make sure it is not 2^n)

### 232. Implement Queue using Stacks
when we push to a stack, it is on the top, 
when we pop we need pop the bottom elements

this needs extra storage: another stack, which means we need two stacks
```cpp
    stack<int> in,out;
    MyQueue() {
        
    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        in.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        int t=peek();
        out.pop();
        return t;
    }
    
    /** Get the front element. */
    int peek() {
        if(out.empty())
        while(in.size())
        {
            out.push(in.top());
            in.pop();
        }
        int t=out.top();
        return t;        
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        return in.empty() && out.empty();
    }
```
Note: possible bug: only when output is empty we can correctly reverse the stack.

### 234. Palindrome Linked List
the approach: use a fast and slow pointer to get the mid point and then compare the first half and second half
edge case:
0 node:
1 node:
2 nodes:
Simplest: reverse the linked list and compare with original
O(1) space: fast and slow and then reverse the 2nd half 
note: if odd number nodes, fast will be on the last node, for even it is null

```cpp
    bool isPalindrome(ListNode* head) {
        if(!head) return 0;
        if(!head->next) return 1;
        if(!head->next->next) return head->val==head->next->val;
        ListNode *fast=head,*slow=head;
        while(fast && fast->next)
        {
            fast=fast->next->next;
            slow=slow->next;
        }
        if(fast) slow=slow->next; //for odd, ignore the mid node
        fast=head;

        slow=reverselist(slow);
        //in case fast and slow is the same node
        //cout<<slow->val<<endl;
        while(slow->next) //this is a bug: shall be while(slow) since a single node will be skipped
        {
            if(fast->val==slow->val)
            {
                fast=fast->next;
                slow=slow->next;
            }
            else return 0;
        }
        return 1;
    }
    ListNode* reverselist(ListNode* head)
    {
        //reverse the link
        ListNode* prev=0;
        while(head)
        {
            ListNode* tmp=head->next;
            head->next=prev;
            prev=head;
            head=tmp;
        }
        return prev;
    }

```
possible problem:
1. empty list is considered to be true
2. reverse list shall return prev instead of head (it is null)
3. reverse list in this is more clear.

### 235. Lowest Common Ancestor of a Binary Search Tree
the node is on left or on right or the root is the ancestor
```cpp
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        //if(!root) return 0;
        //pq relation: left-left, right-right,left-right, right-left
        //if(p==root) return 1;
        //if(q==root) return 2;
        if(root)
        {
        int v=root->val;
        if(p->val>v && q->val>v) return lowestCommonAncestor(root->right,p,q);
        if(p->val<v && q->val<v) return lowestCommonAncestor(root->right,p,q);
        }
        return root;
    }
```    
write in concise way. This is pretty. only 3 cases:
p,q>root
p,q<root
other


### 237. Delete Node in a Linked List
Only access to that node
The approach: replace the value of current node to next node’s value and delete next node (so the node cannot be the tail)
```cpp
    void deleteNode(ListNode* node) {
        node->val=node->next->val;
        node->next=node->next->next;
    }
```

### 242. Valid Anagram
1. same hashmap
2. sort

### 257. Binary Tree Paths
Get all path
Preorder traverse with backtracking
```cpp
    vector<string> binaryTreePaths(TreeNode* root) {
       vector<string> vs;
        string s;
        dfs(root,vs,s);
        return vs;
    }
    void dfs(TreeNode* root,vector<string>& vs,string s)
    {
        if(!root) return;
        if(!root->left && !root->right)
        {
            s+=to_string(root->val);
            vs.push_back(s);
            return;
        }
        s+=to_string(root->val)+"->";
        dfs(root->left,vs,s);
        dfs(root->right,vs,s);
    }
```

### 258. Add Digits
Try O(1) run time without any loop/recursion
The problem, widely known as digit root problem, has a congruence formula:
https://en.wikipedia.org/wiki/Digital_root#Congruence_formula

This is a math problem
A number can be expressed as sum(Ai*10^i)
When add its digits sum(Ai)
We are looking for the digit 
Sum(Ai*10^i)-sum(Ai)=sum(Ai*9999…) and this %9==0
So the two numbers has the same remainder if %9

### 263. Ugly Number
Very trial except input=0

### 268. Missing Number
0 to n, one is missing
Need O(n) time and O(1) memory
This is not so straightforward. 
Approach 1: add 0 to n and minus the array sum
Approach 2: index xor element
```cpp
    int missingNumber(vector<int>& nums) {
        //use 0-n to xor the nums
        int result=0;
        for(int i=0;i<nums.size();i++) result^=nums[i]^(i+1);
        return result;
    }
```

### 278. First Bad Version
```cpp    
	int firstBadVersion(int n) {
        int l=1,r=n;
        while(l<r)
        {
            int mid=l+(r-l)/2;
            if(isBadVersion(mid)) r=mid;
            else l=mid+1;
        }
        return l;
    }
```
	
Trick:
1.	mid=l+(r-l)/2 to avoid overflow
2.	l=mid+1 since we need to find bad version, so good version needs not check again

### 283. Move Zeroes
In-place to move all zeros to array end
Trivial using two pointers

### 290. Word Pattern
From a to b and from b to a must both match!
We can use a third fact:
Char map to integer
String map to integer
```cpp
bool wordPattern(string pattern, string str) {
    map<char, int> p2i;
    map<string, int> w2i;
    istringstream in(str);
    int i = 0, n = pattern.size();
    for (string word; in >> word; ++i) {
        if (i == n || p2i[pattern[i]] != w2i[word])
            return false;
        p2i[pattern[i]] = w2i[word] = i + 1;
    }
    return i == n;
}
```

Traps:
Be sure to deal with diff=0 case
Be sure to deal with null list


### 292. Nim Game
N stones, you can take 1-3 stones each time, determine if I can win
Theorem: The first one who got the number that is multiple of 4 (i.e. n % 4 == 0) will lost, otherwise he/she will win.
Proof:
1.	the base case: when n = 4, as suggested by the hint from the problem, no matter which number that that first player, the second player would always be able to pick the remaining number.
2.	For 1* 4 < n < 2 * 4, (n = 5, 6, 7), the first player can reduce the initial number into 4 accordingly, which will leave the death number 4 to the second player. i.e. The numbers 5, 6, 7 are winning numbers for any player who got it first.
3.	Now to the beginning of the next cycle, n = 8, no matter which number that the first player picks, it would always leave the winning numbers (5, 6, 7) to the second player. Therefore, 8 % 4 == 0, again is a death number.
4.	Following the second case, for numbers between (2*4 = 8) and (3*4=12), which are 9, 10, 11, are winning numbers for the first player again, because the first player can always reduce the number into the death number 8.

### 401. Binary Watch
This is not simple if not thinking right
Appr#1: backtracing
Appr#2: generate all possible values
```cpp
    vector<string> readBinaryWatch(int num) {
        vector<string> ans;
        for(int h=0;h<12;h++)
        {
            for(int m=0;m<60;m++)
            {
                int t=h*64+m;
                bitset<10> bs(t);
                if(bs.count()==num)
                {
                    char str[10];
                    sprintf(str,"%d:%02d",h,m);
                    ans.push_back(string(str));
                }
            }
        }
        return ans;
    }
```

### 344. Reverse String
trivial using two pointer from both end

### 345. reverse vowels
trivial using two pointer
```cpp
    string reverseVowels(string s) {
        if(s.empty()) return s;
       int i=0,j=s.size()-1;
        while(i<j)
        {
            if(isvowel(s[i]) && isvowel(s[j])) swap(s[i++],s[j--]);
            else if(!isvowel(s[i])) i++;
            else j--;
        }
        return s;
    }
    bool isvowel(char c)
    {
        c=tolower(c);
        return c=='a'||c=='e' || c=='i' || c=='o' ||c=='u';
    }
```
Note how to write the while loop:
make exclusive 3 conditions, if else if else to let i++ or j-- or i++,j-- so we do not need to worry about i<j inside the while loop. Often that will introduce bugs.
define clearly when i change and when j changes

### 349. Intersection of Two Arrays
does not allow duplicates
trivial using hash set. Generally approach create one hash from one, and then create the answer on the fly.
```cpp
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
2        unordered_set<int> ms(nums1.begin(),nums1.end());
        unordered_set<int> ans;
        for(int n:nums2)
        {
            if(ms.count(n)) ans.insert(n);
        }
        return vector<int>(ans.begin(),ans.end());
    }
```    

### 350. Intersection of two arrays II
allow duplicates
using hashmap
if arrays are sorted, we can use two pointer to go up.
similarly build the map on one array (+) and decrease the map using other array
```cpp
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int,int> mp;
        for(int n:nums1) mp[n]++;
        vector<int> ans;
        for(int n: nums2)
        {
            if(mp[n]) {ans.push_back(n);mp[n]--;}
        }
        return ans;
    }
```    
### 367. Valid Perfect Square
no sqrt can be used.
Appr# 1
n^2=(n-1)^2+2n-1
so each n^2 is previous (n-1)^2 add an odd number
trap: be careful of integer overflow
Appr# 2: binary search
n^2<num, l=mid+1
n^2>num, r=mid-1
trap: 
easy to fall into infinite loop (when l+1=r case the mid never change)
divide by 0
int overflow.
```cpp
    bool isPerfectSquare(int num) {
        int l=0,r=num;
        while(l<r)
        {
            int mid=l+(r-l)/2;
            long long res=(long long)mid*mid;
            if(res==num) return 1;
            if(res<num) l=mid+1;
            else r=mid-1;
        }
        return l*l==num;
```

### 371. Sum of Two Integers
can not use + -
bit operations: thinking they are binary 
+ -> a^b
carrier flag: a&b

```cpp
int getSum(int a, int b) {
    return b==0? a:getSum(a^b, (a&b)<<1); //be careful about the terminating condition;
}
this will have overflow problem.
or

    int getSum(int a, int b) {
        if (a == 0) return b;
        if (b == 0) return a;

        while (b != 0) {
            int carry = a & (unsigned)b;
            a = a ^ b;
            b = (unsigned)carry << 1;
        }
        return a;
    }
  
```
Be careful bit operations are on unsigned. running time error will occur for negative number

### 374. Guess number higher or lower
binary search
```cpp
    int guessNumber(int n) {
       int l=1,r=n;
        while(l<=r)
        {
            int mid=l+(r-l)/2;
            int t=guess(mid);
            if(t==0) return mid;
            if(t>0) l=mid+1;
            else r=mid-1;
        }
        return 0;
    }
```    

### 383. Ransom Note
trivial using hashmap
```cpp
    bool canConstruct(string ransomNote, string magazine) {
       vector<int> mp(26);
        for(char c: magazine) mp[c-'a']++;
        for(char c: ransomNote) 
        {
            if(mp[c-'a']) mp[c-'a']--;
            else return 0;
        }
        return 1;
    }
```
### 387. First Unique Character in a String
appr1: traverse and count the chars, and then find the first char appear once
appr2: traverse and record first index and count, iterate the map and find the first

### 389. Find the Difference
trivial using xor
iterate on s and t
there is only one difference, so we can set res=t.back() and one loop is done.

### 400. Nth Digit
1 digits 1-9: 9 digits
2 digits 10-99: 90x2
3 digits: 100-999: 900x3
...
n digits: 9*10^(n-1)*n

so the first we can know how many digits
and then we know which number
finally get the nth digit
```java
	public int findNthDigit(int n) {
		int len = 1;
		long count = 9;
		int start = 1;

		while (n > len * count) {
			n -= len * count;
			len += 1;
			count *= 10;
			start *= 10;
		}

		start += (n - 1) / len;
		String s = Integer.toString(start);
		return Character.getNumericValue(s.charAt((n - 1) % len));
	}
```
	


### 404. Sum of Left Leaves
Straightforward: only we need clear the left leaf node
```cpp
    int sumOfLeftLeaves(TreeNode* root) {
        if(!root) return 0;
        int ans=0;
        if(root->left && root->left->left==0 && root->left->right==0) ans+=root->left->val;
        ans+=sumOfLeftLeaves(root->left);
        ans+=sumOfLeftLeaves(root->right);
        return ans;
    }
```

### 405. Convert a Number to Hexadecimal
Key: convert to unsigned
```cpp    
	string toHex(int num) {
        if(!num) return "0";
       unsigned n=num;
        string ans;
        while(n)
        {
            int t=n%16;
            if(t<10) ans+=t+'0';else ans+=t-10+'a';
            n/=16;
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
```

### 409. Longest Palindrome
Only one char can be odd, all other has to be even
Count number of odd char, and subtract n-1
```cpp    
	int longestPalindrome(string s) {
        unordered_map<char,int> mp;
        for(auto c: s) mp[c]++;
        int odd=0;
        int ans=0;
        for(auto t: mp) 
        {
            if(t.second%2) {odd++;}
            ans+=t.second;
        }
        if(odd) ans-=odd-1;
        return ans;
    }
```

### 412. Fizz Buzz
Trivial, no need to say

### 414. Third Maximum Number
O(N) complexity is required
Using set or priority-queue can easily do this. But it is (nlogn)
Basically we need do the three sort ourselves
2,2,3,1
2, m1=2
2, m1=2
3, m1=3, m2=2
1, m1=3,m2=2,m3=1
```cpp
    int thirdMax(vector<int>& nums) {
       long long m1=LLONG_MIN,m2=LLONG_MIN,m3=LLONG_MIN;
        for(int t: nums)
        {
            if(m1<t) {m3=m2;m2=m1;m1=t;}
            else if(t<m1 && m2<t) {m3=m2;m2=t;}
            else if(t<m2 && m3<t) m3=t;
        }
        return m3==LLONG_MIN?m1:m3;
    }
```

Traps: int_min cannot be safe since the input could be int_min
If else if else if has to specify each case clearly to skip equal cases

### 415. Add Strings
Trivial

### 427. Construct Quad Tree
Very hard to understand the problem
Each node has top-left, top-right, bott-left, bott-right 

### 429. N-ary Tree Level Order Traversal
Simple bfs
Edge case: input is null

### 434. Number of Segments in a String
Use stringstream
Or count the char+space for the segment

### 437. Path Sum III
this is not easy at all
1. we can substract the node value until we reach 0, then we add one path
2. we have 3 problem: include the root,(type 2) include the left, include the right,(two subproblem) recursively 
3. for the helper we go down by subtracting the root val (if the root val ==sum then it is also an answer)
two different recursive problem
```cpp
    int pathSum(TreeNode* root, int sum) {
        if(!root) return 0;
        return helper(root,sum)+pathSum(root->left,sum)+pathSum(root->right,sum);
    }
    int helper(TreeNode* root,int sum)
    {
        if(!root) return 0;
        return (root->val==sum?1:0)+helper(root->left,sum-root->val)+
            helper(root->right,sum-root->val);
    }
```

### 438. Find All Anagrams in a String

Using hashmap for a window size
```cpp
    vector<int> findAnagrams(string s, string p) {
       if(p.size()>s.size()) return {};
        unordered_map<char,int> mp;
        for(char c: p) mp[c]++;
        unordered_map<char,int> mp1;
        vector<int> ans;
        for(int i=0;i<=s.size()-p.size();i++)
        {
            if(!i)
            {
                for(int j=0;j<p.size();j++) mp1[s[j]]++;
            }
            else
            {
                mp1[s[i-1]]--;if(mp1[s[i-1]]==0) mp1.erase(s[i-1]);
                mp1[s[i+p.size()-1]]++;
            }
            if(mp1==mp) ans.push_back(i);
        }
        return ans;
    }
```

### 441. arrange coin
Brutal force
Traps: integer overflow
```cpp
    int arrangeCoins(int n) {
        //k(k-1)/2 to k(k+1)/2
        long long tsum=0;
        int i=1;
        while(tsum<=n) {tsum+=i;i++;}
        return i-2;
    }
```

Math: k*(k+1)/2<=n

### 443. string compression
O(1) space
It is not so easy to implement this
There are quite a few details to pay attention to.
1.	reach the end, and not processed
2.	need replace left char
```cpp
    int compress(vector<char>& chars) {
        if(chars.size()<2) return chars.size();
        int i=0,j=1;
        char prev=chars[0];
        int cnt=1;
        while(j<=chars.size())
        {
            if( j==chars.size() || chars[j]!=prev)
            {
                chars[i]=prev;
                if(cnt>=2) 
                {
                    string s=to_string(cnt);
                    for(int k=0;k<s.size();k++) chars[++i]=s[k];
                }
                if(j==chars.size()) return i+1;
                cnt=0;prev=chars[j];i++;
            }
            j++;cnt++;
        }
        return i+1;
    }
```

### 447. Number of Boomerangs
(I,j,k) distance (I,j) = distance (I,k)
Points are on the plane
There are n*(n-1) distances
Brutal force:
For each point calculate its distance to all other points and put in a hashmap and calculate the number of combinations. P(n,2)
```cpp
    int numberOfBoomerangs(vector<pair<int, int>>& points) {
        int n=points.size();
        int ans=0;
        for(int i=0;i<n;i++)
        {
            unordered_map<int,int> mp;
            for(int j=0;j<n;j++)
            {
                if(i==j) continue;
                int dx=points[i].first-points[j].first;
                int dy=points[i].second-points[j].second;
                int dist2=dx*dx+dy*dy;
                mp[dist2]++;
            }
            for(auto t: mp)
            {
                if(t.second>1) ans+=t.second*(t.second-1);
            }
        }
        return ans;
    }
```

### 448. Find All Numbers Disappeared in an Array
O(1) space and O(n) time
eg. [4,3,2,7,8,2,3,1]
4: we change a[3]: 4,3,2,-7,8,2,3,1
3: we change a[2]: 4,3,-2,-7,8,2,3,1
2: we change a[1]: 4,-3,-2,-7,8,2,3,1
7: we change a[6]: 4,-3,-2,-7,8,2,-3,1
8: we change a[7]: 4,-3,-2, -7,8,2,-3,-1
2: we change a[1]: already negative 
3: we change a[2]: already negative
1: we change a[0]: -4,-3,-2,-7,8,2,-3,-1
only the index 4, and 5 are positive, meaning 5 and 6 are not visited.
this type of question: value and index is the same, we can mark visited as negative. If it is positive, then the index is not seen

```cpp
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        for(int i=0;i<nums.size();i++)
        {
            int ind=abs(nums[i])-1;
            if(nums[ind]>0) nums[ind]*=-1;
        }
        vector<int> ans;
        for(int i=0;i<nums.size();i++)
        {
            if(nums[i]>0) ans.push_back(i+1);
        }
        return ans;
    }
```

### 453. Minimum Moves to Equal Array Elements
a move is to increase n-1 element by 1
get the min number of moves

method: keep the max not changed and increase all others
assuming the final is all x, the number of move is x-min
each move we add k-1 into the sum: nx=(n-1)*(x-min)+sum, this is a simple math
x=sum+(n-1)*min
steps=x-min=sum+n*min;
```cpp
    int minMoves(vector<int>& nums) {
        long long sum=accumulate(nums.begin(),nums.end(),0LL);
        long long min0=*min_element(nums.begin(),nums.end());
        int n=nums.size();
        //final is x: k=x-min
        //nx=k*(n-1)+tsum
        return sum-n*min0;
    }
```
traps:
- sum may get overflow
- min will not overflow but min*n will overflow, so use longlong for both.

### 455. Assign Cookie
greedy: always assign the cookie to the first person that s>=g
use two pointers
```cpp
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        int i=0,j=0;
        int ans=0;
        while(i<s.size() && j<g.size())
        {
            if(s[i]>=g[j]) ans++,j++;
            i++;
        }
        return ans;
    }
```    

### 459. Repeated Substring Pattern
if we have AA pattern, then we copy it AAAA, and throw the first and end char, we can find the pattern.
```cpp
    bool repeatedSubstringPattern(string s) {
        string ss=s+s;
        int n=ss.size();
        //check ss.substr(1) contains s
        return ss.substr(1,n-2).find(s)!=string::npos;
    }
```

### 461. Hamming Distance
x and y: number of different bits
appr#1: use bitset
appr#2: x^y will get the different ones to set

### 463. Island Perimeter
not trivial
a grid has 4 sides: left,right,top, bottom
find how many 1 in the map. If without the consideration of surrounding cells, the total perimeter should be the total amount of 1 times 4.
find how many cell walls that connect with both lands. We need to deduct twice of those lines from total perimeter
This is a useful trick: by the iteration we only consider the top and left cell. 
```cpp
int islandPerimeter(vector<vector<int>>& grid) {
        int count=0, repeat=0;
        for(int i=0;i<grid.size();i++)
        {
            for(int j=0; j<grid[i].size();j++)
                {
                    if(grid[i][j]==1)
                    {
                        count ++;
                        if(i!=0 && grid[i-1][j] == 1) repeat++;
                        if(j!=0 && grid[i][j-1] == 1) repeat++;
                    }
                }
        }
        return 4*count-repeat*2;
    }
```

### 475. Heaters
understanding the problem is more important
heater positions are given, we are to find the largest distance of heaters
two ends shall be treated differently. The range is not necessary to be covered.
each house shall be covered by the nearest heater.

```cpp
    int findRadius(vector<int>& houses, vector<int>& heaters) {
        //heater covers min0 to max0 is fine
        sort(houses.begin(),houses.end());
        sort(heaters.begin(),heaters.end());
        int ans=INT_MIN;
        //each house shall be covered by the nearest heater
        for(int h: houses)
        {
            auto it=upper_bound(heaters.begin(),heaters.end(),h);
            if(it==heaters.begin()) ans=max(ans,*it-h);
            else if(it==heaters.end()) ans=max(ans,h-*(it-1));
            else ans=max(ans,min(*it-h,h-*(it-1)));
        }
        return ans;
    }
```
bugs: don't use *--it since --it will always be evaluated first.

### 476. Number Complement
get the highest bit num 1<<bit-1-num 
pay attention to overflow use 1L<<bit

### 482. License Key Formatting
Appr#1
1 remove non alnum char and convert to upper
2. reverse count and insert -

Appr#2
store in a stack and reverse

Appr#3: reverse scan the string and add - in one scan
this is a important hint since the first is to be decided last

### 485. Max Consecutive Ones
pretty simple. reset cnt when see 0 increase cnt when see 1

### 492. Construct the Rectangle
requirement: L*W=area, L>=W and L-W shall be minimized
simple: find the largest factor <=sqrt(n)
traps: 
duplicate number, 
k=0 case, and a lot of duplicates for example [1,1,1,1] k=0
k<0 case
To make it easier, k=0 and k>0 two cases
k=0, we count each number occurance
k>0, we count value+k (not iterate each will include the value-k)
need use a hashmap.

### 496. Next Greater Element I
Using stack and hashmap.
Stack maintains a decreasing sequence
Each element is pushed once and popped
```cpp
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int,int> mp;
        for(int i=0;i<nums1.size();i++) mp[nums1[i]]=i;
        vector<int> ans(nums1.size(),-1);
        stack<int> st;
        for(int i=0;i<nums2.size();i++)
        {
            if(st.size() && nums2[i]>st.top()) 
            {
                while(st.size() && st.top()<nums2[i])
                {
                    int t=st.top();
                    st.pop();
                    if(mp.count(t)) ans[mp[t]]=nums2[i];
                }
            }
            st.push(nums2[i]);
        }
        return ans;
    }
```

### 500. Keyboard Row
Simple using a hashmap char vs group

### 501. Find Mode in Binary Search Tree
Simple, traversal and get the max and put into hashmap

### 504. Base 7
1. keep the sign
2. pay attention to overflow int

### 506. Relative Ranks
Please understand the problem correctly
Given the score, get the rank
Combine with index and sort

### 507. Perfect Number
Up to 1e8
Iterate to up to 1e4 (sqrt)
Edge case: 1 is false since it cannot use itself

### 509. Fibonacci Number
Since it need 0 to 30, F[N+1] cannot cover N=0, runtime error

Even a trivial task will easily have a bug

### 520. Detect Capital
Simple:
First char is lowercase, all need to be lower case
First char is uppercase, remaining has to be all lower case or uppercase
Corner case: empty string
```cpp
    bool detectCapitalUse(string word) {
        if(word.empty()) return 1;
        if(islower(word[0])) //first char is lowercase
        {
            for(auto c: word) if(!islower(c)) return 0;
            return 1;
        }
        else
        {
            //all capital
            int cnt=0;
            for(int i=1;i<word.size();i++)
                if(islower(word[i])) cnt++;else cnt--;
            return abs(cnt)==word.size()-1;
        }
    }
```

### 521. Longest Uncommon Subsequence I
A and B
If A==B there is no common 
If A!=B, the longer one is the answer

### 530. Minimum Absolute Difference in BST
Inorder will form a increasing sequence, neighboring difference is the answer
```cpp    
	int getMinimumDifference(TreeNode* root) {
        int prev=INT_MIN;
        int mindiff=INT_MAX;
        inorder(root,mindiff,prev);
        return mindiff;
    }
    void inorder(TreeNode* root,int& mindiff,int& prev)
    {
        if(!root) return;
        inorder(root->left,mindiff,prev);
        if(prev!=INT_MIN) 
            mindiff=min(mindiff,root->val-prev);
        prev=root->val;
        inorder(root->right,mindiff,prev);
    }
```

When passing prev as a value, the algorithm fails, passing as reference, it passes. Why? 
Since we need to keep the previous as global

### 538. Convert BST to Greater Tree
convert node value to original + all other nodes greater than it.
bst inorder will form a sorted array, and for sorted array this is simple (add a[n+1] to a[n])
postorder
```cpp
    TreeNode* convertBST(TreeNode* root) {
        int prev=INT_MIN;
        postorder(root,prev);
        return root;
    }
    void postorder(TreeNode* root,int& prev)
    {
        if(!root) return;
        postorder(root->right,prev);
        if(prev!=INT_MIN) root->val+=prev;
        prev=root->val;
        postorder(root->left,prev);
    }
```

### 541. Reverse String II
reverse the first k chars every 2k.
if len<k reverse the whole string
divide and conque using recursive
```cpp
    string reverseStr(string s, int k) {
        string ans;
        if(s.empty()) return ans;
        if(s.size()<k)
        {
            reverse(s.begin(),s.end());
            return s;
        }
        reverse(s.begin(),s.begin()+k);
        ans=s.substr(0,2*k);
        if(2*k<s.size()) ans+=reverseStr(s.substr(2*k),k);
        return ans;
    }
```

### 543. Diameter of Binary Tree
diameter is defined the longest path (edge=node-1) across or not across the root
this is a good question
1. pass the root, left depth+right depth
2. not passing the root, max of the left subtree and the right subtree
so two problems:
get the depth of a tree
get the diameter

recursive approach:
```cpp
    int diameterOfBinaryTree(TreeNode* root) {
        if(!root) return 0;
        int ans=depth(root->left)+depth(root->right);
        return max(ans,max(diameterOfBinaryTree(root->left),diameterOfBinaryTree(root->right)));
        
    }
    int depth(TreeNode* root)
    {
        if(!root) return 0;
        return max(depth(root->left),depth(root->right))+1;
    }
```
the above solution will cause stack overflow for a very deep tree.

1. node is visited multiple times

2. we can do the depth calculation and do the diameter at the same time reducing O(N^2) to O(N) and also stack requirement.
When stack overflow, almost the same as O(N^2)

```cpp
    int m_maxDia = 0;
    int diameterOfBinaryTree(TreeNode* root) {
        GetDepth(root);
        return m_maxDia;
    }
    int GetDepth(TreeNode* n)
    {
        if(!n) return 0;
        int left = GetDepth(n->left);
        int right = GetDepth(n->right);
        m_maxDia = max(m_maxDia, left + right);
        return max(left, right) + 1;
    }
```

### 551. Student Attendance Record I
<=1 A
no more than two continuous L
O(N) just scan the string
cnta is global no reset
only when char=L increase cntl, other char reset cntl

### 557. Reverse Words in a String III
reverse word in sentence
trivial using stringstream to process word by word

### 532. K-diff Pairs in an Array
constraints: needs to be unique, length of array up to 1e4
naive: O(N^2) search each element for the pairs
not so straightforward for better algorithm
hashmap

### 559. Maximum Depth of N-ary Tree
trivial
```cpp
    int maxDepth(Node* root) {
        if(!root) return 0;
        int ans=1;
        for(auto t: root->children)
            ans=max(ans,1+maxDepth(t));
        return ans;
    }
```
### 561. Array Partition I
make pair of min the sum largest
sort choose the first
proof

### 563. Binary Tree Tilt
tilt of a tree node is defined as the absolute difference between the sum of all left subtree node values and the sum of all right subtree node values
The tilt of the whole tree is defined as the sum of all nodes' tilt.
two problems:
1. find its left right subtree sum
2. find its tilt and sum all the tilts
this is a very good question. We can do the two at the same time.
one use return, one use passing paramter
using pre, in or post order traversal
```cpp
    int findTilt(TreeNode* root) {
        int sum=0;
        tile(root,sum);
        return sum;
    }
    int tile(TreeNode* root,int& sum)
    {
        if(!root) return 0;
        int leftsum=tile(root->left,sum);
        int rightsum=tile(root->right,sum);
        sum+=abs(leftsum-rightsum);
        return root->val+leftsum+rightsum;
    }
```

### 566. Reshape the Matrix
trivial just index calculation

### 572. Subtree of Another Tree
contains two problems
1. if two trees are equal
2. subtree
good question
```cpp
    bool isSubtree(TreeNode* s, TreeNode* t) {
        if(!s) return 0; //this is needed to use s->left
        if(equal(s,t)) return 1;
        return isSubtree(s->left,t) || isSubtree(s->right,t);
    }
    bool equal(TreeNode* s,TreeNode* t)
    {
        if(!s && !t) return 1;
        if(!s || !t) return 0;
        return s->val==t->val && equal(s->left,t->left) && equal(s->right,t->right);
    }
```
bugs: to avoid runtime error, need add if(!s) return 0, so we can use s->left and s->right

### 575. Distribute Candies
greedy: get number of types and return the min(types,n/2)

### 581. Shortest Unsorted Continuous Subarray
the min > left max
the max < right min
two pointer
allows duplicate
O(N)
implementation is trick
1. lmax and rmin array is not necessary since we are comparing at the same spot
2. we can do one pass to get the lmax and rmin
```cpp
    int findUnsortedSubarray(vector<int>& nums) {
        int n = nums.size(), beg = -1, end = -2, min0 = nums[n-1], max0 = nums[0];
        for (int i=1;i<n;i++) {
          max0 = max(max0, nums[i]);
          min0 = min(min0, nums[n-1-i]);
          if (nums[i] < max0) end = i;
          if (nums[n-1-i] > min0) beg = n-1-i; 
        }
        return end - beg + 1;
   }
```
another approach: sort and compare with original O(nlogn)

### 589. N-ary Tree Preorder Traversal
append a vector to a vector using insert
```cpp
    vector<int> preorder(Node* root) {
        //node first
        vector<int> ans;
        if(!root) return ans;
        ans.push_back(root->val);
        for(auto t: root->children)
        {
            vector<int> vt=preorder(t);
            ans.insert(ans.end(),vt.begin(),vt.end());
        }
        return ans;
    }
```
or a bit faster which need not return vector
```cpp
    vector<int> preorder(Node* root) {
        //node first
        vector<int> ans;
        helper(root,ans);
        return ans;
    }
    void helper(Node* root,vector<int>& ans)
    {
        if(!root) return;
        ans.push_back(root->val);
        for(auto t: root->children)
            helper(t,ans);
    }
```
### 590. N-ary Tree Postorder Traversal
same

### 594. Longest Harmonious Subsequence
max-min=1
```cpp
    int findLHS(vector<int>& nums) {
       unordered_map<int,int> mp;
        for(int n: nums) mp[n]++;
        int ans=0;
        for(auto t: mp)
        {
            if(mp.count(t.first-1)) ans=max(ans,t.second+mp[t.first-1]);
            if(mp.count(t.first+1)) ans=max(ans,t.second+mp[t.first+1]);
        }
        return ans;
    }
```

### 598. Range Addition II	
trivial
```cpp
    int maxCount(int m, int n, vector<vector<int>>& ops) {
        //get the minx miny
        int mx=m,my=n;
        for(auto t: ops)
        {mx=min(t[0],mx),my=min(t[1],my);}
        return mx*my;
    }
```

### 599. Minimum Index Sum of Two Lists	
two hashmap
```cpp
    vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
        unordered_map<string,int> mp1,mp2,mp3;
        for(int i=0;i<list1.size();i++) mp1[list1[i]]=i;
        vector<string> ans;
        int minlen=INT_MAX;
        for(int i=0;i<list2.size();i++) {
            mp2[list2[i]]=i;
            if(mp1.count(list2[i])) 
            {
                minlen=min(minlen,mp1[list2[i]]+i);
                mp3[list2[i]]=mp1[list2[i]]+i;
            }
        }

        for(auto t: mp3)
            if(t.second==minlen) ans.push_back(t.first);
        return ans;
    }
```

### 605. Can Place Flowers	
```cpp
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        //assuming both ends are empty
        int m=flowerbed.size();
        flowerbed.push_back(0);
        flowerbed.insert(flowerbed.begin(),0);
        //a zero region of lenght l can support (l-1)/2 flowers
        int cnt=0;
        for(int i=0;i<flowerbed.size();i++)
        {
            if(flowerbed[i]==0) cnt++;
            else {n-=(cnt-1)/2;cnt=0;}
            if(n<=0) break;
        }
        if(cnt) n-=(cnt-1)/2;
        return n<=0;
        
    }
```
don't forget the last one

### 606. Construct String from Binary Tree	
```cpp
    string tree2str(TreeNode* t) {
        string ans;
        if(!t) return ans;
        ans+=to_string(t->val);
        if(t->right || t->left) ans+="("+tree2str(t->left)+")"; //when 
        if(t->right) ans+="("+tree2str(t->right)+")";
        return ans;
    }
```
### 617. Merge Two Binary Trees	
straightforward
```cpp
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if(!t1 && !t2) return 0;
        if(!t1) return t2;
        if(!t2) return t1;
        t1->val+=t2->val;
        t1->left=mergeTrees(t1->left,t2->left);
        t1->right=mergeTrees(t1->right,t2->right);
        return t1;
    }
```

	

### 628. Maximum Product of Three Numbers
sort is trivial
O(n) method: to get the largest 3 and smallest 2 one pass!
which is a regular method. can be generallized to k max
```java
public int maximumProduct(int[] nums) {
        int max1 = Integer.MIN_VALUE, max2 = Integer.MIN_VALUE, max3 = Integer.MIN_VALUE, min1 = Integer.MAX_VALUE, min2 = Integer.MAX_VALUE;
        for (int n : nums) {
            if (n > max1) {
                max3 = max2;
                max2 = max1;
                max1 = n;
            } else if (n > max2) {
                max3 = max2;
                max2 = n;
            } else if (n > max3) {
                max3 = n;
            }

            if (n < min1) {
                min2 = min1;
                min1 = n;
            } else if (n < min2) {
                min2 = n;
            }
        }
        return Math.max(max1*max2*max3, max1*min1*min2);
    }
```

### 633. Sum of Square Numbers
trivial

### 637. Average of Levels in Binary Tree
any order traversal
```cpp
    vector<double> averageOfLevels(TreeNode* root) {
        unordered_map<int,vector<long long>> mp; //level vs sum,cnt pair
        preorder(root,0,mp);
        vector<double> ans(mp.size());
        for(auto t: mp)
            ans[t.first]=double(t.second[0])/t.second[1];
        return ans;
    }
    void preorder(TreeNode* root,int h,unordered_map<int,vector<long long>>& mp)
    {
        if(!root) return;
        if(mp.count(h)) mp[h][0]+=root->val,mp[h][1]++;
        else mp[h]={root->val,1};
        preorder(root->left,h+1,mp);
        preorder(root->right,h+1,mp);
    }
```


### 643. Maximum Average Subarray I
trivial but there is a trap
```cpp
    double findMaxAverage(vector<int>& nums, int k) {
        int tsum=0,tmax=INT_MIN;
        for(int i=0;i<nums.size();i++)
        {
            if(i<k) tsum+=nums[i];
            else {tmax=max(tmax,tsum);tsum+=nums[i];tsum-=nums[i-k];}
        }
        tmax=max(tmax,tsum);//last step is very important
        return double(tmax)/k;
    }
```
the end compare is important

### 645. Set Mismatch
find the number appearing twice and the missing number
eg.[1,2,2,4]
if we xor 1,2,3,4, we leave 2^3
if we find the element appear twice, then missing number can be calculated: c=2^3, b=2^3^2=3
finding the missing number can use cross all elements seen by negative the seens.
```java
    vector<int> findErrorNums(vector<int>& nums) {
        int dup,miss,res=0;
        for(int i=0;i<nums.size();i++)
        {
            int v=abs(nums[i]);
            res^=(i+1)^v;
            //use it as index to negate the value
            if(nums[v-1]>0) nums[v-1]*=-1;
            else dup=v;
        }
        miss=res^dup;
        return {dup,miss};
    }
  ```    
  Note: one loop is a very efficient algorithm. 

### 653. Two Sum IV - Input is a BST
trivial using any order traversal and hashmap

### 661. Image Smoother
input value: 0-255
using a new array is trivial
in-place can use the high 8 bit

### 665. Non-decreasing Array
When A(i+1)<A[i]. ie Ai-1<=Ai>Ai+1, we need to replace one to make Ai-1<=Ai<=Ai+1. Greedy choice we shall make Ai+1 as small as possible
It could be:
We need replace Ai+1 to be Ai, (when Ai+1<Ai-1)
Or Ai too large. Need replace Ai with Ai-1 (when Ai+1>Ai-1)

```cpp
    bool checkPossibility(vector<int>& nums) {
        int cnt=0;
        for(int i=1;i<nums.size();i++)
        {
            if(nums[i]<nums[i-1])
            {
                cnt++;
                if(i>=2 && nums[i]<nums[i-2]) nums[i]=nums[i-1];
                else nums[i-1]=nums[i];
            }
        }
        return cnt<=1;
    }
```

If previous larger,
There is no i-2 element, we have no choice but replace Ai-1 with Ai to make it smaller
If there are i-2 element, and current value <i-2 element, either we change Ai-1 and Ai both to Ai-2 (which changes 2 element) or change Ai to Ai-1 (one element)

### 669. Trim a Binary Search Tree
Trims all the elements so remaining in the range [L,R]
This is not so straightforward
Root to be trimed:  promote the rightmost node in the left subtree to root or the leftmost node in the right subtree to the root. Postorder is more appropriate for this
Root not to be trimed: root->left=trim(left), root->right=trim(right)
However, above analysis does not get the point
If not trimed
Root value<L, then root and left subtree shall be removed
Root value >R, then root and right subtree shall be removed
```cpp
    TreeNode* trimBST(TreeNode* root, int L, int R) {
        if(!root) return 0;
        if(root->val<L) return trimBST(root->right,L,R);
        if(root->val>R) return trimBST(root->left,L,R);
        root->left=trimBST(root->left,L,R);
        root->right=trimBST(root->right,L,R);
        return root;
    }
```

### 671. Second Minimum Node In a Binary Tree
Only has zero or two children, node value is the smaller of its children
Return 2nd min
Observation: root is smallest, children will be >= root
All the same value, there is no second min
Appr#1: traverse and find the smallest one > root->val
```cpp
    int findSecondMinimumValue(TreeNode* root) {
        if(!root) return -1;
        int ans=INT_MAX;
        traverse(root,root->val,ans);
        return ans==INT_MAX?-1:ans;
    }
    void traverse(TreeNode* root,int v,int& ans)
    {
        if(!root) return;
        if(root->val>v) ans=min(ans,root->val);
        traverse(root->left,v,ans);
        traverse(root->right,v,ans);
    }
```

### 674. Longest Continuous Increasing Subsequence
This is for subarray!.
Trivial, just count, edge case: empty array

### 680. Valid Palindrome II
For example abca, reverse we get acba
We either delete b or c
```cpp    
	bool validPalindrome(string s) {
       string rs=s;
        reverse(rs.begin(),rs.end());
        if(s==rs) return 1;
        int n=s.size();
        for(int i=0;i<s.size();i++)
        {
            if(s[i]!=rs[i]) //either delete the char in s or rs
            {
                return s.substr(0,i)+s.substr(i+1)==rs.substr(0,n-i-1)+rs.substr(n-i) ||
                    rs.substr(0,i)+rs.substr(i+1)==s.substr(0,n-i-1)+s.substr(n-i);
            }
        }
        return 1;
    }
```
	
Be very careful with those index calculations! Very easy to make mistakes

### 682. Baseball Game
Stack

### 686. Repeated String Match
1. A and B shall contain the same char to be able to match
2. B shall maintain the order of A
Think in another way
If A and B is OK to match by repeating
B take one more char from first, whole for several copies, and one or several chars from the last
b.length/a.length+2 shall be able to cover the string b
```cpp
    int repeatedStringMatch(string a, string b) {
        string as = a;
        for (int rep = 1; rep <= b.length() / a.length() + 2; rep++, as += a)
            if (as.find(b) != string::npos) return rep;
        return -1;
    }
```
	
Lesson: if it is not easy from one side, try to conquer it from the other side

### 687. Longest Univalue Path
Not so straightforward
If cross root, the value is the root val
If not cross the root, find it in left subtree and right subtree, choose the longest
1.	final answer if the node number -1
2.	two recursive question

The following solution is incorrect, what is wrong?
```cpp
    int longestUnivaluePath(TreeNode* root) {
        if(!root) return 0;
        //if cross the root
        int len=1+GetPath(root->left,root->val)+GetPath(root->right,root->val);
        int len1=max(longestUnivaluePath(root->left),longestUnivaluePath(root->right));
        return max(len,len1)-1;
    }
    int GetPath(TreeNode* root,int v) //find the longest path with given value
    {
        if(!root) return 0;
        if(root->val!=v) return 0;
        return 1+max(GetPath(root->left,v),GetPath(root->right,v)); //add the root
}
```

We are only calculating the starting the root value, but we did not keep the global max. we can add a global to get the global max
But it is still very inefficient since each node is visited multiple times (LongestUnivaluePath will visit each node once
GetPath will visit its child node another time.

### 690. Employee Importance
Don’t confuse with the problem
The id is arbitrary and is not the index
Convert the vector to hash table first

### 693. Binary Number with Alternating Bits
```cpp    
	bool hasAlternatingBits(int n) {
        if(!n) return 1;
        int prev=-1;
        while(n)
        {
            int cur=n%2;
            n/=2;
            if(cur==prev) return 0;
            prev=cur;
        }
        return 1;
    }
```

### 696. Count Binary Substrings
For example: 1. same amount of 01, 0 and 1 are grouped together
00110011
0: 
00:
001: 01
0011: 0011
00110: 0110x
001100: 1100
0011001: 01,1001x
00110011: 0011, 00110011x

Ending with 0 or 1 we count the 0 or 1
```cpp
    int countBinarySubstrings(string s) {
    int prevRunLength = 0, curRunLength = 1, res = 0;
    for (int i=1;i<s.length();i++) {
        if (s[i] == s[i-1]) curRunLength++;
        else {
            prevRunLength = curRunLength;
            curRunLength = 1;
        }
        if (prevRunLength >= curRunLength) res++;
    }
    return res;        
    }
```
### 697 degree of an array
easy

### 700. Search in a Binary Search Tree
trivial

### 703. Kth Largest Element in a Stream
priority-queue can maintain the k largest, but access the kth element takes O(K) time
build a minheap, (Don't confuse the ask: it asks for the kth element in the sorted)
Note the interface for the constructor using input vector as reference cannot pass the compiler

### 704. Binary search
Binary search pay attention:
1. miss the range
2. infinite loop
```cpp
    int search(vector<int>& nums, int target) {
        int l=0,r=nums.size()-1;
        while(l<=r)
        {
            int mid=l+(r-l)/2;
            if(nums[mid]==target) return mid;
            if(nums[mid]>target) r=mid-1;
            else l=mid+1;
        }
        return -1;
    }
```
The key is: if we use mid+/-1 for next step, then we need evaluate l<=r case
if we just use mid, then we may falling into infinite loop (for example l and r only differs by 1)

### 705. Design HashSet
hashset uses a large prime number modulus.
a vector of linked list (could use other inner data structure for efficiency)
hash function: up to 10^4 elements, value range 10^6, collision 100
to be done later

### 706 design hashmap

### 707 design linkedlist

### 709. To Lower Case
trivial

### 717. 1-bit and 2-bit Characters
determine if last char is a 1bit char
every 1 must follow a 1 or 0
O(N)
or just check the end
00: must be true
110: depends previous

### 720. Longest Word in Dictionary
sort the word according to length. if length is the same, sort alphabetically.
and build a map incrementally and we only need to check if the last char removed exist or not
```cpp
bool cmp(string& a,string& b)
{
    return a.length()<b.length() || (a.length()==b.length() && a<b);
}
class Solution {
public:
    string longestWord(vector<string>& words) {
        sort(words.begin(),words.end(),cmp);
        unordered_map<string,bool> mp;
        string ans;
        for(string w: words)
        {
            if(w.size()==1) {mp[w]=1;if(ans.size()<w.size()) ans=w;}
            else 
            {
                string s=w;
                s.pop_back();
                if(mp.count(s) && mp[s]) 
                {
                    mp[w]=1;
                    if(ans.size()<w.size()) ans=w;
                }
                else mp[w]=0;
            }
        }
        return ans;
    }
};
```
bugs: don't forget to add the one char into answer

### 724. Find Pivot Index
pivot index can be the first or the last element. Do not miss that
```cpp
    int pivotIndex(vector<int>& nums) {
        int tsum=accumulate(nums.begin(),nums.end(),0);
        int prefix=0;
        for(int i=0;i<nums.size();i++)
        {
            if(prefix==tsum-nums[i]-prefix) return i;
            prefix+=nums[i];
        }
        return -1;
    }
```    
### 728. Self Dividing Numbers
trivial

### 733. Flood Fill
bfs or dfs to fill the color
dfs is easier but pay attention to new color is same as old color, will cause infinite loop and shall be treated first.

```cpp
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        //dfs(image,sr,sc,newColor);
        if(image.size()==0) return image;
        int c=image[sr][sc];
        if(c==newColor) return image;
        dfs(image,sr,sc,newColor,c);
        return image;
    }
    void dfs(vector<vector<int>>& image, int sr, int sc, int nc,int oc)
    {
        int m=image.size(),n=image[0].size();
        if(sr<0 ||sr>=m ||sc<0 ||sc>=n || image[sr][sc]!=oc) return;
        image[sr][sc]=nc;
        dfs(image,sr-1,sc,nc,oc);
        dfs(image,sr+1,sc,nc,oc);
        dfs(image,sr,sc+1,nc,oc);
        dfs(image,sr,sc-1,nc,oc);
    }
```

### 744. Find Smallest Letter Greater Than Target
char a-z but it is wraparound, chars are already sorted
length up to 10^4

trivial using upper_bound
```cpp
    char nextGreatestLetter(vector<char>& letters, char target) {
        auto it=upper_bound(letters.begin(),letters.end(),target);
        if(it==letters.end()) return letters[0];
        return *it;
    }
```    
### 746. Min Cost Climbing Stairs
you pay the cost at i and can jump one or two steps. start from 0 or 1 and reach the top at the min cost
dp program
dp[i]=min(dp[i-2],dp[i-1])+cost[i]
final answer is min(dp[n-1],dp[n-2])
The problem is not well formed, the target is the floor above the last
so dp[0]=cost[0], dp[1]=cost[1] and final answer is min(dp[n-1],dp[n-2]

### 747. Largest Number At Least Twice of Others
O(n) find the max and second max (again)
pay attention to:
integer overflow

```cpp
    int dominantIndex(vector<int>& nums) {
        int max1=INT_MIN,max2=INT_MIN;
        int ans=-1;
        for(int i=0;i<nums.size();i++)
        {
            int t=nums[i];
            if(t>max1) {max2=max1,max1=t;ans=i;}
            else if(t>max2) max2=t;
        }
        if(max1==INT_MIN) return -1;
        if(max2==INT_MIN) return ans;
        if(max1>=2*max2) return ans;
        return -1;
    }
```
### 748. Shortest Completing Word
ignore case
1. convert case
2. min length of string
3. first matching word

### 754. Reach a Number
starting from 0 reaching target by 1,2,3,4,..... two directions
greedy choice: 
first go just beyond target, 1+2+..+n=n*(n+1)/2
the difference is diff=target-n(n+1)/2, we can negelate some items
if we negate i, we create a difference of 2i
so if diff is odd:
if n+1 is odd: we add n+1 to even and flip a number
if n+1 is even, we add n+1, n+2 to reach an even
```cpp
    int reachNumber(int target) {
        int i=0;
        target=abs(target);
        int sum=0;
        while(sum<target) sum+=i++;
        int diff=sum-target;
        if(diff%2==0) return i-1;
        else return i+(i%2==0);
    }
```
traps:
1. negative target
2. after loop, i is one greater than the last step


### 762. Prime Number of Set Bits in Binary Representation
trivial

### 766. Toeplitz Matrix
for a matrix mxn, there are m+n diagonal from top left to bottom right
think it simple, just compare (i,j) with (i-1,j-1). We don't need to consider index overflow

### 771 jewels and stones

### 783. Minimum Distance Between BST Nodes
trivial using prev and inorder traversal, same as 530
but I fell into the same trap again.
prev and ans shall both be global so we need use reference passing

### 784. Letter Case Permutation
each char has two options: upper case or lower case 2^n
we can use recursive approach but the implementation is not trivial
```cpp
    vector<string> letterCasePermutation(string S) {
        vector<string> ans;
        backtrack(S,0,ans);
        return ans;
    }
    void backtrack(string s,int i,vector<string>& ans)
    {
        if(i==s.size()) {ans.push_back(s);return;}
        backtrack(s,i+1,ans);
        if(isalpha(s[i]))
        {
            s[i]^=32;
            backtrack(s,i+1,ans);
        }
    }
```    

### 788. Rotated Digits
Rotate 180
0,1,8 no change
2-5, 6-9
So a number can only contain 0182569, but cannot only have 018
Other digits are not allowed
Straightforward:
```cpp
    int rotatedDigits(int N) {
       //not allowed: 347
        //has to contain at least: 2569
        unordered_set<int> notallowed({3,4,7}),musthave({2,5,6,9});
        int ans=0;
        for(int i=1;i<=N;i++)
        {
            int t=i;
            int flag=0;
            while(t)
            {
                int d=t%10;
                t/=10;
                if(notallowed.count(d)) {flag=0;break;}
                if(musthave.count(d)) flag=1;
            }
            ans+=flag;
        }
        return ans;
    }
```

### 796. rotated string
Check same histogram
Check A+A contains B
```cpp
    bool rotateString(string A, string B) {
        //first the two string has the same histogram
        unordered_map<char,int> ma,mb;
        for(char c: A) ma[c]++;
        for(char c: B) mb[c]++;
        if(ma!=mb) return 0;
        return (A+A).find(B)!=string::npos;
    }
```
The first check is unnecessary

### 804. Unique Morse Code Words
Trivial using hashset

### 806. Number of Lines To Write String
trivial, don't forget to add the last line

### 811. Subdomain Visit Count
string input
getline with delim . and space
getline does not skip white spaces. that is a problem!
or use some other read (read the whole string and find . is better)
do not forget to accumulate the counting.

### 812. Largest Triangle Area
brutal force
area of a polygon using points
can be approved using cross product

### 819. Most Common Word
again process strings
hashmap,
trap: the last step don't forget to add the remaining word.

### 821. Shortest Distance to a Character
mthd1: two passes, first pass to get all positions of the character, and then find offset for each char
mthd2: from both direction to get the offset and choose the min
```cpp
    vector<int> shortestToChar(string S, char C) {
        int n = S.size();
        vector<int> res(n, n);
        int pos = -n;
        for (int i = 0; i < n; ++i) {
            if (S[i] == C) pos = i;
            res[i] = min(res[i], abs(i - pos));
        }
        for (int i = n - 1; i >= 0; --i) {
            if (S[i] == C)  pos = i;
            res[i] = min(res[i], abs(i - pos));
        }
        return res;
    }
```
trick: first we assume the pos is on the far left (must be very far)

### 824. Goat Latin
string read
trivial

### 830. Positions of Large Groups
trival, two pointer

### 832. Flipping an Image
flip and then invert, trival

### 836. Rectangle Overlap
this requires some intuition
Given 2 segment (left1, right1), (left2, right2), how can we check whether they overlap?
If these two intervals overlap, it should exist an number x,

left1 < x < right1 && left2 < x < right2

left1 < x < right2 && left2 < x < right1

left1 < right2 && left2 < right1

2d: overlaps at two directions

This is an important approach: if 2D is hard try 1D first. some 2d are just two 1d problem

### 840. Magic Squares In Grid
brutal force is trivial
center is 5, the odd is in the center and even is in the corner
note it also needs satisify it contains all 1 to 9

### 844. Backspace string compare
### 849. Maximize distance to closest person
### 852. Peak index in a mountain array
### 859. Buddy string
### 860. Lemon Change
### 867. Transpose matrix
### 868. Binary gap
### 872. leaf similar trees
### 874. walking robot simulation
### 876. Middle of the linked list
### 883. projection area of 3d shape
### 884. uncommon words from two sentences
### 888. fair candy swap
### 892. surface area of 3d shape
### 893. groups of special equivalent strings
### 896. Monotonic Array
Is_sorted for begin to end or from rbegin to rend
Or use flag_inc and flag_dec to check (using &&

### 897. Increasing Order Search Tree
Similar to convert BST to linked list
A node: left will be null, and its right still goes to right
Root->left
       |->right
Now changes to left->root->right
We need to get the last node of the left subtree (tail)
For the left subtree, the tail is the root
For the right subtree, the tail is the tail (or the parent is better)
So for empty node, we shall return the tail (parent)

### 905. Sort Array By Parity
Two pointer from both end
```cpp
    vector<int> sortArrayByParity(vector<int>& A) {
        int i=0,j=A.size()-1;
        while(i<j)
        {
            if(A[i]%2==0) i++;
            else if(A[j]%2) j--;
            else if(A[i]%2 && A[j]%2==0) 
                swap(A[i++],A[j--]);
        }
        return A;
    }
```
	
Traps: if..else..else if we do not include else we will get wrong logics

### 908. Smallest Range I
Add any x [-K K] to get the min diff between min and max
The min can be changed up to min+K
The max can be changed down to max-K

### 914. X of a Kind in a Deck of Cards
Hashmap and gcd

Gcd for multiple numbers: intial it as 0

### 917. Reverse Only Letters
Two pointers

### 929. Unique Email Addresses
String find

### 933. Number of Recent Calls
Using deque to remove old calls

### 937. Reorder Log Files
Arrange string in log, id pair and sort

### 941. Valid Mountain Array
Climbing from both end and reach at the same spot
```cpp
	bool validMountainArray(vector<int>& A) {
        int n = A.size(), i = 0, j = n - 1;
        while (i + 1 < n && A[i] < A[i + 1]) i++;
        while (j > 0 && A[j - 1] > A[j]) j--;
        return i > 0 && i == j && j < n - 1;
    }
```

### 942. DI String Match
0 to N
DI
I from the min 
D from the max
Note: the last one is the min
```cpp
    vector<int> diStringMatch(string S) {
       int min0=0,max0=S.size();
        vector<int> ans;
        for(char c: S)
        {
            if(c=='D') {ans.push_back(max0--);}
            else ans.push_back(min0++);
        }
        ans.push_back(min0);
        return ans;
    }
```

### 944. Delete Columns to Make Sorted
Delete columns so that all comlumn are sorted
Just remove all those columns which is not sorted

### 949. Largest Time for Given Digits
Shall consider the time together and remove those invalid
4 number only has 4*3*2*1=24 combinations
Note: it needs two digit output

Next_permutation return bool

### 953. Verifying an Alien Dictionary
Convert to normal order
```cpp    
	bool isAlienSorted(vector<string>& words, string order) {
        unordered_map<char,char> mp;
        char t='a';
        for(char c: order) mp[c]=t++;
        for(string& s: words)
        {
            for(int i=0;i<s.size();i++) s[i]=mp[s[i]];
        }
        return is_sorted(words.begin(),words.end());
    }
```

### 922. Sort Array By Parity II
Half odd, half even
Odd element on odd index
Even element on even index
Two pointer from the same end
```cpp
    vector<int> sortArrayByParityII(vector<int>& A) {
       int i=0,j=1;
        while(i<A.size() && j<A.size())
        {
            if(A[i]%2==0) i+=2;
            else if(A[j]%2) j+=2;
            else {swap(A[i],A[j]);i+=2,j+=2;}
        }
        return A;
    }
```

Note: it shall be i<n && j<n instead of ||
If one pointer reaches the end, that means all are done.

### 925. Long Pressed Name
Two pointer to counter the char
But not so easy
I for name, j for typed
When a char in name matched with current char in typed, move forward to next char in name
J keeps going, if the two char does not match, the only case shall be that the char is repeated char in the typed

### 961. N-Repeated Element in Size 2N Array
Trivial but there is some O(N) solution

### 965. Univalued Binary Tree
Can indorder with a helper
Or 
```cpp
    bool isUnivalTree(TreeNode* root) {
        if(root->left)
        {
            if(root->val!=root->left->val || !isUnivalTree(root->left))
                return 0;
        }
        if(root->right)
        {
            if(root->val!=root->right->val || !isUnivalTree(root->right))
                return 0;
        }
        return 1;
    }
```

### 970. Powerful Integers
X, y>0
i>=0
x^i: range from 1 to n-1
to look for the solution is not good
think in a different way, try all combinations and save all those good ones
also x=1 or y=1 are special case
```cpp    
	vector<int> powerfulIntegers(int x, int y, int bound) {
        unordered_set<int> ms;
        for(int a=1;a<bound;a*=x)
        {
            for(int b=1;a+b<=bound;b*=y)
            {
                ms.insert(a+b);
                if(y==1) break;
            }
            if(x==1) break;
        }
        return vector<int>(ms.begin(),ms.end());
    }
```

### 976. Largest Perimeter Triangle
Only requirement a+b>c
Sort and do it reverse direction

### 977. Squares of a Sorted Array
O(nlogn) is trivial
Use two pointer to get the max of square
Cannot do in-place

985
989
993
994
997
999
1002
1005
1009
1010
1013
1018
1021
1022


