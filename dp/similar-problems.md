dp
### general problems
finding the recurrence relation.
it uses what the dp is used for. We need to see if bigger problem can be solved using smaller solutions

338. counting bits<br/>
dp[i]=dp[i/2]+i&1

937. integer break<br/>
break i into j and i-j and i-j could be an integer break again

941. 2 keys keyboard<br/>
dp[i]=dp[j]+i/j if i%j==0

65. unique binary search trees<br/>
for subproblem with i nodes, then dp[i]=sum(dp[j-1]*dp[i-j])

95. unique bst II<br/>
with backtrace

718 max length of common subarray<br/>
dp[i,j]=dp[i-1][j-1]+A[i-1]==B[i-1]

808. soup serving<br/>

935. knight dialer<br/>
next step depends on previous step

815. knight probability in chessboard<br/>

790. Domino and tromino tiling<br/>

801. min swaps to make sequence increasing<br/>

152. max product subarray<br/>
keep min and max

### 2d matrix path problem
2d matrix generally uses 4 or 6 or 8 directions to mark next steps

931. min falling path sum<br/>

64. min path sum<br/>
from top left to bottom right

120 triangle<br/>

63. unique paths<br/>

576. out of boundary paths<br/>

### strategy game
two players play optimally.

338. stone games<br/>
940. predict the winner<br/>
464. can i winner<br/>
if the other player win, then I lose. same subproblem

### palindrome
generally needs two pointers or dp[i,j] to indicate the start and end

932 palindromic substrings<br/>
dp[i,j]=self+dp[i,j-1]+dp[i+1,j]-dp[i+1,j-1]

### ending with a[i]
some problems generally defines dp[i] as the answer ending at ith element, for example subsequences. This is often the most widely used techinque to approach dp problems.

933. arithmetic slices<br/>

300. longest increasing subsequence<br/>
dp[i] is the max length subsequence ending at i.
then we can add it to previous subsequence and get the max length

376. wiggle subsequence<br/>
up[i]: 
dn[i]:

139. word break<br/>
ending at i and see if i can be a cut position

467. unique substrings in wraparound string<br/>
ending with a different char, not a position

64. number of longest increasing subsequence<br/>
dp[i] is the length of subsequence ending at i

### edit distance
edit distance generally works for two strings or one string which can be converted to two string problem.

934. min ascii delete sum for two strings<br/>

942. is subsequences<br/>
to see if one string is a subsequence of another string
only allowed deletion from one string

741. longest palindrome subsequence<br/>
reverse and it is a edit distance problem with both deletion allowed.

### at most k times
the basic idea is to add one more operation based on k-1 results to relax the results.

935. best time to buy and sell stocks with transaction fee<br/>
dp[i]=max(dp[i-1],dp[j]+price[i]-price[j])

309. best time to buy and sell stocks with cooldown<br/>
sell or not sell on the ith day
not sell profit is dp[i-1]
sell profit is max(dp[j-2]+price[i-2]-price[j-2])

813. largest sum of averages<br/>
need divide into k-parts to max the average sum
add one more cut j and the last j to i is a new parts
this is not at most k, but we can use similar ideas

787. cheapest flights within k stops<br/>
add one stop to see if we could reduce prices

### need some preprocessing
some problems could be much easier if we preprocess it first such as sorting, convert to other data structure

936 max length of pair chain<br/>
sort first according to start

740. delete and earn<br/>
after bucket sort into all buckets with duplicated numbers, this becomes a house robber problem.

368. largest divisible subset<br/>
sort it first so that the front number is always smaller

898. bitwise or of subarray<br/>
using hashset to avoid duplicates

6. continuous subarray sum<br/>
multiple of K
prefix sum and %k the same results

### knapsack problem
knapsack involves choosing from candidates, with or without repetition
sometimes combined with some other restrictions.
if no repetition, the iteration on items shall be outside
if repetition is allowed, the iteration on weight (smaller problems) shall be outside

938. shopping offers<br/>
combined with some contrictions

944. target sum<br/>
add + and - to the numbers without repetition.

377. combination sum IV<br/>
with repetition

416. Partition equal subset sum<br/>
this could be solved using knapsack with one set.
no repetition

698. partition to k equal sum subsets<br/>
note: this is similar to knapsack, but actually cannot be solved using knapsack. knapsack does not check all combinations.
and then it will miss correct results. In this case, only dfs can try all possibilities.

301. perfect square<br/>
item is now i^2 with repetition

474. ones and zeros<br/>
similar to shopping offer with number constraints

322. coin change<br/>
using min number of coins

### greedy choice
greedy is generally based on intuition or some basic math

939. counting numbers with unique digits<br/>
simple math

264. ugly number II<br/>
*2 *3 *5 and tracking the current min

### climing stairs
the idea behind climbing stairs is that if we have several methods to reach i, the total method is the sum of all.

837. new 21 game<br/>
there are much more than 2 methods to reach target
also the target is not fixed but a range

### house robber
have some adjacent restrictions

213. house robber II<br/>
circular, two subproblems

740. delete and earn<br/>
need some preprocessing to convert to a house robber problem

### 1d to 2d
814. largest plus sign<br/>
use 1d to get two direction and combined to solve 2d 

221 maximal square<br/>
keep updating the height on each row

### minmax
max prices to solve a specific problem
min of the max for all kinds of problems

121. guess number higher or lower II<br/>





