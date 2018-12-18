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



