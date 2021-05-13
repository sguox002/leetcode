fb coding interview

953. verifying an alien dictionary
convert to sorted order using map

1249. min remove to make valid parenthesis
using stack to eliminate paired.
delete chars from right.

1428. leftmost column with at least a one.
row sorted binary matrix. rows are sorted. so using a pointer.

973. k closest points to origin
pq.

680. valid palindrome II
delete <=1 chars to form palindrome. 
counting odd/even.

1570. dot product of two sparse vectors
hashmap to store the non-zero elements

238. Product of array except self
left and right prod[i]=left(i-1)*right(i+1 to n)

415. add strings
two pointers, reverse strings

560. subarray sum equals k.
hashmap + prefix sum from left to right, and accumulate the map.

211. design add and search words data structure
Trie.

67. Add binary

528. Random Pick with weight
prefix sum to make the array mono-increasing and then rand()%total

124. Binary tree max path sum.
get the left and right max path sum and check if can connect left and right.

269. Alien dictionary
build the graph relation and bfs to get rid layer by layer.

1762. Buildings with an ocean view.
simple get rmax and compare.

56. merge intervals
merge a list of intervals, sort by start and use rmax to merge.

938. range sum of bst
recursive.

173. binary search tree iterator
inorder traversal using stack. (iterative approach)

635. exclusive time of functions ***
stack. end to pop, need to subtract the elasped time. (add an etime to record it)

827. making a large island ***
change <=1 0 to 1. 
union find. and then try each 0 to see if it can connect more.

523. Continuous subarray sum
subarray>=2, sum is multiple of K.
get the prefix sum num[i]%K and apply hashmap
k=0 and k!=0 two cases.

426. convert binary search tree to sorted doubly linked list
use a dummy and prev and do inorder traversal

301. remove invalid parenthesis
min removals. return all possible answers.
bfs to get the min removal. Once we get the first answer, do not remove any more parenthesis

199. binary tree right side view
using an 1d vector to store each layer's rightmost elements

1650 lowest common ancestor of a binary tree III
tree node has a reference to parent. just use the definition to find parents.


273. Integer to english words
uses thousands 

215. Kth largest element in an array
simple pq.

708. Insert into a sorted circular linked list. ***
some edge case: all the same, and insert the same value.

65. valid number
a lot of edge cases.
using find_first_of, find_first_not_of...
exp must be int, the base shall be valid real number.

125. valid palindrome
278. first bad version- binary search
297. serialize and deserialize binary tree ***
using string/stringstream

987. vertical order traversal of a binary tree
270. closest binary search tree value
find the node closest to target.

986. Interval List Intersections
two pointer merge

721 account merge
union find

249. group shifted strings
hashmap, using the char difference

158. read n characters givng read4 II- call multiple times
using deque to store read outs

1539. kth missing positive number
binary search. it is a bit tricky

543. Diameter of binary tree
get left and right depth and max diameter at the same time.

621. Task scheduler
same task need have n cooldown periods. mx*(n-1) vs the task length.

42. trapping rain water
water is trapped by inner lower and left right taller so use stack to find next and previous larger.

1382. Balance a binary search tree
traverse and build

29 divide two integers
using subtract. and binary expansion.
a lot of edge cases.

282. expression add operators ***
use +-* on numbers and get the result=target.
backtracking: 
- from size 1 to n try each prefix as the first number
- skip leading zeros
- apply different operators 

298. random pick index
array with duplicates, randomly output the index of a given number.
(duplex shall have the same probability)
[1,2,3,3,3]
target 3: choose 2,3,4
choose 2: 1*1/2*2/3=1/3
choose 3: 1/2*2/3=1/3
choose 4: 1/3 no-select:(1-1/3)

reservoir sampling

863. all nodes distance K in binary tree.
convert tree to graph and then use bfs.

140 word break II.
string s and a list of dictionary words, return all possible combinations.
use dp to find all possible segmentation and then use dfs to get all combinations.

236. Lowest common ancestor in binary tree.
recursive

71. simplify path
using stack

670. max swap
swap two digits at most once to get the max number.
from left to right, compare current digits with its right max.

317. shortest distance from all buildings
grid: 0 empty, 1, building, 2: obstacle.
find an empty land that reaches all buildings in the shortest distance.
- each building do a bfs to get the shortest distance to each empty land, non-reachable is removed
- get the shortest distance (by adding them altogether)

266. Palindrome permutation
check if permutation can be palindrome. 

139. word break
dp

50. pow(x,n)
binary power

88. merge sorted array
31. next permutation
next greater permutation
greedy: 
- from right to left, find the first sorted pair position
- on the right, find the smallest one > the position element
- swap them and reverse the right.

133. Clone graph
node vs node hashmap

24 longest substring with at most K distinct characters
sliding window to find the max window.

921. Min Add to make parentheses valid.
save invalid into stack

76. Min window substring
sliding window (window shall contain all the given chars)
using cnt as hashmap

536. construct binary tree from string
the string uses () to include the subnodes
recursive. syntax parsing using stack.

146. LRU cache.
hashed linked list 

339. nested list weight sum
dfs to get the depth.

496. diagnal traverse
dealing with all boundary and keep dir changes

314. binary tree vertical order traversal

203. remove linked list elements


























