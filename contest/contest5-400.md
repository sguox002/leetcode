## contest 5
### 400. Nth digit (****)
idea: binary search the number in which area, and then find the number, finally get the digit in the number
```cpp
    int findNthDigit(int n) {
        if(n<10) return n;
        vector<long> num(10,1);//1,9,2*90+9
        num[1]=9;
        for(int i=2;i<10;i++){
            num[i]=9*i*pow(10,i-1)+num[i-1];
            //cout<<num[i]<<" ";
        }
        int ind=upper_bound(num.begin(),num.end(),n)-num.begin(); //>=n
        n-=num[ind-1];
        int t=n/ind-1+pow(10,ind-1); //starting from 10...0, number of digits is ind.
        //cout<<t;
        int rem=n%ind;
        if(rem) t++;else rem=ind; //rem=0, the last digit
        //cout<<t<<" "<<rem;
        string s=to_string(t);
        return s[rem-1]-'0';
    }
```

### 401. Binary Watch (****)
idea: 10 bit all combinations and then check all valid combinations. We can also do the validation during backtracking to save space.
```cpp
    vector<string> readBinaryWatch(int num) {
        //hr: 4bits, min: 6bits (hr max 3 lights, min max 5 lights)
        //use it as a whole is easier 
        //backtrack
        vector<string> ans;
        if(num>8) return ans;
        vector<bitset<10>> vbits;
        bitset<10> t(0);
        backtrack(num,t,0,vbits);
        for(auto t: vbits){
            int n=t.to_ulong();
            int hr=(n&0x3c0)>>6;
            int mi=n&0x3f;
            if(hr>11 || mi>59) continue;
            char str[6];
            sprintf(str,"%d:%02d",hr,mi);
            ans.push_back(string(str));
        }
        return ans;
    }
    void backtrack(int n,bitset<10> t,int start,vector<bitset<10>>& vbit){
        if(n==0){
            vbit.push_back(t);
            return;
        }
        for(int i=start;i<10;i++){
            t[i]=1;
            backtrack(n-1,t,i+1,vbit);
            t[i]=0;
        }
    }
```
### 402. Remove K Digits	(*****)
using stack to remove the peak digits recursively

```cpp
    string removeKdigits(string num, int k) {
        //greedy:
        //1432219: remove 1 digit ->132219
        //remove 2 digit: 12219
        //remove 3 digit: 1219. approach: from left to right, repeatedly remove the first peak digit
        //using stack: if the curr digit is less than previous, then remove the stack top
        
        stack<char> st;
        for(char c: num){
            while(st.size() && k && c<st.top()){
                st.pop();
                k--;
            }
            st.push(c);
            //if(k==0) break;
        }
        while(k--){ //still need to remove, now it is sorted
            st.pop();
        }
        string ans;
        while(st.size()) {
            ans+=st.top();
            st.pop();
        }
        //remove leading zeros
        while(ans.back()=='0') ans.pop_back();
        if(ans.empty()) ans="0";
        reverse(ans.begin(),ans.end());
        return ans;
    }
```

### 403. Frog Jump	(****)
bfs with 3 options: k-1, k, k+1
important optimization: if keep k+1, then A[i] and A[i+1] can only have distance less than i.
can also use dfs (recursive approach)

```cpp
    bool canCross(vector<int>& stones) {
        //assuming at ith stone and previous step is k, then next must have a stone at k+1, k or k-1 distance
        //can use bfs
        unordered_set<int> st;
        for(int i=1;i<stones.size();i++){
            if(stones[i]>stones[i-1]+i) return 0;
            st.insert(stones[i]);
        }
        unordered_set<string> v;
        queue<vector<int>> q; //stores position vs step
        int n=stones.size();
        q.push({0,0});
        while(q.size()) {
            int sz=q.size();
            while(sz--){
                auto p=q.front();
                q.pop();
                int pos=p[0],step=p[1];
                if(pos==stones.back()) return 1;
                int ind=0;
                string s;
                s=to_string(pos+step-1)+","+to_string(step-1);
                if(step>1 && st.count(pos+step-1) && !v.count(s)) {q.push({pos+step-1,step-1});v.insert(s);}
                s=to_string(pos+step)+","+to_string(step);
                if(step && st.count(pos+step)&& !v.count(s)) {q.push({pos+step,step});v.insert(s);}
                s=to_string(pos+step+1)+","+to_string(step+1);
                if(st.count(pos+step+1)&& !v.count(s)) {q.push({pos+step+1,step+1});v.insert(s);}
            }
        }
        return 0;
    }
```