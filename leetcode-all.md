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

## brief on algorithm and data structure

on algorithm, recursion is most important. It is the base for many powerful techniques such as dfs, backtracking, dynamic programming, divide and conquer et al.
divide and conquer, dp, backtracking is generally hard, especially dp could be very hard.
dfs/bfs/union find is easier.
two pointer, sliding window could be tricky.
greedy: the most difficulty part of greedy is to find the correct first step and prove it is correct. Most first step choice is wrong. Once the first step is found, implementation is usually straightforward.

Among the data structure, hashmap and array could be very tricky.
tree, trie all relates to recursion and could be hard.
heap could be tricky. (nonlinear data structure).
stack could be the most tricky one in the data struture.
queue, deque is generally easier.
string could also be tricky since it has a lot of applications.

misc: math, bit manipulation, Binary index tree, segment tree.

## algorithm focused Problems

major algorithms:
- recursive, backtracking, dfs
- bfs
- greedy,
- dp
- divide and conquer.
- search
- sort.

approaches to solve a problem:
- recursion is most important, it reduces to smaller similar problem, consider it.
- if we can divide into left and right two parts -divide and conquer
- if subproblem overlaps, considering dp..
- if searching for a target, considering binary search
- can use data structure to optimize? hashmap, queue, deque
- can apply greedy first step?
- better complexity? consider const or linear algorithm to reduce one order complexity.


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
at most two transactions. based on previous transactions. Note: top down for this will TLE. it is slower than bottom up (due to stack operations).
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
- for ith char we get its left and right and its contribution is L[i]*R[i]. using hashmap to record all the indices. easier to understand and implement.
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
need memoization! 2d dp: dp[n,k] nth step with ending digit k.
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
i is position and k is times so dp[n,k+1].
*** 
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
idea: two rows count number of columns is 11 and then n*(n-1)/2. reduce from O(N^4) to O(N^3)
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

<<-948	Bag of Tokens    		46.2%	Medium	
given intial power P and score 0 and a bag of tokens. each token has different power and can only be used once. 
- if your current power >=token[i] you can take the token, losing token[i] power and get 1 point
- you can lose 1 point and get token[i]
return the max score.

greedy approach: use the point to get the max power. losing min power to get 1 point.
using two pointer to do it from min and max side.
->>

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
level: ****
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
approach: greedy, based on all 'a' and replace from right as much as 'z'to 'a'. first k-=n, and then do one by one.
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
***
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
using equal_range (binary search to get the number).
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
backtracking is similar to dfs, but it generally include put in and take out since we need to gather information during traverse. The add/removal is simulate the stack process using dfs.
optimization in backtracking is very important.
- value passing and reference passing. passing a big array is extra overhead.
- recursion is especially hard to debug so understand the inside is really important.

- general backtracking.
use recursion or dfs but with recording visited elements in array simulating the backtracking.
It is very important to understand backtracking essential is recursion.
dfs, dp and backtracking are all based on recursion.
dfs and backtracking is not overlapped recursives. dp is overlapped recursions.
Try to solve problem from this spective and avoid many mistakes.
These 3 types of algorithm are all about subproblems.

#### general backtracking.
use recursion or dfs but with recording visited elements in array simulating the backtracking.

<<-22	Generate Parentheses    		64.2%	Medium	
value passing can be more concise, but more time.
need keep left>=right at any time.
**
->>

<<-784	Letter Case Permutation    		65.7%	Medium	
string of letter and digits, you can change to lowercase or uppercase
return all combination
backtracking: no change or change to upper/lower.
**
->>

<<-17	Letter Combinations of a Phone Number    		48.0%	Medium	
each digit corresponds to several letters.  actually a backtracking problem.
->>

<<-967	Numbers With Same Consecutive Differences    		44.1%	Medium	
given n=number of digits, adjacent digit's absolute difference=k. return all numbers.
backtrack.
**
->>

<<-797	All Paths From Source to Target    		78.2%	Medium	
given a directed acyclic graph with n node from 0 to n-1. find all possible path from 0 to n-1.
graph is given as adjacency matrix.
backtracking: need to pay attention to last and first node.
**
->>

<<-1215	Stepping Numbers    		42.7%	Medium	
stepping number: its neighboring digits absolute difference is 1. return a sorted list stepping number in range [L,R]
- backtrack: 
->>

<<-1291	Sequential Digits    		57.4%	Medium	
digit[i]=digit[i-1]+1. return all integers in range [L,R]. return in sorted order.
backtrack and then sort.
**
->>


<<-248	Strobogrammatic Number III    		39.9%	Hard	
a strobogrammtic number looks the same when rotated 180 degree.
count number of strobogrammatic number in range [L,R].
0-0,1-1,6-9,9-6,8-8
backtracking: note there is additional, the digit pairs shall be at symmetric positions.
so we need use two pointer to pair them.
edge case: low=high
compare: length the same, compare string, otherwise compare length.
*****
->>
<<-247	Strobogrammatic Number II    		48.1%	Medium	
find all the numbers of length n.
need pair. 1xxx1,6xx9,9xx6,8xxx8
very similar to 248, using two pointer.
***
->>
<<-246	Strobogrammatic Number    		45.4%	Easy	->>

<<-320	Generalized Abbreviation    		52.9%	Medium	
take any substr and replace with its length.
return all possible abbreviations.
backtracking: use current char or not for abbreviation.
if previous is already num, then we shall take it as char. (use a bool to indicate this)
->>

<<-756	Pyramid Transition Matrix    		55.3%	Medium	
given bottom string and a list of allowed string (the top and bottom two forms the string), we are building a pyramid. return if we can build it.
backtracking: 
- the first two as the key and build a list of 3rd chars.
- try all these combinations.
->>

<<-440	K-th Smallest in Lexicographical Order    		29.3%	Hard	
from 1 to n, find the kth lexi smallest integer.
similar to a tree. K up to 10^9. so cannot use backtrack.
we shall count and skip some.
- first get the max number of digits (for n) which is the depth to go.
- all numbers with less depth shall be counted. (this reduces a lot of countings)
- first we find the path for n for example n=13,k=2, the depth=2
- the same level: count the number of nodes '3'-'0'+1=4 nodes, then we know it is 10
->>

<<-386	Lexicographical Numbers    		52.8%	Medium	
given an integer n generate the numbers in lexi order.
backtracking: first digit shall be 1 to 9, then next chars would be 0 to 9.
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

<<-1601	Maximum Number of Achievable Transfer Requests    		47.2%	Hard	
n buildings from 0 to n-1. Given a list of requests transfering from src to dest. All building is full, so net change shall be 0.
return the max number of achievable requests.
n<=20, requests<=16.
backtracking: each request can be used or not used. 2^R.
valid requests: all building is net 0.
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

#### permutations
permutation: loop to use each element as the first position and then a subproblem
- duplicate: using sort and skip, or using hashmap
- swap.
- permutation will not just go right, it shall loop the same whole range.

<<-47	Permutations II    		48.4%	Medium	
all permutation of array with duplicates.
sort and skip duplicates - ie do not start with the same element, but permutation can include duplicates.
- permutation by swap., tricky.
- sort and skip duplicate with a separate vector, easier.
- using hashmap, this is least to make mistakes.
*****
->>

<<-46	Permutations    		65.2%	Medium	
using next permutation or backtracking.
backtracking: loop swap i to start, and go start+1. It is not trivial.
to avoid mistakes, use a separate array and visited array.
->>
<<-267	Palindrome Permutation II    		36.9%	Medium	
given a string s, return all possible palindrome permutation (without duplicates).
- only need get the prefix. 
- get the hashmap and divide by 2. 
- and then the combination is similar to 254. (actually this is permutation).
backtracking: 
****
->>

<<-266	Palindrome Permutation    		62.3%	Easy	
check if the permutation can form a palinrdome- counting.
->>

<<-1079	Letter Tile Possibilities    		75.6%	Medium	
given a list of letters, return number of different string can form.
permutation with duplicates. n!/(n1!n2!..)
-backtrack. with a hashmap.
->>

<<-996	Number of Squareful Arrays    		47.5%	Hard	
given an array, it is squareful if every adjacent pair sum is a square number.
return number of permutation of A is squareful.
- for each number, find all its paired numbers.
- each number may have duplicates, using hashmap to count.
- backtracking: try each number at the first, and then look for paired number.
****
->>

<<-332	Reconstruct Itinerary    		37.3%	Medium	
given a list of tickets [from,to], reconstruct the itinerary in order start with JFK.
if multiple, return the lexi smallest. You must use the ticket all and once.
- sort the ticket (lexi order)
- build a graph 
- dfs starting from the smallest child. Need make sure one dfs will used all the tickets. (permutation with greedy)
- use visited/to visit to get terminate condition 
- use multiset to store identical tickets.
- be very careful delete element from hashset while looping over it. It will cause crash since the iterator is damaged.
->>

<<-1467	Probability of a Two Boxes Having The Same Number of Distinct Balls    		61.3%	Hard	
given 2n balls with k colors. two boxes, each box put n balls. Two boxes are consider different. The balls are shuffled uniformly. calculate the probability that the two boxes has the same number of distinct balls.
input array A[i] is the number of balls with color i. target: each box get n balls, and the same number of colors.
This is a permutation problem.
- before that we need to know the permutation with duplicates n!/(A1!*....*An!)
- then we only need to count the number of permutation with equal distinct color, which can be obtained using backtracking.
- backtracking is combination approach, after we get a valid combination we need to get the permutation
- backtracking for length n array would be (A+1)^n.
- use double for factorial.
->>

- combinations
combination generally loops over each element and only goes to right.
so the range is keeping smaller.

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

<<-77	Combinations    		56.2%	Medium	
given 1 to n, return all possible combination of k numbers. 
backtracking with k. (start from 1 k-1)
->>
<<-377	Combination Sum IV    		45.7%	Medium	
input has no duplicates, find the  number of combinations that sum=target. (elements can be reused)
- dp: dp[t]+=dp[t-num[i]]
that is why we do not use backtrack here (overlapped subproblem).
- target is large, try using hashmap. Note it actually asks for the permutation.
[1,1,2] is different from [2,1,1]
->>
<<-216	Combination Sum III    		59.4%	Medium	
using digit 1-9 choose k digits so that sum to n, each digit can be used at most once.
return all combination.
- backtrack.
->>
<<-40	Combination Sum II    		49.4%	Medium	
input with duplicates, each number can be used only once, return all combinations.
backtrack: sort and loop each element and skip duplicates, and advance pointer.
->>
<<-39	Combination Sum    		58.1%	Medium	
input is unique, each number can be reused. return all possible combination with sum==target. do not need to advance pointer.
backtrack: loop the element as the first and process to the right.
->>

<<-254	Factor Combinations    		47.0%	Medium	
return all possible combination of its factors. (no duplicate combinations)
backtrack: for n: add i n/i to complete a candidate, then use n/i for the subproblem.
see 1735 count ways to make array with product, which limit the number of factors.
it keep get all 2 factors, and then get all 3 factors...
****
->>

<<-416	Partition Equal Subset Sum    		44.3%	Medium	
- dfs for k=2.
- dp knapsack: choose or not choose. This is a special case for 698.
target 0 for all array length is true.
choose or not choose. (do not forget this)!!!
****
->>

<<-698	Partition to K Equal Sum Subsets    		45.3%	Medium	
backtracking: since the length is small, we can use bitmask to indicate which one is used.
complexity: will choose or not choose for one grouping. O(k2^n)
can also do dp: dp[mask|(1<<i)]=dp[mask]+nums[i].
****
->>

<<-473	Matchsticks to Square    		37.9%	Medium	
given a list of stick length, check if we can make a square using all the sticks.
need divide 4 equal parts with sum=tsum/4.
this is exactly the problem: 698 partion into k equal subset
- dfs or backtracking using mask. do not forget the loop.
->>

<<-1286	Iterator for Combination    		70.7%	Medium	
given string, length k, return the the next combination of length k.
backtrack
->>

<<-638	Shopping Offers    		52.1%	Medium	
given a list offers [na,nb,price], and [pricea,priceb] if buy separately. and target number [Na,Nb]
you can use offers repeatedly. return the min cost to buy exactly the numbers.
backtracking: with vector operations, could use dp for memoization. it is a variation of combination sum problem (you can reuse the item).
->>

<<-491	Increasing Subsequences    		46.9%	Medium	
find all increasing subsequence with length>=2
backtracking: using hashset to avoid using it repeatedly. skip duplicates. (starting duplicate and goes right will get same sequence).
similar to dfs once the search is done, we need set it visited. (combination to avoid duplicates)
****
->>

<<-1258	Synonymous Sentences    		67.2%	Medium	
given a list of synonymous pairs, return all possible sentences in sorted order.
- union find the get the disjoint set for each synonymous word
- dfs/backtrack to get all combinations
->>

<<-1735. Count Ways to Make Array With Product
use factor combination and generate the combinations in n slots.
similar to 254.
->>

- try all prefix and solve subproblem.
these may introduce overlaps and can use memoization and becomes dp problems.

<<-282	Expression Add Operators    		36.2%	Hard	
given a digit string. add +-* so that it evaluates to target.
return all possible string.
- backtracking: prefix 1 to n, and remaining apply +-*
- subproblem: note * will need previous result, so we need save previous results.
- try all prefix (valid)
- then try all position to get the 2nd operand
***** hard to implement because of *, you need get previous value as the operand.
->>

<<-290	Word Pattern    		38.1%	Easy	
string separated by space, mapping is simple. just check mutual mapping.
->>

<<-291	Word Pattern II    		43.8%	Hard	
given a pattern and a string, check if s matches the pattern.
mapping relation: each char in pattern can match a string in s.
one way is to try all possibilities.
- pattern must contain duplicates, otherwise we can always map true.
- char to str map, str to char map and they cannot change.
- try all prefix to the first char match.
backtracking + hashmap.
important: different char shall map to different strings.
"sucks"
"teezmmmmteez"
s map to teez
u: map to mm.
c: map to m.
k: map to m. (which is invalid)
problem: the backtracking pair shall happen when there are new inserts. If the mapping already exisit, we cannot pop them.
****
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

<<-131. Palindrome Partitioning
return all possible palindrome partitioning.
typical backtracking, from left to right.
- try all prefix leaving a subproblem
- if the suffix is a pal, add it to answer
****
->>

<<-1593	Split a String Into the Max Number of Unique Substrings    		46.7%	Medium	
backtracking using hashset.
take from one char to the whole string and leave a subproblem.
- try all prefix 
- using hashset to record 
- only needs the number, it is more dp than backtracking. (they are essentially the same, trying all possibilities, dp with memo.)
****
->>

#### board game

<<-52	N-Queens II    		59.1%	Hard	
place n queens on nxn chessboard. return the number of distinct solutions.
- the first row choice will affect the next row. 
- backtracking: check next step is valid or not and then push/pop.
- also can use bitsets to represent the place.
->>

<<-51	N-Queens    		48.2%	Hard	
return all distinct solutions
backtracking.
->>

<<-756	Pyramid Transition Matrix    		55.3%	Medium	
given bottom string and a list of allowed string (the top and bottom two forms the string), we are building a pyramid. return if we can build it.
backtracking: 
- the first two as the key and build a list of 3rd chars.
- try all these combinations.
- form a hashmap the first two char vs last char.
- try each two chars (subproblem) and goes to next two char
- you can first do the dfs to get all next combination or do it a 2d dfs.
2d backtracking: 
- loop over each char, and procceed to right. 
- reach the end, try a reduced new bottom.
****
->>

<<-1307	Verbal Arithmetic Puzzle    		37.6%	Hard	
each character represents a digit from 0 to 9. and given an equation, check if it is solvable.
a 2d backtracking problem with rows and cols.
- using hashmap or vector to record letter digit mapping and used digits.
- reverse each row so that addition is more convenient.
- do it from first column (all rows) and then process to next cols with cf.
approach:
- first char not allowed to map to 0. - notallowed
- d2c and c2d hashmap (using array is faster + used)
- reverse left and right
- need row,col, cf.
- col reaches result size and cf=0, success.
- reach row size, 
if row sum %10 == result mapping, go to next col.
if result not mapped, add the mapping and do push/pop + backtracking.
- for each row, search for next available mapping.
some edge case shall be avoid first: result shorter than left, single digit can allow to be 0.
*****
->>


<<-425	Word Squares    		49.4%	Hard	
kth row and column reads the same. 
given a set of words, find all word squares you can build.
(all words have the same length <=5)
- backtracking: 
- try each string at the first col/row. 
- then 2nd col/row shall start with given str. similar for others -- this is what trie does.
build a trie and do backtracking on it.
- trie, with word index (we need actually return the array)
- start with any words (first row)
- we search to get all candidates with the required prefix.
*****
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
***** still did not pass.
->>

<<-36 valid sudoko ->>
<<-37 soduko solver ->>

### dfs
- dfs to group connected
- dfs avoid visiting parents
- dfs for all paths
- dfs with rank to detect cycles.
a lot of cases can also be done via bfs, union find, backtracking.
backtracking almost the same as dfs, but backtracking generally involves some prune.
both dfs and backtracking are recursion based and involves some tree searching.

just as the name depth first search, dfs is mainly used for pattern searching.

most dfs has a tree like structure.

### searching paths.

<<-688	Knight Probability in Chessboard    		49.6%	Medium	
dfs: out of board, prob=0, k=0 and inside board=1.
need memoization for large board. -- dp since a lot of overlapped subproblem.
->>

<<-1377	Frog Position After T Seconds    		34.2%	Hard	
given an undirected tree with n vertices from 1 to n. Starts at 1, it can jump to direct connected vertices, cannot go back to visited nodes. It randomly jump to its neighbors with same probability. 
given time t and target node, find the probability on the target at time t.
dfs: decrease t and get its neighbors
leaf node: 1/0
->>

<<-79	Word Search    		36.2%	Medium	
given a board and a string, check if the string exist in board
plain dfs- visited change to other char to avoid revisit, after done change it back.
*** 
->>

<<-212	Word Search II    		35.9%	Hard	
given a board with char, and a list of dictionary words. find all the words in the board. (4 direction connected).
- build the trie using the list of words
- dfs on the board and find in the trie.
- root actually is parent node.
- now replace word using trie root since we have a list of nodes 
- avoid go further if there is no such child.
*****
->>

<<-329	Longest Increasing Path in a Matrix    		44.0%	Hard	
do dfs and save longest path in dp.
***
->>

<<-341	Flatten Nested List Iterator    		53.7%	Medium	
dfs to flatten the nested integer. leaf is integer.
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

### connected sets in graph or 2d matrix.
can also use union-find. generally is easy.

<<-417	Pacific Atlantic Water Flow    		41.9%	Medium	
dfs with two states.
->>

<<-130	Surrounded Regions    		28.8%	Medium	
dfs (connected to boundary are not counted)
->>

<<-200	Number of Islands    		47.9%	Medium	->>
<<-1020	Number of Enclaves    		58.5%	Medium	
find those cannot connect to boundary
dfs
->>

<<-695	Max Area of Island    		63.7%	Medium	
dfs calculate the number of nodes in each set.
->>

<<-1254	Number of Closed Islands    		61.1%	Medium	
0 is land, 1 is water.
closed island are those 0s which does not touch the boundary.
dfs: mark if the dfs touched the boundary. or first dfs using boundary 0s to change to 1. and then dfs inner nodes of 0.
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
****
->>

<<-302	Smallest Rectangle Enclosing Black Pixels    		52.1%	Hard	
given a binary grid, 0 white, 1 black.
black pixels are connected and only one region. return the smallest rect enclosed all black pixel.
get the xmin,xmax,ymin,ymax of the pixel region
->>

<<-959	Regions Cut By Slashes    		66.6%	Medium	
upscale 3 by 3 and do bfs/dfs.
->>

<<- 1706. Where will the ball fall
the grid with / and \,
upscale the grid by 3, and then use bfs or dfs to find which one to go. bfs is better (for implementation)
->>

<<-934	Shortest Bridge    		49.2%	Medium	
shortest bridge between two island.
group all positions in each set and try the distance.
dfs to two groups.
->>

<<-749	Contain Virus    		47.1%	Hard	
given a 2d grid, 0 uninfected, 1 infected. a wall can be installed between ajacent cells. Each nigh the virus spreads to its neighboring cells. Each day you can install walls around only one region- the affected area the threatens the most unaffected cells.
if it is possible to save? number of walls required?
- need dfs to get region and number of cells to be infected. = number of walls
- growing of all other regions.
- quarantine region can mark as -1 so no dfs will be done.
- using dfs/bfs/union-find to save each set's infect list.
- store information in heap.
****
->>

### search for paths.

generally need some prunes to avoid TLE.

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


<<-489	Robot Room Cleaner    		71.5%	Hard	
given robot API: move, turnLeft,turnRight, clean
design algorithm to clean the entire room.
simulate the dfs. from current position and goes one direction until blocked and then try other direction. going back one step.
since we do not know the matrix, using hashmap to record visited cell
- clean current cell and mark visited.
- loop 4 directions
- dfs to clean all cells along current direction.
- back off one cell and try other directions.
*****
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

<<-980	Unique Paths III    		77.0%	Hard	
from start to end, 0 can go, -1 cannot go.
return number of paths what walk over every empty cell exactly once.
dfs: visited cannot visit again, final nstep is the total empty cell.
->>

<<-1219	Path with Maximum Gold    		65.6%	Medium	
mxn matrix with gold, 0 means you cannot cross. You cannot visit the same cell again.
you can start and stop any place.
- dfs: at any position with gold do dfs, visited changed to 0. and find the max.
->>

<<-403	Frog Jump    		40.6%	Hard	
first jump is 1, if last jump is k, then next jump could be k-1,k,k+1.
check if frog can land on the last stone.
- bfs: put all possible position in queue
- dfs: check if any works prune.
- dp:  
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

### coloring.
<<-733	Flood Fill    		55.6%	Easy	
start (sr,sc) and flood fill with a new color. dfs.
->>

<<-886	Possible Bipartition    		44.7%	Medium	
dfs coloring alternatively.
->>

<<-1034	Coloring A Border    		45.1%	Medium	
same value and connected. border has at least a neighboring cell which is not the connected group.
dfs to mark negative as visited and then check all negatives and see if they have a cell not in the group.
->>

### cycles.
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


### bfs
- bfs with queue
- bfs with deque pop from both end
- bfs with priority_queue
- bfs with parent information
- bfs with more than one state.
- bfs with shortest distance problem

#### shortest distance with queue (no weight) from source to dest.

<<-127	Word Ladder    		30.7%	Medium	
shortest change from one string to other string in dictionary each time change one char.
bfs: change each char and check if it is in dict, and push in the queue.
->>
<<-126	Word Ladder II    		23.0%	Hard	
find all possible transform with shortest path.
need parent information to trace back using bfs.
->>

<<-433	Minimum Genetic Mutation    		42.6%	Medium	
a gene string is 8 char ACGT, mutation is one single char changed.
given a bank of valid gene strings. given a start string, return the min number of permutation to change to the end string.
bfs. this is same as 127 word ladder.
->>

<<-286	Walls and Gates    		55.5%	Medium	
given a grid, -1: a wall, 0,a gate, inf: empty room.
find the nearest distance of empty room to gate (to the nearest gate)
bfs: gate to empty cell. first fill int-max, the first reach is the nearest distance.
->>

<<-317	Shortest Distance from All Buildings    		42.1%	Hard	
given a grid, 0 means empty, 1 building,2 obstacle.
find an empty cell which has the shortest distance sum to all building.
bfs from all other building and add the distance for each empty cell.
- can do bfs for each building, simpler. using hashmap to record the distance sum. non-reachable cells are deleted.
- do bfs all, we need to have n (if visited n times, no more visit - too complicated, not recommended)
optimization: if one cell is not reachable from any building, do not consider it.
every time we update the map.
****
->>

<<-675	Cut Off Trees for Golf Event    		34.9%	Hard	->>

<<-752	Open the Lock    		52.3%	Medium	
GIVEN 4 circular wheel with 0-9 on it. You can turn around the wheel, each move turn the wheel one digit. lock initial is 0000. You are given a list of deadends that the wheel stops turning.
given a target, return the min total number of returns.
bfs. you can rotate clockwise or counterclockwise.
->>

<<-773	Sliding Puzzle    		60.2%	Hard	
given 2x3 board with 1-5, return min number of moves to solve the board.
bfs: define the possible direction to go, each position has specific next position to go.
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
given a binary matrix, 1 means blocked. You can go 8 directions. return the shortest path from top left ot bottom right.
regular bfs.
->>

<<-1129	Shortest Path with Alternating Colors    		39.6%	Medium	
assemble the edge with odd/even property and do bfs.
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

#### bfs in graph or tree.

<<-210	Course Schedule II    		41.7%	Medium	
given a list of prerquisite, return the ordering of course taken
bfs: remove source nodes layer by layer.
->>
<<-207	Course Schedule    		43.8%	Medium	
given a list of prerquisites, check if we you can finish all the courses.
dfs: no cycle. detect if there is a cycle.
->>

<<-1136	Parallel Courses    		61.1%	Hard	
given a list of prerquisitites relations. In a semester you can study any number of courses if cleared. return the min semester needed.
- put all sources in queue and clear them and do bfs.
->>

<<-301	Remove Invalid Parentheses    		44.1%	Hard	
remove min number of parenthesis to make it valid, return all possible combinations.
backtracking: ???
bfs: if we find the first invalid, we shall stop adding stuff into queue.
try remove each ( or ). slower than backtracking.
->>

<<-527	Word Abbreviation    		55.5%	Hard	
given n strings, generate abbreviation for each word with min length
- begin with first char followed by number of digits + last char
- if conflict using longer prefix
- if cannot keep abbreviation shorter, keep the original.
bfs: first round, remove those string which is unique using first and last.
using hashmap to save the ambiguous strings. the use longer prefix for the remaining.
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
two strings are similar if we can swap two letters in x so that it equals y.  (same string are also similar)
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
merge sort and count. (count in linear time)

<<-315	Count of Smaller Numbers After Self    		42.3%	Hard	
- from right to left and store the elements in sorted order then we can use binary search. but count the higher is still O(N^2) will TLE.
- merge sort: divide and conquer, count right smaller than left. (during a stable sort, the number of smaller elements is exactly the jump from its right to its left. so we use merge sort recording the jumps. O(nlogn).
divide into left and right parts and do merge and count. (when sorted, the counting is easier)
- binary tree: from right to left, build a BST. Since we may have duplicates, add a field to count the duplicates. when we insert a new value, all its left subtree are smaller than it. We use prefix sum to count the sum of duplicates under the subtree.
*****
->>

<<-493	Reverse Pairs    		26.0%	Hard	
impotant reverse pair i<j, nums[i]>2*nums[j]
return number of important reverse pairs.
- using map to store previous visited elements, and binary search O(N^2) since the distance is O(N)
- segment tree.
- merge sort divide and conquer. divide into left and right part and then use two pointer to compare O(n/2)+O(n/4)+....
*****
->>

<<-327	Count of Range Sum    		35.7%	Hard	
return the number of range sums in [L,R].
prefix sum, and save in map: presum vs list of index.
then use binary search to get the lowerbound and upperbound. O(N^2)
other approach:
merge sort: prefix sum and divide into two parts, count the right part - left part in the range. using three pointer to find the region for each element in left. (note prefix shall add 0, and segment needs have at least 2 elements)
binary index tree:
*****
two sorted array counting. two pointer or 3 pointer.
->>

<<-932	Beautiful Array    		60.9%	Medium	
given permutation of 1 to n. rearrange so that for every i<j there is no k i<k<j with 2*A[k]=A[i]+A[j]
idea: A[i]+A[j] must be odd. keep the odd and even separated. If the odd is beautiful then the even follow the same pattern is also beautiful.
- divide and conquer
- first step: divide into odd and even
- further divide using odd, even index elements: rearrange odd indexed elements all then followed by even indexed elements.
another approach:
if A is beautiful, then 2A, 2A+1, 2A-1 is also beautiful.
[1], 2*i-1 ->1 2i->2 [1,2]
[1,2] 2*A-1->[1,3], 2i->[2,4] -->[1,3,2,4]
This is also reverse thinking. If it is hard to rearrange, why not build it from beginning.
*****
->>

<<-95	Unique Binary Search Trees II    		41.6%	Medium	
generate all trees for n node from 1 to n. 
divide and conquer, or recursive. choose root= 1 to n. and then left shall be all smaller elements in array and right shall be the larger elements in the array. 
note: 
- when 0 node, return a vector containing null.
- if use i as root, then left is 1 to i-1 totoal i-1 nodes. right from i+1 to n, total n-i nodes
- then left and right combine.
***
->>

<<-395	Longest Substring with At Least K Repeating Characters    		41.9%	Medium	
each char in the substring appear >= K times.
- divide and conquer: the char appear less than k times will not be a part of the solution, so we can divide it into several parts, and solve them independently. (do not forget the last segment)
- base case: length<k, all char >=k.
***
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

## two pointer

two pointer is base for sliding window and some O(N) algorithm in array or two arrays.
two pointers in sorted array or two sorted array (used in divide and conquer to reduce complexity)
some variations: from same end (left or right), one from left one from right, 3 pointers, 4 pointers et al.
using 2 pointers keep i<=j the restrictions.

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

<<-1023	Camelcase Matching    		57.1%	Medium	
given a list of string, and a pattern string, if you can insert 0 or more lowercase letters into pattern and make two strings equal, we call matched.
- capital shall match exactly.
- using two pointer matching. if capital does not match, return false.
->>

<<-1055	Shortest Way to Form String    		57.2%	Medium	
given string src and target, return the min number of subsequence used to reach target string.
two pointer, with one using mod.
->>

<<-11	Container With Most Water    		51.8%	Medium	
two pointer: water determined by the min height * the width.
one from left one from right, when curr bar is min bar, move the pointer.
->>

<<-18	4Sum    		34.3%	Medium	
a+b+c+d=target from the input array. find all quadruples.
to use two pointer, must sort first. skip duplicates. use i,j,k,l fix i j and then solve two pointer problem in linear time.
O(N^3)
->>
<<-16	3Sum Closest    		46.2%	Medium	
three pointers.
->>

<<-259	3Sum Smaller    		48.4%	Medium	
find the number of triplet that sum < target.
sort and then use 3 pointer.
>=target k--
<target ans+=k-j, j++ (fix i, and solve the two pointer problem)
->>

<<-15	3Sum    		27.4%	Medium	
three pointers.
->>
<<-1	Two Sum    		45.8%	Easy	
hashmap for O(N), non-sorted. 
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

<<-68	Text Justification    		28.7%	Hard	
given a list of words, and maxWidth, padding space so that each line is exactly maxWidth.
extra space shall be distributed evenly. if not divisiable, the left will be assigned more.
each line: starting word and last word has no spaces. When a word cannot add to the line, goes to next line
two pointer: pay attention to the last line. It is not that easy.
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
****
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

<<-1537	Get the Maximum Score    		36.1%	Hard	
two sorted array, a valid path (at the same value you can switch to the other array) return the max path sum.
- path can begin from nums1 or nums2, end at nums1 or nums2.
- choose the max sum branch.
- need to find the common segments.
using merge sort with two pointer. wait until we see identical number. choose the previous max.
***
->>

## sliding window

sliding window sometimes is tricky, especially when combined with other stuff, such as priority_queue, dp.
- fixed sliding window problem - generally easy, some problems (especially containing unrelated elements) can be converted to fixed window.
- two pointer: for min window, grow right until valid, then shrink left. for longest window, grow right until invalid and then shrink left.
- for counting number of valid window, get the max and min valid window.
- also there are other shrinking: for example find the valid window and then we go right to left and find the min window (without using extra data structure). for example subsequence.

### fixed window.
fixed window size sliding is generally easy and not much tricks.
also some times we discard those non-related elements and do fixed window on the new array, and change the variable size to fixed window.
if we can do fixed window, try to use fixed size. (variable window is tricky and easy to make mistakes)

<<-567	Permutation in String    		44.5%	Medium	
check if s2 contains the permutation of s1 as a substring.
fixed window sliding, + hashmap (using vector) when all 0 then we get a valid answer.
**
->>

<<-438	Find All Anagrams in a String    		44.3%	Medium	
find p's anagrams in s.
fixed window size sliding + hashmap.
**
->>

<<-134	Gas Station    		40.5%	Medium	
n gas station along a circular route. ith station with gas[i] and it cost cost[i] from station i to i+1.
you begin with empty gas. return the starting index that you can finish the whole route. (clockwise)
- sliding window with n. net gas= gas[i]-cost[i] at any time shall >=0
- unfold the circular array to 2N array.
for example gas=[1,2,3,4,5],cost = [3,4,5,1,2]
the net=[-2,-2,-2,3,3]
unfold net=[-2,-2,-2,3,3,-2,-2,3,3]
i=0, -2, skip
i=1, -2, skip.
i=2, -2, skip
i=3, valid  --found the answer.
This is O(N^2) and need to be optimized.
- we only need check the min>=0
i=0, prefix sum=[-2,-4,-6,-3,0] min=-6
i=1: prefix sum+2=[-2,-4,-1,+2,0] min=-4
i=2: prefix sum+2=[-2,1,4,2,0] min=-2
i=3: prefix sum+2=[3,6,4,2,0]
greedy: find the prefix sum min ind, answer is ind+1. (can have math proof)
***
->>

<<-1343	Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold    		64.3%	Medium	
fix size sliding window.
->>

<<-1100	Find K-Length Substrings With No Repeated Characters    		73.4%	Medium	
find number of substrings. hashmap sliding window.
->>

<<-1461	Check If a String Contains All Binary Codes of Size K    		46.3%	Medium	
k=2，00，01，10，11.
so using sliding window to get the number and set the flag.
->>

<<-1052	Grumpy Bookstore Owner    		55.5%	Medium	
x minutes window size.
sliding window to find the max grumpy 
->>

<<-1176	Diet Plan Performance    		53.9%	Easy	
for k days, if the sum <L, get -1, if sum>R get 1, otherwise get 0.
fixed size sliding window.
->>

<<-1248	Count Number of Nice Subarrays    		56.6%	Medium	
find number of subarrays with k odd number inside.
sliding window: only keep the odd numbers with fixed k window.
->>

<<-1423	Maximum Points You Can Obtain from Cards    		45.1%	Medium	
take cards from either end, return the max points taking k cards.
equivalent: sliding window to get min sum using n-k.
->>

<<-1456	Maximum Number of Vowels in a Substring of Given Length    		53.5%	Medium	
a substring of length k, return the max number of vowels.
sliding window with size k.
->>

### min/max window problem using two pointer.
generally using two pointer or three pointers to grow and shrink the window.
for min window problem: grows right to find a valid window and then shrink to get the min window
for max window problem: grows right to get a invalid window and then shrink to get the max window
for min+max window problem: need 3 pointer to find the min and max window.

<<-76	Minimum Window Substring    		35.4%	Hard	
min window substring which contains all the characters in t.
- we can ignore chars not in t and maintain hashset 
- sliding window: (this string is not limited to lowercase letters)
two pointer: grows j, when we found a substr satisfying the condition, keeps shrinking window until invalid.
min window.
***
->>

<<-159	Longest Substring with At Most Two Distinct Characters    		49.8%	Medium	
using hashmap+sliding window.
- when map size >2, keep shrinking the window. for valid get the window size.
min window
***
->>

<<-340	Longest Substring with At Most K Distinct Characters    		44.7%	Hard	
two pointer, when hashmap size >k, keep shrinking the left. (greedy we shall keep growing to get longest)
min window
***
->>

<<-904	Fruit Into Baskets    		42.7%	Medium	
you have two baskests, each basket holds one type of fruit. You start at any index of the array. if you cannot choose, stop. each tree you can pick one fruit.
return max amount of fruits
equivalent find the longest subarray with only two types of fruit. (or with two distinct number) 
sliding window + hashmap.
max window
->>

<<-209	Minimum Size Subarray Sum    		38.9%	Medium	
find the min length subarray sum >=target. （input all positives)
hashmap or sliding window: keep adding while >=s then shrinking the left.
***
min window
->>

see why min window subarray sum >=target with input having negatives cannot use sliding window since it is not sorted.
862	Shortest Subarray with Sum at Least K 

<<-424	Longest Repeating Character Replacement    		47.7%	Medium	
return the longest repeating subarray after performing at most k char replacement.
equivalent: find the longest window containing at most k other chars.
- counting char
- make sure in the subarray w-maxcnt>k count the max using hashmap (vector) and find max is O(26).
max window.
****
->>

<<-485	Max Consecutive Ones    		53.5%	Easy	->>
<<-487	Max Consecutive Ones II    		47.9%	Medium	
you can flip 0 to 1 at most once. find the longest window of all 1.
sliding window: record previous previous 0 position and update the length.
simpler: using two pointer, count 0, if >1, then shrink left. (instead record the 0 position)
max window.
->>
<<-1004	Max Consecutive Ones III    		60.1%	Medium	
change 0 to 1 for at most k time.
equivalent: find the longest window containing at most k 0s. (use cnt0)
***
max window.
->>

<<-1493	Longest Subarray of 1's After Deleting One Element    		58.6%	Medium	
sliding window with at most one 0 inside. record previous 0 position.
max window.
->>

<<-1208	Get Equal Substrings Within Budget    		42.9%	Medium	
two equal length string s and t, we want to change s to t by same locations at the cost of |s[i]-t[i]|.
given a maxcost, and find the longest substring <=maxcost.
sliding window.
max window.
->>

<<-1695. Maximum Erasure value.
given an array of positive numbers, erase a subarray containing unique elements. return the max sum of the erased subarray.
using hashmap or hashset. (using hashset, when you add an element which is present in hashset, keep popping left elements until the element is not present).
****
max window.
->>

<<-992	Subarrays with K Different Integers    		49.9%	Hard	
find the number of subarray (the subarray has exactly K different integers).
sliding window with hashmap:
- three pointer, i the leftmost for a valid subarray, j, the left pointer for the min subarray, k the right.
- shrinking j must make sure mp[A[j]]>1, otherwise we make it not equal =K.
min+max window.
****
->>

<<-1358	Number of Substrings Containing All Three Characters    		60.0%	Medium	
a string consists of only a,b,c. find number of strings containing all 3 chars.
- we only need to find the smallest window containing the 3 chars. and all previous including the window are all valid substrings.
sliding window to find the min window containing abc. all prefix. two pointer.
min + max window
->>

<<-713	Subarray Product Less Than K    		40.3%	Medium	
return number of subarray
to avoid overflow, we have to use sliding window using two pointer.
->>

<<-1040	Moving Stones Until Consecutive II    		53.3%	Medium	
min move and max move
sliding window for min: find the window with length=n with min empty slots or max filled.
greedy for max: move the leftmost to next right available position or rightmost to left available slot.
->>
<<-1033	Moving Stones Until Consecutive    		42.6%	Easy	
3 stone piles only at different positions. return the min/max moves to make them consecutive.
->>

<<-1151	Minimum Swaps to Group All 1's Together    		58.7%	Medium	
equivalent to find the window with most 1s. window size=total number of 1s.
num of swap = num1s-max1s
**
->>

<<-1156	Swap For Longest Repeated Character Substring    		47.7%	Medium	
you are allowed to swap two chars. 
equivalent: find the longest window with only one char different from the other one. You need has at least one outside the group.
try each index as the repeat char and expand the window.
i as the start index and expand the subarray.
actually we need try each char as the most frequent char in the subarray and then try each index using two pointer so the complexity would be O(26n).
****
->>

<<-1695. Maximum Erasure value.
given an array of positive numbers, erase a subarray containing unique elements. return the max sum of the subarray.
using hashmap or hashset. (using hashset, when you add an element which is present in hashset, keep popping left elements until the element is not present).
->>

<<-1163	Last Substring in Lexicographical Order    		35.7%	Hard	
or the max substring.
a string is larger: prefix is larger or it is longer.
is the first largest char suffix string the last string?
not really, for example "cacacb", the max is "cb"
build a trie and the rightmost branch is the answer.
- the answer is the suffix string with max char as the first char. (so we can get rid of those starting char not the max char, but worse case will not improve)
- if we compare the n suffix string, we will get O(N^2)
- using two pointer: 
i the best candidate position,
j loop over following chars
k: the length or following chars.
if s[j+k]>s[i+k] then i is not best, move to j.
*****
->>


<<-1703. Min adjacent swaps for k consecutive ones.
in one move, you can swap two adjacent elements (01 array). return min number of moves to have exactly k consecutive ones.
equivalent: find the min window size containing exactly k ones.
sliding window with some tricks
- if we slide window on original array, we need binary search to find the size first.
- if we only slide window on the one's index, then it is fixed k size sliding window.
- then the price is to move all 1 to the median position. we are looking for the min.
all left ones move to left median and all right ones move to right median (left or right could be the same for odd length k)
- note the result depends on the inner distribution so need to calculate the moves in each window.
- do not need to save original values, only index is fine. then we get the prefix sum on the index array.

why? this is similar to best meeting point. for two, any between point will get the same cost. for 3, keep the median unchanged and two moves to the median position)
however, we do not need to move to the median position and that is why we need subtract the extra
which is k*(k+1)/2/2.
*****
hard!!!
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


## sort
- sort makes things easier.
- sort using given order.
qsort using random pivot
merge sort.
bubble sort.
count sort.

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

## bit manipulation
- use bit to represent a different object to simplify (person, skill, ...)
- bit manipulation on integers as a whole (32 bits do the same time)
- extend bits to other base system

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
- general stack: first in last out.
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
../ go upper ./ current, stack to store the path hiearchy, using the / as separator.
***
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

<<-1628	Design an Expression Tree With Evaluate Function    		86.0%	Medium	
given an virtual class interface and postfix string, build the tree and evaluate it.
class design and oop and tree evaluations
- build the tree: [4,5,7,2,+,-,*] it is stack form 4*(5-(7+2))
- use a stack to store the generated treenode.
- inherit a class from the virtual class.
subject: tree, stack, OOP, polymorphism
***
->>

<<-1003	Check If Word Is Valid After Substitutions    		55.6%	Medium	
given a string s, with only 'a','b','c'. check if you can build t by adding "abc" any times.
equivalent: eliminate "abc" and make it empty.
using stack or deque is more convenient since it needs check two chars.
->>

<<-1028	Recover a Tree From Preorder Traversal    		70.3%	Hard	
preorder traversal to get the serialization string, with (dashes), number of dashes=depth+value.
if only one child, it is the left child.
iterative stack: store node and depth. When top depth larger than current, pop it.
add a - at the end for convenience.
->>

### monotonic stack

monotonic stack/deque is often used to implement O(N) algorithm (generally need O(N^2) otherwise).

when need push/pop from both ends, we need use monotonic deque.

this topic is generally hard, and not intuitive. It is critical to understand why we do this.

### one direction: next/prev smaller or larger.

This is often easier, each element is pushed once and popped once. and required information is obtained while popping.

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

<<-1019	Next Greater Node In Linked List    		58.2%	Medium	->>

<<-1475	Final Prices With a Special Discount in a Shop    		75.0%	Easy	
if you buy prices[i] you can get discount at price[j] where j>i and prices[j]<prices[i], j shall be minimized.
stack: find next smaller.
->>

<<-1063	Number of Valid Subarrays    		71.7%	Hard	
valid subarray: leftmost element in the subarray <= other elements in the subarray. 
so the element shall be the min and we are looking for next smaller for each element.
monotonic increasing stack.
->>

<<-1081	Smallest Subsequence of Distinct Characters    		53.3%	Medium	
given a string, find the smallest subsequence with distinct characters.
using monotonic increasing stack, when we see a smaller char behind we pop from stack (if the char has no more behind we cannot pop it)
****
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
*****
->>

### get next smaller/larger and previous smaller/larger
generally involves to find a range or a window. (can be solved using sliding window with O(N^2)).

<<-334	Increasing Triplet Subsequence    		39.9%	Medium	
check if there exist i<j<k and A[i]<A[j]<A[k] (123 pattern)
similar to 132 pattern. s1 to be the left min.
idea: keep monotonic increasing stack, so that A[j] is as small as possible yet keep A[j]>A[i], then if we find current element > stack.top, we find one.
***
->>

<<-456	132 Pattern    		30.5%	Medium	
i<j<k and nums[i]<nums[k]<nums[j]
stack: similar to 334. if we want to do from left, we need keep nums[i] as small as possible and nums[j] as large as possible. if we do from right to left, we want to keep nums[j]>nums[k] and nums[k] as large as possible. this is a decreasing stack. (current one is larger, pop smaller ones).
from right to left, keep the stack the largest in the right, so when we see current val > stack top, we assign s3=stack top. then we see a number < s3, we know curr<s3.
*****
->>

<<-907	Sum of Subarray Minimums    		33.3%	Medium	
sum of all subarray min.
each element can be min. using stack find previous smaller and next smaller to get the range. if left length is L: we can have L*A[i]
- keep an increasing stack, if A[i]<stack top, we know the top element's right smaller. and stack increasing, so stack top element is A[i]'s left smaller.
That is the regular trick to get previous smaller next smaller using one monotonic stack.
***** 
->>

<<-42	Trapping Rain Water    		50.1%	Hard	
monotonic stack: decreasing stack, when current bar is taller we get the water (a v shape).
for each bar, we need find the next greater and prev greater to get the water.
key idea: each bar as the lowest bar and calculate the water it holds.
****
->>

<<-84	Largest Rectangle in Histogram    		36.0%	Hard	
find the largest rectange (think sorted, we need check each one as the lowest bar)
idea: area is min(height(i..j)*(j-i+1)) use each bar as the min bar and calculate the area, so we need to get previous smaller and next smaller. (so uses a increasing stack). the stack top element is the element with left and right smaller.
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
*****
->>

<<-85	Maximal Rectangle    		38.7%	Hard	
row by row check histogram.
->>

<<-1124	Longest Well-Performing Interval    		33.0%	Medium	
change 9 to 1 and <8 to -1, then we are looking for the longest subarray sum >0.
subarray sum can be done using prefix sum. 
can generalize to subarry sum>target.
- using ordered_map, we need check all smaller than current one. complexity would be O(nlogn)
- using stack. keep the new lowest prefix into stack, and then from right, while current is larger than top, pop it and get the answer. 
- using hashmap to record the prefix sum vs first appear index. if prefix>0, then the whole prefix is the valid subarray. if prefix<0, if we have not seen it, record it, otherwise we check score-1 (since the score start from 0, and -1 must appear than -2. (pretty tricky)
->>

### recursive stack

<<-1597	Build Binary Expression Tree From Infix Expression    		63.6%	Hard	
expression tree: leaf nodes are numbers, inner nodes are +-*/.
infix expression: in order traversal of the binary tree
given an input string with (), produce a expression tree with equal infix (omitting the ())
typical syntax analysis using stack.
using vector to store the node* is better for processing.
->>

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
deque monotonic. using index to pop out out of window elements, keep an increasing deque.
similar for sliding window min
sliding window median using heap.
****
->>

<<-862	Shortest Subarray with Sum at Least K    		24.9%	Hard	
find the length. input array may contain negatives.
- prefix sum and record its index and sorted in map then binary search. O(nlogn)
- use deque to get O(N). we are looking for previous prefix sum prej<=prei-k.
monotonic decreasing deque (larger and old value are discarded since they cannot contribute shorter length). The deque front shall be the min so it is a decreasing deque)
- why we cannot use sliding window by growing and shrinking? because it contains negative and shrinking will stop from getting the min window. If check all previous then it will get TLE.
*****
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
or: copy node by node and form a list, then assign the random pointer, then break into two lists.
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


<<-594	Longest Harmonious Subsequence    		47.4%	Easy	
harmonious array: the max min difference is 1.
use sorted hashmap to get the frequency and then for each combine with -1.
***
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

### BSTs
<<-938	Range Sum of BST    		82.6%	Easy	->>

<<-1382	Balance a Binary Search Tree    		75.7%	Medium	
convert to sorted array and then do merge sort
->>

<<-98	Validate Binary Search Tree    		28.2%	Medium	
use min/max recursively. or O(N) in order traversal.
***
->>

<<-285	Inorder Successor in BST    		41.7%	Medium	
- using as normal tree found p and next node (it will keeps going)
given a val, find its next greater.
binary search. val<root, goes to left, else goes to right.
```
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        //binary search the target and find its next.
        //upper_bound
        if(!root) return 0;
        if(p->val<root->val) {
            TreeNode* left=inorderSuccessor(root->left,p);  
            return left==0?root:left;
        } 
        return inorderSuccessor(root->right,p);
    }
```	
->>

<<-230	Kth Smallest Element in a BST    		61.6%	Medium	
- inorder traversal O(N)
- binary search (count), O(nlogn)
- if the tree is modified often? add a counter to each node.
->>

<<-700	Search in a Binary Search Tree    		73.3%	Easy	->>

<<-270	Closest Binary Search Tree Value    		49.2%	Easy	
find the value in O(logn). target is float.
curr_diff=target-root->val, >0 we want to reduce the diff, goes to right.
else goes to left O(h).
->>

<<-272	Closest Binary Search Tree Value II    		51.3%	Hard	
find the k values closest to the target in BST.
O(N): the difference is ia V shape. using a deque to move the window. O(N)
if the tree is balanced: find the closest node and extend left and right (inorder and reverse inorder) 
->>

<<-333	Largest BST Subtree    		36.7%	Medium	
find the subtree which is a BST with largest number of nodes.
postorder: count the nodes and get the lmax and rmin to make sure it is a bst
O(N) complexity.
***
->>

<<-783	Minimum Distance Between BST Nodes    		53.4%	Easy	
absolute difference, just do inorder traversal.
->>

<<-255	Verify Preorder Sequence in Binary Search Tree    		45.9%	Medium	
root left right. 
binary search to find the upper bound and divide into left and right. and check all left < right. similar to divide and conquer.
->>

<<-1214	Two Sum BSTs    		67.8%	Medium	
two bsts, check if a from bst1 and b from bst2 and a+b=target.
hashmap + traverse
->>

<<-1373	Maximum Sum BST in Binary Tree    		38.1%	Hard	
postorder to find the left max, right min and sum and isBST.
->>

<<-1305	All Elements in Two Binary Search Trees    		77.7%	Medium	
approach 1: inorder for tree 1 and tree 2 separately and merge sort.
approach 2: same time inorder traversal and merge. has to use iterative approach
We are doing in-order traversal independently for two trees. When it's time to 'visit' a node on the top of the stack, we are visiting the smallest of two.
->>

<<-99	Recover Binary Search Tree    		41.7%	Hard	
two nodes of the BST was swapped. 
inorder traversal, similar to 1d array, it will create one pair or two pairs of disorder.
one pair: it is adjacent, otherwise two positions.
**
->>

### tree traversal
inorder, postorder, preorder, other order.
no change to tree structure.immutable.
- collect information while traversal to reduce O(N^2) to O(N).


### preorder, inorder, postorder.
<<-111	Minimum Depth of Binary Tree    		38.8%	Easy	->>
<<-129	Sum Root to Leaf Numbers    		50.1%	Medium	->>
<<-117	Populating Next Right Pointers in Each Node II    		40.3%	Medium	->>
<<-116	Populating Next Right Pointers in Each Node    		47.7%	Medium	->>
<<-110	Balanced Binary Tree    		44.0%	Easy	->>
<<-107	Binary Tree Level Order Traversal II    		54.4%	Easy	->>
<<-104	Maximum Depth of Binary Tree    		66.9%	Easy	->>
<<-103	Binary Tree Zigzag Level Order Traversal    		49.3%	Medium	->>
<<-102	Binary Tree Level Order Traversal    		55.7%	Medium	->>
<<-94	Binary Tree Inorder Traversal    		64.8%	Medium	->>
<<-145	Binary Tree Postorder Traversal    		56.4%	Medium	->>
<<-144	Binary Tree Preorder Traversal    		56.7%	Medium	->>
<<-199	Binary Tree Right Side View    		55.2%	Medium	->>
<<-222	Count Complete Tree Nodes    		48.1%	Medium	
1+2+4+...->2^(h+1)-1 check depth diff.
->>
<<-314	Binary Tree Vertical Order Traversal    		46.3%	Medium	
dfs and sort. (with same position, sorted order)
->>
<<-173	Binary Search Tree Iterator    		58.2%	Medium	
binary tree inorder traversal iterative approach using stack.
add all left into stack, pop and add all right into stack.
->>
<<-429	N-ary Tree Level Order Traversal    		65.9%	Medium	->>
<<-510	Inorder Successor in BST II    		59.6%	Medium	
you will have access to the node only, find the next node (with parent as a pointer)
if there is no right, go to parent
if there is right, go to the leftmost.
->>
<<-101	Symmetric Tree    		47.6%	Easy	->>
<<-100	Same Tree    		53.8%	Easy	->>
<<-572	Subtree of Another Tree    		44.3%	Easy	
two recursive problem: isSameTree and isSubtree.
->>
<<-965	Univalued Binary Tree    		67.6%	Easy	->>
<<-617	Merge Two Binary Trees    		74.8%	Easy	
overlapped add them together. do traverse at the same time.
->>
<<-655	Print Binary Tree    		55.4%	Medium	
row number = height, col number shall be odd. even the node does not exist you need reserve space for it.
->>
<<-987	Vertical Order Traversal of a Binary Tree    		37.2%	Medium	->>
<<-690	Employee Importance    		57.9%	Easy	
tree structure for postorder add.
->>

### collect information while traversing.
<<-865	Smallest Subtree with all the Deepest Nodes    		61.4%	Medium	
postorder to get the max depth and the first node with ldepth=rdepth.
similar to the LCA of all the deepest nodes.
->>

<<-894	All Possible Full Binary Trees    		76.5%	Medium	
each node has 0 or 2 children. n nodes.
N must be an odd number, give left/right (1,N-2),(3,N-5)... and do recursive
->>

<<-958	Check Completeness of a Binary Tree    		52.3%	Medium	
- bfs level order traversal. If we see a null and still have nodes following, then it is not a complete tree.
- dfs: find the depth. mark the end, h=mh-1. after end is found, if we see h=mh, then it is not a complete tree.
->>

<<-112	Path Sum    		41.8%	Easy	
check if there is a path sum=target. (from root to leaf)
->>

<<-113	Path Sum II    		48.0%	Medium	
return all root to leaf path with sum=target.
dfs or backtracking.
->>

<<-1022	Sum of Root To Leaf Binary Numbers    		71.2%	Easy	->>

<<-1161	Maximum Level Sum of a Binary Tree    		71.1%	Medium	->>

<<-515	Find Largest Value in Each Tree Row    		61.7%	Medium	
dfs or bfs. simple.
->>

<<-124	Binary Tree Maximum Path Sum    		35.0%	Hard	
a path may pass or not pass the root.
- get the subtree max path sum, left and right including root max path sum
- the including root left/right path sum is similar to 1d array max subarray sum. (connecting or discard)
- post order traversal + max subarray path sum.
****
->>

<<-366	Find Leaves of Binary Tree    		71.3%	Medium	
find leaves and then remove, until no nodes left.
postorder: leaf depth=0, and upper level is 1 and store same level nodes in array...
(leaf=level 0) postorder depth! (we can also start from root depth=0 using dfs)
*** 
->>

<<-513	Find Bottom Left Tree Value    		62.0%	Medium	
find the leftmost value on the bottom level.
dfs when h>maxh record the value.
***
->>

<<-1104	Path In Zigzag Labelled Binary Tree    		72.6%	Medium	
full tree, odd row are labelled from left to right, even rows are labelled from right to left.
just use full tree ordering and then convert.
->>

<<-652	Find Duplicate Subtrees    		51.3%	Medium	
postorder get the subtree's serialization and check hashmap.
for each root, get the subtree serialization.
****
->>

<<-988	Smallest String Starting From Leaf    		46.4%	Medium	
preorder: reach leaf get the string.
->>

<<-993	Cousins in Binary Tree    		52.1%	Easy	->>
<<-872	Leaf-Similar Trees    		64.5%	Easy	->>

<<-543	Diameter of Binary Tree    		48.9%	Easy	
longest path, it could be root to leaf or cross the root.
return longest root to leaf in subtree and cross root path.
***
->>

<<-1026	Maximum Difference Between Node and Ancestor    		68.8%	Medium	
dfs to get the previous min and max, and compare the difference.
****
->>

<<-1120	Maximum Average Subtree    		63.1%	Medium	
get the sum of values and number of nodes, postorder
***
->>

<<-236	Lowest Common Ancestor of a Binary Tree    		47.4%	Medium	->>
<<-235	Lowest Common Ancestor of a Binary Search Tree    		50.9%	Easy	
go to left or right O(log(n))
->>
<<-1123	Lowest Common Ancestor of Deepest Leaves    		67.2%	Medium	
postorder visit and get the maxDepth and then back to root to check if left and right==maxh.
O(N^2) is simple but O(N) is tricky. 
find depth and get the maxh at the same time, postorder from bottom to upper layers: 
```
    int h=0;
    TreeNode* lca=0;
    TreeNode* lcaDeepestLeaves(TreeNode* root) {
        depth(root,0);
        return lca;
    }
    int depth(TreeNode* root,int d){
        h=max(h,d);
        if(!root) return d;
        int left=depth(root->left,d+1);
        int right=depth(root->right,d+1);
        if(left==h && right==h)
            lca=root;
        return max(left,right);
    }
```
****	
->>

<<-111	Minimum Depth of Binary Tree    		38.8%	Easy	->>
<<-129	Sum Root to Leaf Numbers    		50.1%	Medium	->>
<<-1379	Find a Corresponding Node of a Binary Tree in a Clone of That Tree    		84.0%	Medium	
traverse at the same time in original and cloned tree.
->>
<<-1469	Find All The Lonely Nodes    		80.8%	Easy	
parent has only one child, the child is lonely node, dfs.
check its left and right child to store the child if it is the only child.
->>


<<-1506	Find Root of N-Ary Tree    		80.5%	Medium	
given a list nodes (Node*) find the root.
using xor: root only appears once, but all other nodes have parents
using dfs and xor.
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

<<-563	Binary Tree Tilt    		52.2%	Easy	
tilt=absolute difference between left sum and right sum
postorder to get tilt and sum
->>

### build tree
all similar to divide and conquer procedure.

<<-106	Construct Binary Tree from Inorder and Postorder Traversal    		48.5%	Medium	->>
<<-105	Construct Binary Tree from Preorder and Inorder Traversal    		50.5%	Medium	->>

<<-109	Convert Sorted List to Binary Search Tree    		49.2%	Medium	
similar to array find the mid and divide into two parts.
->>
<<-108	Convert Sorted Array to Binary Search Tree    		59.3%	Easy	->>

<<-431	Encode N-ary Tree to Binary Tree    		73.9%	Hard	
idea: the first children as the left, all the other children connected as the right.,
decode reverse the process.
->>
<<-606	Construct String from Binary Tree    		54.8%	Easy	
output tree as string in preorder, null node represented by (). root(left)(right)
->>

<<-889	Construct Binary Tree from Preorder and Postorder Traversal    		66.9%	Medium	->>
<<-1008	Construct Binary Search Tree from Preorder Traversal    		78.6%	Medium	->>

<<-297	Serialize and Deserialize Binary Tree    		48.8%	Hard	
Use # for null node. using stringstream to build the tree recursively.
->>
<<-428	Serialize and Deserialize N-ary Tree    		60.6%	Hard	
serialization needs include number of children for each node. serialization text string for transfer.
->>
<<-331	Verify Preorder Serialization of a Binary Tree    		40.7%	Medium	
serialization uses '#' for null node. 
each node has two children (counting '#')
- each node has one incoming, 2 outcoming.
- root has zero incoming
- '#' has no outcoming
if we count diff=out-in. the diff will always >=0 and will be 0 at the end. (just similar to valid parenthesis, actually you can use () to serialize the tree)
->>

<<-449	Serialize and Deserialize BST    		53.5%	Medium	
preorder: root left right .. and '# ' for null node.
deserialization: also do preorder.
same as serialization of binary tree.
->>

<<-654	Maximum Binary Tree    		80.6%	Medium	
root is the max. build the tree from array.
**
->>

<<-998	Maximum Binary Tree II    		63.6%	Medium	
max tree: node value is greater than all nodes in its subtree.
build from an array, if A[i] is the max, then A[i] will be the root, and left put in left subtree, right put in right subtree.
Now we already build the tree and we append a val to A, return the tree.
- if val>root, root will be the left.
- else add the val to the right tree.
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

<<-582	Kill Process    		62.0%	Medium	
given proces id and its parent id. kill a process will also kill its all ascendants.
build the tree relation (unordered_map ajacent matrix) and do dfs from the id.
**
->>

### apply algorithm in tree.

<<-653	Two Sum IV - Input is a BST    		55.9%	Easy	
tree traversal + hashtable
->>

<<-124	Binary Tree Maximum Path Sum    		35.0%	Hard	
a path may pass or not pass the root.
- get the subtree max path sum, left and right including root max path sum
- the including root left/right path sum is similar to 1d array max subarray sum. (connecting or discard)
- post order. get lmax and rmax including the root, and the connect left and right with root, get the global max.
***
->>

<<-96	Unique Binary Search Trees    		53.8%	Medium	
return number of unique BST using nodes 1 to n.
dp: dp[i] represent the number of unique BST for i nodes. dp[i]+=dp[j-1]*dp[i-j]
->>

<<-250	Count Univalue Subtrees    		52.7%	Medium	
post order traversal, if current subtree is univalue add 1.
->>

<<-298	Binary Tree Longest Consecutive Sequence    		47.6%	Medium	
path shall follow parent child, ascending order.
dfs: if root!=parent+1, then reset the length otherwise =parent.length+1
- preorder: need parent information.
***
->>

<<-549	Binary Tree Longest Consecutive Sequence II    		47.1%	Medium	
longest path with consecutive sequence (either increasing or decreasing). The path can cross the root.
- get left and right longest path, longest sequence ending with root.
- postorder to get the two results: longest increasing, longest decreasing sequence
{inc,dec}, longest=max(longest,inc+dec-1);
****
->>

<<-437	Path Sum III    		47.7%	Medium	
find number of path with sum=target, the path does not need from root to leaf, but any downpath is good.
prefix sum with hashmap while dfs.
similar to subarray sum = target. (1d array range sum problem)
***
->>

<<-508	Most Frequent Subtree Sum    		58.6%	Medium	
postorder sum + hashmap
->>

<<-545	Boundary of Binary Tree    		39.3%	Medium	
this one is hard. left boundary, leaf, right bounday in counter-clockwise.
preorder for the left and leaf 
postorder for the right boundary.
also need judge if the node is left or right boundary
left boundary: lb && !root->left
right boundary: rb && !root->right
****
->>

<<-662	Maximum Width of Binary Tree    		40.2%	Medium	
width is the level width. (according to full tree definition)
dfs with full tree index. 2*i and 2*i+1 for left and right children.
attention: overflow problem.
->>

<<-663	Equal Tree Partition    		39.6%	Medium	
divide into two subtree with same sum by removing one edge.
record each subtree's sum in hashmap. one tree must be a subtree, so we just find sum/2.
- first get total sum, then do postorder get subtree sum.
- or just one pass post order and store subtree sum in hashset.
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

<<-687	Longest Univalue Path    		36.8%	Medium	
postorder: get the left and right max depth with value=root->val, and connect via the root. (left+right)
->>

<<-779	K-th Symbol in Grammar    		38.3%	Medium	
first row 0. The second row will change each 0 to 01 and 1 to 10.
find row N and kth digit.
it forms a complete binary tree. if previous node is 0, then left is 0 and right is 1. if previous node is 1, then left is 1 right is 0. do it recursively.
->>

<<-834	Sum of Distances in Tree    		44.9%	Hard	
given a tree representing as a list of edges. return the sum of distance from i to all other nodes.
this is mostly about optimization since a lot of paths are revisited.
- brutal force starting from each node and do dfs and get the answer. O(N^2)
- build the adjacency matrix.
- start from root 0, do a postorder traversal to get number of nodes in subtree and sum of distance in substree. count[root]=sum(count[child])+1, dist[root]=sum(dist[child])+sum(count[child])
- when we move root from parent to its child i, res[i]=res[root]-count[i]+N-count[i], do a preorder dfs to update.
tree+dp, hard!!
*****
->>

<<-968	Binary Tree Cameras    		38.1%	Hard	
a camera can monitor its adjacent nodes and itself.
return the min number of cameras needed to monitor every node.
idea: greedy approach, we need do postorder traversal and put the camera on the parent node, since it will cover the child and parent and itself
define 3 state： 0： not covered, 1, place a camera, 2: covered.
child 0x or x0, we need add one at parent, 22, we do not place camera but not monitored.
12 or 21->monitored.
greedy。
*****
->>

<<-979	Distribute Coins in Binary Tree    		69.2%	Medium	
given a binary tree with n nodes and n coins. In each move, you can choose two adjacent nodes and move one coin from one to the other. The coin initial distribution is given as tree node value.
return the min number of operations to make each node one coin.
postorder to get the sum of node value -1 (since we need 1): root-1+l+r
the steps from/to root is abs(sum). ans+=abs(sum).
*****
->>

<<-1145	Binary Tree Coloring Game    		51.4%	Medium	
first player choose one node, each player can only choose his neighnors.
second player can have 3 choices: left child, parent,right child. Since the choice will bprevent the first player to choose that branch. So it turns out the count the nodes in all three branches. You can always choose two branches.
->>

<<-1245	Tree Diameter    		61.1%	Medium	
n-ary tree. given a list of edges. 
dfs: get the depth and find the max and 2nd max depth for node. 
diameter is the max depth + 2nd max depth via some node. (similar to find max and 2nd max in array in O(N)
->>

<<-1302	Deepest Leaves Sum    		83.9%	Medium	
two pass is trivial one pass to get the max depth, one pass to get the sum.
one pass: bfs to get the previous row's sum.
one pass: dfs, get the depth and max depth, when d>md, reset the sum.
similar to LCA deepest nodes., subtree of deepest nodes.
->>

<<-1315	Sum of Nodes with Even-Valued Grandparent    		83.8%	Medium	
dfs with parent info.
->>

<<-1339	Maximum Product of Splitted Binary Tree    		37.4%	Medium	
split the tree by removing one edge, return the max product of the two tree's sum.
a+b=tsum, max(ab) it is same to min(|a-b|)
two pass visit. first any order to get the sum, second postorder to get mindiff.
->>

<<-1361	Validate Binary Tree Nodes    		44.9%	Medium	
given a tree with a list of nodes and its left child and right child (-1 means no child)
a tree shall have n nodes, n-1 edges and each node except the root has one pararent, no cycle.
tree property.
->>

<<-1367	Linked List in Binary Tree    		41.1%	Medium	
check if the linked list exist in the tree as a down path.
- find all nodes as starting and do dfs and traverse on linked-list. or
- it can start as root, left or right, so check 3 problem.
***
->>

<<-1372	Longest ZigZag Path in a Binary Tree    		54.4%	Medium	
choose a node and then choose a direction and then switch direction. Get the longest path.
postorder traversal and return the subtree max path, using left branch or right branch,
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

### change the tree: insert, delete, rotate, et al.

<<-226	Invert Binary Tree    		66.1%	Easy	
swap left right.
*
->>

<<-814	Binary Tree Pruning    		73.3%	Medium	
prune all subtree with all 0.
postorder traversal: remove leaf node=0.
->>

<<-669	Trim a Binary Search Tree    		63.2%	Easy	
trim so that all value in range [L,R]
postorder.
**
->>

<<-897	Increasing Order Search Tree    		72.5%	Easy	
change tree to a in-order order tree.
using reverse inorder traversal.
root->right=process(root->right,prev)
...
return process(root->left,prev); prev shall be reference.
level: 3
->>

<<-1325	Delete Leaves With a Given Value    		73.4%	Medium	
post order to remove a leaf.
->>

<<-1038	Binary Search Tree to Greater Sum Tree    		81.5%	Medium	
change the node's value to value+sum of all keys > val.
right, root, left traversal. prefix sum.
->>

<<-1261	Find Elements in a Contaminated Binary Tree    		74.3%	Medium	
recover and find.
root is x then left=2x+1, and right=2x+2. 
dfs to restore.
hashset to find.
->>

<<-1110	Delete Nodes And Return Forest    		67.4%	Medium	
postorder traversal.
->>

<<-951	Flip Equivalent Binary Trees    		65.5%	Medium	
check if you flip left and right and get the other tree.
left vs left, right vs right
left vs right, right vs left.
***
->>

<<-701	Insert into a Binary Search Tree    		76.0%	Medium	
just insert in left or right. (use it as root is much complicated)
there are multiple ways, we can insert it as a leaf node.
->>

<<-450	Delete Node in a BST    		44.9%	Medium	
if key<root, goes left, else goes right
if key==root, 
- if no right, promote the left: root=root->left
- if there is right branch, we first sink the root to its right leftmost node, swap it, and delete the leftmost node.
***
->>

<<-623	Add One Row to Tree    		50.0%	Medium	
add a row of nodes to depth d. original left subtree is still left, original right is still right.
- bfs until d-1 layers (parent nodes) and then do the swaps.
- dfs: add 1 layer, so that cd==d, then do the swap and return current search.
->>

<<-538	Convert BST to Greater Tree    		56.1%	Medium	
node value is changed to the value + sum of all keys greater than it.
right, root, left order.
suffix sum: passing the sum as reference is must.
***
->>

<<-1080	Insufficient Nodes in Root to Leaf Paths    		49.7%	Medium	
a node is insufficient if all path passing this node has a sum < limit.
remove all the insufficient nodes.
dfs and subtract the node from limit. Note if the left and right is removed, the root shall also be removed.
actually this is a first dfs and then postorder traversal.
if the leaf < limit, we return null.
and then postorder the left and right and update the root.
```
    TreeNode* sufficientSubset(TreeNode* root, int limit) {
        if(!root) return 0;
		if(root->left==root->right) 
            return root->val<limit?0:root;
		root->left=sufficientSubset(root->left,limit-root->val);
		root->right=sufficientSubset(root->right,limit-root->val);
		return root->left==root->right?0:root;
    }
```
****	
->>

<<-114	Flatten Binary Tree to Linked List    		50.8%	Medium	
traverse right left root order using a prev node. passing prev using reference.
```
    void flatten(TreeNode* root) {
        TreeNode* prev=0;
        helper(root,prev);
    }
    void helper(TreeNode* root, TreeNode*& prev)
    {
        if(!root) return;
        helper(root->right,prev);
        helper(root->left,prev);
        root->right=prev;
        root->left=0;
        prev=root;
    }
```
using value passing will be incorrect, why? we need pass changes from right to left. if value pass, the prev for the right and left is the same.	
****
->>

<<-156	Binary Tree Upside Down    		55.6%	Medium	
- left child becomes the new root
- root becomes the new right child
- right becomes the new left child
goes left and get the leftmost leaf node, and get the new root.
this is similar to reverse linked list. not very intuitive.
root->left becomes the new root.
root itself becomes the right child root->left->right=root
original right becomes the left child root->left->left=root->right.

follow the recursive and simplest tree operation：
- base case: null tree and leaf only
- left->left and left->right
- root becomes leaf.
***
->>

<<-776	Split BST    		56.3%	Medium	
given a bst and a target value, split into two subtree so that all nodes in the subtree <= target and the other subtree > target.
most of the structure shall be kept.
- if root<=v, then split right, else split left.
- split the tree get smaller and bigger subtree
- if root>target, we go left and split into smaller and larger. root->left=larger.
- if root<=target, we go right and split into smaller and larger, root->right=smaller 
tricky.
*****
->>

<<-919	Complete Binary Tree Inserter    		58.1%	Medium	
using array to represent a complete binary tree.
root child index relation i,2i,2i+1.
- do it like bfs one layer by layer and push_back to the array (naturally the proper position)
- add a node at the end of the last level.
be careful of the index: n is odd, we are adding left child, (0 index).
***
->>

<<-971	Flip Binary Tree To Match Preorder Traversal    		46.1%	Medium	
binary tree with n nodes from 1 to n. you can flip a node's left and right subtree to match the preorder traversal.
our goal is to flip the least number of nodes to match them.
if we can do it, return the list of nodes flipped. otherwise return [-1]
preorder: root, left, right.
first compare root, with A[ind] start from 0, fail then directly return.
check left (preorder traversal) if fail, we first check right and then left.
check right.
two problem: match or not, flipped nodes.
note we need advance start or rewind to original, this is convenient using value passing. then we return the start value so far to reach reference passing. (reference passing: for null node the pointer is advanced but need to get back).
be careful the returned pointer is already advance 1. (so do not add 1 again)
very good tree problem.
*****
->>

<<-1273	Delete Tree Nodes    		63.3%	Medium
the tree is given in the format of： node-parent array.
build the tree and do postorder traversal to get num_nodea and sum. Delete the subtree=reset the sum and num_nodes.
-build the tree using adjacency matrix
- post order to get num node and sum. if sum==0 elmininate the subtree.
***
->>


<<-1516	Move Sub-Tree of N-Ary Tree    		62.1%	Hard	
all nodes have unique values, given two nodes p and q. 
move p subtree to become a direct child and the last child of node q.
???
->>

<<-1660	Correct a Binary Tree    83.2%	Medium	
a node's right point to a node in its right on the same level
approach: dfs with hashmap.
level: 3
->>

<<-1666 Change the root of a binary tree
problem: node has left,right,parent. go up from leaf, if cur has a left, it becomes right child. Its parent becomes left child.
subject: binary tree, very confusing.
level: 4
->>

## binary index tree
see the appendix for binary index tree.
fenwick tree or binary index tree O(nlogn) to update and query range sum in [l,r].

including sum, xor the function shall be reversible. BIT is used to calculate/update the function in O(logn).
A BIT is an array where each element T[i]=sum of elements in original array A in range [g(i),i] with 0<=g[i]<=i.

g(i): replace all i's binary trailing 1 to 0. (so g(i)<=i)
i.e. g(i)=i&(i+1) (eliminate all trailing zeros)

update: for all j with g(j)<=i<=j h(j)=j|(j+1)

- 1d array 
- 2d array
- build the BIT array
- add range sum 
- add update function

both can be used for dynamic cases (query and updates). BIT actually does not have a tree structure so cannot use tree traversal, but its navigation is determined by the simple relation. segment tree although uses the array but its inside logic is binary tree structure, so we can use those tree traversal and is more understandable (can use recursive).
in a lot of cases, they are exchangeable. segment tree is more versatile.

<<-307	Range Sum Query - Mutable    		36.0%	Medium	
array.
binary index tree for 1d array. need convert update to add/increase function. (need keep original array and BIT).
***
->>
<<-303	Range Sum Query - Immutable    		46.3%	Easy	
prefix sum. 1d.
->>

<<-308	Range Sum Query 2D - Mutable    		36.7%	Hard	
update an element.
sum region
prefix sum using similar dp.
binary index tree for 2d array. using 1-index equation
do not know why using the loop variable as the same as input will get incorrect results.
***
->>
<<-304	Range Sum Query 2D - Immutable    		39.7%	Medium	
prefix sum. 2d.
->>

<<-327	Count of Range Sum    		35.7%	Hard	
return the number of range sums in [L,R].
prefix sum, and save in map: presum vs list of index.
then use binary search to get the lowerbound and upperbound.
other approach:
- merge sort (divide and conquer): prefix sum and divide into two parts, count the right part - left part in the range.
- binary index tree: hard to understand why BIT can be used
->>

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

<<-1649	Create Sorted Array through Instructions    		42.0%	Hard	
segment tree 
->>

## segment tree

segment tree is a full binary tree with the root for the whole tree, left is the left array and right is the right array recursively. Using array to store the tree and the index of the child and parent relation is fixed. so needs extra storage.

- compute range sum
- compute range max/min
- compute range gcd/lcm 

we can use a full binary tree (in array) to store the segment tree.
- apply tree's traversal, preorder, postorder, inorder...
- segment tree can solve similar BIT problem.

- the segment tree is stored as a complete tree in array. parent index v, left child index 2*v, and right child index 2v+1.
- the leaf nodes are the single element in original array.
- the tree array need store all parent nodes, max size 4*N.

build the segment tree:
- postorder build: each node contain the segment sum.
- or you can actually build a tree, which we can use tree traversal.

query the sum in A[l,r]:
- go to left or right, or break into left and right two intervals
- recursively get the range sum. (preorder to get the interval sum)

update the elements:
- similar to build, update the leaf and propage to all parents nodes.

segment tree in array:
tree[1] is the root node. so we cn use 2v and 2v+1 as left and right.

<<-308	Range Sum Query 2D - Mutable

- tree: root index=1, range 0 to n-1.
- goes left and right or break into left and right.

```
    vector<int> tree;
    int n;
    NumArray(vector<int>& nums) {
        n=nums.size();
        tree.resize(4*n);
        build_tree(nums,1,0,n-1);
    }
    
    void update(int index, int val) {
        update(1,0,n-1,index,val);//start from root, postorder
    }
    void update(int v,int tl,int tr,int ind,int val){
        if(tl==tr) {tree[v]=val;return;}
        int tm=(tl+tr)/2;
        if(ind<=tm) 
            update(2*v,tl,tm,ind,val); //go to left
        else 
            update(2*v+1,tm+1,tr,ind,val); //go to right
        tree[v]=tree[2*v]+tree[2*v+1]; //update parent
    }
    
    int sumRange(int left, int right) {
        return sum(1,0,n-1,left,right); 
    }
    int sum(int v,int tl,int tr,int l,int r){
        if(l>r) return 0;
        if(l==tl && r==tr) return tree[v]; //found a matching interval sum
        int tm=(tl+tr)/2;
        return sum(2*v,tl,tm,l,min(r,tm))+sum(2*v+1,tm+1,tr,max(l,tm+1),r);
    }
    void build_tree(vector<int>& nums,int v,int tl,int tr){
        if(tl==tr) {tree[v]=nums[tl];return;}
        int tm=(tl+tr)/2;
        build_tree(nums,2*v,tl,tm);
        build_tree(nums,2*v+1,tm+1,tr);
        tree[v]=tree[2*v]+tree[2*v+1];
    }
```
->>

if there are no update, try use merge sort, divide and conquer method.

- build the segment tree, postorder (the parent is the sum/max/min... of the children)
- the tree is stored in array (completed tree storage)
- the leaf node is a single element.
- query [l,r]: do tree traversal
- update one element, update the whole path.

### linked list
- single linked list
- library list
- doubly linked list
- list is the base for hash table.
- cycle detection
- traversal

- linked list is very mistake prone since it easily will produce infinite loop.
- often resursive approach can be applied to most linked-list problem
- linked list is inside data structure for hashmap et al
- list iterator with hashamp often can be used to reduce complexity.

<<-19	Remove Nth Node From End of List    		35.4%	Medium	
two pointer to get nth node.
->>
<<-1474	Delete N Nodes After M Nodes of a Linked List    		74.4%	Easy	->>

<<-24	Swap Nodes in Pairs    		51.6%	Medium	->>

<<-23	Merge k Sorted Lists    		41.4%	Hard	->>

<<-61	Rotate List    		31.3%	Medium	
rotate by k places. find the length and k%=n, and then do the rotation.
->>

<<-82	Remove Duplicates from Sorted List III    		37.6%	Medium	
delete one nodes that have duplicate numbers, leaving only the distinct ones.
similar to a stack. but need keep prev.
->>

<<-80	Remove Duplicates from Sorted Array II    		44.7%	Medium	
duplicates only allows 2 times. two pointer, check nums[i] with nums[i-2]
->>

<<-83	Remove Duplicates from Sorted List    		46.0%	Easy	
easy but tricky, for each node we need iterate until we find no more duplicates.
->>

<<-86	Partition List    		42.5%	Medium	
give a value x, parition so that all nodes less than x comes before node >=x.
two list and merge.
->>

<<-25	Reverse Nodes in k-Group    		43.5%	Hard	
->>

<<-92	Reverse Linked List II    		39.8%	Medium	
reverse from position m to n. do it in place.
->>

<<-143	Reorder List    		39.7%	Medium	
l0->ln->l1->Ln-1--
break into two, reverse one and merge.
->>

<<-142	Linked List Cycle II    		38.9%	Medium	
two pointer., find the cycle entrance.
->>

<<-141	Linked List Cycle    		41.9%	Easy	
two pointer. detect if there is a cycle.
->>

<<-328	Odd Even Linked List    		56.5%	Medium	
group odd value nodes followed by the even list.
create two lists and then connect them.
many edge cases.
use odd, even, (odd->next=even->next,even->next=odd->next, then connect) avoid edge cases.
***
->>

<<-426	Convert Binary Search Tree to Sorted Doubly Linked List    		60.2%	Medium	
inorder traversal do the transformation in place.
- create a dummy node and prev node.
****
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
strings are often mixed with array, equivalent to array of chars.
most important of string is for word processing. The search, replace, regex...

some special algorithm related with string:
rolling hash
KMP.
regex.

KMP uses subpattern in pattern so that we can skip some mismatch.
- it connects pattern + $ + text.
- create a prefix function
- then check s with text.
O(N) 
```
vector<int> computePrefixFunction(const string& p)
{
	int len=p.size();
	int border=0;
	vector<int> s(len);
	for(int i=1;i<len;i++)
	{
		while(border>0 && p[i]!=p[border])
			border=s[border-1];
		if(p[i]==p[border]) border++;
		else border=0;
		s[i]=border;
	}
	return s;
}


vector<int> find_pattern(const string& pattern, const string& text)
{
    vector<int> result;
	int lenp=pattern.size();
	int lens=text.size();
	if(lenp>lens) return result;
    // Implement this function yourself
	vector<int> s=computePrefixFunction(pattern+"$"+text);
	for(int i=lenp+1;i<lens+lenp+1;i++)
		if(s[i]==lenp) result.push_back(i-2*lenp);
    return result;
}
```

### simple
### general string operations
<<-93	Restore IP Addresses    		36.7%	Medium	
ip address missing all dots. stoi.
just try all possibles !!
->>

<<-468	Validate IP Address    		24.6%	Medium	
ipv4 vs ipv6. be careful of all the restrictions.
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

<<-214	Shortest Palindrome    		30.3%	Hard	
given a string, you can add chars in front of it to make it palindrome. return the shortest palindrome.
- equiv find the longest palindrome starting with the first char. try longest first. (TLE)
- KMP to speed it.
****
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
regex or stack.
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

<<-831	Masking Personal Information    		44.6%	Medium	->>
<<-833	Find And Replace in String    		51.0%	Medium	
find and replace shall be done separately.
->>

<<-859	Buddy Strings    		29.9%	Easy	
by swapping a pair in A exactly once, can you get B?
compare one by one: 0 mismatch or 2 mismatch, for 2 mismatch must be able to swap. for 0 mismatch we need to find two same chars in A.
another approach using hashmap to compare
greedy.
->>

<<-890	Find and Replace Pattern    		74.0%	Medium	->>

<<-893	Groups of Special-Equivalent Strings    		67.9%	Easy	
you can swap odd index vs odd index, even index vs even index.
sort by odd or even.
->>

<<-1016	Binary String With Substrings Representing 1 To N    		59.4%	Medium	
just find if all the n string appears in it.
->>

<<-1071	Greatest Common Divisor of Strings    		51.7%	Easy	->>
<<-1323	Maximum 69 Number    		77.9%	Easy	
number consists of 6 and 9 only. You can change at most one digit from 6 to 9 or 9 to 6 to get the max.
find the first 6 and replace it with 9.
->>

<<-1392	Longest Happy Prefix    		41.1%	Hard	
find the longest prefix which is also a suffix (not including itself).
to get O(N) using rolling hash.
->>

<<-1410	HTML Entity Parser    		54.3%	Medium	
find given list of special strings and replace with given string
typical string: find all positions and replace from right.
->>

<<-1065	Index Pairs of a String    		61.1%	Easy	
given a list of dictionary words, find the index.
- brutal force.
- trie.
->>

<<-1078	Occurrences After Bigram    		64.7%	Easy	
first second and store the followed words.
stringstream just keep 2 in the storage, keep updating.
->>

### string more tricky.
<<-214	Shortest Palindrome    		30.3%	Hard	
given a string, you can add chars in front of it to make it palindrome. return the shortest palindrome.
- equiv find the longest palindrome starting with the first char. try longest first. (TLE)
- KMP to speed it. S+"$"+reverse(s) and calculate the prefix function.
->>

<<-524	Longest Word in Dictionary through Deleting    		48.7%	Medium	
given a list of dictionary word, and a string. we can delete some chars to get the dictionary word.
return the longest word with the smallest lexi order.
brutal force: sort the dict in lexi order, check each word in dict and see if it is a subsequence of s. (check subsequence using two pointer)
->>

<<-544	Output Contest Matches    		75.5%	Medium	
team the strongest with the weakest. the initial ranking is 1 to n.
you need always keep strong vs weaker order
for example 4 teams, ((1,4),(2,3)), n=2^k.
a bit tricky:
- store array of strings.
- each round using two pointer match from both ends.
- each round reduces by half.
- until reduce to 1 group.
->>

<<-647	Palindromic Substrings    		61.4%	Medium	
return number of pal substrings (with different start end index)
- expand at all possible position 2n-1 positions
- dp: dp[i,j]=ispal(s,i,j)+dp[i+1,j]+dp[i,j-1]-dp[i+1,j-1], can also be used for subsequence.
- manacher most fast.
***
->>

<<-820	Short Encoding of Words    		51.2%	Medium	
return the shortest encoded string, encoding format:
word#word#...each word in the list can be found in the encoded string.
- we shall embed some common subarray into one word. each word is a suffix of a substring
- using Trie
- or sort by length, short word can be suffix of longer.(build a suffix array hashmap)
->>

<<-1062	Longest Repeating Substring    		57.7%	Medium	
- binary search
- dp solution, id(s[i]==s[j] dp[i,j]=dp[i-1,j-1]
- trie: insert all suffix into trie and update the repeating substr length.
- KMP: for every suffix of s, find the prefix. get the longest
****
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
using hashset to achieve distinct. (for each start index, with different length) -->O(N^3)
- rolling hash to reduce to O(N^2) using fixed size sliding window.
->>

<<-1400	Construct K Palindrome Strings    		62.5%	Medium	
check if string s can be decomposed to k palindrome strings. (use all characters)
count each characters. 
- string size <k, not OK
- odd chars >k, not OK
- other cases are OK.
->>

<<-1404	Number of Steps to Reduce a Number in Binary Representation to One    		50.5%	Medium	
reduce to 1: if it is even, divide by 2, if it is odd, add 1. (remove the LSB using 1 or two steps)
approach 1: simulate the process with string add. O(N^2)
approach 2: optimize with a cf. rememble the bit operation using & and >>.
->>

<<-1408	String Matching in an Array    		62.6%	Easy	
given a list of words, find words which is a substring of another word.
sort by length: shorter one can be longer one's substring
->>


## array

array could be very easy or very hard. It involves a lot of algorithms and data structures.
1d array, 2d array, multi-dimensional

### trivials

### rotate, reverse, sort, traverse...

<<-48	Rotate Image    		58.5%	Medium	->>

<<-189	Rotate Array    		36.1%	Medium	
- using reverse
- using stl rotate.
->>

<<-360	Sort Transformed Array    		49.3%	Medium	
given array and apply ax%2+bx+c to it. return the array in sorted order.
depending on a, it could be a parabolic curve upside or downside.
a==0, check b.
a!=0, using two pointer.
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

<<-498	Diagonal Traverse    		48.9%	Medium	
given a matrix, return the diagonal traverse, up and down...
- can use hashmap and then reverse 
- can use two pointer. rebounce back when hit the 4 walls.
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


### get information from array: search.
search to get desired information: find target, target sum, min/max, shortest longest subarray, subsequence.....
so many patterns to search. 
Generally you need use some other techniques or data structures.

- get min max, median, majority, mode...

<<-169	Majority Element    		59.5%	Easy	
find elements appear more than n/2.
voting algorithm: cancel different votes and accumulate.
->>

<<-229	Majority Element II    		38.1%	Medium	
find elements appear more then n/3 times.
one or two candidate O(N), voting algorithm. need check if they are over n/3.
->>

<<-361	Bomb Enemy    		46.4%	Medium	
given a 2d grid, 'W' wall, 'E' enemy, '0' empty.
a bomb will kill enemies on the row and column until blocked by wall.
return max enemies killed by one bomb.
try each empty cell and count number of enemies killed.
->>

<<-363	Max Sum of Rectangle No Larger Than K    		38.2%	Hard	
idea: get all possible submatrices and reduce to 1d array, and then find max subarray sum <=k.
****
->>

<<-419	Battleships in a Board    		70.6%	Medium	
battle ship are horizontal or vertical, battleship are separated at least one empty cell.
if we see empty + x horizontal or vertical we add one ship. 
->>

<<-442	Find All Duplicates in an Array    		68.4%	Medium	
elements are from 1 to n, some appear twice, other once, find all the elements appear twice.
mark negative using as index. Same as 448.
using value as index.
->>

<<-448	Find All Numbers Disappeared in an Array    		56.0%	Easy	
given 1 to n, some appear twice others appear once. find all disappeared numbers.
- sort 
- use the number-1 as the index and mark seen as negative. those positives are seen twice.
->>

- prefix sum, product

<<-1352	Product of the Last K Numbers    		42.6%	Medium	
sliding window product using prefix product. Actually not related to window. similar to prefix sum.
->>

<<-238	Product of Array Except Self    		60.9%	Medium	
no division allowed.
left and right two direction (prefix suffix product)
->>

<<-525	Contiguous Array    		43.2%	Medium	
binary array find the longest subarray with equal number of 0 and 1.
change 0 to -1, and then use prefix sum to find longest subarray with sum=0;
hashmap
->>

<<-900	RLE Iterator    		54.8%	Medium	
run length encoding iterator.
prefix sum on the element count and do binary search.
->>

- left right two directions, intervals, subarrays...

<<-689	Maximum Sum of 3 Non-Overlapping Subarrays    		46.9%	Hard	
fix left and right and find range for bottom
->>

<<-821	Shortest Distance to a Character    		67.5%	Easy	
in a string find each char to a given char's shortest distance.
two pass to update the distance to left and right.
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

<<-1014	Best Sightseeing Pair    		52.7%	Medium	
given array A[i]=value of ith spot.distance of i and j= |i-j|.
max the function A[i]+A[j]+i-j, i<j.
separate A[i]+i and A[j]-j for left and right side.
using lmax and rmax approach.
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

<<-548	Split Array with Equal Sum    		47.2%	Medium	
given an array, three index i,j,k will split the array into 4 subset (excluding the selected index elements at i,j,k). The 4 subsets shall have the same subarray sum. Check if it is possible.
- prefix sum.
- fix the mid pointer by looping all possible positions
- try all possible left and right positions save all possible valid cut into hashmap.
->>

<<-624	Maximum Distance in Arrays    		39.2%	Medium	
given m sorted array. pick two elements from two different array and get the distance=|a-b|. return the max distance.
note: it is not a matrix. 
get the min of the first col and max from the back of each row.
->>

<<-678 valid parenthesis string
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

<<-680	Valid Palindrome II    		31.4%	Medium	
you can delete at most one char. compare string with reversed string. at least two mismatch. remove the first or the second.
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
<<-29	Divide Two Integers    		16.5%	Medium	
math: using divisor double until <=divid and then use binary coefficient to get all binary coeff..
->>
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
n+1 elements with 1-n, one appear twice. xor to eliminate even occurences.
->>

<<-296	Best Meeting Point    		57.9%	Hard	
given a 2d grid with some people on grid, find the meeting point to minimize the total distance.
all x's median and all y's median.
->>

<<-311	Sparse Matrix Multiplication    		63.1%	Medium	
store value vs i,j then add if i,k vs k,j.
->>

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
c++ function struct tm and mktime. then get the number of days between two dates.
mktime to convert to seconds, tm use referecen 1900/1/1.
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
graph representation
graph traversal, dfs or bfs
connection.

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
dfs coloring:
first judge if colored, if yes check color the same
then color the node and then check its connected neighbors.
***
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
using map to record start and exit then prefix sum to get prefix=0 interval
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
trie is essentially the n-ary tree. generally used for prefix or suffix string. This topic is not hard and can be used similarly as dfs/bfs...
bits are often using trie for xor, and...
trie is important since it is the base data structure for recommendation system such as google search, amazon recommendation et al.

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
- build a trie and search start with. (sort reversely using length)
- sort alphabetically and use dp similar approach.
***
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
quad tree is widely used in geoinfo or map applications. used in google maps, uber et al.

<<-427	Construct Quad Tree    		62.0%	Medium	
topleft,topright,bottLeft,bottRight
if the cell has all the same value, then it is a leaf with value
else we need to divide into 4 cells and recursively build them.
->>

<<-558	Logical OR of Two Binary Grids Represented as Quad-Trees    		45.1%	Medium	
quad tree with 4 children: topleft,topright,bottLeft,bottRight, val,isLeaf.
given two quad trees representing two binary matrices, return a quad tree representing the bit OR of the two quad trees.
quad tree is often used for geoinfo system. It is used to get adaptive resolution for a grid cell.
if cell is leaf node, then subcell will all be 1 (if we divide into smaller cells)
- case 1: q1 or q2 is leaf: with all 1, then the q1 or q2 will get a all 1 cell (subcell is merged), with all 0, then use the other node.
- case 2: q1 and q2 are both non-leaf nodes: recursive do it. return a leaf node or non-leaf node.
->>

### how to optimize the algorithm
optimization is critical especially in competing programming. Also, using complexity analysis to judge how to optimize the algorithms.

- get familiar with data structure complexity
- actual performance of data structure in real using the language. for example vector is for flexible length, array is for fixed length and is having better performance.
- avoid recomputing: visit tree node again and again, visit array elements, the repetitive work is generally reduced by using memoization, collecting information needed while visit once.
- passing value or reference
- prune while searching.

