### Easy
905. sort array by parity
832. flipping image: stl reverse
561. Array Partition I: min pair sum, sort
922. Sort Array By Parity II
867. Transpose Matrix: A[i,j]=a[j,i]


### Medium
950. Reveal Cards In Increasing Order
reverse build the cards

442. Find All Duplicates in an Array
mark the number seen as negative, then those not marked is duplicates (will marked twice)

695. Max Area of Island
dfs or bfs from any nodes
dfs: remember to mark it visited.

238. Product of Array Except Self
left product and right product, two direction methodology

495. Teemo Attacking
need subtract those overlaps

667. Beautiful Arrangement II
1,k+1,2,k,3,k-1,4,k-2....

565. Array Nesting
use visited array, since every nested array will form a cycle. if a node is visited, it is not needed
769. Max Chunks To Make Sorted
split and sort, then connect and the whole array is sorted, return the max chunks
compare with the sorted array. If the two has same hash, then it is a segment
216. Combination Sum III
1-9, k numbers combination sum to n
dfs with backtrace. don't forget to pop out the solved node
78. Subsets
power subsets corresponds to n-bit from 0 to 1111...1
835. Image Overlap
max overlap
714. Best Time to Buy and Sell Stock with Transaction Fee
dp
900. RLE Iterator
count of numbers, number...
use a pointer to record the current head

926. Flip String to Monotone Increasing
min number of flips
if choose i as the mid point, we need flip all 1 in front, and all zeros behind
so left to right scan to get number of 1s
right to left scan to get number of 0s

48. Rotate Image
swap and reverse

729. My Calendar I
double booking
we need a sorted array of intervals and using binary search to find the one to add
731. My Calendar II
tripe booking
The key is to use double booking to create overlaps and to see the new added one overlaps with current overlap
732. My Calendar III: k booking
if we mark the start a new event, and end minus an event, then accumulate all until the ending is the k event. If we choose the max, we can iterate until the end.
segment tree: build a tree with cnt, start +1, end -1, the same idea
```cpp
class MyCalendarThree {
    map<int, int> timeline;
public:
    int book(int s, int e) {
        timeline[s]++; // 1 new event will be starting at [s]
        timeline[e]--; // 1 new event will be ending at [e];
        int ongoing = 0, k = 0;
        for (pair<int, int> t : timeline)
            k = max(k, ongoing += t.second);
        return k;
    }
};
```
62. Unique Paths-dp problem
39. Combination Sum
no duplicates, all combinations sum to target, number can be reused
dfs with backtracking
```cpp
    void helper(vector<int>& nums,int target,vector<vector<int>>& res,vector<int> tmp,int pos)
    {
        if(target==0) {res.push_back(tmp);return;}
        if(target<0) {return;}
        //solved solution?
        for(int i=pos;i<nums.size();i++)
        {
            //tmp.clear();
            tmp.push_back(nums[i]);
            helper(nums,target-nums[i],res,tmp,i);//since it allows duplicates, use i here
            tmp.pop_back();
        }
    }
```
40. Combination Sum II
each number can only be used once
```cpp
    void helper(vector<int>& nums,int target,vector<vector<int>>& res,vector<int> tmp,int pos)
    {
        if(target==0) {res.push_back(tmp);return;}
        if(target<0) {return;}
        //solved solution?
        for(int i=pos;i<nums.size();i++)
        {
            //tmp.clear();
            if(i && nums[i]==nums[i-1]&&i>pos) continue;
            tmp.push_back(nums[i]);
            helper(nums,target-nums[i],res,tmp,i+1);//since it does not allows duplicates, use i here
            tmp.pop_back();
        }
    }
```
216. Combination Sum III
use 1 to 9, and k numbers to sum to n
```cpp
void backtrack(vector<vector<int>> &result, vector<int> &path, int start, int k, int target)
{
    if(target==0 && k==0) //we reach a good solution
    {
        result.push_back(path);
        return;
    }
    for(int i=start; i<=10-k && i<=target; i++)
    {
        path.push_back(i);
        backtrack(result,path,i+1,k-1,target-i);//reduce a smaller problem k-1
        path.pop_back(); //after trial, go back to another
    }
}
```
377. Combination Sum IV
get the number of combinations. number can be repeated
knapsack with repetition

64. Minimum Path Sum
in matrix, dp problem,
59. Spiral Matrix II
fill the matrix from 1 to n^2
need to get: how many loops, and each loop how to iterate
621. Task Scheduler
given a list of tasks and requirement, return the min number of intervals needed
somewhat similar to the playlist
choose the most frequent char and divide it into k interval with n period
and put other inside, the min time is (k-1)*n
however when n is smaller than the period, we need suppress non-necessary idle

718. Maximum Length of Repeated Subarray
straightforward: sliding window to find max overlap
however, dp is the right way to find the largest common substr

611. Valid Triangle Number
a+b>c
sort first and then fix two a and b, find c

873. Length of Longest Fibonacci Subsequence
dp mixed with array tech, fixed i, i+1, and derive i+2

153. Find Minimum in Rotated Sorted Array
binary search

795. Number of Subarrays with Bounded Maximum
subarrays: for example 1,2,3 is a subarray, then it has [1],[1,2],[2],[2,3],[3],[1,2,3] total 6=1+2+3
using two pointers: 

289. Game of Life
state change at the same time: generally needs another copy for changes, but we can use bit in original to do in-place

11. Container With Most Water
this is a stack problem.
principle: 
each element must be pushed and poped once
increasing or decreasing order: depending on the question. 
For this question, when a lower bar comes, it shall be considered as the highest bar to contain water if using it as the right bar. So we need keep the stack increasing.

560. Subarray Sum Equals K
accumulate sum and find cumsum-k if exist
using hashmap

915. Partition Array into Disjoint Intervals
two parts, so that all elements in left < all elements in right
left and right problem: two directions to get the max

945. Minimum Increment to Make Array Unique
sort and add, greedy

870. Advantage Shuffle
Given two arrays A and B of equal size, the advantage of A with respect to B is the number of indices i for which A[i] > B[i].
Return any permutation of A that maximizes its advantage with respect to B.
greedy: if there is higher, use the smallest, if no, use the smallest.
sort it in multiset, if chosen then we remove it

162. Find Peak Element
num[i]>nums[i-1] && nums[i]>nums[i+1] naively by adding two padding INT_MIN
linear scan: only need compare nums[i]>nums[i+1] and that is the peak. (since nums[i-1]<nums[i] is the reverse of nums[i]>nums[i+1].)
binary search: we reduce the problem size by finding a mid, and mid+1
linear scan can find all local peaks, binary search can just find one.

792. Number of Matching Subsequences
find number of words in dict is subsequence of S
making a hashmap of s (the char vs the list of its index, can also use 26 char vector). Then greedy choose the first matching one

670. Maximum Swap
Given a non-negative integer, you could swap two digits at most once to get the maximum valued number. Return the maximum valued number you could get.
greedy: ith digit compare with its behind max. if less, swap

80. Remove Duplicates from Sorted Array II
inplace remove duplicates, allowing at most 2 duplicates. sorted.
two pointers: a[i]>a[j-2] then add it in

73. Set Matrix Zeroes
Inplace set its row and col to be zero.
just collect all zero's row and columns and set them to be zeros

105. Construct Binary Tree from Preorder and Inorder Traversal
in order: left node right
preorder: node left right
so we can get the root from the preorder first, and split the left and right from in-order
and then recursively solve the subproblems.

120. Triangle
min path sum, dp as min path sum.

106. Construct Binary Tree from Inorder and Postorder Traversal
similar to 105

775. Global and Local Inversions
i<j, A[i]>A[j] calls global inversion, j=i+1 is called local inversion
check if local inversion == global inversion. The array is 0-N-1
Apparently we only have local inversion, that means if we compare with 0,1,...N-1 and we only see local inversions.

713. Subarray Product Less Than K
similar to sum, if product >k then remove the oldest one.

74. Search a 2D Matrix
row sorted. next row > previous row
2d search using the property. start from the bottom left

228. Summary Ranges
sorted array, summary of ranges, merge into ranges

56. Merge Intervals
classical: sort the interval, and iterate and merge always to the back, and update the back

825. Friends Of Appropriate Ages

209. Minimum Size Subarray Sum
Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.
common practice: when sum>=s we keep shrinking the range from the left side

63. Unique Paths II-dp
34. Find First and Last Position of Element in Sorted Array-equal range
33. Search in Rotated Sorted Array-binary search
954. Array of Doubled Pairs
if we can arrange it 2*A[i]=A[i+1]
using multiset to store negative and positive separately
229. Majority Element II
find all elements appearing >n/3 times
hashmap

55. Jump Game
array number is the max steps you can jump, find if we can reach the last.
max steps we can reach at i, and also we must be able to reach i (reach>=i)

79. Word Search
dfs at any starting position. we need mark visited position first and then restore

31. Next Permutation
the next bigger permutation. from right to left, choose the first smaller one with larger one behind it, swap
918. Maximum Sum Circular Subarray
two cases: if the max is inside, and if the min is inside
907. Sum of Subarray Minimums
for each element we find a window which A[i] is the min.
















