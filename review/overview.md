leetcode overview

# array
905	Sort Array By Parity		Easy	<br/>
even followed by odd
two pointer, from each end, swap the first odd/even
O(N)+O(1)

832	Flipping an Image		Easy	<br/>
flip horizontal and flip the bit
reverse each row and xor O(N^2)

977	Squares of a Sorted Array		Easy	<br/>
contain negatives
using two pointer from each end.
for example [-5,-4,-3,1,2,3]
first compare -5 vs 3, add 25
compare -4 vs 3, add 16
compare -3 vs 3, add 9
compare -3 vs 2, add 9
compare 1 vs 2, add 4
add 1
O(n) space. O(n) time.

1051. Height checker
number of elements not in order
sort and compare O(nlogn)

561	Array Partition I		Easy	<br/>
partition to pairs so that sum of min to be maximum
greedy:
sort and paired. choose the 1st, 3rd....
proof: 
Sa=min(a1,b1)+min(a2,b2)+....min(an,bn)
Sab=a1+b1+...+an+bn=total=2*Sa-Sd. Sa=(T-Sd)/2
Sd=b1-a1+...+bn-an;
to maximize Sa, need to minimize Sd.

922	Sort Array By Parity II		Easy	<br/>
odd on the odd index, even on even index
similarly using two pointer one for odd and one for even, advance separetely

999	Available Captures for Rook		Easy	<br/>
R: rook
B: bishop
p: pawn.
lowercase for black and uppercase for white
return number of captures for the rook in one move (4 direction)
just check the 4 directions having p.

509	Fibonacci Number		Easy	<br/>
O(2^n) recursive with a lot of overlaps
O(N) dp with O(N) time and space
O(N)+O(1) to reduce dp memory usage

1002 Find Common Characters		Easy	<br/>
including duplicates
cnvert each string into a hashmap (using vector)
find all the common

985	Sum of Even Numbers After Queries		Easy	<br/>
query: [val,ind], we add val to the element at index.
even ->even, add the new val
even->odd, remove the previous even
odd->even, add the new even

867	Transpose Matrix		Easy	<br/>
swap(T(i,j),T(j,i)), be careful that row number != col number
need an extra matrix.

766	Toeplitz Matrix		Easy	<br/>
for every elements compare with its previous i-1, j-1

566	Reshape the Matrix		Easy	<br/>
just simple index conversion, convert to 1d and then convert back to new 2d

1089. duplicate zeros
duplicate the zeros and shift right. need in-place.
brutal force: just shift to right. O(N^2)
two pass: first decide how many zeros, then fill the elements from the other end O(N)

243	Shortest Word Distance 	Easy	<br/>
888	Fair Candy Swap		Easy	<br/>
exchange one, and both have equal candies. return A shall exchange and B shall exchange.
A+B=Const.
A-B=d.
A-d/2=B+d/2. so A shall gives out a candy with d/2 larger than B gives to A.
using hashset to find. 
O(N) hashset searching is constant.

1013 Partition Array Into Three Parts With Equal Sum		Easy	<br/>
needs to be subarray. check if we can.
get the target sum. then we try to divide.

896	Monotonic Array		Easy	<br/>
two pass: check sorted or reverse sorted.

485	Max Consecutive Ones		Easy	<br/>
longest subarray with all 1s.
just scan for left to right. to avoid the last step, add a 0 at the back.
using two pointers.

283	Move Zeroes		Easy	<br/>
move all zeros to behind
just use a two pointer to store the non-zeros. and then fill all zeros behind




169	Majority Element		Easy	<br/>
elements appear more than half
method 1: using a hash map to count O(n)+O(n)
method 2: voting, we just need one number. 
choose the first one as major element
if we see same, ++, diff: --
if cnt==0, then we choose current one as major.

122	Best Time to Buy and Sell Stock II		Easy	<br/>
dp

217	Contains Duplicate		Easy	<br/>
check if duplicate exists
method 1: using hash set to check if number exist O(N) + O(N)

167	Two Sum II - Input array is sorted		Easy	<br/>
- binary search target-A[i] in the previous O(nlogn) +O(1)
- hashset, O(n)+O(n)

697	Degree of an Array		Easy	<br/>
store the starting and ending index of each element

717	1-bit and 2-bit Characters		Easy	<br/>
1 bit: 0
2 bit: 10 and 11.
check if last char is a 1bit character or not.

if there is only one symbol in array the answer is always true (as last element is 0)
if there are two 0s at the end again the answer is true no matter what the rest symbols are( ...1100, ...1000,)
if there is 1 right before the last element(...10), the outcome depends on the count of sequential 1, i.e.
a) if there is odd amount of 1(10, ...01110, etc) the answer is false as there is a single 1 without pair
b) if it's even (110, ...011110, etc) the answer is true, as 0 at the end doesn't have anything to pair with

661	Image Smoother		Easy	<br/>
need an extra copy. otherwise incorrect.

268	Missing Number		Easy	<br/>
0 to n, n is the length, distinct, 
xor with 1 to n.

830	Positions of Large Groups		Easy	<br/>
find all subarrays with 3 or more same chars.
output in lex order.
two pointer

121	Best Time to Buy and Sell Stock		Easy	<br/>
dp

746	Min Cost Climbing Stairs		Easy	<br/>
dp

628	Maximum Product of Three Numbers		Easy	<br/>
greedy

1018 Binary Prefix Divisible By 5		Easy	<br/>
keep appending digits and %5 so that no overflow

118	Pascal's Triangle		Easy	<br/>
ith row has i+1 elements
left and right is 1
inner elements is the sum of previous row neighboring sum.
get the first nth row. 

119	Pascal's Triangle II		Easy	<br/>
only get the kth row
- we can still iterate from the first row, but only use the same storage
- the value of triangle at kth row is C(m,n) (excluding the left and right)

1010 Pairs of Songs With Total Durations Divisible by 60		Easy	<br/>
hashmap to group same %60
then choose from a and 60-a.

989	Add to Array-Form of Integer		Easy	<br/>
simple

27	Remove Element		Easy	<br/>
remove all instance = target. in-place
two pointer: swap i and j.

674	Longest Continuous Increasing Subsequence		Easy	<br/>
brutal force one pass.

1	Two Sum		Easy	<br/>
hashmap

53	Maximum Subarray		Easy	<br/>
subarray with max sum. has negatives.
if the sum drops below 0, then we need restart a new subarray.


66	Plus One		Easy	<br/>
simple

724	Find Pivot Index		Easy	<br/>
sum of its left=sum of its right
left+A[i]+right=tsum=2*left+A[i]=tsum -> 2*left=tsum-A[i]

35	Search Insert Position		Easy	<br/>
lower bound >=

849	Maximize Distance to Closest Person		Easy	<br/>
left and right shall be treated specially and middle.

26	Remove Duplicates from Sorted Array		Easy	<br/>
only one copy is left.
two pointer.

747	Largest Number At Least Twice of Others		Easy	<br/>
find the max and second max.
- find max and second max in O(n)
- sort.

643	Maximum Average Subarray I		Easy	<br/>
length=k.
sliding window to get the max sum.

624	Maximum Distance in 	Easy	<br/>

88	Merge Sorted Array		Easy	<br/>
two pointer merge

840	Magic Squares In Grid		Easy	<br/>
1 to 9 and row, col, diagonal sum are all the same.
check the number of magic square.
brutal force O(N^2)

219	Contains Duplicate II		Easy	<br/>
the index difference <=K.
using hashmap to record the index.
earlier index can be discarded.

941	Valid Mountain Array		Easy	<br/>
two pointer to climb the mountain.

914	X of a Kind in a Deck of Cards		Easy	<br/>
x>=2, count into hashmap and find the gcd

605	Can Place Flowers		Easy	<br/>
flower has to have space in its left and right (except the two boundaries)
to avoid boundary: let the previous flower at -2, and add 0 and 1 at the back.
then check the spots available.

581	Shortest Unsorted Continuous Subarray		Easy	<br/>
if sort the subarray, the whole array is sorted.
sort and find the first different from left and second difference from right

189	Rotate Array		Easy	<br/>
rotate by k steps. [1,2,3,4,5,6,7] rotate by 3 [5,6,7,1,2,3,4]
first limit k in 0 to n-1
method 1: reverse the whole, reverse the first k. reverse the remaining
method 2: using rotate

532	K-diff Pairs in an Array		Easy	<br/>
absolute difference is K.
method 1: sort, loop and find k+A[i], skip duplicates. O(nlogn)
method 2: hashmap O(N)

414	Third Maximum Number		Easy	<br/>
use O(n) to find the max, 2nd max, 3rd max.

665	Non-decreasing Array		Easy	<br/>
check if we can modify at most 1 element and the array is non-decreasing.
greedy: When you find nums[i-1] > nums[i] for some i, you will prefer to change nums[i-1]'s value, since a larger nums[i] will give you more risks that you get inversion errors after position i. But, if you also find nums[i-2] > nums[i], then you have to change nums[i]'s value instead, or else you need to change both of nums[i-2]'s and nums[i-1]'s values.
and then we count the changes.

950	Reveal Cards In Increasing Order		Medium	<br/>
reveal the top, move the next top to bottom, until no cards left
return an initial order which can reveal in increasing order.
approach: this requires, the smallest always on the top. sort it first in descending order, and then use a deque. reverse build the array
i.e. move the back to front, and then insert the element on the front
17,13,7,5,3,2
17 13 17 
11 17 13 
7 13 11 17 
5 17 7 13 11 
3 11 5 17 7 13 
2 13 3 11 5 17 7 

723	Candy Crush 	Medium	<br/>
969	Pancake Sorting		Medium	<br/>
a move: reverse the first k element with k<length to sort the array. any choice
greedy: find the largest and flip it to top, and then flip it to its position
[3,2,4,1]:
largest 4: k=3: [4,2,3,1]
flip k=4: [1,3,2,4]
largest 3: k=2, [3,1,2,4]
flip k=3: [2,1,3,4]
largest 2: k=2, [1,2,3,4]

280	Wiggle Sort 	Medium	<br/>

448	Find All Numbers Disappeared in an Array		Easy	<br/>
1 to n, n is the size of the array, some appear twice, some once, find all missing.

The idea is very similar to problem 442. Find All Duplicates in an Array: https://leetcode.com/problems/find-all-duplicates-in-an-array/.
First iteration to negate values at position whose equal to values appear in array. Second iteration to collect all position whose value is positive, which are the missing values. 
Complexity is O(n) Time and O(1) space.
for example: [4,3,2,7,8,2,3,1]
element 0: 4: we mark A[3] as -7, [4,3,2,-7,8,2,3,1]
element 1: 3: we mark A[2] as -2, [4,3,-2,-7,8,2,3,1]
element 2: 2: we mark A[1] as -3, [4,-3,-2,-7,8,2,3,1]
element 3: -7: we mark A[6] as -3, [4,-3,-2,-7,8,2,-3,1]
element 4: 8: we mark A[7] as -1, [4,-3,-2,-7,8,2,-3,-1]
element 5: 2, we mark A[1] as -3, [4,-3,-2,-7,8,2,-3,-1]
element 6: -3: we mark A[2] as -2, [4,-3,-2,-7,8,2,-3,-1]
element 7: -1: we mark A[0] as -4, [-4,-3,-2,-7,8,2,-3,-1]
only A[4] and A[5] are positive, so 5 and 6 are missing.
cyclic linking.

442	Find All Duplicates in an Array		Medium	<br/>
n elements and each element is in [1,n] some appeared twice, some appeared once. find all appeared twice.
method 1: sort, O(nlogn)
method 2: hashmap counting, O(n)+O(n)
method 3: cyclic linking. mark seen as -1
[4,3,2,7,8,2,3,1]
4: mark A[3]: [4,3,2,-7,8,2,3,1]
3: mark A[2]: [4,3,-2,-7,8,2,3,1]
2: mark A[1]: [4,-3,-2,-7,8,2,3,1]
7: mark A[6]: [4,-3,-2,-7,8,2,-3,1]
8: mark A[7]: [4,-3,-2,-7,8,2,-3,-1]
2: mark A[1]: it is already marked, it appeared twice
3: mark A[2], it is already marked, it appeared twice

370	Range Addition 	Medium	<br/>
531	Lonely Pixel I 	Medium	<br/>
695	Max Area of Island		Medium	<br/>
dfs

1031 Maximum Sum of Two Non-Overlapping Subarrays		Medium	<br/>
left sum and right sum from both direction.
make sure they do not overlaps
The two subarray's length shall be L and M.
sliding window sum max from left
sliding window sum max from right

238	Product of Array Except Self		Medium	<br/>
left product and right product from two direction.
no division is allowed.

245	Shortest Word Distance III 	Medium	<br/>
78	Subsets		Medium	<br/>
array of distinct elements, return all possible subset (power set 2^n)
backtracking

565	Array Nesting		Medium	<br/>
n elements from 0 to n-1. find the longest cycle.
treat it as a linked-list.
using a visited bool array for cycle detection

1011 Capacity To Ship Packages Within D Days		Medium	<br/>
binary search

495	Teemo Attacking		Medium	<br/>
time series, with a duration, this is interval merging.
update the tend and accumulate each segment's length.

835	Image Overlap		Medium	<br/>
two same sized images. translation in 2d. the overlap is the sum of product.
method 1: the dx dy from -(n-1) to (n-1). find the top left xy and bottom right xy, and calculate the overlaps. O(N^4)
method 2: the image could be a sparse matrix. we can store the image with only 1 in a larger sized image in 1d. for this example input max is 30*30
adding shift, max could be 100x100. loop on LA and LB and generate the hashmap i-j vs counter, choose the max.
prepairing O(N^2) looping O(La*Lb)

667	Beautiful Arrangement II		Medium	<br/>
arrange 1 to n, so that neighboring has exactly k distinct integers |a1-a0|=...=|An-1-An\
        //1,k+1,2,k,3,k-1,4,k-2.....but do not use duplicates
        //the distance is k,k-1......1
        //until they meet 
		
1052. grumpy bookstore owner
can apply a technique to not grumpy for x mins.
maximize the satisfaction.
sliding window of x to find the largest grumy window

216. Combination Sum III		
1 to 9, choose k number from them and add to a number n. find all combination.
backtracking.

769	Max Chunks To Make Sorted		Medium	<br/>
split into chunks and sort them separatly and the whole array is sorted.
compare with sorted, put into hashset and if the two are equal, then it is a segment.

1035 Uncrossed Lines		Medium	<br/>
dp, find the longest commin subsequence

714	Best Time to Buy and Sell Stock with Transaction Fee		Medium	<br/>
dp

900	RLE Iterator		Medium	<br/>
array: even index element is the number of repeat times of its next element.
next(n) remove n elements in the sequence
approach: separate the value into a vector, and index into a prefix length. then use binary search to find the index.

287	Find the Duplicate Number		Medium	<br/>
n+1 elements in the range [1,n]. find the duplicate one. assuming only one duplicate.
O(1) and cannot modify the array.
xor 1 to n. then the duplicate will appear 3 times.

926	Flip String to Monotone Increasing		Medium	<br/>
flip 0 to 1 and 1 to 0.
checking all position and left is all 0 and right is all 1.
from both direction and find the min sum of moves.

1014. best sightseeing pair
A[i]+A[j]+i-j maximize it.
two direction to do the A[i]+i and A[j]-j-1

39	Combination Sum		Medium	<br/>
no duplicates, find all combination adds up to target.
each number can be used multiple times.
backtracking or knapsack.

48	Rotate Image		Medium	<br/>
check with a simple example

62	Unique Paths		Medium	<br/>
dp

729	My Calendar I		Medium	<br/>
check if there is double booking. event is defined [s,e). so ending time overlap does not count.
method 1: bind start with +1 and end with -1, and sort it and check if the accumulated >=2
method 2: put the intervals in sorted order (set), and find the insert position and check if there is overlap


1007 Minimum Domino Rotations For Equal Row		Medium	<br/>
greedy

64	Minimum Path Sum		Medium	<br/>
dp

59	Spiral Matrix II		Medium	<br/>
generate the spiral matrix from 1 to n^2.
define top, bottom, left right boundary and fill

533	Lonely Pixel II 	Medium	<br/>
873	Length of Longest Fibonacci Subsequence		Medium	<br/>
dp

718	Maximum Length of Repeated Subarray		Medium	<br/>
dp

978	Longest Turbulent Subarray		Medium	<br/>
sign flipls between each adjacent pair.
brutal force compare with previous

16	3Sum Closest		Medium	<br/>
find the 3 sum closest to target
3 pointer

1040. moving stone until consecutive II
moving window to contain most stones.

621	Task Scheduler		Medium	<br/>
same task needs a cooling interval n. return the min number of intervals needed
greedy: hashmap to count each task, get the max, we need has (max-1) periods. The last period only has the ties.

289	Game of Life		Medium	<br/>
8 neighbors
<2 living neighbors, dies
=2 and 3, live
>3 dies
=3: dead become live
compute next status.
- boundary cells, outside are all 0 (dead)
- use high bits to indicate new status ( so we have old and new status)

611	Valid Triangle Number		Medium	<br/>
a+b>c is a valid triangle
sort first.
iterate two loops on the first two sides, binary search to find the lower_bound. O(n^2logn)

259	3Sum Smaller 	Medium	<br/>

11	Container With Most Water		Medium	<br/>
container min(Hl,Hr)*(r-l)
if Hl is smaller, l++, until we see a larger one
if Hr is smaller, r--, until we see a larger one

974	Subarray Sums Divisible by K		Medium	<br/>
prefix sum%k, same modular will be the subarray

562	Longest Line of Consecutive One in Matrix 	Medium	<br/>

548	Split Array with Equal Sum 	Medium	<br/>
795	Number of Subarrays with Bounded Maximum		Medium	<br/>
915	Partition Array into Disjoint Intervals		Medium	<br/>
parition into two subarray, left elements <=right elements
two direction: get the left max, and right min.

153	Find Minimum in Rotated Sorted Array		Medium	<br/>
binary search

380	Insert Delete GetRandom O(1)		Medium	<br/>
all O(1)
insert, remove, getrandom
a vector store the elements
a ihashmap to store the index (iterator)

945	Minimum Increment to Make Array Unique		Medium	<br/>
sort and then increase by 1

792	Number of Matching Subsequences		Medium	<br/>
a list of words, find number of words which are subsequence of S.
method 1: two pointer to determine if it is subsequence O(L) for each matching. O(nL) complexity
method 2: convert s to a map with the char vs its all indices. binary search for the next indices.

870	Advantage Shuffle		Medium	<br/>
shuffle A so that to maxmize the number of A[i]>B[i].
if cannot beat, use the smallest one. if can beat use the smallest one which can beat.
use a multiset to store or sort and binary search

90	Subsets II		Medium	<br/>
input has duplicates, but output cannot.
approach 1: backtracking with a set to get rid of duplicates
approach 2: sort it. the duplicated k numbers, we have option to include 0,1,2,..k elements of them.
keep appendng to previous results.
for example [1,2,2]
0: []
1: [],[1] (add 0 or 1)
2: [],[1],[2],[1,2],[2,2],[1,2,2] (add 0,1,2 of 2 to previous)

560	Subarray Sum Equals K		Medium	<br/>
find the total number of subarray sums to K.
make a hashset of the prefix sum vs the counter, and find the prefix-K and accumulate the number.

75	Sort Colors		Medium	<br/>
count sorting

40	Combination Sum II		Medium	<br/>
find all unique combination that sums to target
each number can only be used once
backtracking.

162	Find Peak Element		Medium	<br/>
binary search

962	Maximum Width Ramp		Medium	<br/>
a ramp is i<j and A[i]<=A[j] find the max width
greedy: get the lmin and rmax and get the larget width

105	Construct Binary Tree from Preorder and Inorder Traversal		Medium	<br/>
tree

80	Remove Duplicates from Sorted Array II		Medium	<br/>
inplace, leave the duplicates only <=twice.
two pointer

55	Pour Water 	Medium	<br/>
73	Set Matrix Zeroes		Medium	<br/>
if there is a zero in the matrix, set the row and column to be zero
record the row and colum in hashset

670	Maximum Swap		Medium	<br/>
a number, swap at most once to get the max number.
find the first left position and swap with its right max.

120	Triangle		Medium	<br/>
min path sum in triangle, dp

106	Construct Binary Tree from Inorder and Postorder Traversal		Medium	<br/>
tree

775	Global and Local Inversions		Medium	<br/>
global=local, only check the local inversion
0 to n-1, local inversion will only differ by 1.

277	Find the Celebrity 	Medium	<br/>

713	Subarray Product Less Than K		Medium	<br/>
sliding window product <k.
numbers are all >0, so if j-i product >K, then we have m*(m+1)/2 groups, we can keep adding 1,2,...m. (equivalent)

825	Friends Of Appropriate Ages		Medium	<br/>
create a hashmap ages vs count.
two group a*b if can request friend

228	Summary Ranges		Medium	<br/>
sorted array, return the summary of its ranges
two pointers.

56	Merge Intervals		Medium	<br/>
sort using the start, and keep growing the end by merging the start<end intervals

74	Search a 2D Matrix		Medium	<br/>
row sorted and next row is larger than previous
while matrix is sorted, essentially it is a 1d problem.

209	Minimum Size Subarray Sum		Medium	<br/>
all positives find the min size subarray sum>=K.
using two pointer sliding window. when sum>=k record the length. and advance the left pointer

954	Array of Doubled Pairs		Medium	<br/>
array length is even, check if we can reorder so that A[2*i+1]=2*A[i].
first, positive and negatives shall be partitioned
second, put into a multiset and do it from smallest.

34	Find First and Last Position of Element in Sorted Array		Medium	<br/>
binary search or equal range

63	Unique Paths II		Medium	<br/>
dp

33	Search in Rotated Sorted Array		Medium	<br/>
binary search

81	Search in Rotated Sorted Array II		Medium	<br/>
binary search

229	Majority Element II		Medium	<br/>
voting algorithm. >1/3n
need to verify if the two candidates are valid.

55	Jump Game		Medium	<br/>
array represents the max steps you can jump. check if we can reach the end.
using bfs like method. the max jump covers all the element which are all accessible and keep updating the reachable.

918	Maximum Sum Circular Subarray		Medium	<br/>
using max and min to avoid circular.

79	Word Search		Medium	<br/>
find one word in matrix.
dfs!

18	4Sum		Medium	<br/>
using 4 pointers, O(N^3)

31	Next Permutation		Medium	<br/>
get the next largest permutation.
find the first sorted position and swap with the right smallest one which > the current one
then sort the right (reverse if fine since right is decending order)

54	Spiral Matrix		Medium	<br/>
return the elements in sprial order.
using 4 boundaries.

152	Maximum Product Subarray		Medium	<br/>
contain negatives
need keep max and min at the same time
only need the max number so we can use dp.

457	Circular Array Loop		Medium	<br/>
the number in the array is the step you can jump. positive goes forward, negative goes backward.
determine if there is a loop (a loop shall contain all negative or all positives)
start a node and record its sign and visited nodes, to see if we need exit or found a loop.

907	Sum of Subarray Minimums		Medium	<br/>
all subarray's min sum. 
every number can be the min. find the window for each element. using two pointers.

15	3Sum		Medium	<br/>
three pointers O(N^2)

163	Missing Ranges 	Medium	<br/>

1074. number of submatrices that sum to target
using the subarray sum to K for 1d array, expand to 2d array
kadane algorithm.


768	Max Chunks To Make Sorted II		Hard	<br/>
same as I, but this allows for duplicates
using multiset works for both cases.

689	Maximum Sum of 3 Non-Overlapping Subarrays		Hard	<br/>
window size is K for 3 subarrays. maxmize the sum of the 3 non-overlapping subarrays
get the lmax, rmax and the mid has a range to loop

42	Trapping Rain Water		Hard	<br/>
stack problem: keep a decreasing stack, once the incoming one is larger, then pop back all smaller ones.

128	Longest Consecutive Sequence		Hard	<br/>
union-find

782	Transform to Chessboard		Hard	<br/>
min number of moves to transform the matrix into a chess board. you can swap rows or columns
greedy choice: you have to make the first row 0101... and first column into 0101...
matrix[0][0] can be 0 or 1. If n is even, does not matter. if n is odd, it depends 0 is larger or 1 is larger.


154	Find Minimum in Rotated Sorted Array II		Hard	<br/>
find the min, containing duplicates
binary search in 3 cases. ><=

123	Best Time to Buy and Sell Stock III		Hard	<br/>
dp

84	Largest Rectangle in Histogram		Hard	<br/>
find the largest area of the histogram bars.
- add a 0 to both end to avoid boundary
- keep the stack increasing. when incoming is smaller, the current bar can form a rect.

85	Maximal Rectangle		Hard	<br/>
based on stack problem histogram

381	Insert Delete GetRandom O(1) - Duplicates allowed		Hard	<br/>
duplicates has several index with it, hashmap need be key vs a set of indices

57	Insert Interval		Hard	<br/>
sort the intervals, find the lower_bound and keeps merging.

719	Find K-th Smallest Pair Distance		Hard	<br/>
distance |a-b|, find the kth pair.
the difference is the between 0 and max-min.
can use binary search to count the number of dist<m.
the counting is O(N^2)
another method: put the difference in an array with the index as the difference, (hashmap) and then accumulate the cnt until to K.

41	First Missing Positive		Hard	<br/>
two pass: first put elements in its position, does not care about those overboundary ones
second, find the first one which does not equal indx+1

891	Sum of Subsequence Widths		Hard	<br/>
width of a subarray=max-min
a single element: width=0
math problem: 
-. the initial order does not matter
-. for A[i], there are i smaller numbers. so there are 2^i sequences which A[i] is max
-. for A[i], there are n-1-i bigger numbers, so there are 2^(n-1-i) as minimum
res+=A[i]*(2^i-2^(n-1-i))

644	Maximum Average Subarray II 	Hard	<br/>

45	Jump Game II		Hard	<br/>
min number of jumps to get to the last element
bfs.

4	Median of Two Sorted Arrays		Hard	<br/>
binary search for two sorted array

126	Word Ladder II		Hard<br/>
bfs. need to understand deeply the bfs scheme.

# string
709	To Lower Case	76.9%	Easy	
804	Unique Morse Code Words	74.8%	Easy	
657	Robot Return to Origin	71.6%	Easy	
929	Unique Email Addresses	70.7%	Easy	
557	Reverse Words in a String III	64.3%	Easy	
344	Reverse String	63.4%	Easy	
893	Groups of Special-Equivalent Strings	62.8%	Easy	
800	Similar RGB Color 59.7%	Easy	
1065	Index Pairs of a String 59.0%	Easy	
293	Flip Game 58.9%	Easy	
824	Goat Latin	57.9%	Easy	
937	Reorder Log Files	56.9%	Easy	
521	Longest Uncommon Subsequence I	56.5%	Easy	
917	Reverse Only Letters	56.0%	Easy	
788	Rotated Digits	54.3%	Easy	
696	Count Binary Substrings	53.5%	Easy	
1071 Greatest Common Divisor of Strings	53.5%	Easy	
13	Roman to Integer	52.6%	Easy	
520	Detect Capital	52.5%	Easy	
606	Construct String from Binary Tree	51.7%	Easy	
387	First Unique Character in a String	50.2%	Easy	
383	Ransom Note	50.0%	Easy	
541	Reverse String II	45.6%	Easy	
551	Student Attendance Record I	45.3%	Easy	
925	Long Pressed Name	44.4%	Easy	
415	Add Strings	43.8%	Easy	
758	Bold Words in String 42.5%	Easy	
819	Most Common Word	42.2%	Easy	
345	Reverse Vowels of a String	41.6%	Easy	
38	Count and Say	40.7%	Easy	
459	Repeated Substring Pattern	40.0%	Easy	
67	Add Binary	39.4%	Easy	
443	String Compression	37.8%	Easy	
434	Number of Segments in a String	37.0%	Easy	
20	Valid Parentheses	36.7%	Easy	
680	Valid Palindrome II	34.3%	Easy	
14	Longest Common Prefix	33.6%	Easy	
58	Length of Last Word	32.3%	Easy	
28	Implement strStr()	32.2%	Easy	
686	Repeated String Match	31.4%	Easy	
125	Valid Palindrome	31.3%	Easy	
157	Read N Characters Given Read4 29.6%	Easy	
408	Valid Word Abbreviation 29.6%	Easy	
859	Buddy Strings	27.6%	Easy	
544	Output Contest Matches 73.5%	Medium	
890	Find and Replace Pattern	71.2%	Medium	
537	Complex Number Multiplication	65.6%	Medium	
791	Custom Sort String	62.2%	Medium	
1016 Binary String With Substrings Representing 1 To N	61.1%	Medium	
647	Palindromic Substrings	57.1%	Medium	
1023 Camelcase Matching	56.5%	Medium	
856	Score of Parentheses	56.3%	Medium	
553	Optimal Division	55.4%	Medium	
609	Find Duplicate File in System	55.4%	Medium	
22	Generate Parentheses	55.3%	Medium	
635	Design Log Storage System 54.2%	Medium	
1003	Check If Word Is Valid After Substitutions	51.7%	Medium	
12	Integer to Roman	51.2%	Medium	
1062 Longest Repeating Substring 50.4%	Medium	
249	Group Shifted Strings 49.0%	Medium	
539	Minimum Time Difference	48.0%	Medium	
49	Group Anagrams	47.2%	Medium	
833	Find And Replace in String	46.4%	Medium	
916	Word Subsets	45.4%	Medium	
536	Construct Binary Tree from String 45.1%	Medium	
583	Delete Operation for Two Strings	45.1%	Medium	
816	Ambiguous Coordinates	44.2%	Medium	
809	Expressive Words	43.6%	Medium	
681	Next Closest Time 42.7%	Medium	
767	Reorganize String	42.6%	Medium	
1081 Smallest Subsequence of Distinct Characters	42.4%	Medium	
831	Masking Personal Information	42.1%	Medium	
17	Letter Combinations of a Phone Number	41.8%	Medium	
966	Vowel Spellchecker	41.7%	Medium	
848	Shifting Letters	40.9%	Medium	
555	Split Concatenated Strings 39.9%	Medium	
616	Add Bold Tag in String 39.3%	Medium	
186	Reverse Words in a String II 38.0%	Medium	
842	Split Array into Fibonacci Sequence	35.0%	Medium	
227	Basic Calculator II	33.6%	Medium	
522	Longest Uncommon Subsequence II	32.9%	Medium	
678	Valid Parenthesis String	32.8%	Medium	
6	ZigZag Conversion	32.1%	Medium	
385	Mini Parser	31.9%	Medium	
93	Restore IP Addresses	31.7%	Medium	
161	One Edit Distance 31.7%	Medium	
722	Remove Comments	31.3%	Medium	
43	Multiply Strings	30.9%	Medium	
556	Next Greater Element III	30.0%	Medium	
71	Simplify Path	28.9%	Medium	
3	Longest Substring Without Repeating Characters	28.5%	Medium	
5	Longest Palindromic Substring	27.4%	Medium	
271	Encode and Decode Strings 26.9%	Medium	
165	Compare Version Numbers	23.7%	Medium	
91	Decode Ways	22.4%	Medium	
468	Validate IP Address	21.4%	Medium	
151	Reverse Words in a String	16.9%	Medium	
8	String to Integer (atoi)	14.7%	Medium	
761	Special Binary String	51.8%	Hard	
527	Word Abbreviation 50.1%	Hard	
632	Smallest Range	47.7%	Hard	
899	Orderly Queue	47.3%	Hard	
159	Longest Substring with At Most Two Distinct Characters 47.2%	Hard	
770	Basic Calculator IV	45.6%	Hard	
736	Parse Lisp Expression	44.5%	Hard	
772	Basic Calculator III 43.7%	Hard	
340	Longest Substring with At Most K Distinct Characters 39.9%	Hard	
730	Count Different Palindromic Subsequences	39.1%	Hard	
72	Edit Distance	38.0%	Hard	
936	Stamping The Sequence	36.1%	Hard	
115	Distinct Subsequences	35.2%	Hard	
591	Tag Validator	32.8%	Hard	
87	Scramble String	31.7%	Hard	
336	Palindrome Pairs	31.1%	Hard	
76	Minimum Window Substring	31.0%	Hard	
97	Interleaving String	28.2%	Hard	
214	Shortest Palindrome	27.7%	Hard	
158	Read N Characters Given Read4 II - Call multiple times 26.6%	Hard	
32	Longest Valid Parentheses	25.6%	Hard	
10	Regular Expression Matching	25.3%	Hard	
273	Integer to English Words	24.4%	Hard	
30	Substring with Concatenation of All Words	23.7%	Hard	
68	Text Justification	23.5%	Hard	
44	Wildcard Matching	22.9%	Hard	
564	Find the Closest Palindrome	18.8%	Hard	
126	Word Ladder II	17.9%	Hard	
65	Valid Number	14.0%	Hard

# tree
938	Range Sum of BST	79.0%	Easy	
617	Merge Two Binary Trees	70.4%	Easy	
700	Search in a Binary Search Tree	68.5%	Easy	
589	N-ary Tree Preorder Traversal	67.8%	Easy	
590	N-ary Tree Postorder Traversal	67.8%	Easy	
965	Univalued Binary Tree	66.9%	Easy	
559	Maximum Depth of N-ary Tree	65.6%	Easy	
897	Increasing Order Search Tree	64.8%	Easy	
872	Leaf-Similar Trees	63.4%	Easy	
104	Maximum Depth of Binary Tree	60.8%	Easy	
669	Trim a Binary Search Tree	60.4%	Easy	
429	N-ary Tree Level Order Traversal	59.7%	Easy	
637	Average of Levels in Binary Tree	58.9%	Easy	
226	Invert Binary Tree	58.5%	Easy	
1022	Sum of Root To Leaf Binary Numbers	55.3%	Easy	
653	Two Sum IV - Input is a BST	52.6%	Easy	
993	Cousins in Binary Tree	52.5%	Easy	
606	Construct String from Binary Tree	51.7%	Easy	
538	Convert BST to Greater Tree	51.2%	Easy	
108	Convert Sorted Array to Binary Search Tree	51.1%	Easy	
530	Minimum Absolute Difference in BST	50.7%	Easy	
783	Minimum Distance Between BST Nodes	50.6%	Easy	
100	Same Tree	50.2%	Easy	
404	Sum of Left Leaves	49.2%	Easy	
107	Binary Tree Level Order Traversal II	47.1%	Easy	
563	Binary Tree Tilt	47.0%	Easy	
543	Diameter of Binary Tree	46.9%	Easy	
257	Binary Tree Paths	46.2%	Easy	
235	Lowest Common Ancestor of a Binary Search Tree	45.0%	Easy	
270	Closest Binary Search Tree Value
43.9%	Easy	
101	Symmetric Tree	43.6%	Easy	
671	Second Minimum Node In a Binary Tree	43.4%	Easy	
437	Path Sum III	43.0%	Easy	
572	Subtree of Another Tree	41.8%	Easy	
110	Balanced Binary Tree	41.1%	Easy	
501	Find Mode in Binary Search Tree	39.6%	Easy	
112	Path Sum	37.9%	Easy	
111	Minimum Depth of Binary Tree	35.4%	Easy	
687	Longest Univalue Path	33.9%	Easy	
654	Maximum Binary Tree	76.3%	Medium	
701	Insert into a Binary Search Tree	76.0%	Medium	
1008	Construct Binary Search Tree from Preorder Traversal	73.2%	Medium	
814	Binary Tree Pruning	71.1%	Medium	
894	All Possible Full Binary Trees	71.0%	Medium	
979	Distribute Coins in Binary Tree	67.2%	Medium	
366	Find Leaves of Binary Tree 66.0%	Medium	
951	Flip Equivalent Binary Trees	65.3%	Medium	
998	Maximum Binary Tree II	61.6%	Medium	
889	Construct Binary Tree from Preorder and Postorder Traversal	60.3%	Medium	
1026	Maximum Difference Between Node and Ancestor	59.5%	Medium	
513	Find Bottom Left Tree Value	58.6%	Medium	
515	Find Largest Value in Each Tree Row	57.9%	Medium	
94	Binary Tree Inorder Traversal	57.0%	Medium	
582	Kill Process 56.5%	Medium	
865	Smallest Subtree with all the Deepest Nodes	55.9%	Medium	
919	Complete Binary Tree Inserter	55.2%	Medium	
508	Most Frequent Subtree Sum	54.7%	Medium	
510	Inorder Successor in BST II 53.2%	Medium	
776	Split BST 52.6%	Medium	
426	Convert Binary Search Tree to Sorted Doubly Linked List 52.4%	Medium	
666	Path Sum IV 52.3%	Medium	
684	Redundant Connection	52.0%	Medium	
655	Print Binary Tree	51.9%	Medium	
230	Kth Smallest Element in a BST	51.8%	Medium	
144	Binary Tree Preorder Traversal	51.6%	Medium	
156	Binary Tree Upside Down 51.0%	Medium	
250	Count Univalue Subtrees 49.0%	Medium	
173	Binary Search Tree Iterator	48.9%	Medium	
102	Binary Tree Level Order Traversal	48.8%	Medium	
199	Binary Tree Right Side View	48.2%	Medium	
337	House Robber III	48.2%	Medium	
863	All Nodes Distance K in Binary Tree	47.8%	Medium	
449	Serialize and Deserialize BST	47.4%	Medium	
958	Check Completeness of a Binary Tree	47.4%	Medium	
623	Add One Row to Tree	47.3%	Medium	
96	Unique Binary Search Trees	46.5%	Medium	
988	Smallest String Starting From Leaf	46.0%	Medium	
652	Find Duplicate Subtrees	45.7%	Medium	
536	Construct Binary Tree from String 45.1%	Medium	
549	Binary Tree Longest Consecutive Sequence II 44.5%	Medium	
298	Binary Tree Longest Consecutive Sequence 44.1%	Medium	
255	Verify Preorder Sequence in Binary Search Tree 43.7%	Medium	
971	Flip Binary Tree To Match Preorder Traversal	42.9%	Medium	
114	Flatten Binary Tree to Linked List	42.7%	Medium	
129	Sum Root to Leaf Numbers	42.7%	Medium	
103	Binary Tree Zigzag Level Order Traversal	42.0%	Medium	
105	Construct Binary Tree from Preorder and Inorder Traversal	41.5%	Medium	
113	Path Sum II	40.9%	Medium	
450	Delete Node in a BST	40.1%	Medium	
106	Construct Binary Tree from Inorder and Postorder Traversal	39.6%	Medium	
662	Maximum Width of Binary Tree	39.6%	Medium	
742	Closest Leaf in a Binary Tree 39.2%	Medium	
116	Populating Next Right Pointers in Each Node	38.2%	Medium	
663	Equal Tree Partition 38.1%	Medium	
236	Lowest Common Ancestor of a Binary Tree	37.7%	Medium	
95	Unique Binary Search Trees II	35.9%	Medium	
545	Boundary of Binary Tree 35.2%	Medium	
285	Inorder Successor in BST 35.0%	Medium	
117	Populating Next Right Pointers in Each Node II	34.6%	Medium	
222	Count Complete Tree Nodes	34.1%	Medium	
333	Largest BST Subtree 33.2%	Medium	
987	Vertical Order Traversal of a Binary Tree	31.9%	Medium	
98	Validate Binary Search Tree	25.8%	Medium	
1028	Recover a Tree From Preorder Traversal	70.1%	Hard	
431	Encode N-ary Tree to Binary Tree 63.9%	Hard	
428	Serialize and Deserialize N-ary Tree 54.0%	Hard	
145	Binary Tree Postorder Traversal	48.8%	Hard	
272	Closest Binary Search Tree Value II 45.3%	Hard	
297	Serialize and Deserialize Binary Tree	41.2%	Hard	
834	Sum of Distances in Tree	39.6%	Hard	
968	Binary Tree Cameras	35.3%	Hard	
99	Recover Binary Search Tree	34.8%	Hard	
685	Redundant Connection II	30.8%	Hard	
124	Binary Tree Maximum Path Sum	30.2%	Hard

# hash table
771	Jewels and Stones	83.1%	Easy	
760	Find Anagram Mappings 79.4%	Easy	
1086	High Five 76.4%	Easy	
961	N-Repeated Element in Size 2N Array	72.4%	Easy	
1078	Occurrences After Bigram	67.6%	Easy	
1002	Find Common Characters	65.8%	Easy	
811	Subdomain Visit Count	65.5%	Easy	
359	Logger Rate Limiter 65.3%	Easy	
500	Keyboard Row	62.3%	Easy	
463	Island Perimeter	61.1%	Easy	
884	Uncommon Words from Two Sentences	60.9%	Easy	
136	Single Number	60.4%	Easy	
266	Palindrome Permutation 60.1%	Easy	
575	Distribute Candies	59.8%	Easy	
706	Design HashMap	56.2%	Easy	
953	Verifying an Alien Dictionary	55.5%	Easy	
349	Intersection of Two Arrays	55.0%	Easy	
748	Shortest Completing Word	54.5%	Easy	
690	Employee Importance	54.3%	Easy	
705	Design HashSet	54.0%	Easy	
389	Find the Difference	53.2%	Easy	
242	Valid Anagram	52.4%	Easy	
217	Contains Duplicate	52.2%	Easy	
387	First Unique Character in a String	50.2%	Easy	
447	Number of Boomerangs	50.0%	Easy	
409	Longest Palindrome	48.1%	Easy	
599	Minimum Index Sum of Two Lists	48.1%	Easy	
350	Intersection of Two Arrays II	48.0%	Easy	
202	Happy Number	45.5%	Easy	
720	Longest Word in Dictionary	44.9%	Easy	
1	Two Sum	44.2%	Easy	
594	Longest Harmonious Subsequence	44.0%	Easy	
246	Strobogrammatic Number 42.4%	Easy	
645	Set Mismatch	40.8%	Easy	
734	Sentence Similarity 40.7%	Easy	
970	Powerful Integers	39.4%	Easy	
205	Isomorphic Strings	37.5%	Easy	
438	Find All Anagrams in a String	37.4%	Easy	
624	Maximum Distance in Arrays 37.4%	Easy	
219	Contains Duplicate II	35.4%	Easy	
290	Word Pattern	35.1%	Easy	
170	Two Sum III - Data structure design 30.6%	Easy	
204	Count Primes	29.1%	Easy	
535	Encode and Decode TinyURL	76.9%	Medium	
739	Daily Temperatures	60.1%	Medium	
94	Binary Tree Inorder Traversal	57.0%	Medium	
311	Sparse Matrix Multiplication 56.6%	Medium	
451	Sort Characters By Frequency	56.3%	Medium	
1090	Largest Values From Labels	56.0%	Medium	
1072	Flip Columns For Maximum Number of Equal Rows	55.9%	Medium	
609	Find Duplicate File in System	55.4%	Medium	
347	Top K Frequent Elements	55.2%	Medium	
508	Most Frequent Subtree Sum	54.7%	Medium	
648	Replace Words	52.1%	Medium	
781	Rabbits in Forest	51.9%	Medium	
676	Implement Magic Dictionary	51.6%	Medium	
694	Number of Distinct Islands 51.3%	Medium	
454	4Sum II	50.8%	Medium	
981	Time Based Key-Value Store	50.8%	Medium	
939	Minimum Area Rectangle	50.5%	Medium	
249	Group Shifted Strings 49.0%	Medium	
554	Brick Wall	47.8%	Medium	
244	Shortest Word Distance II 47.5%	Medium	
49	Group Anagrams	47.2%	Medium	
1048	Longest String Chain	47.2%	Medium	
718	Maximum Length of Repeated Subarray	46.2%	Medium	
692	Top K Frequent Words	45.9%	Medium	
974	Subarray Sums Divisible by K	45.1%	Medium	
325	Maximum Size Subarray Sum Equals k 44.8%	Medium	
36	Valid Sudoku	43.4%	Medium	
380	Insert Delete GetRandom O(1)	42.9%	Medium	
525	Contiguous Array	42.8%	Medium	
560	Subarray Sum Equals K	42.2%	Medium	
966	Vowel Spellchecker	41.7%	Medium	
314	Binary Tree Vertical Order Traversal 41.1%	Medium	
299	Bulls and Cows	39.6%	Medium	
930	Binary Subarrays With Sum	38.1%	Medium	
957	Prison Cells After N Days	38.1%	Medium	
187	Repeated DNA Sequences	36.2%	Medium	
274	H-Index	34.7%	Medium	
954	Array of Doubled Pairs	34.3%	Medium	
987	Vertical Order Traversal of a Binary Tree	31.9%	Medium	
356	Line Reflection 30.9%	Medium	
18	4Sum	30.7%	Medium	
3	Longest Substring Without Repeating Characters	28.5%	Medium	
355	Design Twitter	27.5%	Medium	
138	Copy List with Random Pointer	27.4%	Medium	
288	Unique Word Abbreviation 20.1%	Medium	
166	Fraction to Recurring Decimal	19.5%	Medium	
895	Maximum Frequency Stack	56.2%	Hard	
632	Smallest Range	47.7%	Hard	
159	Longest Substring with At Most Two Distinct Characters 47.2%	Hard	
711	Number of Distinct Islands II 46.3%	Hard	
770	Basic Calculator IV	45.6%	Hard	
992	Subarrays with K Different Integers	45.2%	Hard	
726	Number of Atoms	44.8%	Hard	
340	Longest Substring with At Most K Distinct Characters 39.9%	Hard	
37	Sudoku Solver	37.3%	Hard	
1001	Grid Illumination	34.3%	Hard	
85	Maximal Rectangle	33.5%	Hard	
358	Rearrange String k Distance Apart 32.9%	Hard	
381	Insert Delete GetRandom O(1) - Duplicates allowed	32.1%	Hard	
710	Random Pick with Blacklist	31.3%	Hard	
336	Palindrome Pairs	31.1%	Hard	
76	Minimum Window Substring	31.0%	Hard	
30	Substring with Concatenation of All Words	23.7%	Hard	
1044	Longest Duplicate Substring	22.8%	Hard	
149	Max Points on a Line	15.8%	Hard	

# stack
 	#	Title	Acceptance	Difficulty	Frequency
1021	Remove Outermost Parentheses	76.0%	Easy	
1047	Remove All Adjacent Duplicates In String	63.3%	Easy	
682	Baseball Game	61.1%	Easy	
496	Next Greater Element I	59.7%	Easy	
844	Backspace String Compare	46.0%	Easy	
232	Implement Queue using Stacks	43.6%	Easy	
225	Implement Stack using Queues	39.5%	Easy	
155	Min Stack	37.3%	Easy	
20	Valid Parentheses	36.7%	Easy	
921	Minimum Add to Make Parentheses Valid	70.2%	Medium	
739	Daily Temperatures	60.1%	Medium	
946	Validate Stack Sequences	57.5%	Medium	
94	Binary Tree Inorder Traversal	57.0%	Medium	
856	Score of Parentheses	56.3%	Medium	
1019	Next Greater Node In Linked List	56.3%	Medium	
439	Ternary Expression Parser
53.6%	Medium	
1003	Check If Word Is Valid After Substitutions	51.7%	Medium	
144	Binary Tree Preorder Traversal	51.6%	Medium	
503	Next Greater Element II	51.2%	Medium	
901	Online Stock Span	49.2%	Medium	
173	Binary Search Tree Iterator	48.9%	Medium	
636	Exclusive Time of Functions	48.7%	Medium	
341	Flatten Nested List Iterator	48.1%	Medium	
394	Decode String	45.1%	Medium	
255	Verify Preorder Sequence in Binary Search Tree
43.7%	Medium	
103	Binary Tree Zigzag Level Order Traversal	42.0%	Medium	
331	Verify Preorder Serialization of a Binary Tree	38.7%	Medium	
735	Asteroid Collision	38.5%	Medium	
150	Evaluate Reverse Polish Notation	32.4%	Medium	
385	Mini Parser	31.9%	Medium	
71	Simplify Path	28.9%	Medium	
907	Sum of Subarray Minimums	27.5%	Medium	
456	132 Pattern	27.4%	Medium	
402	Remove K Digits	26.7%	Medium	
880	Decoded String at Index	23.0%	Medium	
1063	Number of Valid Subarrays
73.8%	Hard	
895	Maximum Frequency Stack	56.2%	Hard	
145	Binary Tree Postorder Traversal	48.8%	Hard	
975	Odd Even Jump	47.6%	Hard	
770	Basic Calculator IV	45.6%	Hard	
272	Closest Binary Search Tree Value II
45.3%	Hard	
726	Number of Atoms	44.8%	Hard	
772	Basic Calculator III
43.7%	Hard	
42	Trapping Rain Water	43.5%	Hard	
85	Maximal Rectangle	33.5%	Hard	
224	Basic Calculator	32.9%	Hard	
316	Remove Duplicate Letters	32.8%	Hard	
591	Tag Validator	32.8%	Hard	
84	Largest Rectangle in Histogram	31.4%	Hard	


# linked list
 	#	Title	Acceptance	Difficulty	Frequency
876	Middle of the Linked List	64.3%	Easy	
206	Reverse Linked List	55.1%	Easy	
237	Delete Node in a Linked List	53.9%	Easy	
21	Merge Two Sorted Lists	47.6%	Easy	
83	Remove Duplicates from Sorted List	42.7%	Easy	
141	Linked List Cycle	37.1%	Easy	
234	Palindrome Linked List	36.2%	Easy	
203	Remove Linked List Elements	35.9%	Easy	
160	Intersection of Two Linked Lists	34.2%	Easy	
707	Design Linked List	21.7%	Easy	
369	Plus One Linked List
56.3%	Medium	
1019	Next Greater Node In Linked List	56.3%	Medium	
817	Linked List Components	54.7%	Medium	
426	Convert Binary Search Tree to Sorted Doubly Linked List
52.4%	Medium	
445	Add Two Numbers II	50.3%	Medium	
328	Odd Even Linked List	49.4%	Medium	
725	Split Linked List in Parts	49.1%	Medium	
24	Swap Nodes in Pairs	45.0%	Medium	
430	Flatten a Multilevel Doubly Linked List	42.5%	Medium	
379	Design Phone Directory
41.6%	Medium	
109	Convert Sorted List to Binary Search Tree	41.1%	Medium	
86	Partition List	37.5%	Medium	
147	Insertion Sort List	37.5%	Medium	
148	Sort List	35.7%	Medium	
92	Reverse Linked List II	35.1%	Medium	
19	Remove Nth Node From End of List	34.3%	Medium	
82	Remove Duplicates from Sorted List II	33.2%	Medium	
142	Linked List Cycle II	32.3%	Medium	
2	Add Two Numbers	31.2%	Medium	
143	Reorder List	31.1%	Medium	
708	Insert into a Cyclic Sorted List
29.1%	Medium	
138	Copy List with Random Pointer	27.4%	Medium	
61	Rotate List	27.3%	Medium	
25	Reverse Nodes in k-Group	36.7%	Hard	
23	Merge k Sorted Lists	34.8%	Hard	

# heap
 	#	Title	Acceptance	Difficulty	Frequency
1046	Last Stone Weight	62.6%	Easy	
703	Kth Largest Element in a Stream	46.5%	Easy	
973	K Closest Points to Origin	62.1%	Medium	
451	Sort Characters By Frequency	56.3%	Medium	
347	Top K Frequent Elements	55.2%	Medium	
378	Kth Smallest Element in a Sorted Matrix	49.6%	Medium	
215	Kth Largest Element in an Array	48.2%	Medium	
692	Top K Frequent Words	45.9%	Medium	
253	Meeting Rooms II
43.0%	Medium	
767	Reorganize String	42.6%	Medium	
743	Network Delay Time	42.2%	Medium	
313	Super Ugly Number	41.6%	Medium	
659	Split Array into Consecutive Subsequences	40.8%	Medium	
1054	Distant Barcodes	38.2%	Medium	
264	Ugly Number II	36.6%	Medium	
787	Cheapest Flights Within K Stops	35.1%	Medium	
373	Find K Pairs with Smallest Sums	34.0%	Medium	
355	Design Twitter	27.5%	Medium	
759	Employee Free Time
61.4%	Hard	
778	Swim in Rising Water	47.8%	Hard	
857	Minimum Cost to Hire K Workers	47.5%	Hard	
786	K-th Smallest Prime Fraction	39.9%	Hard	
407	Trapping Rain Water II	39.3%	Hard	
239	Sliding Window Maximum	38.4%	Hard	
882	Reachable Nodes In Subdivided Graph	38.2%	Hard	
502	IPO	37.9%	Hard	
295	Find Median from Data Stream	36.9%	Hard	
864	Shortest Path to Get All Keys	36.2%	Hard	
818	Race Car	35.1%	Hard	
23	Merge k Sorted Lists	34.8%	Hard	
358	Rearrange String k Distance Apart
32.9%	Hard	
218	The Skyline Problem	31.8%	Hard	
719	Find K-th Smallest Pair Distance	29.2%	Hard	
871	Minimum Number of Refueling Stops	29.0%	Hard

# queue

# trie

# design

# disjoint set

# graph


Algorithms
# dfs
# binary search
# bfs
# backtracking
# divide and conquer
# two pointer
# dynmaic programming

1025	Divisor Game		Easy	<br/>
choose any factor of N (not including n itself) and subtract x.
if you cannot make a move, you lose.
greedy: n is odd, it only has odd factor, you always pass even to the other
if n is even, you can always -1, and pass an odd to the other
so even number I will win.

256	Paint House 	Easy	<br/>
121	Best Time to Buy and Sell Stock		Easy	<br/>
only at most one transaction is allowed.
dp[i]=min(dp[i-1],dp[j]+price[i]-price[j])
reduce to O(N) by iterating update the dp[j]-price[j]

746	Min Cost Climbing Stairs		Easy	<br/>
cost[i] to step to next.
one or two step: dp[i]=min(dp[i-2],dp[i-1])+cost[i-1]
can reduce O(n) space to O(1)

70	Climbing Stairs		Easy	<br/>
number of ways to reach using 1 step or two step
dp[i]=dp[i-1]+dp[i-2]

53	Maximum Subarray		Easy	<br/>
subarray with largest sum. return the largest sum.
it contains negatives. 
- for all negatives, it is the max element.
- for all positives, it is the whole array sum.
- for mixed, if the prefix sum<0 then we need restart a new subarray, (which covers the previous two cases)
local maxsum vs global maxsum

198	House Robber		Easy	<br/>
cannot robber neighboring houses
dp[i]=max(dp[i-1],dp[i-2]+money[i])

303	Range Sum Query - Immutable		Easy	<br/>
find the sum between [i,j]= prefix(i)-prefix(j-1)
if add a zero, which is quite regular, sum(i,j)=prefix(i+1)-prefix(j)

276	Paint Fence 	Easy	<br/>
<br/>
338	Counting Bits		Medium	<br/>
dp[i]=dp[i/2]+i&1
reduce O(n) to O(logn)


750	Number Of Corner Rectangles 	Medium	<br/>
877	Stone Game		Medium	<br/>
even number of piles, total number is odd. taken from either side.
greedy: assuming odd index sum is larger, then we can always choose odd. otherwise always choose even
dp: more general, it does not require n to be even and total to be odd.
max(A[l]-dp[l+1,r],A[r]-dp[l,r-1])

931	Minimum Falling Path Sum		Medium	<br/>
column can only differ from previous row by 1: -1,0,1
dp[i,j]=min(dp[i-1,j-1],dp[i-1,j],dp[i-1,j+1])+M(i,j)

983	Minimum Cost For Tickets		Medium	<br/>
1 day tickets
7 day tickets
30 day tickets
dp[i]=min(dp[i-1]+1day,dp[j]+7day, dp[k]+30day)

647	Palindromic Substrings		Medium	<br/>
count how many pal-substring
dp[i,j] is the number of pal substring in s[i..j]
dp[i,j]=self+dp[i,j-1]+dp[i+1,j]-dp[i+1,j-1]
i dimension depends on i+1
j dimension depends on j-1
so i need reverse loop, and j need forward loop

413	Arithmetic Slices		Medium	<br/>
this needs subarray.
to get a slice: need A[i-2],A[i-1],A[i]
dp[i]=dp[i-1]+1
for example 1,3,5,7,9
its dp value are: 0,0,1,2,3,and the total is 6.

712	Minimum ASCII Delete Sum for Two Strings		Medium	<br/>
make the two strings equal, so two strings both allow deletion.
dp[i,j] is the min ascii deletion sum
s[i-1]==t[j-1]: match dp[i-1,j-1]
delete s[i-1]: dp[i-1,j]+s[i-1]
delete t[j-1]: dp[i,j-1]+t[j-1]
delete both: dp[i-1,j-1]+s[i-1]+t[j-1]
dp[i,j]=min(match, min(del1,del2)) when match
dp[i,j]=min(delboth,min(del1,del2)) when mismatch

651	4 Keys Keyboard 	Medium	<br/>
714	Best Time to Buy and Sell Stock with Transaction Fee		Medium	<br/>
as many as transaction can be made.
dp[i]=max(dp[i-1],dp[j]+price[i]-price[j]-fee)

646	Maximum Length of Pair Chain		Medium	<br/>
similar to longest increasing sequence

638	Shopping Offers		Medium	<br/>
knapsack with several number binding
- backtracking using try and back

343	Integer Break		Medium	<br/>
- math solution: always break into 2 and 3, 3 first unless we reach 4
- dp solution: j*(i-j) vs j*dp[i-j].
dp[i]=max(dp[i],j*(i-j),j*dp[i-j])
note only j*dp[i-j] is not sufficient.

62	Unique Paths		Medium	<br/>
climbing stairs in 2d

1024 Video Stitching		Medium	<br/>
min number of clips to cover from t0 to t1
bfs like method
- sort the clips 
1027 longest arithmetic sequence
- ending with i with different difference vector<unodered_map<int,int>>
- longest sequence

357	Count Numbers with Unique Digits		Medium	<br/>
math using combinations

64	Minimum Path Sum		Medium	<br/>
from top left to bottom right
dp[i,j]=min(dp[i-1,j],dp[i,j-1])+M(i,j)

486	Predict the Winner		Medium	<br/>
same as stone problem.
choose from either end.

392	Is Subsequence		Medium	<br/>
- two pointer
- edit distance 

516	Longest Palindromic Subsequence		Medium	<br/>
if s[i]==s[j], then dp[i,j]=dp[i+1,j-1]+2
else dp[i,j]=max(dp[i+1,j],dp[i,j-1])
can reduce to 1d dp

650	2 Keys Keyboard		Medium	<br/>
copy all
paste
intial has one A.
min number of steps to reach n.
reverse: from n to 1. 
- if n is odd, we find its max divisor and paste i/m times. dp[i]=dp[m]+i/m; (paste i/m-1 times and one copy)
- if n is even, it needs one copy and odd number of paste.
for example 8: 
method 1: use 4, we need 1 copy 1 paste. (4: from 2 need 2 operations,, 2 need again 2 operations), so total is 6.
method 2; use 2, one copy, paste 3 times, it is 4, 2 needs 2 operations, total is also 6.

actually one the first is sufficent. find the max factor and use dp[i]=dp[m]+i/m

1027 Longest Arithmetic Sequence		Medium	<br/>

96	Unique Binary Search Trees		Medium	<br/>
- use any number as the root, divide by left and right
- left is a subproblem and right is another subproblem
- dp[i]+=dp[j]*dp[i-j]

873	Length of Longest Fibonacci Subsequence		Medium	<br/>
a[i]=a[i-1]+a[i-2] or equivalently a[i]-a[i-1]=a[i-2], difference is another element seen before
- any two elements can determine the whole fib sequence
- a[i], a[j] and a[k] forms a sequence then dp[i]=dp[k]+2

1048 Longest String Chain		Medium	<br/>
718	Maximum Length of Repeated Subarray		Medium	<br/>
longest common subarray
- slding window which is O((m+n)*n)
- dp: dp[i][j] represents the max common length for A[0...i-1] with B[0..j-1]
dp[i,j]=dp[i-1][j-1]+1 if A[i-1]==B[j-1]
dp[i,j]=max(dp[i-1][j],dp[i][j-1]) if A[i-1]!=B[j-1] delete one of it 
//note: for A[i-1]!=B[j-]1 dp[i,j]=0. above dp is not for subarray but for subsequence.

740	Delete and Earn		Medium	<br/>
if you earn nums[i] then you need delete all elements equal to nums[i]+1 and nums[i]-1
- this is house robber problem, you have to skip one
- bucket sort to a dense bin.
- dp[i]=max(cnt[i]*nums[i]+dp[i-2],dp[i-1])

351	Android Unlock Patterns 	Medium	<br/>
978	Longest Turbulent Subarray		Medium	<br/>
sign changes between neighboring elements
- one pass to get the sign change, when sign the same, restart a new segment

494	Target Sum		Medium	<br/>
given an array of non-negative numbers, apply +/- to each of it, adds up to the given target.
return the number of combinations.
this is a partition problem using knapsack

- pos-neg=target, pos+neg=total, so 2*pos=total+target, pos=(total+target)/2
- problem is equivalent to: pick a combination to get the new target sum
- typical knapsack dp[w]+=dp[w-wj]
- each element can only be used once. so loop on the element shall be outside.

813	Largest Sum of Averages		Medium	<br/>
partition the array <=k groups. making the sum of average the largest.
similar to the k transaction of the stock problem, we will try to add a new group on k-1:
dp[k][i]: largest sum of average for k groups with array length=i.
dp[k][i]=max(dp[k][i-1],dp[k-1][j]+sum(j to i)/(j-i)),

688	Knight Probability in Chessboard		Medium	<br/>
at a position i, j, jump k moves, what is the possibility in the chessboard
typical dfs with memoization
if anytime goes out of the chessboard, prob=0
if k==0 and still in chessboard, prob=1
every position has 1/8 probability.

309	Best Time to Buy and Sell Stock with Cooldown		Medium	<br/>
one cooldown day, as many transactions.
on the ith day: 
no operation: dp[i-1]
sell on ith day: dp[j]+price[i]-price[j] j shall be at least two days ago.

377	Combination Sum IV		Medium	<br/>
number of combination which sums to target.
each element can be used multiple time. (coin change problem)
knapsack with repetion
loop on elements shall be inside.

838	Push Dominoes		Medium	<br/>
greedy solution
- to avoid boundary we add a left L and right R.
- 'R...R'->'RRRRR'
'R....L'->'RR.LL' or RRRLLL
'L....R' no changes
'L....L'->'LLLLLL'

361	Bomb Enemy 	Medium	<br/>
764	Largest Plus Sign		Medium	<br/>
a matrix with 0/1 only. find the largest plus sign.
brutal force: for each 1, grows 4 direction and find the largest radius. O(N^3)
optimized: very tricky to reduce one order. but it is a loop to do 4 directions.

698	Partition to K Equal Sum Subsets		Medium	<br/>
check if we can partition into k equal subsets
method 1: backtracking 
method 2: dynamic is not suitable for this (it is not knapsack)

279	Perfect Squares		Medium	<br/>
given n, return min number of number square's used so that n=sum of(square)
knapsack with repetion
dp[w]=min(dp[w],dp[w-i*i]+1)

300	Longest Increasing Subsequence		Medium	<br/>
dp[i] is the length ending at i. 
dp[i]=max(dp[i],dp[j]+1) if A[j]<A[i]

416	Partition Equal Subset Sum		Medium	<br/>
knapsack target=tsum/2

935	Knight Dialer		Medium	<br/>
need return the number of path using N digits.
- build adj matrix for each number 0 to 9
- can use dp or dfs
- dp[k][i]: k represents number of steps, i, represents the digits
- one step move is a climbing stairs problem using addition.

1039 Minimum Score Triangulation of Polygon		Medium	<br/>
dp[i,j] repreents the min score to triangulate A[i] to A[j]. we use the inner side vertices to minimize the score
dp[i,j]=min(dp[i,j],dp[i,k]+A[i}*A[j]*A[k]+dp[k,j]) divide into two poly and one triangle.

474	Ones and Zeroes		Medium	<br/>
given m 0s and n 1s, get the max number of strings in dictionary. we need exact match
knapsack with more complexity:
-. calculate each string's 0s and 1s.
-. choose a string or not choose a string, 
- we have three variables string number, m and n.
- knapsack to solve 1 str, 2, str,....k strs. dp[k][m][n] is the answer
dp[k][i][j]=dp[k-1][i][j],dp[k-1][i-m0][j-m1]+1)

120	Triangle		Medium	<br/>
min path sum in triangle, using reverse dp

375	Guess Number Higher or Lower II		Medium	<br/>
when you guess x (1 to n), if wrong you have to pay x. min cost to ensure win.
we guess x, leaves two subproblem left to x-1 and x+1 to right.
we choose the max x+max(dp[l,x-1),dp(x+1,r))
results are the min of all the max.

376	Wiggle Subsequence		Medium	<br/>
if going up A[i]>A[j] up[i]=dn[j]+1

808	Soup Servings		Medium	<br/>
top down recursive with memoization

967	Numbers With Same Consecutive Differences		Medium	<br/>
neighboring digits has the same absolute difference.
return all the combinations. given n is the length and k is difference
typical backtracking problem.

1049 Last Stone Weight II		Medium	<br/>
equivalent to knapsack problem
need add + and - to any number so that the sum is largest.

264	Ugly Number II		Medium	<br/>
ugly number only have 2,3,5 factors. find the nth ugly number
- 2,3,5 and then use *2, *3, *5 choose the min as the next

95	Unique Binary Search Trees II		Medium	<br/>
need return all the trees.
recursive generate left tree and right tree
then combine left and right to get the list of trees

790	Domino and Tromino Tiling		Medium	<br/>
two types of shape, see other details

139	Word Break		Medium	<br/>
break the string into dictionary words. just check if it can break or not
s[i...j] is in the dictionary && dp[j] then dp[i] is true

213	House Robber II		Medium	<br/>
circular array: two house robber I problem

368	Largest Divisible Subset		Medium	<br/>
dp with traceback.
store the parent information so that we can trace back the path.
this is regular practice.

787	Cheapest Flights Within K Stops		Medium	<br/>
dp[i,j] is the cheapest from i to j.
add a stop k to see if can be cheaper

dp[i,j,k]=min(dp[i,j,k-1],dp[i,x,k-1]+prices[k,j] //try all direct flights.

801	Minimum Swaps To Make Sequences Increasing		Medium	<br/>
two arrays, swap at the same position to make both array sorted<br/>
two cases:<br/>
- A[i]>A[i-1] && B[i]>B[i-1]: no swap, then i-1 no swap, swap then swap i-1
- A[i]>B[i-1] && B[i]>A[i-1]: no swap, then swap i-1. swap i, then no swap i-1
- Note: the two cases may overlap, so second case we need take the min

467	Unique Substrings in Wraparound String		Medium	<br/>
- ending with different char guarantee the uniqueness.
- ending with a char, with a length, the substring is fixed (since it is always abcde...zabcd...z

898	Bitwise ORs of Subarrays		Medium	<br/>
for all subarray, bit or of all elements, return number of possible results<br/>
for input ABC<br/>
A<br/>
A|B, B<br/>
A|B|C, B|C,C<br/>
so we take two set, one for final one for intermediate<br/>


63	Unique Paths II		Medium	<br/>
with obstacles

673	Number of Longest Increasing Subsequence		Medium	<br/>
first find the longest increasing subsequence
and find the cnt. (climbing stairs)

221	Maximal Square		Medium	<br/>
- see stack
- can also use dp
finding the max square with all 1s<br/>
similar to histogram but we uses dp<br/>
keep updating the height on each row.<br/>
keep updating its max range at j (left and right) using the height as the min height (this could be obtained using left max and right min dp process)<br/>

304	Range Sum Query 2D - Immutable		Medium	<br/>
return the submatrix sum.
we only need store the prefix sum from top left to (i,j).
submatrice can be obtained easily

576	Out of Boundary Paths		Medium	<br/>
n steps, number of paths out of boundary
dfs with memoization.

837	New 21 Game		Medium	<br/>
this involves some problems:
sliding window
probability
climbing stairs with k steps.

418	Sentence Screen Fitting 	Medium	<br/>
322	Coin Change		Medium	<br/>
kanpsack problem with repetition

152	Maximum Product Subarray		Medium	<br/>
has negatives, keep max and min

5	Longest Palindromic Substring		Medium	<br/>
- brutal force: at each position grows and get the length
- dp: if s[i]==s[j] dp[i,j]=dp[i+1,j-1]+2
need record the start position.

464	Can I Win		Medium	<br/>
given 1 to N to choose, and a target, who first reaches the target wins
backtrace: it is hard
- use bits to indicate a number used or not
- save win or lose status in memoization (hashset)

523	Continuous Subarray Sum		Medium	<br/>
target is multiples of K.
with the same %k value. 
k=0 or k!=0 shall be treated separatly.

91	Decode Ways		Medium	<br/>
dp[i]=a*dp[i-1]+b*dp[i-2]
either the digit itself or combine with previous digit

982	Triples with Bitwise AND Equal To Zero		Hard	<br/>
960	Delete Columns to Make Sorted III		Hard	<br/>
975	Odd Even Jump		Hard	<br/>
312	Burst Balloons		Hard	<br/>
847	Shortest Path Visiting All Nodes		Hard	<br/>
471	Encode String with Shortest Length 	Hard	<br/>
689	Maximum Sum of 3 Non-Overlapping Subarrays		Hard	<br/>
920	Number of Music Playlists		Hard	<br/>
903	Valid Permutations for DI Sequence		Hard	<br/>
410	Split Array Largest Sum		Hard	<br/>
265	Paint House II 	Hard	<br/>
514	Freedom Trail		Hard	<br/>
964	Least Operators to Express Number		Hard	<br/>
940	Distinct Subsequences II		Hard	<br/>
730	Count Different Palindromic Subsequences		Hard	<br/>
546	Remove Boxes		Hard	<br/>
568	Maximum Vacation Days 	Hard	<br/>
691	Stickers to Spell Word		Hard	<br/>
72	Edit Distance		Hard	<br/>
943	Find the Shortest Superstring		Hard	<br/>
956	Tallest Billboard		Hard	<br/>
727	Minimum Window Subsequence 	Hard	<br/>
517	Super Washing Machines		Hard	<br/>
664	Strange Printer		Hard	<br/>
879	Profitable Schemes		Hard	<br/>
403	Frog Jump		Hard	<br/>
968	Binary Tree Cameras		Hard	<br/>
115	Distinct Subsequences		Hard	<br/>
363	Max Sum of Rectangle No Larger Than K		Hard	<br/>
818	Race Car		Hard	<br/>
472	Concatenated Words		Hard	<br/>
1012 Numbers With Repeated Digits		Hard	<br/>
354	Russian Doll Envelopes		Hard	<br/>
123	Best Time to Buy and Sell Stock III		Hard	<br/>
85	Maximal Rectangle		Hard	<br/>
552	Student Attendance Record II		Hard	<br/>
600	Non-negative Integers without Consecutive Ones		Hard	<br/>
87	Scramble String		Hard	<br/>
1000 Minimum Cost to Merge Stones		Hard	<br/>
446	Arithmetic Slices II - Subsequence		Hard	<br/>
741	Cherry Pickup		Hard	<br/>
629	K Inverse Pairs Array		Hard	<br/>
871	Minimum Number of Refueling Stops		Hard	<br/>
902	Numbers At Most N Given Digit Set		Hard	<br/>
97	Interleaving String		Hard	<br/>
132	Palindrome Partitioning II		Hard	<br/>
466	Count The Repetitions		Hard	<br/>
140	Word Break II		Hard	<br/>
174	Dungeon Game		Hard	<br/>
656	Coin Path 	Hard	<br/>
188	Best Time to Buy and Sell Stock IV		Hard	<br/>
32	Longest Valid Parentheses		Hard	<br/>
321	Create Maximum Number		Hard	<br/>
10	Regular Expression Matching		Hard	<br/>
639	Decode Ways II		Hard	<br/>
887	Super Egg Drop		Hard	<br/>
44	Wildcard Matching		Hard<br/>



# sliding window
# recursion


# math
728	Self Dividing Numbers	70.4%	Easy	
number is divisible by all of its digit

942	DI String Match	70.1%	Easy	
greedy, just use two pointer to match

883	Projection Area of 3D Shapes	65.7%	Easy	
get the total area in xy, yz, xz planes.
a bit tricky

908	Smallest Range I	64.5%	Easy	
add an x in [-k,k] for each element, return the smallest possible max-min
it only matters the max and min. and we want to reduce the diff
if the diff>2*k, we can subtract 2*k
otherwise we can make the diff 0

1025 Divisor Game	63.2%	Easy	
greedy: even wins

800	Similar RGB Color 59.7%	Easy	
868	Binary Gap	59.5%	Easy	
1009 Complement of Base 10 Integer	58.9%	Easy	
976	Largest Perimeter Triangle	57.3%	Easy	
812	Largest Triangle Area	56.2%	Easy	
892	Surface Area of 3D Shapes	56.0%	Easy	
258	Add Digits	54.2%	Easy	
13	Roman to Integer	52.6%	Easy	
1056 Confusing Number 52.0%	Easy	
171	Excel Sheet Column Number	51.7%	Easy	
453	Minimum Moves to Equal Array Elements	49.2%	Easy	
598	Range Addition II	48.7%	Easy	
268	Missing Number	48.6%	Easy	
836	Rectangle Overlap	46.6%	Easy	
628	Maximum Product of Three Numbers	46.1%	Easy	
202	Happy Number	45.5%	Easy	
9	Palindrome Number	43.6%	Easy	
1041 Robot Bounded In Circle	43.1%	Easy	
246	Strobogrammatic Number 42.4%	Easy	
231	Power of Two	42.0%	Easy	
326	Power of Three	41.7%	Easy	
645	Set Mismatch	40.8%	Easy	
263	Ugly Number	40.7%	Easy	
367	Valid Perfect Square	39.9%	Easy	
67	Add Binary	39.4%	Easy	
970	Powerful Integers	39.4%	Easy	
441	Arranging Coins	38.0%	Easy	
1037 Valid Boomerang	37.6%	Easy	
172	Factorial Trailing Zeroes	37.4%	Easy	
507	Perfect Number	34.4%	Easy	
914	X of a Kind in a Deck of Cards	34.1%	Easy	
949	Largest Time for Given Digits	34.1%	Easy	
633	Sum of Square Numbers	32.7%	Easy	
754	Reach a Number	32.7%	Easy	
69	Sqrt(x)	31.5%	Easy	
400	Nth Digit	30.4%	Easy	
168	Excel Sheet Column Title	29.1%	Easy	
204	Count Primes	29.1%	Easy	
7	Reverse Integer	25.3%	Easy	
535	Encode and Decode TinyURL	76.9%	Medium	
537	Complex Number Multiplication	65.6%	Medium	
885	Spiral Matrix III	64.2%	Medium	
877	Stone Game	61.4%	Medium	
1017 Convert to Base -2	56.6%	Medium	
413	Arithmetic Slices	55.9%	Medium	
553	Optimal Division	55.4%	Medium	
789	Escape The Ghosts	55.4%	Medium	
573	Squirrel Simulation 53.8%	Medium	
1006 Clumsy Factorial	53.7%	Medium	
462	Minimum Moves to Equal Array Elements II	52.4%	Medium	
858	Mirror Reflection	52.0%	Medium	
781	Rabbits in Forest	51.9%	Medium	
12	Integer to Roman	51.2%	Medium	
869	Reordered Power of 2	51.1%	Medium	
651	4 Keys Keyboard 50.7%	Medium	
672	Bulb Switcher II	50.1%	Medium	
343	Integer Break	47.9%	Medium	
592	Fraction Addition and Subtraction	47.2%	Medium	
357	Count Numbers with Unique Digits	47.0%	Medium	
360	Sort Transformed Array 46.8%	Medium	
423	Reconstruct Original Digits from English	45.7%	Medium	
963	Minimum Area Rectangle II	44.6%	Medium	
247	Strobogrammatic Number II 44.5%	Medium	
319	Bulb Switcher	44.0%	Medium	
279	Perfect Squares	42.2%	Medium	
313	Super Ugly Number	41.6%	Medium	
593	Valid Square	40.6%	Medium	
640	Solve the Equation	40.4%	Medium	
991	Broken Calculator	40.4%	Medium	
1058 Minimize Rounding Error to Meet Target 39.9%	Medium	
670	Maximum Swap	39.7%	Medium	
775	Global and Local Inversions	39.1%	Medium	
634	Find the Derangement of An Array 37.7%	Medium	
478	Generate Random Point in a Circle	36.8%	Medium	
264	Ugly Number II	36.6%	Medium	
223	Rectangle Area	35.9%	Medium	
372	Super Pow	35.6%	Medium	
469	Convex Polygon 35.5%	Medium	
396	Rotate Function	35.2%	Medium	
368	Largest Divisible Subset	34.9%	Medium	
60	Permutation Sequence	33.3%	Medium	
625	Minimum Factorization 32.0%	Medium	
397	Integer Replacement	31.5%	Medium	
1073 Adding Two Negabinary Numbers	31.5%	Medium	
2	Add Two Numbers	31.2%	Medium	
43	Multiply Strings	30.9%	Medium	
356	Line Reflection 30.9%	Medium	
794	Valid Tic-Tac-Toe State	29.6%	Medium	
365	Water and Jug Problem	29.0%	Medium	
1015 Smallest Integer Divisible by K	28.3%	Medium	
50	Pow(x, n)	28.1%	Medium	
523	Continuous Subarray Sum	24.2%	Medium	
910	Smallest Range II	23.7%	Medium	
866	Prime Palindrome	20.2%	Medium	
166	Fraction to Recurring Decimal	19.5%	Medium	
29	Divide Two Integers	16.2%	Medium	
8	String to Integer (atoi)	14.7%	Medium	
296	Best Meeting Point 55.0%	Hard	
660	Remove 9 51.5%	Hard	
996	Number of Squareful Arrays	47.6%	Hard	
899	Orderly Queue	47.3%	Hard	
753	Cracking the Safe	46.6%	Hard	
458	Poor Pigs	45.4%	Hard	
810	Chalkboard XOR Game	45.1%	Hard	
964	Least Operators to Express Number	40.7%	Hard	
972	Equal Rational Numbers	40.1%	Hard	
782	Transform to Chessboard	39.8%	Hard	
517	Super Washing Machines	36.9%	Hard	
248	Strobogrammatic Number III 36.6%	Hard	
1012	Numbers With Repeated Digits	34.8%	Hard	
483	Smallest Good Base	34.2%	Hard	
1067 Digit Count in Range 34.2%	Hard	
829	Consecutive Numbers Sum	33.4%	Hard	
1088 Confusing Number II 33.1%	Hard	
224	Basic Calculator	32.9%	Hard	
906	Super Palindromes	30.4%	Hard	
927	Three Equal Parts	30.4%	Hard	
233	Number of Digit One	30.3%	Hard	
891	Sum of Subsequence Widths	29.1%	Hard	
902	Numbers At Most N Given Digit Set	28.6%	Hard	
780	Reaching Points	27.5%	Hard	
335	Self Crossing	27.0%	Hard	
952	Largest Component Size by Common Factor	26.4%	Hard	
878	Nth Magical Number	25.6%	Hard	
887	Super Egg Drop	24.9%	Hard	
273	Integer to English Words	24.4%	Hard	
805	Split Array With Same Average	24.4%	Hard	
149	Max Points on a Line	15.8%	Hard	
65	Valid Number	14.0%	Hard	

