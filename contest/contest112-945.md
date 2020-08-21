## contest 112
945. min increment to make array unique (greedy)
946. validate stack sequences (stack)
947. most stones removed with same row or column (union-find)
948. bag of tokens (greedy)

### 945. Minimum Increment to Make Array Unique
one move: increment 1 to one element.
greedy: sort and increment neighborings and propagate.
```cpp
    int minIncrementForUnique(vector<int>& A) {
        sort(begin(A),end(A));
        int ans=0;
        for(int i=1;i<A.size();i++){
            int d=A[i]-A[i-1];
            ans+=max(1-d,0);
            A[i]+=max(1-d,0);
        }
        return ans;
    }
```	

### 946. Validate Stack Sequences
given pushed and popped with the same size and permutation of 1 to n.
two pointer, keep pushing if top!=pop, keep popping if top==pop.

```cpp
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        //simulate the stack
        int j=0,n=pushed.size();
        stack<int> st;
        for(int i=0;i<pushed.size();i++){
            st.push(pushed[i]);
            while(st.size() && j<n && st.top()==popped[j]) st.pop(),j++;
        }
        while(j<n && st.size()){
            if(popped[j]!=st.top()) return 0;
            st.pop();
            j++;
        }
        return st.empty();
    }
```	

### 947. Most Stones Removed with Same Row or Column
equivalent to find the number of disjoint set.
- since the m and n is not known, better use hashmap for the parent
- when no parent, we set the parent to itself and add size
- union the row and column (we shall differ the row and col, we use ~col to make it far apart)
- when union, the sz--

```cpp
    unordered_map<int,int> parent;
    int sz;
    int removeStones(vector<vector<int>>& stones) {
        //union-find.. equivalent to find number of set.
        //union column and rows.
        sz=0;
        for(auto p: stones){
            merge(p[0],~p[1]);
        }
        return stones.size()-sz;
    }
    int findp(int i){
        if(!parent.count(i)) {sz++;return parent[i]=i;}
        while(i!=parent[i]) i=parent[i];
        return i;
    }
    void merge(int i,int j){
        int pi=findp(i),pj=findp(j);
        if(pi!=pj){
            parent[pi]=pj;
            sz--;
        }
    }
```

### 948. Bag of Tokens
<em>
ou have an initial power P, an initial score of 0 points, and a bag of tokens.

Each token can be used at most once, has a value token[i], and has potentially two ways to use it.

If we have at least token[i] power, we may play the token face up, losing token[i] power, and gaining 1 point.
If we have at least 1 point, we may play the token face down, gaining token[i] power, and losing 1 point.
Return the largest number of points we can have after playing any number of tokens.
</em>

greedy:
buy the cheapest and sell at the most expensive
using two pointer
keep the max since we may lose points after the max and then we shall stop!!
```cpp
    int bagOfTokensScore(vector<int>& tokens, int P) {
        sort(tokens.begin(), tokens.end());
        int res = 0, points = 0, i = 0, j = tokens.size() - 1;
        while (i <= j) {
            if (P >= tokens[i]) {
                P -= tokens[i++];
                res = max(res, ++points);
            } else if (points > 0) {
                points--;
                P += tokens[j--];
            } else {
                break;
            }
        }
        return res;
    }
```
