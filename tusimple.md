
shangping guo <shangpingguo@gmail.com>
9:44 AM (41 minutes ago)
to me

jd:
https://boards.greenhouse.io/tusimplerelocationjobs/jobs/5114698002

Company Overview
Come join a higher calling and find a deeper purpose!  

As a multi-national Artificial Intelligence Technology Company, we are at the epicenter of the Autonomous Vehicle Universe. Our breakthroughs are leading the industry in autonomous trucking.  

While inventing the framework of Autonomous Driving, our current fleet of autonomous Trucks are helping communities receive much-needed supplies and medical equipment around the clock.   Our people are some of the most talented engineers and contributors who are leaving behind a historic legacy.  

TuSimple was founded half a decade ago with the goal of bringing the top minds in the world together to achieve the dream of a driverless truck solution. With a foundation in computer vision, algorithms, mapping, and Artificial Intelligence, TuSimple is working to create the first global commercially viable Autonomous Freight Network

Job Description

Software Architecture and Engineering(SAE) plays a critical role in autonomous driving systems. As the C++ experts at the SAE team, you will responsible for designing, developing frameworks and modules that provide the next-generation platform and environment for the autonomous driving system.

Responsibilities

Research and identify opportunities for improving platforms, libraries, and pipelines
Engage with teams across the organization to architect innovative solutions
Lead the implementation of high-performance, highly-available, mission-critical systems
Adhere to modern C++ standards and industry best practices
Responsible for integrating multiple modules and systems into one platform
Developing and maintaining key integrating tools and monitoring performance
Design, develop, test, debug, and deploy software modules in autonomous driving system, and/or in related platform and tools software.
Experience

5+ years of production coding experience in C++14 or later
Knowledge of STL best practices, memory safe and thread safe design patterns, TDD or BDD, and ABI/API compatibility
Profiling and debugging tools to resolve performance and stability issues
Design and architect systems to meet complex user and developer requirements
Bonus Points

Experience at a growth-stage startup
Experience in Linux kernel.
Experience in ROS.
Experience on ARM Cortex-A architecture.
Experience in the autonomy, automotive, or robotics industries

TDD： test driven development: agile
Test-driven development (TDD) is a software development process relying on software requirements being converted to test cases before software is fully developed, and tracking all software development by repeatedly testing the software against all test cases.

BDD: behavior driven development: agile
ABI: low level binary interface
API: source interface.

ABI: Application Binary Interface
This is how the compiler builds an application.
It defines things (but is not limited to):

How parameters are passed to functions (registers/stack).
Who cleans parameters from the stack (caller/callee).
Where the return value is placed for return.
How exceptions propagate.

ROS: robot operating system
 flexible framework for writing robot software. It is a collection of tools, libraries, and conventions that aim to simplify the task of creating complex and robust robot behavior across a wide variety of robotic platforms.
 
ARM-cortex A: the apple iphone chip A series
(STM32 is cortex-M series 32 bit)
A series: multicore

memory safe:
buffer overflow, or dangling pointers
for security vulnerability

Access errors: invalid read/write of a pointer
Buffer overflow - out-of-bound writes can corrupt the content of adjacent objects, or internal data (like bookkeeping information for the heap) or return addresses.
Buffer over-read - out-of-bound reads can reveal sensitive data or help attackers bypass address space layout randomization.
Race condition - concurrent reads/writes to shared memory
Invalid page fault - accessing a pointer outside the virtual memory space. A null pointer dereference will often cause an exception or program termination in most environments, but can cause corruption in operating system kernels or systems without memory protection, or when use of the null pointer involves a large or negative offset.
Use after free - dereferencing a dangling pointer storing the address of an object that has been deleted.
Uninitialized variables - a variable that has not been assigned a value is used. It may contain an undesired or, in some languages, a corrupt value.
Null pointer dereference - dereferencing an invalid pointer or a pointer to memory that has not been allocated
Wild pointers arise when a pointer is used prior to initialization to some known state. They show the same erratic behaviour as dangling pointers, though they are less likely to stay undetected.
Memory leak - when memory usage is not tracked or is tracked incorrectly
Stack exhaustion - occurs when a program runs out of stack space, typically because of too deep recursion. A guard page typically halts the program, preventing memory corruption, but functions with large stack frames may bypass the page.
Heap exhaustion - the program tries to allocate more memory than the amount available. In some languages, this condition must be checked for manually after each allocation.
Double free - repeated calls to free may prematurely free a new object at the same address. If the exact address has not been reused, other corruption may occur, especially in allocators that use free lists.
Invalid free - passing an invalid address to free can corrupt the heap.
Mismatched free - when multiple allocators are in use, attempting to free memory with a deallocation function of a different allocator[20]
Unwanted aliasing - when the same memory location is allocated and modified twice for unrelated purposes.


linux kernel:
concurrent programming
selection and configuration of hundreds of kernel features/drivers
config of runtime modification of policies (priority, user, kernel)
memory management
IPC and synchronization
virtual file system
IO control
virtualization
secutiry
communication protocol such as tcp/ip.



tusimple coding:
Graph, BFS, DFS, matrix 之类，考的题目基本都是 medium / hard
shall also include dp, especially working with graph.
 
208. Implement the trie

1824. min sideway jumps (very frequently)

red black tree: self balanced BST
each node has an extra bit to store color 0/1

TuSimple Test11 - Load Balance
Given n servers, a list of tasks and each task $t_i$ has a arrival time $a_i$ and a work load $w_i$. The finishing time of $t_i$ is $a_i + w_i -1$. The servers take task in a robin round manner (plus if the server is still working another task, then skip to next server util find the next avaible or wait unitil next avaible server). Simulate the servers handling tasks and find out which server has a heaviest work load.
using a queue or set, available.
+hashmap.
similar problem lc1606.


lc 115: distinct subsequences
dp.

lc 308: range sum query 2d-mutable

目：给一个List of integer，判断这个List 是否是一个 valid order of BFS of BST( Binary Search Tree)
bfs

是矩阵还原，原矩阵行列相加得到新矩阵，所以逐行逐列还原就会得到原矩阵 (prefix sum)

比较像leetcode1293. 给一个1D数组，每个element代表障碍，自己可以左右移动，求到达终点所需最短路径。
数组里面每个数在1-3之间，例如输入【1，2，3】 代表障碍在【0，1】，【1，2】，【2，3】
每移动一次row自动加1. 个人觉得还是很难的。
same as 1824, dp approach.


是有序数列长度为n，里面有k个数字没有按顺序排进去，让你用比nlogn的算法把它重拍
divide and conquer: two lists
the conflict pair shall be all included in the unsorted array (2k)
O(n+klogk)

给你一堆source-destination data(我的理解就是graph的adjacency list),  和可以jump的次数，要你给出number of distinct paths
比如
1: (6, 8)
2: (3, 4, 5):
6: (1, 0)
8: (7, 9)
3: None
4: ...
...
比如count_path(start_position=1, num_jumps=1) output就是2因为path有[(1, 6), (1, 8)]只能跳一次
- count path from source to dest with no limit, shall use memoization + dfs to search all path
- with limit, we need early terminate those path > limit.
- BFS could be better for this.

，给定一个元素全为0和1的矩阵，输入为行列划分的次数，判断能否把矩阵分成包含相等个数1的子矩阵
divide into mxn submatrices. and we know the number of 1s in each submatrix.
seems that we need use backtracking to try all possible cuts.
dp? we can count the submatrix sum using dp.
or we can check row first and then column?
- row only or column only, reduce to 1d array
approach: check row if not possible, false
check col if not possible false
check submatrices, if not possible, return false.

假设有一个n叉树，结点类定义如下：
class Node {
  List<Node> children;
  List<Node> to;
  List<Integer> distance;
}
一开始distance是空的，这题让你给每个结点填充正确的distance，distance代表这个结点和结点to之间的最短路径长度（实际上就是路径长度，树中两个结点之间有且仅有一条路径）。有一个限制就是如果一个结点a中有to，那么to这个结点的to中一定有a。
find the LCA: and depth sum.

LRUCache

 hiring test 6，题目是 Subsequence Removal，在一个输入的array中移除一个 最短的 按升序排列 的 子列，从而使原array中每一个元素都是unique的，最后返回这个subsequence，如果这个子列不是完全按升序排列，则返回[-1]。
这道题在Google上一时没有搜到类似，我的解法是先使用一个dictionary存储所有出现的数字及其index，用一个list存储要返回的子列，一个int记录index(current_index，初始为-1)，然后遍历sorted(array)中的每一个数，通过对index的比较来解决的。最后赶在还剩三分钟的时候终于把所有11个testcase都过了，因为之前在这里没看到过这个group的oa面经，所以来分享一下。
- min need to remove all duplicates.
- or find the longest unique subsequence so that remaining is sorted. (backtracking or brute force)

实现insertList(vector<string>)，deleteList(vector<string>)分别将字符串列表插入/删除自建的字典数据结构，search(string)负责查找并打印自建字典中所有以该string开头的字符串
followup：
如果输入的vector<string>太长怎么办，比如1000000000000000000000000000
break into several tries. 

Given a non-empty array containing only positive integers, find if the array can be partitioned into four subsets such that the sum of elements in both subsets is equal.
backtracking same as 698 Partionn to k equal subset sum.

50分钟，一题coding + 提问。
给你一个map, map city to other city it can reach, 一个path, path from city to city.
for example,
      AAA
    /   \
  BBB     CCC,
map: AAA->(BBBB, CCC), BBB ->AAA , CCC -> AAA
path: AAA-> BBB -> CCC
当前这条path是invalid, 但是我们可以把CCC替换成AAA, 这样就valid了，替换cost是替换了多少字母使得path变成valid, 当前的cost是3
如果path: ABC -> AAA，ABC这个city是invalid, 则可以把ABC替换成BBB, CCC(AAA->AAA是invalid的), 每一种替换cost = 2
求使一条path valid, cost最小。

我是dfs+backtrack暴力求解，面试官说有更好的方法，45分钟也想不粗来了...

knight

LC269: alien dictionary

LC772: basic calculator III

Given a string "ABA"
get the sum value of all substrings of this string (one unique character is 1 score)

lc239: sliding window min

1197: min knight moves

847 934 773 23 109 224 772 290 291 124 269 1153 480 546 489 128 523

第一次：判断一个undirected graph 是不是一个valid tree，input就是一堆node的值。需要自己写class + test case
第二次：least recent cache leetcode有这道题。同样的自己写class + test case 这题基本就是原题
same as 261 Graph Valid tree, using union-find. cycle detection and num edges=n-1

把二叉树变成双向链表
输入和输出需要自行处理，输入是一行一行String，每行是一个节点和他的两个child
输出把链表元素依次输出成String即可
same as 426. Convert BST to sorted doubly linked list

773. sliding puzzle

145. postorder traversal

The first round:

In a 2D matrix constracted  by "1" and "0",  there are 2 islands constructed by connected "1"s, find the shortest bridge which can connect the 2 islands.

Sollution: grow one of the island until it collides with the other. Pass
dfs.

The 2nd round:
There are a set of nodes, each node will pub some topics, and also subscribe to some topics, say:
node1: pubs=["topic2"], subs=["topic3"]
node2: pubs=["topic3"], subs=["topic4"]
node3: pubs=["topic4"], subs=["topic2"]
find whether there is a loop in these nodes? In this example, there is a loop, so return true

union find?

I misunderstood the question and then I did not have time to do the topological sort, which is the best way to do it. first build the graph, then do topological sort.

847. shortest path visiting all nodes

9月底 电面一轮，medium，具体不记得了。
被HR搁置几周，重发邮件提醒；
10月中 电面二轮，medium，字符串pattern类题目。
11月初 onsite，
一轮medium，多线程+锁；
二轮medium-hard，图；
三轮简历+项目；
四轮简历+项目+基础知识；
1周后 offer，2周deadline。


TuSimple onsite会有几轮详细过项目、考察简历。题目难度不一。基本上面试下来，就能感觉到自己过没过。假如申请General SDE，熟悉C++/system programming应该会有较大加分，因为很多gpu, 硬件driver, simulation engine只能写C++。当然也有full-stack和backend (python, go) 的岗位。

感觉Tusimple在问算法方面不会问特别复杂的数据结构，也不要求你一下子可以写出最优解，会给提示，比较注重思考过程以及和面试官的交流。

一道图论题（就是关于找路径中的最大车载量），其实不难，最后我用dfs+dp的方法解决了，面试官说可以接受，但是他说不是最优解

1153. String Transforms Into Another String

输入是个map，含有city name，和这个cit相通的其他city
[
[AAA: [BBB, CCC, DDD]],
[BBB : [CCC, PPP]],
[PPP: [AAA]]
]
形成的graph是
                  PPP——AAA ——DDD
                             /        \
                         BBB —— CCC
输入还有个path，就是设定好的路线，但可能会有拼写失误 比如
1. example 1:
    BAB -> AAA -> DDD 这条是没法跑的，但把BAB改成BBB就可以了，cost是1

2. example 2:
   PPP -> BBB -> CCC 这条也没法跑，得再加个AAA，cost是3

3. example 3:
  EEE -> AAA -> PPP map里没有出现EEE，但把EEE改成BBB或者DDD都可以，cost是3

4. example 4:
  B -> A ->PAP， 把B变成BBB，A变成AAA，PAP改成PPP，cost是6

输出是求把这条路径变成能走路径的最小cost

insert a node cost is the same as replace a node. so it is still equivalent to the min cost or most similar path problem
but the cost now is an edit distance problem. So it is a two dp problem.

1548. The Most Similar Path in a Graph

210 course schedule

Review:
Tusimple seems heavily on graph problems and most of them are hard levels.
commonly used algorithm: dfs, bfs, dp, union-find.
dijkstra

familiar:
239. sliding window min/max
monotonic deque
sliding window median use multiset and left right pointer.

1824. Min sideway jumps
dp approach: dp[i,j] actually only depends on (i-1,k) and we can save space,

115: distinct subsequences
dp: 2d matrix walk.

934 shortest bridge
- store two islands points and get the min distance. dfs
- another approach: using dfs and find one island and color it with 2 and then bfs to expand until we find 1.

23 merge k sorted lists
priority_queue (multiset)

128 longest consecutive sequences
union find, need to map numbers to index so that union find is convenient.
need discard duplicates, and empty input. (edge cases)

523. Continuous Subarray Sum
hashmap + prefix sum %k.
make sure you add mp[0]=-1 for prefix sum.

1606. Find Servers That Handled Most Number of Requests
available servers: use set to include i and i+k, and enable binary search to find i%k. (lower_bound)
busy server: use pq.
The first tech is the key point to reduce the time to find the proper server.
mainly about using data structure.

698. Partition to K Equal Sum Subsets
backtrack using visited array to record the chosen status.

109. Convert Sorted List to Binary Search Tree
convert to array and build bst.

210 course schedule II ***
ordering of the course taken, graph + bfs.
repeatedly remove source nodes.
```
    vector<int> findOrder(int nc, vector<vector<int>>& pre) {
        vector<unordered_set<int>> adj(nc);
        vector<int> edges(nc);
        for(auto p: pre){
            adj[p[0]].insert(p[1]);
            edges[p[0]]++;
        }
        vector<int> ans;
        vector<bool> visited(nc);
        queue<int> q;
        while(1){ //bfs keeping removing zero outcoming nodes
            for(int i=0;i<nc;i++){
                if(edges[i]==0 && !visited[i]){
                    visited[i]=1;
                    q.push(i);
                    ans.push_back(i);
                    //cout<<i<<endl;
                }
            }
            //removing all incoming edges
            int sz=q.size();
            if(sz==0) break;
            while(sz--){
                int node=q.front();
                q.pop();
                for(int i=0;i<nc;i++){
                    if(!visited[i]){
                        if(adj[i].count(node)) edges[i]--;
                    }
                }
            }
        }
        if(ans.size()!=nc) return {};
        return ans;
    }
};
```

269. Alien Dictionary
build the graph (adjacency hashmap) and incoming
using similar approach as course schedule (source nodes)
if there is cycle, no solution;
if look for smallest, using pq.
graph + bfs （similar to course schedule)

dijkstra + dp (pq or queue): shortest path problem
1548. The most similar path in a graph
Note the tusimple variation asks for the min cost instead of the path.
dijkstra, + dp.
It also needs the path tracing information.
```
    vector<int> mostSimilar(int n, vector<vector<int>>& roads, vector<string>& names, vector<string>& targetPath) {
        //dijkstra dp approach
        vector<vector<int>> adj(n);
        for(auto r: roads){
            adj[r[0]].push_back(r[1]);
            adj[r[1]].push_back(r[0]);
        }
        int m=targetPath.size();
        vector<vector<int>> dp(n,vector<int>(m,INT_MAX)),path(n,vector<int>(m,-1));//dp[i,j] use names i as the jth path
        priority_queue<vector<int>,vector<vector<int>>,greater<>> pq;
        vector<bool> v(n);
        //base: use each node as the first and get the cost
        for(int i=0;i<n;i++){
            dp[i][0]=targetPath[0]!=names[i];
            pq.push({dp[i][0],i,0});
        }
        
        while(pq.size()){
            auto p=pq.top();
            pq.pop();
            int d=p[0],i=p[1],j=p[2];
            //if(v[i]) continue;
            if(j==m-1) break;
            v[i]=1;
            for(auto ch: adj[i]){ //use its neighbors for j+1
                int cost=names[ch]!=targetPath[j+1];
                if(dp[ch][j+1]>d+cost){
                    dp[ch][j+1]=d+cost;
                    path[ch][j+1]=i; //parent
                    pq.push({dp[ch][j+1],ch,j+1});
                }
            }
        }
        //min(dp[i][m-1])
        int last=0;
        for(int i=1;i<n;i++) {
            if(dp[last][m-1]>dp[i][m-1]){
                last=i;
            }
        }
        vector<int> ans;
        for(int j=m-1;j>=0;j--) ans.push_back(last),last=path[last][j];
        reverse(begin(ans),end(ans));
        return ans;
    }
```
Note above code if add the v[i] continue will crash
v is not needed since only shorter disance will be added in the queue.
if has to use v, we need to add all vertex to pq beforehand.
tusimple cost of two strings actually ia another dp edit distance problem.


1153. String Transforms Into Another String
replace all occurrences of one char in str1 to any other char.
- need places to hold an extra char (mapping <26)
- need consistent mapping
```
    bool canConvert(string str1, string str2) {
        if(str1==str2) return 1;
		unordered_map<char,char> mp;
		for(int i=0;i<str1.size();i++){
			char c1=str1[i],c2=str2[i];
			if(mp.count(c1) && mp[c1]!=c2) return 0;
			mp[c1]=c2;
		}
		unordered_set<char> ms;
		for(auto t: mp) ms.insert(t.second);
		return ms.size()<26;
    }
```
	
773 sliding puzzle
graph+bfs
```
    int slidingPuzzle(vector<vector<int>>& board) {
        //convert the board to string and using bfs
        string s;
        for(int i=0;i<board.size();i++)
            for(int j=0;j<board[i].size();j++) s+=board[i][j]+'0';
        string target="123450";
        if(s==target) return 0;
        vector<vector<int>> dir{{1,3},{0,2,4},{1,5},{0,4},{1,3,5},{2,4}};;
        //we need keep track the zero position and try all possible move, or use search
        //not sure if there are repeatable 
        queue<string> q;
        unordered_set<string> visited;
        q.push(s);visited.insert(s);
        int step=0;
        while(!q.empty())
        {
            int sz=q.size();
            for(int i=0;i<sz;i++)
            {
                string t=q.front();q.pop();
                int ind0=t.find_first_of('0');
                for(int j=0;j<dir[ind0].size();j++)
                {
                    string tt=t;
                    swap(tt[ind0],tt[dir[ind0][j]]);
                    if(tt==target) return step+1;
                    if(visited.count(tt)==0) {q.push(tt);visited.insert(tt);}
                }
            }
            step++;
        }
        return -1;
    }
```
	
145. binary tree postorder traversal (iterative)
recursive: left right root (you can always simulate the process)
iterative. left, right, root
equivalent: root,right, left.
using stack: push root, right and left. pop in reverse.
similar: n-ary tree postorder traversal.

preorder:
```
    vector<int> preorderTraversal(TreeNode* root) {
        //root,left,right
        stack<TreeNode*> st;
        vector<int> ans;
        while(root || st.size()){
            while(root){
                ans.push_back(root->val);
                st.push(root);
                root=root->left; //go until left is null
            }
            TreeNode* node=st.top();
            if(node->right) root=node->right;
            st.pop();
        }
        return ans;
    }
```	

inorder
```
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> nodes;
        stack<TreeNode*> todo;
        while (root || !todo.empty()) {
            while (root) {
                todo.push(root);
                root = root -> left;
            }
            root = todo.top();
            todo.pop();
            nodes.push_back(root -> val);
            root = root -> right;
        }
        return nodes;
    }
```

simulate the process:
first goes all to left and pop back
choose right and all go to left

```
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> nodes;
        stack<TreeNode*> st;
        TreeNode* last = NULL;
        while (root || !st.empty()) {
            if (root) {
                st.push(root);
                root = root -> left;
            } else {
                TreeNode* node = st.top();
                if (node -> right && last != node -> right) { 
				//if it has right we need process right first.
                    root = node -> right;
                } else { //both left and right are processed.
                    nodes.push_back(node -> val);
                    last = node;
                    st.pop();
                }
            }
        }
        return nodes;
    }
```	

postorder (left, right, root) is reverse of preorder (root,right,left) so we can take one.	
using the similar approach as preorder (dfs)
```
    vector<int> postorderTraversal(TreeNode* root) {
        //left, right, node. use a stack to store 
        //stack： first node, then right, then left, pop in reverse
        stack<TreeNode*> sl;
        vector<int> s;
        if(!root) return s;
        //starting from root, push all right nodes into stack, until dead end, and then pop a left node
        //save all left nodes in another stack
        TreeNode* p=root;
        while(1)
        {
            while(p) 
            {
                s.push_back(p->val);
                if(p->left) sl.push(p->left);
                p=p->right;
            }
            if(sl.size()) //try the left, note when the node is the leaf
            {
                p=sl.top();sl.pop();
                //s.push_back(p->val);//this will push twice
            }
            else break;
        }
        reverse(s.begin(),s.end());
        return s;
        
    }
```
	

847. shortest path visiting all nodes
you can start and stop any node, visiting multiple times.
graph+ dp + bfs.
bitmask dp dp[i,s] ending with node i with mask s.
(all nodes such as this, or traveller problem uses bitmask dp)

1197. Minimum Knight Moves
regular bfs, but we shall keep in the first coordinate.


Hashmap linked list used often for cache mechanism.

146 LRU cache
linked list hashmap
a list to store the elements
a hashmap from value to list iterator to enable O(1) access to the list member
- each time touch the element, move to the front and update the hashtable


460 LFU cache (based on LRU)
linked list + hashmap + iterator
the data structure is pretty complicated.
need frequency, tie use LRU rule
build a 2d hashmap list
freq vs a list of nodes (in LRU rule)
for each list: key to list iterator
hashmap: key to val+freq


426. Convert Binary Search Tree to Sorted Doubly Linked List
add a dummy node and do inorder traversal. (left for previous and right for next)

308: range sum query 2d-mutable (too hard for interview)
binary index tree or segment tree.
BIT using 0 indexed or 1 indexed, hard to prepare for interview
segment tree is better for interviewing purpose.
build a tree postorder, update the node and recursively the tree
tree in a complete tree stored in array
start from root with id=1
sum: break into two subtree recursively.

```
    vector<int> tree;
    int n;
    NumArray(vector<int>& nums) {
        n=nums.size();
        tree.resize(4*n);
        build_tree(nums,1,0,n-1);
    }
    
    void update(int index, int val) {
        update(1,0,n-1,index,val);//start from root, postorder
    }
    void update(int v,int tl,int tr,int ind,int val){
        if(tl==tr) {tree[v]=val;return;}
        int tm=(tl+tr)/2;
        if(ind<=tm) 
            update(2*v,tl,tm,ind,val); //go to left
        else 
            update(2*v+1,tm+1,tr,ind,val); //go to right
        tree[v]=tree[2*v]+tree[2*v+1]; //update parent
    }
    
    int sumRange(int left, int right) {
        return sum(1,0,n-1,left,right); 
    }
    int sum(int v,int tl,int tr,int l,int r){
        if(l>r) return 0;
        if(l==tl && r==tr) return tree[v]; //found a matching interval sum
        int tm=(tl+tr)/2;
        return sum(2*v,tl,tm,l,min(r,tm))+sum(2*v+1,tm+1,tr,max(l,tm+1),r);
    }
    void build_tree(vector<int>& nums,int v,int tl,int tr){
        if(tl==tr) {tree[v]=nums[tl];return;}
        int tm=(tl+tr)/2;
        build_tree(nums,2*v,tl,tm);
        build_tree(nums,2*v+1,tm+1,tr);
        tree[v]=tree[2*v]+tree[2*v+1];
    }
```

224 Basic calculator
 +-() parsing
 although you can avoid recursion using some tricks
 but the most common way is still recursion.

772. Basic calculator III
+-*/() syntax parsing
 harder than 224. but we can follow the similar procedure using recursion.
 
290 word pattern (easy)

291. word pattern
 backtracking using hashmap and all prefix
using two pointer and hashmap matcing.

124. Binary Tree Maximum Path Sum
postorder to get the left and right max sum. (similar to 1d array max subarray sum)

480 sliding window median
two heap balanced
- multiset to sort the elements in the window k.
- get the mid element pointer (next and prev for the iterator c++11)
- k is odd/even
- pop old element num[i-k] and adjust the mid pointer
- add new element num[i] and adjust the mid pointer. (be careful may have multiple elements, so need to find the iterator).

546. Remove Boxes
typical dp with intervals. 
idea: dp[i,j,k] k same color balls connected to interval (i,j) then we connect the inside balls with same prefix color and divide into two subproblem.
two options: 
- use the k balls and solve the remaining,
- wait and merge with one balls and solve the inside problem first.
top down approach.

489 robot room cleaner
dfs: the grid is not revealed. dfs with other status shall be backtracking.
define current position (x,y)
go along one direction until bumped, try other directions
revealed grid stored in hashmap (visited 2d matrix or string for simplified data structure).
pretty tricky:
- at current position, clean and mark, and perform recursive task (4 directions) (using visited to avoid repeatedly visit)
- go back one cell and keep orientation
- roate 90 degree and check other directions.
this is essentially backtracking (need restore the status)
```
   int dir[4][2]={{0,1},{0,-1},{1,0},{-1,0}};
    int x=0,y=0,curd=0; //position
    unordered_set<string> v;
    void cleanRoom(Robot& robot) {
        string s=to_string(x)+" "+to_string(y);
        if(v.count(s)) return;
        v.insert(s);
        robot.clean();
        for(int d=0;d<4;d++){
            if(robot.move()){
                x+=dir[curd][0],y+=dir[curd][1]; //imporant to be in the if block.
                cleanRoom(robot);
                robot.turnRight();
                robot.turnRight();
                robot.move();
                robot.turnRight();
                robot.turnRight();
				x-=dir[curd][0],y-=dir[curd][1];
            }
            robot.turnRight();
            curd=(curd+1)%4;
        }
    }
```	
	stack overflow: the reason the dir array shall follow the clockwise direction.
```	
    unordered_map<int,unordered_map<int,int>> visited; //this will create a 2d matrix
    int x=0,y=0,dir=0;
    int dir0[4][2]={{0,1},{1,0},{0,-1},{-1,0}}; //clockwise
    void cleanRoom(Robot& robot) {
        if(visited[x][y]==1) return;
        visited[x][y]=1;
        robot.clean();
        //simulate the dfs
        
        for(int d=0;d<4;d++){
            
            if(robot.move()){
                x+=dir0[dir][0],y+=dir0[dir][1];
                cleanRoom(robot);//goes along one direction until cannot, then traceback and try other direction
                robot.turnRight();robot.turnRight();//180 degree rotate
                robot.move();
                robot.turnRight();robot.turnRight();//180 degree rotate
                x-=dir0[dir][0],y-=dir0[dir][1];
            }
            robot.turnRight();
            dir=(dir+1)%4;
        }
    }	
```
	
1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance
floyd-warshall O(N^3)


coding interview
- make sure to clarify the requirements
- make sure to talk aloud the thinking process
- first thinking about most suitable data structure: vector, list, queue, stack, deque, tree, pq, heap, et algorithms
- then think about brutal force or familiar pattern or equivalent 
- implement 
- optimization
- pay attention to edge cases
- pay attention to test cases to fix bugs

deign a fifo using vector or array only

design a concurrent fifo based on previous design

auto_ptr, shared_ptr, weak_ptr, unique_ptr

rvalue reference

inline function

static lib vs dynamic lib

update software, how to keep compatibility

how to debug memory safe problem

focused on regular software practices and c++11 new features.

different processes need pass a lot of data, what is the approach?
if another process crashes, how to deal with?


graph common algorithm:
union-find
MST (kruskal algorithm)
dijkstra
bellman-ford (can handle negatives)
dfs (topolgical sort),bfs,dp
floyd-warshal
prim (for MST problem)


c++11
new algorithms
new containers
atomic operations
type traits
regex
smart pointers
async facility
multithreading library

lambda expressions
[capture](parameters)->return-type {body}

auto type deduction and decltype
auto: derive type from initializer
decltype: return the expression or object type.

initializer

deleted and defaulted
use to control the old private and default constructor

nullptr

delegating constructors
call another constructor in the same class in constructor.

rvalue reference
rvalue reference && can bind to right hand side literals or objects
the primary reason for rvalue is move semantics
for performance purpose, only swap the data member of two objects.
if you class supportss moving, declare constructor as 
Movable(Movable&&); //constructor
Movable&& operator=(Movable&&);//assignment 

stl:
unordered_set/map/multiset/multimap, regex, tuple,...

smart pointer
shared_ptr， unique_ptr
automatically deallocate the resources
unique_ptr: at most one uniqe_ptr pointing at any resources, when unique_ptr is destroyed, the resource is destroyed. Copy of the pointer will cause a compile errors

shared_ptr: allows multiple references to the resource, when the last reference is destroyed, the resource is deallocated. (reference counting)







