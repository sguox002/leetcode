
## biweek 8
### 1180. Count Substrings with Only One Distinct Letter (***)
<em>Problem: Given a string S, return the number of substrings that have only one distinct letter.</em>
idea: find the same char region i,j and the number of subarray is len*(len+1) len=j-i;
two pointers.
### 1181. Before and After Puzzle (***)
<em>Problem:

Given a list of phrases, generate a list of Before and After puzzles.

A phrase is a string that consists of lowercase English letters and spaces only. No space appears in the start or the end of a phrase. There are no consecutive spaces in a phrase.

Before and After puzzles are phrases that are formed by merging two phrases where the last word of the first phrase is the same as the first word of the second phrase.

Return the Before and After puzzles that can be formed by every two phrases phrases[i] and phrases[j] where i != j. Note that the order of matching two phrases matters, we want to consider both orders.

You should return a list of distinct strings sorted lexicographically.
</em>
idea:

brutal force:
- check phase[i] and phase[j] to see if they match the condition.
- using stringstream
- store in a set or sort.

### 1182. Shortest Distance to Target Color (***)
<em>Problem:

You are given an array colors, in which there are three colors: 1, 2 and 3.

You are also given some queries. Each query consists of two integers i and c, return the shortest distance between the given index i and the target color c. If there is no solution return -1.


Example 1:

Input: colors = [1,1,2,1,3,2,2,3,3], queries = [[1,3],[2,2],[6,1]]
Output: [3,0,3]
Explanation: 
The nearest 3 from index 1 is at index 4 (3 steps away).
The nearest 2 from index 2 is at index 2 itself (0 steps away).
The nearest 1 from index 6 is at index 3 (3 steps away).
</em>
idea:
1: [0,1,3]
2: [2,5,6]
3: [4,7,8]
given index and color: binary search the closest in the target color array for the given index.
find the upper_bound and then we get [idx-1,idx] to check which is closer.

### 1183. Maximum Number of Ones (****)
<em>Problem:

Consider a matrix M with dimensions width * height, such that every cell has value 0 or 1, and any square sub-matrix of M of size sideLength * sideLength has at most maxOnes ones.

Return the maximum possible number of ones that the matrix M can have.
</em>

idea:
greedy approach: fill the squares with 1s. the out boundary squares shall not fill if possible.

If we create the first square matrix, the big matrix will just be the copies of this one. (translation copies)
The value of each location in the square matrix will appear at multiple locations in the big matrix, count them.
Then assign the ones in the square matrix with more occurances with 1.

```cpp
    int maximumNumberOfOnes(int width, int height, int sideLength, int maxOnes) {
        //greedy: calculate the number of show times of each point in square
        vector<int> cnt;
        for(int i=0;i<sideLength;i++){
            for(int j=0;j<sideLength;j++){
                int cx=(width-i-1)/sideLength+1;
                int cy=(height-j-1)/sideLength+1;
                cnt.push_back(cx*cy);
            }
        }
        sort(cnt.begin(),cnt.end(),greater<int>());
        int ans=0;
        for(int i=0;i<maxOnes;i++) ans+=cnt[i];
        return ans;
    }
```	
- it basically calculate number of appearance of the points in the square shown in the rectangle.
for example [0,0] can be translated horizontally or veritcally.
we sort it according to its number and fill them first.
- we can also use a min heap to pop non-needed elements.
see other O(1) solutions in discussion on the site.