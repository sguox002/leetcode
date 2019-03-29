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

374. Guess number higher or lower
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

383. Ransom Note
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
387. First Unique Character in a String
appr1: traverse and count the chars, and then find the first char appear once
appr2: traverse and record first index and count, iterate the map and find the first

389. Find the Difference
trivial using xor
iterate on s and t
there is only one difference, so we can set res=t.back() and one loop is done.

400. Nth Digit
1 digits 1-9: 9 digits
2 digits 10-99: 90x2
3 digits: 100-999: 900x3
...
n digits: 9*10^(n-1)*n

so the first we can know how many digits
and then we know which number
finally get the nth digit





448. Find All Numbers Disappeared in an Array
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

453. Minimum Moves to Equal Array Elements
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

455. Assign Cookie
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

459. Repeated Substring Pattern
if we have AA pattern, then we copy it AAAA, and throw the first and end char, we can find the pattern.
```cpp
    bool repeatedSubstringPattern(string s) {
        string ss=s+s;
        int n=ss.size();
        //check ss.substr(1) contains s
        return ss.substr(1,n-2).find(s)!=string::npos;
    }
```

461. Hamming Distance
x and y: number of different bits
appr#1: use bitset
appr#2: x^y will get the different ones to set

463. Island Perimeter
not trivial
a grid has 4 sides: left,right,top, bottom
find how many 1 in the map. If without the consideration of surrounding cells, the total perimeter should be the total amount of 1 times 4.
find how many cell walls that connect with both lands. We need to deduct twice of those lines from total perimeter

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
475. Heaters
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

476. Number Complement
get the highest bit num 1<<bit-1-num 
pay attention to overflow use 1L<<bit

482. License Key Formatting
Appr#1
1 remove non alnum char and convert to upper
2. reverse count and insert -

Appr#2
store in a stack and reverse

Appr#3: reverse scan the string and add - in one scan
this is a important hint since the first is to be decided last

485. Max Consecutive Ones
pretty simple. reset cnt when see 0 increase cnt when see 1

492. Construct the Rectangle
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

538. Convert BST to Greater Tree
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

541. Reverse String II
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

543. Diameter of Binary Tree
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
2. we can do the depth calculation and do the diameter at the same time
```cpp
    int m_maxDia = 0;
    int diameterOfBinaryTree(TreeNode* root) {
        GetDepth(root);
        return m_maxDia;
    }
    int GetDepth(TreeNode* n)
    {
        if(!n)
        {
            return 0;
        }
        int left = GetDepth(n->left);
        int right = GetDepth(n->right);
        m_maxDia = max(m_maxDia, left + right);
        return max(left, right) + 1;
    }
```

551. Student Attendance Record I
<=1 A
no more than two continuous L
O(N) just scan the string
cnta is global no reset
only when char=L increase cntl, other char reset cntl

557. Reverse Words in a String III
reverse word in sentence
trivial using stringstream to process word by word

532. K-diff Pairs in an Array
constraints: needs to be unique, length of array up to 1e4
naive: O(N^2) search each element for the pairs
not so straightforward for better algorithm
hashmap

559. Maximum Depth of N-ary Tree
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
561. Array Partition I
make pair of min the sum largest
sort choose the first
proof

563. Binary Tree Tilt
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

566. Reshape the Matrix
trivial just index calculation

572. Subtree of Another Tree
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

575. Distribute Candies
greedy: get number of types and return the min(types,n/2)

581. Shortest Unsorted Continuous Subarray
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
589. N-ary Tree Preorder Traversal
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
590. N-ary Tree Postorder Traversal
same


another approach: sort and compare with original O(nlogn)

628. Maximum Product of Three Numbers
sort is trivial
O(n) method: to get the largest 3 and smallest 2 one pass!
which is a regular method
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

633. Sum of Square Numbers
trivial
643. Maximum Average Subarray I
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

645. Set Mismatch
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

653. Two Sum IV - Input is a BST
trivial using any order traversal and hashmap

661. Image Smoother
input value: 0-255
using a new array is trivial
in-place can use the high 8 bit

665. Non-decreasing Array
