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

    
