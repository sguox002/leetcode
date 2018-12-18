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




