### Introduction
This document is used for grep
each problem is tagged by <<-problem->> and each problem contains:
problem description
subject or approach
hard level

tag <<- and ->>. tags shall be different so that regex can get the start and end.

whole question: using <<-(.|[\r\n])*?->> to search problems.
add some more restrictives in the search, for example must contain greedy (ignoring case)

multiple subjects search: (?.*key1)(?.*key2) using looking-ahead

single keyword:
(?<=<<-)((?!->>).|[\r\n])?(keyw1)(.|[\r\n])?(?=->>)
<<-((?!->>).|[\r\n])?(keyw1)(.|[\r\n])?->>
two keywords:
<<-((?!->>).|[\r\n])?(keyw1|keyw2)((?!->>).|[\r\n])?(keyw1|keyw2)(.|[\r\n])?->>
demo using the search pattern:
<<-((?!->>).|[\r\n])*?(dp|greedy)((?!->>).|[\r\n])*?(dp|greedy)(.|[\r\n])*?->>

<<-0111. problem
subject: dp with greedy.
level: 1
->>
problems are classified according to the most important classification. we focused on the idea.

## algorithm focused Problems

### dp: edit distance, longest common subsequence et al.

<<-1682. Longest palindromic subsequence II
problem: palindromic subsequence needs to be even length, neighboring chars cannot be the same (except the mid pair).
dp[i,j,k] using 26 chars.
->>

<<-516	Longest Palindromic Subsequence    		54.4%	Medium	
equivalent to:
- longest common subsequence (LCS) (s and reversed s).
- min deletion edit distance (s and reversed s).
- dp[i,j] represent to max length for substr(i...j). 
s[i]==s[j] dp[i,j]=dp[i+1,j-1]+2
else dp[i,j]=max(dp[i+1,j],dp[i,j-1])
->>

<<-5. Longest Palindromic Substring
for substring, 
- dp: dp[i,j] s[i]==s[j] and inner side s[i+1..j-1] must also be a pal-string.
- non-dp: for all possible position, expand and update max size.
->>

<<-647. Palindromic Substrings
problem: count number of palindromic substring.
- non-dp: for all possible position, expand and get the count
- dp: dp[i,j] number of pal-string inside. then dp[i,j]=self+dp[i,j-1]+dp[i+1,j]-dp[i+1,j-1]
->>

<<-730. Count Different Palindromic Subsequences
number of unique palindromic subsequence.
needs unique, add one dimension, ending with char x. (only a,b,c,d are in the string).
dp[i,j,c] represent the number of unique pal-subsequence in the range [i,j].
derive the relation from [i,j-1] and [i+1,j] and [i+1,j-1]
- i>j dp[i,j,x]=0
- i==j, dp[i,j,x]=1 s[i]=x, otherwise 0.
- i<j
  s[i]!=s[j] dp[i,j,x]=dp[i,j-1,x]+dp[i+1,j,x]-dp[i+1,j-1,x]
  s[i]==s[j]=x, dp[i,j,x]=sum(dp[i+1,j-1,c])+2 //we add x and xx.
 since we need all dp[i,j,c] c from 0 to 3, the loop shall be inside.
rating: 5
->>

<<-1143. longest common subsequence
two strings.
very typical dp problem. 
- lcs
- edit distance.
there are quite a few variation of this problem.
->>

<<-1035. Uncrossed lines
find the max number of connected lines (connecting two same number in two lines)
longest common subsequence problem
->>

<<-72. Edit Distance
min operation to convert string 1 to string 2 using ins,del,replace.
dp[i,j] represent the min operations converting s1 length i to s2 with length j.
s[i]==s[j], dp[i,j]=dp[i-1,j-1]
s[i]!=s[j]: 
replace: dp[i-1,j-1]+1
insert: dp[i,j-1]+1
delete: dp[i-1,j]+1
->>

<<-583. Delete operation for two strings
min deletions to make two strings equal. You can delete either strings.
dp[i,j].
if s[i]!=t[j]: you have two options:
delete s[i]: dp[i,j-1]+1
delete t[j]: dp[i-1,j]+1
->>

<<-712. Minimum ASCII delete sum for two strings
find the lowest ascii sum of deleted chars to make two strings equal.
edit distance variation
->>

### longest increasing sequence

<<-1671. Minimum Number of Removals to Make Mountain Array
approach: check each index and get the LIS and then back from right to get the LIS.
subject: dp,longest increasing subsequence O（N^2) or O(nlogn)
level: 3
->>

<<-1691. Maximum Height by stacking cuboids
stack cuboids: width<=prev width, length<=prev width, height<=prev height
return the max height.
- each cuboid has 3 orientations
- sort the cube individually, making the height the biggest
- sort the cubes according to width (smallest)
- apply LIS dp 
->>

### strategy game

<<-1690. Stone game VII
each turn remove the left or rightmost stone, and the score is the sum of the remaining.
minimize the difference.
approach: maximize the score for a and b. applying prefix sum and get the scores easily and choose left or right and solve two smaller subproblem.
->>

### reverse thinking
<<-312. Burst Balloons
burst a balloon, and get the score: product of left and right and the balloon.
idea: choose i as the last balloon to burst and its left and right are virtual balloon with value 1. And then balloon i becomes the right and left for two subproblem.
- top down: is more straightforward.
- bottom up: loop over all left and right will not work, but we need to do one balloon, 2...until n balloons. (base case is one balloon left)
->>
 
### build from previous solution.
<<-1687. Delivering boxes from storage to ports
problem: 
- boxes are given in array with weight and destination port.
- ship limited by max number of boxes and max weight.
- order shall be the given order.
- must return to storage at the end.
- even on the ship and the deliver shall be made in order.
Intuition: dp, since you have a lot of options, and each options will affect the later options.
using some greedy approach: 
- neighboring same port order will only have one trip.
- load as many as possible boxes on ship with the restrictions
- add ith item, and pop out oldest to satisfy restrictions (greedy)
- if the popped one is the same with our oldest, keep popping (greedy)
->>

<<- 1641 Count Sorted Vowel Strings 
given length n.
- dp[i,j] represents number of sorted vowel string ending with char j. the previous char shall be <=curr char. dp[i,j]+=dp[i-1,k] k<=j.
- backtracking
->>

<<-1639	Number of Ways to Form a Target String Given a Dictionary    		38.9%	Hard	
a list of dictionary words, and a target string. You can choose a char from kth column, if you used it, then all left side cannot be used.
return number of ways to form the target.
for each column, find the hashmap (histogram)
then apply dp: dp[i,j] we used length i for target and j columns in dictionary
two options: use j column or not use it.
->>

### bitmask dp
bitmask dp: typical usage the number of elements is very limited and 2^n can fit in a integer.

<<-1659	Maximize Grid Happiness    		29.1%	Hard	
problem: given a grid, and number of introvert and number of extrovert. introvert starts with 120 and lose 30 for each neighbor and extrovert starts with 40 and gain 20 for each neighbor. find the max happiniess.
The critical part is to realize that the current choice only depends on previous n placements. 
using dp with bitmask. top down is easier.
dp state is defined by current position, intro used, extro used, previous n cells intromask, previous n cells extromask. 5d dp.
level: 5
->>



### greedy
<<-1686. Stone game IV
problem: stone are valued differently, A will take first. Who will win.
greedy approach: suppose we have two stones (a,b),(c,d).
if A take a, then B take d. difference a-d
if A take c, then B take b. difference c-b
which A shall take? we shall maximize difference a-d>c-b->a+b>c+d.
so we sort by a+b.
->>


<<-1647. Minimum Deletions to Make Character Frequencies Unique
get the occurrence for each char and count each number's frequency in map.
then choose the largest and delete a char to make it single.
->>

<<-1675. Minimize Deviation in Array
odd number can *2, even number can /2. return the min value of max-min.
since odd can change to even only once, we can change all to even, and then only divide is allowed.
Greedy: reduce the max even by half and evaluate the difference. Since we need both the max and the min, sorting or set is more suitable than priority_queue.
if the max is odd, we stop. You cannot make it smaller.
subject: greedy, heap.
level: 4
->>

<<-1673. Find the most competitive subsequence
problem: find the min subsequence with length = k.
approach: greedy approach: use the first n-k minimum for the first element and then discard the elements till the min (including). using priority_queue O(nlogn) or monotonic deque O(n) or monotonic stack. deque is easier.
subject: deque or stack, monotonic
level: 4
->>

<<-1665	Minimum Initial Energy to finish tasks
problem: each task with required and minimam energy. find the total minimum energy
approach: greedy, sort using min-req and then add all. (we complete the more saved energy task first)
level: 4
->>

<<-1663	Smallest string with a given numeric value
problem: a-z represent 1-26, the sum of all chars is the numeric value. given k (value) and n (length), return the smallest string
approach: greedy, based on all 'a' and replace from right as much as 'z'to 'a'
level: 3
->>

### binary search
<<-1631. Path with minimum effort
given a height grid. from top left to bottom right. find a route with min effort.
effort for a route is the max absolute difference between two consecutive steps.
- binary search + bfs/dfs/union-find..
- dijkstra (greedy + heap)
->>

<<-1648	Sell Diminishing-Valued Colored Balls    		30.4%	Medium	
sell prices=number of balls in same color.
so greedy approach we shall sell the highest volume ball first.
binary search the ball volume until we flatten all high volume ball.
->>
<<-1674. Min moves to make array complimentary
problem: A[i]+A[n-1-i] are all the same. using [1,limit] to replace any number, and find the number of replace. (note all elements <=limit and >=1, this is a critical information), very hard question!
Analysis: from brutal force (we need do each pair for all the targets, and then we can use optimization using difference array to reduce O(n) to O(1) (the curve is always 2s,1s,0s,1s,2s, using difference we only need to record 5 changing point). also binary search can also do it with less memory requirement.
subject: difference array and prefix sum.
level: 5
->>

### backtracking
<<-131. Palindrome Partitioning
return all poosible palindrome partitioning.
typical backtracking
->>

<<-1655	Distribute Repeating Integers    		40.0%	Hard	
problem: given an array of integer and m customer orders, each order is number of integers. Each order shall give the same integer.
check if it is possible to fulfill the order.
important: try order from largest to smallest. (sort the array is useless since it keeps changing). If the highest order is not able fulfill, then it is done (prune)
->>

<<-1681. Minimum Incompatibility
problem: divide into k sets, each set cannot contain duplicate number, incompatibility=max-min for each set. find the min sum of incompatibility.
- backtracking: but it will find all permutations. using set to avoid revisit.
a very tricky backtracking problem.
also a bitmask dp problem.
->>

### dfs

### bfs
<<-1654	Minimum Jumps to Reach Home    		28.0%	Medium	
a line with forbidden position. given a and b. you can jump forward by a, or jump backward with b. You cannot jump backward twice in a row. cannot jump to forbidden position.
given position x, return the min number of jumps to home (at position 0)
bfs: with two status. either from right or from left.
need store position and direction also, the visited array.
->>
### union-find
<<-1632. Rank transform of a matrix
mxn matrix. rank matrix: smallest element in its row and column shall be 1. smaller value smaller rank. rank shall be as small as possible.
greedy approach: 
- start from smallest element, there could be multiples.
- 

## data structure focused problems

### stack

### queue & deque

### heap: priority-queue, set, map
<<-162. furthest building you can reach
problem: using ladder and stones to climb ladders.
approach:
- binary search
- heap: try using stones or ladders first, when one is used up, replace the max stone for a ladder or a ladder for min stones.
->>

### hashset, hashmap

### tree

<<-1660	Correct a Binary Tree    83.2%	Medium	
a node's right point to a node in its right on the same level
approach: dfs with hashmap.
level: 3
->>
<<-235. Lowest Common Ancestor of a Binary Search Tree
use the BST property and determine which branch to go.
->>

<<-236. Lowest Common Ancestor of a Binary Tree
the typical dfs approach.
->>

<<-1644	Lowest Common Ancestor of a Binary Tree II    		59.7%	Medium	
p or q may not exist in the tree.
while searching, return node and return bool.
->>
<<-1650. Lowest Common Ancestor of a Binary Tree III
Problem: node has left,right,parent. The tree is not given.
- approach 1: we can go up to find the root first and then is the same with lca.
- approach 2: go up from p and q, find the first common node.
- approach 3: equivalent to find the intersection node of two linked list. (brilliant)
->>

<<-1676. Lowest Common Ancestor of a Binary Tree IV
now given a list of nodes, find their LCA
approach 1: using 2 node's lca and reduce by half. O(mnlogm)
approach 2: traversal and mark each found nodes using postorder traversal.
it is better to use two states: the answer and the counting. (Note the dfs or postorder, we need assign the answer only the first time).
->>

<<-1666 Change the root of a binary tree
problem: node has left,right,parent. go up from leaf, if cur has a left, it becomes right child. Its parent becomes left child.
subject: binary tree, very confusing.
level: 4
->>

### segment tree or binary index tree

<<-1649	Create Sorted Array through Instructions    		42.0%	Hard	
segment tree 
->>

### linked list
<<-1669 Merge in between linked list
problem: remove a range of nodes from list1 and replace with list2
subject: linked list
approach: get the head and tail of list2 and get the start and end node of list1 and connect
level: 2
->>
<<-1634. Add Two Polynomials Represented as Linked Lists
node has coefficient and power.
two pointer merge.
->>

### array & string
<<-1685. Sum of absolute differences in a sorted array
B[i]=sum(|Aj-Ai|)
since the array is sorted. B[i]=sum(Ai-Aj)+sum(Ak-Ai) j=[0,i],k=[i,n-1]
prefix sum on the difference and simple math
->>
 
<<-1630. Arithmetic subarrays
given an array, and left and right boundary. Determine if the subarray can be rearranged into arithmetic sequence.
brutal force->subarray sort.
->>

<<-1637. Widest Vertical Area Between Two Points Containing No Points
sort by x and then get the max difference
->>
<<-1636. Sort Array by Increasing Frequency
sort array using lambda function with hashmap
->>

<<-1638. Count Substrings That Differ by One Character
- brutal force: O(N^4), find all combinations
- (i,j) and tries all different length. O(N^3), if more than 1 miss, we can stop
- dp: for (i,j) count the number of same char on left and right. O(N^2)
->>

<<-43. Kth Smallest Instructions
grid, from (0,0) to destination using H and V. find the kth instruction.
approach: (m+n)! combinations. each time reduce k.
this is deterministic.
->>
<<-1652. Defuse the bomb.
circular array: k>0 replace the ith element with the sum of the next k numbers
k==0, replace the ith element with 0
k<0, replace the ith element with the sum of previous k number.
approach: circular array using % using sliding window.
->>

<<-1653	Minimum Deletions to Make String Balanced    		46.5%	Medium	
string with only 'a' and 'b'. balanced if sorted. return the min number of deletion.
- longest increasing subsequence using O(N^2) or O(nlogn)
- count left remove b and right remove a and get the min (array)
similar to 01 and we can count the total and left b.
->>

<<-1664	ways to make a fair array
problem: remove an element and make odd index sum =even index sum and get number of indices.
approach: array using two directions, prefix and postfix sum.
level: 3
->>

<<-1662	Check if two string arrays are equivalent
problem: concat and check if correct
approach: brutal force, or using two pointer O(1) two pointer for the word counter, two pointer for the char counter in each word, which word is finished first, increase the word counter.
level: 2
->>

<<-1658	Minimum Operations to Reduce X to Zero    		29.3%	Medium	
problem: remove either end and subtract it from x. return the min number of operation.
- no need dp.
- at the last it will remove some elements from both ends. So it is equivalent to find the longest window sum=tsum-x.
this can be done using sliding window or hashmap.
->>

<<-1657	Determine if Two Strings Are Close    		50.0%	Medium	
op1: swap any two chars.
op2: transform all char a to b and all char b to a.
check if they are close.
equivalent: sort the hashmap and check if they are equal.
->>


<<-1647	Minimum Deletions to Make Character Frequencies Unique    		53.0%	Medium	->>
<<-1646	Get Maximum in Generated Array    		48.2%	Easy	->>

<<-1643	Kth Smallest Instructions    		41.7%	Hard	->>
<<-1642	Furthest Building You Can Reach    		54.7%	Medium	->>
<<-1641	Count Sorted Vowel Strings    		76.9%	Medium	->>
<<-1640	Check Array Formation Through Concatenation    		77.3%	Easy	->>

<<-1638	Count Substrings That Differ by One Character    		68.5%	Medium	->>
<<-1637	Widest Vertical Area Between Two Points Containing No Points    		85.3%	Medium	->>
<<-1636	Sort Array by Increasing Frequency    		66.4%	Easy	->>
<<-1634	Add Two Polynomials Represented as Linked Lists    		58.0%	Medium	->>
<<-1632	Rank Transform of a Matrix    		29.3%	Hard	->>
<<-1631	Path With Minimum Effort    		40.1%	Medium	->>
<<-1630	Arithmetic Subarrays    		78.8%	Medium	->>
<<-1629	Slowest Key    		60.7%	Easy	->>
<<-1628	Design an Expression Tree With Evaluate Function    		86.0%	Medium	->>
<<-1627	Graph Connectivity With Threshold    		36.9%	Hard	->>
<<-1626	Best Team With No Conflicts    		36.7%	Medium	->>
<<-1625	Lexicographically Smallest String After Applying Operations    		62.6%	Medium	->>
<<-1624	Largest Substring Between Two Equal Characters    		59.4%	Easy	->>
<<-1622	Fancy Sequence    		15.8%	Hard	->>
<<-1621	Number of Sets of K Non-Overlapping Line Segments    		41.3%	Medium	->>
<<-1620	Coordinate With Maximum Network Quality    		37.7%	Medium	->>
<<-1619	Mean of Array After Removing Some Elements    		66.3%	Easy	->>
<<-1618	Maximum Font to Fit a Sentence in a Screen    		57.5%	Medium	->>
<<-1617	Count Subtrees With Max Distance Between Cities    		63.0%	Hard	->>
<<-1616	Split Two Strings to Make Palindrome    		36.5%	Medium	->>
<<-1615	Maximal Network Rank    		51.3%	Medium	->>
<<-1614	Maximum Nesting Depth of the Parentheses    		84.4%	Easy	->>
<<-1612	Check If Two Expression Trees are Equivalent    		71.0%	Medium	->>
<<-1611	Minimum One Bit Operations to Make Integers Zero    		56.6%	Hard	->>
<<-1610	Maximum Number of Visible Points    		27.3%	Hard	->>
<<-1609	Even Odd Tree    		54.0%	Medium	->>
<<-1608	Special Array With X Elements Greater Than or Equal X    		62.4%	Easy	->>
<<-1606	Find Servers That Handled Most Number of Requests    		36.0%	Hard	->>
<<-1605	Find Valid Matrix Given Row and Column Sums    		76.9%	Medium	->>
<<-1604	Alert Using Same Key-Card Three or More Times in a One Hour Period    		41.4%	Medium	->>
<<-1603	Design Parking System    		86.1%	Easy	->>
<<-1602	Find Nearest Right Node in Binary Tree    		75.0%	Medium	->>
<<-1601	Maximum Number of Achievable Transfer Requests    		47.2%	Hard	->>
<<-1600	Throne Inheritance    		58.8%	Medium	->>
<<-1599	Maximum Profit of Operating a Centennial Wheel    		43.2%	Medium	->>
<<-1598	Crawler Log Folder    		64.4%	Easy	->>
<<-1597	Build Binary Expression Tree From Infix Expression    		63.6%	Hard	->>
<<-1595	Minimum Cost to Connect Two Groups of Points    		41.9%	Hard	->>
<<-1594	Maximum Non Negative Product in a Matrix    		31.9%	Medium	->>
<<-1593	Split a String Into the Max Number of Unique Substrings    		46.7%	Medium	->>
<<-1592	Rearrange Spaces Between Words    		43.8%	Easy	->>
<<-1591	Strange Printer II    		55.1%	Hard	->>
<<-1590	Make Sum Divisible by P    		27.2%	Medium	->>
<<-1589	Maximum Sum Obtained of Any Permutation    		34.5%	Medium	->>
<<-1588	Sum of All Odd Length Subarrays    		81.1%	Easy	->>
<<-1586	Binary Search Tree Iterator II    		65.6%	Medium	
this we need add prev and hasPrev.
- inorder traversal and put into vector, trivial
- iterative: only maintain the visited nodes using stack.
->>
<<-1585	Check If String Is Transformable With Substring Sort Operations    		48.0%	Hard	->>
<<-1584	Min Cost to Connect All Points    		49.0%	Medium	->>
<<-1583	Count Unhappy Friends    		52.6%	Medium	->>
<<-1582	Special Positions in a Binary Matrix    		64.3%	Easy	->>
<<-1580	Put Boxes Into the Warehouse II    		63.0%	Medium	->>
<<-1579	Remove Max Number of Edges to Keep Graph Fully Traversable    		45.5%	Hard	->>
<<-1578	Minimum Deletion Cost to Avoid Repeating Letters    		60.1%	Medium	->>
<<-1577	Number of Ways Where Square of Number Is Equal to Product of Two Numbers    		37.0%	Medium	->>
<<-1576	Replace All ?'s to Avoid Consecutive Repeating Characters    		48.0%	Easy	->>
<<-1575	Count All Possible Routes    		58.4%	Hard	->>
<<-1574	Shortest Subarray to be Removed to Make Array Sorted    		31.9%	Medium	->>
<<-1573	Number of Ways to Split a String    		30.4%	Medium	->>
<<-1572	Matrix Diagonal Sum    		78.3%	Easy	->>
<<-1570	Dot Product of Two Sparse Vectors    		91.5%	Medium	->>
<<-1569	Number of Ways to Reorder Array to Get Same BST    		50.0%	Hard	->>
<<-1568	Minimum Number of Days to Disconnect Island    		51.1%	Hard	->>
<<-1567	Maximum Length of Subarray With Positive Product    		36.0%	Medium	->>
<<-1566	Detect Pattern of Length M Repeated K or More Times    		42.0%	Easy	->>
<<-1564	Put Boxes Into the Warehouse I    		66.2%	Medium	->>
<<-1563	Stone Game V    		40.2%	Hard	->>
<<-1562	Find Latest Group of Size M    		39.0%	Medium	->>
<<-1561	Maximum Number of Coins You Can Get    		78.9%	Medium	->>
<<-1560	Most Visited Sector in a Circular Track    		56.9%	Easy	->>
<<-1559	Detect Cycles in 2D Grid    		44.8%	Hard	->>
<<-1558	Minimum Numbers of Function Calls to Make Target Array    		62.4%	Medium	->>
<<-1557	Minimum Number of Vertices to Reach All Nodes    		74.3%	Medium	->>
<<-1556	Thousand Separator    		58.8%	Easy	->>
<<-1554	Strings Differ by One Character    		63.1%	Medium	->>
<<-1553	Minimum Number of Days to Eat N Oranges    		28.6%	Hard	->>
<<-1552	Magnetic Force Between Two Balls    		48.2%	Medium	->>
<<-1551	Minimum Operations to Make Array Equal    		77.8%	Medium	->>
<<-1550	Three Consecutive Odds    		65.9%	Easy	->>
<<-1548	The Most Similar Path in a Graph    		55.3%	Hard	->>
<<-1547	Minimum Cost to Cut a Stick    		51.2%	Hard	->>
<<-1546	Maximum Number of Non-Overlapping Subarrays With Sum Equals Target    		43.5%	Medium	->>
<<-1545	Find Kth Bit in Nth Binary String    		57.0%	Medium	->>
<<-1544	Make The String Great    		54.8%	Easy	->>
<<-1542	Find Longest Awesome Substring    		36.0%	Hard	->>
<<-1541	Minimum Insertions to Balance a Parentheses String    		41.9%	Medium	->>
<<-1540	Can Convert String in K Moves    		29.6%	Medium	->>
<<-1539	Kth Missing Positive Number    		53.4%	Easy	->>
<<-1538	Guess the Majority in a Hidden Array    		61.3%	Medium	->>
<<-1537	Get the Maximum Score    		36.1%	Hard	->>
<<-1536	Minimum Swaps to Arrange a Binary Grid    		42.8%	Medium	->>
<<-1535	Find the Winner of an Array Game    		46.9%	Medium	->>
<<-1534	Count Good Triplets    		79.9%	Easy	->>
<<-1533	Find the Index of the Large Integer    		55.1%	Medium	->>
<<-1531	String Compression II    		32.2%	Hard	->>
<<-1530	Number of Good Leaf Nodes Pairs    		55.2%	Medium	->>
<<-1529	Bulb Switcher IV    		70.7%	Medium	->>
<<-1528	Shuffle String    		85.8%	Easy	->>
<<-1526	Minimum Number of Increments on Subarrays to Form a Target Array    		59.3%	Hard	->>
<<-1525	Number of Good Ways to Split a String    		67.2%	Medium	->>
<<-1524	Number of Sub-arrays With Odd Sum    		38.9%	Medium	->>
<<-1523	Count Odd Numbers in an Interval Range    		55.3%	Easy	->>
<<-1522	Diameter of N-Ary Tree    		68.5%	Medium	->>
<<-1521	Find a Value of a Mysterious Function Closest to Target    		43.7%	Hard	->>
<<-1520	Maximum Number of Non-Overlapping Substrings    		34.8%	Hard	->>
<<-1519	Number of Nodes in the Sub-Tree With the Same Label    		36.2%	Medium	->>
<<-1518	Water Bottles    		61.3%	Easy	->>
<<-1516	Move Sub-Tree of N-Ary Tree    		62.1%	Hard	->>
<<-1515	Best Position for a Service Centre    		36.5%	Hard	->>
<<-1514	Path with Maximum Probability    		38.5%	Medium	->>
<<-1513	Number of Substrings With Only 1s    		40.9%	Medium	->>
<<-1512	Number of Good Pairs    		88.0%	Easy	->>
<<-1510	Stone Game IV    		58.5%	Hard	->>
<<-1509	Minimum Difference Between Largest and Smallest Value in Three Moves    		51.6%	Medium	->>
<<-1508	Range Sum of Sorted Subarray Sums    		63.0%	Medium	->>
<<-1507	Reformat Date    		60.1%	Easy	->>
<<-1506	Find Root of N-Ary Tree    		80.5%	Medium	->>
<<-1505	Minimum Possible Integer After at Most K Adjacent Swaps On Digits    		36.1%	Hard	->>
<<-1504	Count Submatrices With All Ones    		61.5%	Medium	->>
<<-1503	Last Moment Before All Ants Fall Out of a Plank    		52.6%	Medium	->>
<<-1502	Can Make Arithmetic Progression From Sequence    		71.2%	Easy	->>
<<-1500	Design a File Sharing System    		45.1%	Medium	->>
<<-1499	Max Value of Equation    		44.9%	Hard	->>
<<-1498	Number of Subsequences That Satisfy the Given Sum Condition    		37.9%	Medium	->>
<<-1497	Check If Array Pairs Are Divisible by k    		40.3%	Medium	->>
<<-1496	Path Crossing    		55.7%	Easy	->>
<<-1494	Parallel Courses II    		31.0%	Hard	->>
<<-1493	Longest Subarray of 1's After Deleting One Element    		58.6%	Medium	->>
<<-1492	The kth Factor of n    		65.9%	Medium	->>
<<-1491	Average Salary Excluding the Minimum and Maximum Salary    		68.9%	Easy	->>
<<-1490	Clone N-ary Tree    		83.7%	Medium	->>
<<-1489	Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree    		52.2%	Hard	->>
<<-1488	Avoid Flood in The City    		24.9%	Medium	->>
<<-1487	Making File Names Unique    		30.0%	Medium	->>
<<-1486	XOR Operation in an Array    		84.0%	Easy	->>
<<-1485	Clone Binary Tree With Random Pointer    		80.1%	Medium	->>
<<-1483	Kth Ancestor of a Tree Node    		29.5%	Hard	->>
<<-1482	Minimum Number of Days to Make m Bouquets    		48.8%	Medium	->>
<<-1481	Least Number of Unique Integers after K Removals    		55.2%	Medium	->>
<<-1480	Running Sum of 1d Array    		89.7%	Easy	->>
<<-1478	Allocate Mailboxes    		55.1%	Hard	->>
<<-1477	Find Two Non-overlapping Sub-arrays Each With Target Sum    		33.4%	Medium	->>
<<-1476	Subrectangle Queries    		89.0%	Medium	->>
<<-1475	Final Prices With a Special Discount in a Shop    		75.0%	Easy	->>
<<-1474	Delete N Nodes After M Nodes of a Linked List    		74.4%	Easy	->>
<<-1473	Paint House III    		48.7%	Hard	->>
<<-1472	Design Browser History    		68.8%	Medium	->>
<<-1471	The k Strongest Values in an Array    		58.3%	Medium	->>
<<-1470	Shuffle the Array    		88.5%	Easy	->>
<<-1469	Find All The Lonely Nodes    		80.8%	Easy	->>
<<-1467	Probability of a Two Boxes Having The Same Number of Distinct Balls    		61.3%	Hard	->>
<<-1466	Reorder Routes to Make All Paths Lead to the City Zero    		61.5%	Medium	->>
<<-1465	Maximum Area of a Piece of Cake After Horizontal and Vertical Cuts    		31.3%	Medium	->>
<<-1464	Maximum Product of Two Elements in an Array    		77.0%	Easy	->>
<<-1463	Cherry Pickup II    		66.2%	Hard	->>
<<-1462	Course Schedule IV    		43.9%	Medium	->>
<<-1461	Check If a String Contains All Binary Codes of Size K    		46.3%	Medium	->>
<<-1460	Make Two Arrays Equal by Reversing Sub-arrays    		72.5%	Easy	->>
<<-1458	Max Dot Product of Two Subsequences    		42.5%	Hard	->>
<<-1457	Pseudo-Palindromic Paths in a Binary Tree    		67.9%	Medium	->>
<<-1456	Maximum Number of Vowels in a Substring of Given Length    		53.5%	Medium	->>
<<-1455	Check If a Word Occurs As a Prefix of Any Word in a Sentence    		65.0%	Easy	->>
<<-1453	Maximum Number of Darts Inside of a Circular Dartboard    		34.7%	Hard	->>
<<-1452	People Whose List of Favorite Companies Is Not a Subset of Another List    		54.3%	Medium	->>
<<-1451	Rearrange Words in a Sentence    		58.4%	Medium	->>
<<-1450	Number of Students Doing Homework at a Given Time    		77.1%	Easy	->>
<<-1449	Form Largest Integer With Digits That Add up to Target    		43.1%	Hard	->>
<<-1448	Count Good Nodes in Binary Tree    		70.2%	Medium	->>
<<-1447	Simplified Fractions    		61.6%	Medium	->>
<<-1446	Consecutive Characters    		61.8%	Easy	->>
<<-1444	Number of Ways of Cutting a Pizza    		53.4%	Hard	->>
<<-1443	Minimum Time to Collect All Apples in a Tree    		54.6%	Medium	->>
<<-1442	Count Triplets That Can Form Two Arrays of Equal XOR    		70.5%	Medium	->>
<<-1441	Build an Array With Stack Operations    		69.3%	Easy	->>
<<-1439	Find the Kth Smallest Sum of a Matrix With Sorted Rows    		60.0%	Hard	->>
<<-1438	Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit    		43.5%	Medium	->>
<<-1437	Check If All 1's Are at Least Length K Places Away    		61.9%	Medium	->>
<<-1436	Destination City    		77.2%	Easy	->>
<<-1434	Number of Ways to Wear Different Hats to Each Other    		38.9%	Hard	->>
<<-1433	Check If a String Can Break Another String    		66.9%	Medium	->>
<<-1432	Max Difference You Can Get From Changing an Integer    		42.9%	Medium	->>
<<-1431	Kids With the Greatest Number of Candies    		88.6%	Easy	->>
<<-1430	Check If a String Is a Valid Sequence from Root to Leaves Path in a Binary Tree    		45.0%	Medium	->>
<<-1429	First Unique Number    		48.4%	Medium	->>
<<-1428	Leftmost Column with at Least a One    		48.2%	Medium	->>
<<-1427	Perform String Shifts    		53.3%	Easy	->>
<<-1426	Counting Elements    		58.9%	Easy	->>
<<-1425	Constrained Subsequence Sum    		44.6%	Hard	->>
<<-1424	Diagonal Traverse II    		44.6%	Medium	->>
<<-1423	Maximum Points You Can Obtain from Cards    		45.1%	Medium	->>
<<-1422	Maximum Score After Splitting a String    		55.9%	Easy	->>
<<-1420	Build Array Where You Can Find The Maximum Exactly K Comparisons    		64.4%	Hard	->>
<<-1419	Minimum Number of Frogs Croaking    		46.9%	Medium	->>
<<-1418	Display Table of Food Orders in a Restaurant    		67.6%	Medium	->>
<<-1417	Reformat The String    		55.6%	Easy	->>
<<-1416	Restore The Array    		36.2%	Hard	->>
<<-1415	The k-th Lexicographical String of All Happy Strings of Length n    		69.9%	Medium	->>
<<-1414	Find the Minimum Number of Fibonacci Numbers Whose Sum Is K    		63.5%	Medium	->>
<<-1413	Minimum Value to Get Positive Step by Step Sum    		65.1%	Easy	->>
<<-1411	Number of Ways to Paint N × 3 Grid    		60.6%	Hard	->>
<<-1410	HTML Entity Parser    		54.3%	Medium	->>
<<-1409	Queries on a Permutation With Key    		81.2%	Medium	->>
<<-1408	String Matching in an Array    		62.6%	Easy	->>
<<-1406	Stone Game III    		56.8%	Hard	->>
<<-1405	Longest Happy String    		51.7%	Medium	->>
<<-1404	Number of Steps to Reduce a Number in Binary Representation to One    		50.5%	Medium	->>
<<-1403	Minimum Subsequence in Non-Increasing Order    		71.3%	Easy	->>
<<-1402	Reducing Dishes    		72.3%	Hard	->>
<<-1401	Circle and Rectangle Overlapping    		42.3%	Medium	->>
<<-1400	Construct K Palindrome Strings    		62.5%	Medium	->>
<<-1399	Count Largest Group    		65.2%	Easy	->>
<<-1397	Find All Good Strings    		37.9%	Hard	->>
<<-1396	Design Underground System    		67.5%	Medium	->>
<<-1395	Count Number of Teams    		82.1%	Medium	->>
<<-1394	Find Lucky Integer in an Array    		63.2%	Easy	->>
<<-1392	Longest Happy Prefix    		41.1%	Hard	->>
<<-1391	Check if There is a Valid Path in a Grid    		45.0%	Medium	->>
<<-1390	Four Divisors    		38.8%	Medium	->>
<<-1389	Create Target Array in the Given Order    		84.4%	Easy	->>
<<-1388	Pizza With 3n Slices    		45.0%	Hard	->>
<<-1387	Sort Integers by The Power Value    		70.6%	Medium	->>
<<-1386	Cinema Seat Allocation    		35.2%	Medium	->>
<<-1385	Find the Distance Value Between Two Arrays    		66.6%	Easy	->>
<<-1383	Maximum Performance of a Team    		34.0%	Hard	->>
<<-1382	Balance a Binary Search Tree    		75.7%	Medium	->>
<<-1381	Design a Stack With Increment Operation    		75.9%	Medium	->>
<<-1380	Lucky Numbers in a Matrix    		71.1%	Easy	->>
<<-1379	Find a Corresponding Node of a Binary Tree in a Clone of That Tree    		84.0%	Medium	->>
<<-1377	Frog Position After T Seconds    		34.2%	Hard	->>
<<-1376	Time Needed to Inform All Employees    		56.2%	Medium	->>
<<-1375	Bulb Switcher III    		63.9%	Medium	->>
<<-1374	Generate a String With Characters That Have Odd Counts    		76.1%	Easy	->>
<<-1373	Maximum Sum BST in Binary Tree    		38.1%	Hard	->>
<<-1372	Longest ZigZag Path in a Binary Tree    		54.4%	Medium	->>
<<-1371	Find the Longest Substring Containing Vowels in Even Counts    		61.4%	Medium	->>
<<-1370	Increasing Decreasing String    		76.2%	Easy	->>
<<-1368	Minimum Cost to Make at Least One Valid Path in a Grid    		55.8%	Hard	->>
<<-1367	Linked List in Binary Tree    		41.1%	Medium	->>
<<-1366	Rank Teams by Votes    		54.5%	Medium	->>
<<-1365	How Many Numbers Are Smaller Than the Current Number    		85.8%	Easy	->>
<<-1363	Largest Multiple of Three    		33.7%	Hard	->>
<<-1362	Closest Divisors    		57.3%	Medium	->>
<<-1361	Validate Binary Tree Nodes    		44.9%	Medium	->>
<<-1360	Number of Days Between Two Dates    		47.4%	Easy	->>
<<-1359	Count All Valid Pickup and Delivery Options    		57.1%	Hard	->>
<<-1358	Number of Substrings Containing All Three Characters    		60.0%	Medium	->>
<<-1357	Apply Discount Every n Orders    		66.3%	Medium	->>
<<-1356	Sort Integers by The Number of 1 Bits    		69.4%	Easy	->>
<<-1354	Construct Target Array With Multiple Sums    		31.3%	Hard	->>
<<-1353	Maximum Number of Events That Can Be Attended    		29.7%	Medium	->>
<<-1352	Product of the Last K Numbers    		42.6%	Medium	->>
<<-1351	Count Negative Numbers in a Sorted Matrix    		76.0%	Easy	->>
<<-1349	Maximum Students Taking Exam    		43.3%	Hard	->>
<<-1348	Tweet Counts Per Frequency    		31.5%	Medium	->>
<<-1347	Minimum Number of Steps to Make Two Strings Anagram    		75.3%	Medium	->>
<<-1346	Check If N and Its Double Exist    		36.7%	Easy	->>
<<-1345	Jump Game IV    		40.1%	Hard	->>
<<-1344	Angle Between Hands of a Clock    		61.3%	Medium	->>
<<-1343	Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold    		64.3%	Medium	->>
<<-1342	Number of Steps to Reduce a Number to Zero    		85.9%	Easy	->>
<<-1340	Jump Game V    		58.3%	Hard	->>
<<-1339	Maximum Product of Splitted Binary Tree    		37.4%	Medium	->>
<<-1338	Reduce Array Size to The Half    		66.8%	Medium	->>
<<-1337	The K Weakest Rows in a Matrix    		69.5%	Easy	->>
<<-1335	Minimum Difficulty of a Job Schedule    		58.5%	Hard	->>
<<-1334	Find the City With the Smallest Number of Neighbors at a Threshold Distance    		46.0%	Medium	->>
<<-1333	Filter Restaurants by Vegan-Friendly, Price and Distance    		56.9%	Medium	->>
<<-1332	Remove Palindromic Subsequences    		62.7%	Easy	->>
<<-1331	Rank Transform of an Array    		57.9%	Easy	->>
<<-1330	Reverse Subarray To Maximize Array Value    		35.9%	Hard	->>
<<-1329	Sort the Matrix Diagonally    		79.3%	Medium	->>
<<-1328	Break a Palindrome    		45.1%	Medium	->>
<<-1326	Minimum Number of Taps to Open to Water a Garden    		45.7%	Hard	->>
<<-1325	Delete Leaves With a Given Value    		73.4%	Medium	->>
<<-1324	Print Words Vertically    		58.7%	Medium	->>
<<-1323	Maximum 69 Number    		77.9%	Easy	->>
<<-1320	Minimum Distance to Type a Word Using Two Fingers    		62.6%	Hard	->>
<<-1319	Number of Operations to Make Network Connected    		54.6%	Medium	->>
<<-1318	Minimum Flips to Make a OR b Equal to c    		63.5%	Medium	->>
<<-1317	Convert Integer to the Sum of Two No-Zero Integers    		56.8%	Easy	->>
<<-1316	Distinct Echo Substrings    		49.4%	Hard	->>
<<-1315	Sum of Nodes with Even-Valued Grandparent    		83.8%	Medium	->>
<<-1314	Matrix Block Sum    		73.6%	Medium	->>
<<-1313	Decompress Run-Length Encoded List    		85.2%	Easy	->>
<<-1312	Minimum Insertion Steps to Make a String Palindrome    		58.8%	Hard	->>
<<-1311	Get Watched Videos by Your Friends    		43.8%	Medium	->>
<<-1310	XOR Queries of a Subarray    		68.9%	Medium	->>
<<-1309	Decrypt String from Alphabet to Integer Mapping    		77.3%	Easy	->>
<<-1307	Verbal Arithmetic Puzzle    		37.6%	Hard	->>
<<-1306	Jump Game III    		60.5%	Medium	->>
<<-1305	All Elements in Two Binary Search Trees    		77.7%	Medium	->>
<<-1304	Find N Unique Integers Sum up to Zero    		76.4%	Easy	->>
<<-1302	Deepest Leaves Sum    		83.9%	Medium	->>
<<-1301	Number of Paths with Max Score    		37.7%	Hard	->>
<<-1300	Sum of Mutated Array Closest to Target    		43.5%	Medium	->>
<<-1299	Replace Elements with Greatest Element on Right Side    		74.6%	Easy	->>
<<-1298	Maximum Candies You Can Get from Boxes    		59.5%	Hard	->>
<<-1297	Maximum Number of Occurrences of a Substring    		48.9%	Medium	->>
<<-1296	Divide Array in Sets of K Consecutive Numbers    		54.9%	Medium	->>
<<-1295	Find Numbers with Even Number of Digits    		80.0%	Easy	->>
<<-1293	Shortest Path in a Grid with Obstacles Elimination    		42.8%	Hard	->>
<<-1292	Maximum Side Length of a Square with Sum Less than or Equal to Threshold    		49.9%	Medium	->>
<<-1291	Sequential Digits    		57.4%	Medium	->>
<<-1290	Convert Binary Number in a Linked List to Integer    		81.9%	Easy	->>
<<-1289	Minimum Falling Path Sum II    		61.8%	Hard	->>
<<-1288	Remove Covered Intervals    		57.1%	Medium	->>
<<-1287	Element Appearing More Than 25% In Sorted Array    		60.2%	Easy	->>
<<-1286	Iterator for Combination    		70.7%	Medium	->>
<<-1284	Minimum Number of Flips to Convert Binary Matrix to Zero Matrix    		69.8%	Hard	->>
<<-1283	Find the Smallest Divisor Given a Threshold    		48.9%	Medium	->>
<<-1282	Group the People Given the Group Size They Belong To    		84.2%	Medium	->>
<<-1281	Subtract the Product and Sum of Digits of an Integer    		85.5%	Easy	->>
<<-1278	Palindrome Partitioning III    		60.2%	Hard	->>
<<-1277	Count Square Submatrices with All Ones    		73.0%	Medium	->>
<<-1276	Number of Burgers with No Waste of Ingredients    		50.0%	Medium	->>
<<-1275	Find Winner on a Tic Tac Toe Game    		52.8%	Easy	->>
<<-1274	Number of Ships in a Rectangle    		65.6%	Hard	->>
<<-1273	Delete Tree Nodes    		63.3%	Medium	->>
<<-1272	Remove Interval    		57.8%	Medium	->>
<<-1271	Hexspeak    		54.9%	Easy	->>
<<-1269	Number of Ways to Stay in the Same Place After Some Steps    		43.1%	Hard	->>
<<-1268	Search Suggestions System    		64.4%	Medium	->>
<<-1267	Count Servers that Communicate    		57.6%	Medium	->>
<<-1266	Minimum Time Visiting All Points    		79.2%	Easy	->>
<<-1265	Print Immutable Linked List in Reverse    		94.5%	Medium	->>
<<-1263	Minimum Moves to Move a Box to Their Target Location    		42.4%	Hard	->>
<<-1262	Greatest Sum Divisible by Three    		48.4%	Medium	->>
<<-1261	Find Elements in a Contaminated Binary Tree    		74.3%	Medium	->>
<<-1260	Shift 2D Grid    		61.5%	Easy	->>
<<-1259	Handshakes That Don't Cross    		53.9%	Hard	->>
<<-1258	Synonymous Sentences    		67.2%	Medium	->>
<<-1257	Smallest Common Region    		60.0%	Medium	->>
<<-1256	Encode Number    		67.0%	Medium	->>
<<-1255	Maximum Score Words Formed by Letters    		69.6%	Hard	->>
<<-1254	Number of Closed Islands    		61.1%	Medium	->>
<<-1253	Reconstruct a 2-Row Binary Matrix    		41.2%	Medium	->>
<<-1252	Cells with Odd Values in a Matrix    		78.3%	Easy	->>
<<-1250	Check If It Is a Good Array    		56.1%	Hard	->>
<<-1249	Minimum Remove to Make Valid Parentheses    		63.2%	Medium	->>
<<-1248	Count Number of Nice Subarrays    		56.6%	Medium	->>
<<-1247	Minimum Swaps to Make Strings Equal    		62.2%	Medium	->>
<<-1246	Palindrome Removal    		45.7%	Hard	->>
<<-1245	Tree Diameter    		61.1%	Medium	->>
<<-1244	Design A Leaderboard    		64.6%	Medium	->>
<<-1243	Array Transformation    		50.6%	Easy	->>
<<-1240	Tiling a Rectangle with the Fewest Squares    		51.3%	Hard	->>
<<-1239	Maximum Length of a Concatenated String with Unique Characters    		48.7%	Medium	->>
<<-1238	Circular Permutation in Binary Representation    		65.4%	Medium	->>
<<-1237	Find Positive Integer Solution for a Given Equation    		69.6%	Easy	->>
<<-1236	Web Crawler    		64.3%	Medium	->>
<<-1235	Maximum Profit in Job Scheduling    		46.0%	Hard	->>
<<-1234	Replace the Substring for Balanced String    		34.1%	Medium	->>
<<-1233	Remove Sub-Folders from the Filesystem    		61.2%	Medium	->>
<<-1232	Check If It Is a Straight Line    		44.3%	Easy	->>
<<-1231	Divide Chocolate    		53.1%	Hard	->>
<<-1230	Toss Strange Coins    		49.3%	Medium	->>
<<-1229	Meeting Scheduler    		53.8%	Medium	->>
<<-1228	Missing Number In Arithmetic Progression    		52.0%	Easy	->>
<<-1227	Airplane Seat Assignment Probability    		61.8%	Medium	->>
<<-1224	Maximum Equal Frequency    		34.2%	Hard	->>
<<-1223	Dice Roll Simulation    		46.8%	Medium	->>
<<-1222	Queens That Can Attack the King    		69.0%	Medium	->>
<<-1221	Split a String in Balanced Strings    		83.8%	Easy	->>
<<-1220	Count Vowels Permutation    		53.8%	Hard	->>
<<-1219	Path with Maximum Gold    		65.6%	Medium	->>
<<-1218	Longest Arithmetic Subsequence of Given Difference    		45.9%	Medium	->>
<<-1217	Minimum Cost to Move Chips to The Same Position    		71.2%	Easy	->>
<<-1216	Valid Palindrome III    		48.8%	Hard	->>
<<-1215	Stepping Numbers    		42.7%	Medium	->>
<<-1214	Two Sum BSTs    		67.8%	Medium	->>
<<-1213	Intersection of Three Sorted Arrays    		79.0%	Easy	->>
<<-1210	Minimum Moves to Reach Target with Rotations    		45.9%	Hard	->>
<<-1209	Remove All Adjacent Duplicates in String II    		57.4%	Medium	->>
<<-1208	Get Equal Substrings Within Budget    		42.9%	Medium	->>
<<-1207	Unique Number of Occurrences    		71.7%	Easy	->>
<<-1206	Design Skiplist    		57.9%	Hard	->>
<<-1203	Sort Items by Groups Respecting Dependencies    		48.6%	Hard	->>
<<-1202	Smallest String With Swaps    		47.5%	Medium	->>
<<-1201	Ugly Number III    		26.2%	Medium	->>
<<-1200	Minimum Absolute Difference    		66.6%	Easy	->>
<<-1199	Minimum Time to Build Blocks    		38.0%	Hard	->>
<<-1198	Find Smallest Common Element in All Rows    		74.9%	Medium	->>
<<-1197	Minimum Knight Moves    		36.7%	Medium	->>
<<-1196	How Many Apples Can You Put into the Basket    		67.9%	Easy	->>
<<-1192	Critical Connections in a Network    		49.4%	Hard	->>
<<-1191	K-Concatenation Maximum Sum    		25.4%	Medium	->>
<<-1190	Reverse Substrings Between Each Pair of Parentheses    		63.8%	Medium	->>
<<-1189	Maximum Number of Balloons    		61.7%	Easy	->>
<<-1187	Make Array Strictly Increasing    		41.5%	Hard	->>
<<-1186	Maximum Subarray Sum with One Deletion    		38.3%	Medium	->>
<<-1185	Day of the Week    		62.5%	Easy	->>
<<-1184	Distance Between Bus Stops    		54.3%	Easy	->>
<<-1183	Maximum Number of Ones    		56.1%	Hard	->>
<<-1182	Shortest Distance to Target Color    		53.2%	Medium	->>
<<-1181	Before and After Puzzle    		44.4%	Medium	->>
<<-1180	Count Substrings with Only One Distinct Letter    		77.2%	Easy	->>
<<-1178	Number of Valid Words for Each Puzzle    		38.4%	Hard	->>
<<-1177	Can Make Palindrome from Substring    		35.7%	Medium	->>
<<-1176	Diet Plan Performance    		53.9%	Easy	->>
<<-1175	Prime Arrangements    		51.5%	Easy	->>
<<-1172	Dinner Plate Stacks    		38.0%	Hard	->>
<<-1171	Remove Zero Sum Consecutive Nodes from Linked List    		41.4%	Medium	->>
<<-1170	Compare Strings by Frequency of the Smallest Character    		59.3%	Easy	->>
<<-1169	Invalid Transactions    		31.5%	Medium	->>
<<-1168	Optimize Water Distribution in a Village    		62.1%	Hard	->>
<<-1167	Minimum Cost to Connect Sticks    		63.8%	Medium	->>
<<-1166	Design File System    		57.8%	Medium	->>
<<-1165	Single-Row Keyboard    		84.6%	Easy	->>
<<-1163	Last Substring in Lexicographical Order    		35.7%	Hard	->>
<<-1162	As Far from Land as Possible    		44.3%	Medium	->>
<<-1161	Maximum Level Sum of a Binary Tree    		71.1%	Medium	->>
<<-1160	Find Words That Can Be Formed by Characters    		67.3%	Easy	->>
<<-1157	Online Majority Element In Subarray    		39.3%	Hard	->>
<<-1156	Swap For Longest Repeated Character Substring    		47.7%	Medium	->>
<<-1155	Number of Dice Rolls With Target Sum    		47.7%	Medium	->>
<<-1154	Day of the Year    		49.3%	Easy	->>
<<-1153	String Transforms Into Another String    		35.9%	Hard	->>
<<-1152	Analyze User Website Visit Pattern    		43.2%	Medium	->>
<<-1151	Minimum Swaps to Group All 1's Together    		58.7%	Medium	->>
<<-1150	Check If a Number Is Majority Element in a Sorted Array    		58.3%	Easy	->>
<<-1147	Longest Chunked Palindrome Decomposition    		59.1%	Hard	->>
<<-1146	Snapshot Array    		36.9%	Medium	->>
<<-1145	Binary Tree Coloring Game    		51.4%	Medium	->>
<<-1144	Decrease Elements To Make Array Zigzag    		45.8%	Medium	->>
<<-1143	Longest Common Subsequence    		58.6%	Medium	->>
<<-1140	Stone Game II    		64.9%	Medium	->>
<<-1139	Largest 1-Bordered Square    		48.1%	Medium	->>
<<-1138	Alphabet Board Path    		49.8%	Medium	->>
<<-1137	N-th Tribonacci Number    		56.2%	Easy	->>
<<-1136	Parallel Courses    		61.1%	Hard	->>
<<-1135	Connecting Cities With Minimum Cost    		58.6%	Medium	->>
<<-1134	Armstrong Number    		78.0%	Easy	->>
<<-1133	Largest Unique Number    		67.1%	Easy	->>
<<-1131	Maximum of Absolute Value Expression    		52.2%	Medium	->>
<<-1130	Minimum Cost Tree From Leaf Values    		67.1%	Medium	->>
<<-1129	Shortest Path with Alternating Colors    		39.6%	Medium	->>
<<-1128	Number of Equivalent Domino Pairs    		46.8%	Easy	->>
<<-1125	Smallest Sufficient Team    		46.9%	Hard	->>
<<-1124	Longest Well-Performing Interval    		33.0%	Medium	->>
<<-1123	Lowest Common Ancestor of Deepest Leaves    		67.2%	Medium	->>
<<-1122	Relative Sort Array    		67.7%	Easy	->>
<<-1121	Divide Array Into Increasing Sequences    		57.6%	Hard	->>
<<-1120	Maximum Average Subtree    		63.1%	Medium	->>
<<-1119	Remove Vowels from a String    		90.2%	Easy	->>
<<-1118	Number of Days in a Month    		57.3%	Easy	->>
<<-1111	Maximum Nesting Depth of Two Valid Parentheses Strings    		72.3%	Medium	->>
<<-1110	Delete Nodes And Return Forest    		67.4%	Medium	->>
<<-1109	Corporate Flight Bookings    		53.8%	Medium	->>
<<-1108	Defanging an IP Address    		88.2%	Easy	->>
<<-1106	Parsing A Boolean Expression    		58.9%	Hard	->>
<<-1105	Filling Bookcase Shelves    		57.9%	Medium	->>
<<-1104	Path In Zigzag Labelled Binary Tree    		72.6%	Medium	->>
<<-1103	Distribute Candies to People    		63.6%	Easy	->>
<<-1102	Path With Maximum Minimum Value    		49.9%	Medium	->>
<<-1101	The Earliest Moment When Everyone Become Friends    		66.8%	Medium	->>
<<-1100	Find K-Length Substrings With No Repeated Characters    		73.4%	Medium	->>
<<-1099	Two Sum Less Than K    		60.8%	Easy	->>
<<-1096	Brace Expansion II    		62.3%	Hard	->>
<<-1095	Find in Mountain Array    		35.7%	Hard	->>
<<-1094	Car Pooling    		59.0%	Medium	->>
<<-1093	Statistics from a Large Sample    		49.0%	Medium	->>
<<-1092	Shortest Common Supersequence     		52.5%	Hard	->>
<<-1091	Shortest Path in Binary Matrix    		38.6%	Medium	->>
<<-1090	Largest Values From Labels    		60.0%	Medium	->>
<<-1089	Duplicate Zeros    		52.2%	Easy	->>
<<-1088	Confusing Number II    		44.8%	Hard	->>
<<-1087	Brace Expansion    		63.0%	Medium	->>
<<-1086	High Five    		79.9%	Easy	->>
<<-1085	Sum of Digits in the Minimum Number    		74.8%	Easy	->>
<<-1081	Smallest Subsequence of Distinct Characters    		53.3%	Medium	->>
<<-1080	Insufficient Nodes in Root to Leaf Paths    		49.7%	Medium	->>
<<-1079	Letter Tile Possibilities    		75.6%	Medium	->>
<<-1078	Occurrences After Bigram    		64.7%	Easy	->>
<<-1074	Number of Submatrices That Sum to Target    		60.9%	Hard	->>
<<-1073	Adding Two Negabinary Numbers    		34.6%	Medium	->>
<<-1072	Flip Columns For Maximum Number of Equal Rows    		61.2%	Medium	->>
<<-1071	Greatest Common Divisor of Strings    		51.7%	Easy	->>
<<-1067	Digit Count in Range    		40.4%	Hard	->>
<<-1066	Campus Bikes II    		54.0%	Medium	->>
<<-1065	Index Pairs of a String    		61.1%	Easy	->>
<<-1064	Fixed Point    		65.6%	Easy	->>
<<-1063	Number of Valid Subarrays    		71.7%	Hard	->>
<<-1062	Longest Repeating Substring    		57.7%	Medium	->>
<<-1061	Lexicographically Smallest Equivalent String    		66.0%	Medium	->>
<<-1060	Missing Element in Sorted Array    		54.5%	Medium	->>
<<-1059	All Paths from Source Lead to Destination    		43.3%	Medium	->>
<<-1058	Minimize Rounding Error to Meet Target    		42.7%	Medium	->>
<<-1057	Campus Bikes    		57.6%	Medium	->>
<<-1056	Confusing Number    		47.6%	Easy	->>
<<-1055	Shortest Way to Form String    		57.2%	Medium	->>
<<-1054	Distant Barcodes    		44.0%	Medium	->>
<<-1053	Previous Permutation With One Swap    		50.5%	Medium	->>
<<-1052	Grumpy Bookstore Owner    		55.5%	Medium	->>
<<-1051	Height Checker    		71.8%	Easy	->>
<<-1049	Last Stone Weight II    		44.3%	Medium	->>
<<-1048	Longest String Chain    		55.2%	Medium	->>
<<-1047	Remove All Adjacent Duplicates In String    		69.7%	Easy	->>
<<-1046	Last Stone Weight    		62.4%	Easy	->>
<<-1044	Longest Duplicate Substring    		31.8%	Hard	->>
<<-1043	Partition Array for Maximum Sum    		66.4%	Medium	->>
<<-1042	Flower Planting With No Adjacent    		48.4%	Medium	->>
<<-1041	Robot Bounded In Circle    		54.4%	Medium	->>
<<-1040	Moving Stones Until Consecutive II    		53.3%	Medium	->>
<<-1039	Minimum Score Triangulation of Polygon    		49.9%	Medium	->>
<<-1038	Binary Search Tree to Greater Sum Tree    		81.5%	Medium	->>
<<-1037	Valid Boomerang    		37.8%	Easy	->>
<<-1036	Escape a Large Maze    		35.1%	Hard	->>
<<-1035	Uncrossed Lines    		56.0%	Medium	->>
<<-1034	Coloring A Border    		45.1%	Medium	->>
<<-1033	Moving Stones Until Consecutive    		42.6%	Easy	->>
<<-1032	Stream of Characters    		48.4%	Hard	->>
<<-1031	Maximum Sum of Two Non-Overlapping Subarrays    		58.6%	Medium	->>
<<-1030	Matrix Cells in Distance Order    		66.9%	Easy	->>
<<-1029	Two City Scheduling    		57.1%	Medium	->>
<<-1028	Recover a Tree From Preorder Traversal    		70.3%	Hard	->>
<<-1027	Longest Arithmetic Subsequence    		50.4%	Medium	->>
<<-1026	Maximum Difference Between Node and Ancestor    		68.8%	Medium	->>
<<-1025	Divisor Game    		66.1%	Easy	->>
<<-1024	Video Stitching    		49.3%	Medium	->>
<<-1023	Camelcase Matching    		57.1%	Medium	->>
<<-1022	Sum of Root To Leaf Binary Numbers    		71.2%	Easy	->>
<<-1021	Remove Outermost Parentheses    		78.5%	Easy	->>
<<-1020	Number of Enclaves    		58.5%	Medium	->>
<<-1019	Next Greater Node In Linked List    		58.2%	Medium	->>
<<-1018	Binary Prefix Divisible By 5    		47.7%	Easy	->>
<<-1017	Convert to Base -2    		59.4%	Medium	->>
<<-1016	Binary String With Substrings Representing 1 To N    		59.4%	Medium	->>
<<-1015	Smallest Integer Divisible by K    		32.6%	Medium	->>
<<-1014	Best Sightseeing Pair    		52.7%	Medium	->>
<<-1013	Partition Array Into Three Parts With Equal Sum    		50.0%	Easy	->>
<<-1012	Numbers With Repeated Digits    		37.8%	Hard	->>

<<-1011	Capacity To Ship Packages Within D Days    		59.3%	Medium	->>
<<-1010	Pairs of Songs With Total Durations Divisible by 60    		47.9%	Easy	->>
<<-1009	Complement of Base 10 Integer    		61.5%	Easy	->>
<<-1008	Construct Binary Search Tree from Preorder Traversal    		78.6%	Medium	->>
<<-1007	Minimum Domino Rotations For Equal Row    		50.9%	Medium	->>
<<-1006	Clumsy Factorial    		53.6%	Medium	->>
<<-1005	Maximize Sum Of Array After K Negations    		52.2%	Easy	->>
<<-1004	Max Consecutive Ones III    		60.1%	Medium	->>
<<-1003	Check If Word Is Valid After Substitutions    		55.6%	Medium	->>
<<-1002	Find Common Characters    		67.8%	Easy	->>
<<-1001	Grid Illumination    		36.3%	Hard	->>
<<-1000	Minimum Cost to Merge Stones    		40.4%	Hard	->>
<<-999	Available Captures for Rook    		66.8%	Easy	->>
<<-998	Maximum Binary Tree II    		63.6%	Medium	->>
<<-997	Find the Town Judge    		49.9%	Easy	->>
<<-996	Number of Squareful Arrays    		47.5%	Hard	->>
<<-995	Minimum Number of K Consecutive Bit Flips    		49.2%	Hard	->>
<<-994	Rotting Oranges    		49.4%	Medium	->>
<<-993	Cousins in Binary Tree    		52.1%	Easy	->>
<<-992	Subarrays with K Different Integers    		49.9%	Hard	->>
<<-991	Broken Calculator    		46.3%	Medium	->>
<<-990	Satisfiability of Equality Equations    		46.0%	Medium	->>
<<-989	Add to Array-Form of Integer    		44.5%	Easy	->>
<<-988	Smallest String Starting From Leaf    		46.4%	Medium	->>
<<-987	Vertical Order Traversal of a Binary Tree    		37.2%	Medium	->>
<<-986	Interval List Intersections    		67.8%	Medium	->>
<<-985	Sum of Even Numbers After Queries    		60.8%	Easy	->>
<<-984	String Without AAA or BBB    		38.2%	Medium	->>
<<-983	Minimum Cost For Tickets    		62.5%	Medium	->>
<<-982	Triples with Bitwise AND Equal To Zero    		55.7%	Hard	->>
<<-981	Time Based Key-Value Store    		53.6%	Medium	->>
<<-980	Unique Paths III    		77.0%	Hard	->>
<<-979	Distribute Coins in Binary Tree    		69.2%	Medium	->>
<<-978	Longest Turbulent Subarray    		46.6%	Medium	->>
<<-977	Squares of a Sorted Array    		72.1%	Easy	->>
<<-976	Largest Perimeter Triangle    		58.4%	Easy	->>
<<-975	Odd Even Jump    		41.7%	Hard	->>
<<-974	Subarray Sums Divisible by K    		49.9%	Medium	->>
<<-973	K Closest Points to Origin    		64.3%	Medium	->>
<<-972	Equal Rational Numbers    		41.8%	Hard	->>
<<-971	Flip Binary Tree To Match Preorder Traversal    		46.1%	Medium	->>
<<-970	Powerful Integers    		40.0%	Easy	->>
<<-969	Pancake Sorting    		68.4%	Medium	->>
<<-968	Binary Tree Cameras    		38.1%	Hard	->>
<<-967	Numbers With Same Consecutive Differences    		44.1%	Medium	->>
<<-966	Vowel Spellchecker    		47.6%	Medium	->>
<<-965	Univalued Binary Tree    		67.6%	Easy	->>
<<-964	Least Operators to Express Number    		44.5%	Hard	->>
<<-963	Minimum Area Rectangle II    		51.5%	Medium	->>
<<-962	Maximum Width Ramp    		45.9%	Medium	->>

<<-961	N-Repeated Element in Size 2N Array    		74.2%	Easy	->>
<<-960	Delete Columns to Make Sorted III    		54.0%	Hard	->>
<<-959	Regions Cut By Slashes    		66.6%	Medium	->>
<<-958	Check Completeness of a Binary Tree    		52.3%	Medium	->>
<<-957	Prison Cells After N Days    		40.3%	Medium	->>
<<-956	Tallest Billboard    		39.8%	Hard	->>
<<-955	Delete Columns to Make Sorted II    		33.4%	Medium	->>
<<-954	Array of Doubled Pairs    		35.4%	Medium	->>
<<-953	Verifying an Alien Dictionary    		53.0%	Easy	->>
<<-952	Largest Component Size by Common Factor    		36.0%	Hard	->>
<<-951	Flip Equivalent Binary Trees    		65.5%	Medium	->>
<<-950	Reveal Cards In Increasing Order    		75.0%	Medium	->>
<<-949	Largest Time for Given Digits    		36.3%	Medium	->>
<<-948	Bag of Tokens    		46.2%	Medium	->>
<<-947	Most Stones Removed with Same Row or Column    		55.3%	Medium	->>
<<-946	Validate Stack Sequences    		63.1%	Medium	->>
<<-945	Minimum Increment to Make Array Unique    		46.5%	Medium	->>
<<-944	Delete Columns to Make Sorted    		70.9%	Easy	->>
<<-943	Find the Shortest Superstring    		43.2%	Hard	->>
<<-942	DI String Match    		73.2%	Easy	->>
<<-941	Valid Mountain Array    		32.3%	Easy	->>
<<-940	Distinct Subsequences II    		41.5%	Hard	->>
<<-939	Minimum Area Rectangle    		51.7%	Medium	->>
<<-938	Range Sum of BST    		82.6%	Easy	->>
<<-937	Reorder Data in Log Files    		54.3%	Easy	->>
<<-936	Stamping The Sequence    		46.8%	Hard	->>
<<-935	Knight Dialer    		45.9%	Medium	->>
<<-934	Shortest Bridge    		49.2%	Medium	->>
<<-933	Number of Recent Calls    		71.9%	Easy	->>
<<-932	Beautiful Array    		60.9%	Medium	->>
<<-931	Minimum Falling Path Sum    		63.1%	Medium	->>
<<-930	Binary Subarrays With Sum    		43.8%	Medium	->>
<<-929	Unique Email Addresses    		67.2%	Easy	->>
<<-928	Minimize Malware Spread II    		41.0%	Hard	->>
<<-927	Three Equal Parts    		34.2%	Hard	->>
<<-926	Flip String to Monotone Increasing    		52.7%	Medium	->>
<<-925	Long Pressed Name    		39.0%	Easy	->>
<<-924	Minimize Malware Spread    		41.9%	Hard	->>
<<-923	3Sum With Multiplicity    		35.9%	Medium	->>
<<-922	Sort Array By Parity II    		70.0%	Easy	->>
<<-921	Minimum Add to Make Parentheses Valid    		74.4%	Medium	->>
<<-920	Number of Music Playlists    		47.4%	Hard	->>
<<-919	Complete Binary Tree Inserter    		58.1%	Medium	->>
<<-918	Maximum Sum Circular Subarray    		34.0%	Medium	->>
<<-917	Reverse Only Letters    		58.4%	Easy	->>
<<-916	Word Subsets    		48.1%	Medium	->>
<<-915	Partition Array into Disjoint Intervals    		45.7%	Medium	->>
<<-914	X of a Kind in a Deck of Cards    		34.5%	Easy	->>
<<-913	Cat and Mouse    		33.8%	Hard	->>
<<-912	Sort an Array    		64.1%	Medium	->>

<<-911	Online Election    		51.0%	Medium	->>
<<-910	Smallest Range II    		27.0%	Medium	->>
<<-909	Snakes and Ladders    		38.9%	Medium	->>
<<-908	Smallest Range I    		65.9%	Easy	->>
<<-907	Sum of Subarray Minimums    		33.3%	Medium	->>
<<-906	Super Palindromes    		32.8%	Hard	->>
<<-905	Sort Array By Parity    		74.9%	Easy	->>
<<-904	Fruit Into Baskets    		42.7%	Medium	->>
<<-903	Valid Permutations for DI Sequence    		53.8%	Hard	->>
<<-902	Numbers At Most N Given Digit Set    		32.0%	Hard	->>
<<-901	Online Stock Span    		60.9%	Medium	->>
<<-900	RLE Iterator    		54.8%	Medium	->>
<<-899	Orderly Queue    		52.8%	Hard	->>
<<-898	Bitwise ORs of Subarrays    		34.0%	Medium	->>
<<-897	Increasing Order Search Tree    		72.5%	Easy	
change tree to a in-order order tree.
using reverse inorder traversal.
root->right=process(root->right,prev)
...
return process(root->left,prev);
level: 3
->>
<<-896	Monotonic Array    		58.0%	Easy	->>
<<-895	Maximum Frequency Stack    		61.9%	Hard	->>
<<-894	All Possible Full Binary Trees    		76.5%	Medium	->>
<<-893	Groups of Special-Equivalent Strings    		67.9%	Easy	->>
<<-892	Surface Area of 3D Shapes    		59.4%	Easy	->>
<<-891	Sum of Subsequence Widths    		32.7%	Hard	->>
<<-890	Find and Replace Pattern    		74.0%	Medium	->>
<<-889	Construct Binary Tree from Preorder and Postorder Traversal    		66.9%	Medium	->>
<<-888	Fair Candy Swap    		58.6%	Easy	->>
<<-887	Super Egg Drop    		27.1%	Hard	->>
<<-886	Possible Bipartition    		44.7%	Medium	->>
<<-885	Spiral Matrix III    		70.0%	Medium	->>
<<-884	Uncommon Words from Two Sentences    		63.8%	Easy	->>
<<-883	Projection Area of 3D Shapes    		67.9%	Easy	->>
<<-882	Reachable Nodes In Subdivided Graph    		42.0%	Hard	->>
<<-881	Boats to Save People    		47.4%	Medium	->>
<<-880	Decoded String at Index    		24.5%	Medium	->>
<<-879	Profitable Schemes    		39.9%	Hard	->>
<<-878	Nth Magical Number    		28.6%	Hard	->>
<<-877	Stone Game    		66.0%	Medium	->>
<<-876	Middle of the Linked List    		68.8%	Easy	->>
<<-875	Koko Eating Bananas    		53.0%	Medium	->>
<<-874	Walking Robot Simulation    		36.5%	Easy	->>
<<-873	Length of Longest Fibonacci Subsequence    		47.9%	Medium	->>
<<-872	Leaf-Similar Trees    		64.5%	Easy	->>
<<-871	Minimum Number of Refueling Stops    		31.8%	Hard	->>
<<-870	Advantage Shuffle    		46.1%	Medium	->>
<<-869	Reordered Power of 2    		53.9%	Medium	->>
<<-868	Binary Gap    		60.8%	Easy	->>
<<-867	Transpose Matrix    		62.3%	Easy	->>
<<-866	Prime Palindrome    		25.0%	Medium	->>
<<-865	Smallest Subtree with all the Deepest Nodes    		61.4%	Medium	->>
<<-864	Shortest Path to Get All Keys    		41.1%	Hard	->>
<<-863	All Nodes Distance K in Binary Tree    		56.8%	Medium	->>
<<-862	Shortest Subarray with Sum at Least K    		24.9%	Hard	->>
<<-	#	Title	Solution	Acceptance	Difficulty	Frequency  ->>
<<-861	Score After Flipping Matrix    		73.1%	Medium	->>
<<-860	Lemonade Change    		51.9%	Easy	->>
<<-859	Buddy Strings    		29.9%	Easy	->>
<<-858	Mirror Reflection    		59.4%	Medium	->>
<<-857	Minimum Cost to Hire K Workers    		50.0%	Hard	->>
<<-856	Score of Parentheses    		61.8%	Medium	->>
<<-855	Exam Room    		43.4%	Medium	->>
<<-854	K-Similar Strings    		38.3%	Hard	->>
<<-853	Car Fleet    		43.3%	Medium	->>
<<-852	Peak Index in a Mountain Array    		71.8%	Easy	->>
<<-851	Loud and Rich    		52.2%	Medium	->>
<<-850	Rectangle Area II    		48.2%	Hard	->>
<<-849	Maximize Distance to Closest Person    		44.3%	Medium	->>
<<-848	Shifting Letters    		44.9%	Medium	->>
<<-847	Shortest Path Visiting All Nodes    		52.7%	Hard	->>
<<-846	Hand of Straights    		55.0%	Medium	->>
<<-845	Longest Mountain in Array    		38.5%	Medium	->>
<<-844	Backspace String Compare    		46.7%	Easy	->>
<<-843	Guess the Word    		46.4%	Hard	->>
<<-842	Split Array into Fibonacci Sequence    		36.4%	Medium	->>
<<-841	Keys and Rooms    		64.9%	Medium	->>
<<-840	Magic Squares In Grid    		37.6%	Medium	->>
<<-839	Similar String Groups    		39.1%	Hard	->>
<<-838	Push Dominoes    		49.3%	Medium	->>
<<-837	New 21 Game    		35.2%	Medium	->>
<<-836	Rectangle Overlap    		46.3%	Easy	->>
<<-835	Image Overlap    		62.0%	Medium	->>
<<-834	Sum of Distances in Tree    		44.9%	Hard	->>
<<-833	Find And Replace in String    		51.0%	Medium	->>
<<-832	Flipping an Image    		77.6%	Easy	->>
<<-831	Masking Personal Information    		44.6%	Medium	->>
<<-830	Positions of Large Groups    		50.0%	Easy	->>
<<-829	Consecutive Numbers Sum    		39.3%	Hard	->>
<<-828	Count Unique Characters of All Substrings of a Given String    		46.5%	Hard	->>
<<-827	Making A Large Island    		46.4%	Hard	->>
<<-826	Most Profit Assigning Work    		38.8%	Medium	->>
<<-825	Friends Of Appropriate Ages    		43.5%	Medium	->>
<<-824	Goat Latin    		66.2%	Easy	->>
<<-823	Binary Trees With Factors    		36.1%	Medium	->>
<<-822	Card Flipping Game    		43.4%	Medium	->>
<<-821	Shortest Distance to a Character    		67.5%	Easy	->>
<<-820	Short Encoding of Words    		51.2%	Medium	->>
<<-819	Most Common Word    		45.2%	Easy	->>
<<-818	Race Car    		39.4%	Hard	->>
<<-817	Linked List Components    		57.4%	Medium	->>
<<-816	Ambiguous Coordinates    		47.6%	Medium	->>
<<-815	Bus Routes    		43.0%	Hard	->>
<<-814	Binary Tree Pruning    		73.3%	Medium	->>
<<-813	Largest Sum of Averages    		50.6%	Medium	->>
<<-812	Largest Triangle Area    		58.6%	Easy	->>
<<-	#	Title	Solution	Acceptance	Difficulty	Frequency  ->>
<<-811	Subdomain Visit Count    		70.7%	Easy	->>
<<-810	Chalkboard XOR Game    		49.1%	Hard	->>
<<-809	Expressive Words    		46.7%	Medium	->>
<<-808	Soup Servings    		40.5%	Medium	->>
<<-807	Max Increase to Keep City Skyline    		84.1%	Medium	->>
<<-806	Number of Lines To Write String    		65.3%	Easy	->>
<<-805	Split Array With Same Average    		26.5%	Hard	->>
<<-804	Unique Morse Code Words    		77.5%	Easy	->>
<<-803	Bricks Falling When Hit    		31.1%	Hard	->>
<<-802	Find Eventual Safe States    		49.3%	Medium	->>
<<-801	Minimum Swaps To Make Sequences Increasing    		39.0%	Medium	->>
<<-800	Similar RGB Color    		61.9%	Easy	->>
<<-799	Champagne Tower    		43.8%	Medium	->>
<<-798	Smallest Rotation with Highest Score    		44.6%	Hard	->>
<<-797	All Paths From Source to Target    		78.2%	Medium	->>
<<-796	Rotate String    		49.4%	Easy	->>
<<-795	Number of Subarrays with Bounded Maximum    		46.9%	Medium	->>
<<-794	Valid Tic-Tac-Toe State    		33.6%	Medium	->>
<<-793	Preimage Size of Factorial Zeroes Function    		40.4%	Hard	->>
<<-792	Number of Matching Subsequences    		47.8%	Medium	->>
<<-791	Custom Sort String    		65.8%	Medium	->>
<<-790	Domino and Tromino Tiling    		39.7%	Medium	->>
<<-789	Escape The Ghosts    		57.8%	Medium	->>
<<-788	Rotated Digits    		57.3%	Easy	->>
<<-787	Cheapest Flights Within K Stops    		39.4%	Medium	->>
<<-786	K-th Smallest Prime Fraction    		41.4%	Hard	->>
<<-785	Is Graph Bipartite?    		48.0%	Medium	->>
<<-784	Letter Case Permutation    		65.7%	Medium	->>
<<-783	Minimum Distance Between BST Nodes    		53.4%	Easy	->>
<<-782	Transform to Chessboard    		46.8%	Hard	->>
<<-781	Rabbits in Forest    		55.1%	Medium	->>
<<-780	Reaching Points    		30.0%	Hard	->>
<<-779	K-th Symbol in Grammar    		38.3%	Medium	->>
<<-778	Swim in Rising Water    		54.0%	Hard	->>
<<-777	Swap Adjacent in LR String    		35.1%	Medium	->>
<<-776	Split BST    		56.3%	Medium	->>
<<-775	Global and Local Inversions    		42.4%	Medium	->>
<<-774	Minimize Max Distance to Gas Station    		47.7%	Hard	->>
<<-773	Sliding Puzzle    		60.2%	Hard	->>
<<-772	Basic Calculator III    		42.5%	Hard	->>
<<-771	Jewels and Stones    		86.7%	Easy	->>
<<-770	Basic Calculator IV    		54.1%	Hard	->>
<<-769	Max Chunks To Make Sorted    		55.2%	Medium	->>
<<-768	Max Chunks To Make Sorted II    		49.4%	Hard	->>
<<-767	Reorganize String    		49.5%	Medium	->>
<<-766	Toeplitz Matrix    		65.6%	Easy	->>
<<-765	Couples Holding Hands    		55.0%	Hard	->>
<<-764	Largest Plus Sign    		46.1%	Medium	->>
<<-763	Partition Labels    		77.7%	Medium	->>
<<-762	Prime Number of Set Bits in Binary Representation    		63.8%	Easy	->>

<<-761	Special Binary String    		58.4%	Hard	->>
<<-760	Find Anagram Mappings    		81.5%	Easy	->>
<<-759	Employee Free Time    		67.4%	Hard	->>
<<-758	Bold Words in String    		46.8%	Easy	->>
<<-757	Set Intersection Size At Least Two    		41.6%	Hard	->>
<<-756	Pyramid Transition Matrix    		55.3%	Medium	->>
<<-755	Pour Water    		43.8%	Medium	->>
<<-754	Reach a Number    		35.1%	Medium	->>
<<-753	Cracking the Safe    		51.4%	Hard	->>
<<-752	Open the Lock    		52.3%	Medium	->>
<<-751	IP to CIDR    		60.6%	Medium	->>
<<-750	Number Of Corner Rectangles    		66.6%	Medium	->>
<<-749	Contain Virus    		47.1%	Hard	->>
<<-748	Shortest Completing Word    		57.1%	Easy	->>
<<-747	Largest Number At Least Twice of Others    		42.6%	Easy	->>
<<-746	Min Cost Climbing Stairs    		50.7%	Easy	->>
<<-745	Prefix and Suffix Search    		34.7%	Hard	->>
<<-744	Find Smallest Letter Greater Than Target    		45.6%	Easy	->>
<<-743	Network Delay Time    		45.1%	Medium	->>
<<-742	Closest Leaf in a Binary Tree    		44.0%	Medium	->>
<<-741	Cherry Pickup    		34.6%	Hard	->>
<<-740	Delete and Earn    		48.9%	Medium	->>
<<-739	Daily Temperatures    		64.1%	Medium	->>
<<-738	Monotone Increasing Digits    		45.1%	Medium	->>
<<-737	Sentence Similarity II    		46.3%	Medium	->>
<<-736	Parse Lisp Expression    		49.8%	Hard	->>
<<-735	Asteroid Collision    		43.0%	Medium	->>
<<-734	Sentence Similarity    		42.2%	Easy	->>
<<-733	Flood Fill    		55.6%	Easy	->>
<<-732	My Calendar III    		60.9%	Hard	->>
<<-731	My Calendar II    		49.8%	Medium	->>
<<-730	Count Different Palindromic Subsequences    		43.1%	Hard	->>
<<-729	My Calendar I    		52.7%	Medium	->>
<<-728	Self Dividing Numbers    		75.0%	Easy	->>
<<-727	Minimum Window Subsequence    		42.1%	Hard	->>
<<-726	Number of Atoms    		50.9%	Hard	->>
<<-725	Split Linked List in Parts    		52.6%	Medium	->>
<<-724	Find Pivot Index    		44.7%	Easy	->>
<<-723	Candy Crush    		71.5%	Medium	->>
<<-722	Remove Comments    		35.5%	Medium	->>
<<-721	Accounts Merge    		50.4%	Medium	->>
<<-720	Longest Word in Dictionary    		48.9%	Easy	->>
<<-719	Find K-th Smallest Pair Distance    		32.2%	Hard	->>
<<-718	Maximum Length of Repeated Subarray    		49.7%	Medium	->>
<<-717	1-bit and 2-bit Characters    		47.8%	Easy	->>
<<-716	Max Stack    		42.8%	Easy	->>
<<-715	Range Module    		39.5%	Hard	->>
<<-714	Best Time to Buy and Sell Stock with Transaction Fee    		55.4%	Medium	->>
<<-713	Subarray Product Less Than K    		40.3%	Medium	->>
<<-712	Minimum ASCII Delete Sum for Two Strings    		59.0%	Medium	->>

<<-711	Number of Distinct Islands II    		48.6%	Hard	->>
<<-710	Random Pick with Blacklist    		32.5%	Hard	->>
<<-709	To Lower Case    		79.8%	Easy	->>
<<-708	Insert into a Sorted Circular Linked List    		32.2%	Medium	->>
<<-707	Design Linked List    		25.3%	Medium	->>
<<-706	Design HashMap    		62.1%	Easy	->>
<<-705	Design HashSet    		64.5%	Easy	->>
<<-704	Binary Search    		53.7%	Easy	->>
<<-703	Kth Largest Element in a Stream    		50.2%	Easy	->>
<<-702	Search in a Sorted Array of Unknown Size    		68.1%	Medium	->>
<<-701	Insert into a Binary Search Tree    		76.0%	Medium	->>
<<-700	Search in a Binary Search Tree    		73.3%	Easy	->>
<<-699	Falling Squares    		42.2%	Hard	->>
<<-698	Partition to K Equal Sum Subsets    		45.3%	Medium	->>
<<-697	Degree of an Array    		54.2%	Easy	->>
<<-696	Count Binary Substrings    		57.0%	Easy	->>
<<-695	Max Area of Island    		63.7%	Medium	->>
<<-694	Number of Distinct Islands    		56.8%	Medium	->>
<<-693	Binary Number with Alternating Bits    		59.6%	Easy	->>
<<-692	Top K Frequent Words    		52.6%	Medium	->>
<<-691	Stickers to Spell Word    		43.5%	Hard	->>
<<-690	Employee Importance    		57.9%	Easy	->>
<<-689	Maximum Sum of 3 Non-Overlapping Subarrays    		46.9%	Hard	->>
<<-688	Knight Probability in Chessboard    		49.6%	Medium	->>
<<-687	Longest Univalue Path    		36.8%	Medium	->>
<<-686	Repeated String Match    		32.6%	Medium	->>
<<-685	Redundant Connection II    		32.8%	Hard	->>
<<-684	Redundant Connection    		58.3%	Medium	->>
<<-683	K Empty Slots    		35.8%	Hard	->>
<<-682	Baseball Game    		65.2%	Easy	->>
<<-681	Next Closest Time    		45.5%	Medium	->>
<<-680	Valid Palindrome II    		36.8%	Easy	->>
<<-679	24 Game    		46.9%	Hard	->>
<<-678	Valid Parenthesis String    		31.4%	Medium	->>
<<-677	Map Sum Pairs    		53.7%	Medium	->>
<<-676	Implement Magic Dictionary    		55.0%	Medium	->>
<<-675	Cut Off Trees for Golf Event    		34.9%	Hard	->>
<<-674	Longest Continuous Increasing Subsequence    		46.0%	Easy	->>
<<-673	Number of Longest Increasing Subsequence    		38.2%	Medium	->>
<<-672	Bulb Switcher II    		51.0%	Medium	->>
<<-671	Second Minimum Node In a Binary Tree    		42.7%	Easy	->>
<<-670	Maximum Swap    		44.5%	Medium	->>
<<-669	Trim a Binary Search Tree    		63.2%	Easy	->>
<<-668	Kth Smallest Number in Multiplication Table    		47.3%	Hard	->>
<<-667	Beautiful Arrangement II    		54.7%	Medium	->>
<<-666	Path Sum IV    		55.3%	Medium	->>
<<-665	Non-decreasing Array    		19.6%	Easy	->>
<<-664	Strange Printer    		41.0%	Hard	->>
<<-663	Equal Tree Partition    		39.6%	Medium	->>
<<-662	Maximum Width of Binary Tree    		40.2%	Medium	->>

<<-661	Image Smoother    		52.0%	Easy	->>
<<-660	Remove 9    		53.8%	Hard	->>
<<-659	Split Array into Consecutive Subsequences    		44.0%	Medium	->>
<<-658	Find K Closest Elements    		41.4%	Medium	->>
<<-657	Robot Return to Origin    		73.4%	Easy	->>
<<-656	Coin Path    		29.3%	Hard	->>
<<-655	Print Binary Tree    		55.4%	Medium	->>
<<-654	Maximum Binary Tree    		80.6%	Medium	->>
<<-653	Two Sum IV - Input is a BST    		55.9%	Easy	->>
<<-652	Find Duplicate Subtrees    		51.3%	Medium	->>
<<-651	4 Keys Keyboard    		52.8%	Medium	->>
<<-650	2 Keys Keyboard    		49.6%	Medium	->>
<<-649	Dota2 Senate    		39.2%	Medium	->>
<<-648	Replace Words    		57.7%	Medium	->>
<<-647	Palindromic Substrings    		61.4%	Medium	->>
<<-646	Maximum Length of Pair Chain    		52.4%	Medium	->>
<<-645	Set Mismatch    		42.3%	Easy	->>
<<-644	Maximum Average Subarray II    		32.5%	Hard	
problem: subarray length>=k and find the max subarray average.
subject: binary search, double with greedy.
approach: subarray length =k to n and we can get the max average for each length.
sum(Ai)/j=average->sum(Ai)=j*average->Sum(Ai-Average)
Sum(Ai-Average)>0 then we know we can increase it. otherwise, we shall decrease it.
this leads to a binary search approach.
To check if the array can get more or less than target average:
- Sum(Ai-target) can be achieved using prefix sum.
- we keep the 0 to i-k minimum sum and prefix sum. If prefix sum>min sum, then we know we can get average > target.
->>
<<-643	Maximum Average Subarray I    		41.7%	Easy	->>
<<-642	Design Search Autocomplete System    		45.5%	Hard	->>
<<-641	Design Circular Deque    		54.5%	Medium	->>
<<-640	Solve the Equation    		42.5%	Medium	->>
<<-639	Decode Ways II    		27.2%	Hard	->>
<<-638	Shopping Offers    		52.1%	Medium	->>
<<-637	Average of Levels in Binary Tree    		64.0%	Easy	->>
<<-636	Exclusive Time of Functions    		53.2%	Medium	->>
<<-635	Design Log Storage System    		59.0%	Medium	->>
<<-634	Find the Derangement of An Array    		40.3%	Medium	->>
<<-633	Sum of Square Numbers    		32.2%	Medium	->>
<<-632	Smallest Range Covering Elements from K Lists    		53.3%	Hard	->>
<<-631	Design Excel Sum Formula    		31.8%	Hard	->>
<<-630	Course Schedule III    		33.6%	Hard	->>
<<-629	K Inverse Pairs Array    		31.4%	Hard	->>
<<-628	Maximum Product of Three Numbers    		47.0%	Easy	->>
<<-627	Swap Salary    		76.6%	Easy	->>
<<-626	Exchange Seats    		64.1%	Medium	->>
<<-625	Minimum Factorization    		32.8%	Medium	->>
<<-624	Maximum Distance in Arrays    		39.2%	Medium	->>
<<-623	Add One Row to Tree    		50.0%	Medium	->>
<<-622	Design Circular Queue    		44.7%	Medium	->>
<<-621	Task Scheduler    		51.1%	Medium	->>
<<-620	Not Boring Movies    		68.8%	Easy	->>
<<-619	Biggest Single Number    		44.0%	Easy	->>
<<-618	Students Report By Geography    		57.7%	Hard	->>
<<-617	Merge Two Binary Trees    		74.8%	Easy	->>
<<-616	Add Bold Tag in String    		44.1%	Medium	->>
<<-615	Average Salary: Departments VS Company    		50.0%	Hard	->>
<<-614	Second Degree Follower    		31.6%	Medium	->>
<<-613	Shortest Distance in a Line    		78.7%	Easy	->>
<<-612	Shortest Distance in a Plane    		60.3%	Medium	->>

<<-611	Valid Triangle Number    		48.8%	Medium	->>
<<-610	Triangle Judgement    		67.3%	Easy	->>
<<-609	Find Duplicate File in System    		60.6%	Medium	->>
<<-608	Tree Node    		68.4%	Medium	->>
<<-607	Sales Person    		64.1%	Easy	->>
<<-606	Construct String from Binary Tree    		54.8%	Easy	->>
<<-605	Can Place Flowers    		31.4%	Easy	->>
<<-604	Design Compressed String Iterator    		37.9%	Easy	->>
<<-603	Consecutive Available Seats    		65.2%	Easy	->>
<<-602	Friend Requests II: Who Has the Most Friends    		55.8%	Medium	->>
<<-601	Human Traffic of Stadium    		43.4%	Hard	->>
<<-600	Non-negative Integers without Consecutive Ones    		34.3%	Hard	->>
<<-599	Minimum Index Sum of Two Lists    		51.2%	Easy	->>
<<-598	Range Addition II    		49.8%	Easy	->>
<<-597	Friend Requests I: Overall Acceptance Rate    		41.5%	Easy	->>
<<-596	Classes More Than 5 Students    		38.5%	Easy	->>
<<-595	Big Countries    		78.0%	Easy	->>
<<-594	Longest Harmonious Subsequence    		47.4%	Easy	->>
<<-593	Valid Square    		43.4%	Medium	->>
<<-592	Fraction Addition and Subtraction    		49.8%	Medium	->>
<<-591	Tag Validator    		34.5%	Hard	->>
<<-590	N-ary Tree Postorder Traversal    		72.9%	Easy	->>
<<-589	N-ary Tree Preorder Traversal    		72.8%	Easy	->>
<<-588	Design In-Memory File System    		46.3%	Hard	->>
<<-587	Erect the Fence    		36.2%	Hard	->>
<<-586	Customer Placing the Largest Number of Orders    		74.5%	Easy	->>
<<-585	Investments in 2016    		56.0%	Medium	->>
<<-584	Find Customer Referee    		73.2%	Easy	->>
<<-583	Delete Operation for Two Strings    		49.4%	Medium	->>
<<-582	Kill Process    		62.0%	Medium	->>
<<-581	Shortest Unsorted Continuous Subarray    		31.4%	Medium	->>
<<-580	Count Student Number in Departments    		50.3%	Medium	->>
<<-579	Find Cumulative Salary of an Employee    		37.6%	Hard	->>
<<-578	Get Highest Answer Rate Question    		40.6%	Medium	->>
<<-577	Employee Bonus    		70.0%	Easy	->>
<<-576	Out of Boundary Paths    		35.5%	Medium	->>
<<-575	Distribute Candies    		61.6%	Easy	->>
<<-574	Winning Candidate    		50.2%	Medium	->>
<<-573	Squirrel Simulation    		55.9%	Medium	->>
<<-572	Subtree of Another Tree    		44.3%	Easy	->>
<<-571	Find Median Given Frequency of Numbers    		45.4%	Hard	->>
<<-570	Managers with at Least 5 Direct Reports    		66.5%	Medium	->>
<<-569	Median Employee Salary    		60.1%	Hard	->>
<<-568	Maximum Vacation Days    		41.2%	Hard	->>
<<-567	Permutation in String    		44.5%	Medium	->>
<<-566	Reshape the Matrix    		60.8%	Easy	->>
<<-565	Array Nesting    		55.7%	Medium	->>
<<-564	Find the Closest Palindrome    		20.1%	Hard	->>
<<-563	Binary Tree Tilt    		52.2%	Easy	->>
<<-562	Longest Line of Consecutive One in Matrix    		46.1%	Medium	->>

<<-561	Array Partition I    		72.5%	Easy	->>
<<-560	Subarray Sum Equals K    		43.9%	Medium	->>
<<-559	Maximum Depth of N-ary Tree    		69.1%	Easy	->>
<<-558	Logical OR of Two Binary Grids Represented as Quad-Trees    		45.1%	Medium	->>
<<-557	Reverse Words in a String III    		71.1%	Easy	->>
<<-556	Next Greater Element III    		31.9%	Medium	->>
<<-555	Split Concatenated Strings    		42.6%	Medium	->>
<<-554	Brick Wall    		50.4%	Medium	->>
<<-553	Optimal Division    		57.1%	Medium	->>
<<-552	Student Attendance Record II    		37.0%	Hard	->>
<<-551	Student Attendance Record I    		46.0%	Easy	->>
<<-550	Game Play Analysis IV    		46.0%	Medium	->>
<<-549	Binary Tree Longest Consecutive Sequence II    		47.1%	Medium	->>
<<-548	Split Array with Equal Sum    		47.2%	Medium	->>
<<-547	Friend Circles    		59.5%	Medium	->>
<<-546	Remove Boxes    		43.7%	Hard	->>
<<-545	Boundary of Binary Tree    		39.3%	Medium	->>
<<-544	Output Contest Matches    		75.5%	Medium	->>
<<-543	Diameter of Binary Tree    		48.9%	Easy	->>
<<-542	01 Matrix    		40.3%	Medium	->>
<<-541	Reverse String II    		48.8%	Easy	->>
<<-540	Single Element in a Sorted Array    		57.9%	Medium	->>
<<-539	Minimum Time Difference    		51.9%	Medium	->>
<<-538	Convert BST to Greater Tree    		56.1%	Medium	->>
<<-537	Complex Number Multiplication    		68.1%	Medium	->>
<<-536	Construct Binary Tree from String    		49.7%	Medium	->>
<<-535	Encode and Decode TinyURL    		80.5%	Medium	->>
<<-534	Game Play Analysis III    		77.7%	Medium	->>
<<-533	Lonely Pixel II    		48.0%	Medium	->>
<<-532	K-diff Pairs in an Array    		34.4%	Medium	->>
<<-531	Lonely Pixel I    		59.3%	Medium	->>
<<-530	Minimum Absolute Difference in BST    		54.3%	Easy	->>
<<-529	Minesweeper    		60.2%	Medium	->>
<<-528	Random Pick with Weight    		44.3%	Medium	->>
<<-527	Word Abbreviation    		55.5%	Hard	->>
<<-526	Beautiful Arrangement    		59.3%	Medium	->>
<<-525	Contiguous Array    		43.2%	Medium	->>
<<-524	Longest Word in Dictionary through Deleting    		48.7%	Medium	->>
<<-523	Continuous Subarray Sum    		24.6%	Medium	->>
<<-522	Longest Uncommon Subsequence II    		34.0%	Medium	->>
<<-521	Longest Uncommon Subsequence I    		58.3%	Easy	->>
<<-520	Detect Capital    		53.8%	Easy	->>
<<-519	Random Flip Matrix    		37.4%	Medium	->>
<<-518	Coin Change 2    		51.0%	Medium	->>
<<-517	Super Washing Machines    		38.4%	Hard	->>

<<-515	Find Largest Value in Each Tree Row    		61.7%	Medium	->>
<<-514	Freedom Trail    		44.6%	Hard	->>
<<-513	Find Bottom Left Tree Value    		62.0%	Medium	->>
<<-512	Game Play Analysis II    		55.4%	Easy	->>

<<-511	Game Play Analysis I    		81.1%	Easy	->>
<<-510	Inorder Successor in BST II    		59.6%	Medium	->>
<<-509	Fibonacci Number    		67.1%	Easy	->>
<<-508	Most Frequent Subtree Sum    		58.6%	Medium	->>
<<-507	Perfect Number    		35.8%	Easy	->>
<<-506	Relative Ranks    		50.9%	Easy	->>
<<-505	The Maze II    		48.1%	Medium	->>
<<-504	Base 7    		46.4%	Easy	->>
<<-503	Next Greater Element II    		57.5%	Medium	->>
<<-502	IPO    		40.9%	Hard	->>
<<-501	Find Mode in Binary Search Tree    		42.9%	Easy	->>
<<-500	Keyboard Row    		65.3%	Easy	->>
<<-499	The Maze III    		41.8%	Hard	->>
<<-498	Diagonal Traverse    		48.9%	Medium	->>
<<-497	Random Point in Non-overlapping Rectangles    		39.0%	Medium	->>
<<-496	Next Greater Element I    		64.7%	Easy	->>
<<-495	Teemo Attacking    		56.0%	Medium	->>
<<-494	Target Sum    		46.0%	Medium	->>
<<-493	Reverse Pairs    		26.0%	Hard	->>
<<-492	Construct the Rectangle    		49.9%	Easy	->>
<<-491	Increasing Subsequences    		46.9%	Medium	->>
<<-490	The Maze    		52.4%	Medium	->>
<<-489	Robot Room Cleaner    		71.5%	Hard	->>
<<-488	Zuma Game    		39.2%	Hard	->>
<<-487	Max Consecutive Ones II    		47.9%	Medium	->>
<<-486	Predict the Winner    		48.3%	Medium	->>
<<-485	Max Consecutive Ones    		53.5%	Easy	->>
<<-484	Find Permutation    		63.9%	Medium	->>
<<-483	Smallest Good Base    		36.0%	Hard	->>
<<-482	License Key Formatting    		43.0%	Easy	->>
<<-481	Magical String    		47.8%	Medium	->>
<<-480	Sliding Window Median    		38.1%	Hard	->>
<<-479	Largest Palindrome Product    		29.3%	Hard	->>
<<-478	Generate Random Point in a Circle    		38.8%	Medium	->>
<<-477	Total Hamming Distance    		50.5%	Medium	->>
<<-476	Number Complement    		65.0%	Easy	->>
<<-475	Heaters    		33.4%	Medium	->>
<<-474	Ones and Zeroes    		43.3%	Medium	->>
<<-473	Matchsticks to Square    		37.9%	Medium	->>
<<-472	Concatenated Words    		44.9%	Hard	->>
<<-471	Encode String with Shortest Length    		48.4%	Hard	->>
<<-470	Implement Rand10() Using Rand7()    		45.8%	Medium	->>
<<-469	Convex Polygon    		37.2%	Medium	->>
<<-468	Validate IP Address    		24.6%	Medium	->>
<<-467	Unique Substrings in Wraparound String    		35.9%	Medium	->>
<<-466	Count The Repetitions    		28.5%	Hard	->>
<<-465	Optimal Account Balancing    		47.6%	Hard	->>
<<-464	Can I Win    		29.4%	Medium	->>
<<-463	Island Perimeter    		66.3%	Easy	->>
<<-462	Minimum Moves to Equal Array Elements II    		54.1%	Medium	->>

<<-461	Hamming Distance    		73.0%	Easy	->>
<<-460	LFU Cache    		35.1%	Hard	->>
<<-459	Repeated Substring Pattern    		43.1%	Easy	->>
<<-458	Poor Pigs    		54.2%	Hard	->>
<<-457	Circular Array Loop    		29.7%	Medium	->>
<<-456	132 Pattern    		30.5%	Medium	->>
<<-455	Assign Cookies    		50.2%	Easy	->>
<<-454	4Sum II    		53.8%	Medium	->>
<<-453	Minimum Moves to Equal Array Elements    		50.5%	Easy	->>
<<-452	Minimum Number of Arrows to Burst Balloons    		49.7%	Medium	->>
<<-451	Sort Characters By Frequency    		63.8%	Medium	->>
<<-450	Delete Node in a BST    		44.9%	Medium	->>
<<-449	Serialize and Deserialize BST    		53.5%	Medium	->>
<<-448	Find All Numbers Disappeared in an Array    		56.0%	Easy	->>
<<-447	Number of Boomerangs    		52.2%	Medium	->>
<<-446	Arithmetic Slices II - Subsequence    		33.0%	Hard	->>
<<-445	Add Two Numbers II    		55.8%	Medium	->>
<<-444	Sequence Reconstruction    		23.2%	Medium	->>
<<-443	String Compression    		42.4%	Medium	->>
<<-442	Find All Duplicates in an Array    		68.4%	Medium	->>
<<-441	Arranging Coins    		42.2%	Easy	->>
<<-440	K-th Smallest in Lexicographical Order    		29.3%	Hard	->>
<<-439	Ternary Expression Parser    		56.3%	Medium	->>
<<-438	Find All Anagrams in a String    		44.3%	Medium	->>
<<-437	Path Sum III    		47.7%	Medium	->>
<<-436	Find Right Interval    		48.2%	Medium	->>
<<-435	Non-overlapping Intervals    		43.7%	Medium	->>
<<-434	Number of Segments in a String    		37.8%	Easy	->>
<<-433	Minimum Genetic Mutation    		42.6%	Medium	->>
<<-432	All O`one Data Structure    		32.9%	Hard	->>
<<-431	Encode N-ary Tree to Binary Tree    		73.9%	Hard	->>
<<-430	Flatten a Multilevel Doubly Linked List    		56.3%	Medium	->>
<<-429	N-ary Tree Level Order Traversal    		65.9%	Medium	->>
<<-428	Serialize and Deserialize N-ary Tree    		60.6%	Hard	->>
<<-427	Construct Quad Tree    		62.0%	Medium	->>
<<-426	Convert Binary Search Tree to Sorted Doubly Linked List    		60.2%	Medium	->>
<<-425	Word Squares    		49.4%	Hard	->>
<<-424	Longest Repeating Character Replacement    		47.7%	Medium	->>
<<-423	Reconstruct Original Digits from English    		47.1%	Medium	->>
<<-422	Valid Word Square    		38.0%	Easy	->>
<<-421	Maximum XOR of Two Numbers in an Array    		53.7%	Medium	->>
<<-420	Strong Password Checker    		13.7%	Hard	->>
<<-419	Battleships in a Board    		70.6%	Medium	->>
<<-418	Sentence Screen Fitting    		32.8%	Medium	->>
<<-417	Pacific Atlantic Water Flow    		41.9%	Medium	->>
<<-416	Partition Equal Subset Sum    		44.3%	Medium	->>
<<-415	Add Strings    		47.9%	Easy	->>
<<-414	Third Maximum Number    		30.6%	Easy	->>
<<-413	Arithmetic Slices    		58.3%	Medium	->>
<<-412	Fizz Buzz    		63.4%	Easy	->>
<<-	#	Title	Solution	Acceptance	Difficulty	Frequency  ->>
<<-411	Minimum Unique Word Abbreviation    		36.8%	Hard	->>
<<-410	Split Array Largest Sum    		45.7%	Hard	->>
<<-409	Longest Palindrome    		52.0%	Easy	->>
<<-408	Valid Word Abbreviation    		31.1%	Easy	->>
<<-407	Trapping Rain Water II    		43.3%	Hard	->>
<<-406	Queue Reconstruction by Height    		67.7%	Medium	->>
<<-405	Convert a Number to Hexadecimal    		44.2%	Easy	->>
<<-404	Sum of Left Leaves    		52.1%	Easy	->>
<<-403	Frog Jump    		40.6%	Hard	->>
<<-402	Remove K Digits    		28.5%	Medium	->>
<<-401	Binary Watch    		48.1%	Easy	->>
<<-400	Nth Digit    		32.3%	Medium	->>
<<-399	Evaluate Division    		53.5%	Medium	->>
<<-398	Random Pick Index    		57.1%	Medium	
reservoir sampling
->>
<<-397	Integer Replacement    		33.2%	Medium	->>
<<-396	Rotate Function    		36.5%	Medium	->>
<<-395	Longest Substring with At Least K Repeating Characters    		41.9%	Medium	->>
<<-394	Decode String    		51.8%	Medium	->>
<<-393	UTF-8 Validation    		37.8%	Medium	->>
<<-392	Is Subsequence    		49.4%	Easy	->>
<<-391	Perfect Rectangle    		30.8%	Hard	->>
<<-390	Elimination Game    		44.7%	Medium	->>
<<-389	Find the Difference    		57.5%	Easy	->>
<<-388	Longest Absolute File Path    		42.1%	Medium	->>
<<-387	First Unique Character in a String    		53.6%	Easy	->>
<<-386	Lexicographical Numbers    		52.8%	Medium	->>
<<-385	Mini Parser    		34.2%	Medium	->>
<<-384	Shuffle an Array    		53.5%	Medium	->>
<<-383	Ransom Note    		53.2%	Easy	->>
<<-382	Linked List Random Node    		52.4%	Medium	->>
reservoir sampling:
Choose k entries from n numbers. Make sure each number is selected with the probability of k/n
Basic idea:
Choose 1, 2, 3, ..., k first and put them into the reservoir.
For k+1, pick it with a probability of k/(k+1), and randomly replace a number in the reservoir.
For k+i, pick it with a probability of k/(k+i), and randomly replace a number in the reservoir.
Repeat until k+i reaches n
Proof:
For k+i, the probability that it is selected and will replace a number in the reservoir is k/(k+i)
For a number in the reservoir before (let's say X), the probability that it keeps staying in the reservoir is
P(X was in the reservoir last time) × P(X is not replaced by k+i)
= P(X was in the reservoir last time) × (1 - P(k+i is selected and replaces X))
= k/(k+i-1) × （1 - k/(k+i) × 1/k）
= k/(k+i)
When k+i reaches n, the probability of each number staying in the reservoir is k/n
Example
Choose 3 numbers from [111, 222, 333, 444]. Make sure each number is selected with a probability of 3/4
First, choose [111, 222, 333] as the initial reservior
Then choose 444 with a probability of 3/4
For 111, it stays with a probability of
P(444 is not selected) + P(444 is selected but it replaces 222 or 333)
= 1/4 + 3/4*2/3
= 3/4
The same case with 222 and 333
Now all the numbers have the probability of 3/4 to be picked
Similar problem: 398

<<-381	Insert Delete GetRandom O(1) - Duplicates allowed    		34.6%	Hard	->>
<<-380	Insert Delete GetRandom O(1)    		48.2%	Medium	->>
<<-379	Design Phone Directory    		47.4%	Medium	->>
<<-378	Kth Smallest Element in a Sorted Matrix    		55.4%	Medium	->>
<<-377	Combination Sum IV    		45.7%	Medium	->>
<<-376	Wiggle Subsequence    		39.9%	Medium	->>
<<-375	Guess Number Higher or Lower II    		41.4%	Medium	->>
<<-374	Guess Number Higher or Lower    		44.0%	Easy	->>
<<-373	Find K Pairs with Smallest Sums    		37.2%	Medium	->>
<<-372	Super Pow    		36.5%	Medium	->>
<<-371	Sum of Two Integers    		50.6%	Medium	->>
<<-370	Range Addition    		63.2%	Medium	->>
<<-369	Plus One Linked List    		58.5%	Medium	->>
<<-368	Largest Divisible Subset    		38.1%	Medium	->>
<<-367	Valid Perfect Square    		41.9%	Easy	->>
<<-366	Find Leaves of Binary Tree    		71.3%	Medium	->>
<<-365	Water and Jug Problem    		30.8%	Medium	->>
<<-364	Nested List Weight Sum II    		63.1%	Medium	->>
<<-363	Max Sum of Rectangle No Larger Than K    		38.2%	Hard	->>
<<-362	Design Hit Counter    		64.6%	Medium	->>

<<-361	Bomb Enemy    		46.4%	Medium	->>
<<-360	Sort Transformed Array    		49.3%	Medium	->>
<<-359	Logger Rate Limiter    		71.5%	Easy	->>
<<-358	Rearrange String k Distance Apart    		35.4%	Hard	->>
<<-357	Count Numbers with Unique Digits    		48.6%	Medium	->>
<<-356	Line Reflection    		32.3%	Medium	->>
<<-355	Design Twitter    		30.8%	Medium	->>
<<-354	Russian Doll Envelopes    		35.8%	Hard	->>
<<-353	Design Snake Game    		34.9%	Medium	->>
<<-352	Data Stream as Disjoint Intervals    		48.1%	Hard	->>
<<-351	Android Unlock Patterns    		49.1%	Medium	->>
<<-350	Intersection of Two Arrays II    		51.7%	Easy	->>
<<-349	Intersection of Two Arrays    		63.8%	Easy	->>
<<-348	Design Tic-Tac-Toe    		55.0%	Medium	->>
<<-347	Top K Frequent Elements    		61.9%	Medium	->>
<<-346	Moving Average from Data Stream    		72.4%	Easy	->>
<<-345	Reverse Vowels of a String    		44.6%	Easy	->>
<<-344	Reverse String    		69.5%	Easy	->>
<<-343	Integer Break    		50.8%	Medium	->>
<<-342	Power of Four    		41.4%	Easy	->>
<<-341	Flatten Nested List Iterator    		53.7%	Medium	->>
<<-340	Longest Substring with At Most K Distinct Characters    		44.7%	Hard	->>
<<-339	Nested List Weight Sum    		75.0%	Easy	->>
<<-338	Counting Bits    		70.0%	Medium	->>
<<-337	House Robber III    		51.0%	Medium	->>
<<-336	Palindrome Pairs    		34.2%	Hard	->>
<<-335	Self Crossing    		28.5%	Hard	->>
<<-334	Increasing Triplet Subsequence    		39.9%	Medium	->>
<<-333	Largest BST Subtree    		36.7%	Medium	->>
<<-332	Reconstruct Itinerary    		37.3%	Medium	->>
<<-331	Verify Preorder Serialization of a Binary Tree    		40.7%	Medium	->>
<<-330	Patching Array    		34.8%	Hard	->>
<<-329	Longest Increasing Path in a Matrix    		44.0%	Hard	->>
<<-328	Odd Even Linked List    		56.5%	Medium	->>
<<-327	Count of Range Sum    		35.7%	Hard	->>
<<-326	Power of Three    		42.0%	Easy	->>
<<-325	Maximum Size Subarray Sum Equals k    		47.0%	Medium	->>
<<-324	Wiggle Sort II    		30.4%	Medium	->>
<<-323	Number of Connected Components in an Undirected Graph    		56.9%	Medium	->>
<<-322	Coin Change    		36.5%	Medium	->>
<<-321	Create Maximum Number    		27.3%	Hard	->>
<<-320	Generalized Abbreviation    		52.9%	Medium	->>
<<-319	Bulb Switcher    		45.2%	Medium	->>
<<-318	Maximum Product of Word Lengths    		51.8%	Medium	->>
<<-317	Shortest Distance from All Buildings    		42.1%	Hard	->>
<<-316	Remove Duplicate Letters    		38.5%	Medium	->>
<<-315	Count of Smaller Numbers After Self    		42.3%	Hard	->>
<<-314	Binary Tree Vertical Order Traversal    		46.3%	Medium	->>
<<-313	Super Ugly Number    		45.6%	Medium	->>
<<-312	Burst Balloons    		52.9%	Hard	->>

<<-311	Sparse Matrix Multiplication    		63.1%	Medium	->>
<<-310	Minimum Height Trees    		34.3%	Medium	->>
<<-309	Best Time to Buy and Sell Stock with Cooldown    		47.8%	Medium	->>
<<-308	Range Sum Query 2D - Mutable    		36.7%	Hard	->>
<<-307	Range Sum Query - Mutable    		36.0%	Medium	->>
<<-306	Additive Number    		29.5%	Medium	->>
<<-305	Number of Islands II    		39.8%	Hard	->>
<<-304	Range Sum Query 2D - Immutable    		39.7%	Medium	->>
<<-303	Range Sum Query - Immutable    		46.3%	Easy	->>
<<-302	Smallest Rectangle Enclosing Black Pixels    		52.1%	Hard	->>
<<-301	Remove Invalid Parentheses    		44.1%	Hard	->>
<<-300	Longest Increasing Subsequence    		43.0%	Medium	->>
<<-299	Bulls and Cows    		44.0%	Medium	->>
<<-298	Binary Tree Longest Consecutive Sequence    		47.6%	Medium	->>
<<-297	Serialize and Deserialize Binary Tree    		48.8%	Hard	->>
<<-296	Best Meeting Point    		57.9%	Hard	->>
<<-295	Find Median from Data Stream    		45.8%	Hard	->>
<<-294	Flip Game II    		50.4%	Medium	->>
<<-293	Flip Game    		61.0%	Easy	->>
<<-292	Nim Game    		54.9%	Easy	->>
<<-291	Word Pattern II    		43.8%	Hard	->>
<<-290	Word Pattern    		38.1%	Easy	->>
<<-289	Game of Life    		56.0%	Medium	->>
<<-288	Unique Word Abbreviation    		22.5%	Medium	->>
<<-287	Find the Duplicate Number    		56.6%	Medium	->>
<<-286	Walls and Gates    		55.5%	Medium	->>
<<-285	Inorder Successor in BST    		41.7%	Medium	->>
<<-284	Peeking Iterator    		46.9%	Medium	->>
<<-283	Move Zeroes    		58.2%	Easy	->>
<<-282	Expression Add Operators    		36.2%	Hard	->>
<<-281	Zigzag Iterator    		59.0%	Medium	->>
<<-280	Wiggle Sort    		64.3%	Medium	->>
<<-279	Perfect Squares    		48.2%	Medium	->>
<<-278	First Bad Version    		36.7%	Easy	->>
<<-277	Find the Celebrity    		42.7%	Medium	->>
<<-276	Paint Fence    		38.7%	Easy	->>
<<-275	H-Index II    		36.1%	Medium	->>
<<-274	H-Index    		36.3%	Medium	->>
<<-273	Integer to English Words    		27.6%	Hard	->>
<<-272	Closest Binary Search Tree Value II    		51.3%	Hard	->>
<<-271	Encode and Decode Strings    		32.2%	Medium	->>
<<-270	Closest Binary Search Tree Value    		49.2%	Easy	->>
<<-269	Alien Dictionary    		33.5%	Hard	->>
<<-268	Missing Number    		52.7%	Easy	->>
<<-267	Palindrome Permutation II    		36.9%	Medium	->>
<<-266	Palindrome Permutation    		62.3%	Easy	->>
<<-265	Paint House II    		45.2%	Hard	->>
<<-264	Ugly Number II    		42.5%	Medium	->>
<<-263	Ugly Number    		41.7%	Easy	->>
<<-262	Trips and Users    		34.6%	Hard	->>

<<-261	Graph Valid Tree    		42.7%	Medium	->>
<<-260	Single Number III    		65.1%	Medium	->>
<<-259	3Sum Smaller    		48.4%	Medium	->>
<<-258	Add Digits    		58.1%	Easy	->>
<<-257	Binary Tree Paths    		52.6%	Easy	->>
<<-256	Paint House    		52.7%	Medium	->>
<<-255	Verify Preorder Sequence in Binary Search Tree    		45.9%	Medium	->>
<<-254	Factor Combinations    		47.0%	Medium	->>
<<-253	Meeting Rooms II    		46.3%	Medium	
problem: given a list of intervals, find the min required room.
idea: overlapped intervals need separate room. so to find max overlapped intervals.
approach: intervals, using map to mark start and ending of an event. prefix sum

->>
<<-252	Meeting Rooms    		55.1%	Easy	
check if intervals have overlaps.
approach: intervals, sort with begin and check begin with previous end.
O(N)
level: 2
->>
<<-251	Flatten 2D Vector    		46.0%	Medium	->>
<<-250	Count Univalue Subtrees    		52.7%	Medium	->>
<<-249	Group Shifted Strings    		56.9%	Medium	->>
<<-248	Strobogrammatic Number III    		39.9%	Hard	->>
<<-247	Strobogrammatic Number II    		48.1%	Medium	->>
<<-246	Strobogrammatic Number    		45.4%	Easy	->>
<<-245	Shortest Word Distance III    		55.6%	Medium	
similar to 244, but the input words could be the same and represent different positions.
using hashmap to store indices for each word, but need check two cases.
->>
<<-244	Shortest Word Distance II    		53.0%	Medium	
similar to 243, but will be called many times with different parameters
using hashmap to record the indices for each word.
->>
<<-243	Shortest Word Distance    		61.4%	Easy	
given two words find the distance (shortest)
one pass: keep updating two indices and find the difference.
O(N) if we do not consider string compare.
->>

<<-242	Valid Anagram    		57.6%	Easy	->>
<<-241	Different Ways to Add Parentheses    		56.5%	Medium	->>
<<-240	Search a 2D Matrix II    		43.6%	Medium	->>
<<-239	Sliding Window Maximum    		44.1%	Hard	->>
<<-238	Product of Array Except Self    		60.9%	Medium	->>
<<-237	Delete Node in a Linked List    		65.5%	Easy	->>
<<-236	Lowest Common Ancestor of a Binary Tree    		47.4%	Medium	->>
<<-235	Lowest Common Ancestor of a Binary Search Tree    		50.9%	Easy	->>
<<-234	Palindrome Linked List    		39.9%	Easy	->>
<<-233	Number of Digit One    		31.5%	Hard	->>
<<-232	Implement Queue using Stacks    		51.0%	Easy	->>
<<-231	Power of Two    		43.8%	Easy	->>
<<-230	Kth Smallest Element in a BST    		61.6%	Medium	->>
<<-229	Majority Element II    		38.1%	Medium	->>
<<-228	Summary Ranges    		41.7%	Easy	->>
<<-227	Basic Calculator II    		37.6%	Medium	->>
<<-226	Invert Binary Tree    		66.1%	Easy	->>
<<-225	Implement Stack using Queues    		46.4%	Easy	->>
<<-224	Basic Calculator    		37.7%	Hard	->>
<<-223	Rectangle Area    		38.0%	Medium	->>
<<-222	Count Complete Tree Nodes    		48.1%	Medium	->>
<<-221	Maximal Square    		38.1%	Medium	->>
<<-220	Contains Duplicate III    		21.3%	Medium	->>
<<-219	Contains Duplicate II    		38.3%	Easy	->>
<<-218	The Skyline Problem    		35.1%	Hard	->>
<<-217	Contains Duplicate    		56.3%	Easy	->>
<<-216	Combination Sum III    		59.4%	Medium	->>
<<-215	Kth Largest Element in an Array    		56.9%	Medium	->>
<<-214	Shortest Palindrome    		30.3%	Hard	->>
<<-213	House Robber II    		37.2%	Medium	->>
<<-212	Word Search II    		35.9%	Hard	->>

<<-211	Design Add and Search Words Data Structure    		39.2%	Medium	->>
<<-210	Course Schedule II    		41.7%	Medium	->>
<<-209	Minimum Size Subarray Sum    		38.9%	Medium	->>
<<-208	Implement Trie (Prefix Tree)    		50.9%	Medium	->>
<<-207	Course Schedule    		43.8%	Medium	->>
<<-206	Reverse Linked List    		64.1%	Easy	->>
<<-205	Isomorphic Strings    		40.1%	Easy	->>
<<-204	Count Primes    		31.9%	Easy	->>
<<-203	Remove Linked List Elements    		38.9%	Easy	->>
<<-202	Happy Number    		50.9%	Easy	->>
<<-201	Bitwise AND of Numbers Range    		39.5%	Medium	->>
<<-200	Number of Islands    		47.9%	Medium	->>
<<-199	Binary Tree Right Side View    		55.2%	Medium	->>
<<-198	House Robber    		42.6%	Easy	->>
<<-197	Rising Temperature    		39.2%	Easy	->>
<<-196	Delete Duplicate Emails    		43.3%	Easy	->>
<<-195	Tenth Line    		32.9%	Easy	->>
<<-194	Transpose File    		24.4%	Medium	->>
<<-193	Valid Phone Numbers    		25.3%	Easy	->>
<<-192	Word Frequency    		25.8%	Medium	->>
<<-191	Number of 1 Bits    		51.3%	Easy	->>
<<-190	Reverse Bits    		41.0%	Easy	->>
<<-189	Rotate Array    		36.1%	Medium	->>
<<-188	Best Time to Buy and Sell Stock IV    		28.8%	Hard	->>
<<-187	Repeated DNA Sequences    		40.9%	Medium	->>
<<-186	Reverse Words in a String II    		44.5%	Medium	->>
<<-185	Department Top Three Salaries    		36.9%	Hard	->>
<<-184	Department Highest Salary    		38.5%	Medium	->>
<<-183	Customers Who Never Order    		55.3%	Easy	->>
<<-182	Duplicate Emails    		63.5%	Easy	->>
<<-181	Employees Earning More Than Their Managers    		58.8%	Easy	->>
<<-180	Consecutive Numbers    		41.1%	Medium	->>
<<-179	Largest Number    		30.1%	Medium	->>
<<-178	Rank Scores    		48.1%	Medium	->>
<<-177	Nth Highest Salary    		32.4%	Medium	->>
<<-176	Second Highest Salary    		32.6%	Easy	->>
<<-175	Combine Two Tables    		62.6%	Easy	->>
<<-174	Dungeon Game    		32.9%	Hard	->>
<<-173	Binary Search Tree Iterator    		58.2%	Medium	
binary tree inorder traversal iterative approach using stack.
add all left into stack, pop and add all right into stack.
->>
<<-172	Factorial Trailing Zeroes    		38.1%	Easy	->>
<<-171	Excel Sheet Column Number    		56.5%	Easy	->>
<<-170	Two Sum III - Data structure design    		34.5%	Easy	->>
<<-169	Majority Element    		59.5%	Easy	->>
<<-168	Excel Sheet Column Title    		31.4%	Easy	->>
<<-167	Two Sum II - Input array is sorted    		55.0%	Easy	->>
<<-166	Fraction to Recurring Decimal    		22.0%	Medium	->>
<<-165	Compare Version Numbers    		29.7%	Medium	->>
<<-164	Maximum Gap    		36.3%	Hard	->>
<<-163	Missing Ranges    		25.1%	Easy	->>
<<-162	Find Peak Element    		43.6%	Medium	->>

<<-161	One Edit Distance    		32.5%	Medium	->>
<<-160	Intersection of Two Linked Lists    		42.0%	Easy	->>
<<-159	Longest Substring with At Most Two Distinct Characters    		49.8%	Medium	->>
<<-158	Read N Characters Given Read4 II - Call multiple times    		35.5%	Hard	->>
<<-157	Read N Characters Given Read4    		36.4%	Easy	->>
<<-156	Binary Tree Upside Down    		55.6%	Medium	->>
<<-155	Min Stack    		45.5%	Easy	->>
<<-154	Find Minimum in Rotated Sorted Array II    		41.8%	Hard	->>
<<-153	Find Minimum in Rotated Sorted Array    		45.6%	Medium	->>
<<-152	Maximum Product Subarray    		32.4%	Medium	->>
<<-151	Reverse Words in a String    		22.8%	Medium	->>
<<-150	Evaluate Reverse Polish Notation    		37.2%	Medium	->>
<<-149	Max Points on a Line    		17.1%	Hard	->>
<<-148	Sort List    		45.2%	Medium	->>
<<-147	Insertion Sort List    		43.8%	Medium	->>
<<-146	LRU Cache    		34.6%	Medium	->>
<<-145	Binary Tree Postorder Traversal    		56.4%	Medium	->>
<<-144	Binary Tree Preorder Traversal    		56.7%	Medium	->>
<<-143	Reorder List    		39.7%	Medium	->>
<<-142	Linked List Cycle II    		38.9%	Medium	->>
<<-141	Linked List Cycle    		41.9%	Easy	->>
<<-140	Word Break II    		33.7%	Hard	->>
<<-139	Word Break    		41.0%	Medium	->>
<<-138	Copy List with Random Pointer    		38.5%	Medium	->>
<<-137	Single Number II     		53.2%	Medium	->>
<<-136	Single Number    		66.1%	Easy	->>
<<-135	Candy    		32.4%	Hard	->>
<<-134	Gas Station    		40.5%	Medium	->>
<<-133	Clone Graph    		37.5%	Medium	->>
<<-132	Palindrome Partitioning II    		30.7%	Hard	->>
<<-131	Palindrome Partitioning    		49.2%	Medium	->>
<<-130	Surrounded Regions    		28.8%	Medium	->>
<<-129	Sum Root to Leaf Numbers    		50.1%	Medium	->>
<<-128	Longest Consecutive Sequence    		45.7%	Hard	->>
<<-127	Word Ladder    		30.7%	Medium	->>
<<-126	Word Ladder II    		23.0%	Hard	->>
<<-125	Valid Palindrome    		37.5%	Easy	->>
<<-124	Binary Tree Maximum Path Sum    		35.0%	Hard	->>
<<-123	Best Time to Buy and Sell Stock III    		39.3%	Hard	->>
<<-122	Best Time to Buy and Sell Stock II    		57.9%	Easy	->>
<<-121	Best Time to Buy and Sell Stock    		51.1%	Easy	->>
<<-120	Triangle    		45.0%	Medium	->>
<<-119	Pascal's Triangle II    		51.4%	Easy	->>
<<-118	Pascal's Triangle    		53.8%	Easy	->>
<<-117	Populating Next Right Pointers in Each Node II    		40.3%	Medium	->>
<<-116	Populating Next Right Pointers in Each Node    		47.7%	Medium	->>
<<-115	Distinct Subsequences    		39.1%	Hard	->>
<<-114	Flatten Binary Tree to Linked List    		50.8%	Medium	->>
<<-113	Path Sum II    		48.0%	Medium	->>
<<-112	Path Sum    		41.8%	Easy	->>

<<-111	Minimum Depth of Binary Tree    		38.8%	Easy	->>
<<-110	Balanced Binary Tree    		44.0%	Easy	->>
<<-109	Convert Sorted List to Binary Search Tree    		49.2%	Medium	->>
<<-108	Convert Sorted Array to Binary Search Tree    		59.3%	Easy	->>
<<-107	Binary Tree Level Order Traversal II    		54.4%	Easy	->>
<<-106	Construct Binary Tree from Inorder and Postorder Traversal    		48.5%	Medium	->>
<<-105	Construct Binary Tree from Preorder and Inorder Traversal    		50.5%	Medium	->>
<<-104	Maximum Depth of Binary Tree    		66.9%	Easy	->>
<<-103	Binary Tree Zigzag Level Order Traversal    		49.3%	Medium	->>
<<-102	Binary Tree Level Order Traversal    		55.7%	Medium	->>
<<-101	Symmetric Tree    		47.6%	Easy	->>
<<-100	Same Tree    		53.8%	Easy	->>
<<-99	Recover Binary Search Tree    		41.7%	Hard	->>
<<-98	Validate Binary Search Tree    		28.2%	Medium	->>
<<-97	Interleaving String    		32.2%	Hard	->>
<<-96	Unique Binary Search Trees    		53.8%	Medium	->>
<<-95	Unique Binary Search Trees II    		41.6%	Medium	->>
<<-94	Binary Tree Inorder Traversal    		64.8%	Medium	->>
<<-93	Restore IP Addresses    		36.7%	Medium	->>
<<-92	Reverse Linked List II    		39.8%	Medium	->>
<<-91	Decode Ways    		25.5%	Medium	->>
<<-90	Subsets II    		48.0%	Medium	->>
<<-89	Gray Code    		49.7%	Medium	->>
<<-88	Merge Sorted Array    		40.0%	Easy	->>
<<-87	Scramble String    		34.2%	Hard	->>
<<-86	Partition List    		42.5%	Medium	->>
<<-85	Maximal Rectangle    		38.7%	Hard	->>
<<-84	Largest Rectangle in Histogram    		36.0%	Hard	->>
<<-83	Remove Duplicates from Sorted List    		46.0%	Easy	->>
<<-82	Remove Duplicates from Sorted List II    		37.6%	Medium	->>
<<-81	Search in Rotated Sorted Array II    		33.3%	Medium	->>
<<-80	Remove Duplicates from Sorted Array II    		44.7%	Medium	->>
<<-79	Word Search    		36.2%	Medium	->>
<<-78	Subsets    		63.7%	Medium	->>
<<-77	Combinations    		56.2%	Medium	->>
<<-76	Minimum Window Substring    		35.4%	Hard	->>
<<-75	Sort Colors    		48.4%	Medium	->>
<<-74	Search a 2D Matrix    		37.1%	Medium	->>
<<-73	Set Matrix Zeroes    		43.8%	Medium	->>
<<-72	Edit Distance    		45.9%	Hard	->>
<<-71	Simplify Path    		33.3%	Medium	->>
<<-70	Climbing Stairs    		48.3%	Easy	->>
<<-69	Sqrt(x)    		34.5%	Easy	->>
<<-68	Text Justification    		28.7%	Hard	->>
<<-67	Add Binary    		46.2%	Easy	->>
<<-66	Plus One    		42.8%	Easy	->>
<<-65	Valid Number    		15.6%	Hard	->>
<<-64	Minimum Path Sum    		55.4%	Medium	->>
<<-63	Unique Paths II    		35.0%	Medium	->>
<<-62	Unique Paths    		55.1%	Medium	->>

<<-61	Rotate List    		31.3%	Medium	->>
<<-60	Permutation Sequence    		39.0%	Hard	->>
<<-59	Spiral Matrix II    		55.3%	Medium	->>
<<-58	Length of Last Word    		33.3%	Easy	->>
<<-57	Insert Interval    		34.5%	Medium	->>
<<-56	Merge Intervals    		40.2%	Medium	->>
<<-55	Jump Game    		34.9%	Medium	->>
<<-54	Spiral Matrix    		34.9%	Medium	->>
<<-53	Maximum Subarray    		47.2%	Easy	->>
<<-52	N-Queens II    		59.1%	Hard	->>
<<-51	N-Queens    		48.2%	Hard	->>
<<-50	Pow(x, n)    		30.7%	Medium	->>
<<-49	Group Anagrams    		58.2%	Medium	->>
<<-48	Rotate Image    		58.5%	Medium	->>
<<-47	Permutations II    		48.4%	Medium	->>
<<-46	Permutations    		65.2%	Medium	->>
<<-45	Jump Game II    		31.1%	Hard	->>
<<-44	Wildcard Matching    		25.1%	Hard	->>
<<-43	Multiply Strings    		34.4%	Medium	->>
<<-42	Trapping Rain Water    		50.1%	Hard	->>
<<-41	First Missing Positive    		33.1%	Hard	->>
<<-40	Combination Sum II    		49.4%	Medium	->>
<<-39	Combination Sum    		58.1%	Medium	->>
<<-38	Count and Say    		45.4%	Easy	->>
<<-37	Sudoku Solver    		45.2%	Hard	->>
<<-36	Valid Sudoku    		49.7%	Medium	->>
<<-35	Search Insert Position    		42.7%	Easy	->>
<<-34	Find First and Last Position of Element in Sorted Array    		36.7%	Medium	->>
<<-33	Search in Rotated Sorted Array    		35.3%	Medium	->>
<<-32	Longest Valid Parentheses    		28.9%	Hard	->>
<<-31	Next Permutation    		33.0%	Medium	->>
<<-30	Substring with Concatenation of All Words    		25.8%	Hard	->>
<<-29	Divide Two Integers    		16.5%	Medium	->>
<<-28	Implement strStr()    		34.9%	Easy	->>
<<-27	Remove Element    		48.8%	Easy	->>
<<-26	Remove Duplicates from Sorted Array    		46.0%	Easy	->>
<<-25	Reverse Nodes in k-Group    		43.5%	Hard	->>
<<-24	Swap Nodes in Pairs    		51.6%	Medium	->>
<<-23	Merge k Sorted Lists    		41.4%	Hard	->>
<<-22	Generate Parentheses    		64.2%	Medium	->>
<<-21	Merge Two Sorted Lists    		54.8%	Easy	->>
<<-20	Valid Parentheses    		39.4%	Easy	->>
<<-19	Remove Nth Node From End of List    		35.4%	Medium	->>
<<-18	4Sum    		34.3%	Medium	->>
<<-17	Letter Combinations of a Phone Number    		48.0%	Medium	->>
<<-16	3Sum Closest    		46.2%	Medium	->>
<<-15	3Sum    		27.4%	Medium	->>
<<-14	Longest Common Prefix    		35.8%	Easy	->>
<<-13	Roman to Integer    		56.2%	Easy	->>
<<-12	Integer to Roman    		55.6%	Medium	->>

<<-11	Container With Most Water    		51.8%	Medium	->>
<<-10	Regular Expression Matching    		27.1%	Hard	->>
<<-9	Palindrome Number    		49.1%	Easy	->>
<<-8	String to Integer (atoi)    		15.5%	Medium	->>
<<-7	Reverse Integer    		25.8%	Easy	->>
<<-6	ZigZag Conversion    		37.2%	Medium	->>
<<-5	Longest Palindromic Substring    		29.9%	Medium	->>
<<-4	Median of Two Sorted Arrays    		30.4%	Hard	->>
<<-3	Longest Substring Without Repeating Characters    		30.9%	Medium	->>
<<-2	Add Two Numbers    		34.6%	Medium	->>
<<-1	Two Sum    		45.8%	Easy	->>

<<-1480. running sum of 1d array *
tag: array, prefix sum
rating: 1 star
->>

<<-1431. kids with greatest number of candies *
tag: array
rating: 1 star
->>

<<-1470. shuffle the array *
tag: array
rating: 1 star
->>

<<-1313. decompress run-length encoded list *
trivial
tag: array
rating: 1 star
->>

<<-1512. number of good pairs **
good pairs: nums[i]==nums[j] i<j.
tag: array, count, hashmap
->>

<<-1389. create target array in the given order **
using array insert.
tag: array, insert
->>

<<-1295. find numbers with even number of digits **
- convert to string O(nlogn)
- using the input range, O(N)
tag: array, 
->>

<<-1304. Find N Unique Integers Sum up to Zero *
greedy: add +/-i pair. for odd length add a 0.
tag: array, greedy
->>

<<-1460. Make Two Arrays Equal by Reversing Sub-arrays **
- using hashmap to increase and decrease 
- convert to hash multiset and check if equal
ie. same set of numbers you can always reverse subarray to get to another permutation!
tag: array, reverse sort.
->>

<<-832. flipping an image *
first flip image horizontally and then flip each pixel.
tag: array, reverse
->>

<<-1502. Can Make Arithmetic Progression From Sequence **
- sort and check. trivial. a[i]*2==a[i-1]+a[i+1]
tag: array
->>

<<-1299. Replace Elements with Greatest Element on Right Side **
get the max from right to left.
tag: array, right to left
->>

<<-1380. Lucky Numbers in a Matrix **
lucky number: it is min in the row and max in the column
not much tricky: save each row's min and col's max.
tag: array, matrix, row and column
->>

<<-1051. height checker *
trivial: sort and compare. or using lmax and rmin for O(N).
tag: sorted array.
->>

<<-1491. Average Salary Excluding the Minimum and Maximum Salary *
trivial
->>

<<-1002. Find Common Characters **
count and get the min. 
- a char to string we shall use string(1,'a'+i)
->>

<<-1160. Find Words That Can Be Formed by Characters **
similar to 1002, using vector<int> for histogram.
tag: array, hashmap
->>

<<-509. fibonacci number *
- using O(1) space and O(N) time. dp
tag: array, recurrence
->>

<<-1385. Find the Distance Value Between Two Arrays **
number of elements in arr1 which |i-j|>d.
- O(MN) approach is straightforward.
- sort B and using binary search O(NlogM)
tag: array, two arrays.
->>

<<-1200. Minimum Absolute Difference **
all numbers are different, need find all pairs with min difference
sort and find the min diff.
tag: sorted array, difference.
->>

<<-1413. Minimum Value to Get Positive Step by Step Sum **
problem statement is not clear, but it is equivalent to prefix sum and get the min.
Note it needs positive result.
tag: array, prefix sum
->>
<<-766. Toeplitz Matrix *
the key is check m[i][j]==m[i-1][j-1].
tag: matrix, loop diagonal.
->>

<<-1394. Find Lucky Integer in an Array **
value==its frequency, return the largest lucky number.
using map to count and then found the first lucky number from largest. (heap)--O(nlogn)
tag: array, hashmap
->>

<<-867. Transpose Matrix *
cannot do it in-place, we need a new matrix (non-square)
tag: matrix, transpose.
->>

<<-985. sum of even numbers after queries *
simple math.
tag: array, math
->>

<<-1260. shift 2d grid
right rotate, or using %
tag: matrix, rotation
->>

<<-189. Rotate Array
right rotate for 1d array. O(1) space.
using reverse...
tag: array, rotation
->>

<<-566. reshape matrix
simple index calculation
tag: 2d array, representation
->>

<<-1331. rank transform of an array.
rank start from 1, each unique number has a different rank, larger number has larger rank. same number has same rank.
vector->set->map
tag: array, set, map
->>

<<-888. fair candy swap
swap one pair and get the same amount of candies. 
brutal force: target (suma-sumb)/2, and check each pair.
tag: array, math
->>

<<-896. monotonic array
- using is_sorted
- all 1 or all 0, we can use two bits to record the status, using &
tag: sorted array
->>

<<-283. move zeros
move zeros to end.
- two pointer.
tag: array, two pointer, manipulation.
->>

<<-122. best time to buy and sell stack II
simple for any number of transactions.
tag: array, continue or discontinue, dp
->>

<<-118. Pascal's Triangle
generate the triangle.
- left and right is 1.
- each layer has same number as its index (1-based)
- m[i,j]=m[i-1,j-1]+m[i-1,j]
- you can think it as a dp.
tag: array, dp
->>

<<-119. Pascal's Triangle II ***
find the kth row.
using only O(K) space.
- dp: to use previous row and remove i-1 dimension, we need reverse loop on j.
tag: array, dp, reverse loop to use previous value.
->>

<<-661. Image smoother
trivial, do use a separate matrix.
tag: matrix, convolution filter
->>

<<-830. Positions of Large Groups **
find all same letter subarray >=3 start and end index.
- common mistake, forget to process the last segment. By avoid that we can add a invalid char at the end.
tag: array, search pattern
->>

<<-1018. Binary Prefix Divisible By 5 **
- only thing: number will not fit in an integer, need %5 each round.
tag: array, modular
->>

<<-628. Maximum Product of Three Numbers *
greedy: sort: the last 3 or the first two and the last one. (negative*negative->positive)
tag: array, math
->>

<<-1232. Check If It Is a Straight Line **
simplify the slope using dx and dy.
tag: array, gcd
->>

<<-989. Add to Array-Form of Integer *
reverse and perform add and carrier flag
->>

<<-66. plus one *
exactly the same as 989
->>

<<-1476. subrectangle queries *
update a subrect
- approach 1: update the values
- approach 2: save all the updates and the latest updates is the final. non-overlapped using previous updates.
->>

<<-41. first missing positive ***
values are not bound by the length.
- use value as index (-1), ignoring those values out of range.
- swap the value to its correct position
- out of range values are ignored.
- second pass check the first one !=i+1
->>

<<-565. array nesting ***
use the value as index, and goes forward, stop until see a duplicate. find the longest chain.
- similar to dfs, with a visited array. try all possible starting point.
->>

<<-448. find all numbers disappeared in an array ****
value as index to form a linked list.
The idea is index covers all the number, but some numbers are missing and some appeared more than once.
- idea: using value as index and mark visited, non-visited are the missing numbers.
tag: array, value as index
rating: 3 star
->>

<<-442. find all duplicates in array ***
values as index and mark seen as negative. if we see a negative, we know it is seen before,
tag: array, value as index
rating: 3 star
->>

<<-287. find the duplicate number ***
n+1 integers from 1 to n, we must have one element appear more than once.
- binary search: given a number (mid), count the number of elements <=mid. If it >mid, we know that duplicate is [1,mid] else it is in [mid,n]
- consider it as a linked list, it will form cycles. the entry point is the duplicate. fast/slow pointer.
rating: 4 star.
->>

<<-457. circular array loop ***
element value is the jump distance and direction. positive jump ahead, negative: back.
check if there is a cycle 
using fast and slow pointer to detect cycle.
->>

<<-689. max sum of 3 non-overlapping subarrays ****
- get the lmax and rmax for subarray sum. and sliding the window in the middle.
- also similar to prefix/postfix but using min/max operator
->>

<<-915. partition array into disjoint intervals ***
partition into left and right, max(left)<min(right)
return the min length of the left.
- lmax and rmin
->>

<<-795. Number of Subarrays with Bounded Maximum ***
return the number of subarray whose max is in the range [L,R].
disjoint intervals.
- dp. appending or discard. 
If A[i]<L can only append to previous dp[i]=dp[i-1]
if A[i]>R, we shall throw it dp[i]=0
A[i] in range [L,R], dp[i]=i-prev prev is previous invalid position 
- two pointer: left and right, defines the valid region. A[i]>R, update the left, A[i]<L,update the right.
tag: array, count, two pointer
->>

<<-16. 3sum closest ***
unsorted input. find three integers whos sum is closest to target.
- using three pointer to reduce O(N^3) to O(N^2)
- sort since we do not care about which numbers. for sorted array can use two pointer.
->>

<<-11. Container With Most Water ****
a container height is determined by the lower.
to reduce the width, we must increase the height.
->>

<<-941. Valid Mountain Array **
one from left one from right, do separately
i==j and i shall not be the first or the last.
->>

<<-1437. Check If All 1's Are at Least Length K Places Away **
tag: two pointer, previous 1's position and current 1's position
rating: 2 star
->>

<<-238. product of array except itself **
tag: prefix and postfix product
rating: 3 star
->>

<<-54. Spiral Matrix ***
return elements in spiral order.
use 4 pointer: top,bottom,left,right four pointers
->>

<<-59. spiral matrix II.
generate the nxn square matrix from 1 to n^2 in spirial order.
same as 54
->>

<<-885. Spiral Matrix III ****
starting at (r0,c0), first goes right, then down, left and up in spiral order to visit all elements
return the visit order. You can go out of boundary.
the steps are 1,1,2,2,3,3,4,4,....
coding is tricky: 
- rotating can use dx, dy conversion.
- [1,1,2,2,3,3,...]-->n/2+1, this we loop n from 0,1,2,...
- on each direction we need check each step..
- rotation clockwise, x,y->y,-x.
->>

<<-1208. Get Equal Substrings Within Budget **
convert s[i] to t[i] cost |s[i]-t[i]|, given max cost, return the longest substring to be able to convert.
- slding window using two pointer.
- cost<max, advance right.
- cost>max, keep advance left.
->>

<<-1509. Minimum Difference Between Largest and Smallest Value in Three Moves **
change at most 3 numbers to any value to min the difference between max and min.
equivalent: discard 3 numbers and get the min difference. sort them and slide window using n-3.
->>

<<-1052. Grumpy Bookstore Owner **
owner grumpy, customer is not satisfied at the minute.
owner can mask X minute so that customers are all satisfied in X min.
approach: 
- get the satisifed customer number
- moving window with X and unmask those not satisifed customer in window. and find the max window.
rating: 3
->>

<<-485. max consecutive ones. **
two pointer: expand j and contract i and get the window satisfying the condition
->>

<<-1343. Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold **
- fixed window sliding
tag: sliding window, fixed
rating: 2 star
->>

<<-835. Image Overlap ****
using 2d sliding window
- x and y has 2N positions,-->O(N^4)
- fill A in a large matrix and move B mask in the big array, still O(N^4)
- those 0's does not contribute. compress it to 1d array.
- convolution..
1d approach:
- compress, save the 1's index from A to LA, B to LB
- sliding: each index in A can overlap with another index in B, provide the offset is i-j.
- so we accumulate each index difference and the max is the answer.
A = [[1,1,0],[0,1,0],[0,1,0]]-->LA=[0,1,4,7]
B = [[0,0,0],[0,1,1],[0,0,1]]-->LB=[4,5,8]
the difference of index:
[-4,-5,-8,-3,-4,-7,0,-1,-4,3,3,-1], -4 appears 3 times.
->>

<<-1493. Longest Subarray of 1's After Deleting One Element ****
you may delete one 1 or one 0.
equivalent: to find the longest window containing at most one 0. answer is len-1
approach: 
- keep counting if it is 1, if it is 0, remember the position and see if we have previous 0
- three pointer: i the previous 0 position, pre the curr 0's position, j the current element position
->>

<<-1184. distance between bus stops *
- two way to start to end.
- mod operator
->>

<<-1366. Rank Teams by Votes ***
m teams, n votes for each team, sort the team
same vote keeps comparing, if tie, sort alphabetically.
- collect the votes for each team each place. [m][26] m<=26, m is the place.
- input is a 2d matrix form mxn.
- count along column to get each team's vote number.
- add the team name (index) to the array -> [m][27]
- sort by comparing until the last one compare by name
tag: array, sort, 2d.
rating: 4
->>

<<-1333. Filter Restaurants by Vegan-Friendly, Price and Distance **
[id,rating,vegfriendly, price, distance]
find restaurants within price and distance, ordered by rating.
- get those indices satisying the requirement (pay attention to veg friendly)
- sort using lambda function using rating first and then id.
->>

<<-1471. The k Strongest Values in an Array ***
sort by |a-m| m is the median.
- sort first to get the median, and then sort by |a-m| (compare class with parameter m).
lambda function [=] capture by value, [&] capture by reference
using this we can use m in lambda function (without a comparator class)
- partial_sort (using heap with only k numbers) nth_element
- using two pointer after sort (since the smallest and largest will be the one with largest difference)
rating: 4
->>

<<-969. pancake sort **
description: reverse the first k (you choice) and sort the array.
greedy: choose the max element and flip it to top and then flip again to its place
note A is the permutation of 1...n. we can use it so the maximum is known.
tag: array, reverse, sort.
rating: 3 star
->>

<<-167. two sum II-input is sorted **
two pointer from each end.
->>

<<-1233. Remove Sub-Folders from the Filesystem ****
- remove all child folder
brutal force: sort by length, sub-folder must be longer, so only check with shorter ones, O(MN^2) M is string length, N is array size. TLE.
Trie: trie to store each level string O(MN)
optimization: sort by string, not by size, then same prefix will be grouped together, we only need to check the last one in the result. avoid the inner loop.
tag: string sort.
rating: 4 star.
->>

<<-74. Search a 2D Matrix **
sorted in 1d.
- binary search using 1d array.
->>

<<-240. Search a 2D Matrix II **
sorted along row and col direction.
- approach 1: start from bottom left, O(M+N)
- approach 2: for each line using binary search O(MlogN)
->>

<<-1409. Queries on a permutation with key ***
query[i] move the element at query[i] to the beginning 
- approach 1: brutal force using vector as hashmap to record value vs index 
- approach 2: binary index tree.
tag: array, value vs index, hashmap
->>

<<-381. insert delete getRandom O(1), duplicate allowed. ***
- store value vs indices in hashmap. (vector will not work since it will not be sorted after the operations)
- pick the first index and swap with the end. update the hashmap.
->>

<<-1157. Online Majority Element In Subarray ***
given a range [L,R], check in the subarray if there is a majority element appear >=threshold times.
approach: store value vs all its indices.
for the range, we binary search each value's appearance in the range.
kindly brutal force.
- optimization occurs looping all values using random pick.
- find the majority element using voting algorithm (since threshold>half the length).
->>

<<-1074. Number of Submatrices That Sum to Target ****
- first try all submatrices combinations and sum reduce to a row. 
- use 1d subarray sum=target to find the number of subarrays.
tag: Kadane's algorithm
->>

<<-1488. Avoid Flood in The City ***
rain[i]>0 means on ith day, fill the lake rain[i].
=0, dry day, and you can choose a full lake to dry it.
if rains on a full lake, there will be a flood.
returns an array of action: -1 for rainy day. if dry a lake put the lake number. (You can put any number for that).
[1,2,0,0,2,1]->[-1,-1,1,2,-1,-1]
[1,2,0,1,2]->[] will flood.
idea:
- we store the dry days in front of each.
- when we see a lake to flood, we use a dry day to empty it. (the dry day must be after it).
similar to two pointer
->>

<<-1169. invalid transaction ****
invalid: 
- amount>1000. transaction: {name,time,amount,city}
- same user, different city in 60 mins
data structure: each user: a list of transaction sorted by time.
unordered_map<string,map<int,pair<int,string>>>
- loop over these components (compare with previous) not very convenient
- need to convert back again to string.
better use unordered_map<string,vector<vector<string>> note input is already sorted by time!
->>

<<-1177. can make palindrome from substring **
replace at most k characters to other chars in the range [L,R].
- save each char's index in hashmap.
- count each char's occurrence in the range.
- only one odd is allowed.
- replace one char from odd group will eliminate two odd groups. (greedy)
->>

<<-1386. cinema seat allocation. **
10 seats, left and right will not consider. left 3, mid 4, right 3. faimily of 4 shall sit in the mid.
left 2+mid left 2, mid 4, mid right 2+ right 2.
- one row mostly 2 families. 2*n.
- check if the row only contains one faimly -1, zero family -2.
- using bitset (ignoring left right) 8 bits.
- using hashtable to record the status.
->>

<<-274. H-Index **
very confusing. H index h: if h paper have at least h citations, and N-h papers has no more than h citations each.
- bucket sort, >=n put in the (n+1)th bucket [0 to n]
- count the number and the first count>=i is the answer.
count sort or bucket sort is O(N).
->>

<<-1146. snapshot array **
->>

<<-1 two sum *->>

<<-1497. Check If Array Pairs Are Divisible by k
count mod k values.
negative values we can use (i-i/k*k+k)%k. it works for both positive and negatives.
->>

<<-18. 4 sum **
using 3 pointer
->>

<<-15. 3 sum **
using two pointer
->>

<<-380. insert delete getrandom O(1) **
array and hashmap to record the array element position (array)
->>

<<-1346. Check If N and Its Double Exist *
check if 2*n or n/2 (n must be even) exist
->>

<<-217. contain duplicate *
using hashset to eliminate duplicates
->>

<<-219. Contains Duplicate II *
same and index difference <=k. using hashmap
->>

<<-840. Magic Squares In Grid *
vaid square: rows, cols, diag sum the same, and contains 1-9 only.
->>

<<-914. X of a Kind in a Deck of Cards **
hashmap to record the number, and then get the gcd for all the numbers.
important: gcd for more than 2 numbers.
->>

<<-1010. Pairs of Songs With Total Durations Divisible by 60 **
using i%60 as the key and add previous matched elments.
->>

<<-1128. Number of Equivalent Domino Pairs **
- domino (a,b)==(b,a), so we can swap them in sorted. and then convert (a,b) to a*10+b and then use hashmap to count.
->>

<<-697. degree of an array ****
degree of an array is the max frequency. find the shortest subarray with same degree.
- store into hashmap, with each index. size will be the frequency.
- find those with max freq and the difference between last and first index will be the shortest subarray
->>

<<-1275. Find Winner on a Tic Tac Toe Game ***
- using separate array for row, col, diag, diag1 is not convenient
- use separate array for A and B is more convenient.
A[8] and B[8]
->>

<<-532. k-diff pairs in an array ****
unique diff pairs (duplicate). abs(diff)=k.
k=0: more than 1 occurrance only contribute one pair
k>0: only count +k or -k once, do not count both.
->>

<<-1329. Sort the Matrix Diagonally ***
diagonal: (i,j),(i+1,j+1)....we can use i-j as the key and group them.
the starting point is the top row and left col.
sort and then fill.
tag: hashmap
rating: 3 star
->>

<<-1222. Queens That Can Attack the King **
- do not do each direction separately, use a loop! (just avoid 0,0)
- use hashmap or bool matrix to record the queen's position
->>

<<-1442. Count Triplets That Can Form Two Arrays of Equal XOR ****
find triplet i<j<=k, so that arr[i]^...arr[j-1]==arr[j]^...arr[k]
very good question. for triplets we generally need two hashmaps.
brutal force: O(N^3)
using prefix xor: for each j check left and right O(N^2)
O(N):
- xor similar to sum, we can use prefix xor.
- equivalent: from i to k, we get xor=0 (since k>i so we at least have 2 elements included)
- once we found [i,k], what would be the number of triplets? (j can from i+1 to k, which is k-i)
- however the relation is not easy:
   - assuming current prefix we know number of same prefix (from hashmap), for example at i1,i2,i3
   - then we can get: k-i1+k-i2+k-i3--->k*3-(i1+i2+i3)-->
   - we can have two hashmap, one is the histogram for xor value, one is for accumulate the i.
tag: array, triplet, hashmap, 3 pointer
rating: 5 star
->>

<<-1395. count number of teams ****
i<j<k find triplets increasing or decreasing.
- brutal force O(N^3) try all triplets
- optimized: for each element count its left larger and smaller, right smaller and larger.
- problems for triplet often can be approached by left and right parts.
tag: array, triplet.
rating: 3 star
->>

<<-1338. Reduce Array Size to The Half *
brutal force: hashmap count and then convert to vector and sort. >=(n+1)/2 to make sure odd is also fine.
->>

<<-1296. Divide Array in Sets of K Consecutive Numbers ****
check if we can divide the array into groups, each group has K consecutive numbers.
- hashmap to record the occurences.
- apparently the one appears the most determines the number of group.
- just like a mountain, each time we bring down a layer with k elements with the highest (top K)
- we can use pq to do this.
- the problem using this approach: we do not know left or right to include.

another approach from smallest:
we start from the min, and remove one occurrence min,min+1,...min+k-1
and repeat this. each time we remove the min.
using map to do this.
this approach does not have two direction dilemma.
tag: greedy, heap, hashmap.
rating: 4
->>

<<-1481. Least Number of Unique Integers after K Removals **
greedy: remove from min occurrance integers.
tag: array, hashmap,greedy
rating: 2
->>

<<-974. subarray sum divisible by k. **
- prefix sum to get subarray sum also do the mod.
- mod and get 0 to k-1. then same remainder will be divisible by k.
->>

<<-560. Subarray Sum Equals K **
- same as 974. be sure to add 0 into the hashmap mp[0]=1;
->>

<<-1508. Range Sum of Sorted Subarray Sums **
description: get all subarray sum and sort, and get the range sum on the sorted subarray sum.
brutal force: using prefix sum. 
->>

<<-1375. bulb switch III ***
only previous bulb all turned on, all turn to blue. find all time when lights are all blue
approach: prefix sum (overflow fixed by minus 1,2,3,4....)
 similar question: reverse subarray to get sort.
 ->>

<<-1529. Bulb Switcher IV **
choose i and flip bulbs form i to end.
min flips to reach target array.
- greedy from left to right.
- if current is different from the flips (odd even), then flip.
->>

<<-73. set matrix zeros **
set its row and column to all zero if A[i,j]=0
->>

<<-75. sort colors ***
in-place using 3 pointers.
three pointer generally uses the mid pointer as the main.
->>
<<-289. Game of Life **
matrix with 1,0, eight neigbors:
- live cell with <2 live ->die
- live cell with 2 or 3 lives ->live
- live cell with >3 lives ->die
- dead cell with 3 lives -> live
using highbyte to store next status.
->>
<<-48. Rotate Image ***
approach: first inverse upside down, then swap up triangle with down triangle.
rating: 3
->>
<<-1089. Duplicate Zeros ****
two pointer: i advance each element, j advance 2 if A[i]==0, when j>=n ends
then we come back from right to end and assign each element.
->>
<<-27. Remove Element **
remove all instance of val in-place
idea: two pointer, bring all those not val to nums[i++]
->>
<<-26. Remove Duplicates from Sorted Array *
almost the same as 27, but duplicates must appear once.
attention: corner cases.
->>
<<-80. Remove Duplicates from Sorted Array II **
remove so that all duplicate appears no more than twice.
use two pointer. 
num[j]>num[j-2] used to determine if they are duplicate.
->>
<<-88. Merge Sorted Array *
A has enough space, merge A and B into A in-place.
in-place: three pointer from right.
->>
<<-268. Missing Number ***
n unique numbers from 0 to n, find the missing one.
using 1 to n to xor them, all appeared are eliminated.
->>
<<-4. median of two sorted array ****
- binary search divide into 4 parts and then reduce the problem size.
- findkth..
->>
<<-378. Kth Smallest Element in a Sorted Matrix *****
sorted along row and col.
- using heap and bfs like approach. (avoid add the same element!) O(mn). stop when we have k elements.
- binary search and count, treat it like 1d. given a target we can easily find number <it. O(mlog(Max-Min))
- another heap: put the first row or first col into the heap and then expand based on it.
->>
<<-719. Find K-th Smallest Pair Distance
size is 10^5-->O(N^2) will TLE, at least O(nlogn)
- sort it. we know the min diff is 0 and max diff is last-first.
- can we do binary search? the diff from 0 to last-first, and there are duplicates.
- given a mid diff, we need to count the number of pairs <=mid.
- counting is still O(N^2), can we optimize it? using two pointer to reduce to O(N).

another method:
- assuming we have a difference matrix (too large we need to calculate it on fly). it is sorted in row and col directions.
- use 378 find kth smallest element in a sorted matrix.
->>
<<-34. find first and last position of element in sorted array. ***
- equal_range
- binary search for the first and last.
left biased and right biased.
->>
<<-792. Number of Matching Subsequences ***
Given string S and a dictionary of words words, find the number of words[i] that is a subsequence of S.
string up to 5*10^4, number of strings 5*10^3

each check will take O(N)->O(MN) will TLE.
idea: store each char's index and using binary search to find each char ->O(MlogN)
->>
<<-900. RLE Iterator **
running length encoding
- brutal force: keep the original input and a count, then loop from each to determine which element
- use prefix and binary search--need use long for all to avoid overflow.
tag: linear search, binary search
rating: 2
->>
<<-782. Transform to Chessboard
convert the 01 matrix by swapping any two rows or any two columns.
get the min number of swaps.
- each row or col number of 0 and number of 1 shall not differ more than 1.
- neighboring rows shall have opposite row1^row2=0b11111..., that is the rule for swapping rows
- swapping cols does not change above property.
so we only have two types of rows.
first count, the two type shall not differ by 1. (odd, even N and we know the first row).
even: we have two options, type 1 on first row, type 2 on first row. choose the min moves.
so it reduces to make row 1 in our desired forms.
->>
<<-954. Array of Doubled Pairs
array the input to pairs so that A[2*i+1]=2*A[i]
- greedy: sort. divide into negative and positive part.
- start from the smallest (negative /2 and positive *2)
->>
<<-775. global and local inversion
A[i]>A[i+1]: local inversion (A is permutation of 0 to n-1. size=n)
A[i]>A[j] i<j: global inversion
check if global inversion=local inversion.
- global inversion is also local inversion
- count local inversion is simple.
- all global inversion is local inversion if true.
that is: no A[i]>A[j] with i+2<=j. 
or from sorted array, how can we produce global=local case? we can only swap neighboring elements.
so abs(A[i]-i)<=1 must meet.

->>
<<-670. Maximum Swap
swap two digits at most once to get the max number
greedy: from left to right find the first sorted pair. swap with its right max.
->>
<<-1144. Decrease Elements To Make Array Zigzag
each move decrease one element by 1.
return the min number of moves needed.
- we have two options: make odd indexed elements smaller or make even indexed elements smaller.
for odd smaller: we can only increase even indexed elements (since increase odd will make it even harder).
element shall be lower than its two neighbors.
->>
<<-945. min increment to make array unique
sort and increase.
->>
<<-870. advantage shuffle
->>
<<-1007. Minimum Domino Rotations For Equal Row
two rows, min number of swap so that either A or B is the same.
- keep A[0], swap all the same
- keep B[0], swap to get all the same.
->>
<<-1503. last moment before all ants fall out of a prank
collision equivalent to not change direction.
so we just need to find the leftmost right direction ant and rightmost left direction ant.
->>
<<-1040. Moving Stones Until Consecutive II
move ending position stones to inside. until all consecutive.
ie: we are shrinking the stones into empty spaces
max steps and min steps.
greedy: 
- min steps-->equivalent to find the window with min empty spaces. (window size is the number of piles)
- max steps-->equivalent to find the window with max empty spaces.


->>
<<-667. Beautiful Arrangement II
use 1 to n, arrange so that |a1-a2|,|a2-a3|....|an-1-An| has exactly k distinct integer
approach:
the difference can be k,k-1,....1,1,1,1...
one way is: k+1,1,k,2,k-1,3...1,k+1
- it is tricky: two pointer i=1,j=k+1 
- we can also use i=1,j=n, and remaining are in order.
rating： 4
->>
<<-769. Max Chunks To Make Sorted
elements are distinct.
idea: greedy + two end max/min. (note the elements are from 0 to n-1.)
a sorted array must have left max<right min.
- simplified to left max==cur. we got a segment.
->>
<<-768. Max Chunks To Make Sorted II
elements are not distinct. and value are not limited [0,n-1].
approach 1: prefix sum, when they equal, we got one section. Similar to 1375 bulb switch III.
approach 2: using lmax and rmin, when they equal we add one section
similar problem 1375.
 ->>
<<-1414. Find the Minimum Number of Fibonacci Numbers Whose Sum Is K
f1=f2=1, fn=fn-1+fn-2
approach: 
- the fib series covers all number range, so any number can be decomposed into several in it.
- min number of fib number: greedy choose the max one <=N, and then find N-m.
- using simple O(1) dp to get a<=k and b>k then reduce to 1+minFib(k-a)
tag: greedy, recursive.
rating: **
->>
<<-1013. Partition Array Into Three Parts With Equal Sum ***
- first tsum%3==0
- second we add to target and get one group
- when number of groups>=3 it means we can do it (why >= > is for target=0)
->>
<<-717. 1-bit and 2-bit Characters ***
0 is represented as 0, 1 represented as 10 or 11.
starting from the back:
- 1110, odd number of 1s, must company with a 0
- count 0 and 1, when seen 1 and then meet a 0, stop.
- even number of 1s: true
- odd number of 1s: must have more than one zero.
->>
<<-849. Maximize Distance to Closest Person ***
left-0, n-1-right, and max segment in the middle/2
->>
<<-605. Can Place Flowers ***
greedy: add a 0 at the beginning and ending to avoid inconvenience
we need to have at least 3 0s to have a flower.
it seems easy but a bit tricky.
->>
<<-1170. Compare Strings by Frequency of the Smallest Character **
f(s)=smallest char's freq. 
find number of f(w)>f(s) in array B.
->>
<<-581. shortest unsorted continuous subarray ***
approach 1: compare to sorted, get left and right point
approach 2: using sorted property get the left and right using max and min.
->>
<<-665. non-decreasing array ****
modifying at most one element to make the array non-decreasing.
approach: if we see a drop, ie. nums[i]<nums[i-1], we have two options:
- if reduce nums[i-1] and make it sorted, we only need one. num[i-1]->num[i]
- if we cannot reduce num[i-1], (nums[i-2]>num[i]) then we need increase num[i]->num[i-1]
->>
<<-1266. min time visiting all nodes ***
from one point to another point, you can go horizontally,vertically,diagonally
min time is max(abs(dx),abs(dy))
->>
<<-561. Array Partition I ***
leng=2n, make it n pairs and maximize the sum of min of each pair.
-greedy: sort and choose the first.
coding is really trivial, but need rough math proof.
a<b<c<d, a+c>a+b.
->>
<<-891. Sum of Subsequence Widths
sequence width=max-min.
- order does not matter, we can sort if it brings good.
- each element can be min or max. in sorted array, min, we need see right, max we need see left.
- A[i] as max, then left 0 to i-1 can be chosen or not chosen, from 000..0 to 11...1, 2^i combinations.
- note A[i] as min is included for max at j>i.
- A[i] as max: dp[i]=(A[i]-A[i-1])*2^0+(A[i]-A[i-2])*2^1+......(A[i]-A[0])*2^(i-1)
dp[i]=A[i]*(2^i-1)-[A[i-1]+A[i-2]*2^1+....+A[0]*2^(i-1)]
dp[i+1]=A[i+1]*[2^(i+1)-1]-[A[i]+A[i-1]*2^1+.....+A[0]*2^i]
dp[i+1]-2*dp[i]=A[i+1]*(2^(i+1)-1)-A[i]*(2^(i+1)-2)-A[i]
dp[i+1]-2*dp[i]=(A[i+1]-A[i])*(2^(i+1)-1)
still too complicated.
we only need to count current number as min, 2^i, as max 2^(n-i-1).
min shall be negative and max shall be positive.

->>
<<-713. Subarray Product Less Than K
input are all positive integers. count all the subarrays whose product <k.
input size up to 5*10^4 and value <1000.
- O(N^2) will not pass, and prefix product will overflow.
- ending with element j. if i to j product <k, then we have i,i+1,...j (j-i+1 subarrays).
- sliding window using two pointer: if product >=k, then we divide left++
similar to sum.
- the left may need to move more than 1. using while instead of if
- corner case k<=1.
->>
<<-209. Minimum Size Subarray Sum
min size subarray sum>=k. input are all positive.
sliding window using two pointer
->>
<<-621. task scheduler
arrange the max frequency task first.
->>
<<-1053. previous permutation with one swap.
get smaller previous permutation. (make it smaller).->>
<<-31. next permutation
->>
<<-57. insert interval
insert an interval to sorted non-overlapped
- keep left
- merge middle
- keep its right.
->>
<<-228. Summary Ranges
merge contiguous numbers into range. input is sorted.
- when we see a element, add back to the answer.
- if it is connected to previous, we update the back.
- this avoid the mistake that we forget the last interval
->>
<<-56. Merge Intervals
a list intervals, merge them.
idea: sort by start, and extend the ending.
similar to 228. first to add into answer back and keeps updating to avoid the mistake.
->>
<<-1465. Max area of a piece of cake after horizontal and vertical cuts
maxh*maxw.
->>
<<-611. valid triangle number
->>
<<-495. Teemo Attacking
if attacks, last for duration time.
- there is overlap need to be subtracted.
- before using it, we compare to previous end and subtract the overlap
- add the duration and update the end
tag: array, merge interval
rating: 3

->>
<<-1287. elements appearing more than 25% in sorted array **
the candidate must be on n/4,n/2 or n*3/4. using equal_range to get the number of occurrance.
->>
<<-169. Majority element ***
find the element appear more than n/2 times. need O(1) space.
voting algorithm, different use -1, and same use +1.
->>
<<-229. Majority Element II
find the element appear more than n/3 times. need O(1) space and O(N) time.
- we may have one or two majority elements
- using two counters to pair out different numbers, and then check the two candidates.
->>
<<-1185. Day of the Week ***
without using built date function, it will take time to implement it.
- days since 1970/1/1
- leap year
- %7 for negative and get the weekday.
->>
<<-1217. Play with Chips **
very hard to understand to move the chips. 
ith chip is at position chips[i]: for example: [2,2,2,3,3], chip 0,1,2 is at position 2 and 4 and 5 are at position 3. After understanding the problem it is really simple.
so we merge all odd position and even positions and move the smaller pile to larger pile.
->>
<<-1399. Count Largest Group **
it needs to get the sum of digits of a number, and count number of groups with max size.
- using hashmap to record the size
- apply the input limit, n<=10000, so max sum of digits is 36. we can use vector, which is more efficient.
->>
<<-999. Available Captures for Rook **
this problem if using dfs you will get totally wrong!!!!
just naively using the problem statement and tries the 8 directions and get the first 'p' or break.
->>
<<-1365. how many numbers are smaller than the current number. ***
O(nlogn) using map
O(n) using bucket sort.
->>
<<-1486. xor operation in an array ***
O(N) straightforward
O(1) do it bit by bit.
->>
<<-1534. count good triplets ***
i,j,k and their abs difference <=a,<=b，<=c respectively.
optimization: to avoid the 3rd loop if i,j cannot meet.
->>
<<-1252. Cells with Odd Values in a Matrix ***
give (r,c) it will increase by 1 the row r and col c.
- construct the matrix approach is straightforward O(MN)
- reduce space, only save row and col. The key point: at each position, the row^col must be 1 to be odd.
->>
<<-1450. number of students doing homework at a given time. ***
intervals
- using map and prefix, overkill O(nlogn)
- loop over all the intervals to see if it covers the given time -O(N) and most straightforward.
->>
<<-1351. Count Negative Numbers in a Sorted Matrix ***
row and col sorted in non-increasing order
- for each row, only check the tails. O(MN)
- the next row will have zeros>=previous row, so we can start from previous position. O(M+N)
both sorted, we need optimize to O(M+N).
->>
<<-1475. Final Prices With a Special Discount in a Shop ****
- O(N^2) brutal force
- next smaller using stack. in-place
->>
<<-905. Sort Array By Parity ***
- brutal force O(N) with O(N) space
- in-place: using two pointer.
->>
<<-922. Sort Array By Parity II ***
half odd, half even, put odd in odd index positions, and even in even index positions
- with extra space, it is trivial
- in-place: search for misplaced odd, even pairs, and swap them using two pointer.
```cpp
    vector<int> sortArrayByParityII(vector<int>& A) {
        int i=0,j=1,n=A.size();
        while(i<n && j<n){
            while(i<n && A[i]%2==0) i+=2;
            while(j<n && A[j]%2) j+=2;
            if(i<n && j<n) swap(A[i],A[j]);
            i+=2,j+=2;
        }
        return A;
    }
```->>
<<-1337. The K Weakest Rows in a Matrix **
matrix is 01 matrix. sum of each row is the strength. if strength is the same, row index smaller is weaker.
return the k-weakest row index.
- stable sort combine the strength and index.
->>
<<-1122. Relative Sort Array ***
sort arr1 using order in arr2, non-appeared elements in the back but sorted.
- using hashset, hashmap, sorted multiset to do this straightforward.
- the input<1000, so we can use count sort (bucket sort).
  - first get the count for each elements
  - add the elements in arr2 and decrease the count
  - add remaining elements with count>0
  ->>
<<-977. Squares of a Sorted Array ***
- two pointer (three pointer) from right to left.

->>
<<-950. Reveal Cards In Increasing Order ****
given an array of numbers, you need find the order so that we can reveal the cards in increasign order.
operation: reveal the top and put the  top to bottom.
Idea: do it reverse way. convert to deque.
- sort the array in descending order.
- if deck has two or more cards, bring the back to front
- add the card to front.
tag: array, deque, reverse thinking
rating: 4 star
   ->>
<<-1330. reverse subarray to maximize array value
sum(|A(i)-A(i+1)|)
-reverse a subarray only affect the two ends
- reverse (a,b) and (c,d), and we get (a,c) and (b,d) the change is |a-c|+|b-d|-|a-b|-|c-d|
- remove the abs, we only remove the abs for non-neighbored elements, 4 cases
->>
<<-747. largest number at least twice of others **
find the max and second max and also the index. one pass.->>
<<-414. Third Maximum Number **
this need ignore the duplicates (max0>max1>max2 required). so when we found identical continue;->>
<<-1464. max product of two elements in an array  ***
all positive integers
- sort and get the max and 2nd max, O(nlogn)
- O(N) to get max and second max one pass, O(n)----that is O(N) to find max and 2nd max
->>
<<-1108. defanging an ip address *
replace . with [.]
trivial
->>
<<-1221. Split a String in Balanced Strings *
cnt L and R.
trivial
->>
<<-709. to lowercase
trivial
->>
<<-1436. Destination City **
a line path. all non-destination city has one incoming and one outgoing path. use income +1, outgoing -1
then we are looking for the one >0
->>
<<-804. Unique Morse Code Words *
using hashset to eliminate duplicates
->>
<<-1309. Decrypt String from Alphabet to Integer Mapping **
a-i represented by 1 to 9, and j to z represented by 10# to 26#
- from right to left.
->>
<<-1370. Increasing Decreasing String **
pick the smallest and append to result
pick the next larger char and append to result
repeat above 2 until you cannot
pick the largest char and append to result
pick the next smaller char and append to result
repeat above 2 until you cannot 

- using hashmap to count each char.
- used one -1. until none is left.
->>
<<-1374. Generate a String With Characters That Have Odd Counts *
n<500
- greedy: if n is odd, use all 'a', if it is even use odd('a')+1'b'
->>
<<-657. Robot Return to Origin *
LRUD, trivial
->>
<<-557. Reverse Words in a String III **
reverse each individual words in a sentence.
- stringstream split into words
- two pointer - in-place
->>
<<-151. reverse words in a string *
reverse word by word. stringstream
->>
<<-541. Reverse String II **
reverse k chars every 2k chars
if left <=k, reverse all of it.
- recursive
- in-place using two pointer- easy to make mistake (be sure to limit in range)
```cpp
    string reverseStr(string s, int k) {
        int i=0,j=0;
        while(j<s.size() ){
            j+=k;
            reverse(begin(s)+max(0,j-k),begin(s)+min(j,(int)s.size()));
            j+=k;
        }
        return s;
    }
```->>
<<-344. Reverse String *
- two pointer or use reverse.
->>
<<-929. Unique Email Addresses **
. in username is ignored
+ in username after it discarded
using hashset.
->>
<<-893. Groups of Special-Equivalent Strings **
you can swap even index chars or odd index chars to convert s to t.
- sort odd chars and even chars and put into hash set.
->>
<<-1455. Check If a Word Occurs As a Prefix of Any Word in a Sentence *
trivial using stringstream
->>
<<-824. Goat Latin *
add ma ...
trivial: substr, append, stringstream
->>
<<-1408. String Matching in an Array *
straightforward, using string::find
->>
<<-1189. Maximum Number of Balloons *
find the min multiplier.
->>
<<-1507. Reformat Date *
convert string to number
hashmap
->>
<<-1446. Consecutive Characters *
max length of identical chars.
- count and get max.
->>
<<-1332. Remove Palindromic Subsequences **
each move to remove a palindrome subsequence. string only contains a and b.
return min number of moves to make it empty.
- if itself is a pal-string, remove in one move.
- if not: two moves, remove a and then remove b.
->>
<<-917. reverse only letters
- isalpha and two pointer
->>
<<-521. longest uncommon subsequence I.->>
<<-522. Longest Uncommon Subsequence II
a list of string, find the longest uncommon subsequence length.
greedy: 
- if no duplicates, longest one will be the answer. --skip duplicates (hashmap)
- the first no duplicate and is not subsequence of longer ones is the answer.
- check subsequence using two pointer.
->>
<<-788. rotated  digits *
it shall contain 2,5,6,9 at least one, cannot include 3,4,7.
->>
<<-1496. Path Crossing **
using hashset to save all paths.
- do not forget (0,0)
->>
<<-1417. reformat the string *
arrange letter and digit, no same type consecutive.
greedy: arrange the longer first.
->>
<<-520. Detect Capital *
if the first or all are capital
->>
<<-383. ransom note *
hashmap
->>
<<-415. add strings *
->>
<<-67. add binary *
string calculation for big numbers are often needed.
->>
<<-43. multiply string **
straightforward.
->>
<<-551. student attendance record I.
check if it is valid. 
->>
<<-819. most common word *
->>
<<-38. count and say *
->>
<<-345. reverse vowels of a string.*
two pointer.
->>
<<-1544. Make The String Great **
remove pair if they are same letter but in different case.
similar to stack.
->>
<<-387. first unique char in a string *
hashmap
->>
<<-434. number of segments in a string **
- count number of space separated segments. using stringstream
- O(1) space, counting space (ignore leading and trailing). equivalent to count space segments.
->>
<<-925. long pressed name **
two pointer cnt and compare
->>
<<-14. longest common prefix *
->>
<<-28. implement strstr *
->>
<<-58. length of last word *
->>
<<-859. Buddy Strings **
swap two letters in A once to get B.
- we shall only have 2 diff positions.
- swap and they shall equal.
->>
<<-837. complex number multiplication *->>
<<-49. group anagram *
hashmap using sorted as key->>
<<-553. optimal division **
get max result a/b/c/d... by adding parentheses. float division.
greedy: except the 2nd one, all other becomes multiplication.
->>
<<-1003. Check If Word Is Valid After Substitutions **
only contains abc, a valid string, you can split into left and right (can be empty), L+abc+R is also valid.
- intuition, similar to strange printer, using stack to eliminate abc.
->>
<<-1451. Rearrange Words in a Sentence *
sort according length and make the first word capital
straightforward.->>
<<-1456. max number of vowels in a substring of given length *
sliding window with fixed size.->>
<<-833. find and replace in string *
- find and record matched index and do replace from end.->>
<<-93. restore IP address *
brutal force to try 1 to 3 digits for each.
->>
<<-696. Count Binary Substrings ***
equal number of 01 and 0 and 1 are grouped consecutive.
- if we have m 0 and m 1s 00..011..1, then we have m substrings.
- this is pretty tricky: count current char and save previous count.
->>
<<-13. roman to integer ***
from right to left.
->>
<<-1422. Maximum Score After Splitting a String **
- left count and right count, get the max.
- one pass: ans=left0+right1=left0+total1-left1=total1+(left0-left1)
->>
<<-937. reorder data in log files ***
using multi_set to allows duplicates
->>
<<-1071. greatest common divisor of strings ***
using similar euler's algorithm on number.
```cpp
    string gcdOfStrings(string str1, string str2) {
        if (str1.size() < str2.size()) return gcdOfStrings(str2, str1);
        if (str2.empty()) return str1;
        if (str1.substr(0, str2.size()) != str2) return "";
        return gcdOfStrings(str1.substr(str2.size()), str2);
    }
```->>
<<-459. Repeated Substring Pattern ***
check if string has repeated pattern.
- using its factor to check (similar to binary search)
- first char in string is first char of repeat pattern, last char is the last char in pattern.
S+S and remove the first and last, S is still a pattern.
->>
<<-443. String Compression ***
using two pointer one for compressed, one for original.
in-place, O(1).
->>
<<-20. valid parenthesis ***
prefix sum, often used for other pair matching algorithm.
one for +1 and paired for -1.
->>
<<-125. Valid Palindrome **
ignore case, only consider alphanumeric.
- two pointer from each side.
->>
<<-680. Valid Palindrome II ***
at most delete one char and see if it is palindrome.
- already a pal-string
- reverse and check the first different. we have a pair of different. delete either one to see if palindrome.
->>
<<-686. Repeated String Match ***
find the min number of times A has to repeat such that B is a substring of it.
- greedy: append A until size>=B. if it contains B, that is it. if not, append one more.
- rolling hash to find substr.
->>
<<-1347. Minimum Number of Steps to Make Two Strings Anagram ***
replace char in t to other char so that t is anagram of s.
two hashmap compare. s-t we need add all positives (or all negatives).
(you cannot use both since we need to make them 0, it is a pair action).
->>
<<-890. Find and Replace Pattern **
greedy replace the word and pattern to abc...
If we do it in-place, the index will be messed up. 
->>
<<-1525. Number of Good Ways to Split a String ***
left and right has the same number of unique chars. compare the left vs the whole.
hashmap count.
->>
<<-791. custom sort string ***
given order of chars, sort a string. 
using custom sort: the index compare.
customized sort: we can use any pred, length, index... and understand the compares
default string compare is called lexicographically order. (dictionary order)
->>
<<-1433. check if a string can break another string ***
if exist some permutation of s >= some permutation of t for all indices. s[i]>=t[i] or s[i]<=t[i]
- count each char, and compare.
- prefix sum compare. only one direction is allowed.
->>
<<-1268. search suggestion system ***
type searchword and return the min 3 matched string (prefix match)
- sort the list and sorted list will have all same prefix together.
- using binary search to find the first one and add more if matched.
- Trie approach: build the Trie and get to the node and do dfs to get the first 3.
->>
<<-647. palindromic substrings **
count number of palindrome substrings
- expand at (i,i) and (i,i+1) and count the substrings
optimization: mancher...
->>
<<-5. Longest Palindromic Substring **
expand (i,i) and (i,i+1) to get the longest substring
->>
<<-609. Find Duplicate File in System **
gives the file like this:
["root/a 1.txt(abcd) 2.txt(efgh)", "root/c 3.txt(abcd)", "root/c/d 4.txt(efgh)", "root 4.txt(efgh)"]
duplicate files are the file with same content.
- hashmap using the content as keyword, and a list of filename as value.
- need to parse to get dir, filename, content. (stringstream)
followup:->>
<<-1. Imagine you are given a real file system, how will you search files? DFS or BFS ?
In general, BFS will use more memory then DFS. However BFS can take advantage of the locality of files in inside directories, and therefore will probably be faster
->>
<<-2. If the file content is very large (GB level), how will you modify your solution?
In a real life solution we will not hash the entire file content, since it's not practical. Instead we will first map all the files according to size. Files with different sizes are guaranteed to be different. We will than hash a small part of the files with equal sizes (using MD5 for example). Only if the md5 is the same, we will compare the files byte by byte
->>
<<-3. If you can only read the file by 1kb each time, how will you modify your solution?
This won't change the solution. We can create the hash from the 1kb chunks, and then read the entire file if a full byte by byte comparison is required.

What is the time complexity of your modified solution? What is the most time consuming part and memory consuming part of it? How to optimize?
Time complexity is O(n^2 * k) since in worse case we might need to compare every file to all others. k is the file size

How to make sure the duplicated files you find are not false positive?
We will use several filters to compare: File size, Hash and byte by byte comparisons.
->>
<<-1233. Remove Sub-Folders from the Filesystem ***
this is so similar to 1268 using sorted string property
after sort, subfolders follows after the parent folder.
dictionary order.
->>
<<-1016. Binary String With Substrings Representing 1 To N **
- brutal force, from 1 to N get each binary string. O(nlogn).
- we can easily build binary string using the range 0,1-3,4-7,8-15,16-31...
0,1
10,11
100,101,110,111
....
everytime we add 1 to the MSB to all previous result.
- greedy: we only need check the longer one since all smaller one are substring of longer one.
that is half of it is enough.
->>
<<-1371. Find the Longest Substring Containing Vowels in Even Counts ****
- intuition: using two pointer sliding window to track vowel's position. the consonants in front of vowel and after vowel shall be counted.
- odd/even we often use xor. for even xor=0
- each vowel shall be even, we can use bitmask for each vowel.
->>
<<-1358. Number of Substrings Containing All Three Characters ***
- greedy: find the smallest prefix/postfix which satisify the condition, and then 1+2+3+....+(n-k) (not convenient)
- sliding window: find the smallest window satisfying the condition, all prefix shall be counted.
abcabc
first 0-2 +1
then 1-3 +2
then 2-4 +3
then 3-5 +4
->>
<<-1324. Print Words Vertically **
only need to insert space when we are filling a later position.
using 2d matrix. more on array.
->>
<<-1545. Find Kth Bit in Nth Binary String ***
s1='0',Si=S(i-1)+"1"+reverse(invert(S(i-1)))
- need to conclude S(i) length is 2^i-1. (1,3,7,15,31.....)
- divide into 3 region, left, mid, right. do it recursively.
->>
<<-1023. Camelcase Matching ***
pattern include uppercase and/or lowercase. You can insert lowercase chars to match.
- two pointer compare. (suppress unwanted chars)
if cur char matches pattern, increase j.
if cur char is lowercase and does not match, increase i.
if does not match and is uppercase, return false.
->>
<<-12. integer to roman. **
- using hashmap.
- simple solution: given all digits representation for thousands, hundreds, tens, ones.
->>
<<-1410. HTML Entity Parser **
replace &quot,&apos,&amp,&gt,&lt,&frasl
- mark those positions and replace from end to begin.
->>
<<-1452. People Whose List of Favorite Companies Is Not a Subset of Another List ***
- companies up to 100.
- arrange the companies into bits (bool array) or bitset.
- not a subset: and will get 0.
->>
<<-539. min time difference ***
- sort the string.
- convert to integer and wrap around.
->>
<<-1081. smallest subsequence of distinct characters ***
- using monotonic increasing stack + hashmap.
- if current char < top, and top char has leftover behind.
- record used and count.
->>
<<-1404. number of steps to reduce a number in binary representation ***
even /2
odd +1
- if cf+bit=0, shift, one move
- if cf+bit=1, need to add 1, and shift,2 moves
O(N) instead of O(N^2) for add.
similar see: 1558. min steps to make target array.
->>
<<-1156. swap for longest repeated character substring ***
you can swap at most one pair to make the substring all same char.
- no swap
- swap, you have one different char and same char outside.
- sliding window:
  - for each char, we consider it as the starting position and keeps expanding to right.
  - make sure the window contains < than total.
->>
<<-767. reorganize string ***
no adjacent chars are the same.
- cannot arrange case
- using heap to arrange most frequent.
- or greedy approach.
->>
<<-1138. alphabet board path ***
from (0,0) to target string, return the sequence of moves (minimum)
- 'z' is the last row and it can only go up.
- if current char is 'z' or prev is 'z', we need to move to 'u' first. that's the key.
- once the coordinate is defined, we can easily get the sequence
->>
<<-916. word subsets ***
two lists of strings, A and B. if every letter in B is a subset of word a.
- hashmap to record the letter in B.
- compare the histogram of each word in A with B.
- note you cannot put all letters in B together. however we can get the max among all of them.
->>
<<-1297. Maximum Number of Occurrences of a Substring ***
substring given: [minSize,maxSize],num of unique letters<=maxLetters.
- the key point is: if we find a substring ==minsize, then its frequency will be higher than longeer substr.
- so we only need check fixed window with minSize. (critical point)
->>
<<-816. Ambiguous Coordinates ***
coordinates (x,y) but , and . is removed. no extra zero.
return all combinations.
- backtrack: divide into left and right, and then add . to left and right, combinate left and right.
- remove all those illegals. (actually not backtrack, but try all combinations)
->>
<<-966. Vowel Spellchecker ***
capitalization: if ignore the case and match, return the word in dict.
vowel errors: 
hashmap: without capital, replace vowels using special char.
->>
<<-809. expressive word. **
two pointer and compare each char's count.
->>
<<-17. letter combination of a phone number **
backtracking
->>
<<-1419. min number of frog croaking **
overlapped intervals. c as the begin and k as the end.
we can use map to get the number of overlapped.
->>
<<-1461. Check If a String Contains All Binary Codes of Size K **
- check all number from 0 to 2^k-1 binary string.
- method 2: sliding window to get the number and mark it.
->>
<<-848. Shifting Letters **
- shift the previous chars. we can do it from right to left.
- do mod.
->>
<<-831. Masking Personal Information **
email address: username replaced by: firstchar+*****+lastchar.
phone number: 7 digit local number, optional country code
other char () space shall be removed.
mask the phone number as ***-***-xxxx for local number
with country code: +**-***-***-xxxx. 
- need a lot of details.
->>
<<-1328. Break a Palindrome **
Given a palindromic string palindrome, replace exactly one character by any lowercase English letter so that the string becomes the lexicographically smallest possible string that isn't a palindrome.
- only need consider the first half. (wrong)
- greedy: find the first >'a' and replace it to 'a', otherwise replace it with 'b' (for all 'a')
->>
<<-1311. Get Watched Videos by Your Friends ***
list of friend: friends for person i. (forms a graph)
list of videos: video watched by person i.
given id, and level, return the list of videos sorted by frequency, tie then lexicographically sort.
- bfs to reach the level.
- hashmap
- sort or pq.
->>
<<-1432. Max Difference You Can Get From Changing an Integer ***
given a number, do the operation exactly two times
- pick a digit x and y, replace all occurrences of x to y. (cannot have leading 0 or becomes 0)
- first time a, second time b, return the max difference between a and b.

greedy:
- get a to the max, b to the min
- to get max, replace the first seen digit<9 to 9
- to get min, replace the first seen digit>1 to 0. If the first seen number>1, replace it to 1.
->>
<<-1513. Number of Substrings With Only 1s **
- count consecutive 1s. for example 111, 1+2+3=6
0110111-->1+2+1+2+3=9
->>
<<-1541. Minimum Insertions to Balance a Parentheses Strings ***
( 2 points, ) 1 point.
idea: count the prefix, ( +1, ) -1. if count>0 and is odd, we need add a ) to make it even.
similar to valid parentheses.
->>
<<-6. zigzag conversion ***
arrange string into nrows in zigzag way, and return reading by row.
->>
<<-842. Split Array into Fibonacci Sequence ***
once the first two numbers are determined, the whole sequence is determined.
the first number length is max n/3, and second number length is also n/3. so we try all possible choices.
backtracking + validation of numbers.
->>
<<-722. remove comments ***
// for single line comment, /*...*/ for multiple line comments
- pair using stack.
- connect to a single line
- remove comments (newline could be deleted or shall preserve).
- split to array of strings.
a more simple one-pass approach:
- input is lines (each element is a line)
- if we see //, we ignore remaining
- if we see /* we keep ignoring until we see */
->>
<<-1177. Can Make Palindrome from Substring **
query [left,right,k]: for the substring [left,right] choose up to k chars to replace any lowercase letter.
check if it is possible to convert the substring into palindrome.
- prefix each char's count so we can get the count in the range fast.
- palindrome: only have one or zero odd char.
->>
<<-1234. Replace the Substring for Balanced String ***
only contains QWER. if each char appears n/4 times called balanced.
return the min length of the substring that can be replaced with any other string of same length to make it balanced.
approach: 
- count each char's occurrence and get number of addition/removal for each char. (only count the extra)
- sliding window to find the min window >=the map.
->>
<<-71. Simplify Path **
unix style directory.
. for current
.. for previous
- using stack.
->>
<<-556. Next Greater Element III ****
32bit integer with the same digits. find the next greater element.
- all digits descending order, impossible
- all digits ascending order, reverse the last pair.
- other cases: from right. 534976, the first sorted pair is 49.
   find the right one > 4, which is 6 and swap them-->536974
   sort the right side.536479
similar: binary same number of set bits, find the next greater or next smaller.
->>
<<-1169. invalid transaction **
using hashmap user vs transaction, keep the string.
->>
<<-678. valid parenthese string ****
there are *s inside the string, * can be a single ( or ) or none.
greedy approach:
- count the open parentheses. 
- max possible open ( and min possible open ( that must be paired. 
- min possible open ( must be 0 at the end.
- max possible >=0
- * as a (, then cmax++, act as a ) cmin--
->>
<<-3. Longest Substring Without Repeating Characters ***
- using hashmap and sliding window. if we see char, we move the left.
- key optimization to avoid O(N^2) j=max(j,mp[s[i]]) to keep the max and we do not need to modify the map.
->>
<<-1487. make file names unique ***
by adding (serialnumber) to the filename. 
using hashmap, default start from 1.
check if the file name exist or not.
- not exist, use the name and start with 1.
- exist, keep increasing from the index
filename vs serialnumber.
->>
<<-1540. Can Convert String in K Moves **
ith move: choose any index j which has not been chosen and shift the char i times.
check if we can convert s into t in no more than k moves.
- same char into other char we need to wait 26.
- hashmap to increase time.
->>
<<-165. compare version number **
read each number and compare,if one has no version number, use 0
->>
<<-468. validate ip address **
ipv4 or ipv6. a lot of edge cases.
->>
<<-8. string to integer (atoi) ***
- stringstream
- consider all corner case.
->>
<<-761. special binary string ****
num 0s = num 1s.
each prefix has at least as many as 0s. (num 1s>=num 0s)
a move: choose two consecutive non-empty special substring of S and swap them.
after any number of moves, find the largest string.
- split S into several special strings (as many as possible)
- special string starts with 1 and end with 0
- sort all
- join

i.e. we strip the first 1 and ending 0, and recursively process the mid part.
->>
<<-632. Smallest Range Covering Elements from K Lists ***
merge sort using pq. and get the range.
->>
<<-899. Orderly Queue ***
for a string, each move: we choose one of the first K letters and move to the end.
return the smallest string.
- k=1: rotating the string and get the smallest
- k>1: we can rotate the whole string, we can always bubble up. sort.
we can use the first 2 elements as a buffer to swap any two adjacent characters. Since we can reach any permutation by swapping adjacent characters (like bubble sort), in this case the minimal reachable permutation is the sorted S.
->>
<<-1316. Distinct Echo Substrings ***
number of substrings which is a+a.
- brutal force: for each starting position, and next start position, check them all.
- rolling hash: and get the range hash value (use some math)/
rolling hash treat it as numbers, may have collision. 
->>
<<-936. stamping the sequence ***
a stamp string, and target string.
stamp will replace the old string.
return the sequence to stamp to reach the target.
- the current intact stamp is the last one. so start from last to first.
- ababc-->ab???->?????
->>
<<-1392. Longest Happy Prefix ***
happy prefix: a prefix is also a suffix
rolling hash!!
->>
<<-76. Minimum Window Substring ****
in O(N) find the min window in S which contains all the chars in T.
- sliding window + hashmap
->>
<<-336. palindrome pairs ***
a list of words, return the pairs if concated, will be a palindrome.
- brutal force try all pairs.
- trie.
->>
<<-214. Shortest Palindrome ****
you can add characters in front of the string to make it palindrome. return the shortest string.
- equivalent: reverse it and add in the back.
- find the longest palindrome substring in the string.
for example: aacecaaa, the longest pal-string is aacecaa.
we can expand at (i,i) and (i,i+1) to get the longest length.
- mancher's algorithm for pal-string
- KMP
->>
<<-32. Longest Valid Parentheses ***
- using stack to eliminate the paired parenthese and remaing to get the valid length.
->>
<<-68. Text Justification ****
- using two pointer
->>
<<-273. integer to english words ***
- every thousand uses the similar approach.
->>
<<-30. substring with concatentation of all words **
each word exactly once. each word has the same length.
- sliding window with size=sum of all word length.
- first shall match one word and then match the second....
->>
<<-564. Find the Closest Palindrome ***
given n, find its closest palindrome number.
- only need to check the half and check all combinations.
->>
<<-65. valid number ***
->>
<<-1290. convert binary number in a linked list to integer *->>
<<-876. middle of the linked list (fast and slow) *->>
<<-237. delete node in linked list  **
given access only to the node.
- swap the node to the back.
->>
<<-206. reverse linked list *->>
<<-21. merge two sorted list * (two pointer)->>
<<-83. remove duplicate from sorted list (prev, next) **->>
<<-141. linked list cycle (fast and slow) **->>
<<-160. intersection of two linked list (two pointer) ***->>
<<-234. palindrome linked list (reverse the 2nd half) **->>
<<-203. remove linked list elements (add a dummy) **->>
<<-1019. Next greater node in linked-list. (montonic stack) ***->>
<<-817. linked list components
given a list and a vector, return number of connected groups.
- hashset.
- only count the tail two nodes are fine.
->>
<<-328. Odd Even Linked List ***
in-place, we are talking the sequence (index). using two pointer one for odd and one for even
do it the same time.->>
<<-430 flatten a multilevel doubly linked list ***
recursive approach
->>
<<-445. add two numbers II. **
MSB first. reverse and add and then reverse result.->>
<<-725. Split Linked List in Parts
divide it into k parts, size difference of any two parts <=1. eariler parts >= later parts.
mod and assign the remainder to the first nodes.
->>
<<-24. Swap Nodes in Pairs 
swap each pair. 
- recursive
- or similar to 328 odd even linked list->>
<<-109. Convert Sorted List to Binary Search Tree **
similar to sorted array to BST.
- mtd1: convert to array.
- mtd2: using list and find the mid as the root. (find mid can use fast and slow)
->>
<<-148. sort list. ***
merge sort: split it into half and merge.
->>
<<-86. Partition List **
given a value x, put all nodes <x in front of nodes >=x (partial sort or kth element)
preserve the original order.
->>
<<-1171. Remove Zero Sum Consecutive Nodes from Linked List ****
repeatedly remove zero sum consecutive nodes.
- prefix sum + hashmap to record the node hashmap<prefix,node>.
- head could be removed. using a dummy node.
->>
<<-147. Insertion Sort List **
O(1) space. for each node find its correct position (only for the left).
->>
<<-1367. Linked List in Binary Tree ***
check if the linked list corresponds to a downward path in binary tree.
- find the nodes which equals to head.
- dfs from those nodes and check if there is a path.
->>
<<-92. reverse linked list II. ***
reverse from position m to n. one pass.
- using dummy,pre, cur, next.
- swap the curr node to pre->next...
->>
<<-143. reorder list ***
L0->Ln->L1->Ln-1->L2....
- edge case: head==0 || head->next==0
- slow fast, the slow will always be on n/2th node. need to record its previous.
- when merge: at the end, the list shall be special treated, otherwise the last node will be not added.
->>
<<-142. linked list cycle II ***
find the entrance of a list cycle.
- fast and slow at the meet point
- one continue, one start from the beginning. meet at the entrance.
- math proof
->>
<<-82. Remove Duplicates from Sorted List II ***
delete all instances with duplicates.
- keep dummy, pre, cur and next. (similar problem for array)
->>
<<-138. copy list with random pointer ***
- copy each node directly after it and then assign the random pointer. and then get it out
->>
<<-19. remove Nth node from end of list **
- two pointer: start another pointer when the first goes N steps. keep the two N distance.
->>
<<-2. add two numbers **
LSB are the first.
->>
<<-61. Rotate List **
rotate the list to right by k places.
- consider it as a circle and find the new head position and break it.
- with pre and cur.
->>
<<-707. Design Linked List **
get(index)
addAthead()
addAtTail()
addAtIndex()
deleteAtIndex()
better keep tail and head at the same time
->>
<<-25. Reverse Nodes in k-Group
if n is not divisible by k, the remaining part is kept unchanged.
this is similar to reverse pairs, and reverse from n to m.
or recursively do the n to m reverse.
->>
<<-23. Merge k Sorted Lists
- priority_queue
- merge two list recursively (pair and merge).
->>
<<-641. design circular double ended queue (deque)
CircularQueue(k)
insertFront/Last
deleteFront/Last
getFront/Last
isEmpty
isFull
circular buffer using vector and two pointer also a count. --circular buffer
note: front and back grows in different direction. <-front back-->
->>
<<-622. Design Circular Queue (ring buffer)
write in the front and read in the back.

CircularQueue(k)
Front/Rearrange
enQueue
deQueue
isEmpty
isFull

using vector, and two pointer and size.
->>
<<-862. shortest subarray with sum at least k. ****
- prefix sum to get the range sum.
- O(N) to get range sum>=k. 
for each pre(i) we need to find the most recent j so that pre[i]-pre[j]>=k.
using a deque to maintain a sorted pre. back is largest, and front is smallest.
When we add pre(i) to the back, we pop all those front which satisfy the condition and update our answer.
pop back all those elements <=pre(i)
compare with the smallest first.
pop those bigger ones.
->>
<<-121. best time to buy and sell stock->>
<<-746. min cost climbing stairs->>
<<-53. maximum subarray->>
<<-674. longest continuous increasing subsequence->>
<<-643. max average subarry I.->>
<<-1277. count square submatrices with all ones->>
<<-1035. uncrossed lines->>
<<-714. best time to buy and sell stock with transaction fee->>
<<-64. min path sum->>
<<-63. unique paths (dp or math)->>
<<-718. max length of repeated subarray->>
<<-1292. max side length of a square with sum less than or equal to threshold->>
<<-873. length of longest fibonacci subsequence->>
<<-120. triangle: min path sum->>
<<-1524. number of subarrays with odd sum->>
<<-55. jump game->>
<<-63. unique path II->>
<<-918. max sum circular subarray->>
<<-152. max product subarray->>
<<-45. jump game II
-dp->>
<<-123. best time to buy and sell stack III.->>
<<-583.delete operation for two strings **
- dp, LCS->>
<<-91. decode ways
A:1 ...Z: 26
given a digit string, number of ways to decode it.
dp: current char attached to previous or independent.->>
<<-115. Distinct Subsequences
string s and t, find the number of distinct subsequence ==table
dp.
->>
<<-1542. Find Longest Awesome Substring
awesome substring: a substring could be pal by any number of swaps.
hashmap for pal-string property.
odd even
bitset to reduce from 2d to 1d dp.->>
<<-72. edit distance
dp->>
<<-1449. Form Largest Integer With Digits That Add up to Target
cost array: cost[i] is the cost for digit i+1.
target cost is given. return the largest number with exact the target cost.
- intuition: we shall prefer more digits. For same length, we prefer bigger digit first.
- a digit can be used multiple times.
- knapsack with repetition. (we choose one, with reduced target but with the same set of choices)->>
<<-730. Count Different Palindromic Subsequences
dp[i,j] the number of different subsequence in range [i,j]
->>
<<-97. interleaving string
if s3 is formed by interleaving of s1 and s2.
dp->>
<<-1531. String Compression II
RLE: you can delete at most k characters to get the min length of RLE.
- dp: delete shall make identical char group connected. (some greedy)
- get RLE length on the fly.->>
<<-10. regular expression matching
dp
->>
<<-44. wildcard matching
dp
binary search:->>
<<-724. find pivot index->>
<<-35. search insert position->>
<<-978. longest turbulent subarray->>
<<-795. Number of Subarrays with Bounded Maximum->>
<<-1011. capacity to ship packages within d days->>
<<-1482. min number of days to make m bouquets->>
<<-153. find min in rotated sorted array->>
<<-154. find min in rotated sorted array II (with duplicate)->>
<<-1300. Sum of Mutated Array Closest to Target->>
<<-1438. longest continuous subarray with absolute diff less than or equal to limit
- binary search
- max/min for window size. using two deque to find max and min separately.->>
<<-33. search in rotated sorted array->>
<<-81. search in rotated sorted array II.->>
<<-126 word ladder II ****
from start word to end word, each time change one letter, get all the paths.
- bfs with parent information.
bfs dfs, union-find->>
<<-695. max area of island->>
<<-1267. count servers that communicate
- union-find: same row, same column, merge row and column, hashmap for the parent
- accumulate each row, each col, and then check each server (i,j) if ith row, jth col >1, this need be counted
->>
<<-947. Most Stones Removed with Same Row or Column
- union find: union the row and column, use hashmap for the parent.->>
<<-1202. Smallest String With Swaps->>
<<-79. word search->>
<<-126. word ladder II->>
<<-128. longest consecutive sequence
unosorted, O(N).
- hashset to store seen elements
- if we see n+1, or n-1, union them.

backtrack->>
<<-78. subsets->>
<<-216. combination sum III->>
<<-39. combination sum->>
<<-40. combination sum II->>
<<-90. subsets II->>
<<-22. generate parentheses **
backtracking->>
<<-816. Ambiguous Coordinates ***->>
<<-17. letter combination of a phone number **->>
<<-842. Split Array into Fibonacci Sequence ***

greedy->>
<<-1247. Minimum Swaps to Make Strings Equal ****
s1 and s2 only contains x and y. you can swap s1[i] and s2[j]
return the min swaps to make them equal or not possible.
- xx vs yy, 1 swap
- xy vs yx: 2 swaps
- greedy: if s1[i]==s2[i] we do not touch them. so we only have xy and yx combinations.
- two xy or two yx can use one move to make them equal.
- one xy and one yx pair use two moves.

sliding window

tree->>
<<-105. construct binary tree from preorder and inorder->>
<<-106. construct binary tree from postorder and inorder->>
<<-606. Construct String from Binary Tree ***
You need to construct a string consists of parenthesis and integers from a binary tree with the preorder traversing way.
The null node needs to be represented by empty parenthesis pair "()". And you need to omit all the empty parenthesis pairs that don't affect the one-to-one mapping relationship between the string and the original binary tree.
- postorder, check left and right, both empty, one empty->>
<<-87. scramble string
binary tree

stack:->>
<<-591. tag validator
stack
->>
<<-1096. brace expansion II
stack
->>
<<-1106. parsing a boolean expression
stack->>
<<-770. basic calculator IV
symbolic
->>
<<-736. parse lisp expression
stack
->>
<<-385. mini parser ***
nested integer. deserialize it.
using stack for recursive.->>
<<-556. Next Greater Element III ****->>
<<-227. basic calculator ***
only include +-*/ and space.
add a 0+ to the string to make it convenient to process.
put results in stack.
->>
<<-856. score of parentheses ***
() has 1 score, AB score A+Balanced
(A) has a score of 2A
- use a stack to store left. 
- we also need store the score between paired (). using two stacks
- the stack keeping score add a sentinal 0 so the results stores on the first element.
->>
<<-962. Maximum Width Ramp
i<=j, and A[i]<=A[j], find the longest j-i.
O(N^2) is simple
how could we reduce the complexity to O(nlogn) or O(N)?
- find unnecessary work and save in cache?
- organize into other data structure?
monotonic stack could be a good candidate.
- maintain a decreasing stack, when we see a big number, we keep popping out and get the max.
[6,0,8,2,1,5]
stack store [6,0] 
5 to 0, pop 0
1 to 6
2 to 2
8 to 6
->>
<<-907. sum of subarray minimum
each element can be min, we need to find left greater and right greater.
using monotnic stack to find them in O(N).->>
<<-42. trapping rain water....monotonic decreasing stack.->>
<<-84. largest rectangle in histogram->>
<<-85. maximal rectangle
- stack: using max histogram approach.
->>
<<- 1249. min remove to make valid parentheses ***
using stack to mark invalid left and right parentheses.
 

heap:->>
<<-1499. Max Value of Equation
coordinates sorted by x. fid the max yi+yj+|xi-xj|->>
<<-767. reorganize string ***->>

### trivials
<<-1680. Concatneation of consecutive binary numbers
left shift and add.
->>
<<-1679. Max number of k-sum pairs
sort and then use two pointer to match.
->>

<<-1678. goal parser interpetation
(al) ->al. and () change to 'o'
simple, using string as stack
->>

<<-161. One Edit Distance
insert exactly one char into s and get t.
del exactly one char from s and get t.
replace one char in s and get t.
- the length shall differ <=1.
- swap s and t if s is shorter than t. (no dp)
->>
<<-1672. Richest customer wealth
trivial
level: 1
->>
<<-1670. Design Front Middle Back Queue
problem: push/pop_back,push/pop_front,push/pop_middle
subject: array or deque.
approach: 
- using vector O(N). 
- using two deque O(1)
level: 2
->>

<<-1668 maximum repeating substring
problem: repeat string and get the max repeat, so that it is a substr of another word.
subject: string
level: 1
->>
<<-1656	Design an Ordered Stream    		80.0%	Easy	->>
<<-1646 Get Maximum in Generated Array->>
<<-1640 check array formation from pieces 
using the piece first number as the key in hashmap->>

<<-1629. Slowest Key
given a string and each key's release time. find the char with longest duration
find the max diff and the char.
->>
<<-1684. Count the number of consistent strings->>

## strategy game

<<-55 jump game
position at 0, each element is the max step you can jump. all positive.
idea: bfs, you have to be able to reach the index to use it.
->>

<<-45. Jump game II.
position at 0, each element is the max step you can jump. min step to reach the last index.
idea: bfs and get each layer. but no need of queue.
->>

<<-174. Dungeon Game
dp: 2d grid. reverse
->>

<<-289. Game of life
inplace using high-byte.
->>

<<-390. Elimination game
sorted integer from 1 to n. remove the first number and every other number until the end, and then from the other side. find the last number left.
we do not need to actually delete them.
each round the left elements is n/2
if odd, then we need advance the pointer by step.
each round the step*2, dir^=1
->>

<<-1345. Jump Game IV
array of integer. position at first index. you can jump to i+1 or i-1 (no over the array boundary) or jump to j if arr[j]==arr[i].
return the min steps to reach the last index.
using hashmap to record same values indices.
->>

<<-1340. Jump Game V.
given an array of integer, and integer d. each time you can jump back or forward [1,d] steps. You can only jump to smaller values and all values in between are smaller than arr[i].
greedy+dp: sort with index and jump from lowest.
->>

<<-353. design snake game
screen size wxh. snake is positioned at (0,0) with length=1.
You are given a list of food positions. When snake eats the food, its length and score increases by 1. Each food will appear one by one. Next will appear only after the previous food is eaten.
snake moves using a deque to represent the snake. 
->>

<<-1690. Stone game VII
remove either the left or right stones, return the max difference.
competition game: same as who is the winner.
->>

<<-1406. Stone game III
given a list of stones, each player can take 1,2 or 3 piles from the first. return who is the winner.
typical competition game: other side's score is negative using dp.
->>

<<-810. Chalkboard XOR game.
given a list of non-negative numbers. take turn to erase one number. If he causes the xor of all remaining elements to be 0, he loses. If starts with 0, he wins. return who is the winner.
bit operations: 
- if xor=0, then first player wins.
- otherwise, erase a number which is different from current xor, leaving a nonzero xor to the other side. Odd length will leave a non-zero to the other side.
greedy approach.
->>

<<-822. Card flipping game
N cards with a number on card back and front.
we choose a card, and flip it. If the number is not on the front, it is good.
return the smallest number which is good.
- cards with same number are not valid
- find the min of all front and back numbers if they are not in the duplicate set.
->>

<<-1306. Jump Game III
given an array of non-negative integers. You are positioned at index start. You can jump to i+arr[i] or i-arr[i]. check if you can reach any index with value 0.
dp: or recursive dfs with visited.
->>

<<-1145. Binary tree coloring game
binary tree with n nodes, n is odd, and each node has unique value from 1 to n.
two players by turn color nodes in different color. You can only color the child or parent of your colored nodes. If one cannot color, pass the turn. If either cannot make a move, game ends. You are the 2nd player, decide if you can win.
you only have 3 options: parent, left or right child. and count the nodes.
->>

<<-1535. Find he winner of an array game
a array of unique integer and k. compare the first two elements, leave the larger one and move the smaller to the end. game ends when an integer wins k consecutive times.
do not need to move the end, but move the larger one to current position and count its winning times.
->>

<<-1275. Find winner on a tic tac toe game
A place x and B place o. row, col, or diagonal the same wins. 
A: ++, B: --, and keep counting.
->>

<<-1040. Moving Stones Until Consecutive II
array of stones at different positions. Move either end stone to unoccupied position so that it is not end point.  min number and max number of moves.
greedy: min move: find the window with min unfilled slots with length=n.
max move: move all stones to leftmost or rightmost (keep one end not changed)
choose the max of the two options.
->>

<<-375. guess number higher or lower II
number from 1 to n. You guess a number if correct, you win. otherwise, you guess x and pay x. return the min money to guarantee win.
min of the max money for the choice. ie. assume each time you guess wrong.
minmax problem.
->>

<<-374. Guess number higher or lower
each guess will get: lower, greater or equal.
return the number.
binary search
->>

<<-464. Can I win
two players take turn to add to total, given max choosable number n. The player first causing total>=target wins.
the number cannot be reused.
who will win.
bitmask dp: record the win and lost status.
->>














