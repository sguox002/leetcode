## biweek 9
### 1196. How Many Apples Can You Put into the Basket (*)
<em>Problem:

You have some apples, where arr[i] is the weight of the i-th apple.  You also have a basket that can carry up to 5000 units of weight.

Return the maximum number of apples you can put in the basket.
</em>
idea:

greedy, from light to heavy, prefix sum.

### 1197. Minimum Knight Moves (****)
<em>Problem:

In an infinite chess board with coordinates from -infinity to +infinity, you have a knight at square [0, 0].

A knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.

Return the minimum number of steps needed to move the knight to the square [x, y].  It is guaranteed the answer exists.
|x|+|y|<300.
</em>

Approach:
- intuition is bfs for shortest move.
- any position can be limited in first quarter.
- bfs will grow substantially if layer number is large.
The critical point for this problem is limit the coordinate in 1st quarter.
```cpp
    int minKnightMoves(int tx, int ty)
    {
        tx = abs(tx), ty = abs(ty);
        int n = 400;
        vector<vector<int>> mat(n, vector<int>(n,-1));

        int dx[] = {-2, -2, -1, -1,  1, 1,  2, 2};
        int dy[] = {-1,  1, -2,  2, -2, 2, -1, 1};

        int row = 0, col = 0, x, y;

        queue<pair<int, int>> q;

        mat[row][col] = 0;
        q.push(make_pair(row,col));

        while(!q.empty())
        {
            if(mat[tx][ty] != -1)
                return mat[tx][ty];

            row = q.front().first;
            col = q.front().second;
            q.pop();

            for(int i = 0; i < 8; i++)
            {
                int x = abs(row + dx[i]);//limit it in 1st quarter.
                int y = abs(col + dy[i]);

                if(x >= 0 and y >= 0 and x < n and y < n and mat[x][y] == -1)
                {
                    mat[x][y] = 1 + mat[row][col];
                    q.push(make_pair(x,y));
                }
            }
        } 
        return mat[tx][ty];
    }
```	

### 1198. Find Smallest Common Element in All Rows (**)
<em>Problem:

Given a matrix mat where every row is sorted in increasing order, return the smallest common element in all rows.

If there is no common element, return -1.
</em>
idea:

- this is so similar to sorted list merge using k pointers with PQ. O(mn)
- using first row number and binary search in other rows. O(mnlog(m))
- count using hashmap.

### 1199. Minimum Time to Build Blocks
<em>Problem:

You are given a list of blocks, where blocks[i] = t means that the i-th block needs t units of time to be built. A block can only be built by exactly one worker.

A worker can either split into two workers (number of workers increases by one) or build a block then go home. Both decisions cost some time.

The time cost of spliting one worker into two workers is given as an integer split. Note that if two workers split at the same time, they split in parallel so the cost would be split.

Output the minimum time needed to build all blocks.

Initially, there is only one worker.
blocks = [1,2,3], split = 1
Split 1 worker into 2, then assign the first worker to the last block and split the second worker into 2.
Then, use the two unassigned workers to build the first two blocks.
The cost is 1 + max(3, 1 + max(1, 2)) = 4.

</em>
Analysis:

- n jobs need split n workers, and one worker can split into two workers.
- parallel work: the time is the max.
- assign worker to block matters to the total time.
- apparently, worker split and assign the longest job, the other guy split and assign him the longest job. seems a greedy approach works. above observation is incorrect, each guy has option to do work or split again, this makes it a binary tree. The leaf node is the time. each branch is the time to finish the job. and the answer is the max of all the branches.
- We can model this entire question as a binary tree that we need to construct with a minimum max depth cost. Each of the blocks is a leaf node, with a cost of its face value. And then each inner node will be of cost split. nodes that are sitting at the same level represent work that is done in parallel. We know there will be len(blocks) - 1 of these inner nodes, so the question now is how can we construct the tree such that it has the minimum depth.
- Huffman is used to build a tree with minimum total weighted path. we can choose either split or not split. This is the same as choosing 0 or 1 in Huffman code. Initially, the optimal tree can be built by using a huffman coding way. Then combine the least two nodes. After that, rebuild the huffman tree.
- thus we get the algorithm: build a min heap and pop the top 2 and add split to the larger one, and put it back until there is one left. This is called Huffman algorithm.
- this is reverse thinking: look each num in blocks as leaf, and merge these leaves with cost "split" until get one root.
for example [1,2,3] with split=1
first leaf [1,2,3]
the [1,2] merge with split, got max 3
3 merge with 3 with split, got 4.

```cpp
    int minBuildTime(vector<int>& blocks, int split) {
        priority_queue<int, vector<int>, greater<int>> pq;
        for(int num: blocks) {
            pq.push(num);
        }
        while (pq.size() > 1) {
            int a = pq.top(); pq.pop();
            int b = pq.top(); pq.pop();
            pq.push(b+split);
        }
        return pq.top();
    }
```