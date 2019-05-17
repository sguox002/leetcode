# Chapter 10 Stack

## contents
1021	Remove Outermost Parentheses		Easy	
682	Baseball Game		Easy	
496	Next Greater Element I		Easy	
844	Backspace String Compare		Easy	
232	Implement Queue using Stacks		Easy	
225	Implement Stack using Queues		Easy	
155	Min Stack		Easy	
20	Valid Parentheses		Easy	

921	Minimum Add to Make Parentheses Valid		Medium	
739	Daily Temperatures		Medium	
946	Validate Stack Sequences		Medium	
94	Binary Tree Inorder Traversal		Medium	
1019 Next Greater Node In Linked List		Medium	
856	Score of Parentheses		Medium	
439	Ternary Expression Parser 	Medium	
1003	Check If Word Is Valid After Substitutions		Medium	
144	Binary Tree Preorder Traversal		Medium	
503	Next Greater Element II		Medium	
636	Exclusive Time of Functions		Medium	
173	Binary Search Tree Iterator		Medium	
901	Online Stock Span		Medium	
341	Flatten Nested List Iterator		Medium	
394	Decode String		Medium	
255	Verify Preorder Sequence in Binary Search Tree 	Medium	
103	Binary Tree Zigzag Level Order Traversal		Medium	
853	Car Fleet		Medium	
331	Verify Preorder Serialization of a Binary Tree		Medium	
735	Asteroid Collision		Medium	
150	Evaluate Reverse Polish Notation		Medium	
385	Mini Parser		Medium	
71	Simplify Path		Medium	
456	132 Pattern		Medium	
907	Sum of Subarray Minimums		Medium	
402	Remove K Digits		Medium	
880	Decoded String at Index		Medium	

895	Maximum Frequency Stack		Hard	
975	Odd Even Jump		Hard	
145	Binary Tree Postorder Traversal		Hard	
770	Basic Calculator IV		Hard	
272	Closest Binary Search Tree Value II 	Hard	
726	Number of Atoms		Hard	
772	Basic Calculator III 	Hard	
42	Trapping Rain Water		Hard	
85	Maximal Rectangle		Hard	
591	Tag Validator		Hard	
224	Basic Calculator		Hard	
316	Remove Duplicate Letters		Hard	
84	Largest Rectangle in Histogram		Hard


## easy

### 1021	Remove Outermost Parentheses		Easy	
when stack is empty the popped is the outmost
or use counter
```cpp
    string removeOuterParentheses(string S) {
        //increase ( and decrease )
        int cnt=0;
        int i=0;
        string ans;
        for(int j=0;j<S.size();j++)
        {
            if(S[j]=='(') cnt++;
            else
            {
                cnt--;
                if(cnt==0) 
                {
                    ans+=S.substr(i+1,j-i-1);
                    i=j+1;
                }
            }
        }
        return ans;
    }
```	

### 682	Baseball Game		Easy	

### 496. Next Greater Element I
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


### 844	Backspace String Compare		Easy	
simple
### 232	Implement Queue using Stacks		Easy	
see design
### 225	Implement Stack using Queues		Easy	
see design
### 155	Min Stack		Easy	
see design
### 20	Valid Parentheses		Easy	
use stack or counter (3 counters) [](){} (counters cannot process the chained cases)
```cpp    bool isValid(string s) {
        stack<char> st;
        for(int i=0;i<s.size();i++)
        {
            if(s[i]=='}' || s[i]==')' || s[i]==']')
            {
                if(st.empty()) return 0;
                char c=st.top();st.pop();
                bool match=(s[i]=='}' && c=='{') || (s[i]==']' && c=='[') || (s[i]==')' && c=='(');
                if(!match) return 0;
            }
            else st.push(s[i]);
        }
        return st.empty();
    }
```

### 503. Next Greater Element II
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



94	Binary Tree Inorder Traversal		Medium	
856	Score of Parentheses		Medium	
439	Ternary Expression Parser 	Medium	(locked)
1003	Check If Word Is Valid After Substitutions		Medium	
144	Binary Tree Preorder Traversal		Medium	
503	Next Greater Element II		Medium	
636	Exclusive Time of Functions		Medium	
173	Binary Search Tree Iterator		Medium	
901	Online Stock Span		Medium	
341	Flatten Nested List Iterator		Medium	
394	Decode String		Medium	
255	Verify Preorder Sequence in Binary Search Tree 	Medium	
103	Binary Tree Zigzag Level Order Traversal		Medium	
853	Car Fleet		Medium	
331	Verify Preorder Serialization of a Binary Tree		Medium	
735	Asteroid Collision		Medium	
150	Evaluate Reverse Polish Notation		Medium	
385	Mini Parser		Medium	
71	Simplify Path		Medium	
456	132 Pattern		Medium	
907	Sum of Subarray Minimums		Medium	
402	Remove K Digits		Medium	
880	Decoded String at Index		Medium	

### 921. Minimum Add to Make Parentheses Valid
simple, removing paired in stack. keep recording extra )
```cpp
    int minAddToMakeValid(string S) {
        //to make it balanced, the () shall be the same amount
        //one we have a ) we need have a balanced in its front
        //once we have a ( we need have a balanced in the behind
        stack<char> st;
        int ans=0;
        for(int i=0;i<S.size();i++)
        {
            if(S[i]=='(') st.push(S[i]);
            else 
            {
                if(st.size()) st.pop();
                else ans++; 
            }
        }
        return ans+st.size();
    }
```

### 739. Daily Temperatures
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
	
### 946. Validate Stack Sequences
simulating the stack push and pop
```cpp
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
       //popped must appear after push and it shall keep the order
        stack<int> st;
        int j=0;
        for(int i=0;i<pushed.size();i++) 
        {
            st.push(pushed[i]);
            //cout<<"push "<<pushed[i]<<endl;
            while(st.size() && st.top()==popped[j])
            {
                st.pop();
                //cout<<"pop "<<j<<" "<<popped[j]<<endl;
                j++;
            }
        }
        //cout<<j<<endl;
        return j==popped.size();

    }
```

### 1019 Next Greater Node In Linked List		Medium	
use the array and build the answer on the fly
```cpp
    vector<int> nextLargerNodes(ListNode* head) {
        stack<int> st;
        vector<int> ans;
        while(head)
        {
            int t=head->val;
            ans.push_back(t);
            while(st.size() && t>ans[st.top()])
            {
                ans[st.top()]=t;
                st.pop();
            }
            st.push(ans.size()-1);
            head=head->next;
        }
        while(st.size()) ans[st.top()]=0,st.pop();
        ans.back()=0;
        return ans;
    }
```	
	
### 856. Score of Parentheses
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

### 1003. Check If Word Is Valid After Substitutions
see string.

### 94. Binary Tree Inorder Traversal
iteration using stack: left, root, right, so we need to put the node into the stack first and then goes to left, and then pop to process the root and then goes to right by following similar steps.

### 144. Binary Tree Preorder Traversal
root left right, so we process root first, then we push its right and process its left

### 636. Exclusive Time of Functions
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

### 341. Flatten Nested List Iterator
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

### 173. Binary Search Tree Iterator
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
### 901. Online Stock Span
stock span is defined as: from today, number of consecuative days  (previous) prices<=today's price.
Typical stack problem since we need old data.
When prices is higher, we need pop out those smaller ones, so we are maintaining a decreasing stack.
```cpp
    vector<int> prices;
    stack<int> st; //stack stores the index with decreasing order
    int days;
    StockSpanner() {
        days=0;
        st.push(0);
        prices.push_back(INT_MAX);
    }
    
    int next(int price) {
        prices.push_back(price);
        //if(st.empty()) {st.push(0);return 1;}
        //int last_day;
        while(st.size() && price>=prices[st.top()]) st.pop();
        days++;
        int ans;
        ans=days-st.top();
        st.push(days);
        return ans;
    }
```	
### 394. Decode String
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

### 103. Binary Tree Zigzag Level Order Traversal
level order traversal and then reverse all even rows (without stack)
using stack to reverse can also be fine (this is an important function of stack)

### 331. Verify Preorder Serialization of a Binary Tree
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

	
### 735. Asteroid Collision
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

### 853. Car Fleet
cars running along the same lane at different speed and different positions, when catched up, the two cars bumps together to one fleet. At the destination, get the number of fleets.
method 1: calculate the time for each car to reach the destination. Sort it according to the positions. Reversely check if ith car needs less time than i+1 car, then the two cars merges to a fleet.
method 2: stack. We put the farthest car into stack, when the next car comes and uses less time, pop the old one. The remaining in the stack will be the number of fleets. (almost the same as method 1 and stack is not really necessary)
```cpp
    int carFleet(int target, vector<int>& position, vector<int>& speed) {
        if(speed.size()==0) return 0;
        vector<double> time(speed.size());
        //need sort these cars according to its position
        vector<vector<int>> cars(speed.size(),vector<int>(2));
        for(int i=0;i<speed.size();i++) {cars[i][0]=position[i];cars[i][1]=speed[i];}
        sort(cars.begin(),cars.end());
        for(int i=0;i<speed.size();i++) time[i]=double(target-cars[i][0])/cars[i][1];
        //copy(time.begin(),time.end(),ostream_iterator<double>(cout," "));
        //merge with previous if its time is less than previous
        for(int i=speed.size()-2;i>=0;i--)
        {
            if(time[i]<time[i+1]) time[i]=time[i+1];
        }
        int num_grp=1;
        //vector<int> fleet(speed.size());
        for(int i=speed.size()-2;i>=0;i--) 
        {
            if(time[i]>time[i+1]) num_grp++;
        }
        return num_grp;
    }
```

### 385. Mini Parser
deserialize the nested integers.
```cpp
    NestedInteger deserialize(istringstream &in) {
        int number;
        if (in >> number)  return NestedInteger(number);
        in.clear();//clear error flag (not a number)
        in.get();//get a char
        NestedInteger list;
        while (in.peek() != ']') 
        {
            list.add(deserialize(in));
            if (in.peek() == ',')
                in.get();
        }
        in.get();
        return list;
    }
```
### 150. Evaluate Reverse Polish Notation
The operator follows the two numbers.
["4", "13", "5", "/", "+"] means: 4+(13/5)
using  a stack: 
if seen an operator, pop the two numbers, calculate it and push back for further evalulation
```cpp
   int evalRPN(vector<string>& tokens) {
        stack<string> st;
        st.push(tokens[0]);
        int i=1;
        int ans;
        while(i<tokens.size())
        {
            if(tokens[i]=="+" || tokens[i]=="-" || tokens[i]=="*" || tokens[i]=="/")
            {
                int b=stoi(st.top());st.pop();
                int a=stoi(st.top());st.pop();
                if(tokens[i]=="+") st.push(to_string(a+b));
                else if(tokens[i]=="-") st.push(to_string(a-b));
                else if(tokens[i]=="*") st.push(to_string(a*b));
                else if(tokens[i]=="/") st.push(to_string(a/b));
            }
            else st.push(tokens[i]);
            i++;
        }
        return stoi(st.top());
    }
```
### 71. Simplify Path
. current dir
.. previous dir
using a stack

### 456. 132 Pattern (**)

this is a classical stack problem. It needs looking for a peak like feature (i<j<k && Ai<A[k]<A[j]).
Suppose we want to find a 123 sequence with s1 < s2 < s3, we just need to find s3, followed by s2 and s1. Now if we want to find a 132 sequence with s1 < s3 < s2, we need to switch up the order of searching. we want to first find s2, followed by s3, then s1.
More precisely, we keep track of highest value of s3 for each valid (s2 > s3) pair while searching for a valid s1 candidate to the left. Once we encounter any number on the left that is smaller than the largest s3 we have seen so far, we know we found a valid sequence, since s1 < s3 implies s1 < s2.

```cpp
    bool find132pattern(vector<int>& nums) {
        //from right side to left and keep in stack
        stack<int> st;
        int s3=INT_MIN;
        for(int i=nums.size()-1;i>=0;i--)
        {
            if(nums[i]<s3) return 1;
            else while(st.size() && nums[i]>st.top())
            {
                s3=st.top();st.pop();
            }
            st.push(nums[i]);
        }
        return 0;
    }
```
Note: the program gets the max s2 and s3 (s2>s3) pairs from right to left. 
pattern which involves previous often uses stack to approach it.

### 402. Remove K Digits
Given a non-negative integer num represented as a string, remove k digits from the number so that the new number is the smallest possible.
greedy: remove the first peak digit for k times
stack: keeping the stack increasing order, when a smaller one comes, pop previous until we got k
see greedy

### 907. Sum of Subarray Minimums
Given an array of integers A, find the sum of min(B), where B ranges over every (contiguous) subarray of A.
method 1: since each number could be the min, we need just calculate the window for each number and get the sum (left*right*A[i])
```cpp
    int sumSubarrayMins(vector<int>& A) {
        int mod=1e9+7;
        //each element can appear many times
        //for element i, we have a window size which A[i] is the minimum
        vector<int> win(A.size());
        int ans=0;
        for(int i=0;i<A.size();i++)
        {
            int l=i,r=i;
            while(A[r+1]>A[i] && r<A.size()-1) r++;
            while(l>0 && A[l-1]>=A[i]) l--;
            ans+=((((r-i+1)*(i-l+1))%mod)*A[i])%mod;
            ans%=mod;
        }
        return ans;
    }
```    
method 2: this is another classical stack using monotone increasing sequence
We push the number in stack, the elements in stack are all smaller than it. When a number is larger just pushed in, when a number smaller, we need pop all larger ones and we get its left. 

### 880. Decoded String at Index
The string keeps repeating and growing, another problem on this.
cannot hold the string for only finding the kth char.

We decode the string and N keeps the length of decoded string, until N >= K.
Then we go back from the decoding position.
If it's S[i] = d is a digit, then N = N / d before repeat and K = K % N is what we want.
If it's S[i] = c is a character, we return c if K == 0 or K == N

C++:

    string decodeAtIndex(string S, int K) {
        long N = 0, i;
        for (i = 0; N < K; ++i)
            N = isdigit(S[i]) ? N * (S[i] - '0') : N + 1;
        while (i--)
            if (isdigit(S[i]))
                N /= S[i] - '0', K %= N;
            else if (K % N-- == 0)
                return string(1, S[i]);
        return "lee215";
    }
	
895	Maximum Frequency Stack		Hard	
Implement FreqStack, a class which simulates the operation of a stack-like data structure.

FreqStack has two functions:

push(int x), which pushes an integer x onto the stack.
pop(), which removes and returns the most frequent element in the stack.
If there is a tie for most frequent element, the element closest to the top of the stack is removed and returned.
```cpp
    unordered_map<int, int> freq;
    unordered_map<int, stack<int>> m;
    int maxfreq = 0;
public:
    void push(int x) {
        maxfreq = max(maxfreq, ++freq[x]);
        m[freq[x]].push(x);
    }

    int pop() {
        int x = m[maxfreq].top();
        m[maxfreq].pop();
        if (!m[freq[x]--].size()) maxfreq--;
        return x;
    }
```
freq hashmap maintains the frequency
each frequency maintains a stack	

975	Odd Even Jump		Hard	
You are given an integer array A.  From some starting index, you can make a series of jumps.  The (1st, 3rd, 5th, ...) jumps in the series are called odd numbered jumps, and the (2nd, 4th, 6th, ...) jumps in the series are called even numbered jumps.

You may from index i jump forward to index j (with i < j) in the following way:

During odd numbered jumps (ie. jumps 1, 3, 5, ...), you jump to the index j such that A[i] <= A[j] and A[j] is the smallest possible value.  If there are multiple such indexes j, you can only jump to the smallest such index j.
During even numbered jumps (ie. jumps 2, 4, 6, ...), you jump to the index j such that A[i] >= A[j] and A[j] is the largest possible value.  If there are multiple such indexes j, you can only jump to the smallest such index j.
(It may be the case that for some index i, there are no legal jumps.)
A starting index is good if, starting from that index, you can reach the end of the array (index A.length - 1) by jumping some number of times (possibly 0 or more than once.)

Return the number of good starting indexes.

We need to jump higher and lower alternately to the end.

Take [5,1,3,4,2] as example.

If we start at 2,
we can jump either higher first or lower first to the end,
because we are already at the end.
higher(2) = true
lower(2) = true

If we start at 4,
we can't jump higher, higher(4) = false
we can jump lower to 2, lower(4) = higher(2) = true

If we start at 3,
we can jump higher to 4, higher(3) = lower(4) = true
we can jump lower to 2, lower(3) = higher(2) = true

If we start at 1,
we can jump higher to 2, higher(1) = lower(2) = true
we can't jump lower, lower(1) = false

If we start at 5,
we can't jump higher, higher(5) = false
we can jump lower to 4, lower(5) = higher(4) = false

```cpp
    int oddEvenJumps(vector<int>& A) {
        int n  = A.size(), res = 1;
        vector<int> higher(n), lower(n);
        higher[n - 1] = lower[n - 1] = 1;
        map<int, int> map;
        map[A[n - 1]] = n - 1;
        for (int i = n - 2; i >= 0; --i) {
            auto hi = map.lower_bound(A[i]), lo = map.upper_bound(A[i]);
            if (hi != map.end()) higher[i] = lower[hi->second];
            if (lo != map.begin()) lower[i] = higher[(--lo)->second];
            if (higher[i]) res++;
            map[A[i]] = i;
        }
        return res;
    }
```
	
145	Binary Tree Postorder Traversal		Hard	
see tree

770	Basic Calculator IV		Hard	
272	Closest Binary Search Tree Value II 	Hard	
locked

726	Number of Atoms		Hard	
```cpp
    string countOfAtoms(string formula) {
        std::map<string, int> stats;
        std::reverse(formula.begin(), formula.end());
        compute(formula, stats);
        string result;
        
        for (auto& kv : stats) {
            result += kv.first;
            if (kv.second > 1) {
                result += std::to_string(kv.second);
            }
        }
        return result;
    }
    
private: // K4(ON(SO3)2)2
    void compute(string& formula, std::map<string, int>& stats) {
        int n = formula.size();
        
        std::stack<int> stk; // factor
        stk.push(1);
        
        string elem; // chemical element, e.g., C, N, O, Na, Mg, Al, etc.
        int factor = 1; // number of this element, default to 1 if not otherwise specified
        
        for (int i = 0; i < n; ++i) {
            auto& c = formula[i];
            if (isdigit(c)) { // E.g., original 312 was reverse to 213.
                int val = 0, expo = 1;
                do {
                    val += (c - '0') * expo;
                    ++i; expo *= 10;
                } while (isdigit(c = formula[i]));
                factor = val; // explicit number of this element
                
                i -= 1;
            } else if (c == ')') { 
                stk.push(factor * stk.top());
                factor = 1; //            Al(OH)3
            } else if (c >= 'A' && c <= 'Z') {
                elem += c;
                std::reverse(elem.begin(), elem.end());
                stats[elem] += factor * stk.top();
                factor = 1;
                elem.clear();
            } else if (c >= 'a' && c <= 'z') {
                elem += c;
            } else { // (c == '(') {  
                stk.pop();  // Ca(OH)2
                // elem.clear();
            } 
        }   
    }
```	
772	Basic Calculator III 	Hard	
locked

42	Trapping Rain Water		Hard	
```cpp
    int trap(vector<int>& height) {
        //using stack similar to largest rectangle in histogram
        //we maintain a stack with index, in decreasing order of height
        int n=height.size();
        stack<int> s;
        int i=0,total=0;

        while(i<n)
        {
            if(s.empty() || height[i]<=height[s.top()]) s.push(i++);
            else
            {
                int tp=s.top();s.pop();
                //min(h(l),h(r))-h(i). If there is no element in stack, the area is 0
                if(!s.empty())
                {
                    int area=(min(height[i],height[s.top()])-height[tp])*(i-s.top()-1);//current one as the minimum
                    total+=area;
                }
            }
        }
        return total;
    }
```	
85	Maximal Rectangle		Hard	
either use 84 or using dynamic 
```cpp
int maximalRectangle(vector<vector<char> > &matrix) {
    if(matrix.empty()) return 0;
    const int m = matrix.size();
    const int n = matrix[0].size();
    int left[n], right[n], height[n];//row by row
    fill_n(left,n,0); 
    fill_n(right,n,n); 
    fill_n(height,n,0);
    int maxA = 0;
    for(int i=0; i<m; i++) 
    {
        int cur_left=0, cur_right=n; 
        for(int j=0; j<n; j++) 
        { // compute height (can do this from either side)
            if(matrix[i][j]=='1') height[j]++; 
            else height[j]=0;
        }
        for(int j=0; j<n; j++) 
        { // compute left (from left to right)
            if(matrix[i][j]=='1') left[j]=max(left[j],cur_left);
            else {left[j]=0; cur_left=j+1;}
        }
        // compute right (from right to left)
        for(int j=n-1; j>=0; j--) 
        {
            if(matrix[i][j]=='1') right[j]=min(right[j],cur_right);
            else {right[j]=n; cur_right=j;}    
        }
        // compute the area of rectangle (can do this from either side)
        for(int j=0; j<n; j++)
            maxA = max(maxA,(right[j]-left[j])*height[j]);
    }
    return maxA;
}
```
591	Tag Validator		Hard	
see string

224	Basic Calculator		Hard	
```cpp
    int calculate(string s) {
        //+/- has lhs and rhs, can use binary tree and post-order traversal evaluation
        //also can use stack to do this. if we meet a ( we need to get the matching ) and evaluate it first
        //recursive approach: search for () first and evaluate using stack
        stack<int> brpos;
        int ind=0,start=0;//s.find_first_of('(');
        while(ind!=string::npos) //exists ()
        {
            ind=s.find_first_of("()",start);
            if(ind!=string::npos)
            {
                char c=s[ind];
                if(c=='(') brpos.push(ind);
                if(c==')')
                {
                    int ind1=brpos.top();
                    brpos.pop();
                    string res=myeval(s.substr(ind1+1,ind-ind1-1)); //evaluate it and update the string
                    s.replace(ind1,ind-ind1+1,res); //replace the string and size will be changed
					start=ind1;
                }
				else start=ind+1;
            }
			
        }
        //eval the string directly
        string res=myeval(s); 
        return stoi(res);
            
    }
    
    string myeval(string s) //there is no () inside, only numbers and +/- and space
    {
        int res=0,i;
        char c;
        stringstream ss(s);
        ss>>res;
        while(!ss.eof())
        {
            c=0; //to skip space
            ss>>c;
            if(c=='+') {ss>>i;res+=i;}
            if(c=='-') {ss>>i;res-=i;}
        }
        return to_string((long long)res);
        
    }
```	
316	Remove Duplicate Letters		Hard	
Given a string which contains only lowercase letters, remove duplicate letters so that every letter appear once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.
greedy
```cpp
    string removeDuplicateLetters(string s) {
        //create a map for each character
        map<char,vector<int>> freq;
        for(int i=0;i<s.size();i++) freq[s[i]].push_back(i);
        //greedy choice: the leftmost char shall have an index than all other max index, this letter shall be as small as possible
        string ans;
        auto it=freq.begin();
        bool found=0;
        int nchar=freq.size();
        while(ans.size()<nchar)
        {
            found=1;
            //cout<<it->first<<endl;
            for(auto it1=freq.begin();it1!=freq.end();it1++)
            {
                if(it1==it) continue;
                if(it->second[0]>it1->second.back()) {it++;found=0;break;}
            }
            if(found) 
            {
                int ind=it->second[0];
                //cout<<"chosen:"<<it->first<<endl;
                ans+=it->first;
                freq.erase(it);
                for(auto it1=freq.begin();it1!=freq.end();it1++) //remove all index smaller than ind
                {
                    auto it2=lower_bound(it1->second.begin(),it1->second.end(),ind);
                    it1->second.erase(it1->second.begin(),it2);
                }
                it=freq.begin();
            }
        }
        return ans;
        
    }
```

84	Largest Rectangle in Histogram		Hard
```cpp
    int largestRectangleArea(vector<int>& heights) {
        //add two guarding nodes to avoid special treatment
        heights.insert(heights.begin(),0);
        heights.push_back(0);
        stack<int> st;
        int maxarea=0;
        for(int i=0;i<heights.size();i++)
        {
            while(!st.empty() && heights[i]<heights[st.top()])
            {
                int tp=st.top();st.pop();
                maxarea=max(maxarea,(i-st.top()-1)*heights[tp]);
            }
            st.push(i);
        }
        return maxarea;
    }
```
	
