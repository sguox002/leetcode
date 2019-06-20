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
left+A[i]+right=tsum
left+right=tsum-A[i]

35	Search Insert Position		Easy	<br/>
849	Maximize Distance to Closest Person		Easy	<br/>
26	Remove Duplicates from Sorted Array		Easy	<br/>
747	Largest Number At Least Twice of Others		Easy	<br/>
643	Maximum Average Subarray I		Easy	<br/>
624	Maximum Distance in 	Easy	<br/>
88	Merge Sorted Array		Easy	<br/>
840	Magic Squares In Grid		Easy	<br/>
219	Contains Duplicate II		Easy	<br/>
941	Valid Mountain Array		Easy	<br/>
914	X of a Kind in a Deck of Cards		Easy	<br/>
605	Can Place Flowers		Easy	<br/>
581	Shortest Unsorted Continuous Subarray		Easy	<br/>
189	Rotate Array		Easy	<br/>
532	K-diff Pairs in an Array		Easy	<br/>
414	Third Maximum Number		Easy	<br/>
665	Non-decreasing Array		Easy	<br/>
<br/>
950	Reveal Cards In Increasing Order		Medium	<br/>
723	Candy Crush 	Medium	<br/>
969	Pancake Sorting		Medium	<br/>
280	Wiggle Sort 	Medium	<br/>
442	Find All Duplicates in an Array		Medium	<br/>
370	Range Addition 	Medium	<br/>
531	Lonely Pixel I 	Medium	<br/>
695	Max Area of Island		Medium	<br/>
1031 Maximum Sum of Two Non-Overlapping Subarrays		Medium	<br/>
238	Product of Array Except Self		Medium	<br/>
245	Shortest Word Distance III 	Medium	<br/>
78	Subsets		Medium	<br/>
565	Array Nesting		Medium	<br/>
1011 Capacity To Ship Packages Within D Days		Medium	<br/>
495	Teemo Attacking		Medium	<br/>
835	Image Overlap		Medium	<br/>
667	Beautiful Arrangement II		Medium	<br/>
769	Max Chunks To Make Sorted		Medium	<br/>
216	Combination Sum III		Medium	<br/>
1035 Uncrossed Lines		Medium	<br/>
714	Best Time to Buy and Sell Stock with Transaction Fee		Medium	<br/>
900	RLE Iterator		Medium	<br/>
287	Find the Duplicate Number		Medium	<br/>
926	Flip String to Monotone Increasing		Medium	<br/>
39	Combination Sum		Medium	<br/>
48	Rotate Image		Medium	<br/>
1014 Best Sightseeing Pair		Medium	<br/>
62	Unique Paths		Medium	<br/>
729	My Calendar I		Medium	<br/>
1007 Minimum Domino Rotations For Equal Row		Medium	<br/>
64	Minimum Path Sum		Medium	<br/>
59	Spiral Matrix II		Medium	<br/>
533	Lonely Pixel II 	Medium	<br/>
873	Length of Longest Fibonacci Subsequence		Medium	<br/>
718	Maximum Length of Repeated Subarray		Medium	<br/>
978	Longest Turbulent Subarray		Medium	<br/>
16	3Sum Closest		Medium	<br/>
621	Task Scheduler		Medium	<br/>
289	Game of Life		Medium	<br/>
611	Valid Triangle Number		Medium	<br/>
1040 Moving Stones Until Consecutive II		Medium	<br/>
259	3Sum Smaller 	Medium	<br/>
11	Container With Most Water		Medium	<br/>
974	Subarray Sums Divisible by K		Medium	<br/>
562	Longest Line of Consecutive One in Matrix 	Medium	<br/>
548	Split Array with Equal Sum 	Medium	<br/>
795	Number of Subarrays with Bounded Maximum		Medium	<br/>
915	Partition Array into Disjoint Intervals		Medium	<br/>
153	Find Minimum in Rotated Sorted Array		Medium	<br/>
380	Insert Delete GetRandom O(1)		Medium	<br/>
945	Minimum Increment to Make Array Unique		Medium	<br/>
792	Number of Matching Subsequences		Medium	<br/>
870	Advantage Shuffle		Medium	<br/>
90	Subsets II		Medium	<br/>
560	Subarray Sum Equals K		Medium	<br/>
75	Sort Colors		Medium	<br/>
40	Combination Sum II		Medium	<br/>
162	Find Peak Element		Medium	<br/>
962	Maximum Width Ramp		Medium	<br/>
105	Construct Binary Tree from Preorder and Inorder Traversal		Medium	<br/>
80	Remove Duplicates from Sorted Array II		Medium	<br/>
755	Pour Water 	Medium	<br/>
73	Set Matrix Zeroes		Medium	<br/>
670	Maximum Swap		Medium	<br/>
120	Triangle		Medium	<br/>
106	Construct Binary Tree from Inorder and Postorder Traversal		Medium	<br/>
775	Global and Local Inversions		Medium	<br/>
277	Find the Celebrity 	Medium	<br/>
713	Subarray Product Less Than K		Medium	<br/>
825	Friends Of Appropriate Ages		Medium	<br/>
228	Summary Ranges		Medium	<br/>
56	Merge Intervals		Medium	<br/>
74	Search a 2D Matrix		Medium	<br/>
209	Minimum Size Subarray Sum		Medium	<br/>
954	Array of Doubled Pairs		Medium	<br/>
34	Find First and Last Position of Element in Sorted Array		Medium	<br/>
63	Unique Paths II		Medium	<br/>
33	Search in Rotated Sorted Array		Medium	<br/>
81	Search in Rotated Sorted Array II		Medium	<br/>
229	Majority Element II		Medium	<br/>
55	Jump Game		Medium	<br/>
918	Maximum Sum Circular Subarray		Medium	<br/>
79	Word Search		Medium	<br/>
18	4Sum		Medium	<br/>
31	Next Permutation		Medium	<br/>
54	Spiral Matrix		Medium	<br/>
152	Maximum Product Subarray		Medium	<br/>
457	Circular Array Loop		Medium	<br/>
907	Sum of Subarray Minimums		Medium	<br/>
15	3Sum		Medium	<br/>
163	Missing Ranges 	Medium	<br/>
<br/>
<br/>
768	Max Chunks To Make Sorted II		Hard	<br/>
689	Maximum Sum of 3 Non-Overlapping Subarrays		Hard	<br/>
42	Trapping Rain Water		Hard	<br/>
128	Longest Consecutive Sequence		Hard	<br/>
782	Transform to Chessboard		Hard	<br/>
154	Find Minimum in Rotated Sorted Array II		Hard	<br/>
123	Best Time to Buy and Sell Stock III		Hard	<br/>
85	Maximal Rectangle		Hard	<br/>
381	Insert Delete GetRandom O(1) - Duplicates allowed		Hard	<br/>
57	Insert Interval		Hard	<br/>
84	Largest Rectangle in Histogram		Hard	<br/>
719	Find K-th Smallest Pair Distance		Hard	<br/>
41	First Missing Positive		Hard	<br/>
891	Sum of Subsequence Widths		Hard	<br/>
644	Maximum Average Subarray II 	Hard	<br/>
45	Jump Game II		Hard	<br/>
4	Median of Two Sorted Arrays		Hard	<br/>
126	Word Ladder II		Hard<br/>
