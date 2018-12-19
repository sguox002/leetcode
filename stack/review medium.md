682. Baseball Game
simple

496. Next Greater Element I
for each number is array 1 find its next greater element in its right. If there is none, return -1.
this is a good practice for stack, keeping the stack decreasing order. 
why? since we need to find the right bigger one. When the new number comes, we know it is the answer and we do not need it in the stack any more.
O(n) since each element is pushed and poped once.
```cpp
    vector<int> nextGreaterElement(vector<int>& findNums, vector<int>& nums) {
        int n=nums.size();
        stack<int> st;
        unordered_map<int,int> mp; //mp the number with its next greater
        for(int i=0;i<n;i++)
        {
            while(!st.empty() && st.top()<nums[i])
            {
                mp[st.top()]=nums[i];
                st.pop();
            }
            st.push(nums[i]);
        }
        vector<int> ans(findNums.size(),-1);
        for(int i=0;i<findNums.size();i++)
        {
            if(mp.count(findNums[i])) ans[i]=mp[findNums[i]];
        }
        return ans;
    }
```
Note:
1. keep popping when the top < nums[i] since previous may all have it as the right
2. every element shall be pushed once
3. it is regular practice using stack comparing in the front or behind in an array
4. stack is generally O(n) complexity and other means generally takes O(N^2)

503. Next Greater Element II
note this is a circular buffer. 
We first push the array in reverse order:
So in the stack we have:
An-1, An-2, ....A1, A0, where A0 is on the top. Thus each element has n-1 elements on its right side.
This is basically the idea of doubling the array to simulate the circular buffer.
complexity O(2*n)
```cpp
    vector<int> nextGreaterElements(vector<int>& nums) {
        int n=nums.size();
        stack<int> st;
        vector<int> res(n,-1);
        for(int i=n-1;i>=0;i--) st.push(nums[i]);
        for(int i=n-1;i>=0;i--)
        {
            while(!st.empty() && st.top()<=nums[i]) st.pop();
            if(!st.empty()) res[i]=st.top();
            st.push(nums[i]);
        }
        return res;
    }
```

844. Backspace String Compare
simple
232. Implement Queue using Stacks
simple
225. Implement Stack using Queues
20. Valid Parentheses
use stack or vector, no difference

155. Min Stack
need a min vector

921. Minimum Add to Make Parentheses Valid
simple, removing paired in stack. keep recording extra )

946. Validate Stack Sequences
simulating the stack push and pop

739. Daily Temperatures
classical stack, need to get how many days after the temperature is higher.
When higher, we need pop all previous lower temperatures, so the stack is in decreasing order.
```cpp
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> st;
        int n=temperatures.size(),j;
        vector<int> ans(n);
        for(int i=0;i<n;i++)
        {
            while(!st.empty() && temperatures[i]>temperatures[j=st.top()])
            {
                ans[j]=i-j;
                st.pop();
            }
            st.push(i);
        }
        return ans;
    }
```
856. Score of Parentheses
() has 1 score, AB has A+B where A and B are number of balanced pairs, (A) is 2*A
this is not so straightforward however.
Use the stack to keep the score:
when ( push 0
when ) check the top: if it is not zero, then double it, else +1
then update top()+current score (for connecting paired parenthese)
```cpp
    int scoreOfParentheses(string S) {
        stack<int> st;
        st.push(0);
        for(int i=0;i<S.size();i++)
        {
            if(S[i]=='(') st.push(0);
            else
            {
                int score=st.top();
                st.pop();
                if(score==0) score=1;
                else score*=2;
                st.top()+=score;
            }
        }
        return st.top();
    }
```
Note: it is important to first push 0 in the stack, otherwise it will be wrong.

94. Binary Tree Inorder Traversal
iteration using stack: left, root, right, so we need to put the node into the stack first and then goes to left, and then pop to process the root and then goes to right by following similar steps.

144. Binary Tree Preorder Traversal
root left right, so we process root first, then we push its right and process its left

636. Exclusive Time of Functions
- get the id, start/end time
- when next start, accumulate the time
- when another function starts, the previous function suspends and the time shall be subtracted from other function.
```cpp
    vector<int> exclusiveTime(int n, vector<string>& logs) {
        //when it starts pushed in the stack, when it ends, pop it up and store in array
        vector<int> ans(n);
        stack<pair<int,int>> st; //id, time
        for(int i=0;i<logs.size();i++)
        {
            int ind=logs[i].find_first_of(':');
            int id=stoi(logs[i].substr(0,ind));
            int ind1=logs[i].find_first_of(':',ind+1);
            if(ind1-ind==6) //start
            {
                int stime=stoi(logs[i].substr(ind1+1));
                st.push(make_pair(id,stime));
            }
            else //endtime
            {
                int etime=stoi(logs[i].substr(ind1+1));
                int dtime=etime-st.top().second+1;
                ans[id]+=dtime;
                st.pop();
                if(!st.empty()) ans[st.top().first]-=dtime;//equivalent to add the time to the start
            }
        }
        return ans;
    }
```
The key here is: we subtract the time used by other function from the start on the stack top.

341. Flatten Nested List Iterator
This problem is very confusing.
The class NestedInteger: it could be a single integer, or a list of Nested Integers for example 5, [5], [4,[5,[6]]
Needs to build an iterator class for a vector of nested integer.
NestedInteger:
isInteger: the nested integer is a single integer
getInteger: only works for single integer NestedInteger
getList: only works for non-single integer nested integer (since a nestedInteger is also a vector of nestedInteger

The iterator shall:
- be able to navigate the vector
- when the node is non-single integer node, we need expand the iterator.
- using the stack to pop the composite node iterator and replace with its inside iterator ( a list of iterator)
- using begin and end, then we do not need save iterator for each element

173. Binary Search Tree Iterator
in order traversal.
starting from root, we need push all its left
when pop one, we need push its right
```cpp
class BSTIterator {
    //use a stack to keep all the parent nodes
    stack<TreeNode*> s;
    void update(TreeNode *root)
    {
        while(root){s.push(root);root=root->left;}
    }
public:
    BSTIterator(TreeNode *root) {
        update(root);
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !s.empty();
    }

    /** @return the next smallest number */
    int next() {
        //get the top of the stack, it is always the leftmost node
        TreeNode *node=s.top();s.pop();
        update(node->right);
        return node->val;
    }
};
```
901. Online Stock Span
stock span is defined as: from today, number of consecuative days  (previous) prices<=today's price.
Typical stack problem since we need old data.
When prices is higher, we need pop out those smaller ones, so we are maintaining a decreasing stack.

394. Decode String
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef"
natural thinking: we push the [] in stack and then we evaluate the most innerside first. Each string inside [] is also a subproblem. We need to assemble to get a bigger problem.
The complexity is the inner string keeps expanding.
A often used method is: when we meet the first [, we leave the remaining as a subproblem. So expanding is always the latter part

```cpp
    string helper(int& pos, string s) {
        int num=0;
        string word = "";
        for(;pos<s.size(); pos++) 
        {
            char cur = s[pos];
            if(cur == '[') 
            {
                string curStr = helper(++pos, s);
                for(;num>0;num--) word += curStr;
            } 
            else if (cur >= '0' && cur <='9') 
                num = num*10 + cur - '0';
            else if (cur == ']') 
                return word;
            else     // Normal characters
                word += cur;
        }
        return word;
    }
```

103. Binary Tree Zigzag Level Order Traversal
level order traversal and then reverse all even rows (without stack)
using stack to reverse can also be fine (this is an important function of stack)

331. Verify Preorder Serialization of a Binary Tree
preorder: root, left, right. Null is represented as #
method 1: when we see a non-null node, it must add two nodes. When we see # it just add itself. (A null root will has only one #)
We can count non-null nodes and +2, adding any node -1, the whole shall be 0
method 2: using stack O(n), push each once and pop once. 
When we meet a #, keep poping until it is not #. Then pop the non-null node. That means the node # # is replace with a #, indicating the node is fully explored and absorbed as a null node #. At the end, each node shall be explored and leaving a # only in the stack.

"9,3,4,#,#,12,#,#,2,#,6,#,#"

stack status

char   stack
'9':   '9'  
'3':   '3','9'
'4':   '4','3','9'
'#':   '#','4','3','9'
'#':   '#','3','9'
'12':  'n', '#', '3','9'
'#':   '#','1', '#', '3','9'
'#':   '#','3','9' -> '#','9'
'2':   '2', '#','9'
'#':   '#', '2', '#','9'
'6':   '6', '#', '2','#','9'
'#':   '#', '6', '#', '2','#','9'
'#':   '#', '2','#','9' -> '#','9' -> '#'
Stack is hard to understand!!!

735. Asteroid Collision
Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.
The right ones trying to go right, and the left ones trying to go left.
O(n) solution: push into the stack, only when the new comer is negative will collide
- at the end, all the negative star has to be on the left, and all the positive star has to be on the right.
- from the left, a negative star will pass through if no positive star on the left;
- keep track of all the positive stars moving to the right, the right most one will be the 1st confront the challenge of any future negative star.
- if it survives, keep going, otherwise, any past positive star will be exposed to the challenge, by being popped out of the stack.

```cpp
    vector<int> asteroidCollision(vector<int>& asteroids) {
        vector<int> ret;
        if (asteroids.size() == 0) {
            return ret;
        }
        
        stack<int> st;
        for (int i = 0; i < asteroids.size(); i++) 
        {
            int curr = asteroids[i];
            bool skip = false;
            while (!st.empty() && (st.top() > 0 && curr < 0)) 
            {
                int t1 = st.top(); st.pop();
                int t2 = asteroids[i];
                if (t1 + t2 == 0) 
                {
                    skip = true;
                    break;
                } 
                else if (abs(t1) > abs(t2)) curr = t1;
                else curr = t2;
            }
            if (!skip) st.push(curr);
        }
        
        while (!st.empty()) 
        {
            ret.emplace(ret.begin(), st.top());
            st.pop();
        }
        
        return ret;
    }
```






