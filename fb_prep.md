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














