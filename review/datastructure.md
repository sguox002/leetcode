# C++ data structure

## vector
-. automatic growing. it grows or shrink by a factor of two
	- cannot rely on its position (pointers)
	- adding to back is amortized O(1)
	- insert is O(n) (has to shift down)
	- delete is O(n) (has to shift up)
-. push_back is O(1)
-. random access is O(1)
constructor:
	{} to enclose a list of elements
	another vector using itertor start and end
iterator: begin, end, rbegin,rend
push_back/pop_back
swap
assign
insert/erase
clear
emplace
empty

1d/2d/3d
does not have to be the same size. it supports much more flexible data structure than the regular c array

other sequential container
stack, queue, deque are generally used for 1d sequential container
## stack: 
only operates on the back, sometimes, string or vector can act as stack. but using stack is more clear.
FILO
top
push/pop
clear
empty

## queue:
push in the back, remove in the front
FIFO
size
empty
front()
back()
push
pop

## deque:
double ended queue. can add/remove from both ends.
## deque is more similar to vector, but it is segment contiguous in memory
begin/end
rbegin/rend
size/empty/resize
[]/at
front/back
assign
push_back/push_front
pop_back/pop_front
insert/erase
swap/clear
deque can be considered as a list of vectors. 

## list
c++ only supports doubly linked list.
list: memory flexible
acess front and back in O(1)
insert/delete is O(1)
find is O(n)
combined with iterator can access the list node in O(1)
begin/end
rbegin/rend
empty/size
front/back
assign
push_front/pop_front
push_back/pop_back
insert/erase
swap
clear

list related operations:
splice: connect two lists
remove: remove elements with given value
remove_if: remove elements with condition
unique: remove duplicate values
merge: merge sorted list
sort: sort elements in container
reverse: reverse the order of elements

actually we can make a library to work with singly linked list too.

## hashset: unordered_set/unordered_multiset
hashset using hashvalue for direct access
collision: using list for each node to connect the keys with same hash value
- access/insert/delete is in average O(1)
- not all types are supported since hash is not easily found
- allows or allows not duplicates
empty/size
begin/end
find/count
equal_range: get range of elements with a specific keys
insert/erase
clear
swap
emplace
bucket_count
bucket_size

## hashmap: unordered_map/unordered_multimap
similar to hash set, but have pairs---dictionary.

non-sequential containers
## Heap:
set/multiset: minheap default, can support maxheap
map/multimap: minheap default, can support maxheap
priority_queue: maxheap default, can support minheap (but its function is very limited)

above containers all sort the elements in some efficient way.
set: use binary tree as the heap
maxheap:
it is a binary tree, root is always the largest one among its two children, recursively.
access the root: O(1)
add a node: attach it to any leaf and sift up, O(logn)
pop the root: replace the root with any leaf and sift down O(logn)
change priority: change and sift up/down O(logn)
remove: change the priority to inf and pop the root. O(logn)

Priority_queue is a special heap using a complete binary tree in array format.
a complete binary tree:
-. depth at most logn
-. can store as array, the parent-child relation is defined by the index
parent index=i, its child 2*i and 2*i+1
insert a node: add to the leftmost leaf node and sift up
pop the root: replace the root with the last leaf node and sift down
so pq is space efficient and stable O(logn) efficiency

heapsort: keeps popping the root and we get O(nlogn) stable efficiency.
so pq is stable and set is more versatile.

set<class T,class compare> we often need to define our own compare class
priority_queue<class T,class container, class compare>, pq also needs to define storage method.
pq is an adapter
set is a container

set: 
size/empty/resize
insert/erase
swap
clear
emplace

find/count
lower_bound/upper_bound/equal_range

pq:
empty/size
top
push/pop

## disjoint set (union find)
disjoint set using tree as its underlie data structure
operations include:
makeset
find
merge

## binary tree

## trie
usually used for string pattern matching (a lot of pattern matching)
often used with google search hints
no built-in data structure

## graph
no built-in data structure

## string
very similar to vector.
begin/end
rbegin/rend
size/length
empty
clear
back
front
push_back/pop_back
insert/delete
assign
replace
swap
c_str
data
copy
find/rfind
find_first_of/find_last_of
find_first_not_of/find_last_not_of
substr
compare
npos

# algorithm
divide and conquer
recursion
dfs
bfs
backtracking
binary search
dynamic programming
two pointers
sliding window
greedy
design
sort






















