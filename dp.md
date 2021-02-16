# Advanced topics on DP

- dp is recursion based algorithm with memoization on solved subproblems.
- dp: overlapped subproblems.
- get familiar with dp patterns and solve using similar approaches.
- top down is generally easier for uncleared recurrence relation.

I grouped the dp problems into the following categories so we can develope intuition in recoginizing pattern. Actually the classification is mixed, and may be solved using other patterns.

- longest common subsequence

- longest increasing subsequence 

- dealing with subarray or interval

- knapsack

- counting number of ways

- interlaced two states

- shortest distance problem

- ending with

- array or matrix traversal

- bitmask dp


### overlapped subproblems or general dp
see how larger problem can be solved using previous smaller subproblem.
- identify the overlapping subproblem feature.
- identify the recurrence relation
- identify the final answer
- identify the base case.

-746	Min Cost Climbing Stairs    		50.7%	Easy	
-338	Counting Bits    		70.0%	Medium	
dp[i]=dp[i>>1]+(i&1)
-279	Perfect Squares    		48.2%	Medium	
given n, find the min number of square number sum, dp[i]=min(dp[i-j*j]+1) with dp[0]=0
-322	Coin Change    		36.5%	Medium	
given n and a list of coins, find the min number of coins used. dp[i]=min(1+dp[i-c]) with dp[0]=0
-343	Integer Break    		50.8%	Medium	
max product dividing n into sum of numbers.
dp[i]=max(dp[i],dp[j]*(i-j),j*(i-j))

-263	Ugly Number    		41.7%	Easy	
-264	Ugly Number II    		42.5%	Medium	
-313	Super Ugly Number    		45.6%	Medium	
-198	House Robber    		42.6%	Easy	
-213	House Robber II    		37.2%	Medium	
-337	House Robber III    		51.0%	Medium	
-740	Delete and Earn    		48.9%	Medium	
-374	Guess Number Higher or Lower    		44.0%	Easy	
-375	Guess Number Higher or Lower II    		41.4%	Medium	
-1387	Sort Integers by The Power Value    		70.6%	Medium	
using hashmap to store only required solutions.
-808	Soup Servings    		40.5%	Medium	
-828	Count Unique Characters of All Substrings of a Given String    		46.5%	Hard
better using non-dp approach. each char can be unique char in subarray in a range. dp approach is harder not intuitive.

Problems with more tricks
-837	New 21 Game    		35.2%	Medium	****
dp[i] is the probability to reach i points.
dp[0]=1, dp[i]=(dp[i-w+1]+....dp[i-1])/W sliding window
>=K, no more accumulate since stop drawing (but sliding window shall keep going)
- add all dp from K to N. (if N>K+W prob=1.0).
sliding window + dp. pretty tricky.

-873	Length of Longest Fibonacci Subsequence    		47.9%	Medium	****
the key to this problem is need to approach using 2d dp.
dp[i,j] the fib ending with arr[i] and arr[j] with i>j.

-650	2 Keys Keyboard    		49.6%	Medium	***
there is one A, copy all, paste. min number of operations to get n 'A's.
dp[i]=min(dp[j]+i/j)

-651	4 Keys Keyboard    		52.8%	Medium	****
four operations: print A, select all, copy, paste. find max number of 'A's if operate n times.
- option 1: print 'A' dp[i]=dp[i-1]+1
- option 2: from j press select+copy+paste+paste.... dp[i]=max(dp[j]*(i-j-1))

-823	Binary Trees With Factors    		36.1%	Medium	****
dp[i] represent the number of ways to construct the tree with root=i.
dp[i]=dp[j]*dp[k], j*k=i, i,j,k must be in the list.

-517	Super Washing Machines    		38.4%	Hard	****

-568	Maximum Vacation Days    		41.2%	Hard	
-656	Coin Path    		29.3%	Hard	
-818	Race Car    		39.4%	Hard	
-887	Super Egg Drop    		27.1%	Hard	
-471	Encode String with Shortest Length    		48.4%	Hard	
-1483	Kth Ancestor of a Tree Node    		29.5%	Hard	
-87	Scramble String    		34.2%	Hard	***

### array or matrix traversal
generally not hard dp problem since the recurrence relation is not hard to find.

- left traversal and right traversal for 1d
-135	Candy    		32.4%	Hard	

- go bottom right and go left top for 2d.
-542	01 Matrix    		40.3%	Medium	
-1162	As Far from Land as Possible    		44.3%	Medium	

- using previous row previous col to get current element

-799	Champagne Tower    		43.8%	Medium	
-174	Dungeon Game    		32.9%	Hard	
-120	Triangle    		45.0%	Medium	
min path sum from top to bottom. (try from bottom to top)
-64	Minimum Path Sum    		55.4%	Medium	
-63	Unique Paths II    		35.0%	Medium	(with obstacles)
-62	Unique Paths    		55.1%	Medium	
this is also a math permutation of m D and n R.
-931	Minimum Falling Path Sum    		63.1%	Medium	
-1289	Minimum Falling Path Sum II    		61.8%	Hard	***
adjacent row cannot choose the same col, col does not need to be adjacent.
so with greedy approach: always choose the min or 2nd min in each row.
-1314	Matrix Block Sum    		73.6%	Medium	
sum -k to k around the cell (i,j). convolution. not a dp problem.
-1594	Maximum Non Negative Product in a Matrix    		31.9%	Medium	
similar to 1d max subarray product. need keep both min and max.

- find square or rectangle in matrix.
generally is more tricky. these problems are all ending with (i,j) problems.

-562	Longest Line of Consecutive One in Matrix    		46.1%	Medium	
horizontal, vertical, diagonal, anti-diagonal.
same: updating the 4 variables ending with (i,j).

-221	Maximal Square    		38.1%	Medium	***
dp[i,j] represent the max square size ending at (i,j).
dp[i,j]=min(dp[i-1,j-1],dp[i-1,j],dp[i,j-1]+1
-1277	Count Square Submatrices with All Ones    		73.0%	Medium	

-1292	Maximum Side Length of a Square with Sum Less than or Equal to Threshold    		49.9% ***
prefix sum, and then loop to get all submatrix sum ending with (i,j)
-750	Number Of Corner Rectangles    		66.6%	Medium	 ***
brutal force will be O(M^2N^2)
to reduce one loop. we count the two rows: number of cols with same 1, and then number of rect = c*(c-1)/2
O(N^2M). This is by changing the loop order.

-1504	Count Submatrices With All Ones    		61.5%	Medium	***
similarly we only store left 1 length, and then count each row from 0 to i with rect ending with (i,j). 

-2d but doing two 1d directions 
-764	Largest Plus Sign    		46.1%	Medium	
-1139	Largest 1-Bordered Square    		48.1%	Medium	***

- two strings or arrays.
-97	Interleaving String    		32.2%	Hard	
-1458	Max Dot Product of Two Subsequences    		42.5%	Hard	
similar to LCS.
-5. Longest Palindromic Substring
-72. Edit Distance
-583. Delete operation for two strings
-712. Minimum ASCII delete sum for two strings
-10	Regular Expression Matching    		27.1%	Hard	
-44	Wildcard Matching    		25.1%	Hard	


### longest common subsequence (LCS)

- two array or two strings and get the longest common subsequence
- get the length or all the LCS.
- longest palindrome problems can be treated as LCS between string and reversed string.
- can also be consider as a 2d matrix traversal.
- dp[i,j] represent the longest common subsequence length for s1 with length=i and s2 with length=j.
- important: develop intuition to convert to equivalent LCS.

-1143. longest common subsequence
-516	Longest Palindromic Subsequence    		54.4%	Medium	
-1035. Uncrossed lines
-583	Delete Operation for Two Strings    		49.4%	Medium	
delete from one string to another string, equiv LCS
-1216	Valid Palindrome III    		48.8%	Hard	
if we can remove at most k chars to make it palindrome. equiv LCS between s and reversed s.
-1312	Minimum Insertion Steps to Make a String Palindrome    		58.8%	Hard	
-1092	Shortest Common Supersequence     		52.5%	Hard	***
find the LCS: the string.

### longest increasing subsequence (LIS)

- generally only one array or string is involved.
- O(nlogn) approaches
- all ending with problem (thus we tried each element as candidats)

-300	Longest Increasing Subsequence    		43.0%	Medium	

-673	Number of Longest Increasing Subsequence    		38.2%	Medium	
LIS and count number of ways to reach LIS. two dp problems.

-368	Largest Divisible Subset    		38.1%	Medium	***
sort and check previous if num[i]%num[j]==0
to get the combination add a prev array to store previous position.

-1239	Maximum Length of a Concatenated String with Unique Characters    		48.7%	Medium	****
using bitset for each string so that it is convenient to check if they have same chars.
dp[i] represent the bitmask for the longest string ending with str[i].
- discard string with duplicate chars.
- grows the dp bitset (for all previous combinations) and check all previous
thus we can use backtracking and shall be easier to understand.
 
-474	Ones and Zeroes    		43.3%	Medium	
choose it dp[i,j]=max(dp[i,j],dp[i-m0,j-n1]+1)

-354	Russian Doll Envelopes    		35.8%	Hard	
2d compare.

-646	Maximum Length of Pair Chain    		52.4%	Medium	
-1048	Longest String Chain    		55.2%	Medium	
-960	Delete Columns to Make Sorted III    		54.0%	Hard	
-955	Delete Columns to Make Sorted II    		33.4%	Medium	
-944	Delete Columns to Make Sorted    		70.9%	Easy	
-1027	Longest Arithmetic Subsequence    		50.4%	Medium	
2d dp with LIS.
-1713. Min operation to make a subsequence
-1218	Longest Arithmetic Subsequence of Given Difference    		45.9%	Medium	
-1187	Make Array Strictly Increasing    		41.5%	Hard	
-1626	Best Team With No Conflicts    		36.7%	Medium	
-1671. Minimum Number of Removals to Make Mountain Array
-1691. Maximum Height by stacking cuboids

### array partitioning (connect or start a new group)

this is a very typical dp pattern. Typical pattern is to partition the array into k parts and to reach global optimal results. We try each element to attach to previous partition or start a new partion and evaluate the optimal results.

-309	Best Time to Buy and Sell Stock with Cooldown    		47.8%	Medium
as many as transactions. ->partition to any parts.
-188	Best Time to Buy and Sell Stock IV    		28.8%	Hard	
at most k transactions.
-123	Best Time to Buy and Sell Stock III    		39.3%	Hard	
at most 2 transactions.
-122	Best Time to Buy and Sell Stock II    		57.9%	Easy	
as many transaction as you want.
-121	Best Time to Buy and Sell Stock    		51.1%	Easy	
at most one transaction (this is the base for all stock problem)
-714	Best Time to Buy and Sell Stock with Transaction Fee    		55.4%	Medium	
all based on max(current - lmin)

-472	Concatenated Words    		44.9%	Hard	
dp[i] represent true or fase to be able to cut here to deocompose it into dictionary words.
-140	Word Break II    		33.7%	Hard	
-139	Word Break    		41.0%	Medium	

-131	Palindrome Partitioning    		49.2%	Medium	
backtrack to get all palindrome substr partitioning.
-132	Palindrome Partitioning II    		30.7%	Hard	
find the min num cut so that every partition is a palindrome.
try every palindrome prefix and cut solve the suffix subproblem.
-1278	Palindrome Partitioning III    		60.2%	Hard	*****
you can replace some characters in s so that we can divide s into K paldinrome substr.
base case: all prefix using k=1 get the cost matrix. dp[i][1] all given.

-53	Maximum Subarray    		47.2%	Easy	
-152	Maximum Product Subarray    		32.4%	Medium	
-918	Maximum Sum Circular Subarray    		34.0%	Medium	
-813	Largest Sum of Averages    		50.6%	Medium	
-1043	Partition Array for Maximum Sum    		66.4%	Medium	
-1105	Filling Bookcase Shelves    		57.9%	Medium	
-1335	Minimum Difficulty of a Job Schedule    		58.5%	Hard	
-1478	Allocate Mailboxes    		55.1%	Hard	


### count number of ways
The idea: to reach current element, add all previous incoming path.

-790	Domino and Tromino Tiling    		39.7%	Medium	*****
given 'I' and 'L' two types of tile. return the number of ways to tile a 2xN board.
ending with 'I' and 'L', two states interlaced.

-115	Distinct Subsequences    		39.1%	Hard	***
to reach i,j: s[i]==t[j] you can choose to use it or not! dp[i,j]=dp[i-1,j]+dp[i-1,j-1]

-518	Coin Change 2    		51.0%	Medium	****
coins can be reused. a 2d knapsack -> 1d knapsack
dp[i,j] to get amount i by using the first j coins.
dp[i,j]=dp[i,j-1]+dp[i-c,j] use or not use current coin (two paths)
to reduce j, dp[i]+=dp[i-c] the j loop shall be the outer loop.
for 2d, loop order does not matter, but for 1d it matters (optimized).

-552	Student Attendance Record II    		37.0%	Hard	
A separate case
ending with P,PL,PLL are all valid. or add the string to all previous valid string.
so dp[i]=dp[i-1]+dp[i-2]+dp[i-3].

-576	Out of Boundary Paths    		35.5%	Medium	
-935	Knight Dialer    		45.9%	Medium	
-1220	Count Vowels Permutation    		53.8%	Hard	
-91	Decode Ways    		25.5%	Medium	
-639	Decode Ways II    		27.2%	Hard	
-1269	Number of Ways to Stay in the Same Place After Some Steps    		43.1%	Hard	
-1411	Number of Ways to Paint N × 3 Grid    		60.6%	Hard	

-634	Find the Derangement of An Array    		40.3%	Medium	
-920	Number of Music Playlists    		47.4%	Hard	

-940	Distinct Subsequences II    		41.5%	Hard	
-1397	Find All Good Strings    		37.9%	Hard	
-1259	Handshakes That Don't Cross    		53.9%	Hard	

-1416	Restore The Array    		36.2%	Hard	
-1359	Count All Valid Pickup and Delivery Options    		57.1%	Hard	
-1575	Count All Possible Routes    		58.4%	Hard	
-1444	Number of Ways of Cutting a Pizza    		53.4%	Hard	
-1524	Number of Sub-arrays With Odd Sum    		38.9%	Medium	
-1301	Number of Paths with Max Score    		37.7%	Hard	
-1621	Number of Sets of K Non-Overlapping Line Segments    		41.3%	Medium	
-1569	Number of Ways to Reorder Array to Get Same BST    		50.0%	Hard	
-647. Palindromic Substrings

-1692. Count ways to distribute candies
-1155	Number of Dice Rolls With Target Sum    		47.7%	Medium	
-1223	Dice Roll Simulation    		46.8%	Medium	
-730. Count Different Palindromic Subsequences
- 1641 Count Sorted Vowel Strings 
-1639	Number of Ways to Form a Target String Given a Dictionary    		38.9%	Hard	
-629	K Inverse Pairs Array    		31.4%	Hard	

### ending with
ending with type of elements, ending with current element et al.
ending with type of elements are often used to reach unique combinations.

-1682. Longest palindromic subsequence II
-413	Arithmetic Slices    		58.3%	Medium	
-446	Arithmetic Slices II - Subsequence    		33.0%	Hard	
-795	Number of Subarrays with Bounded Maximum    		46.9%	Medium	
-467	Unique Substrings in Wraparound String    		35.9%	Medium	

### subarray or intervals
general problem: optimal take one subarray and take it away and try to get global optimal result.
approach: subproblem dp[i,j] to solve subarray from i to j. Then we try to use a k to divide it into two subproblem inside it.
Without using dp approach, this type of problem is extremely hard. Even in dp, it is among the hardest.

-546	Remove Boxes    		43.7%	Hard	
-664	Strange Printer    		41.0%	Hard	
-1000	Minimum Cost to Merge Stones    		40.4%	Hard	
-1246	Palindrome Removal    		45.7%	Hard	
-1000. Minimum Cost to Merge Stones
-312. Burst Balloons


### knapsack
choose some elements (repeated or non-repeated) to reach a target or closest to target.
each element has two options, to be included or excluded.

-494	Target Sum    		46.0%	Medium	
-805	Split Array With Same Average    		26.5%	Hard	
-879	Profitable Schemes    		39.9%	Hard	
-956	Tallest Billboard    		39.8%	Hard	
-1049	Last Stone Weight II    		44.3%	Medium	
-691	Stickers to Spell Word    		43.5%	Hard

### interlaced two states
-1186	Maximum Subarray Sum with One Deletion    		38.3%	Medium	
-801	Minimum Swaps To Make Sequences Increasing    		39.0%	Medium	
-276	Paint Fence    		38.7%	Easy	
-376	Wiggle Subsequence    		39.9%	Medium	
-600	Non-negative Integers without Consecutive Ones    		34.3%	Hard	
-964	Least Operators to Express Number    		44.5%	Hard	
-975	Odd Even Jump    		41.7%	Hard	
-978	Longest Turbulent Subarray    		46.6%	Medium	

### shortest distance problem
-245	Shortest Word Distance III    		55.6%	Medium	
-244	Shortest Word Distance II    		53.0%	Medium	
-243	Shortest Word Distance    		61.4%	Easy	
-490	The Maze    		52.4%	Medium	
-505	The Maze II    		48.1%	Medium	
-499	The Maze III    		41.8%	Hard	
-743	Network Delay Time    		45.1%	Medium	
-787	Cheapest Flights Within K Stops    		39.4%	Medium	
-847	Shortest Path Visiting All Nodes    		52.7%	Hard	
-1125	Smallest Sufficient Team    		46.9%	Hard	
-943	Find the Shortest Superstring    		43.2%	Hard	

### 
-983	Minimum Cost For Tickets    		62.5%	Medium	
-1039	Minimum Score Triangulation of Polygon    		49.9%	Medium	
-1191	K-Concatenation Maximum Sum    		25.4%	Medium	
-1235	Maximum Profit in Job Scheduling    		46.0%	Hard	
-1240	Tiling a Rectangle with the Fewest Squares    		51.3%	Hard	
-1326	Minimum Number of Taps to Open to Water a Garden    		45.7%	Hard	
-1340	Jump Game V    		58.3%	Hard	
-1363	Largest Multiple of Three    		33.7%	Hard	
-1262	Greatest Sum Divisible by Three    		48.4%	Medium	
-1388	Pizza With 3n Slices    		45.0%	Hard	
-1402	Reducing Dishes    		72.3%	Hard	
-1449	Form Largest Integer With Digits That Add up to Target    		43.1%	Hard	
-256	Paint House    		52.7%	Medium	
-265	Paint House II    		45.2%	Hard	
-1473	Paint House III    		48.7%	Hard	
-1531	String Compression II    		32.2%	Hard	
-1548	The Most Similar Path in a Graph    		55.3%	Hard	
-1425	Constrained Subsequence Sum    		44.6%	Hard	
-1696. Jump Game Vi.
-1553	Minimum Number of Days to Eat N Oranges    		28.6%	Hard	
-1611	Minimum One Bit Operations to Make Integers Zero    		56.6%	Hard	

### bitmask dp

-526	Beautiful Arrangement    		59.3%	Medium	
-1066	Campus Bikes II    		54.0%	Medium	
-1542	Find Longest Awesome Substring    		36.0%	Hard	
-1349	Maximum Students Taking Exam    		43.3%	Hard	
-1434	Number of Ways to Wear Different Hats to Each Other    		38.9%	Hard	
-1494	Parallel Courses II    		31.0%	Hard	
-1595	Minimum Cost to Connect Two Groups of Points    		41.9%	Hard	
-1659	Maximize Grid Happiness    		29.1%	Hard	
