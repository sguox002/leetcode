# medium

easy *
straightforward solutions

2	Add Two Numbers    		31.1%	Medium	
6	ZigZag Conversion    		31.9%	Medium	
nrows*2-2 is the period
8	String to Integer (atoi)    		14.6%	Medium	
29	Divide Two Integers    		16.2%	Medium	
use subtract and find scale
be sure to use long for all to avoid overflow
```cpp
    int divide(int dividend, int divisor) {
        int s1=dividend>=0?1:-1;
        int s2=divisor>=0?1:-1;
        long div=dividend,divs=divisor;
        div=abs(div),divs=abs(divs);
        if(div<divs) return 0;
        long ans=0;
        while(divs<=div) //<= is a trap
        {
            long s=scale(div,divs); //use long
            ans+=s;
            div-=divs*s;
        }
        ans*=s1*s2; //restore sign first before judge
        if(ans>INT_MAX || ans<INT_MIN) return INT_MAX;
        return ans;
    }
    long scale(long d1,long d2)
    {
        long ans=1;
        while(d2*2<=d1) d2*=2,ans*=2;
        return ans;
    }
```	
19	Remove Nth Node From End of List    		34.3%	Medium	
it could be the root

24	Swap Nodes in Pairs    		44.7%	Medium	
use curr, prev, next

36	Valid Sudoku    		43.1%	Medium	

43	Multiply Strings    		30.7%	Medium	
48	Rotate Image    		48.5%	Medium	
49	Group Anagrams    		46.8%	Medium	
50	Pow(x, n)    		28.0%	Medium	
62	Unique Paths    		47.6%	Medium	
63	Unique Paths II    		33.5%	Medium	
64	Minimum Path Sum    		46.9%	Medium	
71	Simplify Path    		28.8%	Medium	
73	Set Matrix Zeroes    		39.8%	Medium	
74	Search a 2D Matrix    		34.9%	Medium	
75	Sort Colors    		42.2%	Medium	
80	Remove Duplicates from Sorted Array II    		40.3%	Medium
leave at most two in place
82	Remove Duplicates from Sorted List II    		33.0%	Medium	
86	Partition List    		37.2%	Medium	
give an x and all <x appear on left and >x on its right
use two list and then connect
91	Decode Ways    		22.3%	Medium	
92	Reverse Linked List II    		34.9%	Medium	
reverse m to n, one pass.
1-2-3-4-5 m=2, n=4, that is to reverse 2-3-4 to 4-3-2
find the previous node 1, reverse the 2 with 3 nodes, and connect the prev and next.

93	Restore IP Addresses    		31.5%	Medium	
94	Binary Tree Inorder Traversal    		56.7%	Medium	
95	Unique Binary Search Trees II    		35.7%	Medium	
96	Unique Binary Search Trees    		46.2%	Medium	
98	Validate Binary Search Tree    		25.7%	Medium	
102	Binary Tree Level Order Traversal    		48.5%	Medium	
103	Binary Tree Zigzag Level Order Traversal    		41.8%	Medium	
130	Surrounded Regions    		22.9%	Medium	
138	Copy List with Random Pointer    		27.1%	Medium	
142	Linked List Cycle II    		32.1%	Medium	
144	Binary Tree Preorder Traversal    		51.4%	Medium	

151	Reverse Words in a String    		16.8%	Medium	
152	Maximum Product Subarray    		29.2%	Medium	
165	Compare Version Numbers    		23.6%	Medium	
166	Fraction to Recurring Decimal    		19.5%	Medium	
186	Reverse Words in a String II    		37.7%	Medium	
199	Binary Tree Right Side View    		47.9%	Medium	
200	Number of Islands    		41.5%	Medium	
213	House Robber II    		35.3%	Medium	
215	Kth Largest Element in an Array    		47.8%	Medium	
230	Kth Smallest Element in a BST    		51.5%	Medium	
238	Product of Array Except Self    		55.0%	Medium	
313	Super Ugly Number    		41.3%	Medium	
314	Binary Tree Vertical Order Traversal    		40.9%	Medium	
143	Reorder List    		30.9%	Medium	
find the mid and reverse the second half and then merge
150	Evaluate Reverse Polish Notation    		32.2%	Medium	
simple using stack when we meet a operator we perform calculation and put back
223	Rectangle Area    		35.8%	Medium	
240	Search a 2D Matrix II    		40.9%	Medium	
row sorted, column sorted.
do it from bottom left so row is ascending, col is descending only one choice
similar 74: sorted as a 1d array

easy to medium **
17	Letter Combinations of a Phone Number    		41.6%	Medium	
typical backtracking problem
```cpp
    vector<string> letterCombinations(string digits) {
		vector<string> num={"","","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
		vector<string> ans;
		if(digitis.empty()) return ans;//note the dfs process empty wrong!
		dfs(digits,"",0,ans,num);
        return ans;
    }
	void dfs(string& digits,string t,int ind,vector<string>& ans,vector<string>& nums)
	{
		if(ind==digits.size()) {ans.push_back(t);return;}
		int d=digits[ind]-'0';
		for(int i=0;i<nums[d].size();i++)
		{
			t+=nums[d][i];
			dfs(digits,t,ind+1,ans,nums);
			t.pop_back();
		}
	}
```	
22	Generate Parentheses    		54.9%	Medium	
the rule: number of ( shall >= number of ) at any time
at the end the two must be the same
typical backtracking problem
```cpp
    vector<string> generateParenthesis(int n) {
        vector<string> ans;
		dfs(ans,"",0,0,n);
		return ans;
    }
	void dfs(vector<string>& ans,string t,int i,int j,int n)
	{
		if(i==n && j==n) {ans.push_back(t);return;}
		if(i<n && i>=j){
			t+='('; dfs(ans,t,i+1,j,n);t.pop_back();
		}
		if(j<n && j<i) {
			t+=')',dfs(ans,t,i,j+1,n);t.pop_back();
		}
	}
```	
can be simpler
```cpp
    vector<string> generateParenthesis(int n) {
        vector<string> vs;
        string s;
        int lcount=0,rcount=0;
        build(vs,n,lcount,rcount,s);
        return vs;
    }
    void build(vector<string>& vs,int n,int lcount,int rcount,string s)
    {
        if(s.size()==2*n) {vs.push_back(s);return;}
        if(lcount<n) build(vs,n,lcount+1,rcount,s+"(");
        if(lcount>rcount) build(vs,n,lcount,rcount+1,s+")");
    }
```
since we use s+ in the input, so we do not need to pop back
	

31	Next Permutation    		30.5%	Medium	
in place to get the next greater permutations
right to left find the first sorted position and swap with the first one larger than it.
wrong: it is not the problem swap once to get the next larger one. we need sort the sequence after the pkind

```cpp
    void nextPermutation(vector<int>& nums) {
        int n=nums.size();
		int i=n-1;
		int pkind;
		for(int i=n-1;i>0;i--){
			if(nums[i]>nums[i-1]) {pkind=i-1;break;}
		}
		if(i==0) {reverse(nums.begin(),nums.end());return;} //done

		for(int i=n-1;i>pkind;i--) 
		if(nums[i]>nums[pkind]) {swap(nums[i],nums[pkind]);break;}
		reverse(nums.begin()+i,nums.end()); //the last step is critical to make it smallest after i.
    }

1053	Previous Permutation With One Swap   47.5%	Medium	
this is only part of the 31. we do not need to sort the rest. just one swap.
	
46	Permutations    		55.2%	Medium	
generate all permutations
sort and get next_permutation, straightforward

47	Permutations II    		40.5%	Medium	
contains duplicates
direct approach: using set to avoid duplicates, which is trivial
backtracking:
```cpp
    vector<vector<int> > permuteUnique(vector<int> &num) {
        sort(num.begin(), num.end());
        vector<vector<int> >res;
        recursion(num, 0, num.size(), res);
        return res;
    }
	void recursion(vector<int> num, int i, int j, vector<vector<int> > &res) {
        if (i == j-1) {
            res.push_back(num);
            return;
        }
        for (int k = i; k < j; k++) {
            if (i != k && num[i] == num[k]) continue;
            swap(num[i], num[k]);
            recursion(num, i+1, j, res);
        }
    }
```	

60	Permutation Sequence    		33.1%	Medium	
get the kth permutations using 1 to n numbers n<=9
1xxxx, (n-1)!
2xxxx, (n-1)!
...
nxxxx, (n-1)!
so k%(n-1)! we can determine the first number, and then next....

```
    string getPermutation(int n, int k) {
		string digits;//="123456789";
		for(int i=0;i<n;i++) digits+='1'+i;
        
		string ans;
        k--; //k is 1 based
		while(n)
		{
			int f=factorial(n-1);
			int d=k/f;
			ans+=digits[d];
			digits.erase(digits.begin()+d);
			k%=f;
            n--;
		}
		return ans;
    }
	int factorial(int n)
	{
		int ans=1;
		for(int i=1;i<=n;i++) ans*=i;
		return ans;
	}
```	
54	Spiral Matrix    		30.5%	Medium	
visit the matrix in spiral order
```cpp
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
		if(matrix.empty()) return {};
		int m=matrix.size(),n=matrix[0].size();
		vector<int> ans(m*n);
		int l=0,r=n-1,t=0,b=m-1;
		int cnt=0;
		while(cnt<m*n)
		{
			for(int i=l;i<=r;i++) ans[cnt++]=matrix[t][i];
			if(++t>b) break;
			for(int i=t;i<=b;i++) ans[cnt++]=matrix[i][r];
			if(--r<l) break;
			for(int i=r;i>=l;i--) ans[cnt++]=matrix[b][i];
			if(--b<t) break;
			for(int i=b;i>=t;i--) ans[cnt++]=matrix[i][l];
			if(++l>r) break;
        }
		return ans;
    }
```
the important is to break when l>r || b>t 	
also do not forget the corner case: empty matrix

59	Spiral Matrix II    		46.6%	Medium	
generate the spiral matrix nxn
```cpp
    vector<vector<int>> generateMatrix(int n) {
		vector<vector<int>> ans(n,vector<int>(n));
		int cnt=1;
		int l=0,r=n-1,t=0,b=n-1;
		while(cnt<=n*n)
		{
			for(int i=l;i<=r;i++) ans[t][i]=cnt++;
			if(++t>b) break;
			for(int i=t;i<=b;i++) ans[i][r]=cnt++;
			if(--r<l) break;
			for(int i=r;i>=l;i--) ans[b][i]=cnt++;
			if(--b<t) break;
			for(int i=b;i>=t;i--) ans[i][l]=cnt++;
			if(++l>r) break;
		}
		return ans;
    }
```	

105	Construct Binary Tree from Preorder and Inorder Traversal    		41.1%	Medium	
preorder: root, left, right
inorder: left,root, right
so we find the root in the preorder, and split left and right in inorder

106	Construct Binary Tree from Inorder and Postorder Traversal    		39.3%	Medium	
inorder: left, root, right
postorder: left,right, root
we get the root from postorder in reverse way

116	Populating Next Right Pointers in Each Node    		37.9%	Medium	
perfect binary tree (every parent has two children)
using bfs: first layer, we populate the right pointer for the 2nd layer
```cpp    
	Node* connect(Node* root) {
		if(!root) return root;
		queue<Node*> q;
		q.push(root);
		while(q.size())
		{
			int sz=q.size();
			Node* prev=0;
			for(int i=0;i<sz;i++)
			{
				Node* t=q.front();q.pop();
				if(t->left) 
                {
                    q.push(t->left);
                    if(prev) prev->next=t->left;
                    prev=t->left;
                }
				if(t->right) 
                {
                    q.push(t->right);
                    if(prev) prev->next=t->right;
                    prev=t->right;
                }
			}
		}
		return root;
	}
```	
117	Populating Next Right Pointers in Each Node II    		34.3%	Medium	
regular binary tree, use the above solution no changes.

129	Sum Root to Leaf Numbers    		42.5%	Medium	
add all root to leaf numbers
```cpp
    int sumNumbers(TreeNode* root) {
        int ans=0;
		dfs(root,0,ans);
        return ans;
    }
	void dfs(TreeNode* root,int t,int& ans)
	{
		if(!root) return;
		t=t*10+root->val;
		if(!root->left && !root->right) ans+=t;
		dfs(root->left,t,ans);
		dfs(root->right,t,ans);
	}
```	
179	Largest Number    		25.8%	Medium	
a list of non-negative number, arrange to a largest string
a+b>b+a
```
bool cmp(int a,int b) {return to_string(a)+to_string(b)>to_string(b)+to_string(a);}
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        sort(nums.begin(),nums.end(),cmp);
		string ans;
		for(auto s: nums) ans+=to_string(s);
        while(ans.size() && ans[0]=='0') ans.erase(ans.begin());
		return ans.empty()?"0":ans;
    }
};
```	
201	Bitwise AND of Numbers Range    		35.9%	Medium	
bitwise all numbers [L,R]
straightforward O(N)
```cpp
    int rangeBitwiseAnd(int m, int n) {
		int ans=m;
		for(int i=m+1;i<=n;i++) ans&=i;
		return ans;
    }
```
if N is large, the time is large
O(nlogn)
the neighboring two elements bitand will always get the LSB to be 0 (0,1,0,1,...)
bit by bit then we can get the answer, only iterate 32 times
```cpp
int rangeBitwiseAnd(int m, int n) {
    return (n > m) ? (rangeBitwiseAnd(m/2, n/2) << 1) : m;
}
```	

279	Perfect Squares    		41.9%	Medium	
find the least number of squares added up to a target
knapsack or coin change problem.
```cpp
    int numSquares(int n) {
		vector<int> dp(n+1,INT_MAX);//dp[i] represent for target i, the min number of squares
		dp[0]=0;dp[1]=1;
		for(int t=2;t<=n;t++){
			for(int j=1;j*j<=t;j++){
				if(j*j<=t) dp[t]=min(dp[t],dp[t-j*j]+1);
			}
		}
		return dp[n];
	}
```
284	Peeking Iterator    		40.6%	Medium	
c++ concepts
call base member function and copy constructor

medium ***
11	Container With Most Water    		44.6%	Medium	
two pointer, move the smaller side pointer
12	Integer to Roman    		51.0%	Medium	
just write all ones, tens, hundreds, thousands from 0 to 9
and it is very easy to write then. (straighforward translation from human conversion)
15	3Sum    		24.0%	Medium	
sort it and use 3 pointers.
skip duplicate for i
skip duplicate for j
skip duplicate for k
be sure i, j, k are all in range.
when sum>0 k--
when sum<0 j++
=0, k--,j++
16	3Sum Closest    		45.8%	Medium	
only add a mindiff. similar to 15
18	4Sum    		30.5%	Medium	
O(N^3) use 4 pointers
be sure to skip duplicates, for j, we need keep j>i+1 to compare with previous j (otherwise j will compare with i and will skip some answers)
33	Search in Rotated Sorted Array    		32.9%	Medium	
binary search for first true and last true
when searching for last true, to avoid infinite loop we need m to bias to larger (use ceil instead of floor)

3	Longest Substring Without Repeating Characters    		28.4%	Medium	
using two pointer. j is the left pointer, when we found a same char, we move the pointer to its right
note j=mp[s[i]] will move it back so we cannot left it go back.
update the ans in each iteration not in the if condition. otherwise many bug will be introduced.
it is not so easy to get it right.

```cpp
    int lengthOfLongestSubstring(string s) {
		unordered_map<char,int> mp;
		int j=0,n=s.length();
		int ans=0;
		for(int i=0;i<s.size();i++){
			if(mp.count(s[i])) {j=max(j,mp[s[i]]+1);}
			mp[s[i]]=i;
            ans=max(ans,i-j+1);
            //cout<<j<<" "<<i<<endl;
		}
		return ans;
	}
```	
5	Longest Palindromic Substring    		27.3%	Medium	
dp or direct approach using two pointer
direct approach is straightforward.
dp approach for substring: first inside string must be palindrome and then we grow the string
dp[i,j] represents s[i...j] is a palindrome
dp[i,j]=dp[i+1,j-1]+2 if s[i]==s[j], so we need extra space for dp
i iteration need reverse and j iteration need forward.

```cpp    
    string longestPalindrome(string s) {
       //dp solution using i j
        int n=s.length();
        vector<vector<int>> dp(n,vector<int>(n)); //initialize to 0
        //dp(i,j)=length of p-string
        int ans=0;
        int start=0;
        for(int i=0;i<n;i++) {dp[i][i]=1;ans=1;}
        for(int i=0;i<n-1;i++) 
        {
            if(s[i]==s[i+1]) {dp[i][i+1]=2,ans=2,start=i;}
        }
        for(int i=n-3;i>=0;i--) //since it needs i+1
        {
            for(int j=i+2;j<n;j++) //since it needs j-1
            {
                if(s[i]==s[j] && dp[i+1][j-1]) dp[i][j]=dp[i+1][j-1]+2;
                if(ans<dp[i][j]) ans=dp[i][j],start=i;
            }
        }
        //print(dp);
        return s.substr(start,ans);
    }
```	

39	Combination Sum    		48.5%	Medium	
find all combination sum=target there is no duplicates
each number can be used multiple times
knapsack with backtracking

```cpp
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> ans;
		dfs(candidates,{},0,target,ans);
		return ans;
    }
	void dfs(vector<int>& nums,vector<int> t,int ind,int target,vector<vector<int>>& ans)
	{
		if(target<0) return; //this is necessary
		if(target==0) {ans.push_back(t);return;}
		for(int i=ind;i<nums.size();i++){
			t.push_back(nums[i]);
			dfs(nums,t,i,target-nums[i],ans);
			t.pop_back();
		}
	}
		
```	

40	Combination Sum II    		41.6%	Medium	
contain duplicates
each number can only be used once
cannot contain duplicate solutions

sort it so duplicates appear together
when the one is same as its previous skip.

```cpp
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<vector<int>> ans;
		sort(candidates.begin(),candidates.end());
		dfs(candidates,{},0,target,ans);
		return ans;
    }
	void dfs(vector<int>& nums,vector<int> t,int ind,int target,vector<vector<int>>& ans)
	{
		if(target<0) return; //this is necessary
		if(target==0) {ans.push_back(t);return;}
		for(int i=ind;i<nums.size();i++){
			if(i && i!=ind && nums[i]==nums[i-1]) continue;
			t.push_back(nums[i]);
			dfs(nums,t,i+1,target-nums[i],ans);
			t.pop_back();
		}
	}
```

216	Combination Sum III    		51.5%	Medium	
```cpp
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> ans;
		dfs(k,n,1,{},ans);
		return ans;
    }
	void dfs(int k,int n,int i,vector<int> t,vector<vector<int>>& ans)
	{
		if(k==0 && n==0) {ans.push_back(t);return;}
        if(k<=0 || n<0 || i>9) return;
        //cout<<k<<" "<<n<<endl;
		for(int j=i;j<=9;j++)
		{
			t.push_back(j);
			dfs(k-1,n-j,j+1,t,ans);
			t.pop_back();
		}
	}
```

377. combination sum IV
this is hard using dfs but dp is much simpler. but the recurrence relation is not that easy.


55	Jump Game    		31.9%	Medium	
dp or greedy. 
the key: if you can jump to max, then all inside nodes can be reached.
```cpp
    bool canJump(vector<int>& nums) {
		int reachable=0;
        int n=nums.size();
		for(int i=0;i<n;i++)
		{
            if(reachable>=n-1) return 1;
			if(i<=reachable) reachable=max(reachable,i+nums[i]);
			else return 0;
		}
		return 1;
    }
```	
45. Jump Game II hard
get the min number of jumps.
using bfs like method to see where can we jump
O(N^2) without queue

```cpp
    int jump(vector<int>& nums) {
        //use bfs
        int step=0,n=nums.size();
        if(n<2) return 0;
        int curmax=0,nextmax=0;
        int i=0;
        while(curmax<n-1)
        {
            step++;
            for(;i<=curmax;i++) nextmax=max(nextmax,i+nums[i]);
            curmax=nextmax;
        }
        return step;
    }

 ```
56	Merge Intervals    		35.6%	Medium	
sort and update the max and min
```cpp
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> ans;
        if(intervals.size()==0) return ans;
        sort(intervals.begin(),intervals.end());
        int cur_min=intervals[0][0],cur_max=intervals[0][1];
        for (auto t: intervals)
        {
            if(t[0]<=cur_max) cur_max=max(cur_max,t[1]);
            else{
                ans.push_back({cur_min,cur_max});
                cur_min=t[0];
                cur_max=t[1];
            }
        }
        ans.push_back({cur_min,cur_max});
        return ans;
    }
```
61	Rotate List    		27.1%	Medium	
shift right by k (rotation). same as move the right k nodes to left.
1-2-3-4-5 shift by 2, 4-5-1-2-3
find the tail and connect with the head and then walk len-k
two passes
```cpp
    ListNode* rotateRight(ListNode* head, int k) {
		//find the tail and
        if(!head) return 0;
		int n=0;
		ListNode* node=head;
		while(node->next) {n++,node=node->next;}
		n++;
		node->next=head;
		k%=n;
		for(int i=1;i<n-k;i++) head=head->next; //counting n-k
		ListNode* next=head->next;
		head->next=0;
		return next;
    }
```	

77	Combinations    		47.7%	Medium	
given 1 to n, return all possible combinations of k numbers out of 1...n
simple backtracking
```cpp
    vector<vector<int>> combine(int n, int k) {
		vector<vector<int>> ans;
		dfs(ans,n,k,1,{});
        return ans;
    }
	void dfs(vector<vector<int>>& ans,int n,int k,int ind,vector<int> t)
	{
		if(k==0) {ans.push_back(t);return;}
		for(int i=ind;i<=n;i++)
		{
			t.push_back(i);
			dfs(ans,n,k-1,i+1,t);
			t.pop_back();
		}
	}
```	
78	Subsets    		52.8%	Medium	
the power set all possible including empty
no duplicates are allowed in solution.
input contains no duplicates
approach: 
[1,2,3]
first we have []
for 1: we add [1] and result is [],[1]
for 2: we append and add [],[1],[2],[1,2]
for 3: we append and add [],[1],[2],[1,2],[3],[1,3], [2,3],[1,2,3]
```cpp
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans;
		ans.push_back({});
		for(int i: nums)
		{
			int sz=ans.size();
			for(int j=0;j<sz;j++) {auto t=ans[j];t.push_back(i),ans.push_back(t);}
		}
		return ans;
    }
```

90	Subsets II    		42.4%	Medium	
input contains duplicate
for example [1,2,2]
we already have [],[1],[2],[1,2] for the first 2
when we process the 3rd 2, we cannot append to [] and [1].
[]
for 1: [], [1]
for 2: [], [1], [2], [1,2]
for 2: [], [1], [2], [1,2],[2,2],[1,2,2] we append to newly added, if different add to the total.

```cpp
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<vector<int>> ans;
		sort(nums.begin(),nums.end());
		ans.push_back({});
		int start=0;
		for(int i=0;i<nums.size();i++)
		{
			if(i && nums[i]==nums[i-1]);// start=1;
			else start=0;
			int sz=ans.size();
			for(int j=start;j<sz;j++) {auto t=ans[j];t.push_back(nums[i]),ans.push_back(t);}
			start=sz;
		}
		return ans;
    }
```
79	Word Search    		31.3%	Medium	
simple dfs
```cpp
    bool exist(vector<vector<char>>& board, string word) {
		if(board.empty()) return 0;
		int m=board.size(),n=board[0].size();
		for(int i=0;i<m;i++)
			for(int j=0;j<n;j++)
				if(board[i][j]==word[0] && dfs(board,word,0,i,j)) return 1;
		return 0;
    }
	bool dfs(vector<vector<char>>& b,string& w,int ind,int i,int j)
	{
		if(i<0 || j<0 ||i>=b.size()||j>=b[0].size()||b[i][j]!=w[ind]) return 0;
        if(ind==w.size()-1) return 1; //base case, need have 0 and 1.
		char c=b[i][j];
		b[i][j]='.';
		bool res=dfs(b,w,ind+1,i-1,j) || dfs(b,w,ind+1,i+1,j) 
            ||dfs(b,w,ind+1,i,j-1) ||dfs(b,w,ind+1,i,j+1);
		b[i][j]=c;
		return res;
	}
```	

109	Convert Sorted List to Binary Search Tree    		40.8%	Medium	
a sorted linked list converts to height balanced BST
use the mid node as the root and do it recursively.
for example 1-2-3-4-5:
find mid node 3, put it as root
left=1-2, right=4-5
do it recursively. for one or zero nodes we can directly process
adding a tail node will make things much easier.
note it does not ask the put 1 and 2 in the same level. that is lowest height requirement.

```cpp
TreeNode* sortedListToBST(ListNode* head) {
    if (!head) return nullptr;
    if (!head->next) return new TreeNode(head->val);
    auto fast = head->next, slow = head; //note fast is next, otherwise incorrect, we want find the previous.
    while (fast->next && fast->next->next) {
        fast = fast->next->next;
        slow = slow->next;
    }
    
    auto mid = slow->next;
    slow->next = nullptr; //break into two lists
    auto root = new TreeNode(mid->val);
    root->left = sortedListToBST(head);
    root->right = sortedListToBST(mid->next);
    return root;
}
```


113	Path Sum II    		40.6%	Medium	
regular dfs 
```cpp
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
		vector<vector<int>> ans;
		if(!root) return ans;
		dfs(root,sum,{},ans);
		return ans;
    }
	void dfs(TreeNode* root,int target,vector<int> t,vector<vector<int>>& ans)
	{
		if(!root) return;
		if(target==root->val) {t.push_back(root->val);ans.push_back(t);return;}
		t.push_back(root->val);
		dfs(root->left,target-root->val,t,ans);
		dfs(root->right,target-root->val,ans);
	}
```	
we pass t as value, and same to the left and right, so we do not need to pop back.


114	Flatten Binary Tree to Linked List    		42.4%	Medium	
this is the preorder traversal and convert the output to list, do it in place
root-> left-> right
we can do a reversed preorder so that we can connect prev which is much simpler!!!

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
```


120	Triangle    		39.4%	Medium	
min path sum from top to bottom, each step you may move to adjacent numbers
(i,j) next position can be (i+1,j) and (i+1,j+1)
reverse will be easier dp[i,j]=min(dp[i+1][j],dp[i+1,j+1])+nums[i][j]
```cpp
    int minimumTotal(vector<vector<int>>& triangle) {
        int m=triangle.size(),n=triangle.back().size();
		//vector<vector<int>> dp(m,vector<int>(n));
		//dp[m-1]=triangles.back();
		for(int i=m-2;i>=0;i--)
		{
			for(int j=0;j<=i;j++)
				triangle[i][j]=min(triangle[i+1][j],triangle[i+1][j+1])+triangle[i][j];
		}
		return triangle[0][0];
    }
```

127	Word Ladder    		24.0%	Medium	
shortest length to transform from beginword to endword, each time change one char
bfs
```cpp
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
		//unordered_set<string,vector<string>> adj;
		unordered_set<string> dict(wordList.begin(),wordList.end());
		unordered_set<string> visited;
		int step=0;
		queue<string> q;
		q.push(beginWord);
		
		while(q.size())
		{
			int sz=q.size();
			for(int i=0;i<sz;i++)
			{
				string s=q.front();q.pop();
				visited.insert(s);
				for(string t: dict)
				{
					if(visited.count(t)) continue;
					if(t==endWord) return step+1;
					if(diff(s,t)==1) q.push(t);
				}
			}
			step++;
		}
        return step;
    }
	int diff(string& s,string &t){
		int m=s.size();
		int ans=0;
		for(int i=0;i<m;i++)
			if(s[i]!=t[i]) ans++;
		return ans;
	}
```	
TLE since adding words to q use O(N^2) replace with O(26N) algorithm will be able to pass


131	Palindrome Partitioning    		40.9%	Medium	
return all possible partition
we need to find all possible partition position and length and use dfs (backtracking) to get the combinations
or focus on backtracking without the cutting and data structure.
```cpp
    vector<vector<string>> partition(string s) {
        int n=s.size();	
		vector<vector<string>> ans;
		dfs(s,0,{},ans);
		return ans;
    }
	void dfs(string& s,int ind,vector<string> t,vector<vector<string>>& ans)
	{
		if(ind==s.size()) {ans.push_back(t);return;}
		for(int i=ind;i<s.size();i++)
		{
			if(ispal(s,ind,i)){
				t.push_back(s.substr(ind,i-ind+1));
				dfs(s,i+1,t,ans);
				t.pop_back();
			}
		}
	}
	bool ispal(string& s,int start,int end)
	{
		while(start<=end)
			if(s[start++]!=s[end--]) return 0;
		return 1;
	}
```	
focus on major problem!!!

133	Clone Graph    		26.5%	Medium	
node to id
and then id to node

also need a traversal of the graph bfs using queue or stack

```cpp
    Node* cloneGraph(Node* node) {
        unordered_map<Node*, int> node2id;
		unordered_map<int,Node*> id2node;
		queue<Node*> q;
		
		q.push(node);
		
		int cnt=0;
		node2id[node]=cnt;
		id2node[cnt++]=new Node;
		while(q.size())
		{
			int sz=q.size();
			for(int i=0;i<sz;i++)
			{
				Node* t=q.front();q.pop();
				for(auto n: t->neighbors){
					if(node2id.count(n)) continue;
					node2id[n]=cnt;
					id2node[cnt++]=new Node;
					q.push(n);
				}
			}
		}
		
		for(auto t: node2id){
			int id=t.second;
			Node* n=t.first;
			Node* newnode=id2node[id];
			for(auto nd: n->neighbors){
				int id1=node2id[nd];
				newnode->neighbors.push_back(id2node[id1]);
			}
		}
		return id2node[0];
	}
```	
134	Gas Station    		33.8%	Medium	
return the gas station index where you can start and complete the whole circle.
each station has gas[i] and cost[i] to get to next station.

similar to the jump game, it is a bfs like problem.
also it is a math problem
sum(gas[i]-cost[i])>0: there is a solution. 
gas=[1,2,3,4,5],cost=[3,4,5,1,2]
gas-cost=[-2,-2,-2,3,3], apparently we shall start at 3, where its prefix sum is always >=0
it is very straightforward to get the O(N^2) solution based on above observations.
further observation: the smallest prefix sum next position is the answer
for above: prefix sum is -2,-4, -6,3,0.
```cpp
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int ans=-1,minsum=INT_MAX;
		int prefix=0;
		for(int i=0;i<gas.size();i++){
			prefix+=gas[i]-cost[i];
			if(prefix<minsum){
				minsum=prefix;
				ans=i+1;
			}
		}
		if(prefix<0) ans=-1;
        if(ans==-1) return -1;
		return ans%gas.size();
    }
```	
pay attention to the return.

139	Word Break    		35.3%	Medium	
given a dictionary of words, a string s can be cut to dictionary words
dp: dp[i] if ith position (actually just after the ith position) can be cut
```cpp
    bool wordBreak(string s, vector<string>& wordDict) {
        int n=s.size();
		unordered_set<string> dict(wordDict.begin(),wordDict.end());
		vector<bool> dp(n+1);
		dp[0]=1;
		for(int i=1;i<=n;i++){
			for(int j=i-1;j>=0;j--)
			{
			if(dp[j] && dict.count(s.substr(j,i-j)) {dp[i]=1;break;}
			}
		}
		return dp[n];
    }
```

140. Word Break II hard
cut and form the new string separated by space. return all possible solutions.
backtracking combined with previous dp
but we need keep its previous position to be able to connect.
```cpp
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        int n=s.size();
		unordered_set<string> dict(wordDict.begin(),wordDict.end());
		vector<vector<int>> dp(n+1);
		dp[0]={1};
		for(int i=1;i<=n;i++){
			for(int j=i-1;j>=0;j--)
			{
				if(dp[j].size() && dict.count(s.substr(j,i-j)) )
					dp[i].push_back(j);
			}
		}
		vector<string> ans;
		dfs(s,n,{},dp,ans);
		return ans;
    }
	void dfs(string& s,int ind,vector<string> t,vector<vector<int>>& dp,vector<string>& ans)
	{
		if(ind==0) {ans.push_back(gen_str(t));return;}
		for(auto i: dp[ind]){
			t.push_back(s.substr(i,ind-i));
			dfs(s,i,t,dp,ans);
			t.pop_back();
		}
	}
	string gen_str(vector<string>& t)
	{
		string s;
		for(auto ss: t) s=ss+" "+s;
        s.pop_back();
		return s;
	}
```	
	

147	Insertion Sort List    		37.3%	Medium	
take a number from input and insert to the sorted list
the head will be changed.
```cpp
    ListNode* insertionSortList(ListNode* head) {
        ListNode* dummy=new ListNode(0);
		//two separate lists
		ListNode *p=head,*prev=dummy;
		while(p)
		{
			ListNode* next=p->next;
			prev=dummy;
			while(prev->next && p->val>prev->next->val) prev=prev->next;
			//insert the node
			ListNode* n=prev->next;
			prev->next=p;
			p->next=n;
			p=next;
		}
		return dummy->next;
    }
```	
O(N^2)

148	Sort List    		35.4%	Medium	
we can use 147 but it needs O(nlogn)
split and merge sort
```cpp
	ListNode* sortList(ListNode* head){
		if(!head || !head->next) return head;
		ListNode *fast=head,*slow=head,*prev=0;
		while(fast && fast->next)
			prev=slow,slow=slow->next,fast=fast->next->next;
		ListNode* right=sortList(slow);
		prev->next=0; //terminate the left
		head=sortList(head);
		return merge(head,right);
	}
	ListNode* merge(ListNode* l1,ListNode* l2)
	{
		if(!l1) return l2;
		if(!l2) return l1;
		ListNode* dummy=new ListNode(0),prev=dummy;
		while(l1 && l2)
		{
			if(l1->val<=l2->val){prev->next=l1;prev=l1,l1=l1->next;}
			else {prev->next=l2;prev=l2;l2=l2->next;}
		}
		if(l1) prev->next=l1;
		if(l2) prev->next=l2;
		return dummy->next;
	}
```
153	Find Minimum in Rotated Sorted Array    		43.0%	Medium	
binary search: no duplicate
if mid<right, then r=m (inclusive)
else l=m+1
but it is very easy to make mistakes. 

```
    int findMin(vector<int>& nums) {
		int l=0,r=nums.size()-1;
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(nums[m]<nums[r]) //it is 
				r=m;
			else l=m+1;
		}
		return nums[l];
    }
```	
156	Binary Tree Upside Down    		50.7%	Medium	
locked
161	One Edit Distance    		31.6%	Medium	
locked
162	Find Peak Element    		41.2%	Medium	
multiple peaks, just return one of it. complexity shall be O(logn)
binary search
good example on binary search
1. answer lies [0, n-1] since both ends are negative infinity with A[l]>A[l-1] and A[r]>A[r+1]
2. how to shrink the range
if A[m]>A[m-1], we keep the left valid if we move l to m.
if A[m]>A[m+1], we keep the right valid if we move r to m.
from previous experience we know if we use two different stands, we may miss the correct range.
A[m]>A[m+1] r=m
else (A[m]<=A[m+1]) l=m+1
3. that is from left side we will reach l=peak so break when l==r
```cpp
    int findPeakElement(vector<int>& nums) {
        int l=0,r=nums.size()-1;
		while(l<r)
		{
			int m=l+(r-l)/2;
			if(nums[m]>nums[m+1]) r=m;
			else l=m+1;
		}
		return l;
	}
```	

163	Missing Ranges    		23.2%	Medium	
locked
173	Binary Search Tree Iterator    		48.6%	Medium	
this is an iterative version of in order traversal
using a stack
```cpp
	stack<TreeNode*> st;
	//TreeNode* root;
    BSTIterator(TreeNode* root) {
        //this->root=root;
        traverse(root);
    }
    
    /** @return the next smallest number */
    int next() {
        auto t=st.top();
		st.pop();
		traverse(t->right);
		return t->val;
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !st.empty();
    }
	
	void traverse(TreeNode* root)
	{
		//if(!root) return;
		while(root)
		{
			st.push(root);
			root=root->left;
		}
	}
```	

187	Repeated DNA Sequences    		36.0%	Medium	
find all the 10 char sequence which appeared more than once
only 4 types of characters. 
direct hash is the way. convert the string to number
A: 0
C: 1
G: 2
T: 3
use a unsigned int can hold the value
```cpp
    vector<string> findRepeatedDnaSequences(string s) {
		unordered_map<long,int> ms;
		unordered_map<char,int> c2i={{'A',0},{'C',1},{'G',2},{'T',3}};
		vector<string> ans;
		long hash=0;
		for(int i=0;i<s.size();i++)
		{
			if(i>=10) 
            {
                //cout<<hash<<endl;
                hash-=c2i[s[i-10]]*pow(10,9);
			    hash=hash*10+c2i[s[i]];
                ms[hash]++;
                if(ms[hash]==2) ans.push_back(s.substr(i-9,10));
            }
			else 
            {
                hash=hash*10+c2i[s[i]];
                if(i==9) ms[hash]++;
            }
		}
		return ans;
    }
```
there are some details need pay attention (especially the sliding window and the substr index)

	
207	Course Schedule    		37.8%	Medium	
check if we can complete these courses
build the relation into a graph. if there is no cycle inside it, we are able to finish.
directed graph using dfs
two kinds :
 a ->b 
    ->c
and
a->b
c->b	

```cpp
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> adj(numCourses);
		for(auto t: prerequisites) adj[t[1]].push_back(t[0]);
		vector<int> visited(numCourses); //0: in expore, -1: not explored, 1: explored 
		for(int i=0;i<numCourses;i++)
			if(!visited[i] && !dfs(adj,i,visited)) return 0;
		return 1;
    }
	bool dfs(vector<vector<int>>& adj,int n,vector<int>& visited)
	{
		if(visited[n]==-1) return 0;
		if(visited[n]==1) return 1;
		visited[n]=-1;
        //cout<<n<<endl;
		for(auto t: adj[n])
		{
			if(!dfs(adj,t,visited)) return 0;
		}
		visited[n]=1;
		return 1;
	}
```	
detect cycle always uses 3 states

210	Course Schedule II    		34.8%	Medium
return the course order. backtracking
we cannot just explore, we need start from source nodes.
source nodes have no prerequisites.
for a course with multiple parents, there is order requirement. bfs is more suitable.

```cpp
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> adj(numCourses);
		vector<int> incoming(numCourses);
		for(auto t: prerequisites) {adj[t[1]].push_back(t[0]);incoming[t[0]]++;}
		vector<int> ans;
		vector<int> visited(numCourses); //0: in expore, -1: not explored, 1: explored 
		for(int i=0;i<numCourses;i++)
			if(!incoming[i] && !dfs(adj,i,visited,ans)) return {};
        if(ans.size()<numCourses) return {};
		return {ans.rbegin(),ans.rend()};
    }
	bool dfs(vector<vector<int>>& adj,int n,vector<int>& visited,vector<int>& ans)
	{
		if(visited[n]==-1) return 0;
		if(visited[n]==1) return 1;
		visited[n]=-1;
		
		for(auto t: adj[n])
		{
			if(!dfs(adj,t,visited,ans))
                return 0;
		}
		visited[n]=1;
		ans.push_back(n);
		return 1;
	}
	
```
two iinteresting points:
-. only success we add it into results. thus we need reverse (to avoid bfs)
-. if one graph has no source, then it is not explored!

630. Course Schedule III
have n courses with (t,d), t is the duration time, d is the closing time. course shall be finished at or before the closing date.
find the max number of courses which can take.
interval and greedy
we always choose the shorter courses which ending earlier
for example
[[100, 200], [200, 1300], [1000, 1250], [2000, 3200]]
we choose [100,200] now time is 100
then we choose [200,1300], now time is 300
then we cannot choose [1000,1250] since closing date cannot meet
then we cannot choose the last one either
we shall choose 0-2-1
the idea:
sort the courses using the ending date
take the course, and update the date
if the current course cannot be taken, pop the largest one from pq (making the ending date smaller)

```cpp
	struct comp{
		bool operator()(const vector<int>& a,const vector<int>& b) {return a[1]<b[1];}
	};
    int scheduleCourse(vector<vector<int>>& courses) {
        priority_queue<int> pq;
		sort(courses.begin(),courses.end(),comp());
		int ans=0;
		int date=0;
		for(auto c: courses)
		{
			pq.push(c[0]);
			date+=c[0];
			if(date>c[1])
			{
				date-=q.top();
				pq.pop();
			}
		}
		return pq.size();
	}
```

208	Implement Trie (Prefix Tree)    		38.5%	Medium	
```cpp
	struct Node
	{
		Node* next[26]; //use pointer is much convenient than using array!
		bool is_leaf;
		Node(bool b=0) {fill(next,next+26,(Node*)0);is_leaf=b;}
	};
	Node* root;
    Trie() {
		root=new Node();	
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
		Node* p=root;
        for(char c: word){
			if(!p->next[c-'a'])
				p->next[c-'a']=new Node;
			p=p->next[c-'a'];
		}
		p->is_leaf=1;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
		Node* p=root;
		for(char c: word){
			if(!p->next[c-'a']) return 0;
			p=p->next[c-'a'];
		}
		return p->is_leaf;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
		Node* p=root;
		for(char c: prefix){
			if(!p->next[c-'a']) return 0;
			p=p->next[c-'a'];
		}
		return 1;
    }
```	
note:
-. node does not contain the char itself
-. need a root and a leaf flags
-. use simple array easier than vector

211	Add and Search Word - Data structure design    		30.3%	Medium	
search need supports . to represent any char.
dfs for search. combined with tree and trie.
trie is also a kind of tree. so recursion is often applied.

```cpp
	struct Node
	{
		Node* next[26]; //use pointer is much convenient than using array!
		bool is_leaf;
		Node(bool b=0) {fill(next,next+26,(Node*)0);is_leaf=b;}
	};
	Node* root;
    WordDictionary() {
        root=new Node;
    }
    
    /** Adds a word into the data structure. */
    void addWord(string word) {
		Node* p=root;
        for(char c: word){
			if(!p->next[c-'a'])
				p->next[c-'a']=new Node;
			p=p->next[c-'a'];
		}
		p->is_leaf=1;        
    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
	bool search(string word) {
        return dfs(word,root);
    }
    bool dfs(string word,Node* root)
    {
        for(int i=0;i<word.size() && root;i++)
        {
            if(word[i]=='.')
            {
                for(int j=0;j<26;j++)
                {
                    if(dfs(word.substr(i+1),root->next[j])) return 1;
                }
                return 0;
            }
            else
            {
                int t=word[i]-'a';
                root=root->next[t];
            }
        }
        return root && root->is_leaf;
    }
```	

209	Minimum Size Subarray Sum    		34.9%	Medium	
Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.	
prefix sum and using hashmap to find previous
```cpp
    int minSubArrayLen(int s, vector<int>& nums) {
        vector<int> prefixsum(nums.size()+1);
		//unordered_map<int,int> mp;//prefix sum vs index
		int ans=INT_MAX;
		for(int i=0;i<nums.size();i++)
		{
			prefixsum[i+1]=prefixsum[i]+nums[i];
			int ind=upper_bound(prefixsum.begin(),prefixsum.end(),prefixsum[i+1]-s)-prefixsum.begin();
			if(ind) ans=min(ans,i-ind);
		}
		return ans==INT_MAX?0:ans;
			
    }
two pointer is better O(n)


	
220	Contains Duplicate III    		19.7%	Medium	
see 217, contain duplicate, using hashmap
see 219, contain duplicate II, value the same and index difference <=k
this problem needs |num[i]-num[j]|<=t && |i-j|<=k
if we sort it we can find the number in the range
```cpp
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
		//sort with index
        if(k<0 || t<0) return 0;
		set<pair<long,int>> ms; //value vs index
		for(int i=0;i<nums.size();i++) ms.insert({nums[i],i});
		for(auto it=ms.begin();it!=ms.end();it++) {
			long target=it->first-t; //need find >=target
			auto it1=ms.lower_bound({target,0});
			if(it1!=ms.end() && it1!=it && abs(it1->second-it->second)<=k) return 1;
		}
		return 0;
    }
```
- corner case, input is negative 
- use long to avoid overflow

another approach: do not sort (so we keep the index not changed) and then use a sliding window to find.
	
222	Count Complete Tree Nodes    		33.6%	Medium	
complete binary tree
recursive, judge the height of left and right
```cpp
    int countNodes(TreeNode* root) {
		if(!root) return 0;
        int h1=depth(root->left),h2=depth(root->right);
		if(h1>h2) return (1<<h2)+countNodes(root->left); //left>right, right is full
		return (1<<h1)+countNodes(root->right); //left is full
    }
	int depth(TreeNode* root)
	{
		if(!root) return 0;
		return 1+max(depth(root->left),depth(root->right));
	}
```	


227	Basic Calculator II    		33.4%	Medium	
+-*/
add + at the beginning and end to avoid extra work
when we see a + -,  we complete previous results, and we read next number into template
when we see a * /, we perform temp * next.

```cpp
int calculate(string s) {
    istringstream in('+' + s + '+');
    long long total = 0, term = 0, n;
    char op;
    while (in >> op) {
        if (op == '+' or op == '-') {
            total += term;
            in >> term;
            term *= 44 - op;
        } else {
            in >> n;
            if (op == '*')
                term *= n;
            else
                term /= n;
        }
    }
    return total;
}
```

expression with priorities can always be well handled by stack
3+2*2
we put 3 into stack
```cpp
	int calculate(string s)
	{
		stringstream ss('+'+s+'+');
		stack<int> st;
		int num=0;
		char op;
		while(ss>>op)
		{
			num=0;
			if(ss>>num)
			{
				if(op=='+') st.push(num);
				else if(op=='-') st.push(-num);
				else if(op=='*') {int t=st.top();st.pop();st.push(t*num);}
				else {int t=st.top();st.pop();st.push(t/num);}
			}
		}
		int ans=0;
		while(st.size()) ans+=st.top(),st.pop();
		return ans;
	}
```
stack is a better approach since it is better for understanding.			
similar see 1006 clumsy factorial using stack


228	Summary Ranges    		35.9%	Medium	
merge into intervals, input is sorted array
```cpp
   vector<string> summaryRanges(vector<int>& nums) {
    const int size_n = nums.size();
    vector<string> res;
    if ( 0 == size_n) return res;
    for (int i = 0; i < size_n;) {
        int start = i, end = i;
        while (end + 1 < size_n && nums[end+1] == nums[end] + 1) end++;
        if (end > start) res.push_back(to_string(nums[start]) + "->" + to_string(nums[end]));
        else res.push_back(to_string(nums[start]));
        i = end+1;
    }
    return res;
}
```
a bit tricky to handle the last interval.
			
229	Majority Element II    		32.1%	Medium	
similar 169, more than 1/2
lc229, find all elements more than 1/3
using voting algorithm
direct method: counting using hashmap
voting method O(n) two pass, second pass to verify the candidate.
```cpp
  vector<int> majorityElement(vector<int> &a) {
    int y = 0, z = 1, cy = 0, cz = 0;
    for (auto x: a) {
      if (x == y) cy++;
      else if (x == z) cz++;
      else if (! cy) y = x, cy = 1;
      else if (! cz) z = x, cz = 1;
      else cy--, cz--;
    }
    cy = cz = 0;
    for (auto x: a)
      if (x == y) cy++;
      else if (x == z) cz++;
    vector<int> r;
    if (cy > a.size()/3) r.push_back(y);
    if (cz > a.size()/3) r.push_back(z);
    return r;
  }
```  
236	Lowest Common Ancestor of a Binary Tree    		37.3%	Medium	
using bits to indicate which one is found.
if found both on left, go to left (sub problem), also same for right
if on both side, then root is the answer

```cpp
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        int status=dfs(root,p,q);
		if(root==p || root==q) return root;
		int s1=dfs(root->left,p,q);
		int s2=dfs(root->right,p,q);
		if(s1 && s2) return root;
		if(!s1) return lowestCommonAncestor(root->right,p,q);
		return lowestCommonAncestor(root->left,p,q);
    }
	int dfs(TreeNode* root,TreeNode*p,TreeNode* q)
	{
		if(!root) return 0;
		int ans=0;
		if(root==p) ans|=1;
		if(root==q) ans|=2;
		if(ans==3) return ans;
		ans|=dfs(root->left,p,q);
		ans|=dfs(root->right,p,q);
		return ans;
	}
```
241	Different Ways to Add Parentheses    		49.9%	Medium	
first recursive, divide and conquer

```cpp
    vector<int> diffWaysToCompute(string input) {
        vector<int> result;
        int size = input.size();
        for (int i = 0; i < size; i++) {
            char cur = input[i];
            if (cur == '+' || cur == '-' || cur == '*') {
                // Split input string into two parts and solve them recursively
                vector<int> result1 = diffWaysToCompute(input.substr(0, i));
                vector<int> result2 = diffWaysToCompute(input.substr(i+1));
                for (auto n1 : result1) {
                    for (auto n2 : result2) {
                        if (cur == '+')
                            result.push_back(n1 + n2);
                        else if (cur == '-')
                            result.push_back(n1 - n2);
                        else
                            result.push_back(n1 * n2);    
                    }
                }
            }
        }
        // if the input string contains only number
        if (result.empty())
            result.push_back(atoi(input.c_str()));
        return result;
    }
};
```

add memoization
```cpp
	vector<int> diffWaysToCompute(string input) {
		unordered_map<string, vector<int>> dpMap;
		return computeWithDP(input, dpMap);
	}

	vector<int> computeWithDP(string input, unordered_map<string, vector<int>> &dpMap) {
		vector<int> result;
		int size = input.size();
		for (int i = 0; i < size; i++) {
			char cur = input[i];
			if (cur == '+' || cur == '-' || cur == '*') {
				// Split input string into two parts and solve them recursively
				vector<int> result1, result2;
				string substr = input.substr(0, i);
				// check if dpMap has the result for substr
				if (dpMap.find(substr) != dpMap.end())
					result1 = dpMap[substr];
				else
					result1 = computeWithDP(substr, dpMap);

				substr = input.substr(i + 1);
				if (dpMap.find(substr) != dpMap.end())
					result2 = dpMap[substr];
				else
					result2 = computeWithDP(substr, dpMap);
				
				for (auto n1 : result1) {
					for (auto n2 : result2) {
						if (cur == '+')
							result.push_back(n1 + n2);
						else if (cur == '-')
							result.push_back(n1 - n2);
						else
							result.push_back(n1 * n2);
					}
				}
			}
		}
		// if the input string contains only number
		if (result.empty())
			result.push_back(atoi(input.c_str()));
		// save to dpMap
		dpMap[input] = result;
		return result;
	}
```
	

260	Single Number III    		57.0%	Medium	
similar 136, one element appears once other appear twice (using xor)
similar 137, one element appears once other appear three times (using true table)
this problem is to apply xor three times.

264	Ugly Number II    		36.4%	Medium	
dp simple

274	H-Index    		34.6%	Medium	
275	H-Index II    		35.5%	Medium	


285	Inorder Successor in BST    		34.7%	Medium	
286	Walls and Gates    		49.5%	Medium	
287	Find the Duplicate Number    		49.6%	Medium	
n+1 integers from 1 to n. find the duplicate one.
this type of problem often uses list cycle or change to negative to mark seen
if we consider the A[i] as the next index, so all index are valid and therefore there is a cycle
The important: all values are from 1 to n and there is no 0, so A[0] is not part of any cycle.
except A[0] is the entry point and it is the duplicate number
see 142. Linked list cycle II

288	Unique Word Abbreviation    		20.0%	Medium	
289	Game of Life    		45.4%	Medium	
cell depends on its 8 neighbors. 
live neighbor cells <2, it dies
[2,3], lives
>3: it dies
==3: dead cell become live
approach 1: use a copy and it is straighforward
approach 2: in place (use high bit)


294	Flip Game II    		48.3%	Medium	
298	Binary Tree Longest Consecutive Sequence    		44.0%	Medium	
299	Bulls and Cows    		39.4%	Medium	
300	Longest Increasing Subsequence    		40.8%	Medium	
this is a vrey typical dp problem
dp[i]=max(dp[i],dp[j]+1) if A[i]>A[j]
```cpp
    int lengthOfLIS(vector<int>& nums) {
		int n=nums.size();
        if(n==0) return 0;
		vector<int> dp(n,1);
		for(int i=1;i<n;i++)
		{
			for(int j=i-1;j>=0;j--)
				if(nums[i]>nums[j]) dp[i]=max(dp[i],dp[j]+1);
		}
		return *max_element(dp.begin(),dp.end());
    }
```
similar problems:
increasing triplet subsequences
max length of pair chain
number of longest increasing subsequences
russian doll envelopes
	
304	Range Sum Query 2D - Immutable    		32.3%	Medium	
306	Additive Number    		28.3%	Medium	

307	Range Sum Query - Mutable    		28.6%	Medium	
309	Best Time to Buy and Sell Stock with Cooldown    		44.0%	Medium	
310	Minimum Height Trees    		30.1%	Medium	
311	Sparse Matrix Multiplication    		56.3%	Medium	


318	Maximum Product of Word Lengths    		48.5%	Medium	
319	Bulb Switcher    		43.9%	Medium	
320	Generalized Abbreviation    		48.8%	Medium	
322	Coin Change    		30.4%	Medium	
323	Number of Connected Components in an Undirected Graph    		52.0%	Medium	
324	Wiggle Sort II    		28.0%	Medium	
325	Maximum Size Subarray Sum Equals k    		44.7%	Medium	
328	Odd Even Linked List    		49.1%	Medium	
331	Verify Preorder Serialization of a Binary Tree    		38.6%	Medium	
332	Reconstruct Itinerary    		31.4%	Medium	
333	Largest BST Subtree    		33.0%	Medium	
334	Increasing Triplet Subsequence    		39.6%	Medium	
337	House Robber III    		48.0%	Medium	
338	Counting Bits    		64.7%	Medium	
341	Flatten Nested List Iterator    		47.9%	Medium	
343	Integer Break    		47.8%	Medium	
347	Top K Frequent Elements    		54.9%	Medium	
348	Design Tic-Tac-Toe    		49.7%	Medium	
351	Android Unlock Patterns    		45.8%	Medium	
353	Design Snake Game    		30.5%	Medium	
355	Design Twitter    		27.3%	Medium	
356	Line Reflection    		30.9%	Medium	
357	Count Numbers with Unique Digits    		46.9%	Medium	
360	Sort Transformed Array    		46.8%	Medium	
361	Bomb Enemy    		43.3%	Medium	
362	Design Hit Counter    		59.0%	Medium	
364	Nested List Weight Sum II    		57.7%	Medium	
365	Water and Jug Problem    		28.9%	Medium	
366	Find Leaves of Binary Tree    		65.7%	Medium	
368	Largest Divisible Subset    		34.8%	Medium	
369	Plus One Linked List    		56.2%	Medium	
370	Range Addition    		60.5%	Medium	
372	Super Pow    		35.6%	Medium	
373	Find K Pairs with Smallest Sums    		33.8%	Medium	
375	Guess Number Higher or Lower II    		37.7%	Medium	
376	Wiggle Subsequence    		37.4%	Medium	
377	Combination Sum IV    		43.7%	Medium	
378	Kth Smallest Element in a Sorted Matrix    		49.4%	Medium	
379	Design Phone Directory    		41.2%	Medium	
380	Insert Delete GetRandom O(1)    		42.8%	Medium	
382	Linked List Random Node    		49.3%	Medium	
384	Shuffle an Array    		50.0%	Medium	
385	Mini Parser    		31.8%	Medium	
386	Lexicographical Numbers    		46.1%	Medium	
388	Longest Absolute File Path    		39.2%	Medium	
390	Elimination Game    		43.4%	Medium	
392	Is Subsequence    		46.8%	Medium	
393	UTF-8 Validation    		35.9%	Medium	
394	Decode String    		44.9%	Medium	
395	Longest Substring with At Least K Repeating Characters    		38.6%	Medium	
396	Rotate Function    		35.1%	Medium	
397	Integer Replacement    		31.4%	Medium	
398	Random Pick Index    		49.9%	Medium	
399	Evaluate Division    		47.5%	Medium	
402	Remove K Digits    		26.6%	Medium	
406	Queue Reconstruction by Height    		59.8%	Medium	
413	Arithmetic Slices    		55.8%	Medium	
416	Partition Equal Subset Sum    		40.6%	Medium	
417	Pacific Atlantic Water Flow    		37.4%	Medium	
418	Sentence Screen Fitting    		31.1%	Medium	
419	Battleships in a Board    		65.7%	Medium	
421	Maximum XOR of Two Numbers in an Array    		51.0%	Medium	
423	Reconstruct Original Digits from English    		45.6%	Medium	
424	Longest Repeating Character Replacement    		44.1%	Medium	
426	Convert Binary Search Tree to Sorted Doubly Linked List    		52.0%	Medium	
430	Flatten a Multilevel Doubly Linked List    		42.2%	Medium	
433	Minimum Genetic Mutation    		38.1%	Medium	
435	Non-overlapping Intervals    		41.6%	Medium	
436	Find Right Interval    		42.8%	Medium	
439	Ternary Expression Parser    		53.6%	Medium	
442	Find All Duplicates in an Array    		60.9%	Medium	
444	Sequence Reconstruction    		20.3%	Medium	
445	Add Two Numbers II    		50.1%	Medium	
449	Serialize and Deserialize BST    		47.1%	Medium	
450	Delete Node in a BST    		40.0%	Medium	
451	Sort Characters By Frequency    		56.0%	Medium	
452	Minimum Number of Arrows to Burst Balloons    		46.4%	Medium	
454	4Sum II    		50.6%	Medium	
456	132 Pattern    		27.4%	Medium	
457	Circular Array Loop    		27.6%	Medium	
462	Minimum Moves to Equal Array Elements II    		52.4%	Medium	
464	Can I Win    		27.2%	Medium	
467	Unique Substrings in Wraparound String    		33.9%	Medium	
468	Validate IP Address    		21.3%	Medium	
469	Convex Polygon    		35.4%	Medium	
470	Implement Rand10() Using Rand7()    		44.8%	Medium	
473	Matchsticks to Square    		35.9%	Medium	
474	Ones and Zeroes    		39.7%	Medium	
477	Total Hamming Distance    		48.9%	Medium	
478	Generate Random Point in a Circle    		36.5%	Medium	
481	Magical String    		46.1%	Medium	
484	Find Permutation    		57.5%	Medium	
486	Predict the Winner    		46.8%	Medium	
487	Max Consecutive Ones II    		46.6%	Medium	
490	The Maze    		47.4%	Medium	
491	Increasing Subsequences    		42.0%	Medium	
494	Target Sum    		45.2%	Medium	
495	Teemo Attacking    		52.2%	Medium	
497	Random Point in Non-overlapping Rectangles    		35.8%	Medium	
498	Diagonal Traverse    		45.3%	Medium	
503	Next Greater Element II    		51.1%	Medium	
505	The Maze II    		43.8%	Medium	
508	Most Frequent Subtree Sum    		54.5%	Medium	
510	Inorder Successor in BST II    		52.7%	Medium	
513	Find Bottom Left Tree Value    		58.4%	Medium	
515	Find Largest Value in Each Tree Row    		57.8%	Medium	
516	Longest Palindromic Subsequence    		46.7%	Medium	
518	Coin Change 2    		42.7%	Medium	
519	Random Flip Matrix    		33.1%	Medium
522	Longest Uncommon Subsequence II    		32.9%	Medium	
523	Continuous Subarray Sum    		24.2%	Medium	
524	Longest Word in Dictionary through Deleting    		45.7%	Medium	
525	Contiguous Array    		42.7%	Medium	
526	Beautiful Arrangement    		54.6%	Medium	
528	Random Pick with Weight    		42.8%	Medium	
529	Minesweeper    		52.8%	Medium	
531	Lonely Pixel I    		57.5%	Medium	
533	Lonely Pixel II    		46.1%	Medium	
535	Encode and Decode TinyURL    		76.7%	Medium	
536	Construct Binary Tree from String    		44.9%	Medium	
537	Complex Number Multiplication    		65.5%	Medium	
539	Minimum Time Difference    		47.9%	Medium	
540	Single Element in a Sorted Array    		57.5%	Medium	
542	01 Matrix    		35.6%	Medium	
544	Output Contest Matches    		73.5%	Medium	
545	Boundary of Binary Tree    		35.0%	Medium	
547	Friend Circles    		53.6%	Medium	
548	Split Array with Equal Sum    		43.2%	Medium	
549	Binary Tree Longest Consecutive Sequence II    		44.3%	Medium	
553	Optimal Division    		55.4%	Medium	
554	Brick Wall    		47.6%	Medium	
555	Split Concatenated Strings    		39.8%	Medium	
556	Next Greater Element III    		30.0%	Medium	
560	Subarray Sum Equals K    		42.2%	Medium	
562	Longest Line of Consecutive One in Matrix    		43.7%	Medium	
565	Array Nesting    		52.5%	Medium	
567	Permutation in String    		38.4%	Medium	
573	Squirrel Simulation    		53.6%	Medium	
576	Out of Boundary Paths    		32.0%	Medium	
582	Kill Process    		56.2%	Medium	
583	Delete Operation for Two Strings    		44.8%	Medium	
592	Fraction Addition and Subtraction    		47.0%	Medium	
593	Valid Square    		40.5%	Medium	
609	Find Duplicate File in System    		55.1%	Medium	
611	Valid Triangle Number    		45.1%	Medium	
616	Add Bold Tag in String    		39.1%	Medium	
621	Task Scheduler    		45.5%	Medium	
622	Design Circular Queue    		39.1%	Medium	
623	Add One Row to Tree    		47.3%	Medium	
625	Minimum Factorization    		31.9%	Medium	
634	Find the Derangement of An Array    		37.6%	Medium	
635	Design Log Storage System    		54.0%	Medium	
636	Exclusive Time of Functions    		48.7%	Medium	
638	Shopping Offers    		48.6%	Medium	
640	Solve the Equation    		40.3%	Medium	
641	Design Circular Deque    		49.2%	Medium	
646	Maximum Length of Pair Chain    		48.7%	Medium	
647	Palindromic Substrings    		56.9%	Medium	
648	Replace Words    		51.8%	Medium	
649	Dota2 Senate    		37.6%	Medium	
650	2 Keys Keyboard    		46.6%	Medium	
651	4 Keys Keyboard    		50.5%	Medium	
652	Find Duplicate Subtrees    		45.5%	Medium	
654	Maximum Binary Tree    		76.0%	Medium	
655	Print Binary Tree    		51.9%	Medium	
658	Find K Closest Elements    		38.0%	Medium	
659	Split Array into Consecutive Subsequences    		40.7%	Medium	
662	Maximum Width of Binary Tree    		39.7%	Medium	
663	Equal Tree Partition    		38.0%	Medium	
666	Path Sum IV    		52.2%	Medium	
667	Beautiful Arrangement II    		51.9%	Medium	
670	Maximum Swap    		39.6%	Medium	
672	Bulb Switcher II    		50.0%	Medium	
673	Number of Longest Increasing Subsequence    		33.6%	Medium	
676	Implement Magic Dictionary    		51.7%	Medium	
677	Map Sum Pairs    		51.5%	Medium	
678	Valid Parenthesis String    		32.7%	Medium	
681	Next Closest Time    		42.6%	Medium	
684	Redundant Connection    		51.8%	Medium	
688	Knight Probability in Chessboard    		44.3%	Medium	
692	Top K Frequent Words    		45.7%	Medium	
694	Number of Distinct Islands    		51.2%	Medium	
695	Max Area of Island    		57.2%	Medium	
698	Partition to K Equal Sum Subsets    		42.3%	Medium	
701	Insert into a Binary Search Tree    		75.7%	Medium	
702	Search in a Sorted Array of Unknown Size    		58.7%	Medium	
708	Insert into a Cyclic Sorted List    		28.9%	Medium	
712	Minimum ASCII Delete Sum for Two Strings    		54.5%	Medium	
713	Subarray Product Less Than K    		36.7%	Medium	
714	Best Time to Buy and Sell Stock with Transaction Fee    		50.4%	Medium	
718	Maximum Length of Repeated Subarray    		46.0%	Medium	
721	Accounts Merge    		40.4%	Medium	
722	Remove Comments    		31.2%	Medium	
723	Candy Crush    		63.2%	Medium	
725	Split Linked List in Parts    		48.9%	Medium	
729	My Calendar I    		47.3%	Medium	
731	My Calendar II    		44.3%	Medium	
735	Asteroid Collision    		38.4%	Medium	
737	Sentence Similarity II    		43.3%	Medium	
738	Monotone Increasing Digits    		41.8%	Medium	
739	Daily Temperatures    		60.0%	Medium	
740	Delete and Earn    		45.9%	Medium	
742	Closest Leaf in a Binary Tree    		38.8%	Medium	
743	Network Delay Time    		42.0%	Medium	
750	Number Of Corner Rectangles    		64.5%	Medium	
752	Open the Lock    		45.9%	Medium	
755	Pour Water    		40.4%	Medium	
756	Pyramid Transition Matrix    		51.5%	Medium	
763	Partition Labels    		70.3%	Medium	
764	Largest Plus Sign    		43.3%	Medium	
767	Reorganize String    		42.2%	Medium	
769	Max Chunks To Make Sorted    		51.6%	Medium	
775	Global and Local Inversions    		38.8%	Medium	
776	Split BST    		52.5%	Medium	
777	Swap Adjacent in LR String    		33.3%	Medium	
779	K-th Symbol in Grammar    		37.5%	Medium	
781	Rabbits in Forest    		51.7%	Medium	
785	Is Graph Bipartite?    		43.3%	Medium	
787	Cheapest Flights Within K Stops    		34.8%	Medium	
789	Escape The Ghosts    		55.2%	Medium	
790	Domino and Tromino Tiling    		35.7%	Medium	
791	Custom Sort String    		61.9%	Medium	
792	Number of Matching Subsequences    		42.9%	Medium	
794	Valid Tic-Tac-Toe State    		29.5%	Medium	
795	Number of Subarrays with Bounded Maximum    		43.2%	Medium	
797	All Paths From Source to Target    		70.5%	Medium	
799	Champagne Tower    		34.0%	Medium	
801	Minimum Swaps To Make Sequences Increasing    		34.6%	Medium	
802	Find Eventual Safe States    		43.6%	Medium	
807	Max Increase to Keep City Skyline    		81.4%	Medium	
808	Soup Servings    		37.1%	Medium	
809	Expressive Words    		43.5%	Medium	
813	Largest Sum of Averages    		45.1%	Medium	
814	Binary Tree Pruning    		70.9%	Medium	
816	Ambiguous Coordinates    		44.1%	Medium	
817	Linked List Components    		54.6%	Medium	
820	Short Encoding of Words    		47.1%	Medium	
822	Card Flipping Game    		40.3%	Medium	
823	Binary Trees With Factors    		32.4%	Medium	
825	Friends Of Appropriate Ages    		36.2%	Medium	
826	Most Profit Assigning Work    		35.6%	Medium	
831	Masking Personal Information    		42.1%	Medium	
833	Find And Replace in String    		46.1%	Medium	
835	Image Overlap    		52.3%	Medium	
837	New 21 Game    		31.3%	Medium	
838	Push Dominoes    		43.7%	Medium	
841	Keys and Rooms    		60.2%	Medium	
842	Split Array into Fibonacci Sequence    		34.6%	Medium	
845	Longest Mountain in Array    		34.2%	Medium	
846	Hand of Straights    		49.1%	Medium	
848	Shifting Letters    		40.6%	Medium	
851	Loud and Rich    		47.6%	Medium	
853	Car Fleet    		39.6%	Medium	
855	Exam Room    		38.2%	Medium	
856	Score of Parentheses    		56.1%	Medium	
858	Mirror Reflection    		51.9%	Medium	
861	Score After Flipping Matrix    		69.5%	Medium	
863	All Nodes Distance K in Binary Tree    		47.2%	Medium	
865	Smallest Subtree with all the Deepest Nodes    		55.6%	Medium	
866	Prime Palindrome    		20.1%	Medium	
869	Reordered Power of 2    		50.9%	Medium	
870	Advantage Shuffle    		42.4%	Medium	
873	Length of Longest Fibonacci Subsequence    		46.1%	Medium	
875	Koko Eating Bananas    		46.1%	Medium	
877	Stone Game    		61.3%	Medium	
880	Decoded String at Index    		23.0%	Medium	
881	Boats to Save People    		43.7%	Medium	
885	Spiral Matrix III    		64.0%	Medium	
886	Possible Bipartition    		40.6%	Medium	
889	Construct Binary Tree from Preorder and Postorder Traversal    		59.8%	Medium	
890	Find and Replace Pattern    		71.1%	Medium	
894	All Possible Full Binary Trees    		70.6%	Medium	
898	Bitwise ORs of Subarrays    		33.9%	Medium	
900	RLE Iterator    		50.1%	Medium	
901	Online Stock Span    		48.8%	Medium	
904	Fruit Into Baskets    		41.4%	Medium	
907	Sum of Subarray Minimums    		27.2%	Medium	
909	Snakes and Ladders    		33.2%	Medium	
910	Smallest Range II    		23.6%	Medium	
911	Online Election    		46.7%	Medium	
912	Sort an Array    		63.8%	Medium	
915	Partition Array into Disjoint Intervals    		43.2%	Medium	
916	Word Subsets    		45.2%	Medium	
918	Maximum Sum Circular Subarray    		31.7%	Medium	
919	Complete Binary Tree Inserter    		55.0%	Medium	
921	Minimum Add to Make Parentheses Valid    		70.0%	Medium	
923	3Sum With Multiplicity    		33.4%	Medium	
926	Flip String to Monotone Increasing    		49.4%	Medium	
930	Binary Subarrays With Sum    		37.9%	Medium	
931	Minimum Falling Path Sum    		58.8%	Medium	
932	Beautiful Array    		53.0%	Medium	
934	Shortest Bridge    		43.8%	Medium	
935	Knight Dialer    		40.7%	Medium	
939	Minimum Area Rectangle    		50.4%	Medium	
945	Minimum Increment to Make Array Unique    		42.9%	Medium	
946	Validate Stack Sequences    		57.7%	Medium	
947	Most Stones Removed with Same Row or Column    		54.1%	Medium	
948	Bag of Tokens    		39.3%	Medium	
950	Reveal Cards In Increasing Order    		71.9%	Medium	
951	Flip Equivalent Binary Trees    		65.3%	Medium	
954	Array of Doubled Pairs    		34.3%	Medium	
955	Delete Columns to Make Sorted II    		31.3%	Medium	
957	Prison Cells After N Days    		38.0%	Medium	
958	Check Completeness of a Binary Tree    		46.9%	Medium	
959	Regions Cut By Slashes    		62.3%	Medium	
962	Maximum Width Ramp    		41.4%	Medium	
963	Minimum Area Rectangle II    		44.2%	Medium	
966	Vowel Spellchecker    		41.5%	Medium	
967	Numbers With Same Consecutive Differences    		36.8%	Medium	
969	Pancake Sorting    		62.3%	Medium	
971	Flip Binary Tree To Match Preorder Traversal    		42.7%	Medium	
973	K Closest Points to Origin    		62.4%	Medium	
974	Subarray Sums Divisible by K    		44.6%	Medium	
978	Longest Turbulent Subarray    		45.4%	Medium	
979	Distribute Coins in Binary Tree    		66.9%	Medium	
981	Time Based Key-Value Store    		51.1%	Medium	
983	Minimum Cost For Tickets    		57.2%	Medium	
984	String Without AAA or BBB    		33.5%	Medium	
986	Interval List Intersections    		62.7%	Medium	
987	Vertical Order Traversal of a Binary Tree    		32.4%	Medium	
988	Smallest String Starting From Leaf    		46.5%	Medium	
990	Satisfiability of Equality Equations    		39.7%	Medium	
991	Broken Calculator    		40.0%	Medium	
998	Maximum Binary Tree II    		61.8%	Medium	
1003	Check If Word Is Valid After Substitutions    		51.9%	Medium	
1004	Max Consecutive Ones III    		53.2%	Medium	
1006	Clumsy Factorial    		54.1%	Medium	
1007	Minimum Domino Rotations For Equal Row    		47.6%	Medium	
1008	Construct Binary Search Tree from Preorder Traversal    		72.8%	Medium	
1011	Capacity To Ship Packages Within D Days    		52.8%	Medium	
1014	Best Sightseeing Pair    		48.3%	Medium	
1015	Smallest Integer Divisible by K    		27.9%	Medium	
1016	Binary String With Substrings Representing 1 To N    		62.0%	Medium	
1017	Convert to Base -2    		56.0%	Medium	
1019	Next Greater Node In Linked List    		56.3%	Medium	
1020	Number of Enclaves    		54.9%	Medium	
1023	Camelcase Matching    		56.9%	Medium	
1024	Video Stitching    		47.0%	Medium	
1026	Maximum Difference Between Node and Ancestor    		59.0%	Medium	
1027	Longest Arithmetic Sequence    		46.4%	Medium	
1031	Maximum Sum of Two Non-Overlapping Subarrays    		55.4%	Medium	
1034	Coloring A Border    		42.4%	Medium	
1035	Uncrossed Lines    		51.2%	Medium	
1038	Binary Search Tree to Greater Sum Tree    		80.2%	Medium	
1039	Minimum Score Triangulation of Polygon    		40.2%	Medium	
1040	Moving Stones Until Consecutive II    		45.8%	Medium	
1043	Partition Array for Maximum Sum    		61.3%	Medium	
1048	Longest String Chain    		46.3%	Medium	
1049	Last Stone Weight II    		38.0%	Medium	
1052	Grumpy Bookstore Owner    51.3%	Medium	

1054	Distant Barcodes   37.5%	Medium	

medium to hard ****
89	Gray Code    		45.8%	Medium	
n bits, successive values differs in only one bit.
approach 1:
n=1: 0->1
n=2: 00->01->11->10 (add 0 to previous and and 1 to reverse previous)
n=3: 000->001->011->010->110->111->101->100 (add 0 to previous and and 1 to reverse previous)
approach 2:  backtracking
from MSB to LSB recursively solve the problem.
first half MSB=0 and second half MSB=1

```cpp
	vector<int> grayCode(int n) {
		bitset<32> bits; //0
		vector<int> ans;
		dfs(ans,bits,n);
		return ans;
	}
	void dfs(vector<int>& ans,bitset<32>& bits,int n) {
		if(n==0) {ans.push_back(bits.to_ulong());return;}
		dfs(ans,bits,n-1); 
		bits.flip(n-1); //bit n-1 is the MSB. LSB is 0.
		dfs(ans,bits,n-1);
	}
```
This is a very concise and straight forward backtrack approach, and what is really does is this. 
Say the question wants the number of bits to be k. 
This means that the k-1th bit from the right can have two values. 
So we start with the k-1th bit from the right. We first do not do anything to it. 
See we have already initialized a 32-bit bitset (which is essentially a boolean array) with all 0s. 
Without doing anything to this bit, we call the utils function to do the next task with k+1 that is to do the next bit after it. 
It goes on till we reach the zeroth bit. We now have bitset containing the bits we selected at those positions. 
NOW, we backtrack. Next up after calling utils is, you see, we do a flip. So we flip the 0 and make it a 1 or vice versa and we do the same operation again. 
We choose. We explore. We unchoose. CLASSIC BACKTRACKING! 

81	Search in Rotated Sorted Array II    		32.6%	Medium	
binary search with duplicates inside.
pivot can be anywhere from 0 to n-1
[2,5,6,0,0,1,2]
target>num[mid]: it is in the left
target<num[mid]: it is in the right
= we found it.

```cpp
    bool search(vector<int>& nums, int target) {
        
    }
```	

146. LRU Cache
leastly recently used cache
O(1)
get: get the value for the key
put: change or insert the new key value
hashmap for read O(1)
hashmap key vs list iterator
list of key-value pairs
when read, move the node to front
when put: move the node to front
evict the node in the back
```cpp
	unordered_map<int,list<pair<int,int>>::iterator> mp;
	list<pair<int,int>> cache;
	int sz;
    LRUCache(int capacity) {
        sz=capacity;
    }
    
    int get(int key) {
		if(mp.count(key))
		{
			auto it=mp[key];
			cache.splice(cache.begin(),cache,it);
			mp[key]=cache.begin();
            return it->second;
		}
		return -1;
    }
    
    void put(int key, int value) {
        if(mp.count(key)){
			auto it=mp[key];
			it->second=value;
			cache.splice(cache.begin(),cache,it);
			mp[key]=cache.begin();
		}
		else
		{
			if(cache.size()==sz){
				auto t=cache.back();
				cache.pop_back();
				mp.erase(t.first);
			}
			pair<int,int> p={key,value};
			cache.insert(cache.begin(),p);
			mp[key]=cache.begin();
		}
    }
```	

460. LFU Cache
Least frequently used cache
when frequency is the same, the least recently used key is evicted.
use 3 hashmaps
key to value,freq
key to list iterator
freq to key list.

```cpp
class LFUCache {
    int cap;
    int size;
    int minFreq;
    unordered_map<int, pair<int, int>> m; //key to {value,freq};
    unordered_map<int, list<int>::iterator> mIter; //key to list iterator;
    unordered_map<int, list<int>>  fm;  //freq to key list;
public:
    LFUCache(int capacity) {
        cap=capacity;
        size=0;
    }
    
    int get(int key) {
        if(m.count(key)==0) return -1;
        
        fm[m[key].second].erase(mIter[key]);
        m[key].second++;
        fm[m[key].second].push_back(key);
        mIter[key]=--fm[m[key].second].end();
        
        if(fm[minFreq].size()==0 ) 
              minFreq++;
        
        return m[key].first;
    }
    
   void set(int key, int value) {
        if(cap<=0) return;
        
        int storedValue=get(key);
        if(storedValue!=-1)
        {
            m[key].first=value;
            return;
        }
        
        if(size>=cap )
        {
            m.erase( fm[minFreq].front() );
            mIter.erase( fm[minFreq].front() );
            fm[minFreq].pop_front();
            size--;
        }
        
        m[key]={value, 1};
        fm[1].push_back(key);
        mIter[key]=--fm[1].end();
        minFreq=1;
        size++;
    }
};
```

221	Maximal Square    		33.0%	Medium	
finding the max square with all 1s<br/>
similar to histogram but we uses dp<br/>
keep updating the height on each row.<br/>
keep updating its max range at j (left and right) using the height as the min height (this could be obtained using left max and right min dp process)<br/>

```cpp
    int maximalSquare(vector<vector<char>>& matrix) {
        if(matrix.size()==0) return 0;
        int m=matrix.size(),n=matrix[0].size();
        vector<int> left(n),right(n,n),height(n);
        int max_area=0;
        for(int i=0;i<m;i++)
        {
            int curr_left=0,curr_right=n;
            for(int j=0;j<n;j++) height[j]=(matrix[i][j]=='1')?(height[j]+1):0;
            for(int j=0;j<n;j++)
            {
                if(matrix[i][j]=='0') {curr_left=j+1;left[j]=0;}
                else left[j]=max(curr_left,left[j]);
            }
            for(int j=n-1;j>=0;j--)
            {
                if(matrix[i][j]=='0') {curr_right=j;right[j]=n;} //not inclusive
                else right[j]=min(right[j],curr_right);
            }
            for(int j=0;j<n;j++)
            {
                max_area=max(max_area,min(height[j],(right[j]-left[j])));
            }
        }
        return max_area*max_area;
    }
```
four simple dp:
- find the height at each row
- find the left ending of 1 till j
- find the right ending of 1 after j--
- square area

The above solution is hard to understand, the following dp is much better
```cpp
    int maximalSquare(vector<vector<char>>& matrix) {
        if(matrix.empty()) return 0;
        int ans=0,m=matrix.size(),n=matrix[0].size();
        vector<vector<int>> dp(m,vector<int>(n));
        for(int i=0;i<m;i++) if(matrix[i][0]=='1') ans=dp[i][0]=1;
        for(int i=0;i<n;i++) if(matrix[0][i]=='1') ans=dp[0][i]=1;
        for(int i=1;i<m;i++)
            for(int j=1;j<n;j++){
                if(matrix[i][j]=='1')
                    dp[i][j]=min(dp[i-1][j-1],min(dp[i-1][j],dp[i][j-1]))+1;
                ans=max(ans,dp[i][j]);
            }
        return ans*ans;
                
    }
```	
the side length is the min of neighboring side.
if (i,j) is '0' we cannot form a square
if(i,j) is '1' we depends the other 3 points to add this point.
above can be simplified to 1d space requirement,

84. Largest Rectangle in Histogram
```cpp
    int largestRectangleArea(vector<int>& heights) {
        //add two guarding nodes to avoid special treatment
        heights.insert(heights.begin(),0);
        heights.push_back(0);
        stack<int> st;
        int maxarea=0;
        for(int i=0;i<heights.size();i++)
        {
            while(!st.empty() && heights[i]<heights[st.top()])
            {
                int tp=st.top();st.pop();
                maxarea=max(maxarea,(i-st.top()-1)*heights[tp]);
            }
            st.push(i);
        }
        return maxarea;
    }
```
similar problem 42, trapping rain water
```cpp
    int trap(vector<int>& height) {
        //using stack similar to largest rectangle in histogram
        //we maintain a stack with index, in decreasing order of height
        int n=height.size();
        stack<int> s;
        int i=0,total=0;

        while(i<n)
        {
            if(s.empty() || height[i]<=height[s.top()]) s.push(i++);
            else
            {
                int tp=s.top();s.pop();
                //min(h(l),h(r))-h(i). If there is no element in stack, the area is 0
                if(!s.empty())
                {
                    int area=(min(height[i],height[s.top()])-height[tp])*(i-s.top()-1);//current one as the minimum
                    total+=area;
                }
            }
        }
        return total;
    }
```
11. containing with most waters
this one can be approached using two pointer.
	
	
85. Maximal Rectangle
similarly we can use dp
for (i,j) if it is '1', then we need to get the min width and min length of neighboring 3 points
it means we need to store length and width for each point.
we find the width and find all above min length and increase the height o(N^3)


```cpp
    int maximalRectangle(vector<vector<char> > &matrix) {
        int m=matrix.size();
        if (m==0) return 0;
        int m=matrix[0].size();
        if (m==0) return 0;
        vector<vector<int>> wid(m,vector<int>(m,0));  //number of consecutive 1s to the left of matrix[i][j], including itself

        int area=0;
        for (int i=0;i<m;i++){
            for (int j=0;j<m;j++){
                if (matrix[i][j]=='1'){
                    if (j==0) wid[i][j]=1;
                    else wid[i][j]=wid[i][j-1]+1;
                    int y=1;
                    int x=m;
                    while((i-y+1>=0)&&(matrix[i-y+1][j]=='1')){
                        x=min(x, wid[i-y+1][j]);
                        area=max(area,x*y);
                        y++;
                    } 
                }
            }
        }
```
using height, then it is similar to max rectangle in histogram
```cpp
    int maximalRectangle(vector<vector<char>>& matrix) {
        //can we do it line by line and calculate the max?
        //using the algorithm from the max area in histogram
        //need calculate its height every line
        if(matrix.size()==0) return 0;
        int ncol=matrix[0].size();
        int nrow=matrix.size();
        
        vector<int> height(ncol+1);
        int max_area=0;
        for(int i=0;i<nrow;i++)
        {
            for(int j=0;j<ncol;j++) 
            {
                char c=matrix[i][j];
                if(c=='0') height[j]=0;
                else height[j]++;
            }
            int area=largestRectangleArea(height)       ;
            if(area>max_area) max_area=area;
        }
        return max_area;
    }
    int largestRectangleArea(vector<int> heights) {
        //add two guarding nodes to avoid special treatment
        heights.insert(heights.begin(),0);
        heights.push_back(0);
        stack<int> st;
        int maxarea=0;
        for(int i=0;i<heights.size();i++)
        {
            while(!st.empty() && heights[i]<heights[st.top()])
            {
                int tp=st.top();st.pop();
                maxarea=max(maxarea,(i-st.top()-1)*heights[tp]);
            }
            st.push(i);
        }
        return maxarea;
    }
```	

	
hard medium *****
137	Single Number II     		45.9%	Medium	

every number appear 3 times, only one appears once. find that number
this needs karno map to find the logical relation to make 3 times into 0
input: 00->0, 01->0, 11->0
true table to indicate the state change: (so when it adds to 3 it returns to 0)
a b c | a b
0 0 0   0 0
0 1 0   0 1
1 0 0   1 0
0 0 1   0 1
0 1 1   1 0
1 0 1   0 0
a, b is the two bit status, c is the input.
combine all the 1s sum(all 1) and(all 0)

a=a~b~c+~abc
b=~ab~c+~a~bc
final answer: a|b 
karaugh map

```cpp
    int singleNumber(vector<int>& nums) {
        int a=0;
        int b=0;
        for(int c:nums){
            int ta=(~a&b&c)|(a&~b&~c);
            b=(~a&~b&c)|(~a&b&~c);
            a=ta;
        }
        //we need find the number that is 01,10 => 1, 00 => 0.
        return a|b;        
    }
```

377. Combination Sum IV
find all combination that sum to a target. no duplicates, can used multiple times
nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
1,1,2 is different from 1,2,1 and 2,1,1.
we need keep it in a hashmap and calculate possible combination using the hashmap.
if we have m a and n b, the combination would be: C(m+n, m)
if we have m a and n b and l c, the combination would be C(m+n+l,m)*C(n+l,n)
```cpp
    int combinationSum4(vector<int>& nums, int target) {
		int ans=0;
		unordered_map<int,int> mp;
		dfs(nums,0,target,ans,mp);
		return ans;
    }
	void dfs(vector<int>& nums,int ind,int target,int& ans,unordered_map<int,int> mp)
	{
		if(target==0) 
		{
            int sz=0,n=0;
            for(auto t: mp) 
            {
                sz+=t.second;
                if(t.second) n=t.second;
            }
            int cnt=1;
            for(auto t: mp)
            {
                if(t.second) cnt*=comb(sz,t.second),sz-=t.second;
            }
			ans+=cnt;return;
		}
		if(target<0) return;
		for(int i=ind;i<nums.size();i++)
		{
			mp[nums[i]]++;
			dfs(nums,i,target-nums[i],ans,mp);
			mp[nums[i]]--;
		}
	}
	int comb(int m,int n)
	{//m!/(n!*(m-n)!)
		int ans=1;
    	for(int i=n+1,j=1;i<=m || j<=m-n;i++,j++) {if(i<=m)ans*=i;if(j<=m-n) ans/=j;}
		return ans;
	}
```


this gives correct answer, but it times out since to add to a target, it needs a lot of iterations.

if only needs the number, generally it is an indication that we need use dp.
or equivalently reducing to a similar subproblem (recursion)
```
public int combinationSum4(int[] nums, int target) {
    if (target == 0) {
        return 1;
    }
    int res = 0;
    for (int i = 0; i < nums.length; i++) {
        if (target >= nums[i]) {
            res += combinationSum4(nums, target - nums[i]);
        }
    }
    return res;
}
```

much simpler version using dp
dp[t] is the number of combinations to reach target t.
this is combined with knapsack and climbing stairs.

```cpp
    int combinationSum4(vector<int>& nums, int target) {
        //typical dp knapsack problem
        vector<int> dp(target+1);
		int mod=1e9+7;
        dp[0]=1; //when target is 0, empty choice
        for(int i=1;i<=target;i++)
        {
            for(int j=0;j<nums.size();j++)
            {
                if(i>=nums[j]) dp[i]+=dp[i-nums[j]]%mod;
            }
        }
        return dp[target];
    }
```	
integer overflow, but this pass all tests by limiting all numbers in int range.

```cpp
    int combinationSum4(vector<int>& nums, int target) {
        //typical dp knapsack problem
        vector<long> dp(target+1);
		int mod=INT_MAX;
        dp[0]=1; //when target is 0, empty choice
        for(int i=1;i<=target;i++)
        {
            for(int j=0;j<nums.size();j++)
            {
                if(i>=nums[j]) dp[i]+=dp[i-nums[j]]%mod,dp[i]%=mod;
            }
        }
        return dp[target];
    }
```	

