### Introduction
This document is used for grep
each problem is tagged by <<-problem->> and each problem contains:
problem description
subject or approach
hard level

All problems are from leetcode 2020.

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
find records containing dp + greedy 
<<-((?!->>).|[\r\n])*?(dp|greedy)((?!->>).|[\r\n])*?(dp|greedy)(.|[\r\n])*?->>

find records containing dijkstra:
<<-((?!->>).|[\r\n])*?(dijkstra)(.|[\r\n])*?->>
begin with <<-, followed can be any chars but ->>, non-greedy
then include dijkstra the keyword
followed by any char including newlines, ending with ->>
but it will also match the whole document, why?


<<-0111. problem
subject: dp with greedy.
level: 1
->>
problems are classified according to the most important classification. we focused on the idea.

## algorithm focused Problems

major algorithms:
- recursive, backtracking, dfs
- bfs
- greedy,
- dp
- divide and conquer.
- search
- sort.

## dp
common steps to solve dp problems
- identity it is a dp problem: overlap subproblems, using memoization to avoid repetitive recursive calls, polynomial complexity
- identify base cases.
- define subproblem and subproblem states
- find the state transfer or recurrence relation
- how to get the answer for the original problem.


<<-279	Perfect Squares    		48.2%	Medium	
given n, return the least number of perfect square number that sum to n.
dp[i]=min(dp[i-j*j]+1)
->>

<<-309	Best Time to Buy and Sell Stock with Cooldown    		47.8%	Medium
->>
<<-188	Best Time to Buy and Sell Stock IV    		28.8%	Hard	
at most k transactions. based on previous transactions.
->>
<<-123	Best Time to Buy and Sell Stock III    		39.3%	Hard	
at most two transactions. based on previous transactions
->>
<<-122	Best Time to Buy and Sell Stock II    		57.9%	Easy	
many transactions as possible.
->>
<<-121	Best Time to Buy and Sell Stock    		51.1%	Easy	
perform at most one time. curr-lmin. all stock problems are based on this. top down is more understandable.
all the stock problem can be solved using the same trick price[i]-lmin.
->>
<<-714	Best Time to Buy and Sell Stock with Transaction Fee    		55.4%	Medium	->>

<<-313	Super Ugly Number    		45.6%	Medium	
given a list of prime factors, return the nth superugly number (who's prime factors are only in the list).
dp: with k pointers, ind, check each pointer and advance (multiple advance to avoid duplicates)
->>
<<-264	Ugly Number II    		42.5%	Medium	
find the nth ugly number, with factor 2,3,5.
->>
<<-263	Ugly Number    		41.7%	Easy	
check if it only has factor 2,3,5.
key idea: need advance each pointer if it is multiple of more than one factors
->>

<<-322	Coin Change    		36.5%	Medium	
you can reuse coin, return the min number of coins to make the amount. if not possible return -1.
1d dp since we can reuse it. dp[w]=min(dp[i-c]+1)
->>

<<-337	House Robber III    		51.0%	Medium	
binary tree. 
->>
<<-213	House Robber II    		37.2%	Medium	
a circle.
->>
<<-198	House Robber    		42.6%	Easy	
array.
->>

<<-338	Counting Bits    		70.0%	Medium	
from 0 to n, calculate the number of set bits in each number.
dp: dp[i]=dp[i/2]+i&1
->>

<<-343	Integer Break    		50.8%	Medium	
break into at least two positive interger and max the products.
dp: dp[i]=max(dp[i],dp[j]*(i/j),j*i/j)
->>

<<-368	Largest Divisible Subset    		38.1%	Medium	
in the subset, every pair satisfy a[i]%a[j]=0 or a[j]%a[i]=0
equivalent: they shall have a gcd. or this is a geometric series.
sort and then use dp LIS. with parent information to traceback.
pay attention to edge case: empty or mxlen=1 case.
->>

<<-375	Guess Number Higher or Lower II    		41.4%	Medium	
number from 1 to n. You guess a number if correct, you win. otherwise, you guess x and pay x. return the min money to guarantee win.
min of the max money for the choice. ie. assume each time you guess wrong.
minmax problem.
dp, minmax. top down or bottom up.
->>
<<-374	Guess Number Higher or Lower    		44.0%	Easy	
give direction of guess, binary search.
->>


<<-517	Super Washing Machines    		38.4%	Hard	
given n washmachine in a line. you can pick any machines and pass one address to its left or right at the same time.
return the min number of operations to make all the machines to have the same number of dresses.
- subtract the average and our target is 0
- from left to right, we shall pass all our prefix sum to right.
- the single element max also shall be reduced to 0.
*****
->>

<<-568	Maximum Vacation Days    		41.2%	Hard	
- you are in city 0 on Monday, can only travels among cities [0,n-1]
- cities connected by flights given by matrix flights. 0 means no flight
- you have k weeks to travel. You can take flights at most once per day and only on Monday.
- in each city the max vacation days is given days[i,j] i: the city, j the week 
return the max vacation days you can take during k weeks.
dp[w,n] max vacation days to stay at city n on wth week.
dp[w,n]-previous week at city j. max(dp[w-1,j]+days[j,i]) the key is to derive the relation.
*****
->>

<<-651	4 Keys Keyboard    		52.8%	Medium	
key1: print one 'A'
key2: selet all
key3: copy
key4: paste
press the key N times, return the max print length.
N=1: 1 key1
N=2: 2 key1 twice
N=3: 3 key1 3 times
N=4: 4 key1 4 times
N=5: 5 key1 5 times
N=6: 6 AAA +S+C+P 
N=7: 8 AAAA+S+C+P, AAA+SCPP->9 len(3)*3=9
N=8: 10 len(5)*2 or AAA+SCPPP->3*4=12
N=9: len(6)*2, len(3)*2*2 or AAA+SCPPPP->3*5=15
dp[i]=i press key1 i times
dp[i]=dp[j]*(
S+C+P 3 keys will double it. len[n]=len[j]*(i-j-1)
->>
<<-650	2 Keys Keyboard    		49.6%	Medium	
on screen there is one 'A', you can use copy all and paste.
return the min keys to reach n 'A'.
i can be obtained from j (if i%j==0) by paste multiple times.
dp[i]=dp[j]+i/j (include one copy key and i/j-1 pastes)
->>

<<-656	Coin Path    		29.3%	Hard	
given n elements A[i] is the cost to step on i. You can jump forward 1 to B steps. If A[i]=-1 you cannot jump. start from 0 you have to reach the last index with min cost. return the path (if tie return the lexi smallest path)
dp from right to left (left to right is also fine) dp with path information
to get the lexi smallest path, we need smaller index if tie, thus from right to left is more convenient.
very good question!
*****
->>

<<-691	Stickers to Spell Word    		43.5%	Hard	
given a list of stickers (can reuse) to spell a target string. return the min number of stickers.
- convert target string and stickers into hashmap (no order needed)
- greedy may not work.
- backtracking + memo, reduced target map for the same input. (dp but with up to 26 chars)
- DP top down is more convenient since we involves 26 numbers.
- bottom up dp: dp[string] using hashmap representing min stickers for string s. dp[string]=1+min(dp[string-sticki])
->>

<<-740	Delete and Earn    		48.9%	Medium	
delete num[i] and get score num[i] and delete all elements=nums[i]-1 or nums[i]+1
bucket sort and then we can apply house robbing dp algorthm
dp[i+2]=max(dp[i]+a[i],dp[i+1])
->>

<<-746	Min Cost Climbing Stairs    		50.7%	Easy	
cost[i]: you pay the cost and you then can climb one or two steps. You can start from 0 or 1.
dp. dp[i]=min(dp[i-2],dp[i-1])+cost[i], note this is the cost to reach i+1 or i+2
->>

<<-790	Domino and Tromino Tiling    		39.7%	Medium	
return the number of ways to reach 2xN tile.
dp: two kind of shapes to reach final status. L shape and I shape.
final anwer shall be I shape with n length.
->>

<<-808	Soup Servings    		40.5%	Medium	
given N of each two soup. 4 operations:
100A0B, 75A25B,50A50B,25A75B
each operation has the same probability.
return the probability that A is empty first, plus half the probability A and B empty at the same time.
dp top down. A empty first 1.0, A and B empty same time, 0.5
->>

<<-818	Race Car    		39.4%	Hard	
start at 0 and speed=1, 'A' position+=speed, speed*=2, ‘R： speed=-1/+1 position no change.
given a target position: retrn the shortest instruction length 
dp: pass the target and return, or in front of the target and back and then back
each time we get a smaller target solved or use top down approach.
go back 1 to n-1 steps to get the min number of steps.
->>

<<-823	Binary Trees With Factors    		36.1%	Medium	
given a list of integers, and each number can be used any number of times. Each non-leaf node's value shall equal to the product of their children.
return number of binary trees can make.
if a number as the root and it has factor i and j.
then dp[n]=dp[i]*dp[j], dp[n] represent the number of trees with root=n.
left and right shall be inside the set.
->>

<<-828	Count Unique Characters of All Substrings of a Given String    		46.5%	Hard	
return the sum of all substring's unique number. for example "leetcode" unique number is "ltcod"=5
- brutal force: try all substring and add the unique count.
- for ith char we get its left and right and its contribution is L[i]*R[i]
- dp: instead of calculating substrings, we count each char. 
dp[i] represents the sum of unique char in all substrings for the prefix string len=i.
when we add A[i], we add [i,i],[i-1,i],...[0,i] i+1 suffix strings.
if we did not see this char on the left, we add i+1
if we see this char at j, we add (i-j+1)
dp[i]=dp[i-1]+(i-f)-(f-s) f: the first index, s the second index. (from right to left)
because we have (i-f) more unique chars, (f-s) less unique chars.
- previous have 2 same char at j and k, k<j, dp[i-]1 includes extra substring from [k+1,i-1]
level: *****
->>

<<-837	New 21 Game    		35.2%	Medium	
you can draw number when you have points <K. each draw will randomly get a number in [1,W]. 
what is the probability to have <=N points.
- when you have k-w to k-1, you can draw 1 card.
- prob(x)=prob(x-w)+..+prob(x-1), window is W.
- for <=N we sum all the probability prob(N)+prob(N-1)+....prob(N-W+1)
sliding window + dp
->>

<<-873	Length of Longest Fibonacci Subsequence    		47.9%	Medium	
for current element, we can bind with previous element to form a fib pair.
first choice and then try next. typical dp pattern.
dp[i,j] the max length for the pair with ith and jth element as the ending two elements in fib series
good practice on dp why we need 2d dp.
->>

<<-887	Super Egg Drop    		27.1%	Hard	
given k eggs and 1 to n floors. There exist a floor F, such that above F egg will drop break.
return the min moves need to know F.
- drop below F, one move, k eggs, remain N-F floors
- drop above F, one move, k-1 eggs, remain X-1.
min max problem.
dp[k,n]=1+max(dp[k-1][i-1],dp[k,n-i]) for all i. KN^2
if we convert to dp[m,k] reprenting the max floor we can check using m move and k eggs
dp[m,k]=dp[m-1,k-1]+dp[m-1,k]+1
if egg breaks we can check dp[m-1,k-1] floors
if egg does not break, we can check dp[m-1,k] floors
 ->>
 
### two strings or arrays， edit distance (similar to 2d matrix walk)
- palindrome often is one string, but treated with two arrays.
- generally involves two strings or arrays, sometimes the string and the reverse string.
- dp pattern is generally more obvious and use 2d dp approach.
- treat like a 2d matrix walk, generally involves only previous col or row. (for space optimization)


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

<<-5	Longest Palindromic Substring    		29.9%	Medium	
- odd/even growing at all index.
- dp[i,j] represent the length for substr i,j, dp[i,j]=dp[i+1,j-1]+2
->>

<<-10	Regular Expression Matching    		27.1%	Hard	
'.' match any single char, '*' match 0 or more preceding elements
->>

<<-44	Wildcard Matching    		25.1%	Hard	
'?' match any single char, '*' match 0 or more characters (not preceding)
->>

<<-87	Scramble String    		34.2%	Hard	
randomly choose an index and swap them. check if s1 is a scrambled string of s2.
just similar to tree swap problem. we can recursively check left vs left + right vs right, left vs right + right vs left. (check all index)
dp or recursive approach.
****
->>

### two state interlace
- this generally onvolves limit number of elements with same color et al, or up down..
and you have two options generally. although you can have 2d dp, but using two state transfer would be more understandable.

<<-1186	Maximum Subarray Sum with One Deletion    		38.3%	Medium	
with at most one deletion.
- delete only occurs with negative number
- delete one will help connect left and right to make sum larger.
dp: del[i] and nodel[i] represent the max subarray sum deleting one or not delete one (not necessary i)
del[0]=0,notdel=A[0]
notdel[i]=max(A[i],notdel[i-1]+A[i]), connect to previous or start a new group
del[i]=max(notdel[i-1],del[i-1]+A[i],A[i]) delete i, not delete i, i start a new group
problem pattern: two options interlace. 
->>

<<-801	Minimum Swaps To Make Sequences Increasing    		39.0%	Medium	
two array A and B at the same size, you can swap A[i] vs B[i] so that A and B are both strictly increasing.
return the min number of swaps.
- dp approach: using swap noswap 
A[i-1]<A[i] && B[i-1]<B[i], swap both or do not swap either
A[i-1]<B[i] && B[i-1]<A[i], swap i-1 or swap i.
->>

<<-276	Paint Fence    		38.7%	Easy	
n posts, k colors, no more than two adjacent posts have the same color.
return the total number of ways to paint the fence.
dp use same color and different color. two states transfer.
->>

<<-376	Wiggle Subsequence    		39.9%	Medium	
return the longest length of wiggle subsequence
dp two states: up and down, up is the max length so far, down is the max length so far.
->>

<<-600	Non-negative Integers without Consecutive Ones    		34.3%	Hard	
given n, find the number of integers without consecutive ones.
dp: convert n to binary. previous is 1, then current need to be 0, previous 0, current one can be 1/0
dp0[i]: number of integers with ith bit is 0.
dp1[i]: number of integers with ith bit is 1.
dp0[i]=dp0[i-1]+dp1[i-1]
dp1[i]=dp0[i-1]
subtract the overcount.
->>

<<-964	Least Operators to Express Number    		44.5%	Hard	
given a positive integer x, use +-*/ and x to represent any number.
x/x->get x^0
x*x->get any x^i
so this is x-base number representation with - allowed.
dp: uses + or uses - to see which needs less operator. change + to - need to push forward a cf.
use pos or neg. 
->>

<<-975	Odd Even Jump    		41.7%	Hard	
given an integer array. from some starting index, you can make a series jump. 
odd jump 1,3,5.., even jump: 2,4,6..
odd jump at i: you can jump to the smallest index and smallest possible A[j]>=A[i]
even jump at i: you can jump to the smallest index and largest possible A[j]<=A[i]
possible that you can not make any jump.
good start index: mean you can jump to the end index.
return the number of good starting index.
dp approach:
- equivalent: do it reverse way from last index we jump to see which index we can go. (how do we get this? the base case is the last index)
- go higher or lower shall also reverse.
- put the elements right (visited) in ordered map so we can do binary search. (the later appeared duplicate can replace before element since we only care the first index)
- even odd interlaced. 
- even[i], odd[i] represent we can do an even/odd jump to this index.
base case: the last index odd[n-1]=even[n-1]=1
- answer is the odd[i] (the first jump start here)
->>

<<-978	Longest Turbulent Subarray    		46.6%	Medium	
turbulent: goes up/down... subarray.
dp: use two state inc,dec.
->>

### knapsack
- put elements in two or three options, to reach a target sum or max/min the sum with bound et al.
many times, you need convert to equivalent knapsack problem.

<<-494	Target Sum    		46.0%	Medium	
given an array you can assign + and - to each of the elment, so that the sum=target. return number of ways.
dp, knapsack P-N=target, P+N=tsum.
dp[i,w]+=dp[i-1,w-c] c is the element weight.
->>

<<-805	Split Array With Same Average    		26.5%	Hard	
split array into two parts (not necessary in order) making the two arrays have the same average.
check if it is possible.
take m elements to A, sum(Ai)/m=Sum(Aj)/(n-m) ->sum(Ai)*(n-m)=(Total-Sum(Ai))*m->S=mT/n
we are looking for a subset's sum = mT/n only for those mT%n==0.
now problem converts to pick m number with target sum=mT/n.
- backtracking with optimization (sort from largest to smallest and early return)
- since the length is small, we can use bitmask with m bits (go over all bitmask) C(n,m) choices. pretty large. using the trick to loop over all bitmask with same set bits (can apply binary search here)
- dp: dp[n,k,t] mean if we can get sum t using k elements with array len=n.
dp[n,k,t]=dp[n-1,k,t]||dp[n-1,k-1,t-A[n]), weakness the memory is too large!!
->>

<<-879	Profitable Schemes    		39.9%	Hard	
n people and a list of tasks with profit[i] and group[i]. The people cannot be reused again.
profitable scheme: profit>minProfit people used<=n.
knapsack: all >minProfit will be assigned to minProfit.
dp[p,g] number of good scheme using g people and profit p.
->>

<<-956	Tallest Billboard    		39.8%	Hard	
given an array of rod length, build two rods with equal largest length.
knapsack: a rod can be part a, or part b or discard. use +1, -1 and 0.
max target is totalsum/2.
dp[i,target] represent the max postive sum for i rods with target.
- we only add positive
- choose as a, sum+rod, choose as b, sum-rod, and final score shall be 0.
- maximize the positive score.
->>

<<-1049	Last Stone Weight II    		44.3%	Medium	
x==y, both destoyed, x!=y the smaller destroyed, keep y-x.
equivalent: knapsack to make positive sum >=tsum/2.
->>

### shortest distance problem
dijkstra: using priority_queue, src node to all other nodes with non-negative weight (similar to bfs with visited array to terminate when reach the target).
bellman-ford: using all nodes and edges, src node to all other nodes, can dealing with negative weight.
floyd-warshal: find all pair node shortest distance using dp.

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

<<-490	The Maze    		52.4%	Medium	
check if we can reach the destination.
bfs.
->>

<<-505	The Maze II    		48.1%	Medium	
the ball will not stop until hit a wall. given start and destination position. 
return the shortest distance to reach destination.
dijkstra: use the one direction distance as the weight.
->>
<<-499	The Maze III    		41.8%	Hard	
there is a hole in this maze. the ball will fall in the hole if rolling on the way.
return the min distance and the path to fall in the hole.
if there are multiple ways, return the lexi smallest (using "lurd" for 4 directions)
dijkstra.
->>

<<-743	Network Delay Time    		45.1%	Medium	
given a network of n nodes from 1 to n. given a list of edges [u,v,w], where w is the edge traveling time. We will send a signl from node k and return the tme when all the nodes received the signal.
max distance from source to all other nodes.
- dijkstra
- bellman ford (much slower than dijkstra, 1300 vs 100ms)
->>

<<-787	Cheapest Flights Within K Stops    		39.4%	Medium	
from src to dest within K stops. find the cheapest cost.
shortest distance from src to target 
- bellman ford, 
- dijkstra.
->>

<<-847	Shortest Path Visiting All Nodes    		52.7%	Hard	
a graph is given vector<vector<int>> adj format. You can start/stop at any node and revisit multiple times
return the shortest path visiting all nodes.
The graph indicates there might be some separate groups.
for a connected group.
dijkstra but with all weight the same so use queue to avoid pq.
bitmask dp (state compression)
->>

<<-1125	Smallest Sufficient Team    		46.9%	Hard	
giving a list of required skill and a list of people with skill set. find the min team with required skills. people<60, require skills<16
- convert the required skills and people skills to bitmask number.
- not-want skills are just ignored.
- if we use a,b,c the result is a|b|c. we are looking for the shortest distance to reach required skill number.
- shortest distance using bitmask dp: dp[status,[]] for each status we store the list of person. status up to 1<<n, n is the number of skills. people is the number of nodes. Using bellman-ford we try to relax all the dp.
- we can also use backtrack.
->>

<<-943	Find the Shortest Superstring    		43.2%	Hard	
given a list of words, return the shortest superstring connecting all words
- build a matrix for the pair distance (graph)
- then it becomes a travel's shortest distance problem (with traceback)
- using bitmask dp.

- starting from a node, edge weight is the length to add to reach the next node.
- bitmask can represent the choice of words, but there is no ordering info.
- dp[i,status] represent the min distance ending with node i.
- in the status, we repeatedly choose each as the ending node j: dp[j,prev]
dp[i,status]=min(dp[j,prev]+dist[j,i]
ie. try any node as the last node, and try any node in previous status as the previous ending and connect them and releax the edge.
answer is the min(dp[x,0xffff]) x can be any node.
base case: 1 word
two words, use each word as the end and get the shortest distance.
->>

### memoization

<<-471	Encode String with Shortest Length    		48.4%	Hard	
encode the string: k[str] means str repeats k times. 

Input: s = "abbbabbbcabbbabbbc"
Output: "2[2[abbb]c]"

idea:  dp top down: try all prefix and check number of repetition and then divide into prefix repeat and suffix two subproblems
repeat 1: s1+sub(suffix)
repeat k>1: k[Subprefix]+SubSuffix. memoization.
store the string s min length encoded string in hashmap.
try each prefix and try to encode it and reduce to subproblem.
similar to divide and conquer.
level: *****
->>

<<-1387	Sort Integers by The Power Value    		70.6%	Medium	
power of x is the step to transform to 1: even x/2, odd 3*x+1. need do a range.
use dp to record solved smaller problems. use hashmap to do memoization.
->>

<<-1483	Kth Ancestor of a Tree Node    		29.5%	Hard	
a tree wit n nodes from 0 to n-1. Each node is given the parent node. root is 0.
query the node's kth ancestor.
- dfs will TLE.
just similar to super power, we need use some trick called binary lifting.
The idea is:
dp[i][node]=dp[i-1][dp[i-1][node]], node's 2^i parent = node's 2^(i-1) parent of node's 2^(i-1) parent. 2^i=2^(i-1)*2^(i-1), kind of binary representation.
dp[i,node] represents the 2^i parent of node.
we store the parent for each node's parent 2^0 parent->2^1 parent->2^2 parent->..... up to log2（N）

when search for kth parent:
for example k=5 (0b101), we first get 2^0's parent, and then get parent's 2^2 parent and that is it!
->>

### dp dealing with subarray or intervals

removing a subarray and then left and right connected. Extremely hard to track the array changes or no good data structure to do it. This is one of the difficulty dp subject.

<<-546	Remove Boxes    		43.7%	Hard	
remove box with same color and get k*k points (k is the number of boxes removed)
return the max score.
Similar to 664, but now the number cares.
dp[i,j,k] max points among [i,j] with k same color boxes attached to it. 
->>

<<-664	Strange Printer    		41.0%	Hard	
printer can only a sequence of same char each time.
later printing will cover previous printing.
given a string, return the min number of prints
reverse do it: remove all same char substr.
for example "aba", first remove "b"->"axa"->"" consider all same char is one group.
dp problem: the first choice is not greedy but will affect choices later.
delete a group will connect the left and right.
- all consecutive chars does not matter, we can reduce to one single char (does not depend on the number)
- dp[i,j] represents the min number of prints
dp[i,j]=1+dp[i+1,j]
dp[i,j]=min(dp[i+1,m-1]+dp[m,j]) if s[i]==s[m] //we erase i+1 to m-1 so i and m connects to the right (erase one char).
->>

<<-1000	Minimum Cost to Merge Stones    		40.4%	Hard	
each time you merge k consecutive piles into one pile at the cost of the total sum.
- prefix sum so we can get range sum in O(1)
- dp[i,j] deals with subproblem to merge i to j. dp[i,j]=min(dp[i,j],dp[i,m]+dp[m+1,j])
- also there is a restriction the length (n-1)%(k-1)==0 must be satisfied.
- final answer is dp[0,n-1]
->>

<<-1246	Palindrome Removal    		45.7%	Hard	
each time you can remove a palindrome subarray. return the min number of removals to make it empty.
this is not the same as burst balloon since the last could be a subsequence left.
think from top down, our first step choose s[i,j], we need to connect the left and right, seems very hard to track.
but if we define dp[i,j] as the min removals for s[i,j] then it is much easier
answer is dp[0,n-1]. dp[i,j]=min(dp[i,j],dp[i,k]+d[k+1,j]) if we split at k.
upper triangle dp.
pattern: palindrome, treat as substr(i,j) or two strings. similar to burst balloon.
->>

<<-1000. Minimum Cost to Merge Stones
each time merge k consecutive piles of stones into 1 pile with the cost=sum of stones.
dp[i,j] represent the min cost to reduce to k piles, then dp[i,j]=min(dp[i,k],dp[k+1,j]) 
->>

<<-312. Burst Balloons
burst a balloon, and get the score: product of left and right and the balloon.
idea: choose i as the last balloon to burst and its left and right are virtual balloon with value 1. And then balloon i becomes the right and left for two subproblem.
- top down: is more straightforward.
- bottom up: loop over all left and right will not work, but we need to do one balloon, 2...until n balloons. (base case is one balloon left)
->>

### dp dealing with array partitions
generally dealing with subsequence or subarray and divide into k groups to get max/min scores.

<<-472	Concatenated Words    		44.9%	Hard	
given a list of words, return words which can be concated by other words in the list.
a problem of dp: with all shorter words ahead (sort by length). and reduce to problem: a string can be concat by dictionary words (try prefix).
similar: word break.
level: 5
->>

<<-140	Word Break II    		33.7%	Hard	
return all possible break using the dictionary words.
dp- record the break position and then traceback using dfs.
(similar to 132/131)
->>

<<-139	Word Break    		41.0%	Medium	
check if it is breakable.
->>

<<-132	Palindrome Partitioning II    		30.7%	Hard	
min cuts to partition the string into palindrome strings.
dp[i] represent the min cuts to make the string with length i to be palindrome.
we try every position and grows. (more like a array partition problem or dfs approach)
- odd length: 
- even length 
cut[i]=min(cut[i],cut[j]+1) (i,j) is a palindrome.
->>

<<-131	Palindrome Partitioning    		49.2%	Medium	
return all possible palindrome partition.
backtracking: 
->>

<<-1278	Palindrome Partitioning III    		60.2%	Hard	
return the min number of characters to replace so that string can be split into k palindrome substr.
dp[i,k] represent the min cost for length i and k parts.
dp[i,k]=min(dp[j,k-1]+cost(j,i))
->>

<<-53	Maximum Subarray    		47.2%	Easy	
connect or not.
->>

<<-152	Maximum Product Subarray    		32.4%	Medium	
subarray with max product.
- we need keep min and max at the same time since negative will change min/max
- need to attach to previous or start a new one (when there is 0)
->>

<<-813	Largest Sum of Averages    		50.6%	Medium	
partition the array into at most K groups. the score is sum of group average.
return the max score.
dp: element either append to previous or start a new group
->>

<<-918	Maximum Sum Circular Subarray    		34.0%	Medium	
find subarray max sum.
dp: connect or start a new group, one max one min problem.
->>

<<-1043	Partition Array for Maximum Sum    		66.4%	Medium	
partition into subarray of length <=k. after partitioning, the subarray's value all become the max in the subarray, return the largest sum.
- dp, dp[i] represent the max sum for length i. current element can bind to previous k-1 elements.
very similar to min difficulty in job scheduling.
->>

<<-1105	Filling Bookcase Shelves    		57.9%	Medium	
given book height and width in order, and bookshelf width. find the min height.
dp, either start a new layer or attach to previous layer.
->>

<<-1335	Minimum Difficulty of a Job Schedule    		58.5%	Hard	
given a list of job difficulties. the job shall be scheduled in order. Each day you have to do at least one job. 
job difficulty for one day is the max of all the tasks on that day
schedule difficulty is the sum of difficulty for all the days.
return the min schedule difficulty for d days.
dp: you can assign one to n-d jobs to first day. 
dp[i,d] the min difficulty for i tasks and d days
to add one more task into it, either attached to previous or add a new day
dp[i,d]=min(dp[i-1,d]+delta,dp[i-1,d-1]+diff[i])
attached to previous, i-1,i-2....
top down:
for the first day we have option:</br>
1 tasks, leaving n-1 </br>
2 tasks, leaving n-2</br>
...</br>
n tasks, leaving 0.</br>
that is to say the dp problem is as belows:</br>
min(max(A[0..i])+sub(i+1,d-1))</br>
->>

<<-1478	Allocate Mailboxes    		55.1%	Hard	
n houses on a street, place k mailboxes. return the min total distance.
base case: 
- 1 mailbox, two houses, anywhere between.
- 1 mailbox, multiple houses, median position.
dp[i,k]: i houses, k mailboxes.
- if k==n, then each house has one mailbox, total distance is 0
- ith house can belong to previous group dp[j,k] or start a new group dp[i-1,k-1]
- calculate group total distance would be two pointer two end distance.
->>

### optimization

optimization problem is for max or min. Dp can solve most of them, but bfs, greedy, pq et al can also be the candidates.

<<-983	Minimum Cost For Tickets    		62.5%	Medium	
three prices one day pass, 7 day pass, 30 day pass.
given a list of days, return the min cost needed.
dp: you have 3 options to cover, dp[i] is the min prices covering i items in the front.
->>

<<-1039	Minimum Score Triangulation of Polygon    		49.9%	Medium	
given n-sided polygon, convert to n-2 triangles. the value of triangle is the product of the 3 vertices.
return the min score sum.
dp[i,j] represent the min score for trianglize i to j (clockwise) and connect i,j.
dp[i,j]=min(dp[i,j],dp[i,k]+dp[k,j]+A[i]*A[j]*A[k]) i<k<j.
since k>i. we need loop i reversely.
->>

<<-1191	K-Concatenation Maximum Sum    		25.4%	Medium	
repeat the array k times. find the max subarray sum.
k=1: max subarray sum (current element append to previous or start a new one)
k=2: max subarray sum in circular array (find the min and max)
k=3: k-2 subarrays contribute the sum only.
problem pattern: subarray max sum, start a new group or belong to previous group
->>

<<-1235	Maximum Profit in Job Scheduling    		46.0%	Hard	
given a list of job {start,end,profit} return the max profit you can get. (no simultaneous job can take)
- sort the job by ending time
- then we use dp knapsack to include current job.
- answer is the max among all dp. or knapsack to convery the max till the end.
pattern: use or not use and propagate the results to next.
->>

<<-1239	Maximum Length of a Concatenated String with Unique Characters    		48.7%	Medium	
- use bitset try all combination O(2^n)
- use bitset represent each dictionary word. and then use dp for longest combination, try to add current set to previous set and get the longest one.
->>

<<-1240	Tiling a Rectangle with the Fewest Squares    		51.3%	Hard	
given a mxn matrix, return the min number of squares with integer side length to fill the rect.
from the example, we see greedy does not work.
we only know that the side length shall from 1 to min(m,n).
suppose we first choose a square len: then leave:
A: len x m-len
B: n-len x m-len
c: n-len x len
AB forms a bigger rect n x m-len
BC forms a bigger rect n-len x m.
11x13 gives wrong result since we need consider the L shape as a whole.
->>

<<-1326	Minimum Number of Taps to Open to Water a Garden    		45.7%	Hard	
There are n+1 taps located at 0,1....n with range[i]. it can cover [i-range[i],i+range[i]]. 
return the min number of taps to water the garden [0,n].
greedy: sort with start, and use the one extended right the most.
dp: dp[j]=max(dp[j-range[j]]+1)
similar to 1024 video stitching.
->>

<<-1340	Jump Game V    		58.3%	Hard	
given array and d. You are at 0, at each position you can jump back or forth in [1,d] steps, but you need make sure all elements in (i,j] shall be <A[i].
return max number of nodes you can visit.
dp: top down.
->>

<<-1363	Largest Multiple of Three    		33.7%	Hard	
given a list of digits, return the largest multiple of 3 that can be formed using some of the digits.
- sort descending order
- %3 0,3,6,9 can be used anytime, [1,4,7]%3=1 [2,5,8]%3=2, these two shall be combined.
- greedy approach: total_sum%3==0, use all.
total%3==1, remove the smallest digit%3==1 or two smallest %3=2
total%3==2, remove one the smallest digit%3==2, or two smallest %3=1
- dp approach: sort descending, dp[3]: the largest number with %3=0,1,2.
d%3==0, append it to all.
d%3==1, append dp[1] to make it 2, append dp[2] and make it dp[0]
->>

<<-1262	Greatest Sum Divisible by Three    		48.4%	Medium	
similar to 1363. 
- sort so we can remove smallest
- %3 and get the tsum%3 too.
- remove 1 or 2 depending on tsum%3
also we can use same dp approach as in 1363.
->>

<<-1388	Pizza With 3n Slices    		45.0%	Hard	
given 3n slices in whole pizza, you can pick one and friend a takes next slice clockwise, friend B takes next slice anticlockwise.
dp: similar to house robber. you take i, the i-1 and i+1 is taken. You have to take exactly n/3 slices.
convert circular to two linear dp problem.
->>

<<-1402	Reducing Dishes    		72.3%	Hard	
given n dishes, each dish prepare time is 1. The likeness of the dish is the time (including the previous) or the order * likeness. return the max score. You can discard some dishes.
so it matters the order of dishes.
greedy: we shall leave the highest liked order the last. so sort it. then we try to get sum(A[i]*j) maximized.
dp[i,j] max score for i dishes with j cooked.
cook dish i: dp[i-1,j-1]+like[i]*j
discard dish i: dp[i-1,j]
our answer is the global max.
->>

<<-1449	Form Largest Integer With Digits That Add up to Target    		43.1%	Hard	
cost to paint 1 to 9 are given. Return the largest number given the target cost. (exact cost)
if there is no way, return "0".
- number of digits prefer the digit. (longer length larger number)
- after length is fixed, find number of each digit.
- knapsack with repetition, the number never changes, but the target gets smaller when we make a choice. 1d knapsack.

```cpp
string dp[5001] = {};
string largestNumber(vector<int>& cost, int t) {
    if (t <= 0)
        return t == 0 ? "" : "0";
    if (dp[t].empty()) {
        dp[t] = "0";
        for (int n = 1; n <= 9; ++n) {
            auto res = largestNumber(cost, t - cost[n - 1]);
            if (res != "0" && res.size() + 1 >= dp[t].size())
                dp[t] = to_string(n) + res;
        }
    }
    return dp[t];
}
```
loop from 1 to 9 we prefer larger number with same size.
->>

<<-256	Paint House    		52.7%	Medium	
n houses in a row, color with rgb 3 colors. The cost painting is given a matrix n x 3. no two adjacent color could be the same.
dp[i,k], same as paint house II.
->>

<<-265	Paint House II    		45.2%	Hard	
n houses in a row painted with k colors. the cost painting a house using a color is different, given by matrix.
no ajacent house can be the same color return the min cost to paint all houses.
dp[i,k] means the min cost to paint i houses with ending color k.
dp[i,k]=min(dp[i,k],dp[i-1,j]+cost[i-1,k])
->>

<<-1473	Paint House III    		48.7%	Hard	
given m houses and to be colored with n colors from 1 to n. If the house is already colored, then you cannot paint again.
house[i]=0 not colored, >0 is the color.
cost[m,n] the matrix for color house i using color j+1
given a target, return the min cost to paint the houses into target neighboring groups (neighboring groups are same colored neighbors).
dp, top down is easier:
dp[m,n,target] represent the min cost to paint m houses n colors with target groups.
if current colored, we update the group number
if not colored, we try all different color, and solve the subproblem
->>

<<-1531	String Compression II    		32.2%	Hard	
running length compression.
given string s and number k, you need delete at most k characters such that run length compression is minimized.
- we shall build the RLE string on the fly. keeping the original string unchanged.
- greedy: we keep the most frequent char in range [start,n-1] and delete all other chars. ie. making a range the same chars.
- dp[start,k] subproblem: from start to j (j from start to n) only keep the most frequent chars.
->>

<<-1548	The Most Similar Path in a Graph    		55.3%	Hard	
n cities and m bidirectional roads. each city has a name with 3 chars. It is a connected graph, ie. from city x you can go to any city y.
given a target path, find a path of the same length with the min edit distance (number of different chars between two strings). if multiple paths exist, return any of it.
naive approach: try each node as the start and do dfs for n nodes and compare the edit distance. using memoization
- backtracking
- optimize using memoization dp[start][i][j] the min edit distance using the airport start as the ith city. j is dependent on i j=m-1-i, so we only need 2d. But our answer shall be the path.
use dp[i,j] for the min edit distance using city i as the index j in the target path.
dp[i,j]=min(dp[k,j-1]+diff) k is the node which i connects.
note the cost of two path is the number of city which is different name, not the characters!!!
using prev matrix to record each node's previous index to recover the path.
- dijkstra: shortest path problem. use the edit distance as the edge weight.
use forward update.
add the base dp into pq (min heap)
add all its child into heap (if can reduce the distance)
->>

<<-1425	Constrained Subsequence Sum    		44.6%	Hard	
given a integer array nums, and k. find the max sum of a non-empty subsequence such that the neighboring elements in the sequence satisfy i<j and j-i<=k.
- dp with sliding window max. since in the window k, we have to choose one element.
each element we can choose or discard.
we define dp[i] as the max sum ending with A[i], then:
dp[i]=max(dp[j]...dp[i-1])+A[i], sliding window max using pq or deque.
->>

<<-1696. Jump Game Vi.
given a list of numbers, you are located at element 0. Each time you can jump 1 to k steps. return the max sum you can get from 0 to n-1 element.
dp[i]=max(dp[i-1]...dp[i-k+1])
using priority_queue 
using monotonic deque.
greedy does not work!!
->>

<<-1553	Minimum Number of Days to Eat N Oranges    		28.6%	Hard	
n oranges, you can:
- eat one 
- if n is even you can eat n/2
- if n/3==0, you can eat 2*n/3
approach 1: bfs it will reach to 0 soon
approach 2: dp, the problem is -1, which needs to solve all subproblems. but we only need solve a very small portion. n%2+dp[n/2],n%3+dp[n/3]
using hashmap for dp table.
->>

<<-1611	Minimum One Bit Operations to Make Integers Zero    		56.6%	Hard	
given a number n, you can
- change 0th bit
- change ith bit if (i-1)th bit is 1 and (i-2)th to 0th bit are all 0.
return min number operations to reduce n to 0.
bfs will expand exponentially and will TLE.
consider case 2^k and try to find the relation:
001->0, 1 ops
010->011->001->000, 3 ops
100->101->111->110->010->011->001->000, 7 ops
2^k, need 2^(k+1)-1 ops.
1xxxx->....->11000->10000->....->0 The number is decreasing.
1xxxx to 11000 needs subprob(1xxxx^11000):
if it is 11xxx, then it is same to convert xxx to 000
if it is 10xxx, then it is same to convert 0xxx to 1000 and the op needed is subprob(1000)+subprob(xxx). 1000 to 0 is same as from 0 to 1000.

Now suppose we want to know the number of operations for 1110 to become 0. We know it takes 15 operations for 0 to become 1000, and it takes 4 operations for 1000 to become 1110. We get the solution by 15 - 4.
Note that 4 here is the number of operations from 1000 to become 1110, which is the same as the number of operations from 000 to 110 (ignoring the most significant bit), and it can be computed recursively. The observation gives us: minimumOneBitOperations(1110) + minimumOneBitOperations(0110) = minimumOneBitOperations(1000).
->>

### state transfer

<<-514	Freedom Trail    		44.6%	Hard	
given a ring with given string on it, and another key to spell. You can rotate clockwise or counter-clockwise.
if the char is rotated to 12:00 you need press button in the center. return the number of moves needed to spell the key.
- spell the key one char by one char.
- there could be multiple chars on the ring matching our key char. select one will affect our later spelling.
- using hashmap to record each char's index
- we try all the index for the key char. 
- dp[i,j] represent the min steps to rotate jth key with ith char at 12:00
dp[i,j]=min(d+dp[ind[i],j+1]+1)
->>

<<-1230	Toss Strange Coins    		49.3%	Medium	
n coins, each coin given probability facing up. return the prob for target coins up.
dp[i,k]=p*dp[i-1,k-1]+(1-p)*dp[i-1,k]
pattern: two options, probability.
->>

<<-741	Cherry Pickup    		34.6%	Hard	
go from top left to bottom right and return to bottom left.
use two branches at the same time.
->>
<<-1463	Cherry Pickup II    		66.2%	Hard	
two robots on the top left and top right and each time goes down, leftdiag rightdiag or down. return the max cherrys can pick.
dp: the two robot status is defined as dp[i,j1,j2] is the layer and j1 and j2 are the column index for robot 1 and robot 2. choose the max from previous layer 9 possible combinations. do not go over board.
->>

<<-1320	Minimum Distance to Type a Word Using Two Fingers    		62.6%	Hard	
given a matrix from A to Z. given a word to type and return the min total distance to type the word.
dp[i,l,r] represent the min total distance to type ith char, with left finger at l, and right finger at r.
current char use left finger dist(f1,c)+dp(i+1,c,f2)
current char use right finger dist(f2,c)+dp(i+1,f1,c)
base case: the first move shall be 0.
->>

<<-1547	Minimum Cost to Cut a Stick    		51.2%	Hard	
a wood log labeled 0 to n with n units. 
given a array of cut positions, you should perform the cut in order, you can change the order of cuts.
the cost of one cut is the length of the wood. The total cost is the sum of all cut costs.
n<=10^6, cuts<=100.
return the min total cost.
cut at i wood length n, and has two subproblems, one is length i, other is n-i.
length i and cut positions <i.
length n-i and cut positions >i.
pr-pl+dp[pl,cuts[i],l,i]+dp[cuts[i],pr,i,r]
memoization: 4d dp the memory is too large. but actually we only need pl and pr.
since the cut will be spearated using this two parameters. still too large.
- we can add 0 and n to the cut positions so that pl and pr can be directly represented by cut.
- thus we define dp[i,j] to be: wood from cut[i] to cut[j], cut positions are from i to j.
note the left and right end cannot serve as a cut position.
```cpp
    int minCost(int n, vector<int>& cuts) {
        cuts.push_back(0);
        cuts.push_back(n);
        sort(begin(cuts),end(cuts));
        int m=cuts.size();
        vector<vector<int>> dp(m,vector<int>(m,INT_MAX));
        return helper(0,m-1,cuts,dp);
    }
    int helper(int l,int r,vector<int>& cuts,vector<vector<int>>& dp){
        if(l+1>=r) return 0;
        if(dp[l][r]<INT_MAX) return dp[l][r];
        int ans=INT_MAX;
        for(int i=l+1;i<r;i++){ //cut at cuts[i]
            ans=min(ans,cuts[r]-cuts[l]+helper(l,i,cuts,dp)+helper(i,r,cuts,dp));
        }
        return dp[l][r]=ans;
    }
```	
->>

### ending with 
<<-1682. Longest palindromic subsequence II
problem: palindromic subsequence needs to be even length, neighboring chars cannot be the same (except the mid pair).
dp[i,j,k] using 26 chars.
->>

<<-413	Arithmetic Slices    		58.3%	Medium	
- for subarray, if we find subarray length=L, the slices would be: 1+2+...+L-2
- also can use dp, dp[i+1]=dp[i]+1 if 2*A[i-1]=A[i]+A[i-2]
->>

<<-446	Arithmetic Slices II - Subsequence    		33.0%	Hard	
given a list of numbers, find the number of arithmetic subsequence.
dp: dp[i,diff] represent the length of arithmetic sequence ending with A[i] with the difference = diff.
dp[i,diff]=dp[j,diff]+1, ans+=diff[i,diff]-2 ->hashmap for dp array.
->>

<<-795	Number of Subarrays with Bounded Maximum    		46.9%	Medium	
return number of subarrays whose max is in the range [L,R].
- each element can be a max of some subarray. so we discard those elements not in the range
- to get the range for A[i] as max, we need to get previous greater and next greater which can be done using monotonic stack.
approach 2: use two pointer to define valid region left and right.
approach 3: dp, dp[i] represents the max number of subarray ending with A[i].
A[i]<L, append to previous dp[i]=dp[i-1]
A[i]>R, dp[i]=0;
[L,R]: can combine with all previous after the previous invalid.
->>
<<-903	Valid Permutations for DI Sequence    		53.8%	Hard	
given a DI string, return number of permuation using 0 to n.
dp:
- string empty, there is only 1 way.
- dp[i,j] represents number of permutation using 0 to i ending with j.
how do we get dp[i,j] from previous solution?
since j is in the range [0,i], if we want to end with j, we need put i in front.
if 'D', previous can be in range [j+1,i]
if 'I', previous can be in range [0,j-1]
this can be done by shifting all numbers >=j by 1 (shift them to j+1...i), which has the same number of permutation. (understand this to get the transfer function).
1032, if we want to end 2, we need first change to 1043 then append 2 -> 10432
->>

<<-467	Unique Substrings in Wraparound String    		35.9%	Medium	
s is infinite repeat of a-z. given a string t, find the number of unique substring of t is present in s.

we only need to find the longest substr satisfying the adjacent different is 1 or 25. ending with different char.

dp: ending with for unique substr. common trick to do unique problems (ending with different elements and length) 
->>

### count number of ways

<<-115	Distinct Subsequences    		39.1%	Hard	
find the number of distinct subsequence of s equals t.
dp[i,j] represent the number of distinct sequence for s length i and t length j.
if s[i-1]!=t[j-1] dp[i,j]=dp[i-1,j] (need delete the char in s.)
else we can keep or delete this char: dp[i,j]=dp[i-1,j-1]+dp[i-1,j]
->>

<<-518	Coin Change 2    		51.0%	Medium	
cions can be reused, number of ways to reach up the total money
reusable -> 1d dp reduced money total. dp[w]+=dp[w-c] for all c.
->>

<<-552	Student Attendance Record II    		37.0%	Hard	
a valid record: contain no more than 1 'A' no more than two continuous L.
given length n, return number of valid records.
dp: do not consider 'A', ending with P, ending with L. (this is not easy to get correct relation)
dp[i,P]=dp[i-1,P]+dp[i-1,L]+dp[ 
dp[i,L]=dp[i-2,P]
or we can add P, PL, PLL to previous valid string. dp[i]=dp[i-1]+dp[i-2]+dp[i-3]
then we add A into any position into two subproblem.
->>
<<-551	Student Attendance Record I    		46.0%	Easy	->>

<<-576	Out of Boundary Paths    		35.5%	Medium	
given a mxn grid, initial position (i,j) and move at most N times. find out the number of path out of grid boundary (move to adjacent cell or out of boundary)
top down dp: dp[N,i,j] is the subproblem.
->>

<<-629	K Inverse Pairs Array    		31.4%	Hard	
given 1 to n, find how many different permutations withe exactly K inverse pairs (i<j and A[i]>A[j])
dp[n,k] represent the number of ways to have n elements k inverse pair
when adding n: you can add it to 1 to n position and the number of inversed pairs:
dp[n-1,k],dp[n-1,k-1]...
->>

<<-634	Find the Derangement of An Array    		40.3%	Medium	
given n integers from 1 to n, derangement means no element is in its original position.
find the number of derangement.
try dp: dp[i] represents number of derangement for length i.
we add ith element, you can put Ai in any previous location. suppose we put it on j. (all other not changed instead shift right). if we put at ith position, i is at the end, we get dp[n-2] otherwise dp[n-1] (n-1)*(dp[n-2]+dp[n-1])
->>

<<-639	Decode Ways II    		27.2%	Hard	
A-Z corresponds to 1 to 26. * can be treated as 1-9.
digit string with *, return number of decoding ways.
combine to previous char, or no.
dp[i]=a*dp[i-1]+b*dp[i-2]
->>
<<-91	Decode Ways    		25.5%	Medium	
no *. dp similar, notice contains some illegal case.
->>
<<-920	Number of Music Playlists    		47.4%	Hard	
given N different songs, want to make a list with length L:
- Every song is played at least once
- A song can only be played again only if K other songs have been played
Return the number of possible playlists.  
0<=K<N<=L<=100

dp approach: 
- dp[n,l,k] n songs with list L, and k restraints.
- used n-1 songs, for the l-1 list, the not appeared song can be added. 
dp[n,l,k]=n*dp[n-1,l-1,k]
- if l-1 used n songs already, then we have n-k choices 
dp[n,l,k]=(n-k)*dp[n,l-1,k]
->>


<<-935	Knight Dialer    		45.9%	Medium	
return the number of ways on the phone pad.
build the adjacency matrix and do a dfs like (dp from previous group)
->>

<<-940	Distinct Subsequences II    		41.5%	Hard	
count the number of distinct non-empty subsequence of string s.
- ending with different char with different length are distinct.
- dp[i,c] represent the number of distinct subsequence for length i and ending with c.
- dp[i,c]+=d[i-1,x] x can be any char, add 1 for a single char ending with c.
->>

<<-1397	Find All Good Strings    		37.9%	Hard	
given string s1,s2,and evil. a good string will >=s1 and <=s2 and does not contain evil as a substring.
return the number of good strings.
- answer=less(s2)-less(s1)-less(s2,evil)+less(s1,evil) (less(s1,evil) all string <s1 with evil inside)
- less(s) get the number of strings <s. consider it is a base 26 system, this is the number.
- less(s,evil) evil can be anywhere and it divide into two subproblem: left and right. (note left right still can contain evil as a substring).
dp:
For those strings <= s, and of same length as s,
a: counts the number of strings t, with prefix = evil, i.e. t = evil + t[len(evil):].
b: counts the number of strings t, such that t[1:] contains evil.
c: counts the number of strings t with prefix = evil, and t[1:] contains evil.
By incllusion exclusion principle: helper(s) = a + b - c.
so hard!!!
->>

<<-1269	Number of Ways to Stay in the Same Place After Some Steps    		43.1%	Hard	
You can jump oe step left or right or stay.
return the number of ways to stay in the same place 0 after k moves
dp[i,k] could come from dp[i-1,k-1], dp[i+1,k-1] or dp[i,k-1]
->>

<<-1259	Handshakes That Don't Cross    		53.9%	Hard	
even number of people on a circle suppose 1 to n. number of ways that handshaken no cross.
if i and j handshake, then divide into:
(i+1)%n to (j-1+n)%n suppose we have m1 people
(i-1+n)%n to (j+1)%n n-m1-2 people
dp[i] represent the number of ways for i-pair of people.
dp[i]+=dp[j]*dp[i-1-j]
pattern: make a choice and split two smaller subproblems.
->>

<<-1220	Count Vowels Permutation    		53.8%	Hard	
Each character is a lower case vowel ('a', 'e', 'i', 'o', 'u')
Each vowel 'a' may only be followed by an 'e'.
Each vowel 'e' may only be followed by an 'a' or an 'i'.
Each vowel 'i' may not be followed by another 'i'.
Each vowel 'o' may only be followed by an 'i' or a 'u'.
Each vowel 'u' may only be followed by an 'a'.
construct a string of length n.
dp: convert the relation to before. And then use 5 different vowels.
dp[i,c] or use 5 different array.
pattern: dfs similar, ending with
->>

<<-1416	Restore The Array    		36.2%	Hard	
an array of integer is printed without space. each number is in the range [1,k] and there are no leading 0s. return the number of possible array.
- dp: current digit can attach to previous up to 9 digits. dp[i]+=d[j]
->>

<<-1359	Count All Valid Pickup and Delivery Options    		57.1%	Hard	
pick up always in the front of delivery
for the first positions, we have n options, after we choose the first position, we have 2n-1 options for the 2nd position.
or assume we have finished i-1 pairs, now we want to insert ith pair, the pick up has 2i-1 options, and delivery 2n options, but we have to insert after pickup so it is i. i*(2i-1)
dp[i]=i*(2i-1)*dp[i-1]
->>

<<-1575	Count All Possible Routes    		58.4%	Hard	
array represent the location of cities. given start and end city and fuel. fuel is reduced by the distance. return the number of possible routes.
idea: dp, choose any city as the first; dp[i,fuel] number of routes start at city i with fuel to dest. destination is fixed but start for subproblem is changing.
dp[i,f]+=dp[k,f-dist(i,k)]
->>

<<-1444	Number of Ways of Cutting a Pizza    		53.4%	Hard	
a rect with m x n. 'A' indicates an apple. '.' means empty. given a integer k, you can cut it into k pieces using k-1 cuts.
each cut you can choose horizontal or vertical. If you choose vertical, give the left part to a person. if you cut horizontally, give the upper part to a person. return the number of ways such that each piece contains at least one apple.
- dp, each step we have two options.
- ith cut we shall make sure the left has at least one apple, and remaining k-i apples. for vertical similar.
- dp[k,i,j] kth cut and cut at i,j. using simple dp to get the postfix sum at each position.
- then backtracking with two options using memoization.
->>

<<-1524	Number of Sub-arrays With Odd Sum    		38.9%	Medium	
return %10^9+7.

dp: 
if current number is odd, then odd[i]=1+even[i-1], even[i]=odd[i-1]
if current number is even then even[i]=1+even[i-1], odd[i]=odd[i-1]
odd[i]: number of subarrays ending with element A[i]
even[i]: number of subarrays ending with element A[i]
->>

<<-1301	Number of Paths with Max Score    		37.7%	Hard	
a square matrix, E the end, S the start, 1-9 the score, x obstacles. 
two dp problems:
max score you can get.
number of paths on the max score.
->>

<<-1621	Number of Sets of K Non-Overlapping Line Segments    		41.3%	Medium	
give a list of positions on a line and number k. number of ways to draw k non-overlapping segments, each segment at least has two points.
dp approach: dp[k,i]: number of points<2 all 0. k=1, C(n,2)
each position we can add to previous segment or start a new one.
append: dp[k,i]=dp[k,i-1]
new segment: dp[k,i]+=dp[k-1][j-x]
use prefix sum for dp[k-1][j-x] to reduce complexity.
->>

<<-1569	Number of Ways to Reorder Array to Get Same BST    		50.0%	Hard	
given array a permutation of 1 to n. Insert the elements in order to BST. find the number of different ways to reorder nums so that we get the same BST as nums.
Key information: BST.
- use the first element as the root, and smaller one as the left, bigger ones as the right, both as a subproblem.
- can choose left first or right first.
- for left first, assuming we have m nodes in left, and n nodes in right, then it is equivalent to interleave the left array into right array. C(m+n,n)
nleft*nright*comb(n-1,left)
recursive, combination, modular inverse, pascal triangle (using dp to get combination)
->>


<<-647. Palindromic Substrings
problem: count number of palindromic substring.
- non-dp: for all possible position, expand and get the count
- dp: dp[i,j] number of pal-string inside. then dp[i,j]=self+dp[i,j-1]+dp[i+1,j]-dp[i+1,j-1]
->>

<<-1411	Number of Ways to Paint N × 3 Grid    		60.6%	Hard	
dp: using the first row and derive the next row. limited variations
->>

<<-1692. Count ways to distribute candies
n candies from 1 to n, distribute into k bags, each bag number of candies>=1.
bag order and candy order does not matter.
return the number of ways to distribute
- base case: k=1, dp[i,k]=1
- from i-1 candies to i candies, we have one more candy to place into k bags, k*dp[i-1,k]
- ith candy put into kth bag, dp[i-1,k-1]
dp[i,k]=k*dp[i-1,k]+dp[i-1,k-1]

another approach:
k=2: we can put 1 to n-1 candies into first bag, thus C(n,1)+C(n,2)+...+C(n,n-1), redundant /2.
need some math to derive to above solution.
->>

<<-1155	Number of Dice Rolls With Target Sum    		47.7%	Medium	
dice has 1 to f faces, find number of ways for n dices to get sum target.
dp[i,j]+=dp[i-1,j-x] x is from 1 to f.
- number of paths to current state.
level 3
->>

<<-1223	Dice Roll Simulation    		46.8%	Medium	
given an array Max[i] the max allowed consecutive dice number i.
return the distinct sequence rolling n times (or n dices)
dp[i,j,k] ith dice rolls number j with k consecutive same number as j.
dp[i,j,1] means previous can be 0 to 6 except j. add all of them dp[i-1,x,m], x and m shall loop all cases.
dp[i,j,k] k>1, means previous must be j.
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

### 2d matrix walk
generally current value only depends on the [i,j-1],[i-1,j] and [i-1,j-1].
- in some cases to save space, we need to store previous row and previous column.
- in some cases we need only keep the previous whole n elements.

<<-799	Champagne Tower    		43.8%	Medium	
dp: next row dp[i+1,j]=dp[i+1,j+1]=(dp[i,j]-1)/2
->>

<<-135	Candy    		32.4%	Hard	
distribute candy according to rating. 1d case.
dp: left to right and right to left. simple version for 2d matrix similar problems.
->>

<<-174	Dungeon Game    		32.9%	Hard	->>

<<-221	Maximal Square    		38.1%	Medium	
dp: check row by row, and we only need keep two rows height.
->>

<<-120	Triangle    		45.0%	Medium	
min path sum from top to bottom. dp from bottom to top is more simpler.
->>

<<-97	Interleaving String    		32.2%	Hard	
check if s3 can be interleaved from s1 and s2.
dp: s3[i+j-1]==s1[i-1] case, s3[i+j-1]==s2[j-1] case and both equal case.
->>

<<-64	Minimum Path Sum    		55.4%	Medium	
2d grid
->>
<<-63	Unique Paths II    		35.0%	Medium	
with obstacles in the grid from top left to bottom right.
->>
<<-62	Unique Paths    		55.1%	Medium	
2d grid, dp or simple math problem. with m down and n right.
->>

<<-542	01 Matrix    		40.3%	Medium	
find the distance for each element to the nearest 0.
dp or bfs.
->>

<<-562	Longest Line of Consecutive One in Matrix    		46.1%	Medium	
along horizontal, vertical, diagonal, anti-diagonal.
simple dp one pass.
->>

<<-750	Number Of Corner Rectangles    		66.6%	Medium	
given a 01 matrix, corner rect means the 4 corner is 1. return number of corner rectangles.
idea: two rows count number of columns is 11 and then n*(n-1)/2
->>

<<-764	Largest Plus Sign    		46.1%	Medium	
plus: center is 1 and up down left and right has k (k+1 order)
return the order of the largest plus sign.
- dp: find left and top, then reverse find right and down size. min of all the 4.
- optimization using 1 matrix.
->>

<<-931	Minimum Falling Path Sum    		63.1%	Medium	->>

<<-1074	Number of Submatrices That Sum to Target    		60.9%	Hard	
kadane: get the sum row by row and convert to 1d subarray sum to matrix using hashmap.
strictly it is not dp instead it is hashmap.
->>

<<-1139	Largest 1-Bordered Square    		48.1%	Medium	
similarly use dp to record the horizontal and vertical length of 1s at each position. first goes from top left to bottom right. 
then we from bottom right to top left, check each position [i,j] with a side length [1 to min(h[i,j],v[i,j]] and check its top point and left point
actually we do not need the second pass.
->>

<<-1458	Max Dot Product of Two Subsequences    		42.5%	Hard	
dp[i,j] represent the max dot product of A with length i and B with length j.
dp[i,j]=max(dp[i-1,j],dp[i,j-1],A[i-1]*B[j-1]+dp[i-1,j-1])
->>

<<-1504	Count Submatrices With All Ones    		61.5%	Medium	
dp approach: first round count the number of contiguous 1s along row
second round, goes up row by row and count the submatrix
->>

<<-1292	Maximum Side Length of a Square with Sum Less than or Equal to Threshold    		49.9%	Medium	
using dp to get all sub-matrices's sum. and then check k length from min(i,j) to 1.
->>

<<-1594	Maximum Non Negative Product in a Matrix    		31.9%	Medium	
given a grid, from top left to bottom right, only right and down movement.
return the max product among all the paths.
idea: keep the min and max product for each grid. positive using max, negative using min.
dp: top element or left element.
->>

<<-1162	As Far from Land as Possible    		44.3%	Medium	
given 01 matrix, 0 is water, 1 is land. find the largest distance from watr to land.
dp: two pass get the water cell to top left and then get bottom right.
- 2d matrix two direction update, one direction first, then try another direction
level 3
->>

<<-1277	Count Square Submatrices with All Ones    		73.0%	Medium	
dp[i,j]=0 if mat[i,j]=0
dp[i,j]=min(dp[i-1,j],dp[i,j-1],dp[i-1,j-1])+1 the max length of square.
ans+=dp[i,j]
pattern: 2d matrix.
->>

<<-1289	Minimum Falling Path Sum II    		61.8%	Hard	
2d grid.
->>

<<-1314	Matrix Block Sum    		73.6%	Medium	
given k and mat[i,j] is the sum of all rows [i-k,i+k] and columns [j-k,j+k]
- brutal force
- dp: dp[i,j]=dp[i-1,j]+dp[i,j-1]-dp[i-1,j-1]+mat[i,j]
->>

### longest common subsequence
O(mn) complexity.
need get familiar with this and know the variations equivalent to LCS.

<<-583	Delete Operation for Two Strings    		49.4%	Medium	
min delete to make two strings equal. equivalent LCS
->>

<<-1092	Shortest Common Supersequence     		52.5%	Hard	
two strings, return the shortest common supersequence.
idea: find the LCS and only incorporate one copy of it.
->>

<<-1143. longest common subsequence
two strings.
very typical dp problem. 
- lcs
- edit distance.
there are quite a few variation of this problem.
->>

<<-1216	Valid Palindrome III    		48.8%	Hard	
k palindrome: at most remove k chars and make it palindrome.
equivalent: longest common subsequence problem between s and reversed s.
->>

<<-1312	Minimum Insertion Steps to Make a String Palindrome    		58.8%	Hard	
equivalent: longest common subsequence
->>

<<-1035. Uncrossed lines
find the max number of connected lines (connecting two same number in two lines)
longest common subsequence problem
->>

### longest increasing sequence

longest increasing subsequence can have O(nlogn) optimization which sometimes critical.
idea: maintain an increasing array. if current element is the largest, add it to the back. 
if we found one in the array > current, we replace it (making it smaller so to have better chance for longer array). since the array is sorted, we can use binary search.
The array is the smallest ending element with length i+1.
for example [4,5,6,3]
len=1, [4],[5],[6],[3], so tail[0]=3
len=2, [4,5],[5,6], so tail[1]=5
len=3, [4,5,6], so tail[2]=6.

```cpp
    int lengthOfLIS(vector<int>& nums) {
        vector<int> piles;
        for(int i=0; i<nums.size(); i++) {
            auto it = lower_bound(piles.begin(), piles.end(), nums[i]);
            if (it == piles.end())
                piles.push_back(nums[i]);
            else
                *it = nums[i];
        }
        return piles.size();
    }
```	
<<-673	Number of Longest Increasing Subsequence    		38.2%	Medium	
return the number of LIS.
two dp problem: LIS and number of LIS ending with nums[i]
- if len(i)==len(j)+1, cnt[i]+=cnt[j]
then we add all len=maxlen, cnt
->>

<<-474	Ones and Zeroes    		43.3%	Medium	
given a list of binary string, and two numbers m and n. return the largest subset such that there are at most m 0s and n 1s.
dp: dp[i,j] represent the max length for i 0s and j 1s.
dp[i,j]=dp[i-k0,j-k1]+1 k0 k1 is the current string number of 0s and number of 1s. Similar to LIS.
->>

<<-300	Longest Increasing Subsequence    		43.0%	Medium	->>

<<-354	Russian Doll Envelopes    		35.8%	Hard	
LIS.
->>

<<-646	Maximum Length of Pair Chain    		52.4%	Medium	
given a list of intervals, (a,b) (c,d) can be chained if b<c.
return the length of longest chain.
longest increasing sequence: dp[i] is the max length ending ith pair.
->>

<<-960	Delete Columns to Make Sorted III    		54.0%	Hard	
a list of word, (can treat as a 2d matrix) return the min number of columns to delete making every row sorted.
LIS problem.
->>
<<-955	Delete Columns to Make Sorted II    		33.4%	Medium	
after delete columns, each row gets a string, the strings are in sorted order.
check column: if column is not sorted, it shall be deleted. if equal found, we shall proceed to next column. no need geedy or dp.
->>
<<-944	Delete Columns to Make Sorted    		70.9%	Easy	
after deletion, each column is sorted. return the min number of deletion.
->>

<<-1027	Longest Arithmetic Subsequence    		50.4%	Medium	
each element can bind with previous one with a different difference, since d varies, we shall use array of hashmap for the dp.  
LIS.
->>

<<-1048	Longest String Chain    		55.2%	Medium	
if word1 is predecessor of word2, we can add a char to word1 so that it equals to word2. 
given a list of words return the longest word chain length.
- dp: sort by length and attach to previous.
LIS.
->>

<<-1713. Min operation to make a subsequence
give target array distinct integers, min number of insertion on other array to make target is a subsequence of this array.
this is a LCS problem with O(nm) but too high for this problem.
using the fact elements in target is distinct, we use its index to get an index array in B, and then look for the LIS in O(nlogn) time.
->>

<<-1218	Longest Arithmetic Subsequence of Given Difference    		45.9%	Medium	
dp: longest increasing subsequence
using hashmap dp[i] means the length ending with i.
->>

<<-1187	Make Array Strictly Increasing    		41.5%	Hard	
given two arrays A and B, you need make A increasing by assigning A[i]=B[j].
return the min number of operations.
- sort and remove duplicates in B. so we can use one by one in order.
- now it is similar to LIS connecting two arrays.
dp[i,j] represent the min operations needed to make A[0..i] increasing using B[0...j]
we have two options: 
we can choose one from B (the smallest one > A[i])
if A[i]<prev: we can keep it
there is 1d approach but it is too hard to understand.
problem pattern: LIS, two array dp[i,j]
->>

<<-1626	Best Team With No Conflicts    		36.7%	Medium	
conflict: younger player has higher score, not allowed. same age is not conflict.
target: maximize the score with no conflict.
- greedy: sort by score descending order, choose the higher score first
- then use dp, if current age is smaller, then he can be added with no conflicts.
- equivalent: longest increasing sequence problem LIS, also linear programming.
->>

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

### bitmask dp

bitmask dp: typical usage the number of elements is very limited and 2^n can fit in a integer.

<<-526	Beautiful Arrangement    		59.3%	Medium	
a permutation of 1 to n is beautiful if every A[i] divisible by i or i divisible by A[i] (1-indexed)
return the number of beautiful arrangement. n<=15.
- dp: bitmask dp. fill from left to right.
->>

<<-1066	Campus Bikes II    		54.0%	Medium	
campus given as a 2d grid, n workers and m bikes n<=m at different grid position,
return the min distance sum so that each worker is assigned a unique bike.
- build a distance matrix for person vs bike.
- dp[i,bitmask] represent the shortest distance sum for i people and bitmask(indicate which bike is taken)
- dp[i,bitmask]=min(dp[i-1,pre]+dist(i-1,j)) if we assign bike j to person i-1.
use current status, and loop and assign the bike to i-1 th person to see if we can reduce the total.

also can use dijkstra. 
->>

<<-1542	Find Longest Awesome Substring    		36.0%	Hard	
find longest subarray which can swap to a palindrome.
we only need to know the number of even and odd. a palindrome only allows <=1 odd chars. using bitmask xor, even will get 0, odd will get 1.
- all even, i-dp[mask]
- one odd: i-dp[mask^(1<<j)]
hashmap, odd even xor, dp
->>

<<-1349	Maximum Students Taking Exam    		43.3%	Hard	
given a mxn matrix with some cells broken by 'x', empty cell '.'. One student can be seated but its left, left diagonal, right and right diagonal cannot have a student.
return max student can be seated.
bitmask dp with more complexity:
the empty cells, using previous cell state to validate current row.
dp[i,state] the max students using i rows of seats, with current state.
->>

<<-1434	Number of Ways to Wear Different Hats to Each Other    		38.9%	Hard	
given n people and 40 types of hats from 1 to 40. n<=10.
given a list of preference for each people.
return the number of ways that n people wear different hats from each other. 
- dp problem, ith person choose j, then it will affect the later subproblem.
- bitmask. but if we use hats, 2^40 would be too large. so we need convert hats to person.
dp[i,mask] ith hat with mask of people. 
->>

<<-1494	Parallel Courses II    		31.0%	Hard	
n courses from 1 to n. given a list of dependency [x,y] means x must be taken before y. You can take at most k courses per semester. 
return the min number of semesters to taken all courses.
- one choice may affect further choices, dp + directed graph.
- source nodes first, after source node is processed, remove them.
- multiple sources taken k (combination..)
- next step depends on course taken: bitmask dp.
dp[status]: min number of semesters for courses taken by status.
- once a course is taken, all pre shall be cleared already.
- dp[state|j]=min(dp[state|j],dp[state]+1)
->>

<<-1595	Minimum Cost to Connect Two Groups of Points    		41.9%	Hard	
two groups of points with size m and n, m>=n. m<=12.
connecting to i,j costs cost[i,j]
each point in one group must be connected to one or more points in other group.
return the min cost.
bitmask dp: for each point in group A, it has a bitmask to indicate nodes to connect in B.
idea: 
a. first connect A to B using the total min cost (does not consider all B nodes shall be connected).
b. After done, connect unconnected B nodes to cheapest A nodes.
solving a using bitmask dp. dp[i,mask] represent ith node in A using bitmask in B and get the min cost.
dp[i,mask]=min(cost[i,j]+dp[i+1,mask|(1<<j)])
->>

<<-1659	Maximize Grid Happiness    		29.1%	Hard	
problem: given a grid, and number of introvert and number of extrovert. introvert starts with 120 and lose 30 for each neighbor and extrovert starts with 40 and gain 20 for each neighbor. find the max happiniess.
The critical part is to realize that the current choice only depends on previous n placements. 
using dp with bitmask. top down is easier.
dp state is defined by current position, intro used, extro used, previous n cells intromask, previous n cells extromask. 5d dp.
level: 5
->>

## greedy

greedy problems are actually hard since you need to think hard to find the proper way and is able to prove its correctness.
Take a greedy step and reduce to a smaller problem. Generally need some insights.
Most time, greedy approach is incorrect. So use it by caution.
greedy can often be approached using recursion.
greedy focus more on idea instead of algorithm or data structure.

<<-31	Next Permutation    		33.0%	Medium	
next larger : find from right the first A[i]<A[i+1] and swap it and then reverse the right part.
->>

<<-556	Next Greater Element III    		31.9%	Medium	
find the smallest integer > n with the same digits as n.
- digits in reverse sorted, then we do not have larger one
- otherwise, we find the next_permutation. (we also have prev_permutation)
next_permutation: 
- from right to left find the first one a[i]<a[i+1]
- swap with A[i] with right smallest digit >A[i]
- reverse the right to make it sorted. 124654->125644->125446
->>

<<-484	Find Permutation    		63.9%	Medium	
given "DI" string, return the lexi smallest permutation of 1 to n.
for example "DI" this needs a permutation of 1,2,3. [2,1,3] is the smallest one.
idea: starting from the smallest permutation of 1 to n. then we reverse all D.
for example 'DI', we start from [1,2,3], then we reverse 1,2 ->[2,1,3]
->>

<<-161	One Edit Distance    		32.5%	Medium	
insert/delete/replace exactly one char to make s==t.
insert/delete: length +-1 so swap to convert insert to delete
- find the first mismatch: if length the same compare others, if not remove the char.
->>

<<-179	Largest Number    		30.1%	Medium	
rearrange a list of number to get the max number
greedy: which one goes first using string compare
->>

<<-321	Create Maximum Number    		27.3%	Hard	
given two array of digits, the order shall be preserved. get the max number with length k.
greedy: 
- k=m+n, merge
- k<m+n, try 0 to k numbers from A, try k to 0 from B. then merge.
->>

<<-670	Maximum Swap    		44.5%	Medium	
swap two digits at most once to get the max number.
greedy: find the right max to swap.
->>

<<-681	Next Closest Time    		45.5%	Medium	
given a time hh:mm, you may reuse or not use some digits. return the next closest time. (next greater)
greedy: from right to left, replace current with the next larger digit available. if not possible, replace with the min (expecting we increase left digit) for example: 19:34, we change 4, next bigger is 9, 19:39
23:59->23:52->23:22->22:22
->>

<<-738	Monotone Increasing Digits    		45.1%	Medium	
give a number N, find the largest number with monotone increasing digits. (<=)
idea: 12321->12299 from right to left find the last position increase stops.
for this one, ends at 3. we reduce it by 1, and right replaced by '9'
->>

<<-870	Advantage Shuffle    		46.1%	Medium	
A[i]>B[i] to maximize number of advantage (permutation of A)
greedy: sort A and use the min greater element for A (since it asks for ordering B cannot sort).
->>

<<-397	Integer Replacement    		33.2%	Medium	
if even /2, if odd +1 or -1.
min number of move to reduce n to 1.
- only +1 when it becomes 4 multiples (but 3 is different 3+1->4->2->1, 3-1->2->1
->>

<<-861	Score After Flipping Matrix    		73.1%	Medium	
choose any row or column and toggle the row or column. Each row is interpret as a binary number. return the largest sum of all row numbers.
greedy: toggle each row MSB to 1. then choose columnwise, to make majority to 1.
->>

<<-406	Queue Reconstruction by Height    		67.7%	Medium	
given a list of people with height and number of people in front of him >=his height.
greedy: sort decreasing order. we arrange highest first. and then insert shorter ones.
after that shorter one we know its exact position.
->>

<<-575	Distribute Candies    		61.6%	Easy	
given n candies candy[i] is the candy type. Only eat n/2 candies, return the max number of types she can eat. (n is even)
min(n/2,type.size())
->>

<<-621	Task Scheduler    		51.1%	Medium	
different tasks A-Z. and can be arranged in any order. 
n is the cooldown period. the time between two same tasks shall >=n.
return the least number of time to finish the task.
arrange the most frequent task in n spaced interval, then fill less frequent task. 
- (maxfreq-1)*(n-1) 
- number of maxfreq added.
- if number of different task cannot fit in the n slots, tasks.size()
->>

<<-667	Beautiful Arrangement II    		54.7%	Medium	
given 1 to n, arrange so that adjacent absolute difference has exactly k distinct numbers.
arrange like this 1,k,2,k-1..... l=1, r=k+1, until they meet.
two pointer.
->>

<<-765	Couples Holding Hands    		55.0%	Hard	
N couples sit on 2N chairs. return the min swaps so that every couple sit side by side. couple (2i,2i+1)
greedy:
- divide by 2 and couple has the same id.
- swap can eliminate one or two mismatch.
union-find.
->>

<<-881	Boats to Save People    		47.4%	Medium	
given array of people weight, boat has limit of max weight and most 2 people.
return the min number of boats
greedy: two pointer carray the max + min.
->>

<<-908	Smallest Range I    		65.9%	Easy	
You can add any number in range [-K,K]
greedy: if max-min>2K, otherwise get 0.
->>

<<-910	Smallest Range II    		27.0%	Medium	
add K or -K to each element, return the smallest range obtained.
greedy: sort first, left shall add k, and right shall -k, the problem is where is the separation position.
suppose add k at ith element, A[i]+K may become a new max, A[i+1]-k and it may becomes a new min (right bigger elements cannot be min). still need to compare with A[n-1]-k and A[0]+k.
->>

<<-1041	Robot Bounded In Circle    		54.4%	Medium	
G: go forward by 1, L: turn left, R: turn right.
greedy: repeat the instructions for 4 times and see if repeat. (why 4 times?)
->>

<<-502	IPO    		40.9%	Hard	
at most finish k distinct projects. given a list of projects with profit[i] and min capital C[i] needed. Initially we have capital W. Once finished the project the profit is added to the capital.
return the final max capital.
- combine capital + profit (discard zero profit projects)
- sort by capital
- add all projects with <current capital into heap
- choose the one with max profit.
->>

<<-555	Split Concatenated Strings    		42.6%	Medium	
given a list of strings, and concat them together into a loop. Each string you can choose to reverse or not. find the largest string after cut the loop.
you need to connect using the given order.
idea: connect them in ascending (each word) order, the other way is connected them in descending order.
loop and use ith string for the cut position, and get the global max.
->>

<<-630	Course Schedule III    		33.6%	Hard	
given n courses, each course has duration days and closing day. You will start on the 1st day, return max number of course taken.
greedy: You can start a course <=closing-duration. take shorter courses will have more chances to take more courses, also we need take courses with smallest closing day first.
- sort by closing day
- tracking the start time.
- add the duration into heap
- if s+duration>closing, try to pop the longest course.
key idea: to add current course, pop previous longest course (using priority_queue)
can also use dp. similar to longest increasing subsequence. (interval)
knapsack:dp[i,time] max number of courses including ith course 
->>

<<-659	Split Array into Consecutive Subsequences    		44.0%	Medium	
given a sorted array, see if you can split into 1 or more subsequence with consecutive integers with length>=3
- the highest frequency number determines number of sequence we shall divide  -will not work.
- since it is sorted we can loop over the array, key idea: the smallest must be the start.
there is something not solved yet if we need continue to extend our sequence.
cnt: the frequency map
tail[i]: the number of sequence >=3 ending with i.
```cpp
bool isPossible(vector<int>& nums) {
        unordered_map<int,int> cnt, tails;
        for(int &i : nums) cnt[i]++;
        for(int &i : nums){
            if(!cnt[i]) continue;
            cnt[i]--;
            if(tails[i-1] > 0){ //if we see previous i-1 we connect it.
                tails[i-1]--;
                tails[i]++;
            }
            else if(cnt[i+1] && cnt[i+2]){ //no previous i-1, start a new group
                cnt[i+1]--;
                cnt[i+2]--;
                tails[i+2]++;
            }
            else return false; //cannot add to previous or start a new group
        }
        return true;
    }
```
for example [1,2,3,3,4,5]: frequency [1:1,2:1,3:2,4:1,5:1]
we see 1, and we need 2 and 3, 1:0,2:0,3:1,4:1,5:1 tail[3]=1
we see 2: skip since it is used.
we see 3: skipped since it is used.
we see 3: there is no sequence ending with 2, so start a new one [3,4,5]

greedy+hashmap, this is pretty trick.
->>

<<-846	Hand of Straights    		55.0%	Medium	
reorder the cards in groups, each group with consecutive number and with size W.
greedy: from the smallest. same as 1296.
->>

<<-1296	Divide Array in Sets of K Consecutive Numbers    		54.9%	Medium	
greedy: start from the smallest and then change the hashmap. The smallest has to be the start.
->>

<<-826	Most Profit Assigning Work    		38.8%	Medium	
given n jobs with difficulty[i] and profit[i]. given a list of workers with worker[i] is the worker's max difficulty level. 
each worker at most has one job. but one job can be completed multiple times.
return the max profit.
greedy approach: assign the most profitable task to people who can do it. 
- same difficulty task only need keep the max profit one. possible to have less difficulty but more profitable task.
- sort the worker
- we can get leftmax and do binary search (pair upper_bound will not only compare the first but also the second, so may not get what you expect so use two different array)
->>

<<-857	Minimum Cost to Hire K Workers    		50.0%	Hard	
N worker given with quality[i] and min wage requirement wage[i].
we want to hire k workers for the min amount of money.
restriction:
- each worker is paid in the ratio of his quality compared with other workers.
- each worker must meet min wage requirement.
greedy: 
- Wi/Qi=Wj/Qj=const with Wi>wage[i] and Wj>=wage[j]. total money=sum(Wi)=A*sum(Q). W>=wage[i] then Wi/Qi>=wage[i]/Qi so the max wage[i]/Qi shall be the ratio.
- to min the money, we need to min ratio A, and sum(Q).
- if we sort by wage[i]/Qi, then the k group is the last ratio, and Q is put in a heap.
- once we add a larger ratio, we want to kick out the one with max quality.
often seen optimization problem which one to choose first with adding new and kicking out old.
->>

<<-871	Minimum Number of Refueling Stops    		31.8%	Hard	
given a list of station with position and gas amount, the car starts at 0 with initial fuel. return the min number of refuel to reach target.
can be approached using greedy or dp.
greedy: put all reachable gas into pq, and choose the max. when there is no gas to add, return -1.
dp: convert to equivalent problem dp[t] represent longest distance using t refuels. 
this kind of greedy is often seen using priority_queue. sort it, add it, pop the worst one.
->>

<<-891	Sum of Subsequence Widths    		32.7%	Hard	
sequence width = max-min. return the sum of width of all subsequences.
- a fixed max and min pair can have a lot of sequences (with other elements each has 2 options)
- single element does not contribute max-min=0;
- sort it will solve our dilema since we do not care what the sequence will be.
- A[i] as the max, all left choose or not choose 2^left. A[i] as min all right choose or not chosen, 2^right.
->>

<<-936	Stamping The Sequence    		46.8%	Hard	
given a stamp string and a target string. The next stamp will replace the old one at the same place.
do it reversely, remove the stamp string and replace with ?. find the order of printing.
greedy: find one match and replace
->>

<<-945	Minimum Increment to Make Array Unique    		46.5%	Medium	
each time you can choose to increase one element by 1. 
greedy: sort and then increase the next one. (increase previous will cost more)
->>

<<-954	Array of Doubled Pairs    		35.4%	Medium	
length is even, check if we can reorder A[2i+1]=2*A[i].
use hashmap to get each number's occurrence, sort by the abs value and then start from the smallest and remove the 2x. greedy approach.
->>

<<-984	String Without AAA or BBB    		38.2%	Medium	
give a 'a' and b 'b', return any string without 'aaa' or 'bbb'
greedy: recursive, if a==b choose ab, a==0 choose b, b==0 choose 'a'
a>b choose "aab" else choose "abb" (cannot use "bba")
->>

<<-991	Broken Calculator    		46.3%	Medium	
calculator can only do *2 or -1.
return the min number of operations from x to y.
equivalent from y to x by /2 or +1. even /2, odd +1
->>

<<-995	Minimum Number of K Consecutive Bit Flips    		49.2%	Hard	
a k-bit flip operation: choose window k, flip every bit. 
return the min number of operations so that there is no 0s.
- approach: from left to right, find the first 0 and do a flip.
greedy.
->>

<<-1005	Maximize Sum Of Array After K Negations    		52.2%	Easy	
negate the array element exactly k times, return the max sum.
priority_queue: negate the smallest one first. When all becomes positive, we repeatedly negate the smallest. (odd/even of k decide the final value)
->>

<<-1024	Video Stitching    		49.3%	Medium	
a list of video each with start and end. find the min number of clip that cover [0,T].
greedy on interval: sort with start, for those satifying the start requirement, choose the one with largest end.
subject: interval, greedy.
->>

<<-1029	Two City Scheduling    		57.1%	Medium	
given 2n people flying to A or B. half to A and half to B, cost for each person is given in array.
return the min cost.
[a0,b0],[a1,b1], if we choose a0,b1, then a0+b1>b0+a1, a0-a1>b0-b1 sort by the difference.
greedy.
->>

<<-1058	Minimize Rounding Error to Meet Target    		42.7%	Medium	
given a double array, round up or down to get a sum of target.
if yes, return the smallest rounding error. rounding error=sum(|round(p)-p|.
- if all round up ->max, if all round down we get min. 
- greedy approach: we need use target-min to ceil. which one to choose? the one closet to the ceil. sort according to round to ceil error.
- ceil (note the integer cannot count into ceil for example 8.0)
proof: we need use m=(target-min) ceil so the error would be mCeil+(n-m)Floor=m(ceil-Floor)+nFloor
nFloor is constant and we can ignore, so min m(ceil-Floor) ie we need take the smallest ceil-floor.
this is very similar to Amazon OA five star. 
- priority_queue with first choose.
->>

<<-1111	Maximum Nesting Depth of Two Valid Parentheses Strings    		72.3%	Medium	
very confusing question. count, ( +1 and ) -1. odd number group, even number group.
greedy.
->>

<<-1121	Divide Array Into Increasing Sequences    		57.6%	Hard	
given a sorted array, and k. find out if this array can be divided one or more dijoint increasing subsequence of length >=k.
- get the histogram, the highest frequency indicates number of parts we shall divide.
- greedy: decrease the all the number with maxfreq by 1 (if less than k, we need keep going until we have k. (similar to 659	Split Array into Consecutive Subsequences)
->>

<<-1147	Longest Chunked Palindrome Decomposition    		59.1%	Hard	
problem: decompose into parts and the parts form a palindrome pattern.
greedy: find the shortest prefix = the suffix and do recursion.
try to prove the approach is correct.
->>

<<-1153	String Transforms Into Another String    		35.9%	Hard	
idea: one by one we map the char in str1 to str2 char's.
the mapping cannot change.
you need one unused mapping to transfer the char to non-exist char in str2.
kind of greedy, using hashmap.
->>

<<-1183	Maximum Number of Ones    		56.1%	Hard	
a mxn 01 matrix, given sidelength we shall let any square of side length has at most Max ones.
return the max possible number of 1s.
greedy: we shall put the maxN 1s in the square top left as much as possible. then tile the square across horizontal and vertical. There might be extra on the bottom and right. 
for each position in the square we calculate the occurences and arrange from high to low.
->>

<<-1199	Minimum Time to Build Blocks    		38.0%	Hard	
given a list of block build time needed and split cost time. If two workers split at the same time, they split in parallel and the cost is split. One worker can only work on one block.
initial one worker. Return the min time to build the blocks.
for example blocktime=[1,2], splitcost=5.
- this is a tree structure. if the worker decides to do the work, it becomes a leaf node. if it splits, then it becomes the inner node.
- the inner edge is the split cost and same level can do in parallel and save cost.
greedy algorithm (huffman algorithm): 
build a min heap, take the 2 smallest and add the split cost to the bigger one.
let the worker with smallest task to split (the time is the cost+the larger one)
for example [1,2,3] cost 5
   1
  / \
 5   \
/ \   \
1  2   3
we first split 1->2 at cost 5, the worker 1 split again with cost 5, 2+5+5=12
this is similar to 1167 Min cost to connect sticks
->>

<<-1167	Minimum Cost to Connect Sticks    		63.8%	Medium	
suppose you have (a,b,c), you connect a,b first at the cost of (a+b), then combine c: 2*(a+b)+c, smaller one are repeatedly counted. so use smaller ones first. minheap.
amazon OA: amazon fulfillment builder.
->>

<<-1217	Minimum Cost to Move Chips to The Same Position    		71.2%	Easy	
greedy: odd position move to odd position and even position moves to even cost 0. other cost 1.
so just merge all odd and all even then decide which to move.
->>

<<-1234	Replace the Substring for Balanced String    		34.1%	Medium	
string consists of QWER, string is balanced if each char appears n/4.
return the min length substr to replace to equal length any string to make it balanced.
count each char's count and minus n/4, only keep >0. find the min window with the same hashmap (>= corresponding count).
greedy, hashmap, sliding window
->>
   
<<-1247	Minimum Swaps to Make Strings Equal    		62.2%	Medium	
string s1 and s2 consist of 'x' and 'y' only. You can swap s1[i] vs s2[j] so that they are equal.
two cases: xx vs yy, xy vs yx.
xx vs yy: first x swap vs second y->yx one step
xy vs yx: first convert to xx and yy, 2 steps.
we shall consider columnwise, same char we don't need worry.
x      y
y and  x
one pair of xy vs yx need 1 operations. 
nxy, nyx -> min(nxy,nyx) will be paired using 1 operations
remaining all xy or yx pattern also need to be paired. 
->>

<<-1253	Reconstruct a 2-Row Binary Matrix    		41.2%	Medium	
given a 01 matrix 2xn and the row sum and col sum, reconstruct the array.
row sum gives number of 1s in each row.
col sum gives number of 1s in each col
greedy: fill all colsum=2 first, and reduce the row sum accordingly. colsum=0 both 0.
colsum=1, we fill first row 0 and then row 1.
->>

<<-1266	Minimum Time Visiting All Points    		79.2%	Easy	
from point 1 to point 2 you can go x, y or along diagonal. return the min time to visit all points by order.
sum(min(dx,dy))
->>

<<-1282	Group the People Given the Group Size They Belong To    		84.2%	Medium	
given n people from 0 to n-1, and given a groupSize array groupSize[i] means ith person belongs to a group of groupSize[i].
return the groups with all its members.
greedy: combine size and id, sort using size then fill one group by one group. fill smaller group first since large group has a lot more choices.
->>


<<-1297	Maximum Number of Occurrences of a Substring    		48.9%	Medium	
substr: unique letters in substr <=maxLetters, substr length [minSize,maxSize]
key observation: if a larger size substr satisfy the condition, apparently smaller size of this substr will also satisfy the condition, and shorter substr will have larger number of occurrence. so only check minSize is fine. using hashmap+ sliding window for a fixed size substr.
->>

<<-1328	Break a Palindrome    		45.1%	Medium	
replace one char so that it is not a palindrome, and it becomes the smallest string.
greedy: find the first one not 'a' replace it with 'a' (stop before the mid). if we did not find, replace the last one with 'b'
->>

<<-1374	Generate a String With Characters That Have Odd Counts    		76.1%	Easy	
each char is odd.
greedy: just use two types of char. for even use n-1，1. for odd just use one char.
->>

<<-1403	Minimum Subsequence in Non-Increasing Order    		71.3%	Easy	
a subsequence sum > the remaining subsequence with the following restrictions:
- length is minimized.
- sum is maximized.
greedy: sort and take the largest until we get the sum > the left.
->>

<<-1702. Max binary string after change
You can change 00 to 10 and 10 to 01, return the max string
greedy: 011110 you can always change to 101111, ie. you can reduce one 0 and shift the 0 one step right. Thus we make the binary string larger.
repeat the process until one zero is left.
when input is large and O(n) is needed, generally there is a greedy approach.
->>

<<-1432	Max Difference You Can Get From Changing an Integer    		42.9%	Medium	
pick a digit x and digit y, replace all x in the number to y.
You can apply the operation two times, return the max difference between the two operations.
first time to get the min number and 2nd time we get the max number.
to get the max: replace the first seen <9 digit to 9
to get the min: replace the first seen >1 digit to 0
->>

<<-1433	Check If a String Can Break Another String    		66.9%	Medium	
string a break b, if a[i]>=b[i]. you can permutate a.
greedy: just check histogram and from larger to smaller, add the extra larger one to smaller one by one. O(n)
or just sort them and compare one by one. O(nlogn)
->>

<<-1481	Least Number of Unique Integers after K Removals    		55.2%	Medium	
greedy: using hashmap to record the histogram. and then sort according to the count, remove those smallest first.
->>

<<-1503	Last Moment Before All Ants Fall Out of a Plank    		52.6%	Medium	
ants on different positions, and some goes left and some goes right. Once two ants meet, they change direction. Get the last moment all fell out of the array
greedy: it is equivalent that they do not change direction. So it is simple to just find the rightmost left direction and leftmost right direction time.
this is same idea in airplane seat. All the inside details are not important at all.
->>

<<-1227	Airplane Seat Assignment Probability    		61.8%	Medium	
similar to the last time when all ants drop the plankton. treat all person the same.
greedy. All the inside details are not important at all.
->>

<<-1509	Minimum Difference Between Largest and Smallest Value in Three Moves    		51.6%	Medium	
choose most 3 numbers and change to any value. return the smallest difference.
greedy 1. similar to 1423, max point from cards. change the left or right side
greedy 2: using sliding window with window size=n-3 and find the min Lc1561.
->>

<<-1561	Maximum Number of Coins You Can Get    		78.9%	Medium	
given 3n piles of coins, three players.
You choose any 3 piles, and A takes the max, then you choose, B takes the last.
return the max coins you can get.
greedy: sort and start from the max, choose the max 2 and the min.
similar to the problem in sliding window 
->>

<<-1526	Minimum Number of Increments on Subarrays to Form a Target Array    		59.3%	Hard	
intial all zero, you can choose any subarray and increase each element in it by one. 
return the min steps to reach target array.
A[0]>A[1] for example [3,2]->[2,2]->[1,1]->[0,0]
A[0]<A[1] for example [3,4]->[2,3]->[1,2]->[0,1]->[0,0]
bring A[0] down and its right smaller can be reused.
its right bigger, we need add the difference.
greedy or segment tree.
->>

<<-1529	Bulb Switcher IV    		70.7%	Medium	
flip[i] will flip all lights from i to end. To reach a given target status, return min number of flips.
Initial all off.
from left to right, if A[i] is 0, then we know it is even, else it is odd. （偶消奇不消）
greedy: from left to right, if A[i]=0, and the prefix sum is odd, we need add one here. If A[i]=1 and prefix sum is even we need add one.
->>

<<-1536	Minimum Swaps to Arrange a Binary Grid    		42.8%	Medium	
given a nxn matrix, swap rows to make the cells above main diagonal cells to be all 0. return the min swaps (only neighboring rows can swap), or -1 if not possible.
count trailing zeros for each row, and reduce to 1d problem, bubble sort.
greedy: move the FIRST row satisfying to current top row.
->>

<<-1538	Guess the Majority in a Hidden Array    		61.3%	Medium	
given 01 array without direct access.
a=query(a,b,c,d) 4 different indices, and:
a=4: all the elements are the same
a=2: three elements are the same
a=0: two elements are the same.
return any index of the most frequency value in the array.  tie, return -1.
at most 2*len calls can be made.
greedy: cnt0,cnt1 or cnt1,cnt0, then we conclude which is the majority.
[0,0,1,0,1,1,1,1]
first query(0,1,2,3)=2
second query(4,5,6,7)=4
The idea is we do not need to know 0 or 1, but classified into two cnt0 and cnt1. 
q(0,1,2,3)=3
q(0,1,2,4)=0 ->remove 3 and add 4 causing cnt0-- and cnt1++, which means bit[3]!=bit[4] using this we can classified 3 to n-1 into two baskets.
to determine 0,1,2,3 we query 0,2,3,4,[0,1,3,4],[1,2,3,4]
->>

<<-1541	Minimum Insertions to Balance a Parentheses String    		41.9%	Medium	
one left parenthesis with two right parenthesis calls balance.
just a variation of normal balance using counting.
- left +2, right -1
- prefix >=0
- see a left, the prefix shall be even.
greedy.
->>

<<-1540	Can Convert String in K Moves    		29.6%	Medium	
convert s to t using [1,k] moves, the ith move:
- choose any index not used before, shift the char by i times (circular)
check if it is possible.
greedy: compare when char is not the same, we need shift to t[i]. if used before we need add 26 to it and check if it is >k.
->>

<<-908. Smallest range I.
add x in [-k,k] to each element, find the smallest max-min.
the max difference is max-min-2*k, when less<0, difference is 0
->>

<<-910. Smallest Range II
each element shall add k or -k.
greedy: left+k and right-k, we need loop over all i. One pass.
math, greedy
->>

<<-1551	Minimum Operations to Make Array Equal    		77.8%	Medium	
given array with a[i]=2*i+1. each time you can choose a pair of index and add 1 to one and -1 to the other. return the min operations to make the array all the same
greedy: sorted array, and min operation to make the array go to median.
->>

<<-1558	Minimum Numbers of Function Calls to Make Target Array    		62.4%	Medium	
from all 0 to target array by:
- add 1 to an element
- *2 for all element
equivalent to: minus 1 if odd, /2 if even from target array to all zero.
->>

<<-1576	Replace All ?'s to Avoid Consecutive Repeating Characters    		48.0%	Easy	
greedy: try each ? from 'a' to 'z' so that it is not the same as left and right.
->>

<<-1578	Minimum Deletion Cost to Avoid Repeating Letters    		60.1%	Medium	
given a string, and array of cost for deleting s[i]. return the min cost so that no same character is together.
idea: when there is a group of same chars, the min removing cost is sum(cost)-max(cost), greedy.
->>

<<-1579	Remove Max Number of Edges to Keep Graph Fully Traversable    		45.5%	Hard	
undirected graph: type 1, A can go, type 2: B can go, type 3: both A and B can go.
given a list of edges. find the max edges you can remove so that both A and B can traverse the graph.
greedy: similar to MST, choose type 3 first. Then based on 3, add unconnected 1. based on 3 add type 2 using union find.
->>

<<-1564	Put Boxes Into the Warehouse I    		66.2%	Medium	
push box from left to right only. You can reorder the box.
return the max number of boxes can be pushed in.
greedy: 
- from left to right, get the min height minh[i] minh is monotonically decreasing.
- sort box.
- from right to left, push the smallest box to the right (fill right first). 
->>

<<-1580	Put Boxes Into the Warehouse II    		63.0%	Medium
given a list of boxes with heights, and a list of warehouses with height from left to right.
- you can push from left or right (both sides).
- you can change box order.
return max number of boxes you can push into the warehouse.
greedy: higher box is hard to arrange, so try higher first.
using two pointer, if highest box can get through left, all remaining can pass, advance left.
if highest box can get through right, all remaining can pass, right--
Note this is not the actual pushing order, we can always find a way to push them in if we can find a position for the box.
similar to trap raining water. 

```cpp
    int maxBoxesInWarehouse(vector<int>& boxes, vector<int>& warehouse) {
        sort(boxes.begin(), boxes.end());
        int cnt = 0, left = 0, right = warehouse.size() - 1;
        for(int i = boxes.size() - 1; i >= 0 && left <= right; --i){
            if(boxes[i] <= warehouse[left]){
                ++left; ++cnt;
            } else if(boxes[i] <= warehouse[right]){
                --right; ++cnt;
            }
        }
        return cnt;
    }
```
->>

<<-1585	Check If String Is Transformable With Substring Sort Operations    		48.0%	Hard	
tranform s to t. given operation: choose a substring and sort in-place.
- t<=s since sort will make it smaller.
- sort will make smaller goes to left and bigger goes to right.
- shall have the same histogram.
- we need to check if we can move the digit to desired position. If we hit smaller one, we shall stop there.
for example: 0231->0213->0123
greedy approach: start from smallest digit. for example '0' its index can only shift to left. compare its indices in s and t. indx1[i]>=indx2[i] must be satisfied.
then we erase all 0's from both s and t. 
(inplace erasing the digit: move the digit to the front using two pointer)
```
	bool isTransformable(string s,string t){
		if(s.size()!=t.size()) return 0;
		
		for(char d='0';d<='9';d++){
			vector<int> idx0,idx1;
			string ss,tt;
			for(int i=0;i<s.size();i++){
				if(s[i]==d) idx0.push_back(i);else ss+=s[i];
				if(t[i]==d) idx1.push_back(i);else tt+=t[i];
			}
			if(idx0.size()!=idx1.size()) return 0;
			for(int i=0;i<idx0.size();i++){
				if(idx0[i]<idx1[i]) return 0;
			}
			s=ss,t=tt;
		}
		return 1;
	}
```
starting from '9' does not work:
for example 891 vs 198. 9 satisfy the condition, after removing 9, 8 satisfy the condition, but actually not since 8 cannot go beyond 9.
but start from 1: 1 goes to left, ok. then check 89 vs 98, 8 goes to right, not ok.
->>

<<-1589	Maximum Sum Obtained of Any Permutation    		34.5%	Medium	
an array of integers. a query [l,r] will return the sum A[l] to A[r].
the total sum is the sum of all requests.
return max total sum among all permutation of the array.
idea: event begin and start using hashmap and greedy assign the most frequent one with the largest value.
greedy, hashmap, intevals
->>

<<-1591	Strange Printer II    		55.1%	Hard	
each time the printer will print a rect with same color.
each color can only be used once.
given a matrix of colors, check if we can print it.
idea: hashmap for each color we find its rect. loop and find if the rect contains only the color and 0. Once it is valid, we change all to 0.
greedy.
->>

<<-1599	Maximum Profit of Operating a Centennial Wheel    		43.2%	Medium	
wheel has 4 gondolas, each gondolas can hold 4 people.
wheel rotate clockwise. rotate until next gondonla picking up customer with fixed cost. Boarding ticket price is given.
given an array, A[i] is the number of people coming at the ith rotation.
if you decide stopping picking up customers, all subsequent rotation is free (no cost).
return min number of rotation to max the profit.
- if more than 4 at ith rotation, push to remaining people to next.
- record the profit and number of rotation.
greedy, pickup and reduce problem size.
->>

<<-1605	Find Valid Matrix Given Row and Column Sums    		76.9%	Medium	
given each row sum and each col's sum. return a valid matrix. (all elements >=0).
greedy: A[i,j] is bound by min(rsum[i],csum[j]). 
greedy choose the min as A[i,j], then update the rsum and csum.
->>

<<-1647. Minimum Deletions to Make Character Frequencies Unique
get the occurrence for each char and count each number's frequency in map.
then choose the largest and delete a char to make it single. pushing from higher frequency to lower frequency.
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
problem: find the min lexi subsequence with length = k.
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

- keep invariant condition when shrinking the range.
- left biased and right biased.
- convert problem to binary search if brutal force checking all range works.

<<-4	Median of Two Sorted Arrays    		30.4%	Hard	->>
<<-33	Search in Rotated Sorted Array    		35.3%	Medium	
find the rotated index and then do binary search.
->>
<<-81	Search in Rotated Sorted Array II    		33.3%	Medium	
with duplicates. revised binary search if ==.
->>
<<-154	Find Minimum in Rotated Sorted Array II    		41.8%	Hard	->>
<<-153	Find Minimum in Rotated Sorted Array    		45.6%	Medium	->>

<<-162	Find Peak Element    		43.6%	Medium	
peak greater than its left and right neighbor.
- O(N)
- binary search: compare with right, if larger goes to left, else goes to right.
->>

<<-240	Search a 2D Matrix II    		43.6%	Medium	
rows and columns are sorted. search for target.
from bottom left, treat it like a tree. O(M+N)
->>

<<-74	Search a 2D Matrix    		37.1%	Medium	
sorted actually in 1d, binary search.
->>

<<-278	First Bad Version    		36.7%	Easy	
binary search.
->>

<<-275	H-Index II    		36.1%	Medium	->>
<<-274	H-Index    		36.3%	Medium	->>

<<-410	Split Array Largest Sum    		45.7%	Hard	
split array into m parts (subarray), return the min the largest sum among the m parts.
given max sum, check number of parts we can get.
binary search.
->>

<<-540	Single Element in a Sorted Array    		57.9%	Medium	
each element except one appears exactly twice. find the single element.
- O(N) xor
- log(N): binary search: if mid is single, return.
if mid is not single, and check the index and determine if it is on left or right
->>

<<-635	Design Log Storage System    		59.0%	Medium	
add log with time stamps
query start,end with granularity.
hashmap, binary search
->>

<<-658	Find K Closest Elements    		41.4%	Medium	
sorted array find the k closest to x elements. if tie choose the smaller one.
sort using abs(a-x).
- binary search: find the leftmost element x-A[mid]>A[mid+k]-x.
->>

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

<<-642 Maximum Average Subarray I
fixed window size k. trivial
->>

<<-668	Kth Smallest Number in Multiplication Table    		47.3%	Hard	
binary search counting the number < target. counting shall be O(n).
->>

<<-683	K Empty Slots    		35.8%	Hard	
n bulbs from 1 to n. bulb[i] means on ith day we will turn on bulb at bulb[i].
return he min days such that there exist two turned on bulbs that have exacly k bulbs off between them.
- brutal force: loop from day 1 to day n and record the index TLE.
- insert will be able to return the insert position and we do not need to loop from begin. (check its previous/following)
->>

<<-702	Search in a Sorted Array of Unknown Size    		68.1%	Medium	
you can only access the array using ArrayReader. find the index for the target.
binary search just assume right=INT_MAX.
->>

<<-719	Find K-th Smallest Pair Distance    		32.2%	Hard	
given an array, find the kth smallest pair difference (absolute difference)
sort and do binary search: min is 0, max is the last one - first one.
then give a mid, count number of pairs < mid. (count can use two pointer to reach O(N))
->>

<<-774	Minimize Max Distance to Gas Station    		47.7%	Hard	
given n gas stations in position[i]. add k more stations so that the max distance between adjacent stations is minimized.
- final arrangement the gas station shall spread in between. 
- convert to binary search: given max distance allowed, number of stations to add.
->>

<<-778	Swim in Rising Water    		54.0%	Hard	
convert to binary search equivalent problem.
->>

<<-875	Koko Eating Bananas    		53.0%	Medium	
given N piles of banana, You have H hour time, each hour you take 1 pile and eat at most K bananas.
return the min K so that he can eat all.
binary search conversion.
->>

<<-911	Online Election    		51.0%	Medium	
ith vote gives the time and person of voting. query: giving the time, return the winner.
using a map get the winner at ith time and then use binary search.
get the winner using a map or heap to store the count for each person.
->>

<<-1011	Capacity To Ship Packages Within D Days    		59.3%	Medium	->>

<<-1044	Longest Duplicate Substring    		31.8%	Hard	
give string, duplicated substring (2 or more times). The occurrences may overlap. return the longest substring.
binary search with rolling hash. use binary power to speed the calculation. be sure to check collision
->>

<<-1060	Missing Element in Sorted Array    		54.5%	Medium	
find the kth missing number start from the leftmost
binary search!!
->>

<<-1064	Fixed Point    		65.6%	Easy	
sorted array with distinct numbers, find A[i]=i.
binary search or brutal force.
->>

<<-1095	Find in Mountain Array    		35.7%	Hard	
- find the peak first and do left and right binary search
- find upward and downward side.
->>

<<-1102	Path With Maximum Minimum Value    		49.9%	Medium	
from top left to bottom right, a path is the min value in the path. 
binary search
->>

<<-1150	Check If a Number Is Majority Element in a Sorted Array    		58.3%	Easy	
using equal_range.
->>

<<-1157	Online Majority Element In Subarray    		39.3%	Hard	
given [L,R,threshold] return the majority element >=threahold or -1 if none.
store each element's index into list.
and then binary search, using random to speed up. (rand will get into majority more)
->>

<<-1182	Shortest Distance to Target Color    		53.2%	Medium	
given a list of colors 1,2,3. And query [index,c2] and return the distance to the color.
idea: keep each color's index list. and then binary search to color index. You need also check the previous index.
or we can calculate each index to all 3 color's distance.
->>

<<-1231	Divide Chocolate    		53.1%	Hard	
given an array of sweetness of a bar. You need divide into k+1 parts using k cuts. You will take the min total sweetness part. return the max sweetness you can get.
get the max of all the min.
binary search: given the min sweetness and to see how many parts we can get.
->>

<<-1283	Find the Smallest Divisor Given a Threshold    		48.9%	Medium	
min divisor, sum(A/divisor)<=threshold.
binary search.
->>

<<-1300	Sum of Mutated Array Closest to Target    		43.5%	Medium	
given an array and find the min value so that we change all value>it to value. The sum is closest to target.
binary search: find the min value and then check if left or left-1 will get less distance.
->>

<<-1351	Count Negative Numbers in a Sorted Matrix    		76.0%	Easy	
brutal force or binary search.
->>

<<-1482	Minimum Number of Days to Make m Bouquets    		48.8%	Medium	
n flowers and given bloomday, to make one bouquests, you need k adjacent flowers. we need make m bounquest, find the earliest day
-binary search.
->>

<<-1533	Find the Index of the Large Integer    		55.1%	Medium	
array has only one number larger than the rest, the other are equal, no direct access to the array
compare(l,r,x,y) will compare the sum for [l,r] and [x,y]. find the index.
binary search: make sure the two range has equal length. odd length, even length
->>

<<-1539	Kth Missing Positive Number    		53.4%	Easy	
positive array in strictly increasing order. find the kth missing integer.
binary search: using arr[i]-i-1 compare with k.
->>

<<-1552	Magnetic Force Between Two Balls    		48.2%	Medium	
n baskets at different positions on a line. magnetic force is |p0-p1|. given m balls, return the max of the min magnetic force.
binary search: given a d, and see if we can place m balls.
->>

<<-1574	Shortest Subarray to be Removed to Make Array Sorted    		31.9%	Medium	
binary search: check if we can remove a subarray of length k and make the array sorted.
using sliding window, lmax<=rmin
>>

<<-1618	Maximum Font to Fit a Sentence in a Screen    		57.5%	Medium	
screen w x h and display a text. and given a list of font size. A font size has different height and width. (height is same for the same font size, width is depending on the char).
return the max font size you can use. if not possible return -1.
binary search
->>

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
problem: given an array and make every A[i]+A[n-1-i] all the same using [1,limit] to replace any number, and find the min number of replace. (note all elements <=limit and >=1, this is a critical information), very hard question!
Analysis: from brutal force (we need do each pair for all the targets, and then we can use optimization using difference array to reduce O(n) to O(1) (the curve is always 2s,1s,0s,1s,2s, using difference we only need to record 5 changing point). also binary search can also do it with less memory requirement.
subject: difference array and prefix sum.
level: 5
->>

### backtracking
backtracking problem generally finds all sets required. It can also be used for counting. 
- it is base for some dp problems.
- generally some prune is needed to avoid invalid search.
backtracking is similar to dfs, but it generally include put in and take out.
optimization in backtracking is very important.

<<-22	Generate Parentheses    		64.2%	Medium	->>

<<-47	Permutations II    		48.4%	Medium	
all permutation of array with duplicates.
sort and skip duplicates - ie do not start with the same element, but permutation can include duplicates.
permutation by swap 
->>

<<-46	Permutations    		65.2%	Medium	
using next permutation or backtracking.
->>

<<-52	N-Queens II    		59.1%	Hard	
place n queens on nxn chessboard. return the number of distinct solutions.
- the first row choice will affect the next row. - backtracking
- also can use bitsets to represent the place.
->>

<<-51	N-Queens    		48.2%	Hard	
return all distinct solutions
backtracking.
->>

<<-77	Combinations    		56.2%	Medium	
given 1 to n, return all possible combination of k numbers. 
backtracking with k. (start from 1 k-1)
->>

<<-90	Subsets II    		48.0%	Medium	
array contains duplicates, return all possible subsets (power set)
must not contain duplicate subsets
- sort so that we can skip duplicate elements
- each element can be chosen or not chosen
backtracking.
->>
<<-78	Subsets    		63.7%	Medium	
no duplicates. no need sort and skip duplicates.
->>

<<-248	Strobogrammatic Number III    		39.9%	Hard	
a strobogrammtic number looks the same when rotated 180 degree.
count number of strobogrammatic number in range [L,R].
0-0,1-1,6-9,9-6,8-8
backtracking: note there is additional, the digit pairs shall be at symmetric positions.
so we need use two pointer to pair them.
->>
<<-247	Strobogrammatic Number II    		48.1%	Medium	
find all the numbers of length n.
need pair. 1xxx1,6xx9,9xx6,8xxx8
->>
<<-246	Strobogrammatic Number    		45.4%	Easy	->>

<<-254	Factor Combinations    		47.0%	Medium	
return all possible combination of its factors. (no duplicate combinations)
backtrack: for n: add i n/i to complete a candidate, then use n/i for the subproblem.
see 1735 count ways to make array with product, which limit the number of factors.
->>

<<-267	Palindrome Permutation II    		36.9%	Medium	
given a string s, return all possible palindrome permutation (without duplicates).
- only need get the prefix. 
- get the hashmap and divide by 2. 
- and then the combination is similar to 254.
backtracking: 
->>
<<-266	Palindrome Permutation    		62.3%	Easy	
check if the permutation can form a palinrdome- counting.
->>

<<-282	Expression Add Operators    		36.2%	Hard	
given a digit string. add +-* so that it evaluates to target.
return all possible string.
- backtracking: prefix 1 to n, and remaining apply +-*
- subproblem: note * will need previous result, so we need save previous results.
->>

<<-291	Word Pattern II    		43.8%	Hard	
given a pattern and a string, check if s matches the pattern.
mapping relation: each char in pattern can match a string in s.
one way is to try all possibilities.
- pattern must contain duplicates, otherwise we can always map true.
- char to str map, str to char map and they cannot change.
- try all prefix to the first char match.
backtracking + hashmap.
->>
<<-290	Word Pattern    		38.1%	Easy	
string separated by space, mapping is simple. just check mutual mapping.
->>

<<-320	Generalized Abbreviation    		52.9%	Medium	
take any substr and replace with its length.
return all possible abbreviations.
backtracking: use current char or not for abbreviation.
if previous is already num, then we shall take it as char.
->>

<<-332	Reconstruct Itinerary    		37.3%	Medium	
giveb a list of tickets [from,to], reconstruct the itinerary in order start with JFK.
if multiple, return the lexi smallest. You must use the ticket all and once.
- sort the ticket (lexi order)
- build a graph 
- dfs starting from the smallest child. Need make sure one dfs will used all the tickets.
->>

<<-377	Combination Sum IV    		45.7%	Medium	
input has no duplicates, find the  number of combinations that sum=target.
- dp: dp[t]+=dp[t-num[i]]
->>
<<-216	Combination Sum III    		59.4%	Medium	
input has duplicates, find all unique combinations
- sort
- skip same 
- backtrack.
->>
<<-40	Combination Sum II    		49.4%	Medium	
input is unique, each number can be used only once, return all combinations.
backtrack. advance pointer.
->>
<<-39	Combination Sum    		58.1%	Medium	
input is unique, each number can be reused. return all possible combination with sum==target. do not need to advance pointer.
backtrack.
->>

<<-440	K-th Smallest in Lexicographical Order    		29.3%	Hard	
from 1 to n, find the kth lexi smallest integer.
similar to a tree.
->>

<<-386	Lexicographical Numbers    		52.8%	Medium	
given an integer n generate the numbers in lexi order.
backtracking.
similar to 
->>

<<-425	Word Squares    		49.4%	Hard	
kth row and column reads the same. 
given a set of words, find all word squares you can build.
(all words have the same length <=5)
- backtracking: 
- try each string at the first col/row. 
- then 2nd col/row shall start with given str. similar for others -- this is what trie does.
build a trie and do backtracking on it.
->>

<<-465	Optimal Account Balancing    		47.6%	Hard	
given a list of transaction [x,y,z] person x gave person y $z.
return the min number of transactioons to settle the debt.
- graph problem
- we can get each person's net balance from the list of transactions.
debt>0, need to pay money, debt<0 need to collect money back.
- Starting from first debt debt[0], we look for all other debts debt[i] (i>0) which have opposite sign to debt[0]. Then each such debt[i] can make one transaction debt[i] += debt[0] to clear the person with debt debt[0]. From now on, the person with debt debt[0] is dropped out of the problem and we recursively drop persons one by one until everyone's debt is cleared meanwhile updating the minimum number of transactions during DFS.
- backtracking to get the min transaction: skip duplicates.
->>

<<-416	Partition Equal Subset Sum    		44.3%	Medium	
- dfs for k=2.
- dp knapsack: choose or not choose
->>

<<-698	Partition to K Equal Sum Subsets    		45.3%	Medium	
backtracking: since the length is small, we can use bitmask to indicate which one is used.
complexity: will choose or not choose for one grouping. O(k2^n)
can also do dp: dp[mask|(1<<i)]=dp[mask]+nums[i].
->>
<<-473	Matchsticks to Square    		37.9%	Medium	
given a list of stick length, check if we can make a square using all the sticks.
need divide 4 equal parts with sum=tsum/4.
this is exactly the problem: 698 partion into k equal subset
- dfs or backtracking using mask. do not forget the loop.
->>

<<-488	Zuma Game    		39.2%	Hard	
a row of balls with color RYBGW, and you have several balls in hand. 
each time you can choose a ball at hand and put any place. If 3 or more consecutive you can remoe these balls.
return the min balls insertion to empty all balls or -1 if impossible.
dp intervals: insert a ball to form 3 or more consecutive, or waiting for connection, especially when you eliminate 3 and there is one more not eliminated. so greedy does not work.
board<=16, hand<=5
board="RRWWRRBBRR”， hand="WB"
first put B, since put W will eliminate R and remaining R cannot clear.
RRWWRRBBRR->RRWWRRBBRWR->RRWWRRBBBRWR->RRWWRRRWR->RRWWWR->RRR->""
or do dfs/backtracking without memo.
- which one to go first, m!
- which position to insert n+1 positions
prune: impossible case, remove balls not appeared in board. skip insert identical position.
size>mn, return;
TLE (adding hashset to store seen string will avoid TLE), and also gives wrong answer for:
"WWRRBBWW"
"WRBRW"
even the accepted version is not correct since it does not follow the rule. It uses some greedy approach to use two balls and each time only eliminate 3 balls even it has 3 more such balls.
->>

<<-491	Increasing Subsequences    		46.9%	Medium	
find all increasing subsequence with length>=2
backtracking: using hashset to avoid using it repeatedly. skip duplicates. (starting duplicate and goes right will get same sequence).
->>

<<-638	Shopping Offers    		52.1%	Medium	
given a list offers [na,nb,price], and [pricea,priceb] if buy separately. and target number [Na,Nb]
you can use offers repeatedly. return the min cost to buy exactly the numbers.
backtracking: with vector operations, could use dp for memoization
->>

<<-756	Pyramid Transition Matrix    		55.3%	Medium	
given bottom string and a list of allowed string (the top and bottom two forms the string), we are building a pyramid. return if we can build it.
backtracking: 
- the first two as the key and build a list of 3rd chars.
- try all these combinations.
->>

<<-784	Letter Case Permutation    		65.7%	Medium	
string of letter and digits, you can change to lowercase or uppercase
return all combination
backtracking: no change or change to upper/lower.
->>

<<-797	All Paths From Source to Target    		78.2%	Medium	
given a directed acyclic graph with n node from 0 to n-1. find all possible path from 0 to n-1.
backtracking or dfs.
->>

<<-816	Ambiguous Coordinates    		47.6%	Medium	
(x,y) the . and comma are missing. return all possible pairs of coordinates
backtracking: separate first into two strings and do backtracking by adding .
- left possible strings and right possible strings and then combine.
->>

<<-842	Split Array into Fibonacci Sequence    		36.4%	Medium	
given a string divide it into fib series.
once the first two elements are determined, the whole series are determined.
c=a+b, so c at least is as long as the larger one. limit the first two to half length and try all combinations.
->>

<<-967	Numbers With Same Consecutive Differences    		44.1%	Medium	
given n=number of digits, adjacent digit's absolute difference=k. return all numbers.
backtrack.
->>

<<-996	Number of Squareful Arrays    		47.5%	Hard	
given an array, it is squareful if every adjacent pair sum is a square number.
return number of permutation of A is squareful.
- for each number, find all its paired numbers.
- each number may have duplicate, using hashmap to count for it.
- backtracking: 
->>

<<-1079	Letter Tile Possibilities    		75.6%	Medium	
given a list of letters, return number of different string can form.
permutation with duplicates. n!/(n1!n2!..)
-backtrack. with a hashmap.
->>

<<-1215	Stepping Numbers    		42.7%	Medium	
stepping number: its neighboring digits absolute difference is 1. return a sorted list stepping number in range [L,R]
- backtrack: 
->>

<<-1258	Synonymous Sentences    		67.2%	Medium	
given a list of synonymous pairs, return all possible sentences in sorted order.
- union find the get the disjoint set for each synonymous word
- dfs/backtrack to get all combinations
->>

<<-1286	Iterator for Combination    		70.7%	Medium	
given string, length k, return the the next combination of length k.
backtrack
->>

<<-1291	Sequential Digits    		57.4%	Medium	
digit[i]=digit[i-1]+1. return all integers in range [L,R].
backtrack.
->>

<<-1307	Verbal Arithmetic Puzzle    		37.6%	Hard	
each character represents a digit from 0 to 9. and given an equation, check if it is solvable.
a 2d backtracking problem with rows and cols.
- using hashmap or vector to record letter digit mapping and used digits.
- reverse each row so that addition is more convenient.
- do it from first column (all rows) and then process to next cols with cf.
->>

<<-1467	Probability of a Two Boxes Having The Same Number of Distinct Balls    		61.3%	Hard	
given 2n balls with k colors. two boxes, each box put n balls. Two boxes are consider different. The balls are shuffled uniformly. calculate the probability that the two boxes has the same number of distinct balls.
This is a permutation problem.
- before that we need to know the permutation with duplicates n!/(A1!*....*An!)
- then we only need to count the number of permutation with equal distinct color, which can be obtained using backtracking.
- backtracking is combination approach, after we get a valid combination we need to get the permutation
- backtracking for length n array would be (A+1)^n.
- use double for factorial.
->>

<<-1593	Split a String Into the Max Number of Unique Substrings    		46.7%	Medium	
backtracking using hashset.
take from one char to the whole string and leave a subproblem.
->>

<<-1601	Maximum Number of Achievable Transfer Requests    		47.2%	Hard	
n buildings from 0 to n-1. Given a list of requests transfering from src to dest. All building is full, so net change shall be 0.
return the max number of achievable requests.
n<=20, requests<=16.
backtracking: each request can be used or not used. 2^R.
valid requests: all building is net 0.
->>

<<-131. Palindrome Partitioning
return all poosible palindrome partitioning.
typical backtracking, from left to right.
->>

<<-1655	Distribute Repeating Integers    		40.0%	Hard	
problem: given an array of integer and m customer orders, each order is number of integers. Each order shall give the same integer.
check if it is possible to fulfill the order.
important: try order from largest to smallest. (sort the array is useless since it keeps changing). If the highest order is not able fulfill, then it is done (prune)
->>

<<-1681. Minimum Incompatibility
problem: divide into k sets, each set cannot contain duplicate number, incompatibility=max-min for each set. find the min sum of incompatibility.
- backtracking: but it will find all permutations. using set to avoid revisit.
a very tricky backtracking problem. I still have problem with it.
also a bitmask dp problem.
->>

### dfs
- dfs to group connected
- dfs avoid visiting parents
- dfs for all paths
- dfs with rank to detect cycles.
a lot of cases can also be done via bfs, union find, backtracking.

<<-17	Letter Combinations of a Phone Number    		48.0%	Medium	->>

<<-130	Surrounded Regions    		28.8%	Medium	
dfs (connected to boundary are not counted)
->>

<<-200	Number of Islands    		47.9%	Medium	->>

<<-212	Word Search II    		35.9%	Hard	
given a board with char, and a list of dictionary words. find all the words in the board. (4 direction connected).
- build the trie using the list of words
- dfs on the board and find in the trie.
->>

<<-79	Word Search    		36.2%	Medium	
given a board and a string, check if the string exist in board
plain dfs.
->>

<<-302	Smallest Rectangle Enclosing Black Pixels    		52.1%	Hard	
given a binary grid, 0 white, 1 black.
black pixels are connected and only one region. return the smallest rect enclosed all black pixel.
get the xmin,xmax,ymin,ymax of the pixel region
->>

<<-329	Longest Increasing Path in a Matrix    		44.0%	Hard	
do dfs and save longest path in dp.
->>

<<-341	Flatten Nested List Iterator    		53.7%	Medium	
dfs to flatten the nested integer. leaf is integer.
->>

<<-351	Android Unlock Patterns    		49.1%	Medium	
android locks using a 3x3 grid. No jump is allowed. 
however you can jump if the point is selected before.
given m and n, your lock must involves at least m points and at most n points.
pattern is unique if there is a dot not in other or the order is different.
- symmetric: 1,3,7,9 are symmetric (starting point), 2,4,5,8 are symmetric
so we only consider start from 1,2,5.
- start from 1, it can go to 2,5,4 only, reduce on step.
- start from 2, it can go to 1,3,4,5,6 only, reduce one step.
- start from 5, it can go to 1,2,3,4,6,7,8,9, reduce one step.
- you can skip 1<->3 skip 2, 1<->7 skip 4, 7-9 skip 8, 3-9 skip 6, 3-7 skip 5, 4-6 skip 5...
actually the skip matrix is enough and we can try all 9 positions.
dp or dfs.
->>

<<-403	Frog Jump    		40.6%	Hard	
first jump is 1, if last jump is k, then next jump could be k-1,k,k+1.
check if frog can land on the last stone.
- bfs: put all possible position in queue
- dfs: check if any works prune.
- dp:  
->>

<<-417	Pacific Atlantic Water Flow    		41.9%	Medium	
dfs with two states.
->>

<<-489	Robot Room Cleaner    		71.5%	Hard	
given robot API: move, turnLeft,turnRight, clean
design algorithm to clean the entire room.
simulate the dfs. from current position and goes one direction until blocked and then try other direction. going back one step.
since we do not know the matrix, using hashmap to record visited cell
- clean current cell and mark visited.
- loop 4 directions
- dfs to clean all cells along current direction.
- back off one cell and try other directions.
->>

<<-529	Minesweeper    		60.2%	Medium	
given a 2d grid, 'M' unrevealed mine, 'E' unrevealed empty cell, 'B' revealed blank cell that has no ajacent mines (8 directions), digit '1' to '8' is number of mines adjacent. 'X' revealed mine.
return the next board status:
- if M is revealed,game over change to 'X'
- 'E' change to 'B' if no adjacent mines and all of its adjacent shall be revealed
- 'E' change to digit if there is adjacent mines
dfs.
->>

<<-565	Array Nesting    		55.7%	Medium	
array from 0 to N-1, find the longest link [A[i],A[A[i]],A[A[A[i]]]...).
simiar to dfs, use visited array and dfs.
->>

<<-339	Nested List Weight Sum    		75.0%	Easy
do dfs and depth*element
->>

<<-364	Nested List Weight Sum II    		63.1%	Medium	
now changed to bottom level depth is 1.
approach 1: 
for example 1x+2y+3z=4*(x+y+z)-(3x+2y+1z)
so just (depth+1)*flatsum-weightsum in 339.
approach 2: bfs.
approach 3: do dfs for example 
we see x, sum=x, 
then we see y, sum+=flatsum+y->2x+y, 
we see z: sum+=sum+z ->2x+y+x+y+z, ie we get the flat sum and keep adding. 
->>

<<-688	Knight Probability in Chessboard    		49.6%	Medium	
dfs
->>

<<-695	Max Area of Island    		63.7%	Medium	
dfs calculate the number of nodes in each set.
->>

<<-694	Number of Distinct Islands    		56.8%	Medium	
only need to remove the reference. translation only.
->>

<<-711	Number of Distinct Islands II    		48.6%	Hard	
supports rotation (90,180,270 degrees) or reflections
for each set we save the points with 8 different format and convert to string and push into hashset.
see dihedral_group in wiki
- reflection 2, rotation 4, total 8 combinations
- save all the 8 shapes
- remove the reference (shift to origin)
- sort the 8 shapes so we get the smallest representation and use it as key.
->>

<<-733	Flood Fill    		55.6%	Easy	
start (sr,sc) and flood fill with a new color. dfs.
->>

<<-749	Contain Virus    		47.1%	Hard	
given a 2d grid, 0 uninfected, 1 infected. a wall can be installed between ajacent cells. Each nigh the virus spreads to its neighboring cells. Each day you can install walls around only one region- the affected area the threatens the most unaffected cells.
if it is possible to save? number of walls required?
- need dfs to get region and number of cells to be infected. = number of walls
- growing of all other regions.
- quarantine region can mark as -1 so no dfs will be done.
- using dfs/bfs/union-find to save each set's infect list.
- store information in heap.
->>

<<-886	Possible Bipartition    		44.7%	Medium	
dfs coloring alternatively.
->>

<<-934	Shortest Bridge    		49.2%	Medium	
shortest bridge between two island.
group all positions in each set and try the distance.
dfs to two groups.
->>

<<-959	Regions Cut By Slashes    		66.6%	Medium	
upscale 3 by 3 and do bfs/dfs.
->>

<<- 1706. Where will the ball fall
the grid with / and \,
upscale the grid by 3, and then use bfs or dfs to find which one to go. bfs is better (for implementation)
->>

<<-980	Unique Paths III    		77.0%	Hard	
from start to end, 0 can go, -1 cannot go.
return number of paths what walk over every empty cell exactly once.
dfs: visited cannot visit again, final nstep is the total empty cell.
->>

<<-1020	Number of Enclaves    		58.5%	Medium	
find those cannot connect to boundary
dfs
->>

<<-1034	Coloring A Border    		45.1%	Medium	
same value and connected. border has at least a neighboring cell which is not the connected group.
dfs to mark negative as visited and then check all negatives and see if they have a cell not in the group.
->>

<<-1219	Path with Maximum Gold    		65.6%	Medium	
mxn matrix with gold, 0 means you cannot cross. You cannot visit the same cell again.
you can start and stop any place.
- dfs: at any position with gold do dfs, visited changed to 0. and find the max.
->>

<<-1254	Number of Closed Islands    		61.1%	Medium	
0 is land, 1 is water.
closed island are those 0s which does not touch the boundary.
dfs: mark if the dfs touched the boundary. or first dfs using boundary 0s to change to 1. and then dfs inner nodes of 0.
->>

<<-1306	Jump Game III    		60.5%	Medium	
given an array you are at index start. At i, you can jump to i+A[i] or i-A[i].
check if you can jump to any position with value 0.
dfs using visited hashtable.
->>

<<-55	Jump Game    		34.9%	Medium	
you are at the first element, each element represents the max step you can jump.
determine if you are able to reach the last index.
bfs similar: update the max i+nums[i] 
->>

<<-1377	Frog Position After T Seconds    		34.2%	Hard	
given an undirected tree with n vertices from 1 to n. Starts at 1, it can jump to direct connected vertices, cannot go back to visited nodes. It randomly jump to its neighbors with same probability. 
given time t and target node, find the probability on the target at time t.
dfs: decrease t and get its neighbors
leaf node: 1/0
->>

<<-1462	Course Schedule IV    		43.9%	Medium	
given n course from 0 to n-1 and a list of prerquisitites. given a list of queries (a,b) need to return if a is b's prerequisites.
- the graph will form several connected graphs.
- post order traversal to get the parent child relations.
->>

<<-1559	Detect Cycles in 2D Grid    		44.8%	Hard	
given a matrix, find if there exists a cycle, a cycle is connected path with same value.
dfs with parent position, we cannot go back to immediate parent. If we found we back to a visited node, we found a cycle.
very easy to make mistakes.
->>

<<-1568	Minimum Number of Days to Disconnect Island    		51.1%	Hard	
given a 01 matrix, 0 is water, 1 is island. You can change one island to water in one day. return min number of days to disconnect the island.
greedy: 
- if we have >1 islands, we need do nothing
- critical edge: 1 day
- otherwise disconnect the top left or bottom right cell using 2 days.
using dfs or union find to determine if it is one island.
->>

<<-1600	Throne Inheritance    		58.8%	Medium	
inheritance in the order of dfs. first child is the first inheritance.
birth: a new baby
death: a death of a family member.
using hashmap to represent the family structure.
person vs a list of direct children, with live or death.
dfs, hashmap
->>

### bfs
- bfs with queue
- bfs with deque pop from both end
- bfs with priority_queue
- bfs with parent information
- bfs with more than one state.
- bfs with shortest distance problem

<<-127	Word Ladder    		30.7%	Medium	
shortest change from one string to other string in dictionary
bfs.
->>
<<-126	Word Ladder II    		23.0%	Hard	
find all possible transform with shortest path.
need parent information to trace back using bfs.
->>

<<-286	Walls and Gates    		55.5%	Medium	
given a grid, -1: a wall, 0,a gate, inf: empty room.
find the nearest distance of empty room to gate.
bfs.
->>

<<-301	Remove Invalid Parentheses    		44.1%	Hard	
remove min number of parenthesis to make it valid, return all possible combinations.
backtracking: ???
bfs: if we find the first invalid, we shall stop adding stuff into queue.
try remove each ( or ). slower than backtracking.
->>

<<-317	Shortest Distance from All Buildings    		42.1%	Hard	
given a grid, 0 means empty, 1 building,2 obstacle.
find an empty cell which has the shortest distance sum to all building.
bfs from all other building and add the distance for each empty cell.
- can do bfs for each building, simpler
- do bfs all, we need to have n (if visited n times, no more visit)
optimization: if one cell is not reachable from any building, do not consider it.
->>

<<-433	Minimum Genetic Mutation    		42.6%	Medium	
a gene string is 8 char ACGT, mutation is one single char changed.
given a bank of valid gene strings. given a start string, return the min number of permutation to change to the end string.
bfs.
->>

<<-527	Word Abbreviation    		55.5%	Hard	
given n strings, generate abbreviation for each word with min length
- begin with first char followed by number of digits + last char
- if conflict using longer prefix
- if cannot keep abbreviation shorter, keep the original.
bfs: first round, remove those string which is unique using first and last.
using hashmap to save the ambiguous strings. the use longer prefix for the remaining.
->>

<<-210	Course Schedule II    		41.7%	Medium	
given a list of prerquisite, return the ordering of course taken
bfs: remove source nodes layer by layer.
->>
<<-207	Course Schedule    		43.8%	Medium	
given a list of prerquisites, check if we you can finish all the courses.
dfs: no cycle. detect if there is a cycle.
->>

<<-675	Cut Off Trees for Golf Event    		34.9%	Hard	->>

<<-752	Open the Lock    		52.3%	Medium	
GIVEN 4 circular wheel with 0-9 on it. You can turn around the wheel, each move turn the wheel one digit. lock initial is 0000. You are given a list of deadends that the wheel stops turning.
given a target, return the min total number of returns.
bfs. you can rotate clockwise or counterclockwise.
->>

<<-773	Sliding Puzzle    		60.2%	Hard	
given 2x3 board with 1-5, return min number of moves to solve the board.
bfs: define the possible direction to go.
->>

<<-815	Bus Routes    		43.0%	Hard	
given a list of routes, each route will repeat several stops. given Start to Destination, return the min number of bus we shall take.
- each route is a set, multiple routes may connect at different stops.
- bfs: we add the connected stops in our queue. hashmap to convert to stop to route (a stop may belong to multiple route). visited is for route.
->>

<<-841	Keys and Rooms    		64.9%	Medium	
given n rooms from 0 to n-1. Initially you are in room 0, each room may have some keys to access other room. check if you can unlock each room.
simple bfs
->>

<<-854	K-Similar Strings    		38.3%	Hard	
if we can swap the positions of two letters in A exactly k times so that A=B.
return the smallest k.
bfs to find the shortest distance from A to B.
->>

<<-864	Shortest Path to Get All Keys    		41.1%	Hard	
given 2d grid, '.' empty cell, '#' wall, '@' start, lowercase letter are keys, capital letters are locks. You cannot walk over a lock unless you have the key.
return min number of moves to get all keys. impossible return -1.
using bitmask to represent the keys you are carrying.
bfs.
->>

<<-909	Snakes and Ladders    		38.9%	Medium	
convert to 1d bfs.
->>

<<-994	Rotting Oranges    		49.4%	Medium	->>

<<-1036	Escape a Large Maze    		35.1%	Hard	
from src to target, with obstacles <=200. 
with obstacles, it is only able to enclose a small region. If we cannot escape the enclosed area, we cannot reach target.
bfs or dfs with optimization: obstacles around src, obstacles around target. if dfs can reach out the max radius (defined by 200) then we can reach.
dfs with greedy
->>

<<-1091	Shortest Path in Binary Matrix    		38.6%	Medium	
given a binary matrix, 1 means blocked. You can go 8 directions.
regular bfs.
->>

<<-1129	Shortest Path with Alternating Colors    		39.6%	Medium	
assemble the edge with odd/even property and do bfs.
->>

<<-1136	Parallel Courses    		61.1%	Hard	
given a list of prerquisitites relations. In a semester you can study any number of courses if cleared. return the min semester needed.
- put all sources in queue and clear them and do bfs.
->>

<<-1197	Minimum Knight Moves    		36.7%	Medium	
knight at (0,0), return the min moves to (x,y)
- limit it in the first coordinate.
- bfs: convert target to first coord, and try all 8 directions and build a matrix in first coord.
just convert the jump position to abs(x) and abs(y)
it is also important: limit the xy not go beyond 400x400, use array instead hashset for visited. Otherwise it will TLE soon.
->>

<<-1210	Minimum Moves to Reach Target with Rotations    		45.9%	Hard	
snake spans (0,0) and (0,1), The matrix is 01 matrix, 1 mean obstacles, min moves from top left to bottom right.
- move right one cell, keep previous horizontal/vertical
- move down one cell, keep previous horizontal/vertical
- Rotate clockwise if it's in a horizontal position and the two cells under it are both empty. In that case the snake moves from (r, c) and (r, c+1) to (r, c) and (r+1, c).
- Rotate counterclockwise if it's in a vertical position and the two cells to its right are both empty. In that case the snake moves from (r, c) and (r+1, c) to (r, c) and (r, c+1).

bfs keep the snake's two cell in the queue and push 4 possible positions into queue.
->>

<<-1263	Minimum Moves to Move a Box to Their Target Location    		42.4%	Hard	
with a person to push the box, grid with obstacles.
typical bfs: using two bfs
- clear start/end/player position with empty cell
- if we want to move the box to left,up right,down, the person shall be able to move corresponding position.
->>

<<-1284	Minimum Number of Flips to Convert Binary Matrix to Zero Matrix    		69.8%	Hard	
given a 01 matrix, you can choose a cell and flip it and its neighbors. 
bfs using the matrix as the queue data and visited using its string or matrix.
->>

<<-1293	Shortest Path in a Grid with Obstacles Elimination    		42.8%	Hard	
matrix with 01, 1 is obstacle. given k, you can remove at most k obstacles and return the shortest path.
bfs: using (i,j,numRemoved) for the queue data. 
when number of obstacle > m-1+n-1, then we get the shortest length using dx+dy
this can be used in each step.
->>

<<-1298	Maximum Candies You Can Get from Boxes    		59.5%	Hard	
given n boxes with box[i]=[status, candies, keys, containedBoxes]
status: open or close
candies: number of candies inside
keys: the key list for other boxes.
containedBoxes: a list of boxes inside this box.
given a list of initial open box, return the max candies you can get.
bfs: push all unopened boxes into queue and keep a list of owned keys.
->>

<<-1391	Check if There is a Valid Path in a Grid    		45.0%	Medium	
Given a m x n grid. Each cell of the grid represents a street. The street of grid[i][j] can be:
1 which means a street connecting the left cell and the right cell.
2 which means a street connecting the upper cell and the lower cell.
3 which means a street connecting the left cell and the lower cell.
4 which means a street connecting the right cell and the lower cell.
5 which means a street connecting the left cell and the upper cell.
6 which means a street connecting the right cell and the upper cell.
check if there is a path from top left to bottom right.
regular bfs, if it is able to connect then add into queue.
->>

<<-1654	Minimum Jumps to Reach Home    		28.0%	Medium	
a line with forbidden position. given a and b. you can jump forward by a, or jump backward with b. You cannot jump backward twice in a row. cannot jump to forbidden position.
given position x, return the min number of jumps to home (at position 0)
bfs: with two status. either from right or from left.
need store position and direction also, the visited array.
->>

## union-find

- union-find with nothing, O(n)
- union find with rank O(logn)
- union find with path compression O(1)
- use vector to store parent
- use hashmap to store parent

<<-128	Longest Consecutive Sequence    		45.7%	Hard	
unsorted array find the length of the longest consecutive elements.
- union find O(N)
- sort O(nlogn)
->>

<<-305	Number of Islands II    		39.8%	Hard	
grid is initally all water, given a list of positions to add land. return number of island for each move.
union-find.
->>

<<-323	Number of Connected Components in an Undirected Graph    		56.9%	Medium	
union-find.
->>

<<-685	Redundant Connection II    		32.8%	Hard	
a rooted tree. given a list of directed edges node from 0 to n-1. return an edge that can be removed to that the resulting graph is a rooted tree of n nodes. if there are multiple answers, remove the last one.
- union find. for current edge, if it already has parent, this edge is forming a cycle, a->b, (a,b) and (parent(b),b) are candidate.
two cases: 
- two parents
- a cycle is formed.
->>

<<-684	Redundant Connection    		58.3%	Medium	
undirected gaph. so parent information is not clear from the edges.
the edge forms the cycle. using union find.
->>

<<-721	Accounts Merge    		50.4%	Medium	
name vs a list of email address. email address is unique.
union find to merge email address with parent (index)
->>

<<-737	Sentence Similarity II    		46.3%	Medium	
given a list of similar word pairs, and determine if two sentences are similar
union-find. using string-string for parent structure.
->>
<<-734	Sentence Similarity    		42.2%	Easy	
similarity is not transitive. using hashmap, A-B are mutual.
->>

<<-803	Bricks Falling When Hit    		31.1%	Hard	
given a 2d matrix, 1 represent a brick.
a brick is stable if:
- it is directly connected to the top of the grid or
- its neighboring brick is stable.
given a list of hit which removes the brick. Those unstable brick will fall.
return the number of bricks falling for each hit.
union-find: all stable bricks are connected to top. we can do reversely, adding the hit how many can add.
->>

<<-827	Making A Large Island    		46.4%	Hard	
given a 2d matrix, you can at most change one 0 to 1. return the max size of the island.
- one island only, add 1 (if there is 0)
- more than 1 island, (not necessary the largest two connect), we shall try all 0 cell.
- a cell may be able to connect 1,2,3,4 sets together.
union-find.
->>

<<-839	Similar String Groups    		39.1%	Hard	
two strings are similar if we can swap two letters in x so that it equals y. 
given a list of words, return the number of similar groups
union-find.
->>

<<-924	Minimize Malware Spread    		41.9%	Hard	
remove one node from initial list (not disconnecting)
the one with the most number of nodes in the set.
union-find.
->>

<<-928	Minimize Malware Spread II    		41.0%	Hard	
given a list of nodes initially infected. we can remove one infected node from initial list and break its all connections. return the first node removed which can minimize malware spread.
- disconnect not convenient to do, but we can do add without it.
- union find without given node, and find the min infected set.
->>

<<-947	Most Stones Removed with Same Row or Column    		55.3%	Medium	->>

<<-952	Largest Component Size by Common Factor    		36.0%	Hard	
given a list of numbers, if two numbers share a common factor, there is an edge between them
union find.
->>

<<-990	Satisfiability of Equality Equations    		46.0%	Medium	
given array of strings represent logical relation. check if all equations are satisfied.
union-find the equal
->>

<<-1061	Lexicographically Smallest Equivalent String    		66.0%	Medium	
given string A and B, they are equivalent if we do the mapping. given a string s, return the smallest equivalent string.
union find the put equivalent chars in a set then use the smallest char to replace the string. union find with rank.
->>

<<-1101	The Earliest Moment When Everyone Become Friends    		66.8%	Medium	
union-find
->>
<<-1135	Connecting Cities With Minimum Cost    		58.6%	Medium	
greedy: try smallest edge first, MST using union find.
->>

<<-1202	Smallest String With Swaps    		47.5%	Medium	
given a string and a list of index pairs. You can swap the character pair by the index pair any number of times. return the smallest string.
- union-find: characters in one set can be sorted.
->>

<<-1319	Number of Operations to Make Network Connected    		54.6%	Medium	
union find the disjoint set and connect these disjoint set.
->>

<<-1489	Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree    		52.2%	Hard	
given a undirected graph with n nodes from 0 to n-1. The graph is given by a list of weighted edges [i,j,w]
a MST is the connected graph with min total edge weight.
critical edge: if deleted the MST weight will increase.
pseudo critical edge: if added no weight changed.
- calculate the MST weight
- loop to check remove edge or add edge into MST weight change.
- union find to construct the MST, greedy choose the edges (sort)
->>

<<-1697. Checking existence of edge length limited paths
given a graph with n nodes from 0 to n-1, and a list of edges with length. given a list of query of node i,j and limit distance, check if they are connected using edge <limit.
sort the edge using length
sort the query using length
union find by the limit length in increasing order
->>

<<-1562	Find Latest Group of Size M    		39.0%	Medium	
given array of permutation of 1 to n. a binary string of length n. each element means change bit[i] to 1. 
return the latest step at which there exists a groups of ones of length exactly m. if no such group return -1.
- number of 1s keep increasing.
- ask for latest, i.e the last step producing a M-segment.
 interval merge? union-find.
 maintain two hashmap:
 parent vs node count
 node count vs number of sets.
 ->>

<<-1627	Graph Connectivity With Threshold    		36.9%	Hard	
n cities from 1 to n. Two cities are directly connected if they share a common divisor >threshold.
given two cities, check if they are connected.
union-find: a-b b-c then a and c is connected.
use sieve method to avoid TLE.
->>

<<-1331	Rank Transform of an Array    		57.9%	Easy	
simple version of the 2d rank problem. sort and assign values.
->>

<<-1632. Rank transform of a matrix
mxn matrix. rank matrix: smallest element in its row and column shall be 1. smaller value smaller rank. rank shall be as small as possible. same value on the row and col shall have the same rank.
- greedy: process in sorted order.
- using map to store element vs positions
- using union find to group the same value elements into groups and process each group separately
- maintain row and column current max.
- each group same value elements are assigned the group max +1
->>

## divide and conquer
merge sort, recursion et al often involves in divide and conquer.
divide into two subproblem recursively and then combine two small problem to get the larger problem answer.

<<-315	Count of Smaller Numbers After Self    		42.3%	Hard	
- from right to left and store the elements in sorted order then we can use binary search. to count, it is still O(N^2) will TLE.
- merge sort: divide and conquer, count right smaller than left.
- binary tree: from right to left, build a BST. Since we may have duplicates, add a field to count the duplicates. when we insert a new value, all its left subtree are smaller than it. We use prefix sum to count the sum of duplicates under the subtree.
->>

<<-493	Reverse Pairs    		26.0%	Hard	
impotant reverse pair i<j, nums[i]>2*nums[j]
return number of important reverse pairs.
- using map to store previous visited elements, and binary search O(N^2) since the distance is O(N)
- segment tree.
- merge sort divide and conquer. divide into left and right part and then use two pointer to compare O(n/2)+O(n/4)+....
->>

<<-95	Unique Binary Search Trees II    		41.6%	Medium	
generate all trees. divide and conquer, or recursive.
->>

<<-395	Longest Substring with At Least K Repeating Characters    		41.9%	Medium	
each char in the substring appear >= K times.
- divide and conquer: the char appear less than k times will not be a part of the solution, so we can divide it into several parts, and solve them independently.
->>

<<-932	Beautiful Array    		60.9%	Medium	
given permutation of 1 to n. rearrange so that for every i<j there is no k i<k<j with 2*A[k]=A[i]+A[j]
idea: A[i]+A[j] must be odd. keep the odd and even separated. If the odd is beautiful then the even follow the same pattern is also beautiful.
- divide and conquer
- first step: divide into odd and even
- further divide.
another approach:
if A is beautiful, then 2A, 2A+1, 2A-1 is also beautiful.
[1], 2*i-1 ->1 2i->2 [1,2]
[1,2] 2*A-1->[1,3], 2i->[2,4] -->[1,3,2,4]
This is also reverse thinking. If it is hard to rearrange, why not build it from beginning.
->>

<<-241	Different Ways to Add Parentheses    		56.5%	Medium	
add () to the expression and evaluate all possible results. operator only include +-*
- parentheis appear the digit and after the digit.
- shall keep left>=right.
- () only changes the calculation order, we do not need to do the addition actually.
- we see +-*, and we divide into left and right and combine
- using memoization to record the results - dp.
- divide and conquer using the operators.
->>

## sliding window

sliding window sometimes is tricky, especially when combined with other stuff, such as priority_queue, dp.

<<-134	Gas Station    		40.5%	Medium	
n gas station along a circular route. ith station with gas[i] and it cost cost[i] from station i to i+1.
you begin with empty gas. return the starting index that you can finish the whole route. (clockwise)
- sliding window with n. net gas= gas[i]-cost[i] at any time shall >=0
- unfold the circular array to 2N array.
->>

<<-76	Minimum Window Substring    		35.4%	Hard	
min window substring which contains all the characters in t.
- we can ignore chars not in t and maintain hashset 
- sliding window: (this string is not limited to lowercase letters)
->>

<<-159	Longest Substring with At Most Two Distinct Characters    		49.8%	Medium	
using hashmap+sliding window.
->>

<<-209	Minimum Size Subarray Sum    		38.9%	Medium	
find the min length subarray sum >=target.
hashmap or sliding window.
->>

<<-340	Longest Substring with At Most K Distinct Characters    		44.7%	Hard	
two pointer, when hashmap size >k, keep shrinking the left.
->>
<<-424	Longest Repeating Character Replacement    		47.7%	Medium	
return the longest repeating subarray after performing at most k char replacement.
sliding window with at most k different chars.
- counting char
- make sure in the subarray w-maxcnt>k 
->>

<<-438	Find All Anagrams in a String    		44.3%	Medium	
fixed window size sliding + hashmap.
->>

<<-485	Max Consecutive Ones    		53.5%	Easy	->>
<<-487	Max Consecutive Ones II    		47.9%	Medium	
you can flip 0 to 1 at most once. find the longest window of all 1.
sliding window: record previous previous 0 position and update the length.
->>

<<-904	Fruit Into Baskets    		42.7%	Medium	
you have two baskests, each basket holds one type of fruit. You start at any index of the array. if you cannot choose, stop. each tree you can pick one fruit.
return max amount of fruits
equivalent find the longest subarray with only two types of fruit. 
sliding window + hashmap.
->>

<<-992	Subarrays with K Different Integers    		49.9%	Hard	
sliding window with hashmap
->>

<<-1004	Max Consecutive Ones III    		60.1%	Medium	
change 0 to 1 for at most k time.
equivalent: find the longest window containing at most k 0s. 
->>

<<-1033	Moving Stones Until Consecutive    		42.6%	Easy	
3 stones only.
->>

<<-1040	Moving Stones Until Consecutive II    		53.3%	Medium	
min move and max move
sliding window for min: find the window with length=n with min empty slots or max filled.
greedy for max: move the leftmost to next right available position or rightmost to left available slot.
->>
<<-1052	Grumpy Bookstore Owner    		55.5%	Medium	
sliding window to find the max grumpy 
->>

<<-1100	Find K-Length Substrings With No Repeated Characters    		73.4%	Medium	
find number of substrings. hashmap sliding window.
->>

<<-1151	Minimum Swaps to Group All 1's Together    		58.7%	Medium	
equivalent to find the window with most 1s. window size=total number of 1s.
->>

<<-1156	Swap For Longest Repeated Character Substring    		47.7%	Medium	
equivalent: find the longest window with only one char different from the other one. You need has at least one outside the group.
try each index as the repeat char and expand the window.
->>

<<-1163	Last Substring in Lexicographical Order    		35.7%	Hard	
or the max substring.
a string is larger: prefix is larger or it is longer.
is the first largest char suffix string the last string?
not really, for example "cacacb", the max is "cb"
build a trie and the rightmost brnach is the answer.
- the answer is the suffix string with max char as the first char. (so we can get rid of those starting char not the max char, but worse case will not improve)
- if we compare the n suffix string, we will get O(N^2)
- using two pointer: 
i the best candidate position,
j loop over following chars
k: the length or following chars.
if s[j+k]>s[i+k] then i is not best, move to j.
->>

<<-1176	Diet Plan Performance    		53.9%	Easy	
for k days, if the sum <L, get -1, if sum>R get 1, otherwise get 0.
fixed size sliding window.
->>

<<-1208	Get Equal Substrings Within Budget    		42.9%	Medium	
two equal length string s and t, we want to change s to t by same locations at the cost of |s[i]-t[i]|.
given a maxcost, and find the longest substring <=maxcost.
sliding window.
->>

<<-1248	Count Number of Nice Subarrays    		56.6%	Medium	
find number of subarrays with k odd number inside.
sliding window: only keep the odd numbers with fixed k window.
->>

<<-1343	Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold    		64.3%	Medium	
fix size sliding window.
->>

<<-1352	Product of the Last K Numbers    		42.6%	Medium	
sliding window product using prefix product
->>

<<-1358	Number of Substrings Containing All Three Characters    		60.0%	Medium	
a string consists of only a,b,c. find number of strings containing all 3 chars.
- we only need to find the smallest window containing the 3 chars. and all previous including the window are all valid substrings.
sliding window to find the min window containing abc.
->>

<<-1703. Min adjacent swaps for k consecutive ones.
sliding window with some tricks
- if we slide window on original array, we need binary search to find the size first.
- if we only slide window on the one's index, then it is fixed k size sliding window.
- then the price is to move all 1 to the median position. we are looking for the min.
all left ones move to left median and all right ones move to right median (left or right could be the same for odd length k)
why? this is similar to best meeting point. for two, any between point will get the same cost. for 3, keep the median unchanged and two moves to the median position)
however, we do not need to move to the median position and that is why we need subtract the extra
which is k*(k+1)/2/2.
->>

<<-1423	Maximum Points You Can Obtain from Cards    		45.1%	Medium	
take cards from either end, return the max points taking k cards.
equivalent: sliding window to get min sum using n-k.
->>

<<-1456	Maximum Number of Vowels in a Substring of Given Length    		53.5%	Medium	
a substring of length k, return the max number of vowels.
sliding window with size k.
->>

<<-1461	Check If a String Contains All Binary Codes of Size K    		46.3%	Medium	
k=2，00，01，10，11.
so using sliding window to get the number and set the flag.
->>

<<-1493	Longest Subarray of 1's After Deleting One Element    		58.6%	Medium	
sliding window with at most one 0 inside. record previous 0 position.
->>

<<-1695. Maximum Erasure value.
given an array of positive numbers, erase a subarray containing unique elements. return the max sum of the subarray.
using hashmap or hashset. (using hashset, when you add an element which is present in hashset, keep popping left elements until the element is not present).
->>

## sort
- sort makes things easier.
- sort using given order

<<-147	Insertion Sort List    		43.8%	Medium	
similar to array insertion sort. insert list node one by one.
->>

<<-148	Sort List    		45.2%	Medium	
merge sort!
->>

<<-164	Maximum Gap    		36.3%	Hard	
unsorted array find the max difference between ajacent elements in its sorted form.
do it in O(N).
bucket sort: arrange it in n buckets and we only need to store min/max in each bucket.
->>

<<-324	Wiggle Sort II    		30.4%	Medium	
idea: we shall find the median and divide them into smaller and larger.
brutal force: we arrange left right left right ...
virtual index: this is hard.
->>
<<-280	Wiggle Sort    		64.3%	Medium	
do it inplace. this allows >= and <=
sort and swap neighboring.
->>

<<-215	Kth Largest Element in an Array    		56.9%	Medium	
partitioning
nth_element 
priority_queue.
->>

<<-451	Sort Characters By Frequency    		63.8%	Medium	
lambda with capture
```
        sort(begin(s),end(s),[&](char a,char b){
            return mp[a]>mp[b] || (mp[a] == mp[b] && a < b);
        });
```
note we need take care ==, otherwise they will not sort.	
this is slower than using count sort.	
->>

<<-791	Custom Sort String    		65.8%	Medium	
sort T using S's order.
- count sort.
- use lambda function s.find(a)<s.find(b)
->>

<<-899	Orderly Queue    		52.8%	Hard	
given a string and integer k. each time choose the one of the first k letters and move to the end of string. return the smallest string we can get.
k=1, rotation
k>1, sort, you have a buffer >1 and can swap any pair.
->>

<<-912	Sort an Array    		64.1%	Medium	
implement qsort or merge sort.
->>

<<-969	Pancake Sorting    		68.4%	Medium	
you can choose the prefix with k (k can be any length) length and reverse it.
sort the array and gives the array of k.
greedy: find the max element and flip to the first, and then reverse to the last. max 2n operations.
since the input is the permutation of 1 to n, so we do not need find max.
->>

<<-1030	Matrix Cells in Distance Order    		66.9%	Easy	
distance to reference (r0,c0), lambda function with capture.
->>

<<-1122	Relative Sort Array    		67.7%	Easy	
given two arrays A and B, sort A according to B, elements not in B shall go after in sorted order.
using map and do count sort.
->>

<<-1333	Filter Restaurants by Vegan-Friendly, Price and Distance    		56.9%	Medium	
restaurants[i] = [idi, ratingi, veganFriendlyi, pricei, distancei]
filter them by veganfriendly=true or false, and max price and max distance, first by rating, then by id.
filter and then sort.
->>

<<-1337	The K Weakest Rows in a Matrix    		69.5%	Easy	
weakness = (number of 1s in row && row index)
stable sort.
->>

<<-1366	Rank Teams by Votes    		54.5%	Medium	
each voter votes the rank for all teams. for example "ABC",the voter's ranking is A>B>C.
if tie, then see next position votes.....
if tried every position, then rank by team's name.
lambda or customized sorting.
->>

<<-1451	Rearrange Words in a Sentence    		58.4%	Medium	
rearrange by length, if tie, keep original order.
using stable_sort.
->>

<<-1471	The k Strongest Values in an Array    		58.3%	Medium	
sort using lambda function with a parameter or customized compare function
->>

## two pointer
<<-11	Container With Most Water    		51.8%	Medium	
two pointer: water determined by the min height * the width.
->>

<<-18	4Sum    		34.3%	Medium	
to use two pointer, must sort first.
->>
<<-16	3Sum Closest    		46.2%	Medium	->>
<<-15	3Sum    		27.4%	Medium	->>
<<-1	Two Sum    		45.8%	Easy	
hashmap for O(N)
->>

<<-68	Text Justification    		28.7%	Hard	
given a list of words, and maxWidth, padding space so that each line is exactly maxWidth.
extra space shall be distributed evenly. if not divisiable, the left will be assigned more.
each line: starting word and last word has no spaces. When a word cannot add to the line, goes to next line
two pointer: pay attention to the last line. It is not that easy.
->>

<<-75	Sort Colors    		48.4%	Medium	
only 3 colors
- count sort
- approach 2: three pointers, i for 0, j for 1 and k for 2.
i<j<k. i grows from left and k grows from right to left. 
->>

<<-251	Flatten 2D Vector    		46.0%	Medium	
similar to nested integer, implement next and hasNext
using two pointer: i for inside vector, j for vector elements.
->>

<<-259	3Sum Smaller    		48.4%	Medium	
find the number of triplet that sum < target.
sort and then use 3 pointer.
>=target k--
<target ans+=k-j, j++ (fix i, and solve the two pointer problem)
->>

<<-418	Sentence Screen Fitting    		32.8%	Medium	
we need add or delete a space, using two pointer, for sentence we add/remove a space, for screen we need %width.
->>

<<-466	Count The Repetitions    		28.5%	Hard	
S=[s,n] repeat s n times. 
s1 is obtainable from s2, means that s1 is a subsequence of s2.
given s1,n1,s2,n2 and S1=[s1,n1],S2=[s2,n2]
find the max integer M so that [S2,M] is obtainable from S1.
approach: 
- you do not need to construct the string since n1 or n2 is very large
- two pointer search s1*n1 in s2*n2 (using pointer go monotonically but use % for the char)
- get the max repeat
->>

<<-713	Subarray Product Less Than K    		40.3%	Medium	
return number of subarray
to avoid overflow, we have to use sliding window using two pointer.
->>

<<-727	Minimum Window Subsequence    		42.1%	Hard	
given S and T, find the min size substr of S so that T is a subsequence of it.
a very good question:
approach 1: two pointer: i for S, and j for T. 
first find one match, and from right to left we get the smallest length for this one
then we proceed from next char and find next match
for example: abcdebdde match against bde
we first find "bcde" and from right to left get the smallest length is 4
then we start from c: "cdebdde" and we find "bdde".
- dp approach: dp[i,j] stores the starting index of S[0..i] and T[0...j].
if S[i-1]==T[j-1] then the index no change.
else dp[i,j-1] (since we only matched j-1 chars)
invalid put -1.
->>

<<-777	Swap Adjacent in LR String    		35.1%	Medium	
string of 'L','R','X', you can replace XL to LX or RX to XR.
check if we can convert A to B.
XL->LX we push L to left
RX->XR we push R to right
L cannot pass right R, R cannot pass left L.
check one by one: 
- first they shall one by one equal ignoring x. (two pointer)
- L's index in L(B)>=L(A), R(B)<=R(A)
to make it easier first return -1 for invalid cases.
->>

<<-809	Expressive Words    		46.7%	Medium	
two pointer compare.
->>

<<-844	Backspace String Compare    		46.7%	Easy	
string with # to backspace previous char. return two string equal.
two pointer from right to count.
->>

<<-905	Sort Array By Parity    		74.9%	Easy	
even first.
two pointer: i for current position, j for even number position
->>

<<-948	Bag of Tokens    		46.2%	Medium	
given intial power P and score 0 and a bag of tokens. each token has different power and can only be used once. 
- if your current power >=token[i] you can take the token, losing token[i] power and get 1 point
- you can lose 1 point and get token[i]
return the max score.
greedy approach: use the point to get the max power. losing min power to get 1 point.
using two pointer to do it from min and max side.
->>

<<-1023	Camelcase Matching    		57.1%	Medium	
given a list of string, and a pattern string, if you can insert 0 or more lowercase letters into pattern and make two strings equal, we call matched.
- capital shall match exactly.
- using two pointer matching. if capital does not match, return false.
->>

<<-1055	Shortest Way to Form String    		57.2%	Medium	
given string src and target, return the min number of subsequence used to reach target string.
two pointer, with one using mod.
->>

<<-1537	Get the Maximum Score    		36.1%	Hard	
two sorted array, a valid path (at the same value you can switch to the other array) return the max path sum.
- path can begin from nums1 or nums2, end at nums1 or nums2.
- choose the max sum branch.
- need to find the common segments.
using merge sort with two pointer. wait until we see identical number. choose the previous max.
->>

## bit manipulation

<<-89	Gray Code    		49.7%	Medium	
gray code is gnerated using i^(i>>1)
->>

<<-137	Single Number II     		53.2%	Medium	
every number appear 3 times except one.
- bits: do bit by bit and sum each bit %3 and that is the bit appear once.
- using true type table to get a counter reset to 0 when goes to 3.
->>

<<-136	Single Number    		66.1%	Easy	
each number appear twice except one. xor.
->>

<<-318	Maximum Product of Word Lengths    		51.8%	Medium	
word[i] and word[j] shares no common letters.
using char as bit and then do bit manipulations.
->>

<<-231	Power of Two    		43.8%	Easy	
n&(n-1)->>
<<-326	Power of Three    		42.0%	Easy	
only have factor 3, max3%n==0
->>

<<-342	Power of Four    		41.4%	Easy	
only 0,2,4,6... is set 1. 
& 0xaaaaaaa to check odd bits if set.
num&(num-1) check if it is not 2^n.
->>

<<-371	Sum of Two Integers    		50.6%	Medium	
cannot use +/-. 
carry a&b, result a^b.
getsum((a&b)<<1,a^b)
->>

<<-393	UTF-8 Validation    		37.8%	Medium	
utf8 can have 1,2,3,4 bytes
one byte: 0xxxxxx
two bytes: 110xxxxx 10xxxxxx
three bytes: 1110xxxx,10xxxxxx,10xxxxxx
four bytes: 11110xxx, 10xxxxxx,10xxxxxx,10xxxxxx
using the unique head when we find it remaing bytes is known.
->>

<<-477	Total Hamming Distance    		50.5%	Medium	
number of bits which is different.
find the total hamming distance between all pairs of numbers given an array.
- brutal force O(N^2)
- 32bits check bit by bit and split into two half n*m
->>

<<-898	Bitwise ORs of Subarrays    		34.0%	Medium	
count number of possible values.
- brutal force using hash to remove duplicates
- optimization: add more number into OR will not decrease number of 1s. we can only use current element to or the previous result and add new results into hashset. O(N).
also we can remove duplicates in the input.
->>

<<-1009	Complement of Base 10 Integer    		61.5%	Easy	->>

<<-1238	Circular Permutation in Binary Representation    		65.4%	Medium	
gray code ans[i]=start^i^(i>>1)
->>

<<-1255	Maximum Score Words Formed by Letters    		69.6%	Hard	
a-z has different score given, and a list of dictionary word, and a list of usable characters.
use the characters to form any dictionary words with max score.
- convert given chars to hashmap.
- using bitmask to represent all combinations and check if they are valid combinations
->>

<<-1310	XOR Queries of a Subarray    		68.9%	Medium	
xor is similar to sum, using prefix xor.
->>

<<-1318	Minimum Flips to Make a OR b Equal to c    		63.5%	Medium	
straightforward.
->>

<<-1371	Find the Longest Substring Containing Vowels in Even Counts    		61.4%	Medium	
the exact count does not matter. each vowel shall be even.
so use bit to indicate aeiou's odd even. then use hashmap to find the same status.
->>

<<-1386	Cinema Seat Allocation    		35.2%	Medium	
n rows, each row has 8 seats in 3,4,3 separated by isle with reserved seats. family of 4 shall sit adjacent. one case with isle is 2 people on each side is consider adjacent.
since the first and last cannot seat family member, so discard them.
using bit + hashmap
->>

<<-1442	Count Triplets That Can Form Two Arrays of Equal XOR    		70.5%	Medium	
find i,j,k, such that A[i]^..A[j-1]==A[j]^...A[k]
prefix xor.
it is equivalent to A[i]^...A[k]=0, and j=i+1 to k-1 with k-i-1 combinations.
using prefix and hashmap, this is to find same xor with region>=3.
->>

<<-1452	People Whose List of Favorite Companies Is Not a Subset of Another List    		54.3%	Medium	
the key is to collect all company and convert company to bit and then each person's list converts to a int or a bit vector.
->>

<<-1486	XOR Operation in an Array    		84.0%	Easy	
bums[i]=start+2*i, return xor of the array.
- brutal force O(n)
- optimization O(1): forget the start, if the length is even, the LSB is 0 otherwise, LSB is 1 and then we right shift one bit. (start+2*i)^(start+2*i+2)
->>

<<-1521	Find a Value of a Mysterious Function Closest to Target    		43.7%	Hard	
func(A,l,r) is the bit AND from A[l] to A[r]. find the func value closest to target.
- bit and will get decreased result, never goes up.
- AND_VAL[i,j]=A[i] & AND_VAL(i+1,j-1] starting at ith element with length j.
- do it from right to left.
- limited number of AND_VAL using hashset to eliminate duplicates.
->>


## data structure focused problems

## stack
- general stack
- monotonic stack, to use decreasing or increasing need ask what element is to stay in the stack.
- recursive stack for syntax parsing.
- stack is very useful and sometimes is also hard!
- string, array can also be used as stack if only using pop_back.

understand why we need use monotonic stack is very important. Among them find next smaller/greater is the base problem.

### general stack operations.
<<-32	Longest Valid Parentheses    		28.9%	Hard	
using stack to store index and eliminate valid pairs.
->>

<<-71	Simplify Path    		33.3%	Medium	
../ go upper ./ current , stack.
->>

<<-150	Evaluate Reverse Polish Notation    		37.2%	Medium	
operator follows the operands. using stack.
->>

<<-402	Remove K Digits    		28.5%	Medium	
remove k digits from the number so that the new number is the smallest.
greedy: from left to right, remove the first peak digit.
1432219->132219->12219->1219->119
using stack.
->>

<<-636	Exclusive Time of Functions    		53.2%	Medium	
single threaded: given string: "id:start:time" and "id:end:time"
using stack: subtract the eliminated time in stack.
->>

<<-735	Asteroid Collision    		43.0%	Medium	
given a list of numbers, negative means going left, positive goes right.
two meet, the less heavier will explode, same size both will explode.
fill out the states after all collision done.
using stack. (abs)
->>

<<-856	Score of Parentheses    		61.8%	Medium	
() score=1, AB score=A+B (A) score=2*A.
simulate the stack.
->>

<<-895	Maximum Frequency Stack    		61.9%	Hard	
push
pop: pop the most frequent element in the stack.
hashmap to record each element's frequency
for each frequency, maintain a stack map<int,stack<int>>
->>

<<-921	Minimum Add to Make Parentheses Valid    		74.4%	Medium	
using stack. right mismatch just record, left mismatch still in the stack.
->>

<<-1249	Minimum Remove to Make Valid Parentheses    		63.2%	Medium	
using stack to store unpaired (, and array to store unpaired ). remove paired ones.
->>

<<-946	Validate Stack Sequences    		63.1%	Medium	
two arrays one is pushed values, one is popped values
simulate the stack.
->>

<<-1209	Remove All Adjacent Duplicates in String II    		57.4%	Medium	
keep in stack the char and repeat times.
->>

<<-1221	Split a String in Balanced Strings    		83.8%	Easy	
similar to parenthesis. using stack or counter. if 0 we found a pair.
->>

<<-1309	Decrypt String from Alphabet to Integer Mapping    		77.3%	Easy	
mapping 1-9 maps to ['a'-'i'],10#-26# maps to ['j'-'z']
backward checking #
->>

<<-1381	Design a Stack With Increment Operation    		75.9%	Medium	
push/pop/maxsize/inc inc the bottom k elements in the stack
using two stack for this
->>

<<-1441	Build an Array With Stack Operations    		69.3%	Easy	
simulate stack.
->>

<<-1597	Build Binary Expression Tree From Infix Expression    		63.6%	Hard	
expression tree: leaf nodes are numbers, inner nodes are +-*/.
infix expression: in order traversal of the binary tree
given an input string with (), produce a expression tree with equal infix (omitting the ())
typical syntax analysis using stack.
using vector to store the node* is better for processing.
->>

<<-1628	Design an Expression Tree With Evaluate Function    		86.0%	Medium	
given an virtual class interface and postfix string, build the tree and evaluate it.
class design and oop and tree evaluations
- build the tree: [4,5,7,2,+,-,*] it is stack form 4*(5-(7+2))
- use a stack to store the generated treenode.
- inherit a class from the virtual class.
subject: tree, stack, OOP, polymorphism
->>

### monotonic stack

<<-42	Trapping Rain Water    		50.1%	Hard	
monotonic stack: decreasing, when we see a larger one we get the water.
->>

<<-334	Increasing Triplet Subsequence    		39.9%	Medium	
check if there exist i<j<k and A[i]<A[j]<A[k]
similar to 132 pattern. s1 to be the left min.
->>

<<-456	132 Pattern    		30.5%	Medium	
i<j<k and nums[i]<nums[k]<nums[j]
stack: from right to left, keep the stack the largest in the right, so when we see current val > stack top, we assign s3=stack top. then we see a number < s3, we know curr<s3.
->>

<<-496	Next Greater Element I    		64.7%	Easy	
given two array and find each number of A's greater element in B. A is a subset of B.
monotonic stack: 
->>

<<-503	Next Greater Element II    		57.5%	Medium	
the array is circular.
using stack, extending the array. monotonic stack.
->>

<<-739	Daily Temperatures    		64.1%	Medium	
given a list of temperature each day, return number of days to wait for a warmer day
stack find next greater element
->>

<<-901	Online Stock Span    		60.9%	Medium	
stock span means from today previous stock prices are consecutively <= today's price
stack for previous larger. Montonic stack.
->>

<<-907	Sum of Subarray Minimums    		33.3%	Medium	
sum of all subarray min.
each element can be min. using stack find previous larger and next larger to get the range. if left length is L: we can have L*A[i]
->>


<<-1003	Check If Word Is Valid After Substitutions    		55.6%	Medium	
given a string s, with only 'a','b','c'. check if you can build t by adding "abc" any times.
equivalent: eliminate "abc" and make it empty.
using stack or deque is more convenient since it needs check two chars.
->>

<<-1019	Next Greater Node In Linked List    		58.2%	Medium	->>

<<-1028	Recover a Tree From Preorder Traversal    		70.3%	Hard	
preorder traversal to get the serialization string, with (dashes), number of dashes=depth+value.
if only one child, it is the left child.
iterative stack: store node and depth. When top depth larger than current, pop it.
add a - at the end for convenience.
->>

<<-1063	Number of Valid Subarrays    		71.7%	Hard	
leftmost element in the subarray <= other elements in the subarray.
so the element shall be the min and we are looking for next smaller for each element.
monotonic increasing stack.
->>

<<-1081	Smallest Subsequence of Distinct Characters    		53.3%	Medium	
given a string, find the smallest subsequence with distinct characters.
using monotonic increasing stack, when we see a smaller char behind we pop from stack (if the char has no more behind we cannot pop it)
->>

<<-316	Remove Duplicate Letters    		38.5%	Medium
remove duplicates so that each letter only appear once.
return the lexi smallest string.
- stack + hashmap
- if current char < back, and back char still have in right, then replace it with current char
for exampe "acbacdcbc"
see 'a' ->"a"
see 'c' ->"ac"
see 'b' ->"ab",
see 'a' ->"aa" ->"a"
see 'c' ->"ac"
see "d" ->"acd"
see "c" there is no 'd' in the right ->"acd"
see "b" ->"acdb"
see "c" ->"acdb" 
need a counter map, a visited array.
->>

<<-1124	Longest Well-Performing Interval    		33.0%	Medium	
change 9 to 1 and <8 to -1, then we are looking for the longest subarray sum >0.
subarray sum can be done using prefix sum. 
can generalize to subarry sum>target.
- using ordered_map, we need check all smaller than current one. complexity would be O(nlogn)
- using stack. keep the new lowest prefix into stack, and then from right, while current is larger than top, pop it and get the answer. 
- using hashmap to record the prefix sum vs first appear index. if prefix>0, then the whole prefix is the valid subarray. if prefix<0, if we have not seen it, record it, otherwise we check score-1 (since the score start from 0, and -1 must appear than -2. (pretty tricky)
->>

<<-1130	Minimum Cost Tree From Leaf Values    		67.1%	Medium	
given an array of integer, consider all binary tree:
the leaf nodes are the array elements in order, the inner nodes is the product of left and right max leaf value. return the smallest sum of non-leaf nodes.
[6,2,4] we first combine 2 and 4, which get a sum of 32.
- a number can only combine its nieghboring, cannot cross!
- sorted order will be simple.
Once we are clear about that, we can use greedy approach.
every leaf shall be combined with its left or right. For each element it shall combine to the next larger number to get rid of itself.
So the problem is equivalent to find next larger integer in its left or right.
And this can be solved using stack.
->>

<<-85	Maximal Rectangle    		38.7%	Hard	
row by row check histogram.
->>

<<-84	Largest Rectangle in Histogram    		36.0%	Hard	
find the largest rectange (think sorted, we need check each one as the lowest bar)
idea: area is min(height(i..j)*(j-i+1))
thinking process: 
- there are only n possible candidates since the largest rect has to be one of the height.
- if we use bar[i] as the rect height, we need to make sure left and right are both higher than it
this leads us to find the next larger element on left and right
- using stack to reach O(n): maintain a monotonic increasing stack, when we see a smaller bar, we know we get the next greater bar in the stack.
for example: [6, 7, 5, 2, 4, 5, 9, 3] 
we add a 0 to each end:
stack initial [0]
add 6: [0,6]
add 7: [0,6,7]
add 5: now we know next right is 7. pop 7, get area 7
		pop 6, get area 6*2
		stack [0,5]
the idea:each bar can be the height of some rect, and we are looking for previous smaller and next smaller. so element increasing is fine, using monotonic increasing stack.
->>

<<-1475	Final Prices With a Special Discount in a Shop    		75.0%	Easy	
if you buy prices[i] you can get discount at price[j] where j>i and prices[j]<prices[i], j shall be minimized.
stack: find next smaller.
->>

### recursive stack

<<-439	Ternary Expression Parser    		56.3%	Medium	
T?2:3 F?1:T?4:5
recursive stack 
->>

<<-224	Basic Calculator    		37.7%	Hard	
expression include +-*/() and space.
recursive stack.
->>

<<-227	Basic Calculator II    		37.6%	Medium	
expression only include numbers +-*/
using stack.
->>

<<-385	Mini Parser    		34.2%	Medium	
deserialize the nested integer with string.
using stringstream to read
- integer -> nestedInteger object
- see '[' then it is a list, stop at ']'
pretty trick.
->>

<<-394	Decode String    		51.8%	Medium	
k[encode_str] the str is repeated k times.
recursive stack.
->>

<<-536	Construct Binary Tree from String    		49.7%	Medium	
node val followed by one () or two () for the children.
build the tree recursively. recusive stack.
->>

<<-591	Tag Validator    		34.5%	Hard	
<tagnam>Tag_content</tagname> is valid if tag name and tag_content are both valid.
tag name only contains 1-9 upper case letters
tag content may contain other valid closed tags cdata except unmatched <, start and end tag,
recursive stack.
->>

<<-640	Solve the Equation    		42.5%	Medium	
equation contains number +-= and x and its coefficient.
- collect x's coefficient and number sum.
- stack processing.
->>

<<-726	Number of Atoms    		50.9%	Hard	
atom name: Capital+0 or more lowercase.
include ()
->>

<<-736	Parse Lisp Expression    		49.8%	Hard	
string parser using recursive stack.
operator: add,mult,let
variables
()
->>

<<-761	Special Binary String    		58.4%	Hard	
speical binary string:
- number of 0s=number of 1s
- every prefix has at least as many 1s as 0s. (num1s>=num0s)
a move: choose two consecutive special substring and swap.
what is the largest string possible.
similar to valid parentheses strings: we need swap valid parentheses substr to make it larger.
deeper string shall swap to forward.
approach: arrange the children first to largest and then arrange root level.
for example. ()(()), () and (()) is the same level, (()) has two levels.
same level: we swap the deeper one forward.
- when we see a 1 we process it recursively, sort the same level strings and reorder.
->>

<<-772	Basic Calculator III    		42.5%	Hard	
evaluate the expression including +-*/()
recusive stack.
->>

<<-1087	Brace Expansion    		63.0%	Medium	->>
<<-1096	Brace Expansion II    		62.3%	Hard	->>

<<-1106	Parsing A Boolean Expression    		58.9%	Hard	
recursive stack
->>

<<-1190	Reverse Substrings Between Each Pair of Parentheses    		63.8%	Medium	
recursive stack. syntax parser.
->>

## queue & deque
- monotonic deque for sliding window min/max/median..
- queue is mainly for bfs.

<<-158	Read N Characters Given Read4 II - Call multiple times    		35.5%	Hard	
using a deque to store the read data.
->>

<<-157	Read N Characters Given Read4    		36.4%	Easy	->>

<<-649	Dota2 Senate    		39.2%	Medium	
two parties, randiant and dire. Voting is a round based procedure:
- ban one's right, a senator can make another to lose his rights and all the following rounds
- announce victory: if the senator find all having rights to vote are from same party.
The round-based procedure starts from the first senator to the last senator in the given order. This procedure will last until the end of voting. All the senators who have lost their rights will be skipped during the procedure.
strategy game: greedy: ban the senator from other party next to him, then move himself to the end.
use two queues (use the index)
->>

<<-950	Reveal Cards In Increasing Order    		75.0%	Medium	
given a list of cards, the following operation
take the top one, move the second one to the bottom
need to form a list in increasing order
return the original order of the cards
do it reversely. from sorted order get to the original array by reversing the work.
using deque.
->>

<<-1700 number of students unable to eat lunch
each student has a prefer of lunch, if current lunch is not preferred, back to the queue end.
just simulate the deque
->>

<<-239	Sliding Window Maximum    		44.1%	Hard	
deque monotonic.
->>

<<-862	Shortest Subarray with Sum at Least K    		24.9%	Hard	
find the length.
- prefix sum and record its index and sorted in map then binary search. O(nlogn)
- use deque to get O(N). monotonic decreasing deque (larger and old value are discarded)
->>


## heap: priority-queue, set, map
heap is tree based. priority_queue uses array to represent a tree structure (complete tree).
priority_queue can only access the top, however set/map can access any elements.

<<-295	Find Median from Data Stream    		45.8%	Hard	
keep left and right, left max heap, right min heap.
left.size-right.size()<=1
->>

<<-347	Top K Frequent Elements    		61.9%	Medium	->>

<<-358	Rearrange String k Distance Apart    		35.4%	Hard	
rearrange the string so that identical char are at least k distance.
hashmap to get the frequency
then use heap to arrange highest freq char first.
arrange k chars, (pop) and then update the frequency and put back to heap.
->>

<<-373	Find K Pairs with Smallest Sums    		37.2%	Medium	
two sorted array, find the pair k smallest (u,v) u from A, and v from B, sum sorted.
using heap to store the first row or column.
->>

<<-378	Kth Smallest Element in a Sorted Matrix    		55.4%	Medium	
rows and columns are sorted.
- priority_queue to put neighboring into pq. (note we may add one element several times) not efficient
- priority_queue add first column or row into pq. this we can avoid adding duplicate.
- binary search: count number of less than target.
->>

<<-407	Trapping Rain Water II    		43.3%	Hard	
idea: from the bounary and try the smallest height first.
check its 4 neighbors, hold water max(maxh-h[x,y],0)
->>

<<-480	Sliding Window Median    		38.1%	Hard	
fixed window size K. 
using two heap, one max heap one min heap.
the idea: store to left and right, hold the old value in heap if they are not on the top, using a hashmap to record those old values in heap (why we do not use index, since the old values having different index is the same, we only 
need to expel the value, do not care its index)
then we need carefully balance the two heap, finding out the old value in which side.
- using multiset have more convenience since we can eliminate the old value easily in O(logK), and then find the median also in log(K)
first build the multiset using first k elements, and pointer to the k/2
add a new value, if current value < mid, then our pointer--, else no change
delete old values. this is more concise and clear.
many occassions set is more convenient than pq.
->>

<<-632	Smallest Range Covering Elements from K Lists    		53.3%	Hard	
priority_queue do merge sort.
->>

<<-692	Top K Frequent Words    		52.6%	Medium	
sort frequency form highest to lowest, tie, smaller string first.
pq.
->>

<<-767	Reorganize String    		49.5%	Medium	
no adjacent char is the same.
use hashmap to record the frequency and then use pq.
->>

<<-786	K-th Smallest Prime Fraction    		41.4%	Hard	
given a list of prime numbers in sorted order, distinct. i<j and A[i]/A[j] find the kth pair.
sorted up triangle, sorted in row and col.
similar to sorted matrix, using heap.
->>

<<-973	K Closest Points to Origin    		64.3%	Medium	->>

<<-1054	Distant Barcodes    		44.0%	Medium	
rearrange so that no adjacent are the same.
priority_queue
->>

<<-1057	Campus Bikes    		57.6%	Medium	
assign the shortest distance pair, tie assign to the smallest worker index.
priority_queue
->>

<<-1090	Largest Values From Labels    		60.0%	Medium	
confusing question: values[i] is the value, labels[i] is the label for ith element.
you choose with restriction: each label number <=Limit, total number of selection <=num_wanted.
return the max sum.
priority_queue to store each value with same label<=limit, and then take from bigger to smaller.
->>

<<-1353	Maximum Number of Events That Can Be Attended    		29.7%	Medium	
a list of event [start,end], you can attend an event each day. 
- why we cannot use event start/end map? we are not asking the simultaneously max.
- greedy approach: for those events we can attend at today, we shall take the meeting ending earlier. This hints us using the priority-queue
- sort events by start time
- add the events with start time <=today into pq.
- pop the event with the earliest end time.
->>

<<-1354	Construct Target Array With Multiple Sums    		31.3%	Hard	
given a target array and one array of all 1s. You can repeat the operations:
select an index and change it to the sum of the array.
check if you can reach target array.
- equivalent to go from target to all 1s.
- max element is the sum of previous elements t=sum(A') also the max, s=sum(A), s-t=t-A[i],-->A[i]=2*t-s
so approach is to put the target array into pq, and get the sum s, pop the max and replace with 2*t-s and update s.
- we may encouter one max repeat many times and get TLE. actually this is a regular case, (2s-t)%(s-t)
pretty hard. 

For example, target = [max, a1, a2]. Here is the standard backtracking:

Subtract the largest with the rest of the array, we have the previous array target = [max-(a1+a2), a1, a2].
But the value of max is so large, that max-(a1+a2) is still the largest number in the array.
So we continue to subtract the largest, and we have a new previous array target = [max-2*(a1+a2), a1, a2].
But the value of max is so large, that max-2*(a1+a2) is still the largest number in the array.
Repeat 1-4...

After n iterations, we have a new array target = [max-n*(a1+a2), a1, a2], where max-n*(a1+a2) is not the largest any more.

Can we accelerate the process?
Yes.
We have max-n*(a1+a2) = max % (a1+a2). That is how the % works.
->>

<<-1383	Maximum Performance of a Team    		34.0%	Hard	
given n people with speed and efficiency. return the max performance with k engineers.
performance=sum(speed)*min(efficiency)
-priority_queue: sort descending with efficiency, and put into heap sorted with speed. Add a new worker which is the min efficiency, trade with the min speed in heap (efficiency decrease, sum speed increase)
->>

<<-1405	Longest Happy String    		51.7%	Medium	
string consists of abc only. happy string does not have aaa,bbb,ccc substrings.
given 3 numbers i,j,k, the string has at most i 'a's and j 'b's and k 'c's. return the longest happy string.
priority_queue to arrange the most frequent one first.
->>
<<-1705. Max number of eaten apple
on each day, given apple to grow and number days to rot, eat one apple a day. return max number of apples to eat.
if we use the event start/end will be very complicated since we have to eat one a day.
but using greedy approach: to eat the apple the most close to rot.
->>

<<-1438	Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit    		43.5%	Medium	
equivalent to find the max and min difference <=limit. 
using deque to find min and max.
->>

<<-1439	Find the Kth Smallest Sum of a Matrix With Sorted Rows    		60.0%	Hard	
choose one element from each row and get the kth smallest sum.
- approach 1: priority_queue, push neighboring elements if not seen, try smallest first.
- binary search: we know the min is the first col, max is the last column. given a mid, we count the number of combinations <=mid.
->>

<<-1499	Max Value of Equation    		44.9%	Hard	
given a list of points on 2d plane, sorted by x. 
given an integer k, find the max value of yi+yj+|xi-xj| where |xi-xj|<=k.
- sorted using x, i<j, then yi+yj+xj-xi or xj+yj+yi-xi
using priority_queue to store y-x and x, pop all those out of k range data.
->>

<<-162. furthest building you can reach
problem: using ladder and stones to climb ladders.
approach:
- binary search
- heap: try using stones or ladders first, when one is used up, replace the max stone for a ladder or a ladder for min stones.
->>

## hashset, hashmap
hashmap is a generallized array allowing random access. It is often used to reduce complexity.

<<-3	Longest Substring Without Repeating Characters    		30.9%	Medium	
no need use hashmap, hashset is fine, if contains current char, just shrink from left side.
->>

<<-30	Substring with Concatenation of All Words    		25.8%	Hard	
dict words are the same size. 
fixed window size and 
->>

<<-49	Group Anagrams    		58.2%	Medium	->>
<<-133	Clone Graph    		37.5%	Medium	->>

<<-138	Copy List with Random Pointer    		38.5%	Medium	
hashmap node vs node.
->>

<<-146	LRU Cache    		34.6%	Medium	
least recently used.
list, hashmap, list iterator hashmap.
->>

<<-149	Max Points on a Line    		17.1%	Hard	
using gcd(dx,dy) as the key. but need to take care of duplicated points.
->>
<<-220	Contains Duplicate III    		21.3%	Medium	
abs(nums[i]-nums[j])<=t and abs(i-j)<=k.
to keep index diff<=k, we can use sliding window to pop out of window elements.
bucket sort + also expel the old values out of window.
check values only in the current bin and +1 or -1 bin. (bucket size=t+1 so that same bucket will not diff >n.)
->>
<<-219	Contains Duplicate II    		38.3%	Easy	
num[i]==num[j] with abs(i-j)<=k. hashmap to record value vs index.
->>
<<-217	Contains Duplicate    		56.3%	Easy	
check if there exists duplicate. shrink to set.
->>

<<-249	Group Shifted Strings    		56.9%	Medium	
group all strings with same shifted sequence (shift each char by the same amount)
hashmap: change the first char to a and then other keys 
->>

<<-288	Unique Word Abbreviation    		22.5%	Medium	
the abbreviation: first char+number+last char. 
two char string: no change.
- given a word list (dictionary)
- abbreviation shall not equal to word abbreviation in dictionary
- if word is in dictionary, the abbreviation shall be the same.
->>

<<-299	Bulls and Cows    		44.0%	Medium	
given secret string and guess string.
number of bulls: digits in the correct position
number of cows: digits in guess but in wrong position
hashmap: get secret hashmap does not include bulls.
->>

<<-325	Maximum Size Subarray Sum Equals k    		47.0%	Medium	
prefix sum and using hashmap. only need to store the first index.
->>

<<-353	Design Snake Game    		34.9%	Medium	
given a grid. the snake is initially at (0,0) with length=1.
given an array of food at (ri,ci). snake eat the food, get one point and increase length by 1.
the food appears one by one, will appear after the one is eaten.
the game is over if it cannot move.
SnakeGame(w,h,food)
move(direction): return the score.
snake grow on the tail, use deque represent the snake.
use hashset to store the snake's body position.
->>

<<-355	Design Twitter    		30.8%	Medium	
postTweet
getNewsFeed: retrieve the 10 most recent tweet ids in the user's news feed. ordered from most recent to least recent.
follow
unfollow.
shall maintain:
followed tweetIDs for each user, hashmap user vs list of followed ids.
postTreet: each user shall maintain a list of tweets he posted user vs list of treets
getNewFeed: get treets from himself and followed and get the top 10.
keep an array to store the most recent tweets. 
we can go to each followed id and get the most recent 10 posts and put into heap. expel the old posts.

store the tweet: can also use link-list, it is easy to store and only need the head pointer to access.
->>

<<-359	Logger Rate Limiter    		71.5%	Easy	
message with time stamp comes in chrono order. Each unique message should only be printed at most every 10 seconds.
hashmap message vs next timestamp to print.
->>

<<-379	Design Phone Directory    		47.4%	Medium	
assign, recycle, check if number 
hashmap using used and unused.
->>

<<-381	Insert Delete GetRandom O(1) - Duplicates allowed    		34.6%	Hard	
using vector + hashmap value vs index.
delete just swap to back and pop_back.
->>
<<-380	Insert Delete GetRandom O(1)    		48.2%	Medium	
no duplicates
->>

<<-432	All O(1) Data Structure    		32.9%	Hard	
inc(key): insert key with 1 or add existing key by 1
dec(key): if key val=1, remove it, otherwise decrease it by 1, does not exist nothing
GetMaxKey()
GetMinKey()
inc/dec using hashmap in O(1)
GetMax and GetMin. in O(1):
we can create a hashmap: value vs list of keys (linked list)
to get O(1) for linked-list we needs use hashmap iterator.
this creates a 2d matrix similar structure.
->>

<<-454	4Sum II    		53.8%	Medium	
give 4 arrays and find A[i]+B[j]+C[k]+D[l]=0
first do A+B and then C+D, then do A+B vs C+D using hashmap two sum.
->>

<<-460	LFU Cache    		35.1%	Hard	
LFUCache, get(key), put(key,val)
least recently used key will be evicted.
hashmap: key vs value + freq.
hashmap: key to list iterator 
hashmap: freq vs key list.
get: freq++, update the list node in freq vs key list.
put: modify, add new node. 
->>

<<-523	Continuous Subarray Sum    		24.6%	Medium	
check if there is a subarray sum=n*K, n could be any positive integer. with subarray length >=2.
pre[i]-pre[j]=n*K equivalent to: pre[i]%k=pre[j]%k
->>

<<-532	K-diff Pairs in an Array    		34.4%	Medium	
return number of unique pairs with |A[i]-A[j]|=k i!=j.
pair (A[i],A[j]) different index but duplicate value is not unique.
- sort and then binary search (duplicate skipped) O(nlogn)
- hashmap O(N): k==0 and k>0
->>

<<-535	Encode and Decode TinyURL    		80.5%	Medium	
long url convert to 6 char length tiny url.
build a hash from long url to tiny url.
assign a unique id to url, and then use the id to build a tiny url. each char from 62 chars, up to 62^6
->>

<<-560	Subarray Sum Equals K    		43.9%	Medium	
number of subarrays with sum==k. hashmap + prefix sum.
->>

<<-567	Permutation in String    		44.5%	Medium	
check if s2 contains the permutation of s1 as a substring.
fixed window sliding, + hashmap.
->>

<<-594	Longest Harmonious Subsequence    		47.4%	Easy	
harmonious array: the max min difference is 1.
hashmap and then converts to sorted array. or find +1/-1 (use it for ending element)
->>

<<-697	Degree of an Array    		54.2%	Easy	
max frequency is the degree. find the smallest subarray length that has the same degree.
keep the start and ending index for each element. HashMap
->>

<<-874	Walking Robot Simulation    		36.5%	Easy	
robot faces north first, -2 turn left 90 degree, -1 turn right 90, 1-9 take number of steps along direction.
with obstacles list. 
return the max distance robot reaches from the origin.
simulate the movements
->>

<<-884	Uncommon Words from Two Sentences    		63.8%	Easy	
use hashmap to record if it appears in both.
->>

<<-916	Word Subsets    		48.1%	Medium	
given two array of words A and B. All character (including multiplicity) in B appear in a word of A, then a is called universal
note: if B='["lo","eo"] it only asks there is lo and eo, but not require 2 'o's.
so we are using the max frequency in each word of B.
then check against each word in A.
->>

<<-930	Binary Subarrays With Sum    		43.8%	Medium	
give 01 array and return number of subarray sum=target.
prefix sum + hashmap (prefix occurrence).
->>

<<-937	Reorder Data in Log Files    		54.3%	Easy	->>

<<-953	Verifying an Alien Dictionary    		53.0%	Easy	
given alien letters in sorted order, check given list of words, check if each word is sorted.
map to normal order and compare.
->>

<<-957	Prison Cells After N Days    		40.3%	Medium	
use hashmap to record status and skip cycles.
->>

<<-966	Vowel Spellchecker    		47.6%	Medium	
capitalization: ignore case
vowel errors: ignore vowels
hashmap.
->>

<<-974	Subarray Sums Divisible by K    		49.9%	Medium	->>

<<-981	Time Based Key-Value Store    		53.6%	Medium	
hashmap
->>

<<-1001	Grid Illumination    		36.3%	Hard	
given nxn matrix, a light at [i,j] can illuminate the row, column, diagonal.
given a list of queries, check if the cell is illuminated. after query turn off the all the lamp in the 3x3 submatrix.
hashamp: row, col, diag, diag1. diagonal: rr-cc as key, diagonal 1: rr+cc as key.
->>

<<-1010	Pairs of Songs With Total Durations Divisible by 60    		47.9%	Easy	->>

<<-1072	Flip Columns For Maximum Number of Equal Rows    		61.2%	Medium	
flip columns will not change the equal rows, they are still equal. 
so it is equivalent to find the max equal rows.
using hashmap.
->>

<<-1146	Snapshot Array    		36.9%	Medium	->>

<<-1152	Analyze User Website Visit Pattern    		43.2%	Medium	
this is quite confusing.
->>

<<-1166	Design File System    		57.8%	Medium	
create a path: if path already exist, or if parent path does not exist, return 0;
hashmap.
->>

<<-1169	Invalid Transactions    		31.5%	Medium	->>

<<-1171	Remove Zero Sum Consecutive Nodes from Linked List    		41.4%	Medium	
prefix sum and hashmap to find 0 sum region.
->>

<<-1172	Dinner Plate Stacks    		38.0%	Hard	
infinite stacks on a row. each stack with max capacity.
push(v): add v on the leftmost stack not full
pop() pop the top on the rightmost non-empty stack
popAtStack: pop from the given stack.
- maintain a list of available stacks (sorted)
- maintain a non-empty map (sorted)
similar to 2d matrix, move elements between two maps.
->>

<<-1181	Before and After Puzzle    		44.4%	Medium	
given a list of sentences, connect a pair A[i] the last word is the first word of A[j], return the list in sorted order.
hashmap to map the last word vs the sentence.
->>

<<-1224	Maximum Equal Frequency    		34.2%	Hard	
return the max prefix length so that we can remove one and all elements has the same frequency.
hashmap: check all prefix and check if it is valid.
->>

<<-1329	Sort the Matrix Diagonally    		79.3%	Medium	
sorted in each diagonally. using hashmap i+j as the key.
->>

<<-1338	Reduce Array Size to The Half    		66.8%	Medium	
remove all occurences of a number and reduce the array size <=n/2.
remove the most frequent elements first. hashmap.
->>

<<-1347	Minimum Number of Steps to Make Two Strings Anagram    		75.3%	Medium	
given s and t. You can choose a char in t and replace it with other char. return min steps to make s and t anagram.
- to make histogram the same
- hist(t)-hist(s): all the positives has to be changed to 0.->sum of positives.
->>

<<-1348	Tweet Counts Per Frequency    		31.5%	Medium	
get treet frequency per second, per minute, per hour.
hashmap<string,multiset<int>> to support one second multiple tweets.
->>

<<-1370	Increasing Decreasing String    		76.2%	Easy	
just do the simulation according to the problem.
->>

<<-1395	Count Number of Teams    		82.1%	Medium	
choose a triplet sorted or reversely sorted (no equal).
using a left set and right set, then use each number as the middle, and update left and right.
->>

<<-1396	Design Underground System    		67.5%	Medium	
checkIn(int id, string stationName, int t)
checkOut(int id, string stationName, int t)
getAverageTime(string startStation, string endStation): average time from start to end.
collect all customer with start and end the same. 
hashmap. You can also build route map so that we do not need to find the route from other maps.
->>

<<-1418	Display Table of Food Orders in a Restaurant    		67.6%	Medium	
make a display table according to customer orders.
->>

<<-1426	Counting Elements    		58.9%	Easy	
find all x if x+1 appears.
using hashmap find x+1 or x-1.
->>

<<-1429	First Unique Number    		48.4%	Medium	
a queue keep growing
maintain a unique set and seen set and a vector to represent the queue.
->>

<<-1460	Make Two Arrays Equal by Reversing Sub-arrays    		72.5%	Easy	
You can do many times.
this is check if we can sort to other array.
their hashmap shall be the same.
->>

<<-1477	Find Two Non-overlapping Sub-arrays Each With Target Sum    		33.4%	Medium	
minimze the sum of two subarray length.
- how to find subarray with sum==target, using hashmap.
- get the intervals, and find the min total sum of two non-overlapped region
or we don't need to save the intervals, but keep the current index in the hashmap.
use prefix and suffix to find the region from left and right and then get the  min
->>

<<-1496	Path Crossing    		55.7%	Easy	
given a string of NSEW,N north, S: south, E: east, W: west. Check if it cross itself. initial at (0,0)
hashset: each location store as string.
->>

<<-1487	Making File Names Unique    		30.0%	Medium	
create file, if the file name is seen add suffix (i) i the smallest positive integer not used.
hashmap: record the file name and its suffix number. if it is 0, there is no suffix. maintain a list
->>

<<-1488	Avoid Flood in The City    		24.9%	Medium	
rains[i] means it will rain on ith lake. if the lake is empty, it will full. otherwise it will flood.
rains[i]=0 means no rain, >0 mean there is rain.
when rain[i]==0 you can choose to dry a lake. ans[i] is the lake you choose to dry.
rain[i]>0 then ans[i]=-1. You cannot do anything.
if impossible to avoid flood, return empty array.
greedy approach: [1,2,0,0,2,1] you need to dry 2 and 1 in the two sunny days. we record the full lakes in hashmap, and when we see a rain again on the lake, we check if there is a sunny day after it.
- using hashmap to record lakes to be rained and non-rainned lake
- using binary search to find the last index >= rainny day.
->>

<<-1500	Design a File Sharing System    		45.1%	Medium	
file sharing a large file with chunks from 1 to m. 
when user join the system, a unique id is assigned. when the user leaves, the id can be reused.
when user request a chunk id, the system shall return a list of user who own the chunk. If user get a non-empty list of users, he is granted the chunk.
FileSharing(int m)
join(vector<int> ownedChunk)
leave(int userID)
request(userID,chunkID)
hashmap problem: 
- userID management: need to delete, add, sorted array find first missing integer. or use used, free. free can use pq or set.
- chunk vs user ownedChunk, and chunk vs user.
- note when user request id, then he owns it, we shall update the two hashmaps
->>

<<-1512	Number of Good Pairs    		88.0%	Easy	
find pair a[i]==a[j]. classical hashmap.
->>

<<-1525	Number of Good Ways to Split a String    		67.2%	Medium	
split is good if the two parts have the same number of distinct letters
return the number of good splits.
know the number of unique letters, and counting left side unique letters and derive if it is good.
hashmap.
->>

<<-1577	Number of Ways Where Square of Number Is Equal to Product of Two Numbers    		37.0%	Medium	
given two integer arrays nums1,nums2. return number of triplets if:
nums1[i]^2==nums2[j]*nums2[k] or nums2[i]^2=nums1[j]*nums1[k].
- two similar problem, solve one
- using hashmap to store nums2 occurence.
- i*j=t^2 i==j and i!=j two cases.
->>

<<-1567	Maximum Length of Subarray With Positive Product    		36.0%	Medium	
- using hashmap
- see 0 we stop old one and start new one
- keep track of odd/even status of negative (to keep positive, it shall be the same). we only need keep the first 0/1 position
->>

<<-1590	Make Sum Divisible by P    		27.2%	Medium	
remove the min length subarray to make the sum divisible by p.
hashmap using prefix sum. we record the prefix sum with index, and same prefix value.
tsum%p=x, then the subarray sum shall have sum%p=x.
looking for prefix-x.
->>


<<-1604	Alert Using Same Key-Card Three or More Times in a One Hour Period    		41.4%	Medium	
entry using keycard, entry will record name and time.
return list of name entry 3 or more times in one hour in a single day.
idea: hashmap to record entry time for each person. Then for each person we check one hour period entry times. (this can be approached using deque)
->>

## tree
why tree is important? tree is base for a lot of data structures with O(n) or O(logn) complexity.
- recursive
- iterative
- inorder traversal
- preorder traversal
- postorder traversal
- algorithm in array applied in tree.
- tree is a special graph.
- binary tree, BST, n-ary tree

<<-112	Path Sum    		41.8%	Easy	
check if there is a path sum=target.
->>

<<-113	Path Sum II    		48.0%	Medium	
return all root to leaf path with sum=target.
dfs or backtracking.
->>

<<-111	Minimum Depth of Binary Tree    		38.8%	Easy	->>
<<-129	Sum Root to Leaf Numbers    		50.1%	Medium	->>
<<-124	Binary Tree Maximum Path Sum    		35.0%	Hard	
a path may pass or not pass the root.
- get the subtree max path sum, left and right including root max path sum
- the including root left/right path sum is similar to 1d array max subarray sum. (connecting or discard)
- post order.
->>

<<-117	Populating Next Right Pointers in Each Node II    		40.3%	Medium	->>
<<-116	Populating Next Right Pointers in Each Node    		47.7%	Medium	->>
<<-114	Flatten Binary Tree to Linked List    		50.8%	Medium	
right root left order 
->>
<<-110	Balanced Binary Tree    		44.0%	Easy	->>
<<-109	Convert Sorted List to Binary Search Tree    		49.2%	Medium	
similar to array find the mid and divide into two parts.
->>
<<-108	Convert Sorted Array to Binary Search Tree    		59.3%	Easy	->>
<<-107	Binary Tree Level Order Traversal II    		54.4%	Easy	->>
<<-106	Construct Binary Tree from Inorder and Postorder Traversal    		48.5%	Medium	->>
<<-105	Construct Binary Tree from Preorder and Inorder Traversal    		50.5%	Medium	->>
<<-104	Maximum Depth of Binary Tree    		66.9%	Easy	->>
<<-103	Binary Tree Zigzag Level Order Traversal    		49.3%	Medium	->>
<<-102	Binary Tree Level Order Traversal    		55.7%	Medium	->>
<<-101	Symmetric Tree    		47.6%	Easy	->>
<<-100	Same Tree    		53.8%	Easy	->>
<<-99	Recover Binary Search Tree    		41.7%	Hard	
two nodes of the BST was swapped. 
inorder traversal, similar to 1d array, it will create one pair or two pairs of disorder.
->>
<<-98	Validate Binary Search Tree    		28.2%	Medium	
use min/max recursively. or O(N) in order traversal.
->>
<<-96	Unique Binary Search Trees    		53.8%	Medium	
return number of unique BST using nodes 1 to n.
dp: dp[i] represent the number of unique BST for i nodes. dp[i]+=dp[j-1]*dp[i-j]
->>

<<-94	Binary Tree Inorder Traversal    		64.8%	Medium	->>

<<-145	Binary Tree Postorder Traversal    		56.4%	Medium	->>
<<-144	Binary Tree Preorder Traversal    		56.7%	Medium	->>

<<-156	Binary Tree Upside Down    		55.6%	Medium	
- left child becomes the new root
- root becomes the new right child
- right becomes the new left child
goes left and get the leftmost leaf node, and get the new root.
this is similar to reverse linked list. not very intuitive.
root->left becomes the new root.
root itself becomes the right child root->left->right=root
original right becomes the left child root->left->left=root->right.
->>

<<-199	Binary Tree Right Side View    		55.2%	Medium	->>
<<-222	Count Complete Tree Nodes    		48.1%	Medium	
1+2+4+...->2^(h+1)-1 check depth diff.
->>

<<-226	Invert Binary Tree    		66.1%	Easy	
swap left right.
->>

<<-230	Kth Smallest Element in a BST    		61.6%	Medium	
- inorder traversal
- binary search (count)
- if the tree is modified often? add a counter to each node.
->>

<<-250	Count Univalue Subtrees    		52.7%	Medium	
post order traversal. 
->>

<<-255	Verify Preorder Sequence in Binary Search Tree    		45.9%	Medium	
binary search to find the upper bound and divide into left and right.
->>

<<-270	Closest Binary Search Tree Value    		49.2%	Easy	
find the value in logn. target is float.
curr_diff=target-root->val, >0 we want to reduce the diff, goes to right.
else goes to left O(h).
->>

<<-272	Closest Binary Search Tree Value II    		51.3%	Hard	
find the k values closest to the target in BST.
O(N): the difference is ia V shape. using a deque to move the window. O(N)
if the tree is balanced: find the closest node and extend left and right (inorder and reverse inorder) 
->>

<<-285	Inorder Successor in BST    		41.7%	Medium	
given a val, find its next greater.
binary search. val<root, goes to left, else goes to right.
->>

<<-297	Serialize and Deserialize Binary Tree    		48.8%	Hard	->>

<<-298	Binary Tree Longest Consecutive Sequence    		47.6%	Medium	
path shall follow parent child, ascending order.
dfs: if root!=parent+1, then reset the length otherwise =parent.length+1
->>

<<-314	Binary Tree Vertical Order Traversal    		46.3%	Medium	
dfs and sort. (with same position, sorted order)
->>

<<-331	Verify Preorder Serialization of a Binary Tree    		40.7%	Medium	
serialization uses '#' for null node. 
each node has two children (counting '#')
- each node has one incoming, 2 outcoming.
- root has zero incoming
- '#' has no outcoming
if we count diff=out-in. the diff will always >=0 and will be 0 at the end. (just similar to valid parenthesis)
->>

<<-333	Largest BST Subtree    		36.7%	Medium	
find the subtree which is a BST with largest number of nodes.
postorder: count the nodes and get the lmax and rmin to make sure it is a bst
O(N) complexity.
->>

<<-173	Binary Search Tree Iterator    		58.2%	Medium	
binary tree inorder traversal iterative approach using stack.
add all left into stack, pop and add all right into stack.
->>

<<-366	Find Leaves of Binary Tree    		71.3%	Medium	
find leaves and then remove, until no nodes left.
postorder: leaf depth=0, and upper level is 1...
->>

<<-427	Construct Quad Tree    		62.0%	Medium	
topleft,topright,bottLeft,bottRight
if the cell has all the same value, then it is a leaf with value
else we need to divide into 4 cells and recursively build them.
->>

<<-429	N-ary Tree Level Order Traversal    		65.9%	Medium	->>
<<-428	Serialize and Deserialize N-ary Tree    		60.6%	Hard	
serialization needs include number of children for each node.
->>

<<-431	Encode N-ary Tree to Binary Tree    		73.9%	Hard	
idea: the first children as the left, all the other children connected as the right.,
decode reverse the process.
->>

<<-437	Path Sum III    		47.7%	Medium	
find number of path with sum=target, the path does not need from root to leaf, but any downpath is good.
prefix sum with hashmap while dfs.
->>

<<-450	Delete Node in a BST    		44.9%	Medium	
if key<root, goes left, else goes right
if key==root, we first sink the root to its right leftmost node, swap it, and delete the left most node.
->>
<<-449	Serialize and Deserialize BST    		53.5%	Medium	
preorder: root left right .. and '# ' for null node.
deserialization: also do preorder.
->>

<<-508	Most Frequent Subtree Sum    		58.6%	Medium	
postorder sum + hashmap
->>

<<-510	Inorder Successor in BST II    		59.6%	Medium	
you will have access to the node only, find the next node (with parent as a pointer)
if there is no right, go to parent
if there is right, go to the leftmost.
->>

<<-513	Find Bottom Left Tree Value    		62.0%	Medium	
find the leftmost value on the bottom level.
dfs when h>maxh record the value.
->>

<<-515	Find Largest Value in Each Tree Row    		61.7%	Medium	
dfs or bfs.
->>

<<-538	Convert BST to Greater Tree    		56.1%	Medium	
node value is changed to the value + sum of all keys greater than it.
right, root, left order.
->>

<<-543	Diameter of Binary Tree    		48.9%	Easy	
longest path, it could be root to leaf or cross the root.
return longest root to leaf in subtree and cross root path.
->>

<<-545	Boundary of Binary Tree    		39.3%	Medium	
this one is hard. left boundary, leaf, right bounday in counter-clockwise.
preorder for the left and leaf 
postorder for the right boundary.
also need judge if the node is left or right boundary
left boundary: lb && !root->left
right boundary: rb && !root->right
->>

<<-549	Binary Tree Longest Consecutive Sequence II    		47.1%	Medium	
longest path with consecutive sequence (either increasing or decreasing).
- get left and right longest path, longest sequence ending with root.
- postorder to get the two results: longest increasing, longest decreasing sequence
{inc,dec}, longest=max(longest,inc+dec-1);
- preorder: need parent information.
->>
<<-558	Logical OR of Two Binary Grids Represented as Quad-Trees    		45.1%	Medium	
quad tree with 4 children: topleft,topright,bottLeft,bottRight, val,isLeaf.
given two quad trees representing two binary matrices, return a quad tree representing the bit OR of the two quad trees.
quad tree is often used for geoinfo system. It is used to get adaptive resolution for a grid cell.
if cell is leaf node, then subcell will all be 1 (if we divide into smaller cells)
- case 1: q1 or q2 is leaf: with all 1, then the q1 or q2 will get a all 1 cell (subcell is merged), with all 0, then use the other node.
- case 2: q1 and q2 are both non-leaf nodes: recursive do it. return a leaf node or non-leaf node.
->>

<<-563	Binary Tree Tilt    		52.2%	Easy	
tilt=absolute difference between left sum and right sum
postorder to get tilt and sum
->>


<<-572	Subtree of Another Tree    		44.3%	Easy	
two recursive problem: isSameTree and isSubtree.
->>

<<-582	Kill Process    		62.0%	Medium	
given proces id and its parent id. kill a process will also kill its all ascendants.
build the tree relation and do dfs from the id.
->>

<<-606	Construct String from Binary Tree    		54.8%	Easy	
output tree as string in preorder, null node represented by (). root(left)(right)
->>

<<-617	Merge Two Binary Trees    		74.8%	Easy	
overlapped add them together. do traverse at the same time.
->>

<<-623	Add One Row to Tree    		50.0%	Medium	
add a row of nodes to depth d. original left subtree is still left, original right is still right.
- bfs until d-1 layers (parent nodes) and then do the swaps.
- dfs: add 1 layer, so that cd==d, then do the swap and return current search.
->>

<<-652	Find Duplicate Subtrees    		51.3%	Medium	
postorder get the subtree's serialization and check hashmap.
->>

<<-655	Print Binary Tree    		55.4%	Medium	
row number = height, col number shall be odd. even the node does not exist you need reserve space for it.
->>
<<-654	Maximum Binary Tree    		80.6%	Medium	
root is the max. find the max divide into left and right.
->>
<<-653	Two Sum IV - Input is a BST    		55.9%	Easy	->>

<<-662	Maximum Width of Binary Tree    		40.2%	Medium	
width is the level width. (according to full tree definition)
dfs with full tree index.
->>

<<-663	Equal Tree Partition    		39.6%	Medium	
divide into two subtree with same sum by removing one edge.
record each subtree's sum in hashmap. one tree must be a subtree, so we just find sum/2.
->>

<<-666	Path Sum IV    		55.3%	Medium	
three digit number represent:
hundreds: depth (1-4)
tens: the position in the level as in full binary tree
unit digit: value.
[113,215,221]: 
113 represent depth=1, first node, value=3
215: depth=2, first node, value=5
221: depth=2, second node, value=1
tree is given in ascending order.
return the sum of all paths from root to leaf.
represent the tree with array (full tree representation). nonexistent with 0.
->>

<<-669	Trim a Binary Search Tree    		63.2%	Easy	
trim so that all value in range [L,R]
postorder.
->>

<<-690	Employee Importance    		57.9%	Easy	
tree structure for postorder add.
->>

<<-687	Longest Univalue Path    		36.8%	Medium	
postorder: get the left and right max depth with value=root->val, and connect via the root. (left+right)
->>

<<-701	Insert into a Binary Search Tree    		76.0%	Medium	
just insert in left or right. (use it as root is much complicated)
->>
<<-700	Search in a Binary Search Tree    		73.3%	Easy	->>

<<-776	Split BST    		56.3%	Medium	
given a bst and a target value, split into two subtree so that one <= target and the other > target.
most of the structure shall be kept.
- split the tree get smaller and bigger subtree
- if root>target, we go left and split into smaller and larger. root->left=larger.
- if root<=target, we go right and split into smaller and larger, root->right=smaller 
tricky.
->>

<<-779	K-th Symbol in Grammar    		38.3%	Medium	
first row 0. The second row will change each 0 to 01 and 1 to 10.
find row N and kth digit.
it forms a complete binary tree. if previous node is 0, then left is 0 and right is 1. if previous node is 1, then left is 1 right is 0. do it recursively.
->>

<<-783	Minimum Distance Between BST Nodes    		53.4%	Easy	
absolute difference, just do inorder traversal.
->>

<<-814	Binary Tree Pruning    		73.3%	Medium	
prune all subtree with all 0.
postorder traversal
->>

<<-834	Sum of Distances in Tree    		44.9%	Hard	
given a tree representing as a list of edges. return the sum of distance from i to all other nodes.
this is mostly about optimization since a lot of paths are revisited.
- brutal force starting from each node and do dfs and get the answer. O(N^2)
- build the adjacency matrix.
- start from root 0, do a postorder traversal to get number of nodes in subtree and sum of distance in substree. count[root]=sum(count[child])+1, dist[root]=sum(dist[child])+sum(count[child])
- when we move root from parent to its child i, res[i]=res[root]-count[i]+N-count[i], do a preorder dfs to update.
tree+dp, hard!!
->>

<<-865	Smallest Subtree with all the Deepest Nodes    		61.4%	Medium	
postorder to get the max depth and the first node with ldepth=rdepth.
->>

<<-872	Leaf-Similar Trees    		64.5%	Easy	->>
<<-889	Construct Binary Tree from Preorder and Postorder Traversal    		66.9%	Medium	->>

<<-894	All Possible Full Binary Trees    		76.5%	Medium	
each node has 0 or 2 children. n nodes.
N must be an odd number, give left/right (1,N-2),(3,N-5)... and do recursive
->>

<<-919	Complete Binary Tree Inserter    		58.1%	Medium	
using array to represent a complete binary tree.
root child index relation i,2i,2i+1
->>

<<-897	Increasing Order Search Tree    		72.5%	Easy	
change tree to a in-order order tree.
using reverse inorder traversal.
root->right=process(root->right,prev)
...
return process(root->left,prev);
level: 3
->>

<<-938	Range Sum of BST    		82.6%	Easy	->>

<<-951	Flip Equivalent Binary Trees    		65.5%	Medium	
check if you flip left and right and get the other tree.
left vs left, right vs right
left vs right, right vs left.
->>

<<-958	Check Completeness of a Binary Tree    		52.3%	Medium	
- bfs level order traversal. If we see a null and still have nodes following, then it is not a complete tree.
- dfs: find the depth. mark the end, h=mh-1. after end is found, if we see h=mh, then it is not a complete tree.
->>

<<-968	Binary Tree Cameras    		38.1%	Hard	
a camera can monitor its adjacent nodes and itself.
return the min number of cameras needed to monitor every node.
idea: greedy approach, we need do postorder traversal and put the camera on the parent node, since it will cover the child and parent and itself
define 3 state： 0： not covered, 1, place a camera, 2: covered.
child 0x or x0, we need add one at parent, 22, we do not place camera but not monitored.
12 or 21->monitored.
->>

<<-971	Flip Binary Tree To Match Preorder Traversal    		46.1%	Medium	
binary tree with n nodes from 1 to n. you can flip a node's left and right subtree to match the preorder traversal.
our goal is to flip the least number of nodes to match them.
if we can do it, return the list of nodes flipped. otherwise return [-1]
preorder: root, left, right.
first compare root, with A[ind] start from 0, fail then directly return.
check left (preorder traversal) if fail, we first check right and then left.
check right.
very good tree problem.
->>

<<-965	Univalued Binary Tree    		67.6%	Easy	->>

<<-979	Distribute Coins in Binary Tree    		69.2%	Medium	
given a binary tree with n nodes and n coins. In each move, you can choose two adjacent nodes and move one coin from one to the other. The coin initial distribution is given as tree node value.
return the min number of operations to make each node one coin.
postorder to get the sum of node value -1 (since we need 1)
the steps from/to root is abs(sum).
->>

<<-987	Vertical Order Traversal of a Binary Tree    		37.2%	Medium	->>

<<-988	Smallest String Starting From Leaf    		46.4%	Medium	
preorder
->>

<<-993	Cousins in Binary Tree    		52.1%	Easy	->>
<<-998	Maximum Binary Tree II    		63.6%	Medium	
max tree: node value is greater than all nodes in its subtree.
build from an array, if A[i] is the max, then A[i] will be the root, and left put in left subtree, right put in right subtree.
Now we already build the tree and we append a val to A, return the tree.
- if val>root, root will be the left.
- else add the val to the right tree.
->>

<<-1008	Construct Binary Search Tree from Preorder Traversal    		78.6%	Medium	->>
<<-1022	Sum of Root To Leaf Binary Numbers    		71.2%	Easy	->>

<<-1026	Maximum Difference Between Node and Ancestor    		68.8%	Medium	
dfs to get the previous min and max, and compare the difference.
->>

<<-1038	Binary Search Tree to Greater Sum Tree    		81.5%	Medium	
change the node's value to value+sum of all keys > val.
right, root, left traversal
->>

<<-1080	Insufficient Nodes in Root to Leaf Paths    		49.7%	Medium	
a node is insufficient if all path passing this node has a sum < limit.
remove all the insufficient nodes.
dfs and subtract the node from limit. Note if the left and right is removed, the root shall also be removed.
->>

<<-1104	Path In Zigzag Labelled Binary Tree    		72.6%	Medium	
full tree, odd row are labelled from left to right, even rows are labelled from right to left.
just use full tree ordering and then convert.
->>

<<-1110	Delete Nodes And Return Forest    		67.4%	Medium	
postorder traversal.
->>

<<-1120	Maximum Average Subtree    		63.1%	Medium	
get the sum of values and number of nodes, postorder
->>

<<-236	Lowest Common Ancestor of a Binary Tree    		47.4%	Medium	->>
<<-235	Lowest Common Ancestor of a Binary Search Tree    		50.9%	Easy	
go to left or right O(log(n))
->>

<<-1123	Lowest Common Ancestor of Deepest Leaves    		67.2%	Medium	
postorder visit and get the maxDepth and then back to root to check if left and right==maxh.
tricky.
->>

<<-1145	Binary Tree Coloring Game    		51.4%	Medium	
first player choose one node, each player can only choose his neighnors.
second player can have 3 choices: left child, parent,right child. Since the choice will bprevent the first player to choose that branch. So it turns out the count the nodes in all three branches. You can always choose two branches.
->>

<<-1161	Maximum Level Sum of a Binary Tree    		71.1%	Medium	->>

<<-1214	Two Sum BSTs    		67.8%	Medium	
two bsts, check if a from bst1 and b from bst2 and a+b=target.
hashmap + traverse
->>

<<-1245	Tree Diameter    		61.1%	Medium	
n-ary tree. given a list of edges. 
dfs: get the depth and find the max and 2nd max depth for node. 
diameter is the max depth + 2nd max depth via some node.
->>

<<-1261	Find Elements in a Contaminated Binary Tree    		74.3%	Medium	
recover and find.
root is x then left=2x+1, and right=2x+2
->>

<<-1273	Delete Tree Nodes    		63.3%	Medium
the tree is given in the format of： node-parent array.
build the tree and do postorder traversal to get num_nodea and sum. Delete the subtree=reset the sum and num_nodes.
->>

<<-1302	Deepest Leaves Sum    		83.9%	Medium	
two pass is trivial one pass to get the max depth, one pass to get the sum.
one pass: bfs to get the previous row's sum.
one pass: dfs, get the depth and max depth, when d>md, reset the sum.
->>

<<-1305	All Elements in Two Binary Search Trees    		77.7%	Medium	
approach 1: inorder for tree 1 and tree 2 separately and merge sort.
approach 2: same time inorder traversal and merge. has to use iterative approach
We are doing in-order traversal independently for two trees. When it's time to 'visit' a node on the top of the stack, we are visiting the smallest of two.
->>

<<-1315	Sum of Nodes with Even-Valued Grandparent    		83.8%	Medium	
dfs with parent info.
->>

<<-1325	Delete Leaves With a Given Value    		73.4%	Medium	
post order to remove a leaf.
->>

<<-1339	Maximum Product of Splitted Binary Tree    		37.4%	Medium	
split the tree by removing one edge, return the max product of the two tree's sum.
a+b=tsum, max(ab) it is same to min(|a-b|)
two pass visit. first any order to get the sum, second postorder to get mindiff.
->>

<<-1361	Validate Binary Tree Nodes    		44.9%	Medium	
given a tree with a list of nodes and its left child and right child (-1 means no child)
a tree shall have n nodes, n-1 edges and each node except the root has one pararent, no cycle.
->>

<<-1367	Linked List in Binary Tree    		41.1%	Medium	
check if the linked list exist in the tree as a down path.
- find all nodes as starting and do dfs and traverse on linked-list. or
- it can start as root, left or right, so check 3 problem.
->>

<<-1372	Longest ZigZag Path in a Binary Tree    		54.4%	Medium	
choose a node and then choose a direction and then switch direction. Get the longest path.
postorder traversal and return the subtree max path, using left branch or right branch,
->>

<<-1373	Maximum Sum BST in Binary Tree    		38.1%	Hard	
postorder to find the left max, right min and sum and isBST.
->>

<<-1379	Find a Corresponding Node of a Binary Tree in a Clone of That Tree    		84.0%	Medium	
traverse at the same time in original and cloned tree.
->>

<<-1382	Balance a Binary Search Tree    		75.7%	Medium	
convert to sorted array and then do merge sort
->>

<<-1430	Check If a String Is a Valid Sequence from Root to Leaves Path in a Binary Tree    		45.0%	Medium	
check if the vector is a valid path.
make sure we check if it is a leaf and have the same length. using dfs with depth.
->>

<<-1443	Minimum Time to Collect All Apples in a Tree    		54.6%	Medium	
given a tree of n nodes from 0 to n-1. You spend 1s to walk an edge. There are some apples on some vertices. Start from 0 return the min time to collect all apples and return to 0.
- build the adjacency matrix
- postorder traversal
->>

<<-1448	Count Good Nodes in Binary Tree    		70.2%	Medium	
a node is good if all path from root to it has no nodes with a value > it.
dfs and pass the previous max.
->>

<<-1457	Pseudo-Palindromic Paths in a Binary Tree    		67.9%	Medium	
a tree node value are from 1 to 9. return the number of pseuso-palindrome path from root to leaf. pseudo palindrome: permutation of the path is palindrome.
- dfs
- record the digit's even odd. A path is valid if it has at most one odd.
->>

<<-1469	Find All The Lonely Nodes    		80.8%	Easy	
parent has only one child, the child is lonely node, dfs.
->>

<<-1485	Clone Binary Tree With Random Pointer    		80.1%	Medium	
same as graph clone using hashmap + traverse.
with random pointer. store node vs node in hashmap and then change all pointer's random pointer using the map.
pay attention to null random pointer (they are not in the map).
random pointer may forms a cycle, but this way we have no problem.
->>

<<-1490	Clone N-ary Tree    		83.7%	Medium	
same as deep copy of graph. dfs
->>

<<-1506	Find Root of N-Ary Tree    		80.5%	Medium	
given a list nodes (Node*) find the root.
using xor: root only appears once, but all other nodes have parents
using dfs and xor.
->>

<<-1516	Move Sub-Tree of N-Ary Tree    		62.1%	Hard	
all nodes have unique values, given two nodes p and q. 
move p subtree to become a direct child and the last child of node q.
???
->>

<<-1519	Number of Nodes in the Sub-Tree With the Same Label    		36.2%	Medium	
a tree with n nodes from 0 to n-1, 0 as the root. each node has a char label.
The tree is given by list of edges.
return number of nodes with the same label as node i in the subtree of node i.
- adjacency matrix
- postorder (n-ary tree) return the hashmap （using vector of 26)
->>

<<-1522	Diameter of N-Ary Tree    		68.5%	Medium	
diameter: longest path among any two nodes.
postorder traversal: get the child max depth and 2nd max depth, add them and compare with global diameter
->>

<<-1530	Number of Good Leaf Nodes Pairs    		55.2%	Medium	
given a tree and a distance limit. A good pair leaf their distance <=limit.
leaf distance: must through the root (recursively). so get the left leaf depth and right leaf depth
and find combinations
->>

<<-1572	Matrix Diagonal Sum    		78.3%	Easy	->>
<<-1586	Binary Search Tree Iterator II    		65.6%	Medium	
this we need add prev and hasPrev.
- inorder traversal and put into vector, trivial
- iterative: only maintain the visited nodes using stack.
->>

<<-1602	Find Nearest Right Node in Binary Tree    		75.0%	Medium	
given a node u, find its nearest right node. using dfs and store the previous node.
->>

<<-1609	Even Odd Tree    		54.0%	Medium	
root is level 0, child at level 1. odd indexed level all even, and even index level all odd.
and in strictly increasing order.
dfs and save to a list only the previous node. and check against its index.
->>

<<-1612	Check If Two Expression Trees are Equivalent    		71.0%	Medium	
expression tree: leaf are values a-z, operation + only.
just connect the leaf nodes and sort to check if two are equal.
->>

<<-1660	Correct a Binary Tree    83.2%	Medium	
a node's right point to a node in its right on the same level
approach: dfs with hashmap.
level: 3
->>
<<-235. Lowest Common Ancestor of a Binary Search Tree
use the BST property and determine which branch to go.
->>

<<-1257	Smallest Common Region    		60.0%	Medium	
given regions: each row, the first region contains all remaining regions in the same row.
given two regions, find the smallest region contain the two regions.
- build the parent relation similar to union find
- find the lowest common ancestor for the two regions (save all parents for region1, and then the first match for region2 is the LCA)
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
approach 2: traversal and mark each found nodes using postorder traversal. The first visited node when all nodes are seen is the LCA.
it is better to use two states: the answer and the counting. (Note the dfs or postorder, we need assign the answer only the first time).
->>

<<-1666 Change the root of a binary tree
problem: node has left,right,parent. go up from leaf, if cur has a left, it becomes right child. Its parent becomes left child.
subject: binary tree, very confusing.
level: 4
->>

### segment tree or binary index tree
see the appendix for binary index tree.

<<-1505	Minimum Possible Integer After at Most K Adjacent Swaps On Digits    		36.1%	Hard	
a string of digits, you are allowed to swap two adjacent digits, at most k operations are allowed. return the min string you can obtain.
greedy: bubble sort, k>n*(n+1)/2 it is sort. 
from 0 to 9, find its first index and try to move to top (less equal to k) and leave a subproblem. TLE
segment tree or binary index tree to reduce to O(nlogn)
->>

<<-1409. Queries on a permutation with key ***
query[i] move the element at query[i] to the beginning 
- approach 1: brutal force using vector as hashmap to record value vs index 
- approach 2: binary index tree.
tag: array, value vs index, hashmap
->>

<<-327	Count of Range Sum    		35.7%	Hard	
return the number of range sums in [L,R].
prefix sum, and save in map: presum vs list of index.
then use binary search to get the lowerbound and upperbound.
other approach:
merge sort: prefix sum and divide into two parts, count the right part - left part in the range.
binary index tree:
->>

<<-308	Range Sum Query 2D - Mutable    		36.7%	Hard	
update an element.
sum region
prefix sum using similar dp.
binary index tree.
->>

<<-307	Range Sum Query - Mutable    		36.0%	Medium	
array, instead of 2d matrix.
binary index tree.
->>

<<-304	Range Sum Query 2D - Immutable    		39.7%	Medium	
prefix sum. 2d.
->>
<<-303	Range Sum Query - Immutable    		46.3%	Easy	
prefix sum. 1d.
->>

<<-1649	Create Sorted Array through Instructions    		42.0%	Hard	
segment tree 
->>

### linked list
- single linked list
- libray list
- doubly linked list
- list is the base for hash table.
- cycle detection
- traversal

- linked list is very mistake prone since it easily will produce infinite loop.
- often resursive approach can be applied to most linked-list problem
- linked list is inside data structure for hashmap et al
- list iterator with hashamp often can be used to reduce complexity.

<<-19	Remove Nth Node From End of List    		35.4%	Medium	->>
<<-25	Reverse Nodes in k-Group    		43.5%	Hard	->>
<<-24	Swap Nodes in Pairs    		51.6%	Medium	->>
<<-23	Merge k Sorted Lists    		41.4%	Hard	->>

<<-61	Rotate List    		31.3%	Medium	
rotate by k places. find the length and k%=n, and then do the rotation.
->>
<<-82	Remove Duplicates from Sorted List II    		37.6%	Medium	
delete one nodes that have duplicate numbers, leaving only the distinct ones.
similar to a stack. but need keep prev.
->>
<<-80	Remove Duplicates from Sorted Array II    		44.7%	Medium	
duplicates only allows 2 times. two pointer, check nums[i] with nums[i-2]
->>

<<-86	Partition List    		42.5%	Medium	
give a value x, parition so that all nodes less than x comes before node >=x.
two list and merge.
->>

<<-92	Reverse Linked List II    		39.8%	Medium	
reverse from position m to n. do it in place.
->>

<<-143	Reorder List    		39.7%	Medium	
l0->ln->l1->Ln-1--
break into two, reverse one and merge.
->>
<<-142	Linked List Cycle II    		38.9%	Medium	->>
<<-141	Linked List Cycle    		41.9%	Easy	->>

<<-328	Odd Even Linked List    		56.5%	Medium	
group odd value nodes followed by the even list.
create two lists and then connect them.
->>

<<-426	Convert Binary Search Tree to Sorted Doubly Linked List    		60.2%	Medium	
inorder traversal do the transformation in place.
- create a dummy node and prev node.
->>

<<-430	Flatten a Multilevel Doubly Linked List    		56.3%	Medium	
linked list node with child pointer, recursively flatten the child and connect.
->>

<<-445	Add Two Numbers II    		55.8%	Medium	
reverse link-list and add then reverse.
->>

<<-708	Insert into a Sorted Circular Linked List    		32.2%	Medium	
- empty list
- traverse and find a proper position with prev<val<=cur.
- all are equal and not=target, insert anywhere.
->>

<<-725	Split Linked List in Parts    		52.6%	Medium	
split the list into k consecutive parts. K could be larger than number of nodes.
->>

<<-817	Linked List Components    		57.4%	Medium	
given a linked list of unique numbers and a subset. return the number of connected groups.
hashset and do traversal.

->>

<<-876	Middle of the Linked List    		68.8%	Easy	
fast slow pointer.
->>
<<-83	Remove Duplicates from Sorted List    		46.0%	Easy	
easy but tricky, for each node we need iterate until we find no more duplicates.
->>

<<-1206	Design Skiplist    		57.9%	Hard	
- design a node structure with right and down pointer.
- head always point to the top layer (since we are going right and down)
- build we need from bottom layer to top layer.

add a node: each layer shall have an option to add or not add, but stop if lower layer do not add, top layer do not add either.
in case all added and insertion point not used, we need add a new layer.
->>

<<-1265	Print Immutable Linked List in Reverse    		94.5%	Medium	
save into vector and then reverse print
->>

<<-1474	Delete N Nodes After M Nodes of a Linked List    		74.4%	Easy	->>

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


## string
<<-93	Restore IP Addresses    		36.7%	Medium	
ip address missing all dots.
just try all possibles !!
->>

<<-214	Shortest Palindrome    		30.3%	Hard	
given a string, you can add chars in front of it to make it palindrome. return the shortest palindrome.
- equiv find the longest palindrome starting with the first char. try longest first. (TLE)
- KMP to speed it.
->>

<<-306	Additive Number    		29.5%	Medium	
actually check the string can be partitioned to a fib series.
once we determine the first two elements, the whole array is determined.
try all combinations, the first can goes to half, second can also go to half.
string add.
->>

<<-161. One Edit Distance
insert exactly one char into s and get t.
del exactly one char from s and get t.
replace one char in s and get t.
- the length shall differ <=1.
- swap s and t if s is shorter than t. (no dp)
- choose delete or replace (insert is converted)
->>

<<-388	Longest Absolute File Path    		42.1%	Medium	
given path structure with \n and \t number of \t indicates the level of the path.
return the length of the longest file path.
- read line (eat the \n)
- check number of \t to get the level
- if level < current, pop the saved director level.
- if a file name, get the length.
->>

<<-443	String Compression    		42.4%	Medium	
same char group. size=1, just use the char, otherwise char+repeat num.
two pointer, one count, one modify in-place.
->>

<<-459	Repeated Substring Pattern    		43.1%	Easy	
- check 1 to n/2 length
- or (s+s).substr(1,2n-2) contains str itself.
->>

<<-468	Validate IP Address    		24.6%	Medium	
ipv4 vs ipv6. be careful of all the restrictions.
->>

<<-524	Longest Word in Dictionary through Deleting    		48.7%	Medium	
given a list of dictionary word, and a string. we can delete some chars to get the dictionary word.
return the longest word with the smallest lexi order.
brutal force: sort the dict in lexi order, check each word in dict and see if it is a subsequence of s.
->>

<<-544	Output Contest Matches    		75.5%	Medium	
team the strongest with the weakest. the initial ranking is 1 to n.
you need always keep strong vs weaker order
for example 4 teams, ((1,4),(2,3)), n=2^k.
a bit tricky:
- each round using two pointer match from both ends.
- each round reduces by half.
- until reduce to 1 group.
->>

<<-647	Palindromic Substrings    		61.4%	Medium	
return number of pal substrings (with different start end index)
- expand at all possible position 2n-1 positions
- dp: dp[i,j]=ispal(s,i,j)+dp[i+1,j]+dp[i,j-1]-dp[i+1,j-1], can also be used for subsequence.
- manacher most fast.
->>

<<-686	Repeated String Match    		32.6%	Medium	
return the min times of repeat string a so that b is a substrng of the repeated one.
- at least to make the length >=b.
- shall have the same set of chars.
repeat until a length >=b then add just one to make rotation valid (keeps finding at the end)
->>

<<-718	Maximum Length of Repeated Subarray    		49.7%	Medium	
repeated subarray in two arrays.
- dp: just like LCS
- binary search. rolling hash
->>

<<-722	Remove Comments    		35.5%	Medium	
regex or mark the comment begin and start similar to stack.
->>

<<-758	Bold Words in String    		46.8%	Easy	
given a list keyword, all appearances in string shall be bolded by <b> </b>
the returned string shall have least number of tags possible.
same as 616. just mark the index to be bolded and then check the segment.
->>
<<-616	Add Bold Tag in String    		44.1%	Medium	->>

<<-796	Rotate String    		49.4%	Easy	
check if we can rotate A to get B.
circular: A+A to see if B is a substr.
->>

<<-820	Short Encoding of Words    		51.2%	Medium	
return the shortest encoded string, encoding format:
word#word#...each word in the list can be found in the encoded string.
- we shall embed some common subarray into one word. each word is a suffix of a substring
- using Trie
- or sort by length, short word can be suffix of longer.(build a suffix array hashmap)
->>

<<-831	Masking Personal Information    		44.6%	Medium	->>
<<-833	Find And Replace in String    		51.0%	Medium	
find and replace shall be done separately.
->>

<<-859	Buddy Strings    		29.9%	Easy	
by swapping a pair in A exactly once, can you get B?
compare one by one: 0 mismatch or 2 mismatch, for 2 mismatch must be able to swap. for 0 mismatch we need to find two same chars in A.
->>

<<-890	Find and Replace Pattern    		74.0%	Medium	->>

<<-893	Groups of Special-Equivalent Strings    		67.9%	Easy	
you can swap odd index vs odd index, even index vs even index.
sort by odd or even.
->>

<<-1016	Binary String With Substrings Representing 1 To N    		59.4%	Medium	
just find if all the n string appears in it.
->>

<<-1062	Longest Repeating Substring    		57.7%	Medium	
- binary search
- dp solution, id(s[i]==s[j] dp[i,j]=dp[i-1,j-1]
- trie
- KMP
->>

<<-1065	Index Pairs of a String    		61.1%	Easy	
- brutal force.
- trie.
->>

<<-1071	Greatest Common Divisor of Strings    		51.7%	Easy	->>

<<-1078	Occurrences After Bigram    		64.7%	Easy	
first second and store the followed words.
stringstream just keep 2 in the storage, keep updating.
->>

<<-1233	Remove Sub-Folders from the Filesystem    		61.2%	Medium	
given a list of folders, remove all those subfolders
observation:
- subfolder will be longer than its parent folders, so we can sort the input by length
- we can build a trie, shorter one is inserted first, if we found the folder already, then it is a subfolder

or we sort lexi order, then upper layer folder is a prefix of subfolder.
->>

<<-1316	Distinct Echo Substrings    		49.4%	Hard	
return number of substring which is in the form of A+A.
given a substr length (even) and check its inside pattern.
using hashset to achieve distinct.
->>

<<-1323	Maximum 69 Number    		77.9%	Easy	
number consists of 6 and 9 only. You can change at most one digit from 6 to 9 or 9 to 6 to get the max.
find the first 6 and replace it with 9.
->>

<<-1392	Longest Happy Prefix    		41.1%	Hard	
find the longest prefix which is also a suffix (not including itself).
to get O(N) using rolling hash.
->>

<<-1400	Construct K Palindrome Strings    		62.5%	Medium	
check if string s can be decomposed to k palindrome strings. (use all characters)
count each characters. 
- string size <k, not OK
- odd chars >k, not OK
- other cases are OK.
->>

<<-1404	Number of Steps to Reduce a Number in Binary Representation to One    		50.5%	Medium	
reduce to 1: if it is even, divide by 2, if it is odd, add 1.
approach 1: simulate the process with string add.
approach 2: optimize with a cf.
->>

<<-1408	String Matching in an Array    		62.6%	Easy	
given a list of words, find words which is a substring of another word.
sort by length: shorter one can be longer one's substring
->>

<<-1410	HTML Entity Parser    		54.3%	Medium	
find given list of special strings and replace with given string
typical string: find all positions and replace from right.
->>

## array
<<-48	Rotate Image    		58.5%	Medium	->>
<<-189	Rotate Array    		36.1%	Medium	
- using reverse
- using stl
->>

<<-229	Majority Element II    		38.1%	Medium	
find elements appear more then n/3 times.
one or two candidate O(N), voting algorithm.
->>
<<-169	Majority Element    		59.5%	Easy	
find elements appear more than n/2.
voting algorithm.
->>

<<-238	Product of Array Except Self    		60.9%	Medium	
no division allowed.
left and right two direction (prefix suffix product)
->>

<<-360	Sort Transformed Array    		49.3%	Medium	
given array and apply ax%2+bx+c to it. return the array in sorted order.
depending on a, it could be a parabolic curve upside or downside.
a==0, check b.
a!=0, using two pointer.
->>

<<-361	Bomb Enemy    		46.4%	Medium	
given a 2 d grid, 'W' wall, 'E' enemy, '0' empty.
a bomb will kill enemies on the row and column until blocked by wall.
return max enemies killed by one bomb.
try each empty cell and count number of enemies killed.
->>

<<-363	Max Sum of Rectangle No Larger Than K    		38.2%	Hard	
idea: get all possible submatrices and reduce to 1d array, and then find max subarray sum <=k.
->>

<<-419	Battleships in a Board    		70.6%	Medium	
battle ship are horizontal or vertical, battleship are separated at least one empty cell.
if we see empty + x horizontal or vertical we add one ship. 
->>

<<-442	Find All Duplicates in an Array    		68.4%	Medium	
elements are from 1 to n, some appear twice, other once, find all the elements appear twice.
mark negative using as index. Same as 448.
->>

<<-448	Find All Numbers Disappeared in an Array    		56.0%	Easy	
given 1 to n, some appear twice others appear once. find all disappeared numbers.
- sort 
- use the number-1 as the index and mark seen as negative. those positives are seen twice.
->>

<<-475	Heaters    		33.4%	Medium	
given a list of houses on a line and a list of heaters on the line.
return the min radius of the heater so that each house is covered.
- sort houses, sort heaters
- for each house find the closest heater and get the max distance. (binary search)
->>

<<-481	Magical String    		47.8%	Medium	
s contains only 1 and 2. if we concat the number of contiguous digits and we get the same string of s.
given a integer N, return the number of 1 in the first N digits in the magical string.
odd: add 1, the char in s represent the number of digits to add.
even: add 2
->>


<<-498	Diagonal Traverse    		48.9%	Medium	
given a matrix, return the diagonal traverse, up and down...
- can use hashmap and then reverse 
- can use two pointer. rebounce back when hit the 4 walls.
->>

<<-525	Contiguous Array    		43.2%	Medium	
binary array find the longest subarray with equal number of 0 and 1.
change 0 to -1, and then use prefix sum to find longest subarray with sum=0;
hashmap
->>
<<-548	Split Array with Equal Sum    		47.2%	Medium	
given an array, three index i,j,k will split the array into 4 subset (excluding the selected index elements at i,j,k). The 4 subsets shall have the same subarray sum. Check if it is possible.
- prefix sum.
- fix the mid pointer by looping all possible positions
- try all possible left and right positions save all possible valid cut into hashmap.
->>

<<-611	Valid Triangle Number    		48.8%	Medium	
given an array, find number of triplets which can make a triangle.
a+b>c 
- sort the array and  use two pointer i,j and find k using binary search O(N^2log(n))
- sort the array and use A[i] as the largest side, and then binary search A[l]+A[r] O(N^2)
->>

<<-581	Shortest Unsorted Continuous Subarray    		31.4%	Medium	
one subarray if sorted then the whole array is sorted
compare to sorted array.
->>

<<-624	Maximum Distance in Arrays    		39.2%	Medium	
given m sorted array. pick two elements from two different array and get the distance=|a-b|. return the max distance.
note: it is not a matrix. 
get the min of the first col and max from the back of each row.
->>

<<-678 valid paenthesis string
string include (*) * can represent left or right or empty.
'*' as a left, we increase counter by 1
'*' as a right,we decrease counter by 1
'*' as empty, no change
- recursive approach, passing the left and right counter.
- direct approach
for example "(**())" we get the tree structure
                1           (
		   /    |    \
          0     1     2     * -1,0,1
		 /|\   /|\   /|\    
	  -1  0 1 0 1 2 1 2 3   * -1,0,1
	   0  1 2 1 2 3 2 3 4   (
	  -1  0 1 0 1 2 1 2 3   )
	  -2 -1 0-1 0 1 0 1 2   ) 
- we see 3 paths leading to 0, (with any <0 is invalid) they are (()) and ()(()) and (()())
- the final number will from [min,max], if it covers 0, then we are fine.
so we just counter min and max.  need keep mn>=0 to eliminate invalid paths.
->>

<<-680	Valid Palindrome II    		36.8%	Easy	
remove at most one char to make it palindrome.
->>

<<-680	Valid Palindrome II    		31.4%	Medium	
you can delete at most one char. compare string with reversed string. at least two mismatch. remove the first or the second.
->>

<<-689	Maximum Sum of 3 Non-Overlapping Subarrays    		46.9%	Hard	
fix left and right and find range for bottom
->>

<<-696	Count Binary Substrings    		57.0%	Easy	
substr same number of 0 and 1. and all 0 and all 1 are grouped together.
- 01,0011,000111...pattern.
use pre counter and current counter, when we see a change 01 or 10, we reset.
->>

<<-723	Candy Crush    		71.5%	Medium	
a grid, different value means different type of candies.
0 mean the cell is empty. You need to restore it to a stable state.
- if 3 or more same type candies on row or column, eliminate them all.
- top candies will drop to fill the empty space
- after drop, more cells will eliminate until no more crashes.
approach: since it will only drop to bottom. better to use bottom as the first row. so we only need shift the columns forward.
simulate the process.
->>

<<-769	Max Chunks To Make Sorted    		55.2%	Medium	
given a permutation of 0 to n-1. divide into groups and sort each partition and connect them the whole array is sorted.
return the max number of chunks.
- greedy: compare to sort. if identical +1, using two hashset to save elements in A and sorted. If they are equal this is a group.

->>
<<-768	Max Chunks To Make Sorted II    		49.4%	Hard	
elements could be duplicate. use multiset.
->>

<<-775	Global and Local Inversions    		42.4%	Medium	
permutation of 0 to n-1. global inversion if i<j and A[i]>A[j]
local inversion: A[i]>A[i+1]
check if local inversion=global inversion
- local inversion is also global inversion.
- if we invert the inversion we shall be able to get a sorted array-->
- if(abs(A[i]-i)>1) there is a global inversion.
or lmax <A[i+2].
->>

<<-782	Transform to Chessboard    		46.8%	Hard	
given nxn 01 matrix, swap any two rows or two columns
return min number of mvoes to transform to a chess board or impossible -1
- determine the [0,0] element is 0 or 1. If N is odd, this is fixed.
- if even, have two choices the top left can be 0 or 1.
- if first row is determined, then everything is determined.
- rowSwap compare to row 0
- colSwap compare to col 0
->>

<<-792	Number of Matching Subsequences    		47.8%	Medium	
given a list of words, find number of words which is subsequence of string s.
- check if subsequence will be O(N)
- store each char's index and do binary search 
->>

<<-798	Smallest Rotation with Highest Score    		44.6%	Hard	
given an array and you can pick a index k and rotate k to n-1 to left. after rotation, any elements <=index worth 1 point. A[i] is from 0 t0 n-1.
return the max score we could receive.
- need O(N)
- it is also equivalent to left rotation.
- each step: A[0] goes to A[n-1] and we get 1 point
- left rotation one step: A[i]->A[(i-1+N)%N]

- interval approach: for each element we calculate the rotation when it gains point and lose point.
A[i] at ith position: 
gain point: we move it to the end, which needs (i+1)%N.
lose point: move A[i] to position A[i]-1 (so value>index) which needs (i+1-A[i]+N)%N rotations
find the most overlapped. 
->>

<<-807	Max Increase to Keep City Skyline    		84.1%	Medium	
given a grid with each element height. You are allowed to increase the height
looking from 4 direction up down, left right must be the same as orgiinal.
return the max sum increased.
looking left or right we get the max in each row.
looking up or down, we get the max in each column.
so each element can be increased to col and row max.

->>
<<-821	Shortest Distance to a Character    		67.5%	Easy	
in a string find each char to a given char's shortest distance.
two pass to update the distance to left and right.
->>

<<-822	Card Flipping Game    		43.4%	Medium	
each card with back[i] and front[i]. we choose and flip if the card is not on the front of any card, it is good. for example front=[1,2,4,4,7],back=[1,3,4,1,3].flip the 2nd get 3, which is not on the front. so the smallest good is 2 (front value)
what is the smallest good number?
- front back the same card are not candidates
very confusing
->>

<<-825	Friends Of Appropriate Ages    		43.5%	Medium	
A will not request B if any of the condition is satisified:
- age[B]<=0.5*age[A]+7
- age[B]>age[A]
- age[B]>100 && age[A]<100
otherwise A will request B. return number of requests.
group peope according to their ages.

->>

<<-835	Image Overlap    		62.0%	Medium	
convert to index moving in a 1d array.
then overlap using i-j as key to do hashmap.
->>

<<-838	Push Dominoes    		49.3%	Medium	
given a list of dominoes, and push some dominos at the same time with L or R.
return the final state of the dominos. L means left, R, means right, . means not pushed.
using stack to store dominos.
- L on the left, all left shall be 'L'
- R...L, half R and half L. (odd length mid one will not affect)
- R on the right, all shall be R.
using stack is not straightforward.
we only need find R...L pair and use two pointer to assign the value.
for convenience we can add L and R on the left side and right side.
->>

<<-840	Magic Squares In Grid    		37.6%	Medium	
magic square is 3x3 with 1-9 each and each row, col, diagonal sum are all equal. return number of magic square in matrix.

->>
<<-845	Longest Mountain in Array    		38.5%	Medium	
- can use binary search
- can use one pass O(N) to find up and down and update the max size.
->>

<<-849	Maximize Distance to Closest Person    		44.3%	Medium	
similar to exam room. 
->>

<<-855	Exam Room    		43.4%	Medium	
a single row. sit:
- maximize the distance to the closest person
- multiple option take the lowest index seat.
- nobdy then take 0.
just simulate the process, using vector or set to store the index
->>

<<-885	Spiral Matrix III    		70.0%	Medium	
give a 2d grid with R rows and C columns, start with (r0,c0), goes clockwise.
it goes 1R,1D,2L,2U,3R.....
out of boundary just ignore.
simulate the process 
->>

<<-59 Spiral matrix II.
generate nxn matrix using spiral order (shrinking size)
use 4 boundaries.
->>

<<54 Spiral Matrix
traverse the matrix in spiral order (using 4 bounaries)
->>

<<-900	RLE Iterator    		54.8%	Medium	
run length encoding iterator.
prefix sum on the element count and do binary search.
->>

<<-915	Partition Array into Disjoint Intervals    		45.7%	Medium	
partition into left and right, so that left[i]<=right[j] for any i,j.
left max and rmin.
->>

<<-926	Flip String to Monotone Increasing    		52.7%	Medium	
give a 01 string, you can flip 1 to 0 or 0 to 1.
return the min number of flips to make it monotonic increasing.
count left 1s and right 0s.
->>

<<-927	Three Equal Parts    		34.2%	Hard	
binary array, find the index i,j divide into 3 equal parts.
count 1's pattern, leading zero does not matter, but trailing matters.
->>



<<-942	DI String Match    		73.2%	Easy	->>
<<-941	Valid Mountain Array    		32.3%	Easy	
climb and meet at the peak.
->>

<<-949	Largest Time for Given Digits    		36.3%	Medium	
given 4 digits, and each use once exactly.
try all permutations.
->>

<<-961	N-Repeated Element in Size 2N Array    		74.2%	Easy	
array has N unique numbers, only one appears N times.
- hashset approach O(N)
-  repeated numbers have to appear either next to each other (A[i] == A[i + 1]), or alternated (A[i] == A[i + 2]). The only exception is sequences like [2, 1, 3, 2]. In this case, the result is the last number, so we just return it in the end. 
->>

<<-962	Maximum Width Ramp    		45.9%	Medium	
in the array i<j and A[i]<=A[j] find the max(j-i)
- get the left min array (the array is in descending order)
- reverse left min
- get rmax and compare with lmin and get the max ramp.
tricky.
->>

<<-982	Triples with Bitwise AND Equal To Zero    		55.7%	Hard	
- first get pairs & stored in hashmap
- then check the elements with the pair to get 0.
->>

<<-999	Available Captures for Rook    		66.8%	Easy	->>

<<-1007	Minimum Domino Rotations For Equal Row    		50.9%	Medium	
given two rows of number, you can swap A[i] B[i].
return the min swaps to make A or B the same.
two choices: make all =A[0] or all=B[0]. Then you have two choices put A[0] to A or B.
->>

<<-1013	Partition Array Into Three Parts With Equal Sum    		50.0%	Easy	
count group.
->>
<<-1014	Best Sightseeing Pair    		52.7%	Medium	
given array A[i]=value of ith spot.distance of i and j= |i-j|.
max the function A[i]+A[j]+i-j, i<j.
separate A[i]+i and A[j]-j for left and right side.
using lmax and rmax approach.
->>

<<-1031	Maximum Sum of Two Non-Overlapping Subarrays    		58.6%	Medium	
given the two subarray's length L and M, return the max sum of the two non-overlapped subarray sum.
two problem max(sub(A,L,M),sub(A,M,L)) 
then sliding window to get the lmax and rmax, then combine.
->>

<<-1053	Previous Permutation With One Swap    		50.5%	Medium	
greedy: from right to left find the first larger one, (if equal shall push forward)
[1,9,4,6,7] the first peak is 9, and swap with the max 7. ->17469
->>

<<-1089	Duplicate Zeros    		52.2%	Easy	
inplace duplicate is tricky. using two pointer from left to right and count the 0, and then from right to left to add 0 and place element in right position. O(N)
->>

<<-1099	Two Sum Less Than K    		60.8%	Easy	
find the max pair sum <K. sort previous elements in set and do binary search
->>

<<-1103	Distribute Candies to People    		63.6%	Easy	
n people distributes 1,2,3.....each time one more candy is given to next person, and then return to the beginning. find the number of each person's candies.
do not use math, but use % and add.
->>

<<-1138	Alphabet Board Path    		49.8%	Medium	
a-z arranged by 5 letters a row. print a string, return the path. The only special is the z, we need first go to u. or first go to u and then go to z.
using bfs is overkill. 
->>

<<-1144	Decrease Elements To Make Array Zigzag    		45.8%	Medium	
even index element > odd index elements or odd index elements > even index elements (neighboring)
only -1 is allowed.
to make even smaller, decrease the even to smaller of the two -1
->>

<<-1177	Can Make Palindrome from Substring    		35.7%	Medium	
query [L,R,k] choose the substring [L,R] and check if we can replace at most k chars and rearrange to a pal-string.
a pal string with at most one odd frequency char.
- get the prefix sum for each char (at each position: the number of each char in left so far)
- then we count the number of odd chars. (we can only keep odd,even)
->>

<<-1200	Minimum Absolute Difference    		66.6%	Easy	
find all pairs with diff=min diff.
->>

<<-1213	Intersection of Three Sorted Arrays    		79.0%	Easy	
return sorted list of elements appear in all 3 arrays.
use 3 pointers 
->>

<<-1198	Find Smallest Common Element in All Rows    		74.9%	Medium	
row in sorted order.
this is similar to find all common elements in 3 sorted array.
we can try each element in first row and do binary search for other rows.
->>

<<-1222	Queens That Can Attack the King    		69.0%	Medium	
check all 8 directions around the king
->>

<<-1260	Shift 2D Grid    		61.5%	Easy	
it is actually a 1d rotation->>

<<-1267	Count Servers that Communicate    		57.6%	Medium	
server can be counted only when there is more than 1 server on row or column.
count when the count on row or col >1
->>

<<-1275	Find Winner on a Tic Tac Toe Game    		52.8%	Easy	
classical problem A +1, B -1, and check row, col, diag sum
->>

<<-1287	Element Appearing More Than 25% In Sorted Array    		60.2%	Easy	
the element must appear at n/4,n/2,3n/4,end.
->>

<<-1299	Replace Elements with Greatest Element on Right Side    		74.6%	Easy	
get rmax from right side.
->>

<<-1324	Print Words Vertically    		58.7%	Medium	
not allowing trailing spaces, so when word length exceed rows, add "", when increase columns, all previous add ' '.
->>

<<-1375	Bulb Switcher III    		63.9%	Medium	
n bulbs and given a list of operation op[i] means turn on op[i] at i time. When all previous lights are on, Return the number of moments in which all turned on bulbs are blue.	
prefix sum equals n*(n+1)/2.
greedy math
->>

<<-1409	Queries on a Permutation With Key    		81.2%	Medium	
given a array initial as 1 to n. a query of number will return the index, and then move the element to the beginning.
simulation the operations, but using hashmap to record the index changes,
->>

<<-1424	Diagonal Traverse II    		44.6%	Medium	
2d array but not a matrix. traverse diagonal all go up right.
approach 1: save to 1d with i,j and sort using i+j. O(nlogn) n is the total element number.
approach 2: using hashmap with key=i+j, and then reverse. O(n)
->>

<<-1534	Count Good Triplets    		79.9%	Easy	
i<j<k and |A[i]-A[j]|<=a, |A[j]-A[k]|<=b and |A[i]-A[k]|<=c
brutal force, with some optimizations to reduce loops. each loop satisfy one condition.
->>

<<-1428	Leftmost Column with at Least a One    		48.2%	Medium	
interactive, row in sorted order. each row we find a 1 we push left the index.
->>

<<-1427	Perform String Shifts    		53.3%	Easy	
using rotate and assemble left and right all together, note rotate is left shift.
->>

<<-1437	Check If All 1's Are at Least Length K Places Away    		61.9%	Medium	
find the distance of 1 to previous 1.
->>

<<-1446	Consecutive Characters    		61.8%	Easy	
max length of consecutive chars
->>

<<-1455	Check If a Word Occurs As a Prefix of Any Word in a Sentence    		65.0%	Easy	->>

<<-1464	Maximum Product of Two Elements in an Array    		77.0%	Easy	
find the max and 2nd max in O(n).
->>

<<-1472	Design Browser History    		68.8%	Medium	
easy array problem.
->>

<<-1498	Number of Subsequences That Satisfy the Given Sum Condition    		37.9%	Medium	
given an array of integer, find the number of subsequences such that the min+max<=target.
- the order does not matter, sort it.
- each element can be min or max for some subsequences
- two pointer to get rid of too large elements
- if left has L elements, then 2^L subsequences using current number as max.
->>

<<-1508	Range Sum of Sorted Subarray Sums    		63.0%	Medium	
all subarray's sum and sort in order, give a [l,r] and get the range sum.
- brutal force using prefix sum. O(N^2)
- min heap: 
for example [1,2,3] we can get 3 arrays:
[1],[1,2],[1,2,3]-->[1,3,6]
[2],[2,3]->[2,5]
[3]->[3]
then use k-list merge sort.
->>

<<-1513	Number of Substrings With Only 1s    		40.9%	Medium	
given a binary string, return the number of substrings of all 1s.
count the number of subarrays ending with current 1. 1+2+3+4+...+len.
->>

<<-1554	Strings Differ by One Character    		63.1%	Medium	
a list of words with same length, return if there are two strings differ by 1 char at the same index. need O(nm).
rolling hash: for each column, we ignore the column and calculate the hash value and store in hashmap, if we see it then we found one. rolling hash is faster than string.
->>

<<-1560	Most Visited Sector in a Circular Track    		56.9%	Easy	
Note the whole tracks can be omitted so simplified. Greedy.
->>

<<-1573	Number of Ways to Split a String    		30.4%	Medium	
given a 01 string, divide into 3 subarrays so that number of 1s is the same among s1,s2 and s3.
find the mid string s2 and check number of 0 on its left and right.

->>

<<-1582	Special Positions in a Binary Matrix    		64.3%	Easy	
given a 01 matrix. position (i,j) is special if all other elements in row and col are 0.
two passes: get row sum and col sum. and then A[i,j]==row[i]==co[j]
->>

<<-1583	Count Unhappy Friends    		52.6%	Medium	
n people. preference n x n-1. ith row is ith person's preference over friends. They are divided into pairs. a pair [x,y] is not happy if there is a pair (u,v):
x prefer u over y and u prefers x over v. 
- convert matrix to 2d hashmap so that we can get preference in O(1)
- for all pairs, check if they are not happy.

->>

<<-1588	Sum of All Odd Length Subarrays    		81.1%	Easy	
brutal force is easy.
O(N): subarry with odd length containing A[i], calculate the times.
left we can choose 0,1,...i elements
right we can choose 0,1,2 to n-i. remove those even length (half)
->>

<<-1592	Rearrange Spaces Between Words    		43.8%	Easy	
string operation: read the words, count the spaces and rearrange the spaces
->>
<<-1606	Find Servers That Handled Most Number of Requests    		36.0%	Hard	
k servers from 0 to k-1. each server handles one task at a time.
ith task arrives, if all server is busy the task is dropped.
if(i%k)th server is available, assign the task to it.
otherwise, assign the task to next available server. (wrap around)
each task has a computation time and an arrival time. arrival time is strictly increasing.
find the busiest server.
brutal force: each task we need search k servers, thus O(nk)
optimization: using an ordered map to maintain server's next available time. Once the server is busy we add k to it.
maintain a set for free servers, and a pq for busy servers. When arrival times, update busy servers into free servers. always add two i and i+k into it.
circular array, heap.
->>

<<-1608	Special Array With X Elements Greater Than or Equal X    		62.4%	Easy	
check if there exisit a number that there are exactly x number >=x. x is not necessary an element in the array.
counting sort: since elements value<=1000 and array length <=100.
x is limited <=100, so bin can be limited to 100, values >100 are put in the same bin.
then postfix sum and check sum == index. O(n) 2 passes.
- sort and binary search O(nlogn)
->>


<<-1610	Maximum Number of Visible Points    		27.3%	Hard	
a list of points in 2d plane, an angle and your location in the plane. You can rotate.
return the max number of points you can see.
sliding window with angle in circular array. using atan2 to calculate the angle to our position. unwrap the angles.

->>

<<-1614	Maximum Nesting Depth of the Parentheses    		84.4%	Easy	
depth(A,B)=max(depth(A),depth(B))
depth("("+A+")")=1+depth(A)
equivalent: accumulate ( as +1, and ) as -1, and get the max positive.
parenthese pairing.
->>

<<-1616	Split Two Strings to Make Palindrome    		36.5%	Medium	
select an index, two strings of the same length. Apre+Bpost or Bpre+Apost is palindrome.
skip identical chars from both end, and then get smaller problem.
->>

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
- a very good example how to optimize an algorithm.
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
- at the last it will remove some elements from both ends. So it is equivalent to find the longest window sum=tsum-x. (find the longest window with sum=tsum-target.
this can be done using sliding window + hashmap.
->>

<<-1657	Determine if Two Strings Are Close    		50.0%	Medium	
op1: swap any two chars.
op2: transform all char a to b and all char b to a.
check if they are close.
equivalent: sort the hashmap and check if they are equal.
->>

<<-1643	Kth Smallest Instructions    		41.7%	Hard	
given a mxn grid, from (0,0) to bottom right, and integer k. find the kth smallest path using HV
combination, and reduce k each step. math.
calculate combination using pascal triangle or direct calculation.
->>

<<-1625	Lexicographically Smallest String After Applying Operations    		62.6%	Medium	
given a digit string and a, b.
operation 1: add a to all odd index digits. if past 9, cycle back to 0. (digit+a)%10
operation 2: rotate right by b positions.
return the min string, you can apply any number of operations.
bfs: for each candidate, rotate to get all possible strings (unvisited) and then apply add a.
->>

## math
<<-29	Divide Two Integers    		16.5%	Medium	->>
<<-43	Multiply Strings    		34.4%	Medium	->>
<<-50	Pow(x, n)    		30.7%	Medium	
binary exponential.
->>

<<-60	Permutation Sequence    		39.0%	Hard	
find the kth permutation using 1 to n.
determine the first digit k%(n-1)!
for example n=3, k=3
the first: k%(2!)=1 so the first one is 2. k is updated with 1
k%(1!)=0, so the second one is 1. k is updated with 0
remaining shall be sorted.
->>


<<-201	Bitwise AND of Numbers Range    		39.5%	Medium	
get the lowest bit and then shrink the range by 2 to get higher bits.
->>

<<-223	Rectangle Area    		38.0%	Medium	
find the covered area by two rectangles.
A+B-overlap(A,B)
->>

<<-233	Number of Digit One    		31.5%	Hard	
count total number of digit 1 for all numbers <=n.
from MSB to LSB.
->>

<<-260	Single Number III    		65.1%	Medium	
two elements appear once and others appear two times.
- xor will eliminate all appear twice
- then using one set bit in the xor result to paritition into two parts and do xor in each parts

->>

<<-268	Missing Number    		52.7%	Easy	
missing in [0,n], xor.
->>

<<-287	Find the Duplicate Number    		56.6%	Medium	
n+1 elements with 1-n, one appear twice. xor
->>

<<-296	Best Meeting Point    		57.9%	Hard	
given a 2d grid with some people on grid, find the meeting point to minimize the total distance.
all x's median and all y's median.
->>
<<-311	Sparse Matrix Multiplication    		63.1%	Medium	->>
<<-319	Bulb Switcher    		45.2%	Medium	
n bulbs initially off. turn every 2,3,4,5...
return the number of bulbs that are on after n rounds.
even times restore to initial state
odd times on.
it is like to get rid of the factors, only square number has odd number of factors.
->>

<<-330	Patching Array    		34.8%	Hard	
given a sorted array, add/patch elements to the array such that any number in [1,n] can be formed by the sum of some elements in the array, return min number of patches required.
        //the fastest way to go to n without leaving out any number
        //if the smaller subproblem  solves from 1 to m, then add a number m+1, it solves the problem from 1 to 2m+1
        //the greedy choice is the smaller subproblem's sum+1
->>

<<-335	Self Crossing    		28.5%	Hard	
you are given an array distance to go counter-clockwise. first at (0,0) facing north.
check if the path cross itself.
only 3 scenario:
i cross with i-3 (vertical horizaontal)
i cross with i-4 (overlap same direction)
i cross with i-5 (vertical horizontal)
->>

<<-356	Line Reflection    		32.3%	Medium	
given n points on xy plane. find if there is a line parellel to y-axis, so that all points are symmetric.
- if symmetric the y shall be paired the same.
- the x shall be the mid point of the leftmost and rightmost point.
- for each y, keep the x in sorted order and then check pair sum==2*mid.

->>

<<-357	Count Numbers with Unique Digits    		48.6%	Medium	
count numbers in range [0,10^n] given n with unique digits.
combination: n digits, the first digit 9 choices, second 9 choices, 8,7...
->>

<<-365	Water and Jug Problem    		30.8%	Medium	
fermat theorem.
        //number theory: coprime: can get any number
        //has a gcd, then it can only get any number of N*gcd
        //for example 5,3: 5-3=2, 5-(3-2)=1

->>

<<-372	Super Pow    		36.5%	Medium	
binary exponential. this can use base 10 log10
->>

<<-384	Shuffle an Array    		53.5%	Medium	
- using stl random_shuffle
- swap current (which is similar to stl shuffle)
 for (int i = 0;i < result.size();i++) {
    int pos = rand()%(result.size()-i);
    swap(result[i+pos], result[i]);
  }
->>

<<-390	Elimination Game    		44.7%	Medium	
from left to right, remove the first an every other element, then do it from right
until only one is left. return the last number.
the trick is we convert right to left to left to right equivalently so we jsut need to update our head and remaining elements.
->>

<<-391	Perfect Rectangle    		30.8%	Hard	
given a list of rectange check if they form a perfect rectangle.
- total area = covered rect area.
- only 4 outside points appear once, inner all appear even times.
->>

<<-396	Rotate Function    		36.5%	Medium	
f(0)=A[0]*0+A[1]*1+....+(n-1)*A[n-1]
f(k)=A[k]*0+A[k+1]*1+....+(n-1)*A[(n-1+k)%n]
f(k-1)=A[k-1]*0+A[k]*1+....+(n-1)*A[(n-1+k-1)%n]
f(k)-f(k-1)=A[0]+A[1]+...+A[n-1]-(n-2)*A[(n-1+k)%n]
->>

<<-400	Nth Digit    		32.3%	Medium	
find the nth digit of the infinite sequence 1,2,3....
math: 0: 1 digit, 1-9: 9 digits, 10-99: 90....
->>

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
->>

<<-398	Random Pick Index    		57.1%	Medium	
reservoir sampling
->>

<<-423	Reconstruct Original Digits from English    		47.1%	Medium	
english for digits are out of order. reconstruct the digits.
0,2,4,6,8 has unique letter and use them for the count of these digits
3: three, eight, cnt[h]-cnt[g] is the number of 3.
->>

<<-447	Number of Boomerangs    		52.2%	Medium	
given a list of points on xy plane. D(i,j)=D(i,k) the order matters.
- get all distance from one point to other points.
- save into hashmap, dist occurrences. then choose 2 (P(n,2))
->>

<<-458	Poor Pigs    		54.2%	Hard	
N buckets of liquid, and only 1 is poisonous. 
mins_die: if take poisonous, it will die after mins_die
mins_test: total time to test.
you can choose to feed a pig with some chosen buckets instaneously.
return the min number of pigs needed to figure out the poisonous one.

for example: test time=60, min_die=15, we can test only 4 times.
math base problem: let's take a smaller example: 10 samples with only 1 test allowed, what is the min number of pigs needed? let's assume 5 is poisonous.
0000
0001
0010
0011
0100
0101 (poisonous)
0110
0111
1000
1001
first pig: feed all bit0=1 and died  bit0=1
second pig: feed all bit1=1, and live, bit1=0
third pig, feed all bit2=1, and died, bit2=1
fourth pig, feed all bit3=1, and live, bit3=0, and we get 0101=5.
equivalent: number of bits using base (T+1).
this is also a minmax problem, suppose we all miss.
->>

<<-462	Minimum Moves to Equal Array Elements II    		54.1%	Medium	
given an arra, find the min moves to make all elements the same. a move is to increase or decrease an element by 1.
greedy: all moves to median (like meeting place). sort and use two pointer add the difference.
->>

<<-469	Convex Polygon    		37.2%	Medium	
given a list of points, sequentially connected if forms a convex polygon.
p0-p1, p1-p2, two vector cross product and shall have the same orientation.

->>

<<-470	Implement Rand10() Using Rand7()    		45.8%	Medium	
rand7 generate random integer from 1 to 7. use it generate random in [1,10]
Note that rand49() generates a uniform random integer in [1, 49], so any number in this range has the same probability to be generated. Suppose k is an integer in range [1, 40], P(rand49() = k) = 1/49.

   P(result = k)
= P(rand49() = k in the 1st iteration) +
   P(rand49() > 40 in the 1st iteration) * P(rand49() = k in the 2nd iteration) +
   P(rand49() > 40 in the 1st iteration) * P(rand49() > 40 in the 2nd iteration) * P(rand49() = k in the 3rd iteration) +
   P(rand49() > 40 in the 1st iteration) * P(rand49() > 40 in the 2nd iteration) * P(rand49() > 40 in the 3rd iteration) * P(rand49() = k in the 4th iteration) +
   ...
= (1/49) + (9/49) * (1/49) + (9/49)^2 * (1/49) + (9/49)^3 * (1/49) + ...
= (1/49) * [1 + (9/49) + (9/49)^2 + (9/49)^3 + ... ]
= (1/49) * [1/(1-9/49)]
= (1/49) * (49/40)
= 1/40

Implement randM() using randN() when M > N:
Step 1: Use randN() to generate randX(), where X >= M. In this problem, I use 7 * (rand7() - 1) + (rand7() - 1) to generate rand49() - 1.
Step 2: Use randX() to generate randM(). In this problem, I use rand49() to generate rand40() then generate rand10.

Note: N^b * (randN() - 1) + N^(b - 1) * (randN() - 1) + N^(b - 2) * (randN() - 1) + ... + N^0 * (randN() - 1) generates randX() - 1, where X = N^(b + 1).
->>

<<-478	Generate Random Point in a Circle    		38.8%	Medium	
area=pi*r^2, use sqrt(r) for random and angle random.
->>

<<-479	Largest Palindrome Product    		29.3%	Hard	
find the largest palindrome made from the product of two n-digit number. mod 1337
- product of n-digit number could be 2n digits or 2n-1 digits.
- largest 999...9 smallest 10000..0
- from largest to smallest, build palindrome. and then try to find a factor (the larger one)
->>

<<-483	Smallest Good Base    		36.0%	Hard	
given integer n, fidn the smallest base so that n's digit is all 1.
math: n=sum(b^i)=(b^(i+1)-1)/(b-1) just loop from 2 to log2(n).
->>

<<-497	Random Point in Non-overlapping Rectangles    		39.0%	Medium	
first randomly choose a rect using the area as weight (using prefix sum)
then randomly choose a point in the rect.

->>

<<-519	Random Flip Matrix    		37.4%	Medium	
given a 2d binary matrix initial all 0. 
flip will randomly choose one 0 cell and flip its value.
reset will change to all 0
store the 1 cells in hashmap, rand to random pick a cell, if we hit the 1 cell, 
Reservoir sampling
- once we flip 1, the total size reduce 1 (for rand%total)
- then we remapping the 1 cells to the end.
->>

<<-528	Random Pick with Weight    		44.3%	Medium	
given an array, w[i] represent the weight of ith element. randomly pick the index with w[i]/sum(w)
prefix sum and random choose among [0 totalsum] and then binary search to get the index.
->>

<<-537	Complex Number Multiplication    		68.1%	Medium	->>
<<-539	Minimum Time Difference    		51.9%	Medium	
given a list of time string hh:mm, return the min difference between any pair.
sort and then connect the last with the first. if difference >12 hours add 24 hours.
->>

<<-553	Optimal Division    		57.1%	Medium	
a/b/c/d... you can add (), return the string which can get the max result.
a/(b/c/d...) convert all c,d,..into multiplication.
->>

<<-564	Find the Closest Palindrome    		20.1%	Hard	
could be bigger or smaller.
same length
>length 1000...1
<length 9...9
->>

<<-573	Squirrel Simulation    		55.9%	Medium	
a tree, a squirrel, several nuts. squirrel each time get one nut and put under the tree.
return the min distance.
min(dist(squirrel, nut[i]))+2*dist(tree,nut[j]) i!=j the second item is const.
equiv: 
->>

<<-587	Erect the Fence    		36.2%	Hard	
enclose all the trees in xy plane with min length of ropes.
Monotone chain convex hull
Andrew's monotone chain convex hull algorithm constructs the convex hull of a set of 2-dimensional points in {\displaystyle O(nlogn)}{\displaystyle O(nlogn)} time.

It does so by first sorting the points lexicographically (first by x-coordinate, and in case of a tie, by y-coordinate), and then constructing upper and lower hulls of the points in {\displaystyle O(n)}{\displaystyle O(n)} time.

An upper hull is the part of the convex hull, which is visible from the above. It runs from its rightmost point to the leftmost point in counterclockwise order. Lower hull is the remaining part of the convex hull.

->>

<<-592	Fraction Addition and Subtraction    		49.8%	Medium	
store nomi,denomi in vector, gcd and lcm.
->>

<<-593	Valid Square    		43.4%	Medium	
check if 4 points forms a square. 6 side length 4 same, two same.
->>

<<-625	Minimum Factorization    		32.8%	Medium	
given integer n, find the smallest integer b whose digit product=a.
if there is no answer return 0.
factorization of n, and use these factors to form a number.
to get smallest, we shall try largest factor 9 first and put it on the right.
->>

<<-633	Sum of Square Numbers    		32.2%	Medium	
check if c=a^2+b^2, just do search.
->>

<<-645	Set Mismatch    		42.3%	Easy	
given 1 to n, one number is changed to other number and then one is missing, one is repeated twice
find the two number.
- use number as index to mark seen as negative, thus we can find the repeated.
- once the repeated is found we can get the missing using sum and target sum.
- the missing index will not be touched (so we can check the positive one)
->>

<<-660	Remove 9    		53.8%	Hard	
return nth number after removing all 9 in digits.
this is actually a base-9 problem.
->>

<<-672	Bulb Switcher II    		51.0%	Medium	
n bulbs are all on, there are 4 types of operations:
- flip all
- flip all even
- flip all odd
- flip 3k+1 k=0,1...
after performing m operations unknown, what is the number of different bulb status.
we see: 1+2 equivalent to 3, 1+3 equivalent to 2, 2+3 equivalent to 1.
so only 8 cases:
all on,1,2,3,4,1+4,2+4,3+4
->>
<<-710	Random Pick with Blacklist    		32.5%	Hard	
blacklist contains unique integer in range [0,n]. uniform distribution which avoid numbers in blacklist.
remap to the end of the array. generate rand%newLen, if number is in the blacklist, we use the hashmap one.
->>

<<-724	Find Pivot Index    		44.7%	Easy	
its left sum=right sum. math and prefix sum.
->>

<<-753	Cracking the Safe    		51.4%	Hard	
password is a sequene of n digits using 0 to k-1. if a substr matches then unlock
return the min length that guarantee to open the box.
- to guarantee we need have all the combination up to k^n.
- we need to rearrange so that the total length is min.
- gray code similar: start from 00, take the suffix with n-1 length and add from k-1 to 0
n=2, k=2, we start with "00"
then add 1 ->"001"
then add 0 ->"000" but 00 already used, discard.

->>

<<-754	Reach a Number    		35.1%	Medium	
start from 0, on each move you can go let or right. nth move you go n steps.
return the min steps required to reach target.
- take n steps you go 1+2+..+n=n(n+1)/2
- pass the target
- get the difference
- if it is even we flip several steps
- if it is odd, if(n+1) is odd, we add n+1 to become even and then negate one.
   if n+1 is even, we add n+1 and subtract n+2, becomes even.
->>

<<-780	Reaching Points    		30.0%	Hard	
a move: transform (x,y) to (x,x+y) or (x+y,y)
check if we can go from(sx,sy) to (tx,ty).
(x,y)->(x,x+y)...->(x,mx+y)
(x,y)->(x+y,y)...->(x+ny,y)
combine ->(x+ny,mx+y)
Tx=Sx+nSy; ->(Tx-Sx)%Sy=0, n>=0, Tx>=Sx
Ty=mSx+Sy; ->(Ty-Sy)%Sx=0, m>=0, Ty>=Sy

do it reversely:
- reduce to (x,y+kx) or (x+ky,y). 
first format: Tx=x,ty=y+kx, ty>x, ty%tx reduce to y. 
second format: Tx=x+ky, Ty=y, Tx>Ty, Tx%Ty to reduce to x.
->>


<<-781	Rabbits in Forest    		55.1%	Medium	
each rabbit has some color. some subset of rabbits tell you the number of other rabbits have the same color. return the min number of rabbits in the forest.
[1,1,2], 1+1+2+1=5.
the same number can be considered one color.
different number must be different group.
- using hashmap to count groupsize vs number of group. 
- if n%(x+1)==0 we need n/(x+1)*(x+1) rabbits
- if n%(x+1)!=0, we need (n/(x+1)+1)*(x+1) rabbits
->>

<<-789	Escape The Ghosts    		57.8%	Medium	
from (0,0) to (tx,ty), given some ghosts at different position.
return if you can reach the destination.
greedy: all ghost goes to the destination and wait there.
->>
<<-843	Guess the Word    		46.4%	Hard	
given a list of words, each word is 6 char long. each time guess return number of matched chars. 
idea: randomly take one guess, find all those matched same amount of chars and discard others.
->>
<<-793	Preimage Size of Factorial Zeroes Function    		40.4%	Hard	
f(x) represents the trailing zeros for x!
return the number of f(x)=K.
math: number of trailing zeros is dependent on number of factor 5.
find the rule of 0s.
->>

<<-810	Chalkboard XOR Game    		49.1%	Hard	
A and B takes turn taking away a number. if the remaining xor=0, then the player lose.
greedy approach: suppose take away x: x^rem=res, we want rem!=0, rem=res^x, we shall choose one which has a bit different from res.
so we just need to remove one number which is different from res. 
- also need to be even length, since if A is the last step and no number to remove, he loses.
->>

<<-812	Largest Triangle Area    		58.6%	Easy	
given a list of points, return the largest triangle area.
try all possible combination (triangle area using 3 vertices)
->>

<<-829	Consecutive Numbers Sum    		39.3%	Hard	
given N, return the number of ways to write it as a sum of consecutive integers
assuming start with x with k integers: N=x+x+1+...x+k-1=kx+k(k-1)/2
N-k(k-1)/2=kx  or (N-k*(k-1)/2)%k==0
->>

<<-848	Shifting Letters    		44.9%	Medium	
shift[i] will shift 0 to i by shift[i] times. prefix sum and then %.
->>

<<-853	Car Fleet    		43.3%	Medium	
N cars with position[i] and speed[i], the destination is at target.
a car can never pass, after catch it drives at the same speed.
return the number of car fleets arriving the destination.
- calculate the time for each car arriving at target.
- sort by position.
- the behind cars if the time < previous car, change the time to it.
- count number of groups.
->>

<<-858	Mirror Reflection    		59.4%	Medium	
math to extend the wall and unwrap the reflections
->>

<<-866	Prime Palindrome    		25.0%	Medium	
find smallest prime palindrome >=N.
according to input we limit the range to contruct the palindrome.
- all palindrome with even length is not prime (divisible by 11)
- only consider odd length.
->>

<<-878	Nth Magical Number    		28.6%	Hard	
if it is divisible by a or b.
binary search convert to count.
see similar problem with gcd lcm. 1201 Ugly number III.
->>

<<-883	Projection Area of 3D Shapes    		67.9%	Easy	
matrix with each element the height of 1x1x1 cube stack. return the total area projected in 3 planes.
since projection will only refect the largest one, we shall get the max.
->>

<<-888	Fair Candy Swap    		58.6%	Easy	
exchange to make A and B have the same size. simple math.
->>

<<-892	Surface Area of 3D Shapes    		59.4%	Easy	
given a nxn grid, and A[i,j] is the height of 1x1x1 cube stacks.
return the total surface area.
we only need to look its top left and see if overlapped with previous. Then we need subtract both

->>

<<-902	Numbers At Most N Given Digit Set    		32.0%	Hard	
given a digit set, and n, return number of numbers composed only digits from the set <=n.
pretty hard one.
->>

<<-906	Super Palindromes    		32.8%	Hard	
a number is super palindrome if itself is a palindrome also it is square of some other palindrome. find number of super palindrome in range [L,R].
- itself check is trivials
- x*x=n. reverse using x build odd/even length palindrome and then square it, check it is in the range.
- find the x region
->>

<<-923	3Sum With Multiplicity    		35.9%	Medium	
return number of tuples (i,j,k) A[i]+A[j]+A[k]=target. The array includes duplicate (that's why called multiplicity)
- sort and then use 2 pointer.
- use hashmap to suppress the duplicates, then pick any 3 elements and calculate the combination.
->>

<<-963	Minimum Area Rectangle II    		51.5%	Medium	
given a set of points in xy plane, find the min area of rectangle can be formed.
a rectangle can be defined two equal diagonals with the mid point.
we get all paired lines and store its length and mid point in hashmap vs the point.
find the min in all these rects.
->>

<<-939	Minimum Area Rectangle    		51.7%	Medium	
the rect is parallel to the axis.
- sort by x and the sort by y map<int,set<int>>
- check by x.
->>
<<-970	Powerful Integers    		40.0%	Easy	
given x, y, bound. if n==x^i+y^j then n is a powerful integer. find all powerful integer <=bound.
just try x^0,....x^m and combine with y^j and store in set. Do not need loop for each number in range.
(need reverse thinking a bit)
->>

<<-972	Equal Rational Numbers    		41.8%	Hard	
given two string () enclosed repeating part. check if they are equal.
convert to double precision, 20 digits (double only has 16 decimal digits, 52 bits precision)
->>

<<-977	Squares of a Sorted Array    		72.1%	Easy	->>
<<-976	Largest Perimeter Triangle    		58.4%	Easy	->>

<<-985	Sum of Even Numbers After Queries    		60.8%	Easy	->>

<<-1006	Clumsy Factorial    		53.6%	Medium	
given n, use */+- in order repeatedly. return the value.
for example 9*8/7+6-5*4/3+2-1
- approach 1: using a stack
- i*(i-1)/(i-2)=i+1 when i>=5
->>

<<-1012	Numbers With Repeated Digits    		37.8%	Hard	
given N, return number of numbers with at least 1 repeated digit.
equivalent=N-count(number without repeated digits)
combination: 9*8*7...
->>

<<-1015	Smallest Integer Divisible by K    		32.6%	Medium	
number shall be all 1s. find the smallest number length.
pigeon hole principle: we only need to check those number from length=1 to K. N%K has K values from 0 to K-1. if 0 does not exist, then two duplicate must appear and this will form a cycle. (only remainder matters).
->>

<<-1017	Convert to Base -2    		59.4%	Medium	
negative base but coefficient is still 0/1. get the coefficient if negative then we need convert to 0,1 and increase n. for example get coefficient -1, we change to 1, we need add 2 on n.
->>
<<-1037	Valid Boomerang    		37.8%	Easy	
check 3 points on a line, math.
->>

<<-1067	Digit Count in Range    		40.4%	Hard	
given an integer d and range [L,R], find the number of occurrence of d.
=count(d,R)-count(d,L)
count(d,n) based on 233. Number of Digit One (logN). do it digit by digit
for example looking for 1, 235
digit 5: xx1,1,11,..91, total 24
->>

<<-233 Number of digit one
count(1,n).
->>

<<-1073	Adding Two Negabinary Numbers    		34.6%	Medium	
negative base.
->>

<<-1088	Confusing Number II    		44.8%	Hard	
number rotate 180, it means upside down and left right reverse. the number shall be valid and different from original.
given N, return number of confusing number.
- can only contain 0,1,6,9,8 (0,1,8 upside down is still the same)
- backtrack
- or: find the number of numbers with only 01689 (for different length) and then minus those symmetric ones.
->>

<<-1093	Statistics from a Large Sample    		49.0%	Medium	
min,max,mean,median,mode, math.
->>

<<-1131	Maximum of Absolute Value Expression    		52.2%	Medium	
given two arrays A and B, return the max value |A[i]-A[j]|+|B[i]-B[j]|+|i-j|
we need assemble A[i] B[i],i together. we can always assume i<j.
so 4 cases:
A[i]-A[j]+B[i]-B[j]+i-j ->(A[i]+B[i]+i)-(A[j]+B[j]+j)
A[i]-A[j]+B[j]-B[i]+i-j ->(A[i]-B[i]+i)-(A[j]-B[j]+j)
A[j]-A[i]+B[i]-B[j]+i-j ->(-A[i]+B[i]+i)-(-A[j]+B[j]+j)
A[j]-A[i]+B[j]-B[i]+i-j ->(-A[i]-B[i]+i)-(-A[j]-B[j]+j)
it is clearly we are looking for the same max and min difference of 4 functions.
->>

<<-1175	Prime Arrangements    		51.5%	Easy	
rearrange 1 to n so that prime is at prime index.
combination: m primes take m slots m! and remainder takes n-m slots with (n-m)!
total is m!*(n-m)!
->>

<<-1185	Day of the Week    		62.5%	Easy	
count days between the date and a specific day known weekday and then %7 to get the weekday.
->>


<<-1189	Maximum Number of Balloons    		61.7%	Easy	
gcd of all characters in balloon.
->>


<<-1201	Ugly Number III    		26.2%	Medium	
ugly number is only divisible by a or b or c.
return the nth ugly number.
- math: how to count: given a number n, count the ugly number n/a+n/b+n/c-n/lcm(a,b)-n/lcm(b,c)-n/lcm(a,c)+n/lcm(a,b,c)
- lcm of multiple numbers lcm(a,b,c)=lcm(a,lcm(b,c)) for example lcm(4,5,6)=lcm(5,lcm(4,6))=60
->>


<<-1232	Check If It Is a Straight Line    		44.3%	Easy	
gcd...
->>

<<-1250	Check If It Is a Good Array    		56.1%	Hard	
given an array, check if you can choose a subset and get ax+by+cz...=1 (x,y,z are subsets, a,b,c are some const)
extended euclid theorem, if (x,y,z) coprime, then it is able to find a,b,c..
->>

<<-1276	Number of Burgers with No Waste of Ingredients    		50.0%	Medium	
elementary math solve equation
->>
<<-1330	Reverse Subarray To Maximize Array Value    		35.9%	Hard	
array value is defined as the accumulate absolute difference between adjacent elements. Select a subarray and then reverse it. One operation is allowed. return the max value.
assume we choose a=nums[i-1],b=nums[i],c=nums[j],d=nums[j+1] and we reverse [i,j]
and it becomes a,c...b,d. The value change is |c-a|+|d-b|-|b-a|-|d-c|
abs(a)=max(a,-a)
max(max(c-a,a-c)+max(d-b,b-d)-|b-a|-|d-c|)
we separate to group a and b together
ax(|c-a|+|d-b|-|b-a|-|d-c|)
- max(c-a+d-b-|b-a|-|d-c|) equiv to (-a-b+c+d-|b-a|-|d-c|) for c>a and d>b
- max(c-a+b-d-|b-a|-|d-c|) equiv to (-a+b+c-d-|b-a|-|d-c|) for c>a and d<b
- max(a-c+d-b-|b-a|-|d-c|) equiv to (a-b-c+d-|b-a|-|d-c|) for c<a and d>b.
- max(a-c+b-d-|b-a|-|d-c|) equiv to (a+b-c+d-|b-a|-|d-c|) for c<a and d<b.
we separate (a,b) and (c,d)
there are four cases:
- max(-a-b-|b-a|+c+d-|d-c|)
- max(-a+b-|b-a|+c-d-|d-c|)
- max(a-b-|b-a|-c+d-|d-c|)
- max(a+b-|b-a|-c-d-|d-c|)
two edge case the left and right. we do not have i-1 or i+1 item.

from cs perspective more simpler:
- case 1: reverse 0 to i.
- case 2: reverse i to n-1
- case 3: |c-a|+|d-b|-|b-a|-|d-c| can get max(a,b) and min(a,b) looping and gain 2*(max-min)
->>

<<-1344	Angle Between Hands of a Clock    		61.3%	Medium	
math and unwrap.
->>

<<-1362	Closest Divisors    		57.3%	Medium	
math start from sqrt(n+1) or sqrt(n+2) find the first factor.
->>

<<-1390	Four Divisors    		38.8%	Medium	
straightforward, check all divisors.
math
->>

<<-1401	Circle and Rectangle Overlapping    		42.3%	Medium	
a circle is given by (xc,yc,r) and rectangle is axis aligned given by bottom left and up right point. check if they overlaps.
- overlap: check all points on the rect boundary to the center of circle.
- circle inside rect.
- rect inside circle.
->>

<<-1413	Minimum Value to Get Positive Step by Step Sum    		65.1%	Easy	
simple math.
->>

<<-1422	Maximum Score After Splitting a String    		55.9%	Easy	
binary string, split into two strings, your score is left0+right1.
simple math.
->>

<<-1492	The kth Factor of n    		65.9%	Medium	
brutal force: save all factors in array
optimization: counting only, d and n/d two loops, first go up, second go down.
->>

<<-1515	Best Position for a Service Centre    		36.5%	Hard	
given a list of points on 2d grids, find the location so that the sum of distance to these points is minimzed.
start with an initial location and dx,dy, try to go the 4 direction and see which direction to go.
reduce the step size by half each iteration.
math, numerical analysis
->>

<<-1447	Simplified Fractions    		61.6%	Medium	
gcd.
->>
<<-1453	Maximum Number of Darts Inside of a Circular Dartboard    		34.7%	Hard	
given a list of points on 2d plane, return the max number of points inside a circle of radius r.
- any pair of points can establish two circles (clockwise or counter-clockwise)
- then check number of points inside.
->>

<<-1465	Maximum Area of a Piece of Cake After Horizontal and Vertical Cuts    		31.3%	Medium	
simple math, find the minw and minh and get the product.
->>

<<-1497	Check If Array Pairs Are Divisible by k    		40.3%	Medium	
length of the array is even, divide into pairs, so each pair sum is divisible by k.
hashmap: count remainder's histogram. then use two pointer to check
->>

<<-1622	Fancy Sequence    		15.8%	Hard	
append, addAll, multAll, getIndex
lazy evaluation.
modular for division
math, lazy evaluation.
modular inverse: Fermat Little Theorem
if p is a prime, for any integer a^p-a is a multiple of p.
a^(p-1)-1 is a multiple of p. or a^(p-1)=1 (mod p)
->>

<<-1620	Coordinate With Maximum Network Quality    		37.7%	Medium	
given a list of signal tower on plane (x,y,q).each tower has a radius. The signal q/(1+d), d is the distance from the tower. find the grid having the max signal.
- the region is bound by the tower. so we can get a rect area and check each grid.
->>

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
amazon OA.
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

### trivials & straightforward
<<-693	Binary Number with Alternating Bits    		59.6%	Easy	
compare with prev bit->>

<<-716	Max Stack    		42.8%	Easy	->>
<<-709	To Lower Case    		79.8%	Easy	->>

<<-728	Self Dividing Numbers    		75.0%	Easy	->>

<<-744	Find Smallest Letter Greater Than Target    		45.6%	Easy	->>

<<-748	Shortest Completing Word    		57.1%	Easy	->>
<<-747	Largest Number At Least Twice of Others    		42.6%	Easy	->>
<<-760	Find Anagram Mappings    		81.5%	Easy	->>

<<-762	Prime Number of Set Bits in Binary Representation    		63.8%	Easy	
find number of integers in [L,R] which has a prime number of set bits.
count bits and see if they are prime (limited to 32)
->>

<<-766	Toeplitz Matrix    		65.6%	Easy	->>
<<-771	Jewels and Stones    		86.7%	Easy	->>

<<-788	Rotated Digits    		57.3%	Easy	
rotate 180 degrees and number is still valid and different from original.
given n, return the number of good numbers.
loop and check each.
->>

<<-800	Similar RGB Color    		61.9%	Easy	->>

<<-804	Unique Morse Code Words    		77.5%	Easy	->>
<<-806	Number of Lines To Write String    		65.3%	Easy	
given width of 26 chars, each line is 100 pixels wide. 
return number of lines.
->>

<<-811	Subdomain Visit Count    		70.7%	Easy	->>

<<-819	Most Common Word    		45.2%	Easy	->>

<<-824	Goat Latin    		66.2%	Easy	->>
<<-830	Positions of Large Groups    		50.0%	Easy	->>
<<-832	Flipping an Image    		77.6%	Easy	
flip horizontally and invert.
->>

<<-836	Rectangle Overlap    		46.3%	Easy	->>
<<-852	Peak Index in a Mountain Array    		71.8%	Easy	->>
<<-860	Lemonade Change    		51.9%	Easy	->>

<<-868	Binary Gap    		60.8%	Easy	
longest distance between 2 1s.
->>
<<-867	Transpose Matrix    		62.3%	Easy	->>
<<-896	Monotonic Array    		58.0%	Easy	->>
<<-933	Number of Recent Calls    		71.9%	Easy	->>
<<-929	Unique Email Addresses    		67.2%	Easy	->>
<<-925	Long Pressed Name    		39.0%	Easy	->>
<<-922	Sort Array By Parity II    		70.0%	Easy	->>
<<-917	Reverse Only Letters    		58.4%	Easy	->>
<<-914	X of a Kind in a Deck of Cards    		34.5%	Easy	->>

<<-989	Add to Array-Form of Integer    		44.5%	Easy	->>

<<-997	Find the Town Judge    		49.9%	Easy	->>
<<-1002	Find Common Characters    		67.8%	Easy	->>
<<-1018	Binary Prefix Divisible By 5    		47.7%	Easy	
keep %5 to prevent overflow
->>

<<-1021	Remove Outermost Parentheses    		78.5%	Easy	->>

<<-1047	Remove All Adjacent Duplicates In String    		69.7%	Easy	
simple stack
->>
<<-1046	Last Stone Weight    		62.4%	Easy	
simple stack
->>

<<-1051	Height Checker    		71.8%	Easy	->>

<<-1056	Confusing Number    		47.6%	Easy	
check if it's confusing number.
->>

<<-1085	Sum of Digits in the Minimum Number    		74.8%	Easy	->>
<<-1086	High Five    		79.9%	Easy	->>
<<-1108	Defanging an IP Address    		88.2%	Easy	->>
<<-1119	Remove Vowels from a String    		90.2%	Easy	->>
<<-1118	Number of Days in a Month    		57.3%	Easy	
again check for leap year.
->>

<<-1128	Number of Equivalent Domino Pairs    		46.8%	Easy	->>
<<-1133	Largest Unique Number    		67.1%	Easy	->>

<<-1134	Armstrong Number    		78.0%	Easy	
k is the number of digits, so that sum of digit's k power equal the number.
->>

<<-1137	N-th Tribonacci Number    		56.2%	Easy	->>

<<-1160	Find Words That Can Be Formed by Characters    		67.3%	Easy	->>

<<-1165	Single-Row Keyboard    		84.6%	Easy	->>

<<-1170	Compare Strings by Frequency of the Smallest Character    		59.3%	Easy	->>

<<-1180	Count Substrings with Only One Distinct Letter    		77.2%	Easy	->>

<<-1184	Distance Between Bus Stops    		54.3%	Easy	->>

<<-1196	How Many Apples Can You Put into the Basket    		67.9%	Easy	->>

<<-1207	Unique Number of Occurrences    		71.7%	Easy	->>

<<-1228	Missing Number In Arithmetic Progression    		52.0%	Easy	->>

<<-1252	Cells with Odd Values in a Matrix    		78.3%	Easy	
give (i,j) you increase the row i and col j all by 1.
simple, use + or xor
->>

<<-1271	Hexspeak    		54.9%	Easy	->>

<<-1281	Subtract the Product and Sum of Digits of an Integer    		85.5%	Easy	->>

<<-1290	Convert Binary Number in a Linked List to Integer    		81.9%	Easy	->>

<<-1295	Find Numbers with Even Number of Digits    		80.0%	Easy	->>

<<-1304	Find N Unique Integers Sum up to Zero    		76.4%	Easy	->>

<<-1313	Decompress Run-Length Encoded List    		85.2%	Easy	->>

<<-1317	Convert Integer to the Sum of Two No-Zero Integers    		56.8%	Easy	
no integer can contain digit '0'. just do brutal force.
->>

<<-1332	Remove Palindromic Subsequences    		62.7%	Easy	
string consists of a and b only. return the min step to make it empty.
0: empty
1: palindrome
2: takes all a and all b.
->>

<<-1342	Number of Steps to Reduce a Number to Zero    		85.9%	Easy	->>
<<-1346	Check If N and Its Double Exist    		36.7%	Easy	
hashmap check n/2 or n*2 but make sure n is even to check its half.
->>

<<-1357	Apply Discount Every n Orders    		66.3%	Medium	
hashmap.
->>
<<-1356	Sort Integers by The Number of 1 Bits    		69.4%	Easy	
get the nbits and sort.
->>

<<-1365	How Many Numbers Are Smaller Than the Current Number    		85.8%	Easy	
count sort, sort->>

<<-1380	Lucky Numbers in a Matrix    		71.1%	Easy	
find all the numbers which is the min value of the row and column.
->>
<<-1385	Find the Distance Value Between Two Arrays    		66.6%	Easy	->>
<<-1389	Create Target Array in the Given Order    		84.4%	Easy	->>

<<-1394	Find Lucky Integer in an Array    		63.2%	Easy	
its frequency=its value. simple.
->>
<<-1399	Count Largest Group    		65.2%	Easy	
given 1 to n, they are grouped by the sum of digits, return the number of largest group
hashmap.
->>
<<-1417	Reformat The String    		55.6%	Easy	
letter and digit, rearrange so that no same type together.
len(a)==len(b)
len(a)=len(b)+1
->>

<<-1431	Kids With the Greatest Number of Candies    		88.6%	Easy	->>
<<-1470	Shuffle the Array    		88.5%	Easy	->>
<<-1476	Subrectangle Queries    		89.0%	Medium	->>
<<-1480	Running Sum of 1d Array    		89.7%	Easy	->>

<<-1491	Average Salary Excluding the Minimum and Maximum Salary    		68.9%	Easy	->>

<<-1502	Can Make Arithmetic Progression From Sequence    		71.2%	Easy	->>

<<-1518	Water Bottles    		61.3%	Easy	
numExchange of empty bottles for one bottle of water.
keep track of empty bottles and drink bottles.

->>
<<-1523	Count Odd Numbers in an Interval Range    		55.3%	Easy	->>

<<-1528	Shuffle String    		85.8%	Easy	->>

<<-1544	Make The String Great    		54.8%	Easy	
simple stack.
->>
<<-1694. Reformat phone number.
get rid of non-digits and then rearrange.
->>

<<-1550	Three Consecutive Odds    		65.9%	Easy	->>

<<-1556	Thousand Separator    		58.8%	Easy	
add a . every thousand.
->>

<<-1566	Detect Pattern of Length M Repeated K or More Times    		42.0%	Easy	
just check A[i]==A[i+j*M] j from 1 to k.
->>
<<-1570	Dot Product of Two Sparse Vectors    		91.5%	Medium	->>

<<-1598	Crawler Log Folder    		64.4%	Easy	
simple stack op.
->>
<<-1603	Design Parking System    		86.1%	Easy	->>

<<-1619	Mean of Array After Removing Some Elements    		66.3%	Easy	
remove the top and bottom 5% and get the remaing mean value
sort and sum the middle parts.
->>

<<-1624	Largest Substring Between Two Equal Characters    		59.4%	Easy	
save the first index for each char seen. then find the max length.
hashmap or array.
->>

<<-1646	Get Maximum in Generated Array    		48.2%	Easy	
directly generate the array according to the rule
->>

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
zero sum game:
, a zero-sum game is a mathematical representation of a situation in which each participant's gain or loss of utility is exactly balanced by the losses or gains of the utility of the other participants. If the total gains of the participants are added up and the total losses are subtracted, they will sum to zero
In contrast, non-zero-sum describes a situation in which the interacting parties' aggregate gains and losses can be less than or more than zero. A zero-sum game is also called a strictly competitive game while non-zero-sum games can be either competitive or non-competitive. Zero-sum games are most often solved with the minimax theorem which is closely related to linear programming duality,[1] or with Nash equilibrium.
linear programming
NP-hard: non-deterministic polynomial time hardness.

some games especially zero-sum game
<<-294	Flip Game II    		50.4%	Medium	
two players take turn to flip consecutive "++" into "--", the game ends when play cannot move.
determine if the first player can win.
backtrack+memoization。
record the win string using hashmap. if other party lose, we win.
->>
<<-293	Flip Game    		61.0%	Easy	
get all possible states after one valid move.
simple change all ++.
->>
<<-292	Nim Game    		54.9%	Easy	
by turn you can take 1-3 stones from the heap. The one who removes the last wins.
n=4, lose, 1-3 win, n>4 can reduce to n<=4
greedy.
->>
<<-464	Can I Win    		29.4%	Medium	
given 1 to MaxN and desiredTotal, cannot reuse the integers. Who first causes the total >=desiredTotal wins. check if the first winner wins.
backtracking or dp. store bitmask for win or lose for the first player. 
->>

<<-486	Predict the Winner    		48.3%	Medium	
dp strategy game.
pick one number from either end
->> 

<<-877	Stone Game    		66.0%	Medium	
by turn take either end. dp problem.
->>
<<-1140	Stone Game II    		64.9%	Medium	
A and B takes turn. Initially M=1, each time you can take 1<<x<<2M piles of staones. then we set M=max(M,x).
dp: top down, max(A-B), strategy game.
->>

<<-1406	Stone Game III    		56.8%	Hard	
strategy game: you can take up to 3 piles of stones from the beginning. The sum of stones is the score.
dp: take 1,2 or 3 stones and let the other player negative, max our score.
->>

<<-1510	Stone Game IV    		58.5%	Hard	
A and B by turn take a nonzero square number of stones, if one cannot make a move, lose.
dp: dp[i]=1 if dp[i-j*j]=0
->>

<<-679	24 Game    		47.0%	Hard	
we have 4 digits, using +-*/() to get value of 24.
stack problem.
->>

<<-1510 Stone Game IV    		58.6%	Hard	
competitive: take any square number, who can not make a move, lose. DP
dp: if other side lose, then we win. dp[i]=!dp[i-j*j].
```
    bool winnerSquareGame(int n) {
        //dp[n]-- dp[n-j*j], check if any will give win result
        vector<bool> dp(n+1);
        if(n<2) return 1;
        dp[1]=1;//leave 1 stone for other play, we will lose
        
        for(int i=2;i<=n;i++){
            for(int j=1;j*j<=i;j++){
                if(dp[i-j*j]==0){ //otherside lose, we win.
                    dp[i]=1;
                    break;
                }
            }
        }
        return dp[n];
        
    }
```	
->>

<<-1025 Divisor Game    		66.1%	Easy	
competitive: choose any factor x (not include N itself) of N, and perform N-x, who cannot make a move, lose.
greedy: 1 will lose, 2 will win. if N is even, we can always make it odd for the other by minus 1. If it is odd, its factor are all odd, and leave even for other, always lose.
->>

<<-837	New 21 Game    		35.3%	Medium	
actually not a game, but a dp with sliding window problem.
->>

<<-877	Stone Game    		66.1%	Medium	
even piles, total sum is odd. take stone from either side. who takes more win.
greedy: odd index sum !=even index sum. take the odd/even index stones.
->>

<<-1563 Stone Game V    		40.1%	Hard
A will divide into 2 parts, throw away the part with larger sum and the remaing sum is the points. If two parts are the same, we can choose which one to throw away. Return the max points we can get.
straightforward dp
->>

<<-1686. Stone game IV
problem: stone are valued differently, A will take first. Who will win.
greedy approach: suppose we have two stones (a,b),(c,d).
if A take a, then B take d. difference a-d
if A take c, then B take b. difference c-b
which A shall take? we shall maximize difference a-d>c-b->a+b>c+d.
so we sort by a+b.
->>

<<-682	Baseball Game    		65.6%	Easy	
simple stack problem, not a game
->>

<<-293	Flip Game    		61.1%	Easy	
convert ++ to --, generate all possible next state string.
->>

<<-294	Flip Game II    		50.5%	Medium	
convert ++ to -- by turn, if you cannot make a move, you lose. Return who is the winner.
backtracking and memoization dp approach.
->>

<<-292	Nim Game    		54.9%	Easy	
by turn take 1-3 stones, who take the last stone win. Decide who is the winner.
greedy or math: if n%4==0, it will leave 4n+1,4n+2,4n+3 for the other player, and the other player will always leave 4n to us, then we will lose. n%=4!=0 we can always leave 4n to the other player.
->>

<<-1140	Stone Game II    		64.8%	Medium	
a list of stone, M=1, each time the player can take x=[1,2M] piles and then set M=max(M,x). Return the max stone the first player can take.
dp: prefix sum or suffix sum to get the range sum. memoization.
->>

<<-488	Zuma Game    		38.8%	Hard	
a row of balls with color RYBGW and you have some balls in hand. Each time you can insert one ball into the row and remove groups of same color balls with n>=3. Return the min balls needed or return -1 if you cannot do it.
backtracking: try all possible positions.
->>

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
using hashmap to record same values indices. bfs for min problem.
if all values are the same, will TLE. after using the hashmap, clear it is critical to avoid TLE.
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
each turn remove the left or rightmost stone, and the score is the sum of the remaining.
minimize the difference.
approach: maximize the score for a and b. applying prefix sum and get the scores easily and choose left or right and solve two smaller subproblem.
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

<<-1535. Find the winner of an array game
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


## subject: linear programming 线性规划
linear programming is to get max or min under some inequality restrictions.
<<-1626	Best Team With No Conflicts    		36.7%	Medium	
also see LIS ->>

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

<<-1642	Furthest Building You Can Reach    		54.7%	Medium	
Given an array of building heights, and some bricks and ladders. Find the furthest building you can reach.
- binary search using greedy.
- use brick first, when brick is not enough, replace the one using most bricks with a ladder.
- use ladder first, when ladder is used up, replace a ladder with min bricks needed.
->>

## graph
<<-261	Graph Valid Tree    		42.7%	Medium	
given n nodes and a list of undirected edges, check if it forms a valid tree.
a tree: n nodes and n-1 edges. no cycle. union find. 
->>

<<-269	Alien Dictionary    		33.5%	Hard	
alien language order is unknown. given a list of dictionary words sorted in alien language.
derive the order from the dictionary words.
- according to lexi rule create the graph and counting indegree
- then use bfs to remove all source nodes first.
->>

<<-310	Minimum Height Trees    		34.3%	Medium	
bfs to remove leaf nodes layer by layer.
->>

<<-399	Evaluate Division    		53.5%	Medium	
given a list of division and return the query.
dfs or graph. 
->>

<<-444	Sequence Reconstruction    		23.2%	Medium	
the original array is a permutation of 1 to n. check if given a sequence if it is uniquely constructable using the sequence. (the only one sequence can be constructed)
reconstruction means building a shortest common supersequence (all sequence is a subsequence of the supersequence).
graph problem topology sort.
- first, construct the dependency graph using seqs
- try topological sorting on the dependency graph
- during each step, check whether there is only one option to select the node
if there is more than one options, return False directly
- after getting the topological sorted node list, check whether its length is the same with number of distinct values and whether it's the same with org
- grpah shall keep all nodes
- find source node and dest node. (bfs or dfs are both fine)
please note given seq is not direct connection edge, but an order.
so we can only use indegree.
????
->>

<<-547	Friend Circles  (Number of provinces)  		59.5%	Medium	
graph dfs. graph connection is given as a NxN matrix.
for each node, we need do dfs on the row. 
->>

<<-742	Closest Leaf in a Binary Tree    		44.0%	Medium	
tree has unique value, given a target value which exist in the tree. find the leaf node which is closest to it.
- find the target node and then do a bfs starting on this node, the first leaf.
- recursive: find the node and dfs to get the leaf node depth = min depth.
basically it is two problem: find the node and find the min-depth (note you need rebuild the tree with root=k) or get parent's other subtree distance (shall do recursively since ancestor may have shorter distance)
->>

<<-785	Is Graph Bipartite?    		48.0%	Medium	
dfs coloring
->>
<<-802	Find Eventual Safe States    		49.3%	Medium	
given a directed graph as an adjacency matrix.
a node is safe if starting from it we reach a terminal.
return the list of safe nodes.
all paths shall lead to the terminal, if one fail then fail. cycle detection
->>

<<-851	Loud and Rich    		52.2%	Medium	
n people with different money and quietness[i]. We are given a list of compare richer[i]=[x,y] means x is richer than y.
return answer[i] the least quite person who is richer than person i.
- build a graph
- start dfs from each node and save the min quiteness 
->>

<<-863	All Nodes Distance K in Binary Tree    		56.8%	Medium	
form a graph and bfs from the node.
->>

<<-1042	Flower Planting With No Adjacent    		48.4%	Medium	
n gardens, given a list of edges (bidirectional). In each garden plant one type of 4 types of flowers.
all garden have <=3 paths into or leaving it.
connected garden shall have different color.
return any choice of combination.
graph: confusing.greedy paint nodes one by one.????
->>

<<-1059	All Paths from Source Lead to Destination    		43.3%	Medium	
check if all path from src leads to destination.
dfs or bfs on graph.
->>

<<-1168	Optimize Water Distribution in a Village    		62.1%	Hard	
given n houses in a village. Each house can be provided water via well or pipe.
each house either build a well in it and connect a pipe from another well.
well cost are given as an array and pipe cost is given by edges [i,j,cost]
return the min total cost.
consider dig a well the cost to connect a pipe to a virtual node, and then problem is reduce to MST.
using greedy union find the get the min cost.
->>

<<-1192	Critical Connections in a Network    		49.4%	Hard	
critical edge is not in a cycle.
just discard all those edges involved in a cycle.
- build the adjacency matrix.
- assign all node unvisited -2 (we need use t-1 for parent)
- dfs: define rank/time stamp, so that node shall have rank 0 to n-1. 
- how to avoid visiting cycles infinitely:
we first mark the node being visited an max n.
- if the node is visited, return its rank, this rank <= assigned rank. so it is a cycle
- mark the node in a cycle to n so you don't need to visit it again and again.

->>

<<-1203	Sort Items by Groups Respecting Dependencies    		48.6%	Hard	
graph topology sort, do not understand at all.
->>

<<-1311	Get Watched Videos by Your Friends    		43.8%	Medium	
n people from 0 to n-1. 
array WatchedVideos[i] are the list of videos watched by ith person
array friend[i] are the list of friends for ith person.
given an id, and k, return the videos watched by id's k-level friend, sorted descendently by frequency.
- build a graph
- bfs to kth level and get all the videos and store in hashmap.
->>

<<-1334	Find the City With the Smallest Number of Neighbors at a Threshold Distance    		46.0%	Medium	
given n cities and bidirectional edge [a,b,w] with weight.
Return the city with the smallest number of cities that are reachable through some path and whose distance is at most distanceThreshold, If there are multiple such cities, return the city with the greatest number.
- loop over each city and find number of reachable cities under threshold.
- dijkstra to try the closest neighbors first.
->>

<<-1368	Minimum Cost to Make at Least One Valid Path in a Grid    		55.8%	Hard	
given a matrix with 1,2,3,4 representing right,left,down,up.
you can modify a sign with cost 1. Return the min cost to make a valid path from top left to bottom right.
 if we can go from current cell to next cell, the cost is 0
 if we cannot go from current cell to next cell, the cost is 1.
 Equivalent problem is then find the shortest cost path from (0,0) to (m-1,n-1)
 dijkstra O(E+VlogV) using priority_queue
 01 BFS: O(E+V) using deque. 0 cost is added in queue front, 1 cost is added to queue back, so we try 0 first. queue saves the position and cost to this position. (A simplified dijkstra)
->>

<<-1376	Time Needed to Inform All Employees    		56.2%	Medium	
employees form a tree structure. array manager gives each employee's manager, -1 indicates he is the head.
informTime array is the time needed for employee[i] to inform its subordinates.
the shortest time is the max path time, using adjacency matrix and dfs.
->>

<<-1436	Destination City    		77.2%	Easy	
destination has no outcoming edge.
->>

<<-1466	Reorder Routes to Make All Paths Lead to the City Zero    		61.5%	Medium	
given n cities from 0 to n-1 forming a tree with root 0. 
and the tree is given by a list of directional edges. 
Now we want to reorient the edges and so that each city can go to city 0.
return the min number of edges to be changed.
adjacency matrix record the edge information. and then do a dfs on incoming edge from root. record the missing edges.
->>

<<-1514	Path with Maximum Probability    		38.5%	Medium	
given an undirected weighted graph of n nodes from 0 to n-1. The graph is given by a list of edges, edge weight is the probability. Given two nodes start and end, find the path with max probability.
Bellman-ford will TLE
dijkstra: pq store the possibility and start node, choose the max node and check all its neighbors:
if the neighbor used can increase its probability, then we update and add this node into pq.
->>

<<-1557	Minimum Number of Vertices to Reach All Nodes    		74.3%	Medium	
greedy: only source nodes are not reachable
->>

<<-1584	Min Cost to Connect All Points    		49.0%	Medium	
a list of 2d points with integer coordinates, the cost of connecting two points is the manhattan distance |x1-x2|+|y1-y2|. return the min cost to make all points connected.
greedy: connect shortest distance first, and then use union-find. Minimum Spanning Tree.
there are N^2 edges. using heap will be faster here since we do not use all the edges.
dijkstra: using pq to try smallest edge first. Use the fact that we only need n-1 edges.
->>

<<-1615	Maximal Network Rank    		51.3%	Medium	
n cities from 1 to n. given a list of edges, indicating there is a bidirectional road between two cities.
network rank of two cities: the total incoming edges of two cities. If there is direct connection, it is only counted once.
return the max rank (for all possible pairs)
- set to store the edges
- calculate each node's incoming edges
- count all pairs of cities rank.
->>

<<-1617	Count Subtrees With Max Distance Between Cities    		63.0%	Hard	
n cities from 1 to n forming a tree. the tree is represented by a list of edges (bidirectional).
a subset is a list of nodes which are connected without outside nodes. n<15
find the number of subsets which the max distance in the subset is d.
idea: calculate any two pair distance using dp or bellman
try all combination states (using bitmask) and calculate the distance in the subset (you must use the nodes in the subset). Make sure judge if it is a valid subset (nnode=nedge+1)
->>

## intervals
<<-57	Insert Interval    		34.5%	Medium	->>
<<-56	Merge Intervals    		40.2%	Medium	->>

<<-218	The Skyline Problem    		35.1%	Hard	
binary index tree (query and update)
linesweeping: 
interval: store position vs height add/removal in map. We actually look for the maxh at each location. if same as previous we ignore it.
- sort by x
- get the max
 maintain a map (left add, right remove, add use negative, remove use positive)
 from left to right, maintain a multiset of left heights.
 add removal the height using the map.
 https://leetcode.com/problems/the-skyline-problem/discuss/61273/C%2B%2B-69ms-19-lines-O(nlogn)-clean-solution-with-comments
->>

<<-352	Data Stream as Disjoint Intervals    		48.1%	Hard	
represent the data stream as a list of disjoint interval.
interval merge: add a data mp[n]++, mp[n+1]-- and then using prefix sum.
->>

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

<<-370	Range Addition    		63.2%	Medium	
given a list of range to add [i,j,inc]. return the final array.
interval add/remove event.
->>

<<-436	Find Right Interval    		48.2%	Medium	
right interval is the start>=previous end, and the smallest one.
find each intervals right index, if no return -1.
sort interval by start with index.
->>
<<-435	Non-overlapping Intervals    		43.7%	Medium	
return min number of intervals to remove to make them non-overlapping.
equivalent to longest non-overlapped interval.
greedy: always choose the one with smaller ending.
->>

<<-452	Minimum Number of Arrows to Burst Balloons    		49.7%	Medium	
given a list of balloon with [start,end], return the min number of arrows to burst all balloons
greedy: sort by end, hit the smallest end will eliminate all those balloons with start<=end.

->>

<<-495	Teemo Attacking    		56.0%	Medium	
interval merging
->>

<<-699	Falling Squares    		42.2%	Hard	
a list of squares [xi,leni] with x position and side length. Touched squares will glue together.
return a list of height at position i (left max).\
similar to skyline problem, using interval.
Similar to skyline concept, going from left to right the path is decomposed to consecutive segments, and each segment has a height. Each time we drop a new square, then update the level map by erasing & creating some new segments with possibly new height. There are at most 2n segments that are created / removed throughout the process, and the time complexity for each add/remove operation is O(log(n)).
- map to record the interval
- find the range max height
- erase the intervals in the range
- add the new interval with new height.
->>

<<-554	Brick Wall    		50.4%	Medium	
find the min intersection drawing a vertical line (excluding the last).
interval: intervals are sorted. prefix sum and count the presum's occurrances. The max one is the answer.
->>

<<-218. The Skyline Problem
->>
<<-715	Range Module    		39.5%	Hard	
track interval.
->>

<<-732	My Calendar III    		60.9%	Hard	
k booking.
->>
<<-731	My Calendar II    		49.8%	Medium	
avoid triple booking
->>
<<-729	My Calendar I    		52.7%	Medium	
check if double booking
->>

<<-757	Set Intersection Size At Least Two    		41.6%	Hard	
interval [a,b] contains all integer from a to b.
given a list of intervals, return the min size of set s so that the set intersect with each interval with at least two numbers.
- sort by end. first need takes the end 2,
- we use two numbers p1,and p2 to track, add 0, add 1, add 2.
->>

<<-759	Employee Free Time    		67.4%	Hard	
given a list of interval reprresenting the working time for each employee.
return a list of common interval freetime.
->>

<<-763	Partition Labels    		77.7%	Medium	
partition to as many parts as possible so each char only apprear in at most one part.
similar to interval: record the last index, update the curmax and when we see the index==curmax, we know there is no more.
->>

<<-850	Rectangle Area II    		48.2%	Hard	
given a list of rect defined by bottom left and top right points, all rects aligned with axis.
return the total area covered.
- 2d interval overlap, loop on x and interval on y direction.
- sort by x, if x the same then by y.
- and then loop on x and connect to previous x for the area.
the y segment is added at left x and removed at right x. so we set a add/remove flag to remove the segment.
->>

<<-986	Interval List Intersections    		67.8%	Medium	
merge the intersection, two pointer on intervals.
->>

<<-1094	Car Pooling    		59.0%	Medium	->>

<<-1109	Corporate Flight Bookings    		53.8%	Medium	
n flights labeled from 1 to n. a list of array in the format [i,j,k] means from flight i to flight j we booked k seats. return each flight's booking number
interval event start/end.
->>

<<-1229	Meeting Scheduler    		53.8%	Medium	
find the common available time interval >=d.
interval overlap 
->>

<<-1272	Remove Interval    		57.8%	Medium	
given a list of intervals and a interval to be removed. return the list of intervals.
sort by start and check each interval.
->>

<<-1288	Remove Covered Intervals    		57.1%	Medium	
sort the intervals using start. then an interval with end < curr end, it is covered.
->>

<<-1419	Minimum Number of Frogs Croaking    		46.9%	Medium	
interval overlapping. 
first count croak's occurences. C means a start K means end. then get the max.
->>

<<-1701. Average waiting time
given customer arrival time and order preparing time. return the average waiting time.
simulate the services
->>

<<-1450	Number of Students Doing Homework at a Given Time    		77.1%	Easy	
- brutal force checking all intervals
- map and using event start, exit and prefix sum.
->>

<<-1520	Maximum Number of Non-Overlapping Substrings    		34.8%	Hard	
if the substring contains c, it must contain all occurences of c.
if there are multiple answers, return the one with min total length.
- find each char's first and last index.
- extend range using inside chars. -this is very tricky. (once we found the interval no change the we stop)
- sort intervals and get the max non-overlapping substrings. always uses smaller segment first.
Very hard.
->>

<<-1546	Maximum Number of Non-Overlapping Subarrays With Sum Equals Target    		43.5%	Medium	
two problems here:
- find all subarrays with sum==target, this can be done using hashmap.
- find the longest non-overlapping intervals. sort by end and connect.
->>

## recursive
<<-679	24 Game    		46.9%	Hard	
given 4 number check if you can use +-*/() to reach 24.
division is real division not integer division.
- no order
- idea: pick any 2 numbers and do +-*/ and then reduce to 3 numbers
->>

<<-880	Decoded String at Index    		24.5%	Medium	
digit in the string d means the previous string will be copied d-1 times.
return the char at index. (note a23 mean a is repeated 2 time becoming aa3 and then aa is repeated 3 times.)
We decode the string and N keeps the length of decoded string, until N >= K.
Then we go back from the decoding position.
If it's S[i] = d is a digit, then N = N / d before repeat and K = K % N is what we want.
If it's S[i] = c is a character, we return c if K == 0 or K == N
???
->>

<<-1274	Number of Ships in a Rectangle    		65.6%	Hard	
given api hasShip(topRight, bottomLeft), reduce size until one single point
recursive.
->>

<<-1414	Find the Minimum Number of Fibonacci Numbers Whose Sum Is K    		63.5%	Medium	
recursive, find the number most close to K and reduce to a small problem.
->>

<<-1415	The k-th Lexicographical String of All Happy Strings of Length n    		69.9%	Medium	
string only consists of a,b,c. happy string has no same char adjacent. return the kth happy string.
- approach 1: backtracking. try a b c in order.
- approach 2: math, to build string of size n, 3*2^(n-1) (the first char has 3 options, then two options for all). using recursive, k<2^(n-1) first char is 'a'. k<2^n, first char is b.
->>

<<-1545	Find Kth Bit in Nth Binary String    		57.0%	Medium	
S1="0"
Si=Si-1+"1"+reverse(invert(Si-1)) for i>1. 
find the kth bit in Nth binary string.
- derive the length: len(i)=2*len(i-1)+1: 1,3,7,15,... 2^(n-1)
- if it is the mid, left or right, then two subproblem
recursive approach.
->>

## trie
trie is essentially the n-arry tree. generally used for prefix or suffix string.
bits are often using trie for xor, and...

<<-208	Implement Trie (Prefix Tree)    		50.9%	Medium	->>
<<-211	Design Add and Search Words Data Structure    		39.2%	Medium	->>

<<-336	Palindrome Pairs    		34.2%	Hard	
given a list of words, return all the pairs so that word[i]+word[j] is palindrome.
- brutal force.
- trie: add reversed string into trie. 
since a suffix match cannot guarantee the concat to be palindrome, so we need to also check curremt to end if a palindrome.
->>

<<-421	Maximum XOR of Two Numbers in an Array    		53.7%	Medium	
- from MSB to LSB, each time to leave the bits, we only have one or two numbers. 
- keep the max prefix to xor with the current bits (make sure the number exist)
- trie. [8,10,2] binary [1000,1010,0010]
       x
     /   \
    0     1  ->1
   /     /
  0     0    ->0
   \   / \
    1 0   1  ->1
   / /   /
  0 0   0    ->0
then loop each element and see what's the max xor with the element and get the global max.
O(32n)
->>

<<-588	Design In-Memory File System    		46.3%	Hard	
simulate these functions:
ls: list file or files under directory
mkdir: add a dir. if not exist, create it.
addContentToFile: create or append content.
readContentFromFile: return the file content.
trie: for search and retrieving file. leaf node can be a file or end of directory.
->>

<<-642	Design Search Autocomplete System    		45.5%	Hard	
input sentence ending by #
for each char, return the top 3 hot sentences that the have same prefix as typed
- sort by frequency
- same frequency sort by lex order.
- dfs + pq to sort the top 3 (or no pq, just O(N) to get the top 3)
->>

<<-648	Replace Words    		57.7%	Medium	
root word, successor= root word+some other word
replace all successor with root word, if match more use the root with shortest length.
- using hashmap for root word, and search each word in string char by char
- trie (prefix searching, the first ending match)
->>

<<-676	Implement Magic Dictionary    		55.0%	Medium	
given a list of word, and a string, check if we change exactly one char in the string and can match the dictionary word.
Trie: make a trie on string or dictionary word? we can replace the char to * to represent any char. for the string we only need add n strings inside to trie. and then we use dictionary word to match.
but the search pattern can be changed so we still have to use dictionary word.
- hashmap is also ok.
->>

<<-677	Map Sum Pairs    		53.7%	Medium	
design mapsum and insert (string,value) pairs, 
sum(prefix) sum all the values with key prefix = prefix.
- direct approach: store in hashmap.
- Trie: startWith. when we insert we add the value for each node. so we directly know the sum. (only store value in leaf node will need dfs and will not save time)
note this include a modification function. use the delta (need save a hashmap)
thus no dfs needed.
->>

<<-720	Longest Word in Dictionary    		48.9%	Easy	
longest word which can be built one char from other words in the dictionary.
build a trie and search start with. (sort reversely using length)
->>

<<-745	Prefix and Suffix Search    		34.7%	Hard	
given a list of dictionary words, give prefix string and suffix string, find the word index with given prefix and suffix. If multiple find the one with largest index.
- how to combine prefix and suffix in one search?
- suffix+$+original string, then search pattern suffix+$+prefix.
- same string use the larger index so naturally we get the larger
->>

<<-1032	Stream of Characters    		48.4%	Hard	
given a dictionary, and check if the last several character is in the dictionary
trie: add the dictionary words (reversed) into trie, and then check the reversed stream strings.
->>

<<-1178	Number of Valid Words for Each Puzzle    		38.4%	Hard	
given a list of puzzle strings and a list of words. return number of valid words for each puzzle.
a valid word: it contains the first letter of the puzzle word. all letters shall be in the puzzle word.
- we shall include first char and types of all chars in each puzzle word.
- char order in word and puzzle word does not matter. the quantity does not matter.
- so we preprocess the word and puzzle word.
- trie approach: (trie is good for multiple string vs multiple string find compare)
   - keep only one instance of char and sort, add into trie.
   - puzzle searches: does not match, goes next, first must match.
- bitmask: we can convert each string to a number. if a word is valid  (w&p)==w and w[first]==1
complexity would be O(N^2)
->>

<<-1268	Search Suggestions System    		64.4%	Medium	
classical trie problem.
->>

<<-1707. Maximum XOR with an element from array.
given num and mi, find the max xor using num to xor with the element in array <=mi.
using Trie to build a 32 bit trie.
->>

<<-1698. Number of distinct substrings in a string.
- using hashset to eliminate duplicate
- using trie to count/build trie. however if we use naive trie, will TLE. we do not need add the whole, but add a char by char following the previous char. still TLE.
- rolling hash: there may exist conflicts.
- dp approach: dp[i]: the longest common prefix at position i. 
for example, abcdefcd, d,cd are duplicate. we then need subtract them.
first c is counted, second c shall be discarded
first cd is counted, second cd shall be discarded.
...
->>

### quad tree
quad tree is widely used in geoinfo or map applications.


## misc
<<-869	Reordered Power of 2    		53.9%	Medium	
if we can reorder the digit to get power of 2. only 32 candidates.
sort the string and compare.
->>

<<-1154	Day of the Year    		49.3%	Easy	
leap year, count days
->>

<<-1236	Web Crawler    		64.3%	Medium	
multithreading cpp.
->>

<<-1237	Find Positive Integer Solution for a Given Equation    		69.6%	Easy	->>

<<-1244	Design A Leaderboard    		64.6%	Medium	->>
<<-1243	Array Transformation    		50.6%	Easy	->>

<<-1256	Encode Number    		67.0%	Medium	
0->"" (binary 0)
1->"0" (binary 1)
2->"1" (binary 10)
3->"00" (binary 11)
4->"01" (binary 100)
5->"10" (binary 101)
6->"11" (binary 110)
7->"000" (binary 111)
binary format n = (n+1) and remove the MSB.
->>

<<-1360	Number of Days Between Two Dates    		47.4%	Easy	
use a reference date and count days using leap year.
->>

<<-1420	Build Array Where You Can Find The Maximum Exactly K Comparisons    		64.4%	Hard	
given n, m, k, build the array with n elements:
A[i] in the range [1,m]
->>

<<-913	Cat and Mouse    		33.8%	Hard	->>
<<-882	Reachable Nodes In Subdivided Graph    		42.0%	Hard	->>
<<-770	Basic Calculator IV    		54.1%	Hard	
expression with varibles, variable vs value is given, evaluate the expression.
symbolic evaluation
->>
<<-755	Pour Water    		43.8%	Medium	->>
<<-751	IP to CIDR    		60.6%	Medium	->>
<<-717	1-bit and 2-bit Characters    		47.8%	Easy	
from right to left.
->>
<<-707	Design Linked List    		25.3%	Medium	->>
<<-706	Design HashMap    		62.1%	Easy	->>
<<-705	Design HashSet    		64.5%	Easy	->>
<<-704	Binary Search    		53.7%	Easy	->>
<<-703	Kth Largest Element in a Stream    		50.2%	Easy	
heap
->>
<<-682	Baseball Game    		65.2%	Easy	
simple stack operation
->>
<<-674	Longest Continuous Increasing Subsequence    		46.0%	Easy	
subarray, just count.
->>
<<-671	Second Minimum Node In a Binary Tree    		42.7%	Easy	
use the array second min method.
->>
<<-665	Non-decreasing Array    		19.6%	Easy	->>
<<-661	Image Smoother    		52.0%	Easy	->>
<<-657	Robot Return to Origin    		73.4%	Easy	->>
<<-643	Maximum Average Subarray I    		41.7%	Easy	->>
<<-641	Design Circular Deque    		54.5%	Medium	->>
<<-637	Average of Levels in Binary Tree    		64.0%	Easy	->>
<<-628	Maximum Product of Three Numbers    		47.0%	Easy	->>
<<-627	Swap Salary    		76.6%	Easy	->>
<<-631	Design Excel Sum Formula    		31.8%	Hard	
excel function: 
Excel(H,W), H 1-26, W A to Z.
set(r,c) set the value
sum(row,col,vector<string>) C[r,c]=sum of all the value represented by string
A1 ->row 1 col A, A1:B2 means the top cell is A1 and bottom right cell is B2.
->>
<<-622	Design Circular Queue    		44.7%	Medium	->>
<<-609	Find Duplicate File in System    		60.6%	Medium	
file with directory and content. duplicate file with same content.
hashmap, string.
->>
<<-605	Can Place Flowers    		31.4%	Easy	->>
<<-604	Design Compressed String Iterator    		37.9%	Easy	
each char followed by a digit. prefix sum on the digit.
->>
<<-599	Minimum Index Sum of Two Lists    		51.2%	Easy	
two friends each has a list of favorite restaurants, return the common interest with least index sum.
->>
<<-598	Range Addition II    		49.8%	Easy	
each time choose a rect [0,0] to [r,c] and increase by one.
return the number of max integer.
->>
<<-590	N-ary Tree Postorder Traversal    		72.9%	Easy	->>
<<-589	N-ary Tree Preorder Traversal    		72.8%	Easy	->>
<<-566	Reshape the Matrix    		60.8%	Easy	->>
<<-561	Array Partition I    		72.5%	Easy	
group into pairs and add all the pair min. return the max sum
greedy: sort and choose the min. math proof.
->>
<<-559	Maximum Depth of N-ary Tree    		69.1%	Easy	->>
<<-557	Reverse Words in a String III    		71.1%	Easy	
in place or use stringstream.
->>
<<-541	Reverse String II    		48.8%	Easy	
reverse the first k chars every 2k.
->>

<<-533	Lonely Pixel II    		48.0%	Medium	
given a grid with black/white pixels and an integer N. 
find the number of black pixels satify: (located at (r,c)
- row r and col c both contains exactly N black pixels
- for all rows that have black pixel at column c, they should be same as row R.
hashmap:
->>
<<-531	Lonely Pixel I    		59.3%	Medium	
lonely: the row and column do not have any other black pixels.
rowcnt, colcnt.
->>
<<-530	Minimum Absolute Difference in BST    		54.3%	Easy	->>
<<-522	Longest Uncommon Subsequence II    		34.0%	Medium	->>
<<-521	Longest Uncommon Subsequence I    		58.3%	Easy	->>
<<-520	Detect Capital    		53.8%	Easy	->>
<<-509	Fibonacci Number    		67.1%	Easy	->>
<<-507	Perfect Number    		35.8%	Easy	
n=sum of its divisor (not including itself)
->>
<<-506	Relative Ranks    		50.9%	Easy	->>
<<-504	Base 7    		46.4%	Easy	->>

<<-501	Find Mode in Binary Search Tree    		42.9%	Easy	
most frequent one.
->>
<<-500	Keyboard Row    		65.3%	Easy	->>
<<-492	Construct the Rectangle    		49.9%	Easy	
given area, find [l,w], L>=W and difference L-W is minimized.
math, find the largest factor.
->>
<<-482	License Key Formatting    		43.0%	Easy	->>
<<-476	Number Complement    		65.0%	Easy	->>
<<-461	Hamming Distance    		73.0%	Easy	->>
<<-463	Island Perimeter    		66.3%	Easy	
check its top and left for overlaps. dp, math.
->>
<<-455	Assign Cookies    		50.2%	Easy	->>
<<-453	Minimum Moves to Equal Array Elements    		50.5%	Easy	->>
<<-441	Arranging Coins    		42.2%	Easy	->>
<<-434	Number of Segments in a String    		37.8%	Easy	->>
<<-422	Valid Word Square    		38.0%	Easy	->>

<<-457	Circular Array Loop    		29.7%	Medium	
circular array with positive and negative. postive goes forward and negative goes back.
check if there is any cycle. movement shall be one direction.
- if see 0, stop, not a cycle
- along the path, mark visited if we see visited and detect a cycle.
- modular for negative, positive.
->>
<<-420	Strong Password Checker    		13.7%	Hard	->>
<<-415	Add Strings    		47.9%	Easy	->>
<<-414	Third Maximum Number    		30.6%	Easy	->>
<<-412	Fizz Buzz    		63.4%	Easy	->>
<<-411	Minimum Unique Word Abbreviation    		36.8%	Hard	->>
<<-409	Longest Palindrome    		52.0%	Easy	->>
<<-408	Valid Word Abbreviation    		31.1%	Easy	->>
<<-405	Convert a Number to Hexadecimal    		44.2%	Easy	->>
<<-404	Sum of Left Leaves    		52.1%	Easy	->>
<<-401	Binary Watch    		48.1%	Easy	->>
<<-392	Is Subsequence    		49.4%	Easy	->>
<<-389	Find the Difference    		57.5%	Easy	->>
<<-387	First Unique Character in a String    		53.6%	Easy	->>
<<-383	Ransom Note    		53.2%	Easy	->>
<<-369	Plus One Linked List    		58.5%	Medium	->>
<<-367	Valid Perfect Square    		41.9%	Easy	
do not use sqrt, n^2=1+3+5+...
->>
<<-362	Design Hit Counter    		64.6%	Medium	
count number of hits in the past 5 mins.
map and binary search, but we need loop to get the sum. could use optimization.
->>
<<-350	Intersection of Two Arrays II    		51.7%	Easy	
count in A, and loop on B, if see one, decrease the map.
->>
<<-349	Intersection of Two Arrays    		63.8%	Easy	
->>
<<-348	Design Tic-Tac-Toe    		55.0%	Medium	
zero sum game, use +1 for play A, and -1 for player B.
->>
<<-794	Valid Tic-Tac-Toe State    		33.6%	Medium	
'X' is first hand. so the number of X>=number of O.
check win status.
if A win, Nx>Ny
if B win, Nx==Ny
->>
<<-346	Moving Average from Data Stream    		72.4%	Easy	
deque->>
<<-345	Reverse Vowels of a String    		44.6%	Easy	->>
<<-344	Reverse String    		69.5%	Easy	->>

<-289	Game of Life    		56.0%	Medium	
in-place change.
->>
<<-277	Find the Celebrity    		42.7%	Medium	->>

<<-284	Peeking Iterator    		46.9%	Medium	
given a class Iterator, implement PeekingIterator.
->>
<<-283	Move Zeroes    		58.2%	Easy	
move 0 inplace to the end. two pointer: just bring non-zeros forward.
->>

<<-281	Zigzag Iterator    		59.0%	Medium	
two array, implement iterator to return their elements alternatively.
- store curr pointer end pointer for two array. use queue to alternatively.
->>

<<-273	Integer to English Words    		27.6%	Hard	
->>

<<-271	Encode and Decode Strings    		32.2%	Medium	
send over network: need let other side knows the length and know start and end.
->>

<<-258	Add Digits    		58.1%	Easy	->>
<<-257	Binary Tree Paths    		52.6%	Easy	->>
<<-242	Valid Anagram    		57.6%	Easy	->>
<<-237	Delete Node in a Linked List    		65.5%	Easy	->>
<<-234	Palindrome Linked List    		39.9%	Easy	->>
<<-232	Implement Queue using Stacks    		51.0%	Easy	->>

<<-228	Summary Ranges    		41.7%	Easy	
turn a list of numbers into ranges.
->>
<<-225	Implement Stack using Queues    		46.4%	Easy	->>
<<-206	Reverse Linked List    		64.1%	Easy	->>
<<-205	Isomorphic Strings    		40.1%	Easy	->>
<<-204	Count Primes    		31.9%	Easy	->>
<<-203	Remove Linked List Elements    		38.9%	Easy	->>
<<-202	Happy Number    		50.9%	Easy	->>
<<-191	Number of 1 Bits    		51.3%	Easy	->>
<<-190	Reverse Bits    		41.0%	Easy	->>

<<-187	Repeated DNA Sequences    		40.9%	Medium	
find repeated 10-letter dna sequence
'A': 0x41, 'C': 0x43, 'G'=0x47, 'T'=0x54. or just convert digits.
hashmap.
->>
<<-172	Factorial Trailing Zeroes    		38.1%	Easy	->>
<<-171	Excel Sheet Column Number    		56.5%	Easy	->>
<<-170	Two Sum III - Data structure design    		34.5%	Easy	->>
<<-167	Two Sum II - Input array is sorted    		55.0%	Easy	->>
<<-168	Excel Sheet Column Title    		31.4%	Easy	->>
<<-166	Fraction to Recurring Decimal    		22.0%	Medium	->>
<<-165	Compare Version Numbers    		29.7%	Medium	->>
<<-163	Missing Ranges    		25.1%	Easy	->>
<<-160	Intersection of Two Linked Lists    		42.0%	Easy	->>
<<-155	Min Stack    		45.5%	Easy	->>
<<-125	Valid Palindrome    		37.5%	Easy	->>
<<-119	Pascal's Triangle II    		51.4%	Easy	->>
<<-118	Pascal's Triangle    		53.8%	Easy	->>
<<-88	Merge Sorted Array    		40.0%	Easy	->>
<<-73	Set Matrix Zeroes    		43.8%	Medium	->>
<<-70	Climbing Stairs    		48.3%	Easy	->>
<<-69	Sqrt(x)    		34.5%	Easy	binary search!->>
<<-67	Add Binary    		46.2%	Easy	->>
<<-66	Plus One    		42.8%	Easy	->>
<<-58	Length of Last Word    		33.3%	Easy	->>
<<-14	Longest Common Prefix    		35.8%	Easy	->>
<<-38	Count and Say    		45.4%	Easy	->>
<<-7	Reverse Integer    		25.8%	Easy	->>
<<-2	Add Two Numbers    		34.6%	Medium	->>
<<-35	Search Insert Position    		42.7%	Easy	->>
<<-34	Find First and Last Position of Element in Sorted Array    		36.7%	Medium	->>
<<-28	Implement strStr()    		34.9%	Easy	->>
<<-27	Remove Element    		48.8%	Easy	->>
<<-26	Remove Duplicates from Sorted Array    		
<<-9	Palindrome Number    		49.1%	Easy	->>
<<-21	Merge Two Sorted Lists    		54.8%	Easy	->>
<<-20	Valid Parentheses    		39.4%	Easy	->>

## leetcode problem list

<<-65	Valid Number    		15.6%	Hard	
a lot of corner cases.
->>

<<-41	First Missing Positive    		33.1%	Hard	->>
<<-37	Sudoku Solver    		45.2%	Hard	->>
<<-36	Valid Sudoku    		49.7%	Medium	->>

<<-13	Roman to Integer    		56.2%	Easy	->>
<<-12	Integer to Roman    		55.6%	Medium	->>

<<-8	String to Integer (atoi)    		15.5%	Medium	->>
<<-6	ZigZag Conversion    		37.2%	Medium	->>



### common small tricks:

modular inverse
Fermat little thereom:
for prime p, a^p-a is a multiple of p. 
a*(a^(p-1)-1) is a multipe of p.
thus a^(p-1)=1 (mod m)
or a^(p-2)=a^(-1) (mod m)
using a^(p-2) (binary power in log(p) time) to get the modular.
```cpp
    long modinv(int a,int n,int p){ //calculate a^-1%p -->a^(p-2)%p
        if(n==0) return 1;
        if(n%2) return a*modinv((long)a*a%p,n/2,p)%p;
        return modinv((long)a*a%p,n/2,p)%p;
    }
```	

iterate all sub bitmask for a bitmask
start from the largest
```
for(int s=m;s;s=(s-1)&m){
}
```

next bigger number with same number of set bits
```
    unsigned next_bigger(unsigned a) {
      /* works for any word length */
      unsigned c = (a & -a);
      unsigned r = a+c;
      return (((r ^ a) >> 2) / c) | r;
    }    
```
```
    int gcd(int a,int b)
    {
        if(!b) return a;
        return gcd(b,a%b);
    }
    int lcm(int a,int b)
    {
        int div=gcd(a,b);
        return a*b/div;
    }
```	

dijkstra and shortest path problem
dijkstra find the shortest path between two nodes, trying the shortest edge first. non-negative weight.
- one source to all other nodes, shortest path tree.
dijkstra's algorithm:

s1: Mark all nodes unvisited. Create a set of all the unvisited nodes called the unvisited set.
s2: Assign to every node a tentative distance value: set it to zero for our initial node and to infinity for all other nodes. Set the initial node as current.[16]
s3: For the current node, consider all of its unvisited neighbours and calculate their tentative distances through the current node. Compare the newly calculated tentative distance to the current assigned value and assign the smaller one. For example, if the current node A is marked with a distance of 6, and the edge connecting it with a neighbour B has length 2, then the distance to B through A will be 6 + 2 = 8. If B was previously marked with a distance greater than 8 then change it to 8. Otherwise, the current value will be kept.
s4: When we are done considering all of the unvisited neighbours of the current node, mark the current node as visited and remove it from the unvisited set. A visited node will never be checked again.
s5: If the destination node has been marked visited (when planning a route between two specific nodes) or if the smallest tentative distance among the nodes in the unvisited set is infinity (when planning a complete traversal; occurs when there is no connection between the initial node and remaining unvisited nodes), then stop. The algorithm has finished.
s6: Otherwise, select the unvisited node that is marked with the smallest tentative distance, set it as the new "current node", and go back to step 3.

see the Maze II.


skip list:

The worst case search time for a sorted linked list is O(n) as we can only linearly traverse the list and cannot skip nodes while searching. For a Balanced Binary Search Tree, we skip almost half of the nodes after one comparison with root. For a sorted array, we have random access and we can apply Binary Search on arrays.

Can we augment sorted linked lists to make the search faster? The answer is Skip List. The idea is simple, we create multiple layers so that we can skip some nodes. See the following example list with 16 nodes and two layers. The upper layer works as an “express lane” which connects only main outer stations, and the lower layer works as a “normal lane” which connects every station. Suppose we want to search for 50, we start from first node of “express lane” and keep moving on “express lane” till we find a node whose next is greater than 50. Once we find such a node (30 is the node in following example) on “express lane”, we move to “normal lane” using pointer from this node, and linearly search for 50 on “normal lane”. In following example, we start from 30 on “normal lane” and with linear search, we find 50.



What is the time complexity with two layers? The worst case time complexity is number of nodes on “express lane” plus number of nodes in a segment (A segment is number of “normal lane” nodes between two “express lane” nodes) of “normal lane”. So if we have n nodes on “normal lane”, √n (square root of n) nodes on “express lane” and we equally divide the “normal lane”, then there will be √n nodes in every segment of “normal lane” . √n is actually optimal division with two layers. With this arrangement, the number of nodes traversed for a search will be O(√n). Therefore, with O(√n) extra space, we are able to reduce the time complexity to O(√n).

if we keep top layer is half of bottom layer, then we get log(n) complexity.

some subjects on algorithm

## base problem (number position system)

### balance base-3 problem

using -1,1,0 to represent a number
n=sum(Ai*3^i), then Ai would be 0, 1, 2.
we have to change 2 to -1 using 2=3-1 (3-1)*3^i=3^(i+1)-3^i. that is we need add a carry flag to the next when the digit is 2.

```cpp
  while(n){
    int d=n%3;
    n/=3;
    int cf=0;
    if(d==2)
      cf=1;
     n+=cf;
  }
```
cases: n=0 and n<0, for negative cases: we just reverse every digits.

### negative base problem
using similar approach:
n=sum(Ai*p^i), Ai is 0 to |p|-1
when p<0, n%p will get the result from 0 to p+1 (negatives) and we need convert the digit to positive:
Ai=-p+Ai+p
Ai*p^i=(Ai-p+p)*p^i=(Ai-p)*p^i+p^(i+1), that means when we get the remainder <0, we add a carrier to next level
```cpp
  while(n){
    int d=n%d;
    n/=d;
    int cf=0;
    if(d<0){
      cf=1;
    }
    n+=cf;
 }
 ```
 see LC1017. Convert to Base -2
 
 ### gray code
 number to gray code: i^(i>>1)
 see LC89. Gray Code
 gray code to number: 
 We will move from the most significant bits to the least significant ones (the least significant bit has index 1 and the most significant bit has index k). 
 g3  g2  g1  g0
 |  /|  /|  /|
 V/  V / V/  V
 b3  b2  b1  b0
 
 or equiv:
 b3=g3
 b2=g3^g2
 b1=g3^g2^g1
 b0=g3^g2^g1^g0
 
```cpp
int rev_g (int g) {
  int n = 0;
  for (; g; g >>= 1)
    n ^= g;
  return n;
}
```
- gray code can form a hamiltonian cycle in a hypercube with each bit as a dimension in space.
hamiltonian cycle  is a graph cycle (i.e., closed loop) through a graph that visits each node exactly once.
- gray code is used in DAC to minimize conversion errors
- gray code is also used in genetic algorithm theory
- can be used to solve tower of hanoi
tower of hanoi: three rods with a stack of disks in ascending order, move it one by one to another rod, following the ascending order all the time.
represent the status using binary: the MSB represents the largest disk and the LSB represents the smallest disk
each time we only change one bit, which is equiv to a move.

Practice:
It is necessary to arrange numbers from 0 to 2^(N+M)-1 in the matrix with 2^N rows and 2^M columns. 
Moreover, numbers occupying two adjacent cells must differ only in single bit in binary notation. 
Cells are adjacent if they have common side. 
Matrix is cyclic, i.e. for each row the leftmost and rightmost matrix cells are considered to be adjacent 
(the topmost and the bottommost matrix cells are also adjacent).

Input
The first line of input contains two integers N and M (0<N,M; N+M<=20).

Output
Output file must contain the required matrix in a form of 2^N lines of 2^M integers each.

Sample test(s)

Input
1 1

Output
0 2
1 3

the number shall have n+m bits (from 0 to 2^(n+m)-1) and each row is a cyclic gray code with m bits, each col is a cyclic gray code with n bits
but the starting is different. using different start value (xor the start value).
see LC1238. Circular Permutation in Binary Representation (1d case of this problem)


## algebra
### binary exponential
using divide and conquer and convert x^n to a O(logn) algorithm.
```cpp
long long binpow(long long a, long long b) {
    long long res = 1;
    while (b > 0) {
        if (b & 1)
            res = res * a;
        a = a * a;
        b >>= 1;
    }
    return res;
}
```

example 1: effective computation x^n%m
```cpp
long long binpow(long long a, long long b, long long m) {
    a %= m;
    long long res = 1;
    while (b > 0) {
        if (b & 1)
            res = res * a % m;
        a = a * a % m;
        b >>= 1;
    }
    return res;
}
```

example 2: effective computation of fib series
F(n)=F(n-1)+F(n-2)
change to a matrix format and it reduces to a M^n problem

example 3: geometric transformation, translation, scale, rotation et al
represent the transformation using matrix and it reduces to matrix multiplication and power
similar to opengl, we need to add 4th dimension to support translation.

example 4: two number product modulo problem (ab)%m->(2*a/2*b)%m

example 5: number of paths of length k in a graph.
represent the graph in a matrix G (undirected, unweighted), edge can be visited multiple times
G[i,j] represents number of paths between i and j.
G is the case for k=1
starting from k: C(k+1)=C(k)*G
C(k+1)=G^k
matrix multiplication is O(N^3)

example 6: shortest paths of a fixed length k
We are given a directed weighted graph G with n vertices and an integer k. For each pair of vertices (i,j) we have to find the length of the shortest path between i and j that consists of exactly k edges.

represent the graph using adjacency matrix, where G[i,j] represents the weight from i to j. if there is no edge, it shall be inf.
and G is the case for k=1.

again assume L(k) is the matrix for k, then we advance one step to get k+1:
L(k+1,i,j)=min(L(k,i,p)+G(p,j)) for all p=0 to n-1
if we define the min operation as @, L(k+1)=L(k)@G, then we get:
L(K)=G@G....@G (since the min operation is associative ie min(a,min(b,c))=min(min(a,b),c)
and then we can apply the binary exponential on this operator.

example 7: generalization: shortest paths up to length k.
for each vertex v, we duplicate a vertex v' and create edge [v,v'] and a self cycle in [v',v'] with weight 0. then we can convert the problem [u,v] with at most k to a problem from [u,v'] with a fixed length k+1.

practice questions:
Many well-known cryptographic operations require modular exponentiation. That is, given integers x, y and n, compute x^y mod n. In this question, you are tasked to program an efficient way to execute this calculation.
Input
The input consists of a line containing the number c of datasets, followed by c datasets, followed by a line containing the number ‘0’.
Each dataset consists of a single line containing three positive integers, x, y, and n, separated by blanks. You can assume that 1 < x, n < 2^15 = 32768, and 0 < y < 2^31 = 2147483648.
Output
The output consists of one line for each dataset. The i-th line contains a single positive integer z such that z = x^y mod n
for the numbers x, y, z given in the i-th input dataset.
Sample Input
2
2 3 5
2 2147483647 13
0
Sample Output
3
11

question 2:
get n^k the first 3 digits and last 3 digits
we can get all the digits using n^k%10

question 3:
parking lot with 2n-2 spaces, 4 types of cars, total number > parking spaces, and each type of cars > parking spaces.
parking the car with n ajacent cars the same make (exactly), return the total number of parking combinations
2n-2 spaces, 4 types of cars, exactly n the same.
analysis: n treated as one, then we have 2n-2-n=n-2 selections. When n cars fixed in position, we cannot choose same car in its adjacent position, left and right two cases: any combinations

question 4:
find the last digits of a^b
a^b reduce the last digit d^b



### gcd and euclid algorithm
### gcd
```cpp
int gcd (int a, int b) {
    if (b == 0)
        return a;
    else
        return gcd (b, a % b);
}

int gcd (int a, int b) {
    while (b) {
        a %= b;
        swap(a, b);
    }
    return a;
}

int lcm (int a, int b) {
    return a / gcd(a, b) * b;
}
```
O(log(min(a,b))
suppose d=gcd(a,b), then both a and b are divisible by d:
a%b=a-[a/b]*b
divide each side by d: then we see a%b is divisible by d also

gcd function is associative which is often used in some problems:
gcd(gcd(a,b),c)=gcd(a,gcd(b,c))
this can applies to >2 numbers, such as if a group of number exist coprime combinations

### extended euclid algorithm
ax+by=gcd(a,b)
this finds a way to represent gcd(a,b) using a and b (find its coefficient)
- it guarantee the existence of solution (water jug problem)
- get the solution (x,y)

ax+by=gcd(a,b)=gcd(b,a%b)
bx+(a%b)y=g
bx+a-[a/b]*by=g

base case: a=0, x=1, gcd=b
```cpp
int gcd(int a, int b, int & x, int & y) {
    if (a == 0) {
        x = 0;
        y = 1;
        return b;
    }
    int x1, y1;
    int d = gcd(b % a, a, x1, y1);
    x = y1 - (b / a) * x1;
    y = x1;
    return d;
}
```
note:
- extended euclid algorithm also works for more than two numbers.

### Linear Diophantine Equation
ax+by=c
- a=b=c=0, infinite solutions
- a=b=0 c!=0, no solutions
- find any solution if c is divisible by gcd(a,b)
ax+by=c
ax'*c/g+by'*c/g=c
x=x'*c/g
y=y'*c/g

```cpp
int gcd(int a, int b, int &x, int &y) {
    if (a == 0) {
        x = 0; y = 1;
        return b;
    }
    int x1, y1;
    int d = gcd(b%a, a, x1, y1);
    x = y1 - (b / a) * x1;
    y = x1;
    return d;
}

bool find_any_solution(int a, int b, int c, int &x0, int &y0, int &g) {
    g = gcd(abs(a), abs(b), x0, y0);
    if (c % g) {
        return false;
    }

    x0 *= c / g;
    y0 *= c / g;
    if (a < 0) x0 = -x0;
    if (b < 0) y0 = -y0;
    return true;
}
```
- get all solutions
a(x+b/g)+b(y-a/g)=c
so x=x'+b/g and y=y'-a/g is also a solution we can just add a k integer factor

- find solutions in range [xmin,xmax] and [ymin,ymax]
the idea is to shift the solution so that x is in the range and get y range, and then shift y into [ymin, ymax] and get x range
and then get the two intersection.
```cpp
void shift_solution(int & x, int & y, int a, int b, int cnt) {
    x += cnt * b;
    y -= cnt * a;
}

int find_all_solutions(int a, int b, int c, int minx, int maxx, int miny, int maxy) {
    int x, y, g;
    if (!find_any_solution(a, b, c, x, y, g))
        return 0;
    a /= g;
    b /= g;

    int sign_a = a > 0 ? +1 : -1;
    int sign_b = b > 0 ? +1 : -1;

    shift_solution(x, y, a, b, (minx - x) / b);
    if (x < minx)
        shift_solution(x, y, a, b, sign_b);
    if (x > maxx)
        return 0;
    int lx1 = x;

    shift_solution(x, y, a, b, (maxx - x) / b);
    if (x > maxx)
        shift_solution(x, y, a, b, -sign_b);
    int rx1 = x;

    shift_solution(x, y, a, b, -(miny - y) / a);
    if (y < miny)
        shift_solution(x, y, a, b, -sign_a);
    if (y > maxy)
        return 0;
    int lx2 = x;

    shift_solution(x, y, a, b, -(maxy - y) / a);
    if (y > maxy)
        shift_solution(x, y, a, b, sign_a);
    int rx2 = x;

    if (lx2 > rx2)
        swap(lx2, rx2);
    int lx = max(lx1, lx2);
    int rx = min(rx1, rx2);

    if (lx > rx)
        return 0;
    return (rx - lx) / abs(b) + 1;
}
```
- Find the solution with minimum value of x+y
x=x'+b/g
y=y'-a/g
x+y=x'+y'+k*(b-a)/g
b>a choose smallest k
b<a: choose max k.

### fibocci series
- matrix form:
(Fn Fn+1)=(F0 F1)⋅P^n
p=[0,1;1,1]

- fast doubling method:
F(2k)=F(k)*(2F(k+1)-F(k))
F(2k+1)=F(k+1)^2+F(k)^2
```cpp
pair<int, int> fib (int n) {
    if (n == 0)
        return {0, 1};

    auto p = fib(n >> 1);
    int c = p.first * (2 * p.second - p.first);
    int d = p.first * p.first + p.second * p.second;
    if (n & 1)
        return {d, c + d};
    else
        return {c, d};
}
```
- periodicity modulo p

## prime numbers
Sieve of Eratosthenes 筛法
```cpp
int n;
vector<bool> is_prime(n+1, 1);
is_prime[0] = is_prime[1] = 0;
for (int i = 2; i <= n; i++) {
    if (is_prime[i] && (long long)i * i <= n) {
        for (int j = i * i; j <= n; j += i)
            is_prime[j] = 0;
    }
}
```
complexity analysis:
storage: O(n), time: O(nloglog(n))

optimization: 
- sifting to only the root
```cpp
int n;
vector<char> is_prime(n+1, true);
is_prime[0] = is_prime[1] = false;
for (int i = 2; i * i <= n; i++) {
    if (is_prime[i]) {
        for (int j = i * i; j <= n; j += i)
            is_prime[j] = false;
    }
}
```
- only process odd numbers
- block sieving
It follows from the optimization "sieving till root" that there is no need to keep the whole array is_prime[1...n] at all time. For performing of sieving it's enough to keep just prime numbers until root of n, i.e. prime[1... sqrt(n)], split the complete range into blocks, and sieve each block separately. In doing so, we never have to keep multiple blocks in memory at the same time, and the CPU handles caching a lot better.

Let s be a constant which determines the size of the block, then we have ⌈ns⌉ blocks altogether, and the block k (k=0...⌊ns⌋) contains the numbers in a segment [ks;ks+s−1]. We can work on blocks by turns, i.e. for every block k we will go through all the prime numbers (from 1 to n−−√) and perform sieving using them. It is worth noting, that we have to modify the strategy a little bit when handling the first numbers: first, all the prime numbers from [1;n−−√] shouldn't remove themselves; and second, the numbers 0 and 1 should be marked as non-prime numbers. While working on the last block it should not be forgotten that the last needed number n is not necessary located in the end of the block.

Here we have an implementation that counts the number of primes smaller than or equal to n using block sieving.

```cpp
int count_primes(int n) {
    const int S = 10000;

    vector<int> primes;
    int nsqrt = sqrt(n);
    vector<char> is_prime(nsqrt + 1, true);
    for (int i = 2; i <= nsqrt; i++) {
        if (is_prime[i]) {
            primes.push_back(i);
            for (int j = i * i; j <= nsqrt; j += i)
                is_prime[j] = false;
        }
    }

    int result = 0;
    vector<char> block(S);
    for (int k = 0; k * S <= n; k++) {
        fill(block.begin(), block.end(), true);
        int start = k * S;
        for (int p : primes) {
            int start_idx = (start + p - 1) / p;
            int j = max(start_idx, p) * p - start;
            for (; j < S; j += p)
                block[j] = false;
        }
        if (k == 0)
            block[0] = block[1] = false;
        for (int i = 0; i < S && start + i <= n; i++) {
            if (block[i])
                result++;
        }
    }
    return result;
}
```

this is good for followup to save memory requirement and improve cache performance.

integer prime factorization:
```cpp
vector<long long> trial_division1(long long n) {
    vector<long long> factorization;
    for (long long d = 2; d * d <= n; d++) {
        while (n % d == 0) {
            factorization.push_back(d);
            n /= d;
        }
    }
    if (n > 1)
        factorization.push_back(n);
    return factorization;
}
```
number of divisior
in a map: prime vs count, number of divisor is product of each cnt+1

sum of divisor:
1+p+p^2+...+p^k for p =(p^(k+1)-1)/(p-1) and then multiply all these sum

## data structure
### min stack /min queue
- find the min in a stack i O(1)
save the element and current min as a pair in a single stack

- find the min (same problem) using queue
add to the back and remove from the front
using deque.
keep the deque in nondecreasing order, so the front is always the min.
not all the elements are stored
when removing an element, needs check if the front is the element

- queue modification using two stacks
all elements are stored
do not need to check the element

- Finding the minimum for all subarrays of fixed length M.
above three methods are all able to solve it.

### sparse table
sparse table is a data structure for range query of immutable array.
the idea is to precompute the range queries for 2^j intervals (similar to binary reprsentation of any number)
store the precomputed queries in a 2d matrix:
for length=n, there are at least log2(n) items.
st[i,j] represents the range for [i,i+2^j)
st[i,j] fits nicely [i,i+2^(j-1）） and [i+2^(j-1),i+2^j) (both length 2^(j-1))
recurrence relation:
st[i,j]=f(st[i,j-1]+st[i+2^(j-1),j-1])
f is the function of query.

- range sum query:
```cpp

long long st[MAXN][K];

for (int i = 0; i < N; i++)
    st[i][0] = array[i];

for (int j = 1; j <= K; j++)
    for (int i = 0; i + (1 << j) <= N; i++)
        st[i][j] = st[i][j-1] + st[i + (1 << (j - 1))][j - 1];
		
query:

long long sum = 0;
for (int j = K; j >= 0; j--) {
    if ((1 << j) <= R - L + 1) {
        sum += st[L][j];
        L += 1 << j;
    }
}		
```

- range min query
compute the min of the range [L，R].
min(st[L,j],st[R-2^j+1,j]) j=log2(R-L+1)
this splits into two section.
first range L+[0,2^j-1], this mostly does not cover R.
second range: R-2^j+1+[0,2^j-1]
this two ranges overlaps.

```cpp
//fast calculate log2
int log[MAXN+1];
log[1] = 0;
for (int i = 2; i <= MAXN; i++)
    log[i] = log[i/2] + 1;

//calculate spare table st.
int st[MAXN][K];

for (int i = 0; i < N; i++)
    st[i][0] = array[i];

for (int j = 1; j <= K; j++)
    for (int i = 0; i + (1 << j) <= N; i++)
        st[i][j] = min(st[i][j-1], st[i + (1 << (j - 1))][j - 1]);	
//query in O(1)
int j = log[R - L + 1];
int minimum = min(st[L][j], st[R - (1 << j) + 1][j]);
```
above approach only works for idempotent function (which can applies multiple times without changing the results)
for example the min function, but sum function cannot work.

### disjoint set
- naive implementation - lead to chain in worst cases
- path compression 
- union by rank or size. (rank: the depth, size: number of vertices)
will has constant access time.

- link by index (index is a random number)
- coin flip linking (using random number odd/even to decide the linking)

applications:
- Connected components in a graph
add vertices and edges into a graph
This application is quite important, because nearly the same problem appears in Kruskal's algorithm for finding a minimum spanning tree. 
Using DSU we can improve the O(mlogn+n^2) complexity to O(mlogn).
using DSU:
we sort the edges from small to long
using the edges to union two vertices. if both vertices are in the same set, discard it.
stop when we get N-1 edges

- Search for connected components in an image
One of the applications of DSU is the following task: there is an image of n×m pixels. Originally all are white, but then a few black pixels are drawn. You want to determine the size of each white connected component in the final image.

For the solution we simply iterate over all white pixels in the image, for each cell iterate over its four neighbors, and if the neighbor is white call union_sets. Thus we will have a DSU with nm nodes corresponding to image pixels. The resulting trees in the DSU are the desired connected components.

The problem can also be solved by DFS or BFS, but the method described here has an advantage: it can process the matrix row by row (i.e. to process a row we only need the previous and the current row, and only need a DSU built for the elements of one row) in O(min(n,m)) memory.

- Store additional information for each set
you can store any information in the set, such as size, depth,...

- Compress jumps along a segment / Painting subarrays offline
example: array of length L, and each query [l,r,c] will paint the cells in range [l,r] to the color c.
return the final color of the array.
paint in reverse order, we only need to take care of unpainted cells.
thus we can union the same color.
```cpp
for (int i = 0; i <= L; i++) {
    make_set(i);
}

for (int i = m-1; i >= 0; i--) {
    int l = query[i].l;
    int r = query[i].r;
    int c = query[i].c;
    for (int v = find_set(l); v <= r; v = find_set(v)) {
        answer[v] = c;
        parent[v] = v + 1;
    }
}
```

For the solution we can make a DSU, which for each cell stores a link to the next unpainted cell. Thus initially each cell points to itself. 
After painting one requested repaint of a segment, all cells from that segment will point to the cell after the segment.
when painted, point to its right. if it is painted, then it points to it next unpainted right.
for example:
last paint is [8,9,0], then 8->9->10
paint [5,11,1], then we get 5->6->7->10->11->12
good problem.

- Support distances up to representative
add distance to representative (the root of the set)
```cpp
void make_set(int v) {
    parent[v] = make_pair(v, 0);
    rank[v] = 0;
}

pair<int, int> find_set(int v) {
    if (v != parent[v].first) {
        int len = parent[v].second;
        parent[v] = find_set(parent[v].first);
        parent[v].second += len;
    }
    return parent[v];
}

void union_sets(int a, int b) {
    a = find_set(a).first;
    b = find_set(b).first;
    if (a != b) {
        if (rank[a] < rank[b])
            swap(a, b);
        parent[b] = make_pair(a, 1);
        if (rank[a] == rank[b])
            rank[a]++;
    }
}
```

- Support the parity of the path length / Checking bipartiteness online

### Fenwick tree
used for updating a range and query a range using a reversible function.
most often used function is sum
both operation complexity is O(logn)

For array A[0..N] we precompute the T[0..N]
for every T[i] it is the function on the range [g[i],i]

def sum(int r): #calculate the sum for A[0,r] using intervals seamlessly.
    res = 0
    while (r >= 0):
        res += t[r]
        r = g(r) - 1
    return res

def increase(int i, int delta): #increase A[i], we need change all T involving i.
    for all j with g(j) <= i <= j:
        t[j] += delta
	
note: for sum, we are reducing the r, for increasing, we increase the j
0-based index:
g[i]=i&(i+1)
h[j]=j|(j+1)

finding sum:
```cpp
struct FenwickTree {
    vector<int> bit;  // binary indexed tree
    int n;

    FenwickTree(int n) {
        this->n = n;
        bit.assign(n, 0);
    }

    FenwickTree(vector<int> a) : FenwickTree(a.size()) {
        for (size_t i = 0; i < a.size(); i++)
            add(i, a[i]);
    }

    int sum(int r) {
        int ret = 0;
        for (; r >= 0; r = (r & (r + 1)) - 1)
            ret += bit[r];
        return ret;
    }

    int sum(int l, int r) {
        return sum(r) - sum(l - 1);
    }

    void add(int idx, int delta) {
        for (; idx < n; idx = idx | (idx + 1))
            bit[idx] += delta;
    }
};
```

2d BIT:
```cpp
struct FenwickTree2D {
    vector<vector<int>> bit;
    int n, m;

    // init(...) { ... }

    int sum(int x, int y) {
        int ret = 0;
        for (int i = x; i >= 0; i = (i & (i + 1)) - 1)
            for (int j = y; j >= 0; j = (j & (j + 1)) - 1)
                ret += bit[i][j];
        return ret;
    }

    void add(int x, int y, int delta) {
        for (int i = x; i < n; i = i | (i + 1))
            for (int j = y; j < m; j = j | (j + 1))
                bit[i][j] += delta;
    }
};
```

1-based index:
g[i]=i-i&(-i)
h[i]=i+i&(-i)

```cpp
struct FenwickTreeOneBasedIndexing {
    vector<int> bit;  // binary indexed tree
    int n;

    FenwickTreeOneBasedIndexing(int n) {
        this->n = n + 1;
        bit.assign(n + 1, 0);
    }

    FenwickTreeOneBasedIndexing(vector<int> a)
        : FenwickTreeOneBasedIndexing(a.size()) {
        init(a.size());
        for (size_t i = 0; i < a.size(); i++)
            add(i, a[i]);
    }

    int sum(int idx) {
        int ret = 0;
        for (++idx; idx > 0; idx -= idx & -idx)
            ret += bit[idx];
        return ret;
    }

    int sum(int l, int r) {
        return sum(r) - sum(l - 1);
    }

    void add(int idx, int delta) {
        for (++idx; idx < n; idx += idx & -idx)
            bit[idx] += delta;
    }
};
```

common operation:
point update, range query
range update, point query
the idea is update all the bit +x after L, and update -x all the bit after R.
```cpp
void add(int idx, int val) {
    for (++idx; idx < n; idx += idx & -idx) //++idx is for 1-based
        bit[idx] += val;
}

void range_add(int l, int r, int val) {
    add(l, val);
    add(r + 1, -val);
}

int point_query(int idx) {
    int ret = 0;
    for (++idx; idx > 0; idx -= idx & -idx)
        ret += bit[idx];
    return ret;
}
```
range update, range query
```cpp
def add(b, idx, x):
    while idx <= N:
        b[idx] += x
        idx += idx & -idx

def range_add(l,r,x):
    add(B1, l, x)
    add(B1, r+1, -x)
    add(B2, l, x*(l-1))
    add(B2, r+1, -x*r)

def sum(b, idx):
    total = 0
    while idx > 0:
        total += b[idx]
        idx -= idx & -idx
    return total

def prefix_sum(idx):
    return sum(B1, idx)*idx -  sum(B2, idx)

def range_sum(l, r):
    return sum(r) - sum(l-1)
```    

### sgement tree
find the sum or min in a range in O(logn) using segment tree.
segment tree is a full binary tree with the root for the whole tree, left is the left array and right is the right array recursively. Using array to store the tree and the index of the child and parent relation is fixed.
- build the tree
build from bottom layer (post-order)
```cpp
void build(int a[], int v, int tl, int tr) {
    if (tl == tr) {
        t[v] = a[tl];
    } else {
        int tm = (tl + tr) / 2;
        build(a, v*2, tl, tm);
        build(a, v*2+1, tm+1, tr);
        t[v] = t[v*2] + t[v*2+1]; //postorder 
    }
}
```
the left side covers [tl,tm] and right side [tm+1,tr]
- query the sum
if the range [tl,tr] is the same as the node, then just return it
if not we need go to left recursively or/and right recursively

```cpp
int sum(int v, int tl, int tr, int l, int r) {
    if (l > r) 
        return 0;
    if (l == tl && r == tr) {
        return t[v];
    }
    int tm = (tl + tr) / 2;
    return sum(v*2, tl, tm, l, min(r, tm))
           + sum(v*2+1, tm+1, tr, max(l, tm+1), r);
}
```
- update a value
if a value is changed it only affects the ranges which includes the element.
also using post-order traversal
```cpp
void update(int v, int tl, int tr, int pos, int new_val) {
    if (tl == tr) {
        t[v] = new_val;
    } else {
        int tm = (tl + tr) / 2;
        if (pos <= tm)
            update(v*2, tl, tm, pos, new_val);
        else
            update(v*2+1, tm+1, tr, pos, new_val);
        t[v] = t[v*2] + t[v*2+1];
    }
}
```

- query the max or the min
we can use very similar approach, using max or min function
t[v]=max(t[2*v],t[2*v+1])

- query the max and its number of occurrence
store a pair of information: the max in the range and its occurrence
similarly using post-order.
```cpp
pair<int, int> t[4*MAXN];

pair<int, int> combine(pair<int, int> a, pair<int, int> b) {
    if (a.first > b.first) 
        return a;
    if (b.first > a.first)
        return b;
    return make_pair(a.first, a.second + b.second);
}

void build(int a[], int v, int tl, int tr) {
    if (tl == tr) {
        t[v] = make_pair(a[tl], 1);
    } else {
        int tm = (tl + tr) / 2;
        build(a, v*2, tl, tm);
        build(a, v*2+1, tm+1, tr);
        t[v] = combine(t[v*2], t[v*2+1]);
    }
}

pair<int, int> get_max(int v, int tl, int tr, int l, int r) {
    if (l > r)
        return make_pair(-INF, 0);
    if (l == tl && r == tr)
        return t[v];
    int tm = (tl + tr) / 2;
    return combine(get_max(v*2, tl, tm, l, min(r, tm)), 
                   get_max(v*2+1, tm+1, tr, max(l, tm+1), r));
}

void update(int v, int tl, int tr, int pos, int new_val) {
    if (tl == tr) {
        t[v] = make_pair(new_val, 1);
    } else {
        int tm = (tl + tr) / 2;
        if (pos <= tm)
            update(v*2, tl, tm, pos, new_val);
        else
            update(v*2+1, tm+1, tr, pos, new_val);
        t[v] = combine(t[v*2], t[v*2+1]);
    }
}
```

- compute the gcd or lcm in range
this is similar to max/min/sum, these functions are all associative.

other advance segment tree
- counting the number of zeros in range, and find the kth 0 index
segment tree stores the number of zeros in each range and find kth can use a binary tree traversal (preorder traversal or descending the segment tree):
```cpp
int find_kth(int v, int tl, int tr, int k) {
    if (k > t[v])
        return -1;
    if (tl == tr)
        return tl;
    int tm = (tl + tr) / 2;
    if (t[v*2] >= k)
        return find_kth(v*2, tl, tm, k);
    else 
        return find_kth(v*2+1, tm+1, tr, k - t[v*2]);
}
```
- searching the first prefix >= target
this can be obtained using lower_bound in O(logn)
or use binary tree search using segment tree.

- Searching for the first element greater than a given amount
give x and a range [l,r], find the first element
we build  a max segment tree and then search in the segement tree.

```cpp
int get_first(int v, int lv, int rv, int l, int r, int x) {
    if(lv > r || rv < l) return -1;
    if(l <= lv && rv <= r) {
        if(t[v] <= x) return -1;
        while(lv != rv) {
            int mid = lv + (rv-lv)/2;
            if(t[2*v] > x) {
                v = 2*v;
                rv = mid;
            }else {
                v = 2*v+1;
                lv = mid+1;
            }
        }
        return lv;
    }

    int mid = lv + (rv-lv)/2;
    int rs = get_first(2*v, lv, mid, l, r, x);
    if(rs != -1) return rs;
    return get_first(2*v+1, mid+1, rv, l ,r, x);
}
```

confused about binary index tree vs segment tree?
both can be used for dynamic cases (query and updates). BIT actually does not have a tree structure so cannot use tree traversal, but its navigation is determined by the simple relation. segment tree although uses the array but its inside logic is binary tree structure, so we can use those tree traversal and is more understandable (can use recursive).

in a lot of cases, they are exchangeable. segment tree is more versatile.

- find max subsegment sum:
given a range [l,r], find the subsegment max sum.
using segment tree, we store 4 information:
max prefix sum
max suffix sum
sum of segment
max subsegment sum in the segment

then we can determine parent's information from the left right child

```cpp
struct data {
    int sum, pref, suff, ans;
};

data combine(data l, data r) {
    data res;
    res.sum = l.sum + r.sum;
    res.pref = max(l.pref, l.sum + r.pref);
    res.suff = max(r.suff, r.sum + l.suff);
    res.ans = max(max(l.ans, r.ans), l.suff + r.pref);
    return res;
}

data make_data(int val) {
    data res;
    res.sum = val;
    res.pref = res.suff = res.ans = max(0, val);
    return res;
}

void build(int a[], int v, int tl, int tr) {
    if (tl == tr) {
        t[v] = make_data(a[tl]);
    } else {
        int tm = (tl + tr) / 2;
        build(a, v*2, tl, tm);
        build(a, v*2+1, tm+1, tr);
        t[v] = combine(t[v*2], t[v*2+1]);
    }
}

void update(int v, int tl, int tr, int pos, int new_val) {
    if (tl == tr) {
        t[v] = make_data(new_val);
    } else {
        int tm = (tl + tr) / 2;
        if (pos <= tm)
            update(v*2, tl, tm, pos, new_val);
        else
            update(v*2+1, tm+1, tr, pos, new_val);
        t[v] = combine(t[v*2], t[v*2+1]);
    }
}
data query(int v, int tl, int tr, int l, int r) {
    if (l > r) 
        return make_data(0);
    if (l == tl && r == tr) 
        return t[v];
    int tm = (tl + tr) / 2;
    return combine(query(v*2, tl, tm, l, min(r, tm)), 
                   query(v*2+1, tm+1, tr, max(l, tm+1), r));
}
```

- saving the entire array of a range
we can sort and store in some data structure, such as set, map et al.

- Find the smallest number greater or equal to a specified number. No modification queries.
We want to answer queries of the following form: for three given numbers (l,r,x) we have to find the minimal number in the segment a[l…r] which is greater than or equal to x.

store the whole array in sorted order:
```cpp
vector<int> t[4*MAXN];

void build(int a[], int v, int tl, int tr) {
    if (tl == tr) {
        t[v] = vector<int>(1, a[tl]);
    } else { 
        int tm = (tl + tr) / 2;
        build(a, v*2, tl, tm);
        build(a, v*2+1, tm+1, tr);
        merge(t[v*2].begin(), t[v*2].end(), t[v*2+1].begin(), t[v*2+1].end(),
              back_inserter(t[v]));
    }
}

int query(int v, int tl, int tr, int l, int r, int x) {
    if (l > r)
        return INF;
    if (l == tl && r == tr) {
        vector<int>::iterator pos = lower_bound(t[v].begin(), t[v].end(), x);
        if (pos != t[v].end())
            return *pos;
        return INF;
    }
    int tm = (tl + tr) / 2;
    return min(query(v*2, tl, tm, l, min(r, tm), x), 
               query(v*2+1, tm+1, tr, max(l, tm+1), r, x));
}
```

- Find the smallest number greater or equal to a specified number. With modification queries.
store the array in multiset and allows quickly update the element
leetcode practice questions on BIT and segment tree.

### treap (tree heap)
store a pair (x,y) for x it is a binary search tree, for y it is a max heap.
x can be considered as key, and y can be considered as priority.
insert O(logn)
search O(logn)
erase O(logn)
build O(n) for sorted, O(nlogn) for unsorted
union(T1,T2) O(Mlog(N/M)) removing duplicates
intersect(T1,T2) O(Mlog(N/M))

