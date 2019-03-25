189. Rotate Array
approach1: double the array and return begin()+k for n elements
approach2: k to end and then followed by 0 to k
approach 3:
in-place O(1) memory approach:
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
traps: need make k<n

190. Reverse Bits
1. don't misunderstand the question. It ask reverse, not 1 to 0 and 0 to 1
2. it needs the trailing and leading 0s to fill 32 bit
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

191. Number of 1 Bits
trivial

198. House Robber
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

202. Happy Number
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

203. Remove Linked List Elements
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
            {
                prev->next=curr->next;
            }
            else prev=curr;
            curr=curr->next;
        }
        return dummy->next;
    }
```

204. Count Primes
basic math problem
n could be very large, pay attention to overflow problem
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

205. Isomorphic Strings
we just map both to abc order
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
206 reverse linked list
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

217. Contains Duplicate
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
219. Contains Duplicate II
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

225. Implement Stack using Queues
only support push to the back and pop to the front
when we push an element we need pop all front and add back to it

226. Invert Binary Tree
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

231. Power of Two
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

232. Implement Queue using Stacks
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

234. Palindrome Linked List
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
    void print(ListNode* head)
    {
        while(head)
        {
            cout<<head->val<<"->";
            head=head->next;
        }
    }
```
possible problem:
1. empty list is considered to be true
2. reverse list shall return prev instead of head (it is null)

235. Lowest Common Ancestor of a Binary Search Tree

power of 2:
1. num>0
2. num & num-1 ==0

power of 3
- num>0
- keep divide 3 until go to 1

power of 4:
- num>0
- even bit is 1 ( num & num-1==0 and num&0x555555 to make sure it is not 2^n)

344. Reverse String
trivial using two pointer from both end

345. reverse vowels
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
Note to write the while loop:
make exclusive 3 conditions, if else if else to let i++ or j-- or i++,j-- so we do not need to worry about i<j inside the while loop. Often that will introduce bugs.

349. Intersection of Two Arrays
does not allow duplicates
trivial using hash set

350. Intersection of two arrays II
allow duplicates
using hashmap
if arrays are sorted, we can use two pointer to go up

367. Valid Perfect Square
no sqrt can be used.
Appr# 1
n^2=(n-1)^2+2n-1
so each n^2 is previous (n-1)^2 add an odd number
trap: be careful of integer overflow
Appr# 2: binary search
n^2<num, l=mid+1
n^2>num, r=mid-1
trap: 
easy to fall into infinite loop.
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

371. Sum of Two Integers
can not use + -
bit operations: thinking they are binary 
+ -> a^b
carrier flag: a&b

```cpp
int getSum(int a, int b) {
    return b==0? a:getSum(a^b, (a&b)<<1); //be careful about the terminating condition;
}

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



    }
```





