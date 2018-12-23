Divide and conquer: divide the problem into several smaller sized similar problems and then assemble the results to get the final results.
Divide and conquer generally needs to consider two parts separately plus case where overlap the two cases. It generally make things much more complicated but will generally improve the efficiency from O(N) to O(logN)

169. Majority Element

find the element over [n/2] times.

- algorithm 1: sort and get the mid value.
- algorithm 2: voting: O(N) iterate over the array and remove different pairs, using count, when the same ++, otherwise --
- algorithm 3: divide and conquer, divide by half and get the majority element from left and right, choose the count larger. base case: the array only has one number.
- algorithm 4: hash table

53. Maximum Subarray

find the largest sum of the subarray
- dp: dp[i] is the max sum ending at i, then dp[i]=max(dp[i-1]+a[i],a[i])
- divide and conquer: 
the idea is accumulate to get the prefix sum and the max subarray sum is then the largest difference of the prefix sum.
for each subarray v1 and v2, we define the following:
  - l: the sum of the sub array with largest sum starting from the first element
  - m: the sum of the sub array with largest sum
  - r: the sum of the sub array with largest sum ending at the last element
  - s: the sum of the whole array
  then:
  l=max(v1.l,v1.s+v2.l) //appending the left to right
  m=max(v1.l+v2.r,max(v1.m,v2.m)) //subarray cross the v1 and v2, or in v1 or v2
  r=max(v2.r,v1.r+v2.s) //reverse direction
  s=v1.s+v2.s;
  base case: array of size=1.
  
932. Beautiful Array
a permutation of 1 to N, and there is no a[k]*2=a[i]+a[j] i<k<j
when we have a[k]*2!=a[i]+a[j]
-. we can add any number to it and get a new beautiful array
-. we can multiply a number to it and get a new beautiful array
-. a[i] is odd, a[j] is even, then a[k]*2 will satisfy the beatiful array.
instead of divide and conquer we use the reverse way to grow the array.
Assume we have a beautiful array:
A1=2*A-1 is also a beautiful array
A2=2*A is also a beautiful array
starting from a smallest beautiful array {1}.
```cpp
    vector<int> beautifulArray(int N) {
        vector<int> res = {1};
        while (res.size() < N) {
            vector<int> tmp;
            for (int i : res) if (i * 2 - 1 <= N) tmp.push_back(i * 2 - 1);
            for (int i : res) if (i * 2 <= N) tmp.push_back(i * 2);
            res = tmp;
        }
        return res;
    }
```

241. Different Ways to Add Parentheses
a expression with numbers, + - *, by applying parentheses, we can get different results.
return all possible results.
The operators are the natural position for splitting the expression into left and right part and then we combine the left and right results.
```cpp
    unordered_map<string,vector<int>> memo;
    vector<int> diffWaysToCompute(string input) {
        if(input.size()==0) return vector<int>();
        vector<int> res;
        if(memo.count(input)) return memo[input];
        //divide the string into two parts and do the calculations
        for(int i=0;i<input.size();i++)
        {
            if(input[i]!='+' && input[i]!='-' && input[i]!='*') continue;

            vector<int> res1=diffWaysToCompute(input.substr(0,i)); 
            vector<int> res2=diffWaysToCompute(input.substr(i+1));
            //perform the operations between res1 and res2 using +-*
            for(int k=0;k<res1.size();k++)
            {
                for(int j=0;j<res2.size();j++)
                {
                    if(input[i]=='+') res.push_back(res1[k]+res2[j]);
                    else if(input[i]=='-') res.push_back(res1[k]-res2[j]);
                    else if(input[i]=='*') res.push_back(res1[k]*res2[j]);
                }
            }
            
        }
        if(res.empty()) res=vector<int>(1,stoi(input));
        memo[input]=res;
        return res; //when it has no results return the numbers
    }
```
- when there is no operator in the expression, we need return the number
- left and right combination produce l*r number of different results
- possibly get duplicate results but it is ok since we need different parentheses.

215. Kth Largest Element in an Array

return the kth largest element in its sorted order.
- naive: sort and then get the k-th largest element. sorting/multiset/maxheap/priority-queue O(nlogn)
- improve: only store k elements in maxheap or priority-queue, so sorting is more efficient O(nlogK)
- parital sorting: using c++ nth_element
```cpp
    int findKthLargest(vector<int>& nums, int k) {
        nth_element(nums.begin(), nums.begin() + k - 1, nums.end(), greater<int>());
        return nums[k - 1];
    }
```
- divide and conquer using the same idea as qsort, randomly pick the pivot and separate the array into two parts.
Quicksort
In quicksort, in each iteration, we need to select a pivot and then partition the array into roughly two halves:

  - Elements larger than or equal to the pivot;
  - Elements smaller than or equal to the pivot.
First let's do an example with the array [3, 2, 1, 5, 4, 6] in the problem statement. Let's assume in each time we select the leftmost element to be the pivot, in this case, 3. We then use it to partition the array, which results in [5, 6, 4, 3, 1, 2]. Now 3 is in the 3rd (0-based) position and we know that it is the 4th (1-based) largest element. So, have you noticed how partition is related to this problem?

In the above example, now we know that 3 is the 4th largest element. If we are asked to find the 2nd largest element, it should be to the left of 3. If we are asked to find the 5th largest element, it should be to the right of 3. So, in the average sense, the problem is reduced to approximately half of its original size, giving the recursion T(n) = T(n/2) + O(n) in which O(n) is the time for partition. This recursion, once solved, gives T(n) = O(n) and thus we have a linear time solution. Note that since we only need to consider one half of the array, the time complexity is O(n). If we need to consider both the two halves of the array, like quicksort, then the recursion will be T(n) = 2T(n/2) + O(n) and the complexity will be O(nlogn).

Of course, O(n) is the average/best time complexity. In the worst case, the recursion may become T(n) = T(n - 1) + O(n) and the complexity will be O(n^2).

The algorithm is as follows.

  - Initialize left to be 0 and right to be nums.size() - 1;
  - Partition the array, if the pivot is at the k-1-th position, return it (we are done);
  - If the pivot is right to the k-1-th position, update right to be the left neighbor of the pivot;
  - Else update left to be the right neighbor of the pivot.
  - Repeat 2.
  
 The complexity analysis of the divide and conquer is valuable.
 
 240. Search a 2D Matrix II
 each row sorted, each column sorted
 Thinking the matrix a tree with root at the left bottom corner. Then we know how to go.
 
 312. Burst Balloons
 see dp, it also relates with divide and conquer
 
 903. Valid Permutations for DI Sequence
 D: decrease, I: increase
 using 0 to N for the n-len string. Get the number of permutation.
 dp[i,j]: i the length, j: ending number of j
 see dp
 
 514. Freedom Trail
 see dp
 
 315. Count of Smaller Numbers After Self
 convert the array into a BST with duplicate numbers, and each node also contains the sum of its child
 ```cpp
 struct Node{
    Node *left,*right;
    int dup,sum_left,val;
    Node(int v,int s):val(v),sum_left(s),dup(1){left=right=0;}
};
class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        vector<int> result(nums.size());
        Node* root=0;
        for(int i=nums.size()-1;i>=0;i--)
        {
            root=insert_tree(nums[i],root,result,i,0);
        }
        return result;
    }
    
    Node* insert_tree(int num,Node*root,vector<int>& result,int ind,int presum)
    {
        if(!root) {root=new Node(num,0);result[ind]=presum;}
        else if(num==root->val) {root->dup++;result[ind]=presum+root->sum_left;}
        else if(num<root->val) {root->sum_left++;root->left=insert_tree(num,root->left,result,ind,presum);}
        else root->right=insert_tree(num,root->right,result,ind,presum+root->sum_left+root->dup);
        return root;
    }
};
```

23. Merge k Sorted Lists
using a minheap or a priority-queue

282. Expression Add Operators
given a string of digits from 0 to 9. add + - * to reach the target.
return all possibility of combinations

dfs: try +-* at each position and split into two half and evaluate the results

 
 

