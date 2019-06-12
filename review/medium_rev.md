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
328	Odd Even Linked List    		49.1%	Medium	
sort first odd and then even.
break into two lists and then connect
347	Top K Frequent Elements    		54.9%	Medium	
hashmap and then put into a pq
```cpp
	struct comp{
		bool operator()(const pair<int,int>& a,const pair<int,int>& b){
			return a.second<b.second;
		}
	};
    vector<int> topKFrequent(vector<int>& nums, int k) {
		unordered_map<int,int> mp;
		for(int i: nums) mp[i]++;
		priority_queue<pair<int,int>>,vector<pair<int,int>>,comp> pq(mp.begin(),mp.end());
		vector<int> ans;
		while(k){
			auto t=pq.top();
			pq.pop();
			ans.push_back(t.first);
		}
		return ans;
    }
```	
357	Count Numbers with Unique Digits    		46.9%	Medium	
combination and permutations

360	Sort Transformed Array    		46.8%	Medium	
361	Bomb Enemy    		43.3%	Medium	
362	Design Hit Counter    		59.0%	Medium	
364	Nested List Weight Sum II    		57.7%	Medium	
365	Water and Jug Problem    		28.9%	Medium	
math problem

366	Find Leaves of Binary Tree    		65.7%	Medium	
368	Largest Divisible Subset    		34.8%	Medium	
sort and similar to find the longest subsequence Si%Sj
but need get the list so use backtracking or dp with previous information

```cpp
    vector<int> largestDivisibleSubset(vector<int>& nums) {
    }
```	

369	Plus One Linked List    		56.2%	Medium	
392	Is Subsequence    		46.8%	Medium	
two pointer or edit distance
```
    bool isSubsequence(string s, string t) {
		int i=0,j=0;
		while(i<s.size() && j<t.size())
		{
			if(s[i]==s[j]) i++;
			j++;
		}
		return i==s.size() && j<=t.size();
    }
```	
similar problems:
word frequency
kth largest element in an array
sort characters by frequency
split array into consecutive subsequences
top k frequent words
k closest points to origin

445	Add Two Numbers II    		50.1%	Medium	
linked list MSB to LSB.
reverse first

451	Sort Characters By Frequency    		56.0%	Medium	
simple, hashmap and then put in a heap or sort

452	Minimum Number of Arrows to Burst Balloons    		46.4%	Medium	
balloon have start and end. find the min number of arrows needed to burst all balloons
interval,using the end. 
use the leftmost ending, burst all ballons with start<=this end;
```cpp
	bool comp(vector<int>& a,vector<int>& b) {return a[1]<b[1];}
    int findMinArrowShots(vector<vector<int>>& points) {
        if(points.empty()) return 0;
		sort(points.begin(),points.end(),comp);
		int i=0,j=0,ans=0,n=points.size();
		int cur_end=points[0][1];
		while(j<n){
			while(j<n && points[j][0]<=cur_end) j++;
			ans++;
			if(j<n) cur_end=points[j][1];
		}
		return ans;
    }
```	
454	4Sum II    		50.6%	Medium	
reduce 4 list into two list
a+b+c+d=0
straightforward

478	Generate Random Point in a Circle    		36.5%	Medium	
x=rcos(angle),y=rsin(angle)
if we choose r between 0,1 randomly, angle from 0 to 2*pi, then we got more points in the center
x^2+y^2=r^2, if x and y to be uniform distributed, then r^2 shall be uniform distributed
```cpp
	double r,cx,cy;
    Solution(double radius, double x_center, double y_center) {
        r=radius,cx=x_center,cy=y_center;
    }
    
    vector<double> randPoint() {
        double r2=sqrt(rand()*1.0/RAND_MAX)*r;
		double angle=rand()*2.0*3.1415926/RAND_MAX;
		return {r2*cos(angle)+cx,r2*sin(angle)+cy};
		
    }
```
- don't forget the center.
- normalize using rand_max



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

319	Bulb Switcher    		43.9%	Medium	

322	Coin Change    		30.4%	Medium	
min number of coins, if cannot add up, return -1
knapsack with reusage of coins
```cpp
    int coinChange(vector<int>& coins, int amount) {
		int n=coins.size();
		vector<int> dp(amount+1,INT_MAX);
		dp[0]=0;
		for(int i=1;i<=amount;i++)
		{
			for(auto c: coins){
				if(i>=c && dp[i-c]!=INT_MAX) {dp[i]=min(dp[i],dp[i-c]+1);}
			}
		}
		return dp[amount]==INT_MAX?-1:dp[amount];
    }
```	


393	UTF-8 Validation    		35.9%	Medium	
   Char. number range  |        UTF-8 octet sequence
      (hexadecimal)    |              (binary)
   --------------------+---------------------------------------------
   0000 0000-0000 007F | 0xxxxxxx
   0000 0080-0000 07FF | 110xxxxx 10xxxxxx
   0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
   0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```cpp
    bool validUtf8(vector<int>& data) {
        int count = 0;
        for (auto c : data) {
            if (count == 0) {
                if ((c >> 5) == 0b110) count = 1;
                else if ((c >> 4) == 0b1110) count = 2;
                else if ((c >> 3) == 0b11110) count = 3;
                else if ((c >> 7)) return false;
            } else {
                if ((c >> 6) != 0b10) return false;
                count--;
            }
        }
        return count == 0;
    }
```
need to observe the rule!!

397	Integer Replacement    		31.4%	Medium	
greedy. n is even n/2, n is odd we add+1 or -1.
divide is fast, so if we +1 or -1 to make it 4 multiples it will be faster
If n + 1 % 4 == 0, replace n with n + 1 will short the path. Otherwise, replace n with n - 1 is always the right direction.

433	Minimum Genetic Mutation    		38.1%	Medium	
gene sequence is 8 chars long, ACGT only
a mutation: one char changed only
bank: all valid gene sequence
min number of permutations from start to end.
bfs to get the min distance: 
```cpp
    int minMutation(string start, string end, vector<string>& bank) {
		unordered_set<string> ms(bank.begin(),bank.end()),visited;
		queue<string> q;
		char gene[]={'A','C','G','T'};
		q.push(start);
		visited.insert(start);
		int step=0;
		while(q.size()){
			int sz=q.size();
			for(int i=0;i<sz;i++){
				string s=q.front();
				q.pop();
				for(char& c: s){
					char oc=c;
					for(char tc: gene){
						if(tc==c) continue;
						c=tc;
						if(visited.count(s) || !ms.count(s)) {c=oc;continue;}
                        //cout<<s<<endl;
						if(s==end) return step+1;
						q.push(s),visited.insert(s);
						c=oc;
					}
				}
			}
			step++;
		}
		return -1;
	}
```	
note need restore the string at two branches.

508	Most Frequent Subtree Sum    		54.5%	Medium	
```cpp
    vector<int> findFrequentTreeSum(TreeNode* root) {
       //dfs search and form the hashmap during visit
        map<int,int,greater<int>> mp; 
		//it is better to use map (sorted) instead of unordered maxheap would be better
        dfs(root,mp);
        //reverse iterate to get the max
        auto it=mp.begin();
        vector<int> res;
        res.push_back(it->second);
        int sum0=it->second;
        while(++it!=mp.end())
        {
            if(it->second<sum0) break;
            res.push_back(it->second);
        }
        return res;
    }
    
    int dfs(TreeNode* root,map<int,int,greater<int>>& mp)
    {
        if(!root) return 0;
        int sum=root->val;
        int lsum=0,rsum=0;
        if(root->left) lsum=dfs(root->left,mp);
        if(root->right) rsum=dfs(root->right,mp);
        sum+=lsum+rsum;
        mp[sum]++;
        return sum;    
    }
```	
510	Inorder Successor in BST II    		52.7%	Medium	
513	Find Bottom Left Tree Value    		58.4%	Medium	
last row, leftmost node
```cpp
    int findBottomLeftValue(TreeNode* root) {
        //can use breath first search
        int h=height(root);
        if(h==1) return root->val;
        if(height(root->left)<height(root->right)) 
            return findBottomLeftValue(root->right);
        return findBottomLeftValue(root->left);
    }
    
    int height(TreeNode* root)
    {
        if(!root) return 0;
        return 1+max(height(root->left),height(root->right));
    }
```
	
515	Find Largest Value in Each Tree Row    		57.8%	Medium	
```cpp
    vector<int> largestValues(TreeNode* root) {
        map<int,int> mp;
        inorder(root,0,mp);
        vector<int> ans;
        for(auto it=mp.begin();it!=mp.end();it++) ans.push_back(it->second);
        return ans;
    }
    void inorder(TreeNode* root,int level,map<int,int>& mp)
    {
        if(!root) return;
        inorder(root->left,level+1,mp);
        if(mp.count(level)) mp[level]=max(mp[level],root->val);
        else mp[level]=root->val;
        inorder(root->right,level+1,mp);
    }
```	
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
similar problem 842. Split Array into Fibonacci Sequence
the first two number determines the whole string
a+b=c, if a has m digits, b has n digits, c has at least max(m,n) digits.
the first two number can use up to half of the total length
dfs to try all combinations
a can have 1 digit to n/2-1 digits
b can have 1 digit to n/2-1 digits
total_len=m+n+max(m,n). m+n<2/3len
for long string, it is very easy overflow so using string add

```cpp
    bool isAdditiveNumber(string num) {
		int n=num.size();
		for(int i=1;i<=(n+1)/2;i++)
		{
			for(int j=1;i+j<=(n+1)*2/3;j++)
			{
				if(valid_fib(num,i,j)) return 1;
			}
		}
		return 0;
    }
	bool valid_fib(string& num,int l1,int l2)
	{
		string a=num.substr(0,l1),b=num.substr(l1,l2);
        if(a.length()>1 && a[0]=='0') return 0;
        if(b.length()>1 && b[0]=='0') return 0;
		int l=l1+l2;
        bool valid=0;
		while(l<num.size()){
            string s=stringadd(a,b);
            //cout<<a<<" "<<b<<endl;
			if(num.substr(l,s.size())!=s) return 0;
            valid=1;
			l+=s.length();
			a=b,b=s;
		}
		return valid;
	}
    string stringadd(string& a,string& b){
        string ans;
        int cf=0;
        int m=a.size(),n=b.size();
        int i=m-1,j=n-1;
        while(cf || i>=0 || j>=0)
        {
            int d=(i>=0?a[i]-'0':0)+(j>=0?b[j]-'0':0)+cf;
            ans+=d%10+'0';
            cf=d/10;
            i--,j--;
        }
        return {ans.rbegin(),ans.rend()};
    }
```	
-. use string add to do large number addition
-. number cannot start with 0
-. have to have at least 3 numbers (if it skip the loop it is incorrect)
-. a can up to half length, a+b can up to 2/3 length

842	Split Array into Fibonacci Sequence    		34.6%	Medium	
need return any valid sequence, each number limits as integer, so up to 9 digits.
use backtracking
```cpp
    vector<int> splitIntoFibonacci(string S) {
		vector<int> ans;
		dfs(S,0,ans);
		return ans;
    }
	bool dfs(string& s,int ind,vector<int>& ans)
	{
		int n=s.size();
		if(ind>=n && ans.size()>=3) return 1;
		int maxsize=s[ind]=='0'?1:10;
		for(int i=1;i<=maxsize && ind+i<=s.size();i++)
		{
			long a=stoll(s.substr(ind,i));
			if(a>INT_MAX) return 0;
			int sz=ans.size();
			if(sz>=2 && (long)ans[sz-1]+ans[sz-2]<a) return 0;
			if(sz<=1 || (long)ans[sz-1]+ans[sz-2]==a){
				ans.push_back(a);
				if(dfs(s,ind+i,ans)) return 1;
				ans.pop_back();
			}
		}
		return 0;
	}
```	
be sure to use long for intermediate calculation


307	Range Sum Query - Mutable    		28.6%	Medium	
find the range sum [i,j]
update(i,val) will change the value at i
prefix sum: range sum=prefix[j]-prefix[i-1]
update val[i] will change all the prefix sum including val[i] by adding the difference
```cpp
    vector<int> prefix;
	
	NumArray(vector<int>& nums) {
        //data=nums;
		prefix.push_back(0);
		for(int i: nums) prefix.push_back(prefix.back()+i);
    }
    
    void update(int i, int val) {
        int old=prefix[i+1]-prefix[i];
		int diff=val-old;
		for(int j=i+1;j<prefix.size();j++) prefix[j]+=diff;
    }
    
    int sumRange(int i, int j) {
        return prefix[j+1]-prefix[i];
    }
```
this introduce a new data structure called segment tree
a segment tree is a binary tree with each node a segment.
it can have O(logn) complexity
```cpp
struct SegmentTreeNode {
    int start, end, sum;
    SegmentTreeNode* left;
    SegmentTreeNode* right;
    SegmentTreeNode(int a, int b):start(a),end(b),sum(0),left(nullptr),right(nullptr){}
};
class NumArray {
    SegmentTreeNode* root;
public:
    NumArray(vector<int> &nums) {
        int n = nums.size();
        root = buildTree(nums,0,n-1);
    }
   
    void update(int i, int val) {
        modifyTree(i,val,root);
    }

    int sumRange(int i, int j) {
        return queryTree(i, j, root);
    }
    SegmentTreeNode* buildTree(vector<int> &nums, int start, int end) {
        if(start > end) return nullptr;
        SegmentTreeNode* root = new SegmentTreeNode(start,end);
        if(start == end) {
            root->sum = nums[start];
            return root;
        }
        int mid = start + (end - start) / 2;
        root->left = buildTree(nums,start,mid);
        root->right = buildTree(nums,mid+1,end);
        root->sum = root->left->sum + root->right->sum;
        return root;
    }
    int modifyTree(int i, int val, SegmentTreeNode* root) {
        if(root == nullptr) return 0;
        int diff;
        if(root->start == i && root->end == i) {
            diff = val - root->sum;
            root->sum = val;
            return diff;
        }
        int mid = (root->start + root->end) / 2;
        if(i > mid) {
            diff = modifyTree(i,val,root->right);
        } else {
            diff = modifyTree(i,val,root->left);
        }
        root->sum = root->sum + diff;
        return diff;
    }
    int queryTree(int i, int j, SegmentTreeNode* root) {
        if(root == nullptr) return 0;
        if(root->start == i && root->end == j) return root->sum;
        int mid = (root->start + root->end) / 2;
        if(i > mid) return queryTree(i,j,root->right);
        if(j <= mid) return queryTree(i,j,root->left);
        return queryTree(i,mid,root->left) + queryTree(mid+1,j,root->right);
    }
};
```

309	Best Time to Buy and Sell Stock with Cooldown    		44.0%	Medium	

310	Minimum Height Trees    		30.1%	Medium	
undirected graph choose any node as the root and return those nodes will min height
remove all leaf node until there is one or two nodes

318	Maximum Product of Word Lengths    		48.5%	Medium	
leni*lenj and word[i] and word[j] shares no common character
group the length into hashset
using bit & to judge if they have common chars

```cpp
    int maxProduct(vector<string>& words) {
		int maxprod=0;
		vector<int> hash(words.size());
		for(int i=0;i<words.size();i++)
		{
			string& s=words[i];
			int t=0;
			for(auto c: s) t|=(1<<(c-'a'));
			hash[i]=t;
            //cout<<s<<":"<<t<<endl;
			for(int j=i-1;j>=0;j--){
				if((hash[i]&hash[j])==0) 
                {
                    int t=words[i].length()*words[j].length();
                    maxprod=max(maxprod,t);
                }
			}
		}
		return maxprod;
	}
    
```	
- use | to indicate the char existence
- use & to judge no common
- note the bit has low priority, add ()


324	Wiggle Sort II    		28.0%	Medium	
in-place O(n) sort so that a[0]<a[1]>a[2]<a[3]....
greedy: a[0],a[2]... is the sorted first half, a[1],a[3].... is the 2nd half
first break the array into half (for odd sized, the first half (n+1)/2)
O(nlogn)
or find the median value. nth_element

O(1) uses index mapping. don't know what it is.

331	Verify Preorder Serialization of a Binary Tree    		38.6%	Medium	
null node is saved as #
The observation is important:

Denote the number of null nodes as nullCnt, the number of actual nodes as nodeCnt.

For a binary tree, the number of null nodes is always the number of actual nodes plus 1. nullCnt==nodeCnt+1;

So,

if nullCnt>nodeCnt+1, the tree is invalid.
if nullCnt<nodeCnt+1, the tree is incomplete.
if nullCnt==nodeCnt+1, the tree is complete and can't be extended.
We just need to keep track of nullCnt and nodeCnt as we go through the sequence and check these conditions above.

Actually, recording nullCnt-nodeCnt is enough, so you can further improve the code.

all non-null node provides 2 outdegree and 1 indegree (2 children and 1 parent), except root
all null node provides 0 outdegree and 1 indegree (0 child and 1 parent).

Suppose we try to build this tree. During building, we record the difference between out degree and in degree diff = outdegree - indegree. When the next node comes, we then decrease diff by 1, because the node provides an in degree. If the node is not null, we increase diff by 2, because it provides two out degrees. If a serialization is correct, diff should never be negative and diff will be zero when finished.

```cpp
public boolean isValidSerialization(String preorder) {
    String[] nodes = preorder.split(",");
    int diff = 1;
    for (String node: nodes) {
        if (--diff < 0) return false;
        if (!node.equals("#")) diff += 2;
    }
    return diff == 0;
}
```

this is like validate the parenthesis, we can use stack
```cpp
    public boolean isValidSerialization(String preorder) {
        // using a stack, scan left to right
        // case 1: we see a number, just push it to the stack
        // case 2: we see #, check if the top of stack is also #
        // if so, pop #, pop the number in a while loop, until top of stack is not #
        // if not, push it to stack
        // in the end, check if stack size is 1, and stack top is #
        if (preorder == null) {
            return false;
        }
        Stack<String> st = new Stack<>();
        String[] strs = preorder.split(",");
        for (int pos = 0; pos < strs.length; pos++) {
            String curr = strs[pos];
            while (curr.equals("#") && !st.isEmpty() && st.peek().equals(curr)) {
                st.pop();
                if (st.isEmpty()) {
                    return false;
                }
                st.pop();
            }
            st.push(curr);
        }
        return st.size() == 1 && st.peek().equals("#");
    }
```
	
332	Reconstruct Itinerary    		31.4%	Medium	
from JFK, return the smallest order.
build a graph data structure and always tries the smallest first
data structure is more important. need visit all nodes same time as it appears.
so need a mechanism to record the visit of all the stations.
only when success, we add the result.
```cpp
unordered_map<string, multiset<string>> targets;
vector<string> route;
vector<string> findItinerary(vector<pair<string, string>> tickets) {
    for (auto ticket : tickets)
        targets[ticket[0]].insert(ticket[1]);
    visit("JFK");
    return vector<string>(route.rbegin(), route.rend());
}

void visit(string airport) {
    while (targets[airport].size()) {
        string next = *targets[airport].begin();
        targets[airport].erase(targets[airport].begin());
        visit(next);
    }
    route.push_back(airport);
}
```
note multiset is necessary since it may contain cycles around two airports.

```cpp
    vector<string> findItinerary(vector<pair<string, string>> tickets) {
        multiset<pair<string,string>> tset(tickets.begin(),tickets.end());
        unordered_map<string,multiset<string>> mp;
        for(int i=0;i<tickets.size();i++) mp[tickets[i].first].insert(tickets[i].second);
        vector<string> ans;
        ans.push_back("JFK");
        dfs(mp,"JFK",tset,ans);
        return ans;
    }
    
    bool dfs(unordered_map<string,multiset<string>>& mp,string node,multiset<pair<string,string>>& to_visit,vector<string>& ans)
    {
        
        //cout<<node<<" "<<to_visit.size()<<endl;
        if(to_visit.size()==0) return 1;
        if(mp[node].size()==0) return 0;
        
        for(auto it=mp[node].begin();it!=mp[node].end();it++)
        {
            string next=*it;
            //mp need delete this
            //mp[node].erase(next);//mp cannot be modified since the path may fail
            pair<string,string> p=make_pair(node,next);
            auto it1=to_visit.find(p);
            if(it1!=to_visit.end())
            {
                //cout<<node<<"->"<<next<<" ";
                to_visit.erase(it1);//remove this edge
                ans.push_back(next);
                if(dfs(mp,next,to_visit,ans)) {return 1;}
                to_visit.insert(p);//put back this edge if failed
                //cout<<"pop "<<ans.back()<<endl;
                ans.pop_back();
            }
        }
        return 0;
    }
```	
	

333	Largest BST Subtree    		33.0%	Medium	
334	Increasing Triplet Subsequence    		39.6%	Medium	
O(n) time and O(1) space
return if there is such sequence
stack could be a good choice (we need to keep updating the min and second min so not very good)

```cpp
bool increasingTriplet(vector<int>& nums) {
    int c1 = INT_MAX, c2 = INT_MAX;
    for (int x : nums) {
        if (x <= c1) {
            c1 = x;           // c1 is min seen so far (it's a candidate for 1st element)
        } else if (x <= c2) { // here when x > c1, i.e. x might be either c2 or c3
            c2 = x;           // x is better than the current c2, store it
        } else {              // here when we have/had c1 < c2 already and x > c2
            return true;      // the increasing subsequence of 3 elements exists
        }
    }
    return false;
}
```	
need use <= for compare, how to make sure c1 appears in front of c2?
initial : [1, 2, 0, 3], small = MAX, big = MAX
loop1 : [1, 2, 0, 3], small = 1, big = MAX
loop2 : [1, 2, 0, 3], small = 1, big = 2
loop3 : [1, 2, 0, 3], small = 0, big = 2 // <- Uh oh, 0 technically appeared after 2
loop4 : return true since 3 > small && 3 > big // Isn't this a violation??

If you observe carefully, the moment we updated big from MAX to some other value, that means that there clearly was a value less than it (which would have been assigned to small in the past). What this means is that once you find a value bigger than big, you've implicitly found an increasing triplet.


337	House Robber III    		48.0%	Medium	
in binary tree

338	Counting Bits    		64.7%	Medium	
from 0 to N, counting number of 1s in each number. O(n) space and time requirement.
dp[i]=dp[i/2]+i&1
or dp[i]=dp[i&(i-1)]+1


343	Integer Break    		47.8%	Medium	
dp or math, break into a series sum which product is the largest.

348	Design Tic-Tac-Toe    		49.7%	Medium	
351	Android Unlock Patterns    		45.8%	Medium	
353	Design Snake Game    		30.5%	Medium	
355	Design Twitter    		27.3%	Medium	
Design a simplified version of Twitter where users can post tweets, follow/unfollow another user and is able to see the 10 most recent tweets in the user's news feed. Your design should support the following methods:

postTweet(userId, tweetId): Compose a new tweet.
getNewsFeed(userId): Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
follow(followerId, followeeId): Follower follows a followee.
unfollow(followerId, followeeId): Follower unfollows a followee.


356	Line Reflection    		30.9%	Medium	
370	Range Addition    		60.5%	Medium	
372	Super Pow    		35.6%	Medium	
math problem on modulus


375	Guess Number Higher or Lower II    		37.7%	Medium	
376	Wiggle Subsequence    		37.4%	Medium	
dp using up and dn
if current one vs previous forms up, then up=dn+1 (previous longest dn)
```cpp
    int wiggleMaxLength(vector<int>& nums) {
		if(nums.size()==0) return 0;
		int up=1,dn=1;
		for(int i=1;i<nums.size();i++){
			if(nums[i]<nums[i-1]) dn=up+1;
			else if(nums[i]>nums[i-1]) up=dn+1;
		}
		return max(up,dn);
    }
```
this is a reduced memory version of 2d dp

379	Design Phone Directory    		41.2%	Medium	
380	Insert Delete GetRandom O(1)    		42.8%	Medium	
random choose: hashmap to record iterator information, vector to store the data

382	Linked List Random Node    		49.3%	Medium	
hashmap to map id to node and node to id

384	Shuffle an Array    		50.0%	Medium	
randomly get its permutations
```cpp
    vector<int> arr, idx;
public:
    Solution(vector<int> nums) {
        srand(time(NULL));
        arr.resize(nums.size());
        idx.resize(nums.size());
        for (int i=0;i<nums.size();i++){
            arr[i] = nums[i];
            idx[i] = nums[i];
        }
    }
    
    /** Resets the array to its original configuration and return it. */
    vector<int> reset() {
        for (int i=0;i<arr.size();i++)
            arr[i] = idx[i];
        return arr;    
    }
    
    /** Returns a random shuffling of the array. */
    vector<int> shuffle() {
         int i,j;
         for (i = arr.size() - 1; i > 0; i--) {
            j = rand() % (i + 1);
            swap(arr[i], arr[j]);
         }
         return arr;    
    }
```
rand to choose 0 to i to swap.


386	Lexicographical Numbers    		46.1%	Medium	
O(nlogn) algorithm, n up to 5,000,000
```cpp
bool cmp(int a,int b) {return to_string(a) < to_string(b);}
class Solution {
public:
    vector<int> lexicalOrder(int n) {
        vector<int> ans(n);
        for(int i=0;i<n;i++) ans[i]=i+1;
        sort(ans.begin(),ans.end(),cmp);
        return ans;
    }
};
```
not good enough, but we can use simple approach to arrange the nubers

Just repeatedly try from 1 to 9, 1 -> 10 -> 100 first, and then plus 1 to the deepest number. Take 13 as example:
1 -> 10 -> (100) -> 11 -> (110) -> 12 -> (120) -> 13 -> (130) -> (14) -> 2 -> (20) ... -> 9 -> (90)

```cpp
    vector<int> lexicalOrder(int n) {
        vector<int> res;
        helper(1, n, res);
        return res;
    }
    
    void helper(int target, int n, vector<int>& res) {
        if (target > n) return;
        res.push_back(target);
        helper(target * 10, n, res);
        if (target % 10 != 9) helper(target+1, n, res);
    }
```
	
388	Longest Absolute File Path    		39.2%	Medium	
\n: number \n is the level
.: the file name

390	Elimination Game    		43.4%	Medium	
array 1 to n, first from left to right, every 2 eliminated
then right to left, every 2 eliminated.
return the last element left.

After first elimination, all the numbers left are even numbers.
Divide by 2, we get a continuous new sequence from 1 to n / 2.
For this sequence we start from right to left as the first elimination.
Then the original result should be two times the mirroring result of lastRemaining(n / 2).
```cpp
int lastRemaining(int n) {
    return n == 1 ? 1 : 2 * (1 + n / 2 - lastRemaining(n / 2));
}
```



396	Rotate Function    		35.1%	Medium	
find the math relation
```cpp
    int maxRotateFunction(vector<int>& A) {
        //this is the inner product of two vectors
        //derived from the recursive relation F(k)-F(k-1)=Sum(A)-nA(n-k)
        int sum=0,allsum=0;
        int n=A.size();
        for(int i=0;i<n;i++) {sum+=A[i];allsum+=i*A[i];}//F(0) and sum(A)
        int maxsum=allsum;
        for(int i=1;i<n;i++)
        {
            allsum+=sum-n*A[n-i];
            if(allsum>maxsum) maxsum=allsum;
        }
        return maxsum;
    }
```	

398	Random Pick Index    		49.9%	Medium	
random return the index of the target (duplicate may exist)
similar problem:
linked list random node
random pick with blacklist
random pick with weight

```cpp
vector<int> _nums;
public:
    Solution(vector<int> nums) {
        _nums = nums;
    }

    int pick(int target) {
        int n = 0, ans = -1;
        for(int i = 0 ; i < _nums.size(); i++)
        {
            if(_nums[i] != target) continue;
            if(n == 0){ans = i; n++;}
            else
            {
                n++;
                if(rand() % n == 0){ans = i;}// with prob 1/(n+1) to replace the previous index
            }
        }
        return ans;
    }
```
	
399	Evaluate Division    		47.5%	Medium	
dfs relation

402	Remove K Digits    		26.6%	Medium	
remove k digit to make it smaller
from left to right, remove peaks
if left, remove the right digits
remove leading zeros
```cpp
string removeKdigits(string num, int k) {
        string res;
        int keep = num.size() - k;
        for (int i=0; i<num.size(); i++) {
            while (res.size()>0 && res.back()>num[i] && k>0) {
                res.pop_back();
                k--;
            }
            res.push_back(num[i]);
        }
        res.erase(keep, string::npos);
        
        // trim leading zeros
        int s = 0;
        while (s<(int)res.size()-1 && res[s]=='0')  s++;
        res.erase(0, s);
        
        return res=="" ? "0" : res;
    }
```

406	Queue Reconstruction by Height    		59.8%	Medium	
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]
[7,0],[7,1]
[7,0],[6,1],[7,1]
[5,0],[7,0],[6,1],[7,1]
[5,0],[7,0],[5,2],[6,1],[7,1]
[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]
first put the tallest in order and then insert lowers at its place
so every time when we insert, all elements are taller than it.


413	Arithmetic Slices    		55.8%	Medium	
return the number of arithmetic sequence
simple dp

416	Partition Equal Subset Sum    		40.6%	Medium	
two subsets
simple knapsack, jsut choose one set equal to sum/2

417	Pacific Atlantic Water Flow    		37.4%	Medium	
upper left belongs to pacific, lower right belongs to atlantic
find those cells can flow to both
starting from upper left boundary and lower right boundary
assuming the ocean is INT_MIN and we are seeking larger values
if a cell can be reached by both, then it is a choice
dfs
```cpp
    vector<pair<int, int>> res;
    vector<vector<int>> visited;
    void dfs(vector<vector<int>>& matrix, int x, int y, int pre, int preval){
        if (x < 0 || x >= matrix.size() || y < 0 || y >= matrix[0].size()  
                || matrix[x][y] < pre || (visited[x][y] & preval) == preval) 
            return;
        visited[x][y] |= preval;
        if (visited[x][y] == 3) res.push_back({x, y});
        dfs(matrix, x + 1, y, matrix[x][y], visited[x][y]); 
        dfs(matrix, x - 1, y, matrix[x][y], visited[x][y]);
        dfs(matrix, x, y + 1, matrix[x][y], visited[x][y]); 
        dfs(matrix, x, y - 1, matrix[x][y], visited[x][y]);
    }

    vector<pair<int, int>> pacificAtlantic(vector<vector<int>>& matrix) {
        if (matrix.empty()) return res;
        int m = matrix.size(), n = matrix[0].size();
        visited.resize(m, vector<int>(n, 0));
        for (int i = 0; i < m; i++) 
        {
            dfs(matrix, i, 0, INT_MIN, 1);
            dfs(matrix, i, n - 1, INT_MIN, 2);
        }
        for (int i = 0; i < n; i++) 
        {
            dfs(matrix, 0, i, INT_MIN, 1);
            dfs(matrix, m - 1, i, INT_MIN, 2);
        }
        return res;
    }
```
	
418	Sentence Screen Fitting    		31.1%	Medium	

419	Battleships in a Board    		65.7%	Medium	
battle ship can only be horizontal or vertical
and have space to separate
```cpp
        //bfs search similar to island
        //but we need use O(1) and one pass
        int m=board.size(),n=board[0].size();
        int ans=0;
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                //a battle ship will have a . at least one direction, left, right, up or down
                //or a battle ship is surrounded by dots
                if(board[i][j]=='X' && (i==0 || board[i-1][j]!='X') && (j==0 || board[i][j-1]!='X')) ans++;
            }
        }
        return ans;
    }
```
421	Maximum XOR of Two Numbers in an Array    		51.0%	Medium	
need O(n) time
from MSB to LSB, divide into two different groups
and again and again. unless we do not have two choices.
2 group->4 groups->8 groups....
for example we have: 
101,110,100 vs 011,001 for bit 2
01,10,00 vs 11,01 for bit 1 
left one, zero, right one zero

{01,00}+{10} vs {11},{01}
{01,00} vs {11} and {10} vs {01} all get 1 for bit1
for bit0: 

approach: from MSB to LSB check bit by bit. when a bit is done mask it by 1
put in hash table, by & the mask (the lower bits are all cleared).
candidate set the ith bit to be 1, then we must be able to find A^B=candidate
it is equivalent A=B^candidate.


```cpp
    int findMaximumXOR(vector<int>& nums) {
        //use a hashmap to store 01 set from MSB to LSB
        //gradually grows the bits and use the completed bits as mask to filter out those numbers

        int max = 0, mask = 0;
        unordered_set<int> t;
        // search from left to right, find out for each bit is there 
        // two numbers that has different value
        for (int i = 31; i >= 0; i--)
        {
            // mask contains the bits considered so far
            mask |= (1 << i);
            t.clear();
            // store prefix of all number with right i bits discarded
            for (int j=0;j<nums.size();j++) t.insert(mask & nums[j]);//the first several bits
            // now find out if there are two prefix with different i-th bit
            // if there is, the new max should be current max with one 1 bit at i-th position, which is candidate
            // and the two prefix, say A and B, satisfies:
            // A ^ B = candidate
            // so we also have A ^ candidate = B or B ^ candidate = A
            // thus we can use this method to find out if such A and B exists in the set 
            int candidate = max | (1<<i);
            for (auto it=t.begin();it!=t.end();it++)
            {
                int prefix=*it;
                if (t.find(prefix ^ candidate) != t.end())
                {
                    max = candidate;
                    break;
                }
            }
        }
        return max;
    }
```
it natually becomes a trie
for example: [3,10,5,25,2,8]
因为二进制元素只有0,1。考虑使用二叉树来构建前缀树。

根节点为符号位0
从高位到低位依次构建
左子树为1节点，又子树为0节点
[3,10,5,25,2,8] 按照以上规则构建如下图（省略高位0）

```cpp
struct TrieNode{
    int val;
    TrieNode *left;
    TrieNode *right;
    TrieNode(int x) : val(x), left(NULL), right(NULL) {}
};
class Solution {
public:

    int findMaximumXOR(vector<int>& nums) {
        TrieNode* root = new TrieNode(0);


        //建树
        TrieNode* curNode = root;
        for(int i = 0; i < nums.size(); i++){
            for(int j = 31; j >= 0; j--) {
                int tmp = nums[i] & (1 << j);
                if(tmp == 0){
                    if(!curNode->right){
                        curNode->right = new TrieNode(0);
                    }
                    curNode = curNode->right;
                }else{
                    if(!curNode->left){
                        curNode->left = new TrieNode(1);
                    }
                    curNode = curNode->left;
                }
                //cout << curNode->val;
            }
            //cout << endl;
            curNode = root;
        }

        //匹配最大异或值
        int max = 0;
        for(int i = 0; i < nums.size(); i++){
            int res = 0;
            for(int j = 31; j >= 0; j--){
                int tmp = nums[i] & (1 << j);
                //cout << (1 << j) << "\t" << tmp << endl;
                if(curNode->left && curNode->right){
                    if(tmp == 0){
                        curNode = curNode->left;
                    }else {
                        curNode = curNode->right;
                    }    
                }else {
                    curNode = curNode->left == NULL ? curNode->right:curNode->left;
                }
                res += tmp ^ (curNode->val << j);
                //cout << curNode->val;
            }
            curNode = root;
            //cout << endl;
            max = max > res?max:res;
        }

        return max;
    }
};
```

423	Reconstruct Original Digits from English    		45.6%	Medium	
```cpp
    string originalDigits(string s) {
        string digit[]={"zero","one","two","three","four","five","six","seven","eight","nine"};
        //letter's occurance if it only occurs once, then that number is definitely in
        //0 2 4 6 8 has distinct char inside. we can safely pick them out
        vector<int> cnt(26,0);
        for(int i=0;i<s.size();i++) cnt[s[i]-'a']++;
        vector<int> a(10,0); //to store the number of words
        a[0]=cnt['z'-'a'];
        a[2]=cnt['w'-'a'];
        a[4]=cnt['u'-'a'];
        a[6]=cnt['x'-'a'];
        a[8]=cnt['g'-'a'];
        a[3]=cnt['h'-'a']-a[8];
        a[5] = cnt['f'-'a'] - a[4];
        a[7] = cnt['v'-'a'] - a[5];
        a[1] = cnt['o'-'a'] - a[0] - a[2] - a[4];
        a[9] = cnt['i'-'a'] - a[5] - a[6] - a[8];   
        string res;
        for(int i=0;i<10;i++)
            res.append(a[i],i+'0');
        return res;
    }
```
	
424	Longest Repeating Character Replacement    		44.1%	Medium	
you can replace any char to other char to get longest repeating char
this is the find the longest window which contain at most k+1 type of characters
wrong: only can have len-mostcommon<=k
```cpp
	int characterReplacement(string s, int k) {
		vector<int> cnt(26);
		int i=0,j=0; //use two pointer sliding
		int maxlen=0;
		while(j<s.size()){
			char c=s[j];
			int ind=c-'A';
			cnt[ind]++;
			while( j-i+1-(*max_element(cnt.begin(),cnt.end())) > k) 
				cnt[s[i++]-'A']--;
			maxlen=max(maxlen,j-i+1);
            j++;
		}
		return maxlen;
	}
```
similar problem: longest substr with at most k distinct characters

426	Convert Binary Search Tree to Sorted Doubly Linked List    		52.0%	Medium	
430	Flatten a Multilevel Doubly Linked List    		42.2%	Medium	
recursive
```cpp
    Node* flatten(Node* head)
    {
        Node* last=0;
        return helper(head,last);
    }
    Node* helper(Node* head,Node*& last) {
        if(!head) return 0;
        Node* node=head;
        while(node)
        {
            //cout<<node->val<<endl;
            if(node->child)
            {
                Node* next=node->next;//save the next
                node->next=helper(node->child,last);
                node->next->prev=node;
                node->child=0;
                last->next=next;
                if(next) next->prev=last;
                node=last;
            }
            if(!node->next) last=node;
            node=node->next;
            
        }
        return head;
    }
```
	
435	Non-overlapping Intervals    		41.6%	Medium	
remove min number of intervals so that left are non-overlapping
equivalent to find the longest non-overlapping intervals
simple dp similar to longest increasing subsequence
sort using the ending (greedy: the smallest ending first):
```cpp
bool cmp(Interval& a,Interval& b) {return a.end<b.end;}
class Solution {
public:
    int eraseOverlapIntervals(vector<Interval>& intervals) {
        //if we sort the intervals according to start, the end is also sorted for non-overlapped cases
        //or the other problem: the largest number of steps 
        //greedy: always choose the interval with the ending smaller??
        if(intervals.size()==0) return 0;
        sort(intervals.begin(),intervals.end(),cmp);
        int ans=0;
        int cur_end=intervals[0].end;ans++;
        for(int i=0;i<intervals.size();i++)
        {
            //find the next interval using current ending, start>=current ending
            if(intervals[i].start>=cur_end) {cur_end=intervals[i].end;ans++;}
        }
        return intervals.size()-ans;
    }
};
```

436	Find Right Interval    		42.8%	Medium	
find the index of the interval on its closet right. if none, put it -1
sort with index and apply binary search to find
```cpp
bool cmp(const pair<Interval,int>& a,const pair<Interval,int>& b) {return a.first.start<b.first.start;}
class Solution {
public:
    vector<int> findRightInterval(vector<Interval>& intervals) {
        int n=intervals.size();
		vector<pair<Interval,int>> v(n);
		for(int i=0;i<n;i++) v[i]=make_pair(intervals[i],i);
		sort(v.begin(),v.end(),cmp);
		vector<int> ans(n,-1);
		for(int i=0;i<n;i++)
		{
			int ind=int(lower_bound(v.begin(),v.end(),make_pair(Interval(intervals[i].end,0),0),cmp)-v.begin());
			if(ind!=n) ans[i]=v[ind].second;
		}
		return ans;
    }
};
```


439	Ternary Expression Parser    		53.6%	Medium	
442	Find All Duplicates in an Array    		60.9%	Medium	
some appear once, some appear twice
find all appeared twice, elements are in 1 to n (n=size of the array)
use the trick to mark the seen
```cpp
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> res;
        
        for (int n: nums) 
        {
            int pos = abs(n);
            if(nums[pos - 1] > 0) nums[pos - 1] *= -1;
            else res.push_back(pos);
        }
        
        return res;
    }
```
	
444	Sequence Reconstruction    		20.3%	Medium	

449	Serialize and Deserialize BST    		47.1%	Medium	
preorder traversal
```cpp
    string serialize(TreeNode* root) {
        ostringstream os;
        serialize(root,os);
        return os.str();
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        istringstream is(data);
        return deserialize(is);
    }
    
private:
    void serialize(TreeNode* root, ostringstream& os)
    {
        if(root)
        {
            os<<root->val<<" ";
            serialize(root->left,os);
            serialize(root->right,os);
        }
        else os<<"# ";
    }
    TreeNode* deserialize(istringstream& is)
    {
        string s;
        is>>s;
        if(s=="#") return 0;
        TreeNode* p=new TreeNode(stoi(s));
        p->left=deserialize(is);
        p->right=deserialize(is);
        return p;
    }
```	
450	Delete Node in a BST    		40.0%	Medium	
find the node, swap with its right leftmost (if right exist) and then delete the leaf node
if not, promote the left.
if leaf node, 
```cpp
    TreeNode* deleteNode(TreeNode* root, int key) {
        if(!root) return 0;
        
        if(root->val==key){ //swap the rightmost node in the left branch with root
            if(!root->left) return root->right;
            if(!root->right) return root->left;
            TreeNode* node=findmin(root->right);
            root->val=node->val;
            root->right=deleteNode(root->right,root->val);
            return root;
        }
        if(key<root->val) root->left=deleteNode(root->left,key);
        else root->right=deleteNode(root->right,key);
        return root;
    }
    TreeNode* findmin(TreeNode* root)
    {
        if(!root) return 0;
        while(root->left) root=root->left;
        return root;
    }
```	



456	132 Pattern    		27.4%	Medium	
sequence pattern, we generally has
- binary search: the peak 
- stack: for peaks, for patterns,
- increasing, decreasing: min min2/max max2
Ai<Ak<Aj i<j<k, find a peak pattern
```cpp
    bool find132pattern(vector<int>& nums) {
        //from right side to left and keep in stack
        stack<int> st;
        int s3=INT_MIN;
        for(int i=nums.size()-1;i>=0;i--)
        {
            if(nums[i]<s3) return 1;
            while(st.size() && nums[i]>st.top())
            {
                s3=st.top();st.pop();
            }
            st.push(nums[i]);
        }
        return 0;
    }
```	
for example [3,1,4,2]: note we are doing reverse way.
2: push 2
4: pop 2, s3=2, push 4
1: 1<2 and we found 1,4,2
using a stack and a variable and current element, we build a 3 way relation


457	Circular Array Loop    		27.6%	Medium	
circular array with positive and negative, positive go forward k steps, negative go backward k steps
cycle must be one direction
Just think it as finding a loop in Linkedlist, except that loops with only 1 element do not count. 
Use a slow and fast pointer, slow pointer moves 1 step a time while fast pointer moves 2 steps a time. 
If there is a loop (fast == slow), we return true, 
else if we meet element with different directions, then the search fail, we set all elements along the way to 0. 
Because 0 is fail for sure so when later search meet 0 we know the search will fail.

```cpp
    bool circularArrayLoop(vector<int>& nums) {
		int n=nums.size();
		for(int i=0;i<n;i++){
			if(iscycle(nums,i)) return 1;
		}
		return 0;
    }
	int sign(int x) {return x>=0?1:-1;}
    bool iscycle(vector<int>& num,int ind){
		if(!num[ind]) return 0;
		int n=num.size();
		int i=ind,j=ind;
		int dir=sign(num[ind]);
		unordered_set<int> visited;
        visited.insert(i);
		bool cycle=1;
		do{ //at least jump one step
			int ti=((i+num[i])%n+n)%n;
            //cout<<ti<<" ";
			if(i==ti || !num[ti] || sign(num[ti])!=dir) {cycle=0;break;}
			int tj=((j+num[j])%n+n)%n;
            if(j==tj || !num[tj] || sign(num[tj])!=dir) {cycle=0;break;}
			int tj1=((tj+num[tj])%n+n)%n;
			if(tj==j || tj==tj1 || !num[tj1] || sign(num[tj1])!=dir) {cycle=0;break;}
			visited.insert(ti);
			//visited.insert(j);
            i=ti,j=tj1;
		}while(i!=j);
        //cout<<endl;
		if(!cycle) 
			for(auto t: visited) num[t]=0;
		return cycle;
	}
	
```	
- every step (slow or fast) we need check: if one step return to itself, if it is 0, if sign different

462	Minimum Moves to Equal Array Elements II    		52.4%	Medium	
a move is +1 or -1 to an element, purpose: to make all element equal
return the min number of moves.
intuition: goes to the median? maths
if the last target is t: then we have min(sum(|Ai-t|))
- sort the array
- for two numbers, choose target in the middle range, which value does not matter t-a+b-t=b-a
- for 3 numbers, choose the median will minimize: t-a+|b-t|+c-t=c-a+|b-t| to minimize it b=t;

```cpp
    public int minMoves2(int[] nums) {
        Arrays.sort(nums);
        int i = 0, j = nums.length-1;
        int count = 0;
        while(i < j){
            count += nums[j]-nums[i];
            i++;
            j--;
        }
        return count;
    }
```	
similar 453: a move is increment n-1 element by 1


464	Can I Win    		27.2%	Medium	
numbers 1 to n, cannot reuse, add together and who first cause the sum>=target wins
dfs try all possibilities. max choosable num<=20
we can use bit to indicate usage
```cpp
    unordered_set<int> win,lose;
    bool canIWin(int maxChoosableInteger, int desiredTotal) {
        //recursive approach, repeat all choice and reduce problem into smaller one
        //record all solved solution
        if(desiredTotal<=maxChoosableInteger) return 1;
        if(desiredTotal>maxChoosableInteger*(maxChoosableInteger+1)/2) return 0;
        int status=0;
        return canIWin(maxChoosableInteger,desiredTotal,status);
    }
    bool canIWin(int m,int sum,int status)
    {
        if(sum<=0) return 0; //already to the dead end, but still did not win
        if(win.count(status)) return 1;
        if(lose.count(status)) return 0;
        for(int i=1;i<=m;i++)
        {
            int bit=1<<i;
            if(status & bit) continue; //already solved, need the solved results recorded
            bool res=canIWin(m,sum-i,status|bit);
            if(!res) //surely win, why use ! since it is the even times.
            {
                win.insert(status);
                return 1;
            }
        }
        lose.insert(status);//after all trials, cannot win
        return 0;
    }
```
486	Predict the Winner    		46.8%	Medium	
choosing from either end, predict who is the winner
dp
```cpp
    bool PredictTheWinner(vector<int>& nums) {
        int n=nums.size();
        vector<vector<int>> dp(n,vector<int>(n,INT_MIN)); 
        //dp[i][j]: the score difference using the subarray from i to j i<=j
        for(int i=0;i<n;i++) dp[i][i]=nums[i];
        for(int l=n-2;l>=0;l--)
        {
            for(int r=l+1;r<n;r++) //r>=l
                dp[l][r]=max(dp[l][r],max(nums[l]-dp[l+1][r],nums[r]-dp[l][r-1]));
        }
        //print(dp);
        return dp[0][n-1]>=0;
    }
```
similar 464. adding to total	

467	Unique Substrings in Wraparound String    		33.9%	Medium	
s is repeat of a to z. given a string p, find number of its substr are present in s
for example zab, we have z,a,za,b,ab,zab
ending at differnt char.
```cpp
    int findSubstringInWraproundString(string p) {
        //according to the observation, we only need reserve the longest substr ending with a specific letter
        //for example, d, it can be d,cd,bcd,abcd,zabcd.... all other substr are all counted
        vector<int> max_substrlen(26); //substr ending with letter a-z
        int maxlen=0;
        for(int i=0;i<p.length();i++)
        {
            if(i && (p[i]-p[i-1]==1 || p[i]-p[i-1]==-25)) maxlen++;
            else maxlen=1;
            max_substrlen[p[i]-'a']=max_substrlen[p[i]-'a']>maxlen?max_substrlen[p[i]-'a']:maxlen;
        }
        //copy(max_substrlen.begin(),max_substrlen.end(),ostream_iterator<int>(cout," "));
        int sum=accumulate(max_substrlen.begin(),max_substrlen.end(),0);
        return sum;
    }
```	

468	Validate IP Address    		21.3%	Medium	
check ipv4 or ipv6
ipv4: 4 numbers separated by . from 0 to 255, leading zeros are invalid
ipv6: 8 hex numbers with 16 bits separated by :, leading zeros are legal


469	Convex Polygon    		35.4%	Medium	
470	Implement Rand10() Using Rand7()    		44.8%	Medium	
What if you could generate a random integer in the range 1 to 49? How would you generate a random integer in the range of 1 to 10? What would you do if the generated number is in the desired range? What if it is not?

Algorithm

This solution is based upon Rejection Sampling. The main idea is when you generate a number in the desired range, output that number immediately. If the number is out of the desired range, reject it and re-sample again. As each number in the desired range has the same probability of being chosen, a uniform distribution is produced.

Obviously, we have to run rand7() function at least twice, as there are not enough numbers in the range of 1 to 10. By running rand7() twice, we can get integers from 1 to 49 uniformly. Why?
    int rand10() {
        int row, col, idx;
        do {
            row = rand7();
            col = rand7();
            idx = col + (row - 1) * 7;
        } while (idx > 40);
        return 1 + (idx - 1) % 10;
    }
473	Matchsticks to Square    		35.9%	Medium	
different length, each stick is used just one time exactly.
this is divide the sticks into 4 equal part. using knapsack
dfs:
```cpp
    bool makesquare(vector<int>& nums) {
        //each side adds up to total/4
        if(nums.size()==0) return 0;
        int total=accumulate(nums.begin(),nums.end(),0);
        int side=total/4;
        if(total%4) return 0;
        unordered_set<int> visited;
        sort(nums.begin(),nums.end(),greater<int>());
        for(int i=0;i<4;i++)
        {
            if(!dfs(nums,side,0,visited)) return 0;
        }
        return visited.size()==nums.size();
    }
    bool dfs(vector<int>& sticks,int target,int start_ind,unordered_set<int>& visited)
    {
        if(target==0) return 1;
        if(target<0) return 0;
        for(int i=start_ind;i<sticks.size();i++)
        {
            if(visited.count(i)) continue;
            visited.insert(i);
            if(dfs(sticks,target-sticks[i],i+1,visited)) return 1;
            visited.erase(i); //put them back if failed
        }
        return 0;
    }
```
	


474	Ones and Zeroes    		39.7%	Medium	
max number of strings that you can form using m 0s and n 1s
dp knapsack
```cpp
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m+1,vector<int>(n+1));
        vector<vector<int>> cnt(strs.size(),vector<int>(2));
        for(int i=0;i<strs.size();i++)
        {
            //calculate 0s and 1s
            int m=0;//number of ones
            for(int j=0;j<strs[i].size();j++) m+=strs[i][j]-'0';
            cnt[i][0]=strs[i].size()-m;
            cnt[i][1]=m;
            //cout<<cnt[i][0]<<" "<<cnt[i][1]<<endl;
        }
        //iterate all the words
        for(int k=0;k<strs.size();k++)
        {
            for(int i=m;i>=cnt[k][0];i--)
            {
                for(int j=n;j>=cnt[k][1];j--)
                {
                    dp[i][j]=max(dp[i][j],dp[i-cnt[k][0]][j-cnt[k][1]]+1);
                }
            }
        }
        return dp[m][n];
    }
```	

477	Total Hamming Distance    		48.9%	Medium	
hamming distance is the number of different bits of two numbers
find total hamming distance between all pairs of numbers
```cpp
    int totalHammingDistance(vector<int>& nums) {
        //brutal force approach will have O(n^2)
        //generally if we cannot efficiently solve we can switch to bit by bit approach
        //we do bit by bit for all nums
        //if p numbers have 0 and q numbers have 1 on the same bit, the number of combination is choose one from p and choose one from q
        int res=0;
        for(int i=0;i<32;i++)
        {
            int nbits=0;
            for(int j=0;j<nums.size();j++)
            {
                if(nums[j]) {nbits+=nums[j]&1;nums[j]/=2;}
            }
            res+=nbits*(nums.size()-nbits);
        }
        return res;
    }
```	

	

481	Magical String    		46.1%	Medium	
the string is only 1 and 2. Contigurous occurence is the same as the string
given N as input, return the number of 1s in the first N number in the magical string.
122: also the counting
122: 2 1s followed
12211:  followed by 1 2
122112122122:
1 22 11 2 1 22 1 22
1  2   2   1 1 2   1  2 
```cpp
    int magicalString(int n) {
        string S = "122";
        int i = 2;
        while (S.size() < n)
            S += string(S[i++] - '0', S.back() ^ 3);
        return count(S.begin(), S.begin() + n, '1');
    }
```
so hard to understand, sigh	

484	Find Permutation    		57.5%	Medium	

487	Max Consecutive Ones II    		46.6%	Medium	

490	The Maze    		47.4%	Medium	
491	Increasing Subsequences    		42.0%	Medium	
find all increasing subsequence with length>=2
backtracking
```cpp
    vector<vector<int>> findSubsequences(vector<int>& nums) {
		vector<vector<int>> ans;
		dfs(nums,0,{},ans);
		return ans;
    }
	void dfs(vector<int>& nums,int start,vector<int> t,vector<vector<int>>& ans)
	{
		if(t.size()>=2) ans.push_back(t);
		if(start==nums.size()) return;
		
		for(int i=start;i<nums.size();i++){
			if(t.empty() || nums[i]>=t.back()){
				t.push_back(nums[i]);
				dfs(nums,i+1,t,ans);
				t.pop_back();
			}
		}
	}
			
```	
above solution will incur many duplicate solution for example:
[4,6,7,7]
4,6
	4,6,7
		4,6,7,7
		pop 7
	pop 7
	4,6,7 duplicate
pop 6
	4,7
		4,7,7
		pop 7
	pop 7
	4,7 duplicate
start from 6
	6,7
		6,7,7
		pop 7
	pop 7
	6,7 duplicate
start 7:
	7,7
	pop 7
- using a hashset we can eliminate duplicates
- add a visited data structure to avoid duplicates: the idea: if we meet a duplicate number we skip
A short explanation to the hash table: this is to record used numbers so that if we ever encounters a duplicate later, it will be skipped.
For example, if we have {4, 6(1), 7, 6(2), 6(3), ...}. At some point the hash table is {4, 6(1)}. When we visit 6(2) or 6(3), since we find 6(1) is already in the table, we skip to the next number.

494	Target Sum    		45.2%	Medium	
assign the array number + or -, to see how many ways to add to target
psum-nsum=target
psum+nsum=total
2*psum=target+total
so this is to check number of ways to get psum=(target+total)/2;
knapsack: 
```cpp
	int findTargetSumWays(vector<int>& nums, int S) {
		int tsum=accumulate(nums.begin(),nums.end(),0);
		if(abs(S)>tsum) return 0; //
		if((tsum+S)%2) return 0; //odd is impossible
		int target=(tsum+S)/2;
		vector<int> dp(target+1);
		dp[0]=1;
		for(int w=1;w<=target;w++){
			for(int i: nums){
				if(w>=i) dp[w]+=dp[w-i];
			}
		}
		return dp[target];
	}
```
sveral mistakes in above solution:
-. dp[w] need check all previous solution dp[i] i<w.
-. the same number cannot be used multiple times, so the iteration on nums shall be outside
-. iteration on w shall be reversed. we are using previous results!!!!
correct version:
```cpp
	int findTargetSumWays(vector<int>& nums, int S) {
		int tsum=accumulate(nums.begin(),nums.end(),0);
		if(abs(S)>tsum) return 0; //
		if((tsum+S)%2) return 0; //odd is impossible
		int target=(tsum+S)/2;
		vector<int> dp(target+1);
		dp[0]=1;
		for(int i: nums){
			for(int w=target;w>=i;w--){
				dp[w]+=dp[w-i];
			}
		}
		return dp[target];
	}
```	

518	Coin Change 2    		42.7%	Medium	
number of method to get the change of amount
knapsack which allows multiple usage
```cpp
    int change(int amount, vector<int>& coins) {
        if(amount == 0) return 1;
        if(coins.size() == 0) return 0;
        vector<int> dp(amount + 1, 0);//1d dp problem
        //dp[i]: number of changes for amount i using the coins
        dp[0] = 1;
        for(int i = 0; i < coins.size(); ++ i)
        {
            for(int j = coins[i]; j <= amount; ++ j)
            {
                dp[j] += dp[j - coins[i]];
            }
        }
        return dp[amount];
    }
```
similar see: 494. target sum


495	Teemo Attacking    		52.2%	Medium	
attacking and poison for a duration, return the total poison time.
this is the interval overlapping problem
note: understand the proble precisely, if it is in poison state, attack will not make the poison time longer.
```cpp
    int findPoisonedDuration(vector<int>& timeSeries, int duration) {
        //timeseries point is the left side point, duration is the length of the line segment
        //the problem needs to accumulate the line. timeseries is sorted
        //brutal force solution
        int tp=0;
        int tend=0;
        for(int i=0;i<timeSeries.size();i++)
        {
            tp+=duration-(timeSeries[i]<tend)*(tend-timeSeries[i]);
            tend=timeSeries[i]+duration;
        }
        return tp;
        
    }
```	

497	Random Point in Non-overlapping Rectangles    		35.8%	Medium	
a list of rectangles, random points bound in the rectangles.
rectangles are not overlapped so we can choose randomly a rect, and then randomly generate points.

```cpp
    vector<vector<int>> rect;
    vector<int> r_area;
    int total_area;
    Solution(vector<vector<int>> rects) {
        rect = rects;
        int total = 0;
        for (int i = 0; i < rects.size(); i++) {
            total += (rects[i][2] - rects[i][0]+1)*(rects[i][3] - rects[i][1]+1);
            r_area.push_back(total);
        }
        total_area = total;
    }
    
    vector<int> pick() {
        int random_area = rand()%total_area+1;
        int i = 0;
        for (; i < r_area.size(); i++) {
            if (random_area <= r_area[i]) break;
        }
        int dis_x = rand()%(rect[i][2] - rect[i][0]+1);
        int dis_y = rand()%(rect[i][3] - rect[i][1]+1);
        return {rect[i][0] + dis_x, rect[i][1] + dis_y};
    }
```	
random pick a rect using the area. points probability proportional to area.

498	Diagonal Traverse    		45.3%	Medium	
up diagnonal: i-j=const
down diagonal: i+j=const
```cpp
vector<int> findDiagonalOrder(vector<vector<int>>& matrix) {
        if(matrix.empty()) return {};
        
        const int N = matrix.size();
        const int M = matrix[0].size();
        
        vector<int> res;
        for(int s = 0; s <= N + M - 2; ++s)
        {
            // for all i + j = s
            for(int x = 0; x <= s; ++x) 
            {
                int i = x;
                int j = s - i;
                if(s % 2 == 0) swap(i, j);

                if(i >= N || j >= M) continue;
                
                res.push_back(matrix[i][j]);
            }
        }
        
        return res;
    }
```

503	Next Greater Element II    		51.1%	Medium	
circular array
using stack we first create the circular queue by doubling the size
```cpp
    vector<int> nextGreaterElements(vector<int>& nums) {
        //it is very important to understand what it needs: it needs the first one who is bigger on the circular path
        //approach: sort and keep its index and then build a map with index to next bigger one
        //brutal force O(n^2). if sorted with index, then find the min element index difference is also O(n^2)
        int n=nums.size();
        stack<int> st;
        vector<int> res(n,-1);
        for(int i=n-1;i>=0;i--) st.push(nums[i]);
        for(int i=n-1;i>=0;i--)
        {
            //cout<<st.size()<<":"<<st.top()<<endl;
            while(!st.empty() && st.top()<=nums[i]) st.pop();
            if(!st.empty()) res[i]=st.top();
            st.push(nums[i]);
        }
        for(int i=0;i<n;i++) cout<<res[i]<<" ";
        return res;
        
    }
```

505	The Maze II    		43.8%	Medium	

516	Longest Palindromic Subsequence    		46.7%	Medium	
dp:
```cpp
    int longestPalindromeSubseq(string s) {
        // dp[i][j] longest palindrome subsequence within s[i...j]
        //
        // dp[i][j] = dp[i+1][j-1] + 2 if s[i] == s[j]
        // dp[i][j] = max(dp[i+1][j], dp[i][j-1]) if s[i] != s[j]
        // 
        // dp[i][i] = 1. dp[i][i-1] = 0.
        //
        // To save space, use 1d array.
        // i: n-1 -> 0. j : i->n-1
        
        int n = s.size();
        vector<int> dp(n, 0);
        
        for(int i = n-1; i >= 0 ; --i) 
        {
            int p1, p2 = dp[i];
            dp[i] = 1;
            for(int j = i+1; j < n; ++j) 
            {
                p1 = dp[j];
                dp[j] = (s[i] == s[j]) ?  p2+2 : max(p1, dp[j-1]);
                p2 = p1;
            }
        }
        return dp[n-1];
    }
```
	

	
519	Random Flip Matrix    		33.1%	Medium
random flip, index 0 to m-1 and 0 to n-1
reservior sampling

522	Longest Uncommon Subsequence II    		32.9%	Medium	
see LUS I first. just check if the two string equal, otherwise, the longer one is the LUS
a series of string to get the LUS:
```cpp
    int findLUSlength(vector<string>& strs) {
        if (strs.empty()) return -1;
        int rst = -1;
        for(auto i = 0; i < strs.size(); i++) {
            int j = 0;
            for(j=0; j < strs.size(); j++) {
                if(i==j) continue;
                if(LCS(strs[i], strs[j])) break;  // strs[j] is a subsequence of strs[i]
            }
            // strs[i] is not any subsequence of the other strings.
            if(j==strs.size()) rst = max(rst, static_cast<int>(strs[i].size()));
        }
        return rst;
    }
    // iff a is a subsequence of b
    bool LCS(const string& a, const string& b) {
        if (b.size() < a.size()) return false;
        int i = 0;
        for(auto ch: b) {
            if(i < a.size() && a[i] == ch) i++;
        }
        return i == a.size();
    }
```
	

523	Continuous Subarray Sum    		24.2%	Medium	
sum=mK m>=2
k=0 or k!=0

```cpp
    bool checkSubarraySum(vector<int>& nums, int k) {
        //accumulate first and then check (a-b)%k==0
        vector<int> accum(nums.size()+1,0);
        unordered_map<int,vector<int>> cntmap;
        for(int i=0;i<nums.size();i++) accum[i+1]=accum[i]+nums[i];
        if(k)
			for(int i=1;i<accum.size();i++)
			{
				int t=accum[i]%k;
				cntmap[t].push_back(i); //same remainder, 3 the same must be true, 2 possibly be true
				if(cntmap[t].size()>2) return 1;
				if(cntmap[t].size()==2)
				{
					if(abs(cntmap[t][0]-cntmap[t][1])>1) return 1;
				}
			}
        else
			for(int i=0;i<accum.size();i++)
			{
				for(int j=i+2;j<accum.size();j++)
					if(accum[j]==accum[i]) return 1;
			}
            
        return 0;
        
    }
```	
524	Longest Word in Dictionary through Deleting    		45.7%	Medium	
Given a string and a string dictionary, find the longest string in the dictionary that can be formed by deleting some characters of the given string. If there are more than one possible results, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.
straightforward:
```cpp
    string findLongestWord(string s, vector<string>& d) {
        //edit distance or two pointers, batch matching also
        string ans;
        for(int i=0;i<d.size();i++)
        {
            if(isSub(d[i],s))
            {
                //cout<<d[i]<<endl;
                if(ans.empty()) ans=d[i];
                else
                {
                    if(d[i].size()>ans.size()) ans=d[i];
                    else if(d[i].size()==ans.size() && ans>d[i])ans=d[i];    
                }
            }
        }
        return ans;
    }
    bool isSub(string t, string s)
    {
        int i=0,j=0;
        for(int i=0;i<t.size();i++)
        {
            while(j<s.size() && t[i]!=s[j]) j++;
            if(j==s.size()) return 0;
        }
        return 1;
    }
```
	
525	Contiguous Array    		42.7%	Medium	
Given a binary array, find the maximum length of a contiguous subarray with equal number of 0 and 1.
```cpp
	int findMaxLength(vector<int>& nums) {
		//approach: convert to -1 1 series, if the sum is the same as before, it is the length, cumsum!
		map<int, int> myMap;
		map<int, int>::iterator it;
		int sum = 0;
		int maxLen = 0;
		myMap[0] = -1;
		for (int i = 0; i < nums.size(); i++)
		{
			sum += (nums[i] == 0) ? -1 : 1;
			it = myMap.find(sum); 
			if (it != myMap.end())
				maxLen = max(maxLen, i - it->second);
			else
				myMap[sum] = i;
		}
		return maxLen;
	}
```
526	Beautiful Arrangement    		54.6%	Medium	
number 1 to N, 
-1. number at ith position is divisible by i. (position is also from 1 to N)
-2. i is divisible by the number at ith position
return the number of constructions

backtracking swap any two valid permutations
```cpp
    int countArrangement(int N) {
        vector<int> vs(N);
        for(int i=0;i<N;i++) vs[i]=i+1;
        return count(vs,N);
    }
    int count(vector<int>& vs,int n)
    {
        if(n<=0) return 1;
        int ans=0;
        for(int i=0;i<n;i++)
        {
            if(vs[i]%n==0 || n%vs[i]==0)
            {
                swap(vs[i],vs[n-1]);
                ans+=count(vs,n-1);
                swap(vs[i],vs[n-1]);//restore it
            }
        }
        return ans;
    }
```
667	Beautiful Arrangement II    		51.9%	Medium	
given n and k, numbers are from 1 to n, the list shall be:
neigboring diff has exactly k distinct number, pick any valid 
```cpp
    vector<int> constructArray(int n, int k) {
        vector<int> vs(n);
        //for(int i=0;i<n;i++) vs[i]=i+1;
        //1,k+1,2,k,3,k-1,4,k-2.....but do not use duplicates
        //the distance is k,k-1......1
        //until they meet 
        int l=1,r=k+1;
        int cnt=0;
        while(l<=r) //when l+1==r r cannot be added
        {
            vs[cnt++]=l++;
            if(r>=l) vs[cnt++]=r--;//when r==l+1, the difference is 1
        }
        //we already used 1...k+1, the remaining difference is 1!
        for(int i=cnt;i<n;i++) vs[i]=i+1;
        return vs;
    }
```	
	
528	Random Pick with Weight    		42.8%	Medium	
each index has a weight, randomly pick index with weight.
accumulate the weight and binary search the region
```cpp
    vector<int> prefix;
    Solution(vector<int>& w) {
        prefix.push_back(0);
        for(int i: w) prefix.push_back(i+prefix.back());
    }
    
    int pickIndex() {
        int total=prefix.back();
        int num=rand()%total;
        //cout<<total<<" "<<num<<endl;
        auto it=upper_bound(prefix.begin(),prefix.end(),num)-prefix.begin();
        return it-1;
    }
```	

529	Minesweeper    		52.8%	Medium	
dfs:
```cpp
    vector<vector<char>> updateBoard(vector<vector<char>>& board, vector<int>& click) {
        if(board[click[0]][click[1]] == 'M')
        {
            board[click[0]][click[1]] = 'X';
            return board;
        }
        reveal(board,click[0],click[1]);
        return board;
    }
   
    void reveal(vector<vector<char>>& board, int x, int y){
         int dir[][2]={{0,1},{0,-1},{1,0},{-1,0},{1,1},{1,-1},{-1,1},{-1,-1}};
        int m=board.size(),n=board[0].size();
        if(board[x][y] == 'E')
        {
            //search 8 adjacent squares
            int count = 0;
            for(int i=0;i<8;i++)
            {
                int tx=x+dir[i][0],ty=y+dir[i][1];
                if(tx>=0 && tx<m && ty>=0 && ty<n && board[tx][ty]=='M') count++;
            }
            if(count>0) board[x][y] = '0'+count;
            else
            {
                board[x][y] = 'B';
                for(int i=0;i<8;i++)
                {
                    int tx=x+dir[i][0],ty=y+dir[i][1];
                    if(tx>=0 && tx<m && ty>=0 && ty<n) reveal(board,tx,ty);
                }
            }
        }
    }
```	

531	Lonely Pixel I    		57.5%	Medium	
533	Lonely Pixel II    		46.1%	Medium	
535	Encode and Decode TinyURL    		76.7%	Medium	
```cpp
    string dict = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    int id = 0;
    unordered_map<string,string> m;  //key is longURL, value is shortURL
    unordered_map<int, string> idm;  //key is id in DB, value is longURL
    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        if(m.find(longUrl) != m.end())return m[longUrl];
        string res = "";
        id++;
        int count = id;
        while(count > 0)
        {
            res = dict[count%62] + res;
            count /= 62;
        }
        while(res.size() < 6)
        {
            res = "0" + res;
        }
        m[longUrl] = res;
        idm[id] = longUrl;
        return res;
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
        int id = 0;
        for(int i = 0; i < shortUrl.size(); i++)
        {
            id = 62*id + (int)(dict.find(shortUrl[i]));
        }
        if(idm.find(id) != idm.end())return idm[id];
        return "";
    }
```	
536	Construct Binary Tree from String    		44.9%	Medium	
537	Complex Number Multiplication    		65.5%	Medium	
trivial
539	Minimum Time Difference    		47.9%	Medium	
time difference (need to wrap to 24 hours)
a day=1440 mins, and diff=min(diff,1440-diff)

540	Single Element in a Sorted Array    		57.5%	Medium	
every element appears twice except one
O(logn) and O(1)
need use binary search
observation:
length =2n+1 must be odd
the target has A[i-1]<A[i]<A[i+1] (case 1), otherwise we have A[i-1]==A[i]!=A[i+1] (case 2) or A[i-1]!=A[i]==A[i+1] (case 3)
if we meet case 1: done
if we meet case 2: and we have even number elements, the target is on the right, else on the left
if we meet case 3: similarly
```cpp
    int singleNonDuplicate(vector<int>& nums) {
        //use xor for single non duplicate, but for sorted array we need more intelligient
        //the array size shall be 2n+1. need binary search
        int n=nums.size();
        if(nums[0]!=nums[1]) return nums[0];
        if(nums[n-1]!=nums[n-2]) return nums[n-1];
        int l=0,r=n,mid=(r+l)/2;
        while(l<r)
        {
            mid=(r+l)/2;
            //cout<<l<<" "<<r<<" "<<mid<<"["<<nums[l]<<","<<nums[r]<<":"<<nums[mid-1]<<","<<nums[mid]<<","<<nums[mid+1]<<"]"<<endl;
            if(nums[mid]!=nums[mid-1] && nums[mid]!=nums[mid+1]) return nums[mid];
            if(nums[mid]==nums[mid-1]) //mid==left
            {
                //cout<<"left"<<endl;//note the odd even from left is not right when l starts from the 2nd number
                if((mid+1)%2==0) {l=mid+1;} //include the mid, left side is even
                else {r=mid-1;}
            }
            if(nums[mid]==nums[mid+1]) //mid=right
            {
                //cout<<"right"<<endl;
                if((n-mid)%2==0) {r=mid;} //include the mid, right side is even
                else{l=mid+1;}
            }
        }
    }
```


542	01 Matrix    		35.6%	Medium	
find the nearst distance to 0 for all cells
this is a very interesting bfs:
first level: all 1s has neighboring 0s. other pushed to queue (and marked as -1)
2nd level: all queued elements, if the neighboring has positives and distance=level (2), then we know it is nearest, otherwise add into queue again


first update neighboring cells using 0 cells (they are defined). If the non zero cell has neighboring cells, no change, otherwise mark it as -1. and add into queue (not processed)
other cells can be reached using bfs (choose the min):
pop one from queue, if its neighboring has a distance >0 then we use min_val+1 to update the cell

```cpp
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
      //first scan to update all distance=1
        //use a queue to keep all not solved elements
        int m=matrix.size(),n=matrix[0].size();
        vector<vector<int>> ans(m,vector<int>(n));
        
        queue<int> q;
        for(int i=0;i<m;i++)
            for(int j=0;j<n;j++) 
                if(matrix[i][j]) ans[i][j]=update_closest(matrix,i,j,q);
        //print(matrix);
        int dir[][2]={{0,1},{0,-1},{1,0},{-1,0}};
        //note process 1 first, 2 second,3,4,5....., otherwise there would have a problem depending on the update sequence
        int level=1;
        while(!q.empty()) //process the element in q: if there is an element is >0 around it, use the min()+1
        {
            int sz=q.size();
            for(int tt=0;tt<sz;tt++)
            {
                int t=q.front();q.pop();
                int i=t/n,j=t%n;
                int min_val=INT_MAX;
                for(int t=0;t<4;t++)
                {
                    int x=i+dir[t][0],y=j+dir[t][1];
                    if(x>=0 && x<m && y>=0 && y<n && ans[x][y]>0) 
                        min_val=min(min_val,ans[x][y]);
                }
                if(min_val==level) ans[i][j]=min_val+1;
                else {q.push(i*n+j);}
            }
            level++;
            //cout<<i<<" "<<j<<":"<<ans[i][j]<<endl;
            //cout<<"updated matrix:"<<endl;print(ans);
        }

        return ans;
    }
    
    int update_closest(vector<vector<int>>& matrix,int i,int j,queue<int>& q)
    {
        int m=matrix.size(),n=matrix[0].size();
        int dir[][2]={{0,1},{0,-1},{1,0},{-1,0}};
        for(int t=0;t<4;t++)
        {
            int x=i+dir[t][0],y=j+dir[t][1];
            if(x>=0 && x<m && y>=0 && y<n && !matrix[x][y]) return 1;
        }
        q.push(i*n+j);
        return -1;
    }
```
	

544	Output Contest Matches    		73.5%	Medium	
545	Boundary of Binary Tree    		35.0%	Medium	
547	Friend Circles    		53.6%	Medium	
dfs or disjoint set

548	Split Array with Equal Sum    		43.2%	Medium	
549	Binary Tree Longest Consecutive Sequence II    		44.3%	Medium	
553	Optimal Division    		55.4%	Medium	
simple math

554	Brick Wall    		47.6%	Medium	
n list of interval arrays in sorted order, in each interval array, there is no overlap
find the least number of overlaps 
accumulate to get each end point and put into a hashmap, the max frequency one is the answer


555	Split Concatenated Strings    		39.8%	Medium	
556	Next Greater Element III    		30.0%	Medium	
a number, return the smallest number > it which has the same digits
from right to left, find the first sorted position d[i]<d[i+1], swap with the smallest digit >d[i] and swap it, and then sort the right part.
(or reverse)
actually it is the next_permutation

560	Subarray Sum Equals K    		42.2%	Medium	
return the total number of subarrays

using hashmap, and keeps looking for sum-k in previous. if we found one we add one.
add a prefix sum=0 is necessary to include a[0] into it.

```cpp
    int subarraySum(vector<int>& nums, int k) {
        int cum=0; // cumulated sum
        unordered_map<int,int> rec; // prefix sum recorder
        int cnt = 0; // number of found subarray
        rec[0]++; // to take into account those subarrays that begin with 0
        for(int i=0;i<nums.size();i++)
        {
            cum += nums[i];
            cnt += rec[cum-k];
            rec[cum]++;
        }
        return cnt;
    }
```
	

562	Longest Line of Consecutive One in Matrix    		43.7%	Medium	
565	Array Nesting    		52.5%	Medium	
0 to n-1 elements, A[i], A[A[i]].... 
treat it as linked list and this is to find the largest cycle
note: every node is accessible. if one node is in a cycle, all nodes on the cycle 
straightforward:
```cpp
    int arrayNesting(vector<int>& nums) {
        //strategy: visited, it will form multiple circles and we keep the circle length
        int maxlen=0;
        int cycle_len=0;
        vector<bool> visited(nums.size());
        for(int i=0;i<nums.size();i++)
        {
            if(!visited[i]) //start a new cycle
            {
                cycle_len=1;
                int ind=nums[i];
                visited[i]=1;
                
                while(!visited[ind]) {visited[ind]=1;ind=nums[ind];cycle_len++;}
            }
            if(cycle_len>maxlen) maxlen=cycle_len;
        }
        return maxlen;
    }
```
	

567	Permutation in String    		38.4%	Medium	
check if a permutation of s1 is a substr in s2
sliding window with a hashmap, the two hashmap must equal
```cpp
	bool checkInclusion(string s1, string s2) {
		if(s2.size()<s1.size()) return 0;
		vector<int> cnt1(26),cnt2(26);
		for(char c: s1) cnt1[c-'a']++;
		//sliding window
		int w=s1.size();
		for(int i=0;i<s2.size();i++)
		{
			char c=s2[i];
			if(i>=w) cnt2[s2[i-w]-'a']--;
			cnt2[c-'a']++;
			if(i>=w-1 && cnt1==cnt2) return 1;
		}
		return 0;
	}
```	
573	Squirrel Simulation    		53.6%	Medium	
576	Out of Boundary Paths    		32.0%	Medium	
mxn grid, and start at i,j. move at most N times, find the number of path to move out of boundary
dfs or dp.
dfs with memoization
```cpp
	int findPaths(int m, int n, int N, int i0, int j0) {
		vector<vector<vector<long>>> dp(N+1,vector<vector<long>>(m,vector<long>(n,-1)));
		return dfs(m,n,N,i0,j0,dp);
	}
	long dfs(int m, int n, int N, int i, int j,vector<vector<vector<long>>>& dp)
	{
		if(i<0 || j<0 || i>=m || j>=n) return 1;
        if(N<=0) return 0;
        int mod=1e9+7;
        //cout<<N<<" "<<i<<" "<<j<<endl;
		if(dp[N][i][j]>=0) return dp[N][i][j];
		dp[N][i][j]=dfs(m,n,N-1,i+1,j,dp)%mod+
            dfs(m,n,N-1,i-1,j,dp)%mod+
            dfs(m,n,N-1,i,j+1,dp)%mod+
            dfs(m,n,N-1,i,j-1,dp)%mod;
        dp[N][i][j]%=mod;
		return dp[N][i][j];
	}```
note: 
- shall initialize as -1 since 0 could be an answer. 
- need use long to avoid overflow

	
		
582	Kill Process    		56.2%	Medium	
583	Delete Operation for Two Strings    		44.8%	Medium	
edit disance. delete in either string

592	Fraction Addition and Subtraction    		47.0%	Medium	
just use some math

593	Valid Square    		40.5%	Medium	
a square shall have 4 sides the same two diagonal the same

609	Find Duplicate File in System    		55.1%	Medium	
map the contents with its path

611	Valid Triangle Number    		45.1%	Medium	
return the number of valid triangles.
a+b>c is the triangle
straightforward.
```cpp
    int triangleNumber(vector<int>& nums) {
       //triangle any a+b>c any a-b<c so use the two min for add, use the max and min for subract
        //combination: C(n,3)
        //a natural solution: sort the sides and for a pair of sides, the final side selection is limited
        //also only need a+b>c sufficient
        //need consider duplicates: note the problem includes all duplicates
        sort(nums.begin(),nums.end());
        int n=nums.size();
        int num_tri=0;
        for(int i=0;i<n-2;i++)
        {
            for(int j=i+1;j<n-1;j++)
            {
                int t=nums[i]+nums[j];
                auto it=lower_bound(nums.begin()+j+1,nums.end(),t);
                int k=distance(nums.begin()+j+1,it);
                num_tri+=k;
            }
        }
        return num_tri;
    }
```	
616	Add Bold Tag in String    		39.1%	Medium	
621	Task Scheduler    		45.5%	Medium	
task A to Z, same task needs has a interval >=n
greedy: hashmap to record task frequency
```cpp
    int leastInterval(vector<char>& tasks, int n) {
        unordered_map<char,int>mp;
        int count = 0;
        for(auto e : tasks)
        {
            mp[e]++;
            count = max(count, mp[e]);
        }
        
        int ans = (count-1)*(n+1);
        for(auto e : mp) if(e.second == count) ans++;
        return max((int)tasks.size(), ans);
    }
```
	
622	Design Circular Queue    		39.1%	Medium	
ring buffer
```cpp
    vector<int> q;
    int head,tail;
    int sz;
public:
    /** Initialize your data structure here. Set the size of the queue to be k. */
    MyCircularQueue(int k) {
        q.resize(k);
        sz=head=tail=0;
    }
    
    /** Insert an element into the circular queue. Return true if the operation is successful. */
    bool enQueue(int value) {
        if(isFull()) return 0;
        q[tail++]=value;
        tail%=q.size();
        sz++;
        return 1;
    }
    
    /** Delete an element from the circular queue. Return true if the operation is successful. */
    bool deQueue() {
        if(isEmpty()) return 0;
        head++;head%=q.size();
        sz--;
        return 1;
    }
    
    /** Get the front item from the queue. */
    int Front() {
        if(isEmpty()) return -1;
        return q[head];
    }
    
    /** Get the last item from the queue. */
    int Rear() {
        if(isEmpty()) return -1;
        return q[(tail-1+q.size())%q.size()];
    }
    
    /** Checks whether the circular queue is empty or not. */
    bool isEmpty() {
        return sz==0;
    }
    
    /** Checks whether the circular queue is full or not. */
    bool isFull() {
        return sz==q.size();
    }
```	
641	Design Circular Deque    		49.2%	Medium
```cpp
    vector<int> dq;
    int sz,head,tail;
public:
    /** Initialize your data structure here. Set the size of the deque to be k. */
    MyCircularDeque(int k) {
        dq.resize(k);
        sz=head=tail=0;
    }
    
    /** Adds an item at the front of Deque. Return true if the operation is successful. */
    bool insertFront(int value) {
        if(isFull()) return 0;
        dq[head++]=value;
        sz++;
        head%=dq.size();
        return 1;
    }
    
    /** Adds an item at the rear of Deque. Return true if the operation is successful. */
    bool insertLast(int value) {
        if(isFull()) return 0;
        tail--;
        tail=(tail+dq.size())%dq.size();
        dq[tail]=value;
        sz++;
        
        return 1;
    }
    
    /** Deletes an item from the front of Deque. Return true if the operation is successful. */
    bool deleteFront() {
        if(isEmpty()) return 0;
        head--;
        head=(head+dq.size())%dq.size();
        sz--;
        return 1;
    }
    
    /** Deletes an item from the rear of Deque. Return true if the operation is successful. */
    bool deleteLast() {
        if(isEmpty()) return 0;
        tail++;
        tail=(tail+dq.size())%dq.size();
        sz--;
        return 1;
    }
    
    /** Get the front item from the deque. */
    int getFront() {
        if(isEmpty()) return -1;
        return dq[(head-1+dq.size())%dq.size()];
    }
    
    /** Get the last item from the deque. */
    int getRear() {
        if(isEmpty()) return -1;
        return dq[tail];
    }
    
    /** Checks whether the circular deque is empty or not. */
    bool isEmpty() {
        return sz==0;
    }
    
    /** Checks whether the circular deque is full or not. */
    bool isFull() {
        return sz==dq.size();
    }
```


623	Add One Row to Tree    		47.3%	Medium	
given a layer add a layer of nodes
bfs:
first find the layer d-1, using bfs
then: create the nodes and link to child. the depth can be maxdepth+1

```cpp
    TreeNode* addOneRow(TreeNode* root, int v, int d) {
        //using bfs
        if(d==1) //as the root
        {
            TreeNode *p=new TreeNode(v);
            p->left=root;
            return p;
        }
        int h=height(root);
        queue<TreeNode*> q;
        //queue<bool> lr;
        q.push(root);
        //lr.push(0);
        for(int i=1;i<min(d-1,h);i++) 
        {
            int sz=q.size();
            for(int j=0;j<sz;j++)
            {
                TreeNode* p=q.front();q.pop();;
                if(p->left) {q.push(p->left);}
                if(p->right) {q.push(p->right);}
            }
        }
        int sz=q.size();
        for(int i=0;i<sz;i++)
        {
            //create a node and swap the content with node in q
            TreeNode* pnew=new TreeNode(v);
            TreeNode* pold=q.front();q.pop();
            //cout<<pold->val<<" ";
            TreeNode *tmp=pold->left;
            pold->left=new TreeNode(v);
            pold->left->left=tmp;
            tmp=pold->right;
            pold->right=new TreeNode(v);
            pold->right->right=tmp;
        }
        return root;
    }
    int height(TreeNode* root)
    {
        if(!root) return 0;
        return 1+max(height(root->left),height(root->right));
    }
```	

625	Minimum Factorization    		31.9%	Medium	
634	Find the Derangement of An Array    		37.6%	Medium	
635	Design Log Storage System    		54.0%	Medium	
636	Exclusive Time of Functions    		48.7%	Medium	
single thread, function id: start: starttime, function id:end:endtime
a typical stack problem
need to pay attention: when one function is running, all other functions are suspended so the time needs to be subtracted.
```cpp
    vector<int> exclusiveTime(int n, vector<string>& logs) {
        //when it starts pushed in the stack, when it ends, pop it up and store in array
        vector<int> ans(n);
        stack<pair<int,int>> st; //id, time
        for(int i=0;i<logs.size();i++)
        {
            int ind=logs[i].find_first_of(':');
            int id=stoi(logs[i].substr(0,ind));
            int ind1=logs[i].find_first_of(':',ind+1);
            if(ind1-ind==6) //start
            {
                int stime=stoi(logs[i].substr(ind1+1));
                st.push(make_pair(id,stime));
            }
            else //endtime
            {
                int etime=stoi(logs[i].substr(ind1+1));
                int dtime=etime-st.top().second+1;
                ans[id]+=dtime;
                st.pop();
                if(!st.empty()) ans[st.top().first]-=dtime;//equivalent to add the time to the start
            }
        }
        return ans;
    }
```	


638	Shopping Offers    		48.6%	Medium	
dp, knapsack or recursive
```cpp
void operator+=(vector<int> &a, const vector<int> &b) {
    for (int i = 0; i < a.size(); i++)
        a[i] += b[i];
}

void operator-=(vector<int> &a, const vector<int> &b) {
    for (int i = 0; i < a.size(); i++)
        a[i] -= b[i];
}

bool operator<(const vector<int> &a, const int &n) {
    for (int i : a)
        if (i < n)
            return true;
    return false;
}

int operator*(const vector<int> &a, const vector<int> &b) {
    int res = 0;
    for (int i = 0; i < a.size(); i++)
        res += a[i] * b[i];
    return res;
}    
class Solution {
public:
    int shoppingOffers(vector<int>& price, vector<vector<int>>& special, vector<int>& needs, int cost = 0) {
        if (needs < 0) return INT_MAX;

        int m = cost + needs * price;//buy everything separately

        for (auto &offer : special) 
        {
            if (cost + offer.back() >= m) // pruning
                continue;
            needs -= offer; //add this offer
            m = min(m, shoppingOffers(price, special, needs, cost + offer.back()));
            needs += offer; //put it back (pop)
        }

        return m;
    }
```
	
640	Solve the Equation    		40.3%	Medium	
use simple math

	
646	Maximum Length of Pair Chain    		48.7%	Medium	
dp, similar to longest increasing subsequence
```cpp
    int findLongestChain(vector<vector<int>>& pairs) {
        sort(pairs.begin(),pairs.end()); //sort with start
        vector<int> dp(pairs.size(),1);
        for(int i=1;i<pairs.size();i++)
        {
            //dp[i] is the number of longest chain for ith pair, it shall be the previous chain+1
            for(int j=i-1;j>=0;j--)
            {
                if(pairs[j][1]<pairs[i][0]) {dp[i]=dp[j]+1;break;}
            }
        }
        return dp[pairs.size()-1];
    }
```	

647	Palindromic Substrings    		56.9%	Medium	
count the number of palindromic substrings in a string
dp:
```cpp
    int countSubstrings(string s) {
        //dp solution: using 2d approach
        //dp[i,j]=self+dp[i,j-1]+dp[i+1,j]-dp[i+1,j-1]
        //dp[i,j] is the number of p-string for S(i...j)
        //i: reverse, j: normal
        int n=s.size();
        vector<vector<int>> dp(n,vector<int>(n));
        for(int i=0;i<n;i++) dp[i][i]=1; //itself is a p-string
        
        for(int i=n-2;i>=0;i--)
        {
            for(int j=i+1;j<n;j++) //j>i must be met
            {
                int self=isPalindrome(s,i,j);
                dp[i][j]=self+dp[i][j-1]+dp[i+1][j]-dp[i+1][j-1];
            }
        }
        return dp[0][n-1];
    }
    int isPalindrome(string& s,int i,int j)
    {
        //two pointer early exit
        while(i<j) 
        {
            if(s[i]==s[j]) {i++,j--;} else return 0;
        }
        return 1;
    }
```	

648	Replace Words    		51.8%	Medium	
replace the words with its prefix root. If there are multiple, choose the shortest one.
using trie and startWith



649	Dota2 Senate    		37.6%	Medium	
greedy, ban its next 
```cpp
string predictPartyVictory(string senate) {
        queue<int> q1, q2;
        int n = senate.length();
        for(int i = 0; i<n; i++)
            (senate[i] == 'R')?q1.push(i):q2.push(i);
        while(!q1.empty() && !q2.empty()){
            int r_index = q1.front(), d_index = q2.front();
            q1.pop(), q2.pop();
            (r_index < d_index)?q1.push(r_index + n):q2.push(d_index + n);
        }
        return (q1.size() > q2.size())? "Radiant" : "Dire";
    }
```
	
650	2 Keys Keyboard    		46.6%	Medium	
copy all and paste, begin with a A, you need to use min number of steps to reach n A
from 1 to n by doubling or reverse from n to 1 by cutting by half.
```cpp
    int minSteps(int n) {
        vector<int> dp(n+1);
        dp[1]=0;dp[2]=2;dp[3]=3;
        for(int i=4;i<=n;i++)
        {
            if(i%2) //odd number always need one copy,even number of paste 2,4,6,8...can only from its max divisor
            {
                int m=max_divisor(i);
                dp[i]=dp[m]+i/m;
            }
            else //even number always need one copy, odd number of paste
            {
                if(i%2==0) dp[i]=dp[i/2]+2;
                if(i%4==0) dp[i]=min(dp[i],dp[i/4]+4);
            }
        }
        return dp[n];
    }
    
    int max_divisor(int n) //find the max divisor except itself
    {
        int res=1;
        for(int i=2;i<n/2;i++) if(n%i==0) res=i;
        return res;
    }
```
	

651	4 Keys Keyboard    		50.5%	Medium	
652	Find Duplicate Subtrees    		45.5%	Medium	
serialize the tree into string and store in hashmap
```cpp
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        unordered_map<string,int> mp;
        vector<TreeNode*> res;
        postorder(root,mp,res);
        return res;
    }
    string postorder(TreeNode* root,unordered_map<string,int>& mp,vector<TreeNode*>& v)
    {
        if(!root) return "# ";
        string serial=to_string(root->val)+" "+postorder(root->left,mp,v)+" "+postorder(root->right,mp,v)+" ";
        if(mp.count(serial) && mp[serial]==1) {v.push_back(root);}
        mp[serial]++;
        return serial;
    }
```	
654	Maximum Binary Tree    		76.0%	Medium	
from array, root is the max in the array and left is the left part array and right is the right part array
construct the tree
divide and build
```cpp
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return divide_build(nums,0,nums.size());
    }
    TreeNode* divide_build(vector<int>& nums,int l,int r) //use [l,r) range
    {
        if(l>=r) return 0;
        int ind=(int)(max_element(nums.begin()+l,nums.begin()+r)-nums.begin());
        TreeNode* root=new TreeNode(nums[ind]);
        root->left=divide_build(nums,l,ind);
        root->right=divide_build(nums,ind+1,r);
        return root;
    }
```
	
655	Print Binary Tree    		51.9%	Medium	
root is always on the mid of the two child
```cpp
    vector<vector<string>> printTree(TreeNode* root) {
        //the column is dependent on the height ncol=ncol_prev+2^h
        //on each level, the element's position is also fixed: they shall be in the middle of each subsection
        //m=h, n=2^h-1
        
        if(!root) return vector<vector<string>>();
        int m=treeheight(root),n=pow(2,m)-1;
        vector<vector<string>> res(m,vector<string>(n));
        //recursively determine each element's position
        print_helper(root,0,0,n-1,res);
        return res;
    }
    int treeheight(TreeNode* root)
    {
        if(!root) return 0;
        return max(treeheight(root->left),treeheight(root->right))+1;
    }
    void print_helper(TreeNode* root,int level,int l,int r,vector<vector<string>>& res)
    {
        if(!root) return;
        int mid=l+(r-l)/2;
        res[level][mid]=to_string(root->val);
        print_helper(root->left,level+1,l,mid-1,res);
        print_helper(root->right,level+1,mid+1,r,res);
    }
```
	
658	Find K Closest Elements    		38.0%	Medium	
in sorted array, given x and find the k closest elements in the array.
if there is a tie, the smaller number is preferred.
sort using abs(n-x)
```cpp
    struct cmp
    {
        int x;
        cmp(int a):x(a){};
        bool operator()(int a,int b) {return abs(a-x)<abs(b-x);}
    };
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        //sort using abs(a-x)
        sort(arr.begin(),arr.end(),cmp(x));
        vector<int> ans(arr.begin(),arr.begin()+k);
        sort(ans.begin(),ans.end());
        return ans;
    }
```
	

659	Split Array into Consecutive Subsequences    		40.7%	Medium	
split array into consecuative intergers with at least 3 numbers, check if we can do it
```cpp
    bool isPossible(vector<int>& nums) {
        unordered_map<int, int> freq, need;
        for (int num : nums) ++freq[num];
        for (int num : nums) {
            if (freq[num] == 0) continue;
            else if (need[num] > 0) {
                --need[num];
                ++need[num + 1];
            } else if (freq[num + 1] > 0 && freq[num + 2] > 0) {
                --freq[num + 1];
                --freq[num + 2];
                ++need[num + 3];
            } else return false;
            --freq[num];
        }
        return true;
    }
```
	
662	Maximum Width of Binary Tree    		39.7%	Medium	
the child node for a full binary tree: parent i, and child 2*i, 2*i+1
this will soon overflow for a long linked list like tree
using bfs to set the left as 0 for each layer to avoid overflow


663	Equal Tree Partition    		38.0%	Medium	
666	Path Sum IV    		52.2%	Medium	

670	Maximum Swap    		39.6%	Medium	
a non-negative number, swap at most one time to get the max valued number
for example: 2736->7236, leftmost sorted swap with is right max
```cpp
    int maximumSwap(int num) {
        //this is a selection sort
        string s=to_string(num);
        int len=s.size();
        //char maxc=*max_element(s.begin(),s.end());
        int pos;
        int firstpos=INT_MAX;
        for(int i=0;i<s.size();i++)
        {
            int ind=(int)(max_element(s.rbegin(),s.rbegin()+len-1-i)-s.rbegin());//this gives the first max
            ind=len-1-ind;
            if(s[i]<s[ind])
            {
                swap(s[i],s[ind]);
                break;
            }
        }
        //swap the last max with the first non-max
        return stoll(s);
    }
```
	
672	Bulb Switcher II    		50.0%	Medium	
```cpp
    int flipLights(int n, int m) {
        if (m == 0 || n == 0) return 1;
        if (n == 1) return 2;
        if (n == 2) return m == 1? 3:4;
        if (m == 1) return 4;
        return m == 2? 7:8;
    }
```	
673	Number of Longest Increasing Subsequence    		33.6%	Medium	
unsorted array, find the number of longest increasing subsequence
dp: need find the longest subsequence and also need to record the number
```cpp
    int findNumberOfLIS(vector<int>& nums) {
        //dp solution
        //vlen[k+1]: the length of the longest subsequence for nums[k]
        //cnt[k+1]: the number of longest subsequence for nums[k]
        //vlen[k+1]=max(vlen[i])
        //cnt[k+1]=sum(cnt(i)) for all cnt[i] with vlen[i]+1=vlen[k+1]
        int n=nums.size(),ans=0;
        vector<int> vlen(n,1),cnt(n,1);
        int maxlen=1;
        for(int i=1;i<n;i++)
        {
            for(int j=0;j<i;j++)
            {
                if(nums[i]>nums[j])
                {
                    if(vlen[i]<vlen[j]+1) {vlen[i]=vlen[j]+1;cnt[i]=cnt[j];}
                    else if(vlen[i]==vlen[j]+1) cnt[i]+=cnt[j];
                }
            }
            maxlen=max(maxlen,vlen[i]);
        }
        for(int i=0;i<n;i++) 
        {if(vlen[i]==maxlen) ans+=cnt[i];}
        return ans;
    }
```
	
676	Implement Magic Dictionary    		51.7%	Medium	
given a list of words, and check if a word modified exactly one char, and it is in the dictionary
either build the map with all possible combinations and then check if one combination matches

```cpp
    unordered_map<string,set<string>> mydict;
    MagicDictionary() {
        
    }
    
    /** Build a dictionary through a list of words */
    void buildDict(vector<string> dict) {
        for(int i=0;i<dict.size();i++) 
        {
            for(int j=0;j<dict[i].size();j++)
            {
                string s=dict[i];
                s[j]='$';
                mydict[s].insert(dict[i]);
            }
        }
    }
    
    /** Returns if there is any word in the trie that equals to the given word after modifying exactly one character */
    bool search(string word) {
        //try replace the word 
        for(int i=0;i<word.size();i++)
        {
            string s=word;
            s[i]='$';
            //if match one
            if(mydict.count(s) && (!mydict[s].count(word) || (mydict[s].size()>1))) return 1;//need check other matches
        }
        return 0;
    }
```
	
677	Map Sum Pairs    		51.5%	Medium	
insert string integer, sum return the sum of all key words with it as prefix.
using trie: record the val, find the prefix and use dfs for all path containing the prefix
```cpp
struct Node
{
    Node* next[26]; //use pointer is much convenient than using array!
    bool is_leaf;
    int val;
    Node(bool b=0) {fill(next,next+26,(Node*)0);is_leaf=b;val=0;}
};
class Trie {
public:
    /** Initialize your data structure here. */
    Node* root;
    Trie() {
        root=new Node();
    }
    
    /** Inserts a word into the trie. */
    Node* insert(string word) {
        Node* p=root;
        for(int i=0;i<word.size();i++)        
        {
            int ind=word[i]-'a';
            if(!p->next[ind]) p->next[ind]=new Node();
            p=p->next[ind];
        }
        p->is_leaf=1;
        return p;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        Node* p=find(word);
        return p && p->is_leaf; //has to end with a leaf node
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Node* p=find(prefix);
        return p; //does not have to end with a leaf node
    }
    
    Node* find(string word)
    {
        Node* p=root;
        for(int i=0;i<word.size();i++)
        {
            int ind=word[i]-'a';
            if(p->next[ind]==0) return 0;
            p=p->next[ind];
        }
        return p;
    }
};

class MapSum {
public:
    /** Initialize your data structure here. */
    Trie t;
    MapSum():t(Trie()) {
        
    }
    
    void insert(string key, int val) {
        Node*p=t.insert(key);
        p->val=val;
    }
    
    int sum(string prefix) {
        Node* p=t.find(prefix);
        int ans=0;
        //dfs search all paths to sum the values
        dfs(p,ans);
        return ans;
    }
    void dfs(Node* root,int& tsum)
    {
        //if(root->is_leaf) {tsum+=root->val;return;}
        if(!root) return;
        tsum+=root->val;
        for(int i=0;i<26;i++)
        {
            if(root->next[i]) dfs(root->next[i],tsum);
        }
    }
```	

678	Valid Parenthesis String    		32.7%	Medium	
contain (*) * can be treated as ( or ) or a char
```cpp
    bool checkValidString(string s) {
        //using stack, 
        return is_validstring(s,0,0,0);
    }
    bool is_validstring(const string& s,int i,int nleft,int nright)
    {
        if(nleft<nright) return 0;
        if(i==s.length())
        {
            return nleft==nright;
        }
        if(s[i]=='(') return is_validstring(s,i+1,nleft+1,nright);
        if(s[i]==')') return is_validstring(s,i+1,nleft,nright+1);
        else //use it as left, right or nothing
        {
            return is_validstring(s,i+1,nleft,nright) || is_validstring(s,i+1,nleft+1,nright) || is_validstring(s,i+1,nleft,nright+1);
        }
    }
```
	

681	Next Closest Time    		42.6%	Medium	
684	Redundant Connection    		51.8%	Medium	
undirected graph, return an edge that can be removed to make it as a tree. if there are multiple answer,return the last edge in the list.
cycle detection
```cpp
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n=edges.size();
        vector<int> parents(2*n);
        for(int i=0;i<2*n;i++) parents[i]=i;
        for(int i=0;i<n;i++)
        {
            int f=edges[i][0],t=edges[i][1];
            if(find(parents,f)==find(parents,t)) return edges[i];
            else parents[find(parents,f)]=find(parents,t);
        }
        return vector<int>(2);
    }
    int find(vector<int>& parent,int f)
    {
        while(f!=parent[f]) f=parent[f];
        return f;
    }
```	

688	Knight Probability in Chessboard    		44.3%	Medium	
move exactly k steps
```cpp
    double memo[100][25][25]={0};
    //vector<vector<vector<bool>>> visited(100,vector<vector<int>>(25,vector<int>(25)));
    double knightProbability(int N, int K, int r, int c) {
        //there are 8 directions
        //check how many are outside and how many inside. if the possibility is 0 stop
        //if steps are exhausted, stop! The probability is the product of all its 
        //double res=0;
    
        //cnt+=inside(r+1,c+2,N)+inside(r+1,c-2,N)+inside(r+2,c+1,N)+inside(r+2,c-1,N);
        //cnt+=inside(r-1,c+2,N)+inside(r-1,c-2,N)+inside(r-2,c+1,N)+inside(r-2,c-1,N);
        if(outside(r,c,N)) return 0;
        if(K==0) return 1;
        if(N<3) return 0;
        
        if(memo[K-1][r][c]>0) return memo[K-1][r][c];
        double res1=0;
        res1+=knightProbability(N,K-1,r+1,c+2)/8;
        res1+=knightProbability(N,K-1,r+1,c-2)/8;
        res1+=knightProbability(N,K-1,r+2,c+1)/8;
        res1+=knightProbability(N,K-1,r+2,c-1)/8;
        res1+=knightProbability(N,K-1,r-1,c+2)/8;
        res1+=knightProbability(N,K-1,r-1,c-2)/8;
        res1+=knightProbability(N,K-1,r-2,c+1)/8;
        res1+=knightProbability(N,K-1,r-2,c-1)/8;
        memo[K-1][r][c]=res1;
        return res1;
    }
    bool outside(int r,int c,int N)
    {
        return (r<0 || r>=N || c<0 || c>=N);
    }
```
	
692	Top K Frequent Words    		45.7%	Medium	
a list of words, return the top k frequency words
if two have the same frequency, the smaller comes first
priority-queue
```cpp
struct cmp
{
   bool operator()(const pair<string,int>&a, const pair<string,int>& b)  
   {
       return a.second<b.second ||(a.second==b.second && a.first>b.first);
   }
};
class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        //calculate histogram of each words and sort these words
        unordered_map<string,int> wmap;
        for(int i=0;i<words.size();i++) wmap[words[i]]++;
        priority_queue<pair<string,int>,vector<pair<string,int>>,cmp> pq(wmap.begin(),wmap.end());
        vector<string> vs(k);
        for(int i=0;i<k;i++) {vs[i]=pq.top().first;pq.pop();}
        return vs;
    }
```
	
694	Number of Distinct Islands    		51.2%	Medium	
bfs

695	Max Area of Island    		57.2%	Medium	
coloring and dfs
```cpp
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int m=grid.size(),n=grid[0].size();
        int maxarea=0;
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]==1) maxarea=max(maxarea,dfs(grid,i,j));
            }
        }
        return maxarea;
    }
    int dfs(vector<vector<int>>& grid,int i,int j)
    {
        int m=grid.size(),n=grid[0].size();
        if(i<0 || j<0 || i>=m || j>=n) return 0;
        if(grid[i][j]!=1) return 0;
        grid[i][j]=2;
       return dfs(grid,i-1,j)+dfs(grid,i+1,j)+dfs(grid,i,j-1)+dfs(grid,i,j+1)+1;
    }
```
	
698	Partition to K Equal Sum Subsets    		42.3%	Medium	
find if it is possible to parition into k subsets
first, tsum shall be divisible by k, 2nd, max can not be greater than target
backtracking
```cpp
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int sum = 0;
        for(int num:nums)sum+=num;
        if(k <= 0 || sum%k != 0)return false;
        vector<int> visited(nums.size(), 0);
        return canPartition(nums, visited, 0, k, 0, 0, sum/k);
    }
    
    bool canPartition(vector<int>& nums, vector<int>& visited, int start_index, int k, int cur_sum, int cur_num, int target){
        if(k==1)return true;
        if(cur_sum == target && cur_num >0 )return canPartition(nums, visited, 0, k-1, 0, 0, target);
        for(int i = start_index; i<nums.size(); i++){
            if(!visited[i]){
                visited[i] = 1;
                if(canPartition(nums, visited, i+1, k, cur_sum + nums[i], cur_num++, target))return true;
                visited[i] = 0;
            }
        }
        return false;
    }
```
	
701	Insert into a Binary Search Tree    		75.7%	Medium	
```cpp
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if(!root) return new TreeNode(val);
        if(val>root->val) root->right=insertIntoBST(root->right,val);
        else root->left=insertIntoBST(root->left,val);
        return root;
    }
```
	
702	Search in a Sorted Array of Unknown Size    		58.7%	Medium	

708	Insert into a Cyclic Sorted List    		28.9%	Medium	
712	Minimum ASCII Delete Sum for Two Strings    		54.5%	Medium	
dp with edit distance

713	Subarray Product Less Than K    		36.7%	Medium	
sliding window with two pointer
```cpp
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        int n=nums.size();
        //cout<<n;
        if(k<=*min_element(nums.begin(),nums.end())) return 0;
        //if(*max_element(nums.begin(),nums.end())<2)
            //return n*(n+1)/2;
        //use two pointers to find the region
        int pi=0,pj=0;
        int ans=0;
        int prod=1;
        while(pi<n)
        {
            for(;pj<n;pj++)
            {
                
                if(prod*nums[pj]>=k) break;
                prod*=nums[pj];
            }
            int m=pj-pi;
            ans+=m;
            //cout<<nums[pi]<<" "<<m<<":"<<ans<<endl;
            prod/=nums[pi];
            if(pi==pj) {pj++;prod=1;}
            pi++;
        }
        return ans;
    }
```	
714	Best Time to Buy and Sell Stock with Transaction Fee    		50.4%	Medium	
dp
718	Maximum Length of Repeated Subarray    		46.0%	Medium	
max length of subarray appeared in both
longest common subarray, dp
```cpp
    int findLength(vector<int>& A, vector<int>& B) {
        //longest common substring
        int m=A.size(),n=B.size();
        vector<vector<int>> dp(m+1,vector<int>(n+1)); 
        //dp[i, j] represents the longest common subarray between A[0..i-1] and B[0...j-1] 
        //boundary: dp[0][0]=0, 0th row and 0th column all zero
        int max0=0;
        for(int i=1;i<=m;i++)
        {
            for(int j=1;j<=n;j++)
            {
                if(A[i-1]==B[j-1]) dp[i][j]=dp[i-1][j-1]+1;
                //else dp[i][j]=0;
                max0=max(max0,dp[i][j]);
            }
        }
        return max0;
    }
```
	
721	Accounts Merge    		40.4%	Medium	
user, a list of emails
if several user has a common email, then it is the same user account
using union-find
```cpp
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        int n=accounts.size();
        //make the disjoint set using the index
        unordered_map<string,int> email_parent;
        vector<int> parent(n);
        for(int i=0;i<n;i++)
        {
            parent[i]=i; //initially it points itself
            for(int j=1;j<accounts[i].size();j++) 
            {
                if(email_parent.count(accounts[i][j])) //this email has appeared
                {
                    merge(parent,i,email_parent[accounts[i][j]]);
                }
                else email_parent[accounts[i][j]]=i;
            }
        }
        //now each set logical relation is formed using disjoint set
        //using set to remove duplicates and sort them
        unordered_map<int,set<string>> parent_email;
        for(auto it=email_parent.begin();it!=email_parent.end();it++)
        {
            int id=find(parent,it->second);
            parent_email[id].insert(it->first);
        }
        //form the final answer
        vector<vector<string>> ans;
        for(auto it=parent_email.begin();it!=parent_email.end();it++)
        {
            ans.push_back(vector<string>());
            ans.back().push_back(accounts[it->first][0]); //owner name
            ans.back().insert(ans.back().begin()+1,it->second.begin(),it->second.end());
        }
        return ans;
    }
    int find(vector<int>& parent,int i) //find its parent id
    {
        while(parent[i]!=i) i=parent[i];
        return i;
    }
    
    void merge(vector<int>& parent,int i,int j)
    {
        int i_id=find(parent,i);
        int j_id=find(parent,j);
        if(i_id==j_id) return;
        if(i_id<j_id) parent[j_id]=i_id;
        else parent[i_id]=j_id;
    }
```	

722	Remove Comments    		31.2%	Medium	
using regex is too hard
```cpp
    vector<string> removeComments(vector<string>& s) {
        vector<string> ans;
        bool inBlock = false;
        string sf;
        for (auto &t:s) {
            for (int i = 0; i < t.size();) {
                if (!inBlock) {
                    if (i + 1 == t.size()) sf += t[i++];
                    else {
                        string m = t.substr(i,2);
                        if (m == "/*") inBlock = 1, i+=2;
                        else if (m == "//") break;
                        else sf += t[i++];
                    }
                }
                else {
                    if (i + 1 == t.size()) i++;
                    else {
                        string m = t.substr(i,2);
                        if (m == "*/") inBlock = 0, i+=2;
                        else i++;
                    }
                }
            }
            if (sf.size() && !inBlock) ans.push_back(sf), sf = "";
        }
        return ans;
    }
```
	
725	Split Linked List in Parts    		48.9%	Medium	
given k, divide it into k parts
first get the whole length
```cpp
    vector<ListNode*> splitListToParts(ListNode* root, int k) {
        vector<ListNode*> parts(k, nullptr);
        int len = 0;
        for (ListNode* node = root; node; node = node->next)
            len++;
        int n = len / k, r = len % k; // n : minimum guaranteed part size; r : extra nodes spread to the first r parts;
        ListNode* node = root, *prev = nullptr;
        for (int i = 0; node && i < k; i++, r--) {
            parts[i] = node;
            for (int j = 0; j < n + (r > 0); j++) {
                prev = node;
                node = node->next;
            }
            prev->next = nullptr;
        }
        return parts;
    }
```
	
729	My Calendar I    		47.3%	Medium	
if exist double booking
store the event in set (sorted)

731	My Calendar II    		44.3%	Medium	
triple booking. 
approach 1: use double booking and check against the double booking
approach 2: when a new event +1, meet the end, -1

735	Asteroid Collision    		38.4%	Medium	
two directions. right positive, left negative, absolute value is the mass.
using stack 
```cpp
    vector<int> asteroidCollision(vector<int>& a) {
        vector<int> s; // use vector to simulate stack.
        for (int i = 0; i < a.size(); i++) {
            if (a[i] > 0 || s.empty() || s.back() < 0) // a[i] is positive star or a[i] is negative star and there is no positive on stack
                s.push_back(a[i]);
            else if (s.back() <= -a[i]) { // a[i] is negative star and stack top is positive star
                if(s.back() < -a[i]) i--; // only positive star on stack top get destroyed, stay on i to check more on stack.
                s.pop_back(); // destroy positive star on the frontier;
            } // else : positive on stack bigger, negative star destroyed.
        }
        return s;
    }
```

737	Sentence Similarity II    		43.3%	Medium	
738	Monotone Increasing Digits    		41.8%	Medium	
given N find the largest number with monotone increasing digits <=N
from right to left when we see current digit < previous, we decrease previous
the followed can be all 9
```cpp
    int monotoneIncreasingDigits(int N) {
        string n_str = to_string(N);
        
        int marker = n_str.size();
        for(int i = n_str.size()-1; i > 0; i --) {
            if(n_str[i] < n_str[i-1]) {
                marker = i;
                n_str[i-1]--;// = n_str[i-1]-1;
            }
        }
        
        for(int i = marker; i < n_str.size(); i ++) n_str[i] = '9';
        
        return stoi(n_str);
    }
```


739	Daily Temperatures    		60.0%	Medium	
how many days to wait for a warmer days. if not possible, put is as 0
stack or array to simulate the stack
```cpp
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> st;
        int n=temperatures.size(),j;
        vector<int> ans(n);
        for(int i=0;i<n;i++)
        {
            while(!st.empty() && temperatures[i]>temperatures[j=st.top()])
            {
                ans[j]=i-j;
                st.pop();
            }
            st.push(i);
        }
        return ans;
    }
```	

740	Delete and Earn    		45.9%	Medium	
delete and earn nums[i], delete nums[i]-1 and nums[i]+1
bucket sort to max range, and apply house robbing
```cpp
    int deleteAndEarn(vector<int>& nums) {
        int n = 10001;
        vector<int> values(n, 0);
        for (int num : nums)
            values[num] += num;

        int take = 0, skip = 0;
        for (int i = 0; i < n; i++) {
            int takei = skip + values[i];
            int skipi = max(skip, take);
            take = takei;
            skip = skipi;
        }
        return max(take, skip);
    }
```	

742	Closest Leaf in a Binary Tree    		38.8%	Medium	

743	Network Delay Time    		42.0%	Medium	
directed graph, given a list of edges with the travel time.
starting from a node, how long does it take for all nodes to receive the signal.
if impossible return -1;

using bellman or dijkstra by relaxing the distance using all edges repeating all nodes
```cpp
    int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        vector<int> dist(N + 1, INT_MAX);
        dist[K] = 0;
        for (int i = 0; i < N; i++) 
        {
            //for (vector<int> e : times) 
            for(int j=0;j<times.size();j++)
            {
                vector<int>&  e=times[j];
                int u = e[0], v = e[1], w = e[2];
                if (dist[u] != INT_MAX && dist[v] > dist[u] + w) 
                {
                    dist[v] = dist[u] + w;
                }
            }
        }

        int maxwait = 0;
        for (int i = 1; i <= N; i++)
            maxwait = max(maxwait, dist[i]);
        return maxwait == INT_MAX ? -1 : maxwait;
    }
```	




750	Number Of Corner Rectangles    		64.5%	Medium	
752	Open the Lock    		45.9%	Medium	
digits 0-9. initial at 0000, a list of deadends if you turn to one of them, locker stopps turning
minimum turns to the target.
bfs:
```cpp
    int openLock(vector<string>& deadends, string target) {
        unordered_set<string> dead(deadends.begin(),deadends.end());
        //scr='0000'to target
        //using dfs or bfs searching
        //each char has two option: +1 or -1
        if(dead.count(target) ||dead.count("0000")) return -1;
        if(target=="0000") return 0;
        return bfs(target,dead);
        //dfs(src,target,0,min_steps,dead);
        //return min_steps;
    }
    int bfs(string target,unordered_set<string>& dead)
    {
        queue<string> q;
        q.push("0000");
        unordered_set<string> visited;
        visited.insert("0000");
        int level=0;
        while(!q.empty())
        {
            int sz=q.size();
            for(int i=0;i<sz;i++)
            {
                string s=q.front();q.pop();
                //cout<<s<<" ";
                //add its 8 neighbors to the queue
                for(int j=0;j<4;j++)
                {
                    string t=s;
                    int c;
                    c=s[j]-'0';
                    t[j]=(c+1)%10+'0';
                    if(dead.count(t) || visited.count(t)) {}
                    else
                    {
                        //cout<<t<<" ";
                        if(t==target) return level+1;
                        q.push(t);visited.insert(t);
                    }
                    t[j]=(c-1+10)%10+'0';
                    if(dead.count(t) || visited.count(t)) continue;
                    //cout<<t<<" ";
                    if(t==target) return level+1;
                    q.push(t);visited.insert(t);//}
                }
            }
            //cout<<endl;
            level++;
        }
        return -1;
    }
```	

755	Pour Water    		40.4%	Medium	
756	Pyramid Transition Matrix    		51.5%	Medium	
dfs

763	Partition Labels    		70.3%	Medium	
using char's last index.
keep updating the max dist
when index==maxdist, then we get a group.

764	Largest Plus Sign    		43.3%	Medium	
straightforward or naiva approach
```cpp
    int orderOfLargestPlusSign(int N, vector<vector<int>>& mines) {
        vector<vector<int>> mat(N,vector<int>(N,1));
        for(int i=0;i<mines.size();i++) mat[mines[i][0]][mines[i][1]]=0;
        int max_ord=0;
        for(int i=0;i<N;i++)
        {
            for(int j=0;j<N;j++)
            {
                if(mat[i][j]) max_ord=max(max_ord,order(mat,i,j));
            }
        }
        return max_ord;
    }
    
    int order(vector<vector<int>>& M,int i,int j)
    {
        int k=1,N=M.size();
        while(1)
        {
            //if(i-k<0||i+k>N-1||j-k<0||j+k>N-1) return k;
            if(i-k<0 || i+k>N-1 || j-k<0 || j+k>N-1) return k;
            if(M[i-k][j] && M[i+k][j] && M[i][j-k] && M[i][j+k]) k++;
            else return k;
        }
    }
```
	
767	Reorganize String    		42.2%	Medium	
adjacent char are different.
using priority_queue

769	Max Chunks To Make Sorted    		51.6%	Medium	
compare with sorted

775	Global and Local Inversions    		38.8%	Medium	
local inversion is also global inversion.
that means it shall only include local inversion
```cpp
    bool isIdealPermutation(vector<int>& A) {
	for (int i = 0; i < A.size(); ++i) {
            if (abs(A[i] - i) > 1) return false;
        }
	return true;
    }
```
	
776	Split BST    		52.5%	Medium	
777	Swap Adjacent in LR String    		33.3%	Medium	
string contains LRX
a move: XL->LX or RX->XR
given start and end, check if it is possible to transform from start to end
L can keep moving left until meet R
R can keep moving right until meet L
can we just ignore the X? no the L and R position information get lost.
add position information.

```cpp
    bool canTransform(string start, string end) {
        //L can move left until meet R only
        //R can move right until meet L only
        //the end string shall keep exactly the same LR ordering,
        //with all L index<=its original index, and all its R index>=original R index
        vector<int> lpos,rpos;
        string s;
        for(int i=0;i<start.size();i++)
        {
            if(start[i]=='L') {lpos.push_back(i);s+="L";}
            if(start[i]=='R') {rpos.push_back(i);s+='R';}
        }
        vector<int> lpos1,rpos1;
        string s1;
        for(int i=0;i<end.size();i++)
        {
            if(end[i]=='L') {lpos1.push_back(i);s1+="L";}
            if(end[i]=='R') {rpos1.push_back(i);s1+='R';}
        }
        if(s!=s1) return 0;
        for(int i=0;i<lpos.size();i++) if(lpos[i]<lpos1[i]) return 0;
        for(int i=0;i<rpos.size();i++) if(rpos[i]>rpos1[i]) return 0;
        return 1;
    }
```	



779	K-th Symbol in Grammar    		37.5%	Medium	
0->01 1->10. this forms a complete binary tree
kth digit in Nth row.

the idea is: using k to determine the kth node located on left or right side on each layer.
```cpp
    int kthGrammar(int N, int K) {
        vector<int> bnum(N);
        for(int i=N;i>=1;i--) {bnum[i-1]=K;K=(K+1)/2;}
        int last_symbol=0;
        for(int i=0;i<N-1;i++)
        {
            //cout<<"parent="<<last_symbol<<" K="<<bnum[i+1]<<endl;
            if(bnum[i+1]%2==0) //right branch
            {
                if(last_symbol) last_symbol=0;else last_symbol=1;
            }
            else //left branch
            {
                if(last_symbol) last_symbol=1;else last_symbol=0;
            }
        }
        return last_symbol;
    }
```
	
781	Rabbits in Forest    		51.7%	Medium	
rabbit will see how many other rabbits in the same color.

```cpp
    int numRabbits(vector<int>& answers) {
        unordered_map<int,int> mp; //num-color vs number of rabits
        for(int i=0;i<answers.size();i++) mp[answers[i]+1]++;
        int num_rab=0;
        for(auto it=mp.begin();it!=mp.end();it++)
        {
                if(it->second%it->first) num_rab+=(it->second/it->first+1)*it->first;
                else num_rab+=it->second/it->first*it->first;
        }
        return num_rab;
    }
```	
785	Is Graph Bipartite?    		43.3%	Medium	
dfs using coloring
```cpp
    bool isBipartite(vector<vector<int>>& graph) {
        //use coloring 0: not color, 1, -1
        int n=graph.size(); //num of nodes
        vector<int> color(n); 
        for(int i=0;i<n;i++)
        {
            if(!color[i] && !dfs(graph,i,color,1)) return 0;
        }
        return 1;
    }
    bool dfs(vector<vector<int>>& graph,int i,vector<int>& color,int nc)
    {
        if(color[i]) return color[i]==nc;
        color[i]=nc;
        for(int j=0;j<graph[i].size();j++)
        {
            if(!dfs(graph,graph[i][j],color,-nc)) return 0; //adjacent reverse color
        }
        return 1;
    }
```	

787	Cheapest Flights Within K Stops    		34.8%	Medium	
bellman
```cpp
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K)
    {
        K++;
        vector<vector<int>> dp(n, vector<int>(K+1,INT_MAX));
        dp[src][0] = 0;
        for(int i = 1; i <= K; i++)
        {
            for(int j = 0; j < n; j++)   //To update ans[j][i](using i steps), we copy ans[j][i-1] first
                dp[j][i] = dp[j][i-1];
            for(const vector<int>& f: flights) //iterate all flights: f[0]: from, f[1]: to, f[2], price
            {
                if(dp[f[0]][i-1]<INT_MAX)
                dp[f[1]][i] = min(dp[f[1]][i], dp[f[0]][i-1] + f[2]);
            }
        }
        if(dp[dst][K] == INT_MAX) return -1;
        return dp[dst][K];
    }
```
	
789	Escape The Ghosts    		55.2%	Medium	
just to see who arrives the destination first
```cpp
    bool escapeGhosts(vector<vector<int>>& ghosts, vector<int>& target) {
        //to see who is closer to the target
        int min_dist=INT_MAX;
        int dx,dy;
        int dist=abs(target[0])+abs(target[1]);
        for(int i=0;i<ghosts.size();i++)
        {
            dx=abs(ghosts[i][0]-target[0]);
            dy=abs(ghosts[i][1]-target[1]);
            if(dx+dy<dist) return 0;
            min_dist=min(min_dist,dx+dy);
            //cout<<min_dist<<":"<<dx+dy<<endl;
        }
        
        return dist<min_dist;
    }
```
	
790	Domino and Tromino Tiling    		35.7%	Medium	
having L and I shape
build 2N tiles
return the number of ways to build it.
dp: 
```cpp
    int numTilings(int N) {
        int md=1e9;
        md+=7;
        vector<long long> v(1001,0);
        v[1]=1;
        v[2]=2;
        v[3]=5;
        if(N<=3)
            return v[N];
        for(int i=4;i<=N;++i){
            v[i]=2*v[i-1]+v[i-3]; 
            v[i]%=md;
        }
        return v[N];
	}
```


791	Custom Sort String    		61.9%	Medium	
s is sorted using some customized method, no duplicate
sort string T
count sorting T.
```cpp
    string customSortString(string S, string T) {
        unordered_map<char,int> mp1;//char, count
        for(int i=0;i<T.size();i++) mp1[T[i]]++;
        string s;
        for(int i=0;i<S.size();i++)        
        {
            char c=S[i];
            if(mp1.count(c)) 
            {
                s.append(mp1[c],c);
                mp1.erase(c);
            }
        }
        //remaining characters, what ever sequence
        for(auto it=mp1.begin();it!=mp1.end();it++)
            s.append(it->second,it->first);
        return s;
    }
```	

792	Number of Matching Subsequences    		42.9%	Medium	
a list of word, find the number of words which is a subsequence of S
word array is large, so try to avoid naive approach.
to improve a bit, save all the index for each char (26), bianry search, then update the index
```cpp
    int numMatchingSubseq(string S, vector<string>& words) {
        unordered_map<char,vector<int>> mp,tmp;
        for(int i=0;i<S.size();i++) mp[S[i]].push_back(i);
        int num=0;
        for(int i=0;i<words.size();i++)
        {
            bool match=1;
            int prev_ind=-1;
            for(int j=0;j<words[i].size();j++)
            {
                char c=words[i][j];
                if(mp.count(c))
                {
                    auto it=upper_bound(mp[c].begin(),mp[c].end(),prev_ind);
                    if(it==mp[c].end()) {match=0;break;}
                    else prev_ind=*it;//int(it-mp[c].begin());
                    //cout<<words[i]<<":"<<prev_ind<<endl;
                }
                else {match=0;break;}
            }
            if(match) num++;
        }
        return num;
    }
```	

794	Valid Tic-Tac-Toe State    		29.5%	Medium	
array 3x3, start 'x' first.
```cpp
bool validTicTacToe(vector<string>& board) {
    bool wony = false, wonx = false;
    int rows = board.size();
    int cols = rows ? board[0].size() : 0;
    int diax = 0, diay = 0, xdiax = 0, xdiay = 0, xx = 0, yy = 0;
    for(int i = 0; i < rows; i++) 
    {
        int rowsx = 0, rowsy = 0;
        int colsx = 0, colsy = 0;
        for(int j = 0; j < cols; j++) 
        {
            // see current row
            if(board[i][j] == 'X') {rowsx++,xx++;}//xx:total x
            if(board[i][j] == 'O') {rowsy++, yy++;}//yy:total O
            // see ith column
            if(board[j][i] == 'X') colsx++;
            if(board[j][i] == 'O') colsy++;
        }
        
        // see both the diagonals
        if(board[i][i] == 'X') diax++;
        if(board[i][i] == 'O') diay++;
        if(board[i][cols - 1 - i] == 'X') xdiax++;
        if(board[i][cols - 1 - i] == 'O') xdiay++;
        
        if(rowsx == 3 || colsx == 3 || diax == 3 || xdiax == 3)
            wonx = true;
        
         if(rowsy == 3 || colsy == 3 || diay == 3 || xdiay == 3)
            wony = true;
    }
    
    if(xx < yy || !(xx == yy || xx == yy + 1))
        return false;
    
    if((wonx && wony) || (wonx && xx == yy) || (wony && xx != yy))
        return false;
    
    return true;
}
```
795	Number of Subarrays with Bounded Maximum    		43.2%	Medium	
max in the subarray is in the range [L,R]
A[i]<L
A[i]>R
L<=A[i]<=R
```cpp
    int numSubarrayBoundedMax(vector<int>& A, int L, int R) {
        int preId=0;
        int lowId=0;
        int count=0;
        int maxP=-1;
        for(int i=0; i<A.size(); i++){
            if(A[i]>=L && A[i]<=R){
                count+=i-preId+1;
                lowId=i;
            }
            else if(A[i]<L && maxP>=L){
                count+=lowId-preId+1;
            }
            else if(A[i]>R){
                maxP=-1;
                preId=i+1;
                lowId=i;
            }
            if(maxP<A[i] && A[i]<=R) maxP=A[i];
        }
        return count;
    }
```
	
797	All Paths From Source to Target    		70.5%	Medium	
bfs or dfs with backtracking
```cpp
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
       //dfs to search all the path
        vector<vector<int>> ans;
        dfs(graph,0,vector<int>(1,0),ans);
        return ans;
    }
    void dfs(vector<vector<int>>& graph,int node,vector<int> v,vector<vector<int>>& ans)
    {
        if(node==graph.size()-1) {ans.push_back(v);return;}
        for(int i=0;i<graph[node].size();i++)
        {
            v.push_back(graph[node][i]);
            dfs(graph,graph[node][i],v,ans);
            v.pop_back();
        }
    }
```

799	Champagne Tower    		34.0%	Medium	
dp: given the surplus to j and j+1 on row i+1.
```cpp
    double champagneTower(int poured, int query_row, int query_glass) {
        //dp: given the surplus to j and j+1 on next layer
        vector<vector<double>> dp(query_row+2,vector<double>(query_row+2));
        dp[0][0]=poured;
        for(int i=0;i<=query_row;i++)
        {
            for(int j=0;j<=i;j++)
            {
                if(dp[i][j]>1)
                {
                    double surplus=dp[i][j]-1;
                    dp[i+1][j]+=surplus/2;
                    dp[i+1][j+1]+=surplus/2;
                    dp[i][j]=1.0;
                }
            }
        }
        return dp[query_row][query_glass];
    }
```
	
801	Minimum Swaps To Make Sequences Increasing    		34.6%	Medium	
swap A and B at the same position
greedy or dp
```cpp
    int minSwap(vector<int>& A, vector<int>& B) {
        const size_t n = A.size();
        vector<int> swap(n, n), no_swap(n, n);
        swap[0] = 1;
        no_swap[0] = 0;
        for (size_t i = 1; i < n; ++i) {
            if (A[i] > A[i - 1] && B[i] > B[i - 1]) {
                // If we swap position i, we need to swap position i - 1.
                swap[i] = swap[i - 1] + 1;
                
                // If we don't swap position i , we should not swap position i - 1.
                no_swap[i] = no_swap[i - 1];
            }
            
            if (A[i] > B[i - 1] && B[i] > A[i - 1]) {
                // If we swap position i, we should not swap position i - 1.
                swap[i] = std::min(swap[i], no_swap[i - 1] + 1);
                
                // If we don't swap position i, we should swap position i - 1.
                no_swap[i] = std::min(no_swap[i], swap[i - 1]);
            }
        }
        
        return std::min(swap.back(), no_swap.back());
    }
```
	
802	Find Eventual Safe States    		43.6%	Medium	
directed graph, if goes to a cycle, all nodes on the cycle shall be removed.

dfs
```cpp
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        //dfs to see if it can walk to a terminal (all paths)
        vector<int> ans;
        vector<int> safenodes(graph.size(),-1);
        for(int i=0;i<graph.size();i++)
        {
            unordered_set<int> visited;
            if(isSafeNode(graph,i,visited,safenodes)) ans.push_back(i);
        }
        return ans;
    }
    
    bool isSafeNode(vector<vector<int>>& graph,int start,unordered_set<int> visited,vector<int>& safenodes)
    {
        //all paths reaches to terminal. If one case fail, then fail
        //failed case: we had a cycle
        //we need add memoization 
        if(safenodes[start]>-1) return safenodes[start];
        if(graph[start].size()==0) {return 1;} //cannot mark it as safe if only one path is safe
        if(visited.count(start)) {return 0;} //
        for(int i=0;i<graph[start].size();i++)
        {
            visited.insert(start);
            if(!isSafeNode(graph,graph[start][i],visited,safenodes)) 
            {
                //all visited nodes shall mark as not safe
                safenodes[start]=0;
                return 0;
            }
            visited.erase(start);
        }
        safenodes[start]=1;
        return 1;
    }
```	
807	Max Increase to Keep City Skyline    		81.4%	Medium	
looking from up or bottom no change
looking from left or right no change
straighforward: just compare its row max and col max and increase the diff.

```cpp
    int maxIncreaseKeepingSkyline(vector<vector<int>>& grid) {
        int m=grid.size(),n=grid[0].size();
        vector<int> max_lr(m),max_ub(n);
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++) max_lr[i]=max(max_lr[i],grid[i][j]);
        }
        for(int j=0;j<n;j++)
        {
            for(int i=0;i<m;i++) max_ub[j]=max(max_ub[j],grid[i][j]);
        }
        int total=0;
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++) total+=min(max_lr[i],max_ub[j])-grid[i][j];
        }
        return total;
    }
```	
808	Soup Servings    		37.1%	Medium	
recursive or dfs
```cpp
vector<vector<double>> prob(200,vector<double>(200,-1.0));//goes to N->5000/25
class Solution {
public:
    //using dfs
    double soupServings(int N) {
        int m=ceil(N/25.0);//each serving is 25
        if(m>=200) return 1.0;
        return helper(m,m);
    }
    double helper(int ma,int mb)
    {
        if(ma<=0 && mb>0) return 1.0;
        if(ma<=0 && mb<=0) return 0.5;
        if(mb<=0) return 0.0;
        if(prob[ma][mb]>0) return prob[ma][mb]; //has to put behind the above since ma mb cannot be used as index
        return prob[ma][mb]=0.25*(helper(ma-4,mb)+helper(ma-3,mb-1)+helper(ma-2,mb-2)+helper(ma-1,mb-3));
    }
```
	
809	Expressive Words    		43.5%	Medium	
two pointer compare

813	Largest Sum of Averages    		45.1%	Medium	
dp:
```cpp
    double largestSumOfAverages(vector<int>& A, int K) {
        if(A.empty() || K == 0)  return 0;
        vector<vector<double>> dp(K+1,vector<double>(A.size(),0));
        for(int i=1;i<A.size();i++) A[i]+=A[i-1];
        for(int i=0;i<A.size();i++) dp[1][i]=double(A[i])/(i+1);
        for(int k = 2; k <= K; k++)
        {
            for(int i = k-1; i < A.size(); i++) //0 to i needs k groups
            { 
                for(int j = k-2 ; j < i; j++) //0 to j needs k-1 groups
                {
                    dp[k][i] = max(dp[k-1][j]+double(A[i]-A[j])/(i - j),dp[k][i]);
                }
            }
        }
        return dp[K][A.size()-1];
    }
```	

814	Binary Tree Pruning    		70.9%	Medium	
all subtree not containing 1 is removed. 
similar to the problem: sufficientSubset
```cpp
    TreeNode* pruneTree(TreeNode* root) {
        return prune(root);
     
    }
    
    TreeNode* prune(TreeNode*& root)
    {
        if(!root) return 0;
        if(root->left)
        {
            if(!root->left->left && !root->left->right && !root->left->val) root->left=0;
            else root->left=prune(root->left);
        }
        
        if(root->right)
        {
            if(!root->right->left && !root->right->right && !root->right->val) root->right=0;
            else root->right=prune(root->right);
        }
        if(!root->left && !root->right && !root->val) //need remove itself
            root=0;
        return root;        
    }
```

816	Ambiguous Coordinates    		44.1%	Medium	
return all the possible coordinate using the string (x,y)
```cpp
    vector<string> ambiguousCoordinates(string S) {
        //add a , in the string, for the separated half, we can add a . to each of them
        //first we separate it using ,
        //only rule: it cannot be all zero, except it has one zero
        vector<string> vs;
        string s=S.substr(1,S.length()-2); //remove ()
        //cout<<s<<endl;
        for(int i=1;i<=s.length()-1;i++)
        {
            string a=s.substr(0,i);
            string b=s.substr(i);
            //cout<<"check "<<a<<" "<<b<<endl;
            helper(a,b,vs);
        }
        return vs;
    }
    
    void helper(string a,string b,vector<string>& vs)
    {
        if(!is_legal(a) || !is_legal(b) ) return;
        //both are legal, then we can add decimal to each of the string
        //zero or one decimal point to the string
        vector<string> va=add_decimal(a);
        vector<string> vb=add_decimal(b);
        //cout<<"add decimal:"<<b<<":"<<vb.size()<<endl;
        for(int i=0;i<va.size();i++)
        {
            for(int j=0;j<vb.size();j++) vs.push_back("("+va[i]+", "+vb[j]+")");
        }
    }
    bool is_legal(string& a)
    {
        if(stoi(a)==0) return a.size()==1;
        //if(a.size()>1 && a[0]=='0') return 0;
        return 1;
    }
    
    bool is_validInt(string& a) //the decimal 
    {
        if(a.size()>1 && a[0]=='0') return 0;
        return 1;
    }
    bool is_validDecimal(string& s) //after the ., cannot be zero at the end
    {
        return s.back()!='0';
    }
    vector<string> add_decimal(string& s)
    {
        vector<string> ans;
        if(s.size()==1 || s[0]!='0') ans.push_back(s); //if no decimal is legal?
        //note the last digit after decimal cannot be 0
        for(int i=1;i<=s.size()-1;i++)
        {
            string a=s.substr(0,i),b=s.substr(i);
            if(b.back()=='0') continue;
            if(is_validInt(a) && is_validDecimal(b)) 
                ans.push_back(a+"."+b);
        }
        return ans;
    }
```
	
817	Linked List Components    		54.6%	Medium	
```cpp
    int numComponents(ListNode* head, vector<int>& G) {
        unordered_set<int> myset(G.begin(),G.end());
        int grp=0,cnt=0;
        while(head)
        {
            if(myset.count(head->val)) cnt++;
            else {if(cnt) {grp++;cnt=0;}}
            head=head->next;
        }
        if(cnt) grp++;
        return grp;
    }
```
	
820	Short Encoding of Words    		47.1%	Medium	
if a word is a suffix of another word, then the word is hidden
sort the words from longest to shortest
build suffix tree by adding all its suffix
```cpp
bool cmp(const string& a,const string& b) {return a.size()>b.size();}
class Solution {
public:
    int minimumLengthEncoding(vector<string>& words) {
        //only can be encoded if one words in a suffix of another word
        //sort the string using its length
        
        unordered_set<string> ms;
        int total=0,total_len=0;
        //sort the string by length
        sort(words.begin(),words.end(),cmp);
        for(int i=0;i<words.size();i++)
        {
            if(ms.count(words[i])) //found in the suffix, this word is hidden
            {
                total++;continue;
            }
            total_len+=words[i].size();
            ms.insert(words[i]);
            for(int j=1;j<words[i].size()-1;j++) ms.insert(words[i].substr(j));
        }
        return total_len+=(words.size()-total);
    }
};
```

822	Card Flipping Game    		40.3%	Medium	
card has front number and back number.
If the number X on the back of the chosen card is not on the front of any card, then this number X is good.
what is the smallest number that is good.
```cpp
    int flipgame(vector<int>& fronts, vector<int>& backs) {
       unordered_set<int> ms;
        for(int i=0;i<fronts.size();i++)
            if(fronts[i]==backs[i]) ms.insert(fronts[i]);
        int min_num=INT_MAX;
        for(int i=0;i<fronts.size();i++) if(!ms.count(fronts[i])) min_num=min(min_num,fronts[i]);
        for(int i=0;i<fronts.size();i++) if(!ms.count(backs[i])) min_num=min(min_num,backs[i]);
        return min_num==INT_MAX?0:min_num;
    }
```	

823	Binary Trees With Factors    		32.4%	Medium	
given an array, elements>1, build a tree: nodes value=its product of child
return the number of trees can build.
number can be used multiple times
dp:
```cpp
    int numFactoredBinaryTrees(vector<int>& A) {
        //a single node automatically satisify
        //this is a coin-change similar problem
        //for any number we shall try all possible combinations
        vector<long long> dp(A.size(),1);//long long is key!
        sort(A.begin(),A.end());
        const int tt=1000000000+7;
        //A[i]=A[j]*A[i/j]
        unordered_map<int,int> ms;//value with index
        for(int i=0;i<A.size();i++)
        {
            ms[A[i]]=i;
            for(int j=i-1;j>=0;j--) 
            {
                if(A[i]%A[j]==0)//find one factor
                {
                    int t=A[i]/A[j];
                    if(ms.count(t)) dp[i]+=(dp[j]*dp[ms[t]])%tt;
                }
            }
        }
        //copy(dp.begin(),dp.end(),ostream_iterator<int>(cout," "));
        long long ans=0;
        ans=accumulate(dp.begin(),dp.end(),0LL);
        //for(int i=0;i<dp.size();i++) ans=(ans+dp[i])%tt;
        return ans%tt;//%tt+7;
    }
```
	
825	Friends Of Appropriate Ages    		36.2%	Medium	
cannot make friend request if any condition is true:
age[B] <= 0.5 * age[A] + 7
age[B] > age[A]
age[B] > 100 && age[A] < 100

```cpp
    int numFriendRequests(vector<int>& ages) {
        int va[121]={0};
        for(int i=0;i<ages.size();i++) va[ages[i]]++;
        //larger age friend request smaller age
        //age difference
        //can use binary search
        int total=0;
        for(int i=1;i<=120;i++)
        {
            if(i>14 && va[i]>1) total+=va[i]*(va[i]-1); //C1 condition
            if(i>1 && va[i]) //2nd condition, only friend request with smaller age
            {
                int smallest_age=i/2+7+1;//>age C1 condition
                //cout<<smallest_age<<endl;
                for(int j=smallest_age;j<i;j++) total+=va[j]*va[i];
            }
        }
        return total;
    }
```	
826	Most Profit Assigning Work    		35.6%	Medium	
difficulty, profit
worker[i] is the ability to work on job with difficulty
each worker can be assigned at most one work
return the largest profit
approach:
build a sorted map: difficulty vs max profit
search for the person who can do the job with the max profit
zip difficulty and profit as jobs.
sort jobs and sort 'worker'.
2 pointers idea, for each worker, find his maximum profit he can make under his ability.
Because we have sorted jobs and worker, we will go through two lists only once.
It will be only O(M+N).
Time Complexity
O(NlogN + MlogM), as we sort list.

```cpp
    int maxProfitAssignment(vector<int>& difficulty, vector<int>& profit, vector<int>& worker) {
        map<int,int> vp;
        for(int i=0;i<profit.size();i++) 
			vp[difficulty[i]]=max(vp[difficulty[i]],profit[i]);
        vp[INT_MAX]=0;//add one as the end so worker>max can work
        sort(worker.begin(),worker.end());
        int i=0,max_profit=0,total=0; //max shall be 0
        for(auto it=vp.begin();it!=vp.end();it++)
        {
            while(it->first>worker[i])
            {
                total+=max_profit;
                //cout<<worker[i]<<":"<<max_profit<<endl;
                i++;
                if(i>=worker.size()) break;
            }
            if(i>=worker.size()) break;
            max_profit=max(max_profit,it->second);
        }
        return total;
    }
```	

831	Masking Personal Information    		42.1%	Medium	
email: masked as firstletter+5*+lastletter@....
phone number: include digits +/- ()
10 digits for local number formated as ***-***-4digits
country code: +digits marked as +***
straightforward:
```cpp
    string maskPII(string S) {
        //email contains a @, otherwise it is phone number
        transform(S.begin(),S.end(),S.begin(),::tolower);
        size_t pos=S.find('@');
        string ss;
        if(pos==string::npos) //it is phone number
        {
            for(int i=0;i<S.size();i++) 
            {
                //cout<<S[i]<<endl;
                if(isdigit(S[i])) {ss+=S[i];}
            }
            //cout<<ss;
            string t=ss;
            if(t.size()<=10) ss="***-***-"+t.substr(t.size()-4);
            else {ss="+";ss.append(t.size()-10,'*');ss+="-***-***-"+t.substr(t.size()-4);}
            
        }
        else
        {
            ss+=S[0];
            ss+="*****";
            ss+=S[pos-1]+S.substr(pos);
            //cout<<s<<endl;
        }
        return ss;
    }
```	

833	Find And Replace in String    		46.1%	Medium	
given a list of replace operations

do it from right to left so that no index change for string in the front
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
	
835	Image Overlap    		52.3%	Medium	
two images are square matrix. return the largest overlap;
direct approach
```cpp
    int largestOverlap(vector<vector<int>>& A, vector<vector<int>>& B) {
        int n=A.size();
        int max_overlap=0;
        for(int i=-n+1;i<n;i++) //sliding is 2n
        {
            for(int j=-n+1;j<n;j++)
            {
                max_overlap=max(max_overlap,overlap(A,B,i,j));
            }
        }
        return max_overlap;
    }
    int overlap(vector<vector<int>>& A, vector<vector<int>>& B,int x,int y)
    {
        //the other top left coordinate is (x,y)
        //need find the overlap of the two image, 
        int xl,xr,yt,yb;
        int n=A.size();
        xl=max(x,0);
        xr=min(x+n-1,n-1);
        yt=max(y,0);
        yb=min(y+n-1,n-1);
        //cout<<x<<","<<y<<": "<<xl<<","<<xr<<","<<yt<<","<<yb<<endl;
        int res=0;
        for(int i=xl;i<=xr;i++)
        {
            for(int j=yt;j<=yb;j++) res+=A[i][j]*B[i-x][j-y];
        }
        return res;
    }
```
	
837	New 21 Game    		31.3%	Medium	
sliding window and climing stairs
```cpp
     double new21Game(int N, int K, int W) {
        //this is like climbing stairs. ith position can be obtained
        //dp[i]=sum(dp[j])/W j=i-1 to i-W
        if(N>=K+W || K==0) return 1.0;
        vector<double> dp(K+1); //dp[i] is the probability to reach i points
        dp[0]=1.0;
        //dp[i]=sum(dp[j])/W for j=i-1 to i-w
        double wsum=0;

        for(int i=1;i<=K;i++)
        {
            if(i<=W) dp[i]=wsum+=dp[i-1]/W;
            else {dp[i]=wsum+=(dp[i-1]-dp[i-W-1])/W;}
        }
        //copy(dp.begin(),dp.end(),ostream_iterator<double>(cout," "));
        double ans=0;
        for(int i=K-1;i>=max(K-W,0);i--)
        {
            int d=K-1-i;
            int len=min(W-d,N-K+1);
            ans+=len*dp[i]/W;
        }
        return ans;
    }
```
	
838	Push Dominoes    		43.7%	Medium	
Intuition:
Whether be pushed or not, depend on the shortest distance to 'L' and 'R'.
Also the direction matters.
Base on this idea, you can do the same thing inspired by this problem.
https://leetcode.com/problems/shortest-distance-to-a-character/discuss/125788/

Here is another idea that focus on 'L' and 'R'.
'R......R' => 'RRRRRRRR'
'R......L' => 'RRRRLLLL' or 'RRRR.LLLL'
'L......R' => 'L......R'
'L......L' => 'LLLLLLLL'

Time Complexity:
O(N)

C++:

    string pushDominoes(string d) {
        d = 'L' + d + 'R';
        string res = "";
        for (int i = 0, j = 1; j < d.length(); ++j) {
            if (d[j] == '.') continue;
            int middle = j - i - 1;
            if (i > 0) res += d[i];
            if (d[i] == d[j]) res += string(middle, d[i]);
            else if (d[i] == 'L' && d[j] == 'R') res += string(middle, '.');
            else res += string(middle / 2, 'R') + string(middle % 2,'.') + string(middle / 2, 'L');
            i = j;
        }
        return res;
    }
	
	
841	Keys and Rooms    		60.2%	Medium	
given a list of room with keys
bfs
```cpp
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        unordered_set<int> visited;
        queue<int> to_visit;
        to_visit.push(0);
        while(!to_visit.empty()) {
            int curr = to_visit.front();
            to_visit.pop();
            visited.insert(curr);
            for (int k : rooms[curr]) if (visited.find(k) == visited.end()) to_visit.push(k);
        }
        return visited.size() == rooms.size();
    }
```
	

845	Longest Mountain in Array    		34.2%	Medium	
two pass to get the up length
```cpp
    int longestMountain(vector<int>& A) {
        vector<int> up1(A.size()),up2(A.size());
        for(int i=1;i<A.size();i++) if(A[i]>A[i-1]) up1[i]=up1[i-1]+1;
        for(int i=A.size()-2;i>=0;i--) if(A[i]>A[i+1]) up2[i]=up2[i+1]+1;
        int max0=INT_MIN;
        for(int i=0;i<A.size();i++) if(up1[i] && up2[i]) max0=max(max0,up1[i]+up2[i]);
        return max0==0||max0==INT_MIN?0:max0+1;
    }
```	
846	Hand of Straights    		49.1%	Medium	
divide the cards into W sized and they are consecatuve
saved into a sorted map
```cpp
    bool isNStraightHand(vector<int> hand, int W) {
        map<int, int> c;
        for (int i : hand) c[i]++;
        for (auto it : c)
            if (c[it.first] > 0)
                for (int i = W - 1; i >= 0; --i)
                    if ((c[it.first + i] -= c[it.first]) < 0)
                        return false;
        return true;
    }
```	

848	Shifting Letters    		40.6%	Medium	
shift the first i+1 letters x times
suffix sum to get each char's shift times
```cpp
    string shiftingLetters(string S, vector<int>& shifts) {
        int n=shifts.size();
        for(int i=n-2;i>=0;i--) shifts[i]+=shifts[i+1]%26;
        for(int i=0;i<S.size();i++) 
        {
            int t=(S[i]-'a'+shifts[i])%26+'a';
            S[i]=t;
        }
        return S;
    }
```
	
851	Loud and Rich    		47.6%	Medium	
dfs
```cpp
    unordered_map<int, vector<int>> richer2;
    vector<int> res;
    vector<int> loudAndRich(vector<vector<int>>& richer, vector<int>& quiet) {
        for (auto v : richer) richer2[v[1]].push_back(v[0]);
        res = vector<int> (quiet.size(), -1);
        for (int i = 0; i < quiet.size(); i++) dfs(i, quiet);
        return res;
    }

    int dfs(int i, vector<int>& quiet) {
        if (res[i] >= 0) return res[i];
        res[i] = i;
        for (int j : richer2[i]) 
            if (quiet[res[i]] > quiet[dfs(j, quiet)]) res[i] = res[j];
        return res[i];
    }
```
	
853	Car Fleet    		39.6%	Medium	
if a car catch up, it becomes the same speed as previous
```cpp
    int carFleet(int target, vector<int>& position, vector<int>& speed) {
        if(speed.size()==0) return 0;
        vector<double> time(speed.size());
        //need sort these cars according to its position
        vector<vector<int>> cars(speed.size(),vector<int>(2));
        for(int i=0;i<speed.size();i++) {cars[i][0]=position[i];cars[i][1]=speed[i];}
        sort(cars.begin(),cars.end());
        for(int i=0;i<speed.size();i++) time[i]=double(target-cars[i][0])/cars[i][1];
        //copy(time.begin(),time.end(),ostream_iterator<double>(cout," "));
        //merge with previous if its time is less than previous
        for(int i=speed.size()-2;i>=0;i--)
        {
            if(time[i]<time[i+1]) time[i]=time[i+1];
        }
        int num_grp=1;
        //vector<int> fleet(speed.size());
        for(int i=speed.size()-2;i>=0;i--) 
        {
            if(time[i]>time[i+1]) num_grp++;
        }
        return num_grp;
    }
```
	
855	Exam Room    		38.2%	Medium	
this is an interval problem
Very straight forward idea.
Use a list L to record the index of seats where people sit.

seat():
1. find the biggest distance at the start, at the end and in the middle.
2. insert index of seat
3. return index

leave(p): pop out p

Time Complexity:
O(N) for seat() and leave()

C++:

    int N;
    vector<int> L;
    ExamRoom(int n) {
        N = n;
    }

    int seat() {
        if (L.size() == 0) {
            L.push_back(0);
            return 0;
        }
        int d = max(L[0], N - 1 - L[L.size() - 1]);
        for (int i = 0; i < L.size() - 1; ++i) d = max(d, (L[i + 1] - L[i]) / 2);
        if (L[0] == d) {
            L.insert(L.begin(), 0);
            return 0;
        }
        for (int i = 0; i < L.size() - 1; ++i)
            if ((L[i + 1] - L[i]) / 2 == d) {
                L.insert(L.begin() + i + 1, (L[i + 1] + L[i]) / 2);
                return L[i + 1];
            }
        L.push_back(N - 1);
        return N - 1;
    }

    void leave(int p) {
        for (int i = 0; i < L.size(); ++i) if (L[i] == p) L.erase(L.begin() + i);
    }
	
856	Score of Parentheses    		56.1%	Medium	
Given a balanced parentheses string S, compute the score of the string based on the following rule:

() has score 1
AB has score A + B, where A and B are balanced parentheses strings.
(A) has score 2 * A, where A is a balanced parentheses string.
 ```cpp
     int scoreOfParentheses(string S) {
        stack<int> st;
        st.push(0);
        for(int i=0;i<S.size();i++)
        {
            if(S[i]=='(') st.push(0);
            else
            {
                int score=st.top();
                st.pop();
                if(score==0) score=1;
                else score*=2;
                st.top()+=score;
            }
        }
        return st.top();
    }
```
	
858	Mirror Reflection    		51.9%	Medium	
expand to 1d case
```cpp
    int mirrorReflection(int p, int q) {
        int t=p*q/gcd(p,q);
        //cout<<t;
        int m=t/p,n=t/q;
        //n: the number of extension along x, m: the number of extension along y
        //m,n four combinations: 00,01,10,11
        if(n%2 && m%2==0)  return 0;
        if(n%2 && m%2) return 1;
        if(n%2==0 && m%2) return 2;
    }
    int gcd(int a, int b)
    {
        if(b==0) return a;
        else return gcd(b,a%b);
    }
```
	
861	Score After Flipping Matrix    		69.5%	Medium	
flip row or columns and each row is a binary number, return the max sum
greedy, the MSB > sum of all LSB
```cpp
    int matrixScore(vector<vector<int>>& A) {
       //toggle the 0th col to 1 and then make 1 to be the majority
        int m=A.size(),n=A[0].size();
        for(int i=0;i<m;i++)
        {
            if(A[i][0]==0) //toggle this row
            {
                for(int j=0;j<n;j++) A[i][j]^=1;
            }
        }
        for(int j=1;j<n;j++)
        {
            int cnt1=0;
            for(int i=0;i<m;i++) cnt1+=A[i][j];
            if(cnt1<(m+1)/2)
            {
                for(int i=0;i<m;i++) A[i][j]^=1;
            }
        }
        int score=0;
        for(int i=0;i<m;i++)
        {
            int t=0;
            for(int j=0;j<n;j++) t+=(A[i][j]<<(n-1-j));
            score+=t;
        }
        return score;
    }
```
	
863	All Nodes Distance K in Binary Tree    		47.2%	Medium	
approach 1: convert the tree into graph and then use bfs or dfs

865	Smallest Subtree with all the Deepest Nodes    		55.6%	Medium	
check depth of left and right
if depth is the same, return root
if left>right, subproblem for left else for right
O(N^2) 
```cpp
int depth(TreeNode *root) {
    return !root ? 0 : max(depth(root->left), depth(root->right)) + 1;
}

TreeNode* subtreeWithAllDeepest(TreeNode* root) {
    int d = depth(root->left) - depth(root->right);
    return !d ? root : subtreeWithAllDeepest(d > 0 ? root->left : root->right);
}
```
we always want O(N) for tree problems
similar to the lowest common ancestor.
bfs: to find the last layer's left most node and right most node and find their lowest common ancestor
```cpp
    TreeNode* lca( TreeNode* root, TreeNode* p, TreeNode* q ) {
        if ( !root || root == p || root == q ) return root;
        TreeNode *left = lca( root->left, p, q );
        TreeNode *right = lca (root->right, p, q );

        return !left? right: !right? left: root;
    }
    
    TreeNode* subtreeWithAllDeepest(TreeNode* root) {
        if ( !root || !root->left && !root->right ) return root;
        TreeNode *leftMost = NULL;
        TreeNode *rightMost = NULL;
        
        queue<TreeNode*> q;
        q.push(root);
        while( !q.empty() ) {
            int levelSize = q.size();
            for(int level = 0; level < levelSize; level++ ) {
                TreeNode* node = q.front(); q.pop();
                if ( level == 0 ) leftMost = node;
                if ( level == levelSize - 1 ) rightMost = node;
                
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
                
            }
        }
        return lca( root, leftMost, rightMost );
    }
```
	
866	Prime Palindrome    		20.1%	Medium	
Find the smallest prime palindrome greater than or equal to N.
N<1e8
brutal force
All palindrome with even digits is multiple of 11. so only check odd digits
```cpp
    int primePalindrome(int N) {
        if (8 <= N && N <= 11) return 11;
        for (int x = 1; x < 100000; ++x) {
            string s = to_string(x), r(s.rbegin(), s.rend());
            int y = stoi(s + r.substr(1));
            if (y >= N && isPrime(y)) return y;
        }
        return -1;
    }
    bool isPrime(int num) {
        if (num < 2 || num % 2 == 0) return num == 2;
        for (int i = 3; i * i <= num; i += 2)
            if (num % i == 0) return false;
        return true;
    }
```
	
869	Reordered Power of 2    		50.9%	Medium	
brutal force: check 2^0 to 2^31 the digits is same as original

870	Advantage Shuffle    		42.4%	Medium	
greedy: if we cannot beat, choose the smallest one vs its largest
```cpp
	vector<int> advantageCount(vector<int>& A, vector<int>& B) {
	  multiset<int> s(begin(A), end(A));
	  for (auto i = 0; i < B.size(); ++i) {
		auto p = *s.rbegin() <= B[i] ? s.begin() : s.upper_bound(B[i]);
		A[i] = *p;
		s.erase(p);
	  }
	  return A;
	}
```

873	Length of Longest Fibonacci Subsequence    		46.1%	Medium	
record the previous two elements.
```cpp
    int lenLongestFibSubseq(vector<int>& A) {
        unordered_map<int, int> m;
        int n = A.size(), res = 0;
        int dp[n][n];//1d: up to ith element, 2d: n-1 element 
        for (int i = 0; i < n; ++i) 
        {
            m[A[i]] = i;
            for (int j = 0; j < i; ++j) 
            {
                int k=-1;
                int d=A[i]-A[j];
                if(m.count(d)) k=m[d];//search for n-2 element
                dp[i][j] = (k>=0 && A[k]<A[j]) ? dp[j][k] + 1 : 2;
                res = max(res, dp[i][j]);
            }
        }
        return res > 2 ? res : 0;
    }
```	

875	Koko Eating Bananas    		46.1%	Medium	
N piles of banana, each hour koko eat K bananas. if the pile has less than K bananas, it eats it and does not eat in the hour.
H hours, find the min K such that it can finish all in H hours
binary search.
```cpp
    int minEatingSpeed(vector<int>& piles, int H) {
        //binary search to satisfy the sum(A/K)=H
        //min= average
        //max= max
        int total=0;
        total=accumulate(piles.begin(),piles.end(),0);
        int min=max(total/H,1);
        int max=*max_element(piles.begin(),piles.end());
        int mid=(min+max)/2;
        while(min<max)
        {
            mid=(min+max)/2;
            int time=eat_time(piles,mid);
            if(time>H) min=mid+1;
            else max=mid;//need goes to smaller 
        }
        return (min+max)/2;
    }
    int eat_time(vector<int>& piles,int k)
    {
        int time=0;
        for(int i=0;i<piles.size();i++) time+=(piles[i]+k-1)/k;
        return time;
    }
```	

877	Stone Game    		61.3%	Medium	
even number of piles, total stones are odd number
greedy: choose odd or even index stones

880	Decoded String at Index    		23.0%	Medium	
leet2code3: repeat leet 2 times, repeat leetcode 3 times
return the kth character
K<10^9, the decoded string <2^63
note each number is a single digit. can be multiple
ha22 not repeat 22 times, but repeat ha 2 times haha and then repeat haha 2 times.

just keep the length updating. if length==K we got it
if length>K, 
```cpp

string decodeAtIndex(string &S, int K, long long len = 0) {
  for (auto i = 0; i < S.size(); ++i) {
    if (isalpha(S[i])) {
      if (++len == K) return string(1, S[i]);
    }
    else {
      if (len * (S[i] - '0') >= K) return decodeAtIndex(S, K % len == 0 ? len : K % len);
      len *= S[i] - '0';
    }
  }
}
```
optimized
```cpp
    string decodeAtIndex(string S, int K) {
        long N = 0, i;
        for (i = 0; N < K; ++i)
            N = isdigit(S[i]) ? N * (S[i] - '0') : N + 1;
        while (i--)
            if (isdigit(S[i]))
                N /= S[i] - '0', K %= N;
            else if (K % N-- == 0)
                return string(1, S[i]);
        return "lee215";
    }
```
	
881	Boats to Save People    		43.7%	Medium	
two people a time, weight limit. return the min number of boats
greedy+two pointer
```cpp
    int numRescueBoats(vector<int>& people, int limit) {
        sort(people.begin(),people.end());
        int i=0,j=people.size()-1;
        int total=0;
        while(i<=j)
        {
            if(people[i]+people[j]<=limit) {i++;}
            j--;
            total++;
        }
        return total;
    }
```	

885	Spiral Matrix III    		64.0%	Medium	
while changing direction, length along direction also needs change.

```cpp
    vector<vector<int>> spiralMatrixIII(int R, int C, int r, int c) {
        vector<vector<int>> res = {{r, c}};
        int x = 0, y = 1, tmp;
        for (int n = 0; res.size() < R * C; n++) {
            for (int i = 0; i < n / 2 + 1; i++) {
                r += x, c += y;
                if (0 <= r && r < R && 0 <= c && c < C)
                    res.push_back({r, c});
            }
            tmp = x, x = y, y = -tmp;
        }
        return res;
    }
```	
886	Possible Bipartition    		40.6%	Medium	
disliked pair cannot be on the same party
convert edges into adjacency matrix and do dfs coloring
```cpp
    bool possibleBipartition(int N, vector<vector<int>>& dislikes) {
        vector<int> color(N);
        vector<vector<int>> adj(N);
        for(int i=0;i<dislikes.size();i++)
        {
            adj[dislikes[i][0]-1].push_back(dislikes[i][1]-1);
            adj[dislikes[i][1]-1].push_back(dislikes[i][0]-1);
        }
        for(int i=0;i<N;i++)
        {
            if(!color[i] && !dfs(adj,i,color,1)) return 0;
        }
        return 1;
    }
    bool dfs(vector<vector<int>>& adj,int i,vector<int>& color,int nc)
    {
        if(color[i]) return nc==color[i];
        color[i]=nc;
        for(int j=0;j<adj[i].size();j++) //adj[i] could be empty
        {
            int node=adj[i][j];
            if(color[node] && color[node]!=-nc) return 0;
            if(!color[node] && !dfs(adj,node,color,-nc)) return 0;
        }
        return 1;
    }
```
	
889	Construct Binary Tree from Preorder and Postorder Traversal    		59.8%	Medium	
For two subarrays pre[a,b] and post[c,d], if we want to reconstruct a tree from them, we know that pre[a]==post[d] is the root node.

[root][......left......][...right..]  ---pre
[......left......][...right..][root]  ---post
pre[a+1] is the root node of the left subtree.
Find the index of pre[a+1] in post, then we know the left subtree should be constructed from pre[a+1, a+idx-c+1] and post[c, idx].

for example pre=[1,2,4,5,3,6,7], post = [4,5,2,6,7,3,1]
pre[0] and post.back() is the root. the first in the left (preorder) will be the last left node in post.
so 2,4,5 is the left tree and 6,7,3 is the right tree.

Here is my code:

class Solution {
public:
    unordered_map<int, int> m; // value->index
    TreeNode* constructFromPrePost(vector<int>& pre, vector<int>& post) {
        int len = post.size();
        for (int i = 0; i < len; i++) m[post[i]] = i;
        return construct(pre, post, 0, len - 1, 0, len - 1);
    }
    
    TreeNode* construct(vector<int>& pre, vector<int>& post, int a, int b, int c, int d) {
        TreeNode* n = new TreeNode(pre[a]);
        if (a == b) return n;
        int t = pre[a + 1];
        int idx = m[t];
        int len = idx - c + 1;
        n->left = construct(pre, post, a + 1, a + len, c, c + len - 1);
        if (idx + 1 == d) return n;
        n->right = construct(pre, post, a + len + 1, b, idx + 1, d - 1);
        return n;
    }
};
```
890	Find and Replace Pattern    		71.1%	Medium	
words pattern, all converted to same pattern
```cpp
    vector<string> findAndReplacePattern(vector<string>& words, string pattern) {
        pattern=strmap(pattern);
        vector<string> ans;
        for(int i=0;i<words.size();i++)
        {
            //cout<<words[i]<<"->"<<strmap(words[i])<<endl;
            if(strmap(words[i])==pattern) ans.push_back(words[i]);
        }
        return ans;
    }
    string strmap(string str)
    {
        int j=0;
        unordered_map<char,char> ms;
        for(int i=0;i<str.size();i++)
        {
            if(ms.count(str[i])) str[i]=ms[str[i]];
            else {ms[str[i]]=j+'a';str[i]=j+'a';j++;}
        }
        return str;
    }
```
	
894	All Possible Full Binary Trees    		70.6%	Medium	
N nodes: the combination is:
left+right+1=N
1,3,5,7,....
```cpp
    vector<TreeNode*> allPossibleFBT(int N) {
        vector<TreeNode*> vt;
        if(N<1 || N%2==0) return vt;
        if(N==1) {vt.push_back(new TreeNode(0));return vt;}
        N=N-1; //has a root subtracted
        for(int i=1;i<N;i+=2) //i start from 1 since it shall be balanced
        {
            vector<TreeNode*> vl=allPossibleFBT(i);
            vector<TreeNode*> vr=allPossibleFBT(N-i);
            for(int j=0;j<vl.size();j++)
                for(int k=0;k<vr.size();k++)
                {
                    TreeNode* r=new TreeNode(0);
                    r->left=vl[j];
                    r->right=vr[k];
                    vt.push_back(r);
                }
        }
        return vt;
    }
```	

898	Bitwise ORs of Subarrays    		33.9%	Medium	
return number of possible bitor results for all subarray
brutal force:
```cpp
    int subarrayBitwiseORs(vector<int>& A) {
        unordered_set<int> s;
        for(int i=0;i<A.size();i++)
        {
            int t=0;
            for(int j=i;j<A.size();j++) 
                t|=A[j],s.insert(t);
            //s.insert(A[i]);
        }
        return s.size();
    }
```
TLE

```cpp
    int subarrayBitwiseORs(vector<int>& A) {
        //sort(A.begin(),A.end());
        //auto it=unique(A.begin(),A.end());
        //A.resize(it-A.begin());
        unordered_set<int> s,cur;
        for(int i=0;i<A.size();i++)
        {
            unordered_set<int> t;
            for(auto it=cur.begin();it!=cur.end();it++) 
                t.insert(*it|A[i]);
            t.insert(A[i]);//itself
            for(auto it=t.begin();it!=t.end();it++)
                s.insert(*it);
            cur=t;
        }
        //copy(s.begin(),s.end(),ostream_iterator<int>(cout, " "));
        
        
        return s.size();
    }
```

900	RLE Iterator    		50.1%	Medium	
run length encoding, [3,8,0,9,2,5] it is three 8 zero 9 and two 5s
save two arrays: one array for the numbers one array for the repeat (accum sum to make it growing)
```cpp
    vector<long long> v1;
    vector<int> vnum;
    long long cur;
    RLEIterator(vector<int> A) {
        cur=0;
        for(int i=0;i<A.size();i++)
        {
            if(i%2) vnum.push_back(A[i]);
            else v1.push_back(A[i]+(i==0?0:v1.back()));
        }
    }
    
    int next(int n) {
        cur+=n;
        //binary search to find the position
        auto it=lower_bound(v1.begin(),v1.end(),cur); //<=
        if(it==v1.end()) return -1;
        int ind=it-v1.begin();
        return vnum[ind];
    }
```	

901	Online Stock Span    		48.8%	Medium	
previous days: max number of consective days <= today's price
stack
```cpp
    vector<int> prices;
    stack<int> st; //stack stores the index with decreasing order
    int days;
    StockSpanner() {
        days=0;
        st.push(0);
        prices.push_back(INT_MAX);
    }
    
    int next(int price) {
        prices.push_back(price);
        //if(st.empty()) {st.push(0);return 1;}
        //int last_day;
        while(st.size() && price>=prices[st.top()]) st.pop();
        days++;
        int ans;
        ans=days-st.top();
        st.push(days);
        return ans;
    }
904	Fruit Into Baskets    		41.4%	Medium	
tree[i] is the fruit type. 
two baskets: each basket can only have one fruit of any type.
return the total amount of fruits, you can start at any tree and only can go to right
understanding the problem is critical: each basket can only have one type of fruit
so this is to find the longest subarray which contains only two types of elements
sliding window using two pointer
```cpp
    int totalFruit(vector<int>& tree) {
        //this is to get the longest subsequence with only two elements inside
        //need to be the longest one
        //use two pointer
        int i=0,j=0;
        int maxlen=0;
        unordered_map<int,int> mp;
        for(int i=0;i<tree.size();i++)
        {
            //j=i;
            while(mp.size()<=2 && j<tree.size()) {mp[tree[j]]++;j++;}
            maxlen=max(maxlen,j-i-1);
            if(mp.size()<=2) maxlen=max(maxlen,j-i);
            if(j==tree.size()) break;
            mp[tree[i]]--;
            if(mp[tree[i]]<=0) mp.erase(tree[i]);
            //cout<<i<<" "<<j<<" "<<maxlen<<endl;
            //for(auto it=mp.begin();it!=mp.end();it++) cout<<it->first<<":"<<it->second<<" ";
            //cout<<endl;
        }
        return maxlen;
    }
```

907	Sum of Subarray Minimums    		27.2%	Medium	
sum of all subarray mins.
each number can be a min and can appear multiple times.
if nums[i] is the min in the window [L,R] then the sum is (R-j)*(j-L)
```cpp
    int sumSubarrayMins(vector<int>& A) {
        int mod=1e9+7;
        //each element can appear many times
        //for element i, we have a window size which A[i] is the minimum
        vector<int> win(A.size());
        int ans=0;
        for(int i=0;i<A.size();i++)
        {
            int l=i,r=i;
            while(A[r+1]>A[i] && r<A.size()-1) r++;
            while(l>0 && A[l-1]>=A[i]) l--;
            //int seg=win[i]=r-l+1;
            //cout<<l<<" "<<i<<" "<<r<<endl;
            //cout<<(r-i+1)*(i-l+1)*A[i]<<endl;
            ans+=((((r-i+1)*(i-l+1))%mod)*A[i])%mod;
            ans%=mod;
        }
        return ans;
    }
```	
909	Snakes and Ladders    		33.2%	Medium	
board arranged 1 to n*n from bottom left left->right->left...
you can go 1 to 6 steps (die roll)
if you reach a snake: you go to the destination
if you reach a ladder, you go to the destination
return the min steps to reach n*n
The tricky part: 
the index vs the grid value. first reversed, second, we have a row reverse
the n*n can be on [0,0] or [0,n-1].
note: the value keeps growing, so using value is easier instead of using index.
this is more like a 1d problem. converting the board to a 1d array shall make it easier.

bfs: 
```cpp
	int snakesAndLadders(vector<vector<int>>& board) {
		vector<int> b1d;
		int n=board.size();
		for(int i=n-1;i>=0;i--)
		{
			int trow=n-1-i;
			if(trow%2==0) for(int t: board[i]) b1d.push_back(t);
			else for(int j=n-1;j>=0;j--) b1d.push_back(board[i][j]);
		}
        //copy(b1d.begin(),b1d.end(),ostream_iterator<int>(cout," "));cout<<endl;
        vector<bool> visited(b1d.size());
		queue<int> q;
		q.push(0);
        visited[0]=1;
		int step=0;
		while(q.size())
		{
			int sz=q.size();
			for(int i=0;i<sz;i++)
			{
				int ind=q.front();
				q.pop();
				for(int j=1;j<=6;j++)
				{
                    if(ind+j>n*n) break;
                    if(ind+j==n*n) return step+1;
                    if(visited[ind+j]) continue;
					if(b1d[ind+j]<0) {q.push(ind+j);visited[ind+j]=1;}
					else {
                        if(b1d[ind+j]==n*n) return step+1;
                        if(!visited[b1d[ind+j]-1]){
                            q.push(b1d[ind+j]-1); //index
                            visited[b1d[ind+j]-1]=1;
                        }
                    }
				}
			}
			step++;
		}
		return -1;
	}
```	
- sometimes, ladder and snake might not be the shortest, need keep all the queued nodes
- sometimes, the ladder and snake will introduce a loop.
- early determine the report is not a good habit, but we can leave it for judge at the pop time.
- first start can be -1 or >0
- still has some cases failure: when we have continuous ladder. These nodes shall be placed in the same layer.
so change the if to a while loop.
-1 1 2 -1
2 13 15 -1
-1 10 -1 -1
-1 6 2 8
the shortest path is -1->(10->13)->16, ie. the snake and ladder shall not count one step

910	Smallest Range II    		23.6%	Medium	
add +k or -k to each element
smallest difference of max and min
two number: a<b
if the diff >2*k we shall a+k and b-k, new diff is b-a-2*k
if the diff <2*k, we have choice:
a, b both +k or -k
Sort the vector.
Assuming there is a point, on the left of the point, all elements add K, on the right of the point, all elements substract K, check each possible point. (a point is between two numbers).
```cpp
class Solution {
public:
    int smallestRangeII(vector<int>& A, int K) {
        sort(A.begin(), A.end());
        int res = A[A.size() - 1] - A[0];
        int left = A[0] + K, right = A[A.size() - 1] - K;
        for (int i = 0; i < A.size() - 1; i++) {
            int maxi = max(A[i] + K, right), mini = min(left, A[i + 1] - K);
            res = min(res, maxi - mini);
        }
        return res;
    }
};
```
this one is simpler to understand. It is equivalent to add 0 or 2*K to each number
so we add to smaller number hoping to reduce the difference.
```cpp
    int smallestRangeII(vector<int> A, int K) {
        sort(A.begin(), A.end());
        int n = A.size(), mx = A[n - 1], mn = A[0], res = mx - mn;
        for (int i = 0; i < n - 1; ++i) {
            mx = max(mx, A[i] + 2 * K);
            mn = min(A[i + 1], A[0] + 2 * K);
            res = min(res, mx - mn);
        }
        return res;
    }
```	

911	Online Election    		46.7%	Medium	
compare with counter and time (if tie)
using set or priority_queue to make a heap.
lee has a simpler using map:
In the order of time, we count the number of votes for each person.
Also, we update the current lead of votes for each time point.
if (count[person] >= count[lead]) lead = person
Time Complexity: O(N)

Query part:
Binary search t in times, find out the latest time point no later than t.
Return the lead of votes at that time point.
Time Complexity: O(logN)
using a map to record time and person (binded)
using a hashmap to record the number of votings
```cpp
    map<int, int> m;
    TopVotedCandidate(vector<int> persons, vector<int> times) {
        int n = persons.size(), lead = -1;
        unordered_map<int, int> count;
        for (int i = 0; i < n; ++i) m[times[i]] = persons[i];
        for (auto it : m) {
            if (++count[it.second] >= count[lead])lead = it.second;
            m[it.first] = lead;
        }
    }

    int q(int t) {
        return (--m.upper_bound(t))-> second;
    }
```	

912	Sort an Array    		63.8%	Medium	
length of the array up to 1e4
range from -5e4 to 5e4
quick sort: choose a random pivot, all smaller than it put left, other right

```cpp
    void quickSort(vector<int>& V, int from, int to) {
       if (from + 1 >= to) return;
       // choose random pivot:
       int piv = V[from + rand() % (to - from)];
       
       int i = from - 1, j = to;
       while (true) {
          do i++; while (V[i] < piv);
          do j--; while (V[j] > piv);
          if (i >= j) break;
          swap(V[i], V[j]);
       }
       quickSort(V, from, j + 1);
       quickSort(V, j + 1, to);
    }

    vector<int> sortArray(vector<int>& nums) {
      quickSort(nums, 0, nums.size());
      return nums;
    }
```	
merge sort
bubble sort
count sort

915	Partition Array into Disjoint Intervals    		43.2%	Medium	
all left <= all right
both left and right cannot be empty
make left smallest
two direction get max
```cpp
    int partitionDisjoint(vector<int>& A) {
        int n = A.size(), pmax = 0;
        vector<int> B(n);
        B[n - 1] = A[n - 1];
        for (int i = n - 2; i > 0; --i)
            B[i] = min(A[i], B[i + 1]);
        for (int i = 1; i < n; ++i) {
            pmax = max(pmax, A[i - 1]);
            if (pmax <= B[i]) return i;
        }
    }
```
	
916	Word Subsets    		45.2%	Medium	
straightforward using hashmap compare

918	Maximum Sum Circular Subarray    		31.7%	Medium	
max subarray sum
approach 1: double the array and avoid circular
approach 2: find the max and min at the same time. if one cross the end, the other must be inside.

```cpp
    int maxSubarraySumCircular(vector<int>& A) {
        int total = 0, maxSum = -30000, curMax = 0, minSum = 30000, curMin = 0;
        for (int a : A) {
            curMax = max(curMax + a, a);
            maxSum = max(maxSum, curMax);
            curMin = min(curMin + a, a);
            minSum = min(minSum, curMin);
            total += a;
        }
        return maxSum > 0 ? max(maxSum, total - minSum) : maxSum;
    }
```
compare with the single element to start a new group
	

919	Complete Binary Tree Inserter    		55.0%	Medium	
recursive: depth is the same, goes to right. one case, if right is also full, we need add a layer.
array format: parent i, child 2*i and 2*i+1, full tree using array is always the first choice
```cpp
    vector<TreeNode*> tree;
    CBTInserter(TreeNode* root) {
        tree.push_back(root);
        for(int i = 0; i < tree.size();++i) 
        {
            if (tree[i]->left) tree.push_back(tree[i]->left);
            if (tree[i]->right) tree.push_back(tree[i]->right);        
        }
    }
    
    int insert(int v) {
        int N = tree.size();
        TreeNode* node = new TreeNode(v);
        tree.push_back(node);
        if (N % 2)
            tree[(N - 1) / 2]->left = node;
        else
            tree[(N - 1) / 2]->right = node;
        return tree[(N - 1) / 2]->val;        
    }
    
    TreeNode* get_root() {
        return tree[0];
    }
```	


921	Minimum Add to Make Parentheses Valid    		70.0%	Medium	
remove all balanced using stack
```cpp
    int minAddToMakeValid(string S) {
        //to make it balanced, the () shall be the same amount
        //one we have a ) we need have a balanced in its front
        //once we have a ( we need have a balanced in the behind
        stack<char> st;
        int ans=0;
        for(int i=0;i<S.size();i++)
        {
            if(S[i]=='(') st.push(S[i]);
            else 
            {
                if(st.size()) st.pop();
                else ans++; 
            }
        }
        return ans+st.size();
    }
```
	
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
sliding window with at most K zeros inside
```cpp
    int longestOnes(vector<int>& A, int K) {
       //a sliding window which contains zeros<=K
        int i=0,j=0;
        int cnt0=0,ans=0;
        while(j<A.size())
        {
            if(A[j]==0) cnt0++;
            while(cnt0>K) cnt0-=(A[i++]==0); 
            ans=max(ans,j-i+1);
            j++;
        }
        return ans;
    }
```	
1006	Clumsy Factorial    		54.1%	Medium	
clumsy(10) = 10 * 9 / 8 + 7 - 6 * 5 / 4 + 3 - 2 * 1.

```cpp
    int clumsy(int N) {
       int ans=N;
        char op[]={'*','/','+','-'};
        stack<int> st;
        int j=0;
        for(int i=N-1;i>=1;i--)
        {
            switch(op[j%4])
            {
                case '*': ans*=i;break;
                case '/': ans/=i;break;
                case '+': if(st.size()) {ans=st.top()-ans;st.pop();}ans+=i;break;
                case '-': st.push(ans);ans=i;break;
            }
            //cout<<ans<<endl;
            j++;
        }
        if(st.size()) {ans=st.top()-ans;st.pop();}
        return ans;
    }
```	
1007	Minimum Domino Rotations For Equal Row    		47.6%	Medium	
must swap to the first column.
```cpp
    int minDominoRotations(vector<int>& A, vector<int>& B) {
        int n = A.size();
        for (int i = 0, a = 0, b = 0; i < n && (A[i] == A[0] || B[i] == A[0]); ++i) {
            if (A[i] != A[0]) a++;
            if (B[i] != A[0]) b++;
            if (i == n - 1) return min(a, b);
        }
        for (int i = 0, a = 0, b = 0; i < n && (A[i] == B[0] || B[i] == B[0]); ++i) {
            if (A[i] != B[0]) a++;
            if (B[i] != B[0]) b++;
            if (i == n - 1) return min(a, b);
        }
        return -1;
    }
```	

1008	Construct Binary Search Tree from Preorder Traversal    		72.8%	Medium	
root is the max > left but < right
```cpp
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        //root, left, right
        return helper(preorder,0,preorder.size());
    }   
    TreeNode* helper(vector<int>& preorder,int l,int r)        
    {
        //divide by two parts, the left and right, the first is the root
        if(l>=r) return NULL;
        TreeNode *root=new TreeNode(preorder[l]);
        int left=l+1;
        int right=r;
        right=upper_bound(preorder.begin()+left,preorder.begin()+right,preorder[l])-preorder.begin();
        /*for(int i=left;i<right;i++) 
        {
            if(preorder[i]>preorder[l]) {right=i;break;}
        }*/
        root->left=helper(preorder,left,right);
        root->right=helper(preorder,right,r);
        return root;
    }
```	
1011	Capacity To Ship Packages Within D Days    		52.8%	Medium	
binary search: find the first true condition
```cpp
    int shipWithinDays(vector<int>& weights, int D) {
        //the ans>=the max of single element
        //< the sum of the array
        int tsum=accumulate(weights.begin(),weights.end(),0);
        int avg=tsum/D;
        int max0=*max_element(weights.begin(),weights.end());
        if(D<2) return tsum;
        //target value >max(max0,avg)
        int left=max(max0,avg),right=tsum;
        while(left<right)
        {
            int mid=(left+right)/2;
            int parts=num_parts(weights,mid);

            if(parts>D) left=mid+1; //the target is too small
            else right=mid; //target is too big
        }
        return left;
    }
    int num_parts(vector<int>& weights,int target)
    {
        int ans=0;
        int psum=0;
        for(int i=0;i<weights.size();i++)
        {
            int t=psum+weights[i];
            if(t>target) //cannot exceed the target
            {
                ans++;
                psum=0;
            }
            psum+=weights[i];
        }
        return ans+(psum<=target);//the last part if combined with previous then it is counted
    }
```
	

1014	Best Sightseeing Pair    		48.3%	Medium	
max the score A[i]+A[j]+i-j i<j, using two direction max
```cpp
    int maxScoreSightseeingPair(vector<int>& A) {
        vector<int> lmax(A.size(),INT_MIN);
        lmax[0]=A[0];
        int n=A.size();
       for(int i=1;i<A.size();i++) 
           lmax[i]=max(A[i]+i,lmax[i-1]);
        int ans=lmax[n-2]+A[n-1]-n+1;
        A[n-1]-=n-1;
        for(int i=A.size()-2;i>0;i--)
        {
            A[i]=max(A[i]-i,A[i+1]);
            ans=max(lmax[i-1]+A[i],ans);
        }
        return ans;
    }
```
	
1015	Smallest Integer Divisible by K    		27.9%	Medium	
N divisible by K and N only contains digit 1
For a given K, we evaluate 1 % K, 11 % K, 111 % K, ..., 11...1 (K '1's) % K.

If any remainder is 0, then the current number is the smallest integer divisible by K.
If none of the remainders is 0, then at some point, there must be some duplicate remainders (due to Pigeonhole principle), as the K remainders can only take at most K-1 different values excluding 0. In this case, no number with the pattern 1...1 is divisible by K. This is because once a remainder has a duplicate, the next remainder will be in a loop, as the previous remainder determines the next_mod, i.e., next_mod = (10 * prev_mod + 1) % K. Therefore, we will never see remainder==0.
A simple example is when K is 6. Once we see 1111 % 6 = 1, we immediately know 11111 % 6 will be 5, since 1 % 6 = 1 and 11 % 6 = 5. Therefore, there will be no such number that is divisible by 6.

1 % 6 = 1
11 % 6 = 5
111 % 6 = 3
1111 % 6 = 1
11111 % 6 = 5
111111 % 6 = 3
Also, it is easy to see that for any number whose last digit is not in {1, 3, 7, 9}, we should return -1.

Theorem: there must be one number from the K-long candidates list [1, 11, 111, ..., 111..111(with K ones)], which is the multiple of K.

Why? Let's think in the opposite way. Is it possible that none of the K candidates is the multiple of K?

If true, then the module of these K candidate numbers (mod K) must be in [1, .., K-1] (K-1 possible module values). Thus, there must be 2 candidate numbers with the same module. Let's denote these two numbers as N_i with i ones, and N_j with j ones and i<j.

Thus N_j-N_i = 1111...1000...0 with (j-i) ones and i zeros. N_j-N_i = 111..11 (j-i ones) * 100000..000 (i zeros). We know that N_i and N_j have the same module of K. Then N_j-N_i is the multiple of K. However, 100000..000 (i zeros) does not contain any factors of K (K is neither multiple of 2 nor multiple of 5). Thus, 111..11 (j-i ones) is the multiple of K. This is contradictory to our assumption that all K candidates including 111..11 (j-i ones) are not multiple of K.

Finally, we know that there is at least one number, from the first K candidates, which is the multiple of K.


```cpp
    int smallestRepunitDivByK(int K) {
        for (int r = 0, N = 1; N <= K; ++N)
            if ((r = (r * 10 + 1) % K) == 0)
                return N;
        return -1;
    }
```	

1016	Binary String With Substrings Representing 1 To N    		62.0%	Medium	
direct approach: get the binary representation and find if pattern match
```cpp
    bool queryString(string S, int N) {
      while (N > 0) {
        auto s = bitset<32>(N--).to_string();
        if (S.find(s.substr(s.find("1"))) == string::npos) return false;
      }
      return true;
    }
```
	
1017	Convert to Base -2    		56.0%	Medium	

same as base 2
```cpp
    string base2(int N) {
        string res = "";
        while (N) {
            res = to_string(N & 1) + res;
            N = N >> 1;
        }
        return res == ""  ? "0" : res;
    }

    string baseNeg2(int N) {
        string res = "";
        while (N) {
            res = to_string(N & 1) + res;
            N = -(N >> 1);
        }
        return res == "" ? "0" : res;
    }
```	
1019	Next Greater Node In Linked List    		56.3%	Medium	
using stack with linked list
```cpp
    vector<int> nextLargerNodes(ListNode* head) {
        stack<int> st;
        vector<int> ans;
        while(head)
        {
            int t=head->val;
            ans.push_back(t);
            while(st.size() && t>ans[st.top()])
            {
                ans[st.top()]=t;
                st.pop();
            }
            st.push(ans.size()-1);
            head=head->next;
        }
        while(st.size()) ans[st.top()]=0,st.pop();
        ans.back()=0;
        return ans;
    }
```
	
1020	Number of Enclaves    		54.9%	Medium	
cell 0 is sea, cell 1 is land. return number of islands which cannot walk off the boundary
straightforward using dfs coloring.

```cpp
    int numEnclaves(vector<vector<int>>& A) {
        int m=A.size(),n=A[0].size();
        for(int i=0;i<m;i++)
        {
            if(A[i][0]) dfs(A,i,0);
            if(A[i][n-1]) dfs(A,i,n-1);
        }
        for(int i=1;i<n-1;i++)
        {
            if(A[0][i]) dfs(A,0,i);
            if(A[m-1][i]) dfs(A,m-1,i);
        }
        int ans=0;
        for(int i=0;i<m;i++)
            for(int j=0;j<n;j++) ans+=A[i][j];
        return ans;
    }
    void dfs(vector<vector<int>>& A,int i,int j)
    {
        int m=A.size(),n=A[0].size();
        if(i<0 || j<0 || i>=m || j>=n) return;
        if(!A[i][j]) return;
        A[i][j]=0;
        dfs(A,i-1,j);
        dfs(A,i+1,j);
        dfs(A,i,j-1);
        dfs(A,i,j+1);
    }
```
	
1023	Camelcase Matching    		56.9%	Medium	
uppercase matching
```cpp
    vector<bool> camelMatch(vector<string>& queries, string pattern) {
        //one by one search: greedy, all left shall be lowercase
        vector<bool> ans(queries.size());
        for(int i=0;i<queries.size();i++)
        {
            ans[i]=match(queries[i],pattern);
        }
        return ans;
    }
    bool match(string& w,string& p)
    {
        int i=0,j=0;
        while(j<p.size() && i<w.size())
        {
            if(w[i]==p[j]) j++;
            else if(isupper(w[i])) return 0;
            i++;
        }
        while(i<w.size()) if(isupper(w[i++])) return 0;
        return j==p.size();
    }
```
	
1024	Video Stitching    		47.0%	Medium	
interval, min number of clips to cover time period [0,T]
```cpp
bool cmp(vector<int> a,vector<int> b) {return a[0]<b[0] || (a[0]==b[0] && a[1]<b[1]);}
class Solution {
public:
    int videoStitching(vector<vector<int>>& clips, int T) {
        //looking for the min number of intervals covering the whole duration
        sort(clips.begin(),clips.end(),cmp);
        if(clips[0][0]<0 || clips.back()[1]<T) return -1;
        //for(int i=0;i<clips.size();i++) cout<<clips[i][0]<<","<<clips[i][1]<<endl;
        int n=clips.size();
        int ans=0;
        //get the leftmost interval with the longest coverage
        int t=0,i=0;
        while(t<T)
        {
            int tend=t;
            for(int i=0;i<n;i++)
                if(clips[i][0]<=t) tend=max(tend,clips[i][1]);else break;
            if(tend<=t) return -1;
            t=tend;
            ans++;
        }
        return ans;
    }
};
```

1026	Maximum Difference Between Node and Ancestor    		59.0%	Medium	
abs(node-ancestor) ancestor is explored
```cpp
	int maxAncestorDiff(TreeNode* root) {
		int maxdiff=0;
		int mn=root->val,mx=root->val;
		preorder(root,mn,mx,maxdiff);
		return maxdiff;
	}
	void preorder(TreeNode* root,int mn,int mx,int& maxdiff)
	{
		if(!root) return;
		maxdiff=max(maxdiff,max(root->val-mn,mx-root->val));
		mn=min(mn,root->val);
		mx=max(mx,root->val);
		preorder(root->left,mn,mx,maxdiff);
		preorder(root->right,mn,mx,maxdiff);
	}
```
	
1027	Longest Arithmetic Sequence    		46.4%	Medium	
dp combined with hashmap since the difference is not bounded.
```cpp
    int longestArithSeqLength(vector<int>& A) {
        //ending with a diff and length
        //dp shall contain i, diff
        int n=A.size();
        vector<unordered_map<int,int>> dp(n); //dp[i] the max length at i with difference d
        //at least 3 elements
        int maxlen=2;
        for(int i=1;i<n;i++)
        {
            for(int j=i-1;j>=0;j--)
            {
                int d=A[i]-A[j];
                if(dp[j].count(d)) {dp[i][d]=max(dp[i][d],dp[j][d]+1);}
                else dp[i][d]=max(dp[i][d],2);
                maxlen=max(maxlen,dp[i][d]);
            }
        }
        return maxlen;
    }
```	
1031	Maximum Sum of Two Non-Overlapping Subarrays    		55.4%	Medium	
both direction max sum

1034	Coloring A Border    		42.4%	Medium	
to avoid new color is same to other color, change it to negative
```cpp
    vector<vector<int>> colorBorder(vector<vector<int>>& grid, int r0, int c0, int color) {
        int oc=grid[r0][c0];
        dfs(grid,r0,c0,oc,color);
        for(int i=0;i<grid.size();i++)
            for(int j=0;j<grid[0].size();j++)
                if(grid[i][j]<0) grid[i][j]=color;
        return grid;
    }
    void dfs(vector<vector<int>>& grid,int r,int c,int oc,int nc)
    {
        if(r<0 ||c<0 ||r>=grid.size() || c>=grid[0].size() || grid[r][c]!=oc) return;
        grid[r][c]=-oc;
        dfs(grid,r+1,c,oc,nc);
        dfs(grid,r-1,c,oc,nc);
        dfs(grid,r,c+1,oc,nc);
        dfs(grid,r,c-1,oc,nc);
        if(r && c && r<grid.size()-1 && c<grid[0].size()-1)
        {
            if((abs(grid[r-1][c])==oc) && 
               (abs(grid[r+1][c])==oc) && 
               (abs(grid[r][c+1])==oc) && 
               (abs(grid[r][c-1])==oc))
                grid[r][c]=oc;
        }
    }
```
	
1035	Uncrossed Lines    		51.2%	Medium	
longest common subsequence
using dp
```cpp
    int maxUncrossedLines(vector<int>& A, vector<int>& B) {
        //preproc(A,B); //to save some space and computation, remove those not appear in both
        //copy(A.begin(),A.end(),ostream_iterator<int>(cout," "));cout<<endl;
        //copy(B.begin(),B.end(),ostream_iterator<int>(cout," "));cout<<endl;
        int m=A.size(),n=B.size();
        vector<vector<int>> dp(m+1,vector<int>(n+1));
        for(int i=1;i<=m;i++)
        {
            for(int j=1;j<=n;j++)
            {
                if(A[i-1]==B[j-1]) dp[i][j]=dp[i-1][j-1]+1;
                else dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
            }
        }
        return dp[m][n];
    }
```	
1038	Binary Search Tree to Greater Sum Tree    		80.2%	Medium	
reversed inorder traversal
```cpp
    TreeNode* bstToGst(TreeNode* root) {
        int prev=0;
        postorder(root,prev);
        return root;
    }
    void postorder(TreeNode* root,int &prev)
    {
        if(!root) return;
        postorder(root->right,prev);
        root->val+=prev;
        prev=root->val;
        postorder(root->left,prev);
    }
```
	
1039	Minimum Score Triangulation of Polygon    		40.2%	Medium	
similar to bellman or dijkstra
```cpp
    int minScoreTriangulation(vector<int>& A) {
    /*
    dp[i][j] means the minimum score to triangulate A[i] ~ A[j],
while there is edge connect A[i] and A[j].

We enumerate all points A[k] with i < k < j to form a triangle.

The score of this triangulation is dp[i][j], dp[i][k] + dp[k][j] + A[i] * A[j] * A[k]
*/
        int n = A.size();
        vector<vector<int>> dp(n, vector<int>(n));
        for (int j = 2; j < n; ++j) {
            for (int i = j - 2; i >= 0; --i) { //i<k<j that is why we need reverse iteration
                dp[i][j] = INT_MAX;
                for (int k = i + 1; k < j; ++k)
                    dp[i][j] = min(dp[i][j], dp[i][k] + dp[k][j] + A[i] * A[j] * A[k]);
            }
        }
        return dp[0][n - 1];
    }
```
	
1040	Moving Stones Until Consecutive II    		45.8%	Medium	
sliding window with min space.

1043	Partition Array for Maximum Sum    		61.3%	Medium	
subarry length <=K. return the max sum(maxj*lenj) j is the subarray number.
sliding window with max, combined with dp.
```cpp
    int maxSumAfterPartitioning(vector<int>& A, int K) {
        //dp
        if(K==1) return accumulate(A.begin(),A.end(),0);
        int n=A.size();
        vector<int> dp(A.size());
        int cur_max=0;
        priority_queue<int> pq;
        for(int i=0;i<A.size();i++)
        {
            pq.push(A[i]);
            if(i<K) 
            {
                cur_max=pq.top();
                dp[i]=(i+1)*cur_max;
            }
            else //over K window
            {
                if(pq.top()==A[i-K]) pq.pop();//until the max is the one to remove out of the window
                cur_max=pq.top();
                int tmax=0;//A[i];
                for(int j=0;j<K;j++) //the length including ith element
                {
                    tmax=max(tmax,A[i-j]);
                    dp[i]=max(dp[i],tmax*(j+1)+dp[i-j-1]);
                }
            }
         }
        return dp[n-1];
    }
```
not using pq:
```cpp
    int maxSumAfterPartitioning(vector<int>& A, int K) {
        int N = A.size();
        vector<int> dp(N);
        for (int i = 0; i < N; ++i) {
            int curMax = 0;
            for (int k = 1; k <= K && i - k + 1 >= 0; ++k) {
                curMax = max(curMax, A[i - k + 1]);
                dp[i] = max(dp[i], (i >= k ? dp[i - k] : 0) + curMax * k);
            }
        }
        return dp[N - 1];
    }	
```

1048	Longest String Chain    		46.3%	Medium	
dp:
```cpp
bool cmp(string& a,string &b) {return a.size()<b.size();}
class Solution {
public:
    int longestStrChain(vector<string>& words) {
        sort(words.begin(),words.end(),cmp);
        //copy(words.begin(),words.end(),ostream_iterator<string>(cout," "));cout<<endl;
        int n=words.size();
        vector<int> dp(n,1); //dp[i] is the longest chain ending with word[i]
        for(int i=1;i<n;i++)
        {
            for(int j=i-1;j>=0;j--)
            {
                if(words[j].size()+1==words[i].size())
                {
                    if(ispred(words[j],words[i])) 
                        dp[i]=max(dp[i],dp[j]+1);
                }
            }
        }
        //copy(dp.begin(),dp.end(),ostream_iterator<int>(cout," "));
        return *max_element(dp.begin(),dp.end());
    }
    bool ispred(string a,string b) //a is shorter
    {
        
        int i=0,j=0;
        while(i<a.size() && j<b.size())
        {
            if(a[i]!=b[j]) j++;
            else i++,j++;
        }
        bool res=(i==a.size() && j<=b.size());
        //cout<<a<<" "<<b<<" "<<res<<endl;
        return res;
    }
};
```

1049	Last Stone Weight II    		38.0%	Medium	
knapsack with + - and to minimize the results.
psum-nsum=min
psum+num=total
min+total=2*psum
min=2*psum-total>=0 we shall make psum as much close to total/2 as possible.
```cpp
    int lastStoneWeightII(vector<int>& stones) {
        int n=stones.size();
        int tsum=accumulate(stones.begin(),stones.end(),0);
        vector<vector<bool>> dp(tsum/2+1,vector<bool>(n+1));
        //boundary
        for(int i=0;i<=n;i++) dp[0][i]=1;//any sized choose 0.
        int maxsum=0;
        for(int i=1;i<=tsum/2;i++)
        {
            for(int j=1;j<=n;j++)
            {
                if(dp[i][j-1] ||(i>=stones[j-1] && dp[i-stones[j-1]][j-1]))
                {
                    dp[i][j]=1;
                    maxsum=max(maxsum,i);
                }
            }
        }
        return tsum-2*maxsum;
    }
```	

1052	Grumpy Bookstore Owner    51.3%	Medium	
x sliding window and choose the max customer in the window
```cpp
    int maxSatisfied(vector<int>& customers, vector<int>& grumpy, int X) {
        int ans=0;
        int sum=0;
        int n=customers.size();
        for(int i=0;i<n;i++) if(!grumpy[i]) sum+=customers[i];
        //cout<<sum;
        for(int i=0;i<=n-X;i++)
        {
            int add=0;
            for(int j=0;j<X;j++)
            {
                int ind=i+j;
                if(grumpy[ind]) add+=customers[ind];
            }
            ans=max(ans,sum+add);
        }
        return ans;
    }
```
1053. Previous permutation with one swap
from right find the first peak and swap with the leftmost on it right 
```cpp
    vector<int> prevPermOpt1(vector<int>& A) {
        if(is_sorted(A.begin(),A.end())) return A;
        //swap the first peak (from right) with its later max < the peak value
        int pkind=0;
        for(int i=A.size()-2;i>=0;i--)
        {
            if(A[i]>A[i+1]) {pkind=i;break;}
        }
        
        for(int i=A.size()-1;i>pkind;i--) 
            if(A[pkind]>A[i]) {
                while(i>pkind && A[i]==A[i-1]) i--;
                swap(A[i],A[pkind]);break;}
        return A;
    }
```
	
1054	Distant Barcodes   37.5%	Medium	
this is same as the 358 621.

1072. Flip Columns For Maximum Number of Equal Rows
we need check how many rows are identical. 
just follow flip column will not be able to solve it.
up to 300x300.
hashmap is the way to go, but how to use the key?
up to 300 digits cannot fit in any integer. string? not good for checking against the map

Finding maximum number of rows that have identical relative ordering with respect to the top elements.
this is very smart since we change to one case: all start with 1.

```cpp
    int maxEqualRowsAfterFlips(vector<vector<int>>& matrix) {
		//row must be same or flip each bit the same.
		//so problem is converted to find the max number of such rows.
		int m=matrix.size(),n=matrix[0].size();
		unordered_map<string,int> mp;
		for(auto& v: matrix){
			int t=v[0];
			string s;
			for(int i: v) {if(i==t) s+='1';else s+='0';}
			mp[s]++;
		}
		//find the max
		int ans=0;
		for(auto t: mp) ans=max(ans,t.second);
		return ans;
	}
```	

1073. Adding Two Negabinary Numbers
similar to -2 base 

1079. Letter Tile Possibilities
this asks for permutation, we can use combination and then get combination
- using a set to reduce duplicate
- sort the results to reduce duplicate permutation
- a combination to get the permutation
```cpp
    int numTilePossibilities(string tiles) {
        unordered_set<string> ms;
        int ans=0;
        vector<bool> visited(26);
        dfs(tiles,0,"",ms);
        //for each string in the ms, we can calculate number of its permutation
        for(auto t: ms)
            ans+=numperm(t);
        return ans;
    }
    void dfs(string& s,int start,string t,unordered_set<string>& ms)
    {
        if(t.size()) {string tt=t;sort(tt.begin(),tt.end());ms.insert(tt);}
        
        for(int i=start;i<s.size();i++)
        {
            t+=s[i];
            dfs(s,i+1,t,ms);
            t.pop_back();
        }
    }
    int numperm(string& t)
    {
        //cout<<t<<endl;
        vector<int> cmap(26);
        for(char c: t) cmap[c-'A']++;
        int n=t.length();
        int ans=1;
        for(int i=0;i<26;i++) //C(n,m) for each
            if(cmap[i]>0) {
                ans*=comb(n,cmap[i]);
                n-=cmap[i];
            }
        return ans;
    }
    //see 377
    int comb(int m,int n)
	{//m!/(n!*(m-n)!)
		int ans=1;
    	for(int i=n+1,j=1;i<=m || j<=m-n;i++,j++) {if(i<=m)ans*=i;if(j<=m-n) ans/=j;}
		return ans;
	}
```

1080. Insufficient Nodes in Root to Leaf Paths
first we can come up the O(N^2) approach:
two recursive problem: 
- if the sum from root to leaf > limit, return 1, else return 0
- left and right subproblem limt-root->val

```cpp
    TreeNode* sufficientSubset(TreeNode* root, int limit) {
		//dfs to get sum from root to leaf
		if(!root) return NULL;
		if(dfs_sum(root,0,limit)==0) //need delete this node
			return NULL;
		root->left=sufficientSubset(root->left,limit-root->val);
		root->right=sufficientSubset(root->right,limit-root->val);
		return root;
    }
	//count number of branches with sum>limit
	int dfs_sum(TreeNode* root,int sum,int limit)
	{
		if(!root) return 0;
		sum+=root->val;
		if(!root->left && !root->right) //if the branch sum>limit
			return sum>=limit;
		return dfs_sum(root->left,sum,limit)+
		dfs_sum(root->right,sum,limit);
	}
```
O(N)
think it along a path, limit-root for left and right.
```cpp
    TreeNode* sufficientSubset(TreeNode* root, int limit) {
        if (root->left == root->right) //leaf node
            return root->val < limit ? NULL : root;
        if (root->left)
            root->left = sufficientSubset(root->left, limit - root->val);
        if (root->right)
            root->right = sufficientSubset(root->right, limit - root->val);
        return root->left == root->right ? NULL : root;
    }
```	

1081. Smallest Subsequence of Distinct Characters
identical to 316
using stack. if current char < current res.back, we need pop it back.
```cpp
    string smallestSubsequence(string s, string res = "") {
      int cnt[26] = {}, used[26] = {};
      for (auto ch : s) ++cnt[ch - 'a']; //hashmap
      for (auto ch : s) {
        --cnt[ch - 'a'];
        if (used[ch - 'a']++ > 0) continue; //already used.
        while (!res.empty() && res.back() > ch && cnt[res.back() - 'a'] > 0) {
          used[res.back() - 'a'] = 0;
          res.pop_back();
        }
        res.push_back(ch);
      }
      return res;
    }   
```	
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

341	Flatten Nested List Iterator    		47.9%	Medium	
has a base class NestedInteger
has member :
isInteger
getInteger
getList
using a stack to store the recursive relation or a vector
stack is better since it will remove the first element first
every time call has next we decompose a list until we get an integer

```cpp
	stack<NestedInteger> st;
    NestedIterator(vector<NestedInteger> &nestedList) {
        for(int i=nestedList.size()-1;i>=0;i--)
			st.push(nestedList[i]);
    }

    int next() {
		if(hasNext()){
			auto t=st.top();
			st.pop();
			return t.getInteger();
		}
    }

    bool hasNext() {
        while(st.size()){
			auto curr=st.top();
			if(curr.isInteger()) return 1;
			st.pop();
			auto t=curr.getList();
			int sz=t.size();
			for(int i=sz-1;i>=0;i--)
				st.push(t[i]);
		}
		return 0;
    }
```	
385	Mini Parser    		31.8%	Medium	
this is very similar to the problem 341
given a string, we need to serialize the nestedInteger object
serializing and deserializing used to read/write the objects, which are often required.
[123,[456,[789]]]
this is a nestedInteger with a integer and another nestedInteger [456,[789]]
base class NestedInteger has:
default constructor
constructor
isInteger
getIntegersetInteger
add
getList

recursive:
```cpp
    NestedInteger deserialize(string s) {
        istringstream in(s);
        return deserialize(in);
    }

    NestedInteger deserialize(istringstream &in) {
        int number;
        if (in >> number) //a number
            return NestedInteger(number);
        in.clear();
        in.get(); //start with [
        NestedInteger list;
        while (in.peek() != ']') {
            list.add(deserialize(in));
            if (in.peek() == ',')
                in.get();
        }
        in.get();
        return list;
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

394	Decode String    		44.9%	Medium	
stack and recursion problem!
approach 1: using stack but no recursion.
approach 2: no stack but uses recursion
approach 3: use position and keep updating

"3[a2[c]]"->3[acc]->accaccacc
between a paired [] it is a subproblem, we may need two stacks
3[a]2[bc]: copy 3, a, copy 2, bc
3[a2[c]]: push 3, push a, 2 c, acc and copy 3 times
```cpp
    string decodeString(string s) {
        string ans;
        stack<string> strs;
        stack<int> copies;
        int num=0;
        for(char c: s){
            if(isdigit(c)) num=num*10+c-'0';
            else if(isalpha(c)) ans+=c;
            else if(c=='['){
                strs.push(ans);
                copies.push(num);
                num=0,ans="";
            }
            else{
                string t=ans;
                int n=copies.top();
                //cout<<t<<": "<<n<<endl;
                while(--n>0) ans+=t;
                ans=strs.top()+ans;
                strs.pop(),copies.pop();
            }
        }
        return ans;
    }
```	
recurive: only the start char to end, no ending specified.

```cpp
    string decodeString(string s) {
        int i = 0;
        return decodeString(s, i);
    }
	string decodeString(string& s,int& i)
	{
		string ans;
		while(i<s.length() && s[i]!=']'){
			if(!isdigit(s[i]) ans+=s[i++];
			else{
				int n=0;
				while(i<s.length() && isdigit(s[i]) n=n*10+s[i++]-'0';
				i++;//[
				string t=decodeString(s,i);
				i++;//]
				while(n--) ans+=t;
			}
		}
		return ans;
	}
```
this problem is very similar to the atom problem. 726 bunber of atoms, use lower and upper case

373	Find K Pairs with Smallest Sums    		33.8%	Medium	
two sorted array and find the k pairs with smallest sum
the elements can be used multiple times
using merge sort. or pq.
for example: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
approach 1: m*n sum push into pq
```cpp
	struct comp{
		bool operator()(const vector<int>& a,const vector<int>& b){
			return a[0]+a[1]<b[0]+b[1];
		}
	};
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
		vector<vector<int>> ans;
		priority_queue<vector<int>,vector<vector<int>>,comp> pq;
		for(int i: num1)
			for(int j: nums2){
				pq.push({i,j});
				if(pq.size()>k) pq.pop();
			}
		while(pq.size()){
			ans.push_back(pq.top());
			pq.pop();
		}
		return {ans.rbegin(),ans.rend()};
    }
```	
however we did not take advantage that two arrays are sorted and above solution is very slow.
minheap:
first using the first row combined with the first element in row 2, put them in minheap

every time when we get the min suppose it is M[i,j], then the next larger would be among M[i+1,j] and M[i,j+1]
note some elements will be pushed several times and causes wrong results.
```cpp
	struct comp{
		bool operator()(const vector<int>& a,const vector<int>& b){
			return a[0]+a[1]>b[0]+b[1];
		}
	};
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
		vector<vector<int>> ans;
        if(nums1.empty() || nums2.empty() || k<=0) return ans;
        unordered_set<int> visited;
        int m=nums1.size(),n=nums2.size();
		priority_queue<vector<int>,vector<vector<int>>,comp> pq;//pq store a,b,i,j
		pq.push({nums1[0],nums2[0],0,0});
        
		while(k-- && pq.size()){
			auto t=pq.top();pq.pop();
			ans.push_back({t[0],t[1]});
            
			int i=t[2],j=t[3];
			if(i+1<nums1.size() && j<nums2.size() && !visited.count((i+1)*n+j)) {
                pq.push({nums1[i+1],nums2[j],i+1,j});
                visited.insert((i+1)*n+j);
            }
			if(j+1<nums2.size() && i<nums1.size() && !visited.count(i*n+j+1) ) {
                pq.push({nums1[i],nums2[j+1],i,j+1});
                visited.insert(i*n+j+1);
            }
		}
		return ans;
    }
```	
we need to introduce a hashset to mark visited.

378	Kth Smallest Element in a Sorted Matrix    		49.4%	Medium
row and columns are sorted
find the kth element in 2D
we can build a pq. 
this is the same as 373 with multiple sorted lists involved
use similar code:
	
	struct comp{
		bool operator()(const vector<int>& a,const vector<int>& b){
			return a[0]>b[0];
		}
	};    
    int kthSmallest(vector<vector<int>>& matrix, int k) {
		int ans;
        int n=matrix.size();
        unordered_set<int> visited;
        
		priority_queue<vector<int>,vector<vector<int>>,comp> pq;//pq store a,b,i,j
		pq.push({matrix[0][0],0});
        
		while(k-- && pq.size()){
			auto t=pq.top();pq.pop();
			int i=t[1]/n,j=t[1]%n;
			ans=t[0];
			if(i+1<n && j<n && !visited.count((i+1)*n+j)) {
                pq.push({matrix[i+1][j],(i+1)*n+j});
                visited.insert((i+1)*n+j);
            }
			if(j+1<n && i<n && !visited.count(i*n+j+1) ) {
                pq.push({matrix[i][j+1],i*n+j+1});
                visited.insert(i*n+j+1);
            }
		}
		return ans;
    }
```
we can use bool array for visited to speed

395	Longest Substring with At Least K Repeating Characters    		38.6%	Medium	
using two pointer to define a range, and count number of unique letters and each letter's count.
all chars appear less than k times shall be discarded.
two pointer is not that easy
aaabb
bbaaa
ababa
need another counter to record the frequency

use divide and conquer: found those char with freq<K and divide it into left and right.

```cpp
    int longestSubstring(string s, int k) {
        //divide and conquer using recursion
        if(s.size()<k) return 0;
        int mp[26]={0};
        for(int i=0;i<s.size();i++) mp[s[i]-'a']++;
        //find the first one with count<k it is not included
        int indx=-1;
        for(int i=0;i<s.size();i++)
        {
            if(mp[s[i]-'a']<k) {indx=i;break;}
        }
        if(indx<0) return s.size();
        
        int left=longestSubstring(s.substr(0,indx),k);
        int right=longestSubstring(s.substr(indx+1),k);
        return max(left,right);
    }
```	
experience: if you cannot solve it easily with one method, try different algorithm
recursive and divide and conquer will divide problem into smaller and easier problems
