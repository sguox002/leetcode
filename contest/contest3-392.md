## contest 3
### 392. Is subsequence (***)
greedy match using two pointer. When all chars in s are matched, T can be non-completed.	
```cpp
    bool isSubsequence(string s, string t) {
        //greedy using two pointer
        int i=0,j=0;
        while(i<s.size() && j<t.size()){
            while(j<t.size() && t[j]!=s[i]) j++;
            i++;j++;
        }
        return i==s.size() && j<=t.size();
    }
```
### 393. utf8 validation (***)
idea: find the first byte signature and then check remaining contains 10 in binary
```cpp
    bool validUtf8(vector<int>& data) {
        int i=0,rem=0;
        while(i<data.size()){
            if(rem==0){
                if(data[i]<0x80) rem=0;
                else if((data[i]&0xe0)==0xc0) rem=1;
                else if((data[i]&0xf0)==0xe0) rem=2;
                else if((data[i]&0xf8)==0xf0) rem=3;
                else return 0;
            }
            else{
                if((data[i]&0xc0)!=0x80) return 0;
                rem--;
            }
            i++;
        }
        return rem==0;
    }
```

### 394. decode string (****)
recursive stack: common practice, add decoded results into stack and recusively solve subproblem and update the one in the stack top.
```cpp
    string decodeString(string s) {
        //using stack to do recursive
        int ncopy=0;
        stack<string> st;
        string t;
        int i=0;
        //cout<<s<<": "<<endl;
        while(i<s.size()){
            char c=s[i];
            if(isdigit(c)){
                if(t.size()) st.push(t);
                t.clear();
                ncopy=c-'0';
                while(i+1<s.size() && isdigit(s[i+1])){
                    ncopy=ncopy*10+(s[++i]-'0');
                }
                i++;
            }
            else if(isalpha(c)){
                t=c;
                while(i+1<s.size() && isalpha(s[i+1])){
                    t+=s[++i];
                }
                i++;
            }
            else if(c=='['){
                int j=i+1,p=1;
                while(p){
                    if(s[j]=='[') p++;
                    if(s[j]==']') p--;
                    j++;
                }
                
                j--;
                //from i+1 to j-1 (inclusive)
                string t=decodeString(s.substr(i+1,j-i-1));
                //repeat:
                string nt;
                for(int k=0;k<ncopy;k++) nt+=t;
                if(st.size()) st.top()+=nt;
                else st.push(nt);
                i=j+1;
                t.clear();
            }
        }
        string ans=t;
        while(st.size()){
            ans=st.top()+ans;
            st.pop();
        }
        //cout<<ans<<endl;
        return ans;
    }
```
### 395. Longest Substring with At Least K Repeating Characters (*****)
divide and conquer
idea: using the char appear less than k times as a separator

```cpp
    int longestSubstring(string s, int k) {
        //divide and conquer
        //find the char less then k times and divide into two parts
        return helper(s,k,0,s.size());
    }
    int helper(string& s,int k,int start,int end){
        //cout<<s.substr(start,end-start)<<endl;
        if(end-start<k) return 0;
        vector<int> cnt(26);
        
        for(int i=start;i<end;i++) 
            cnt[s[i]-'a']++;
        int ans=0,l=0;
        bool found=0;
        for(int i=start;i<end;i++){
            int t=cnt[s[i]-'a'];
            if(t && t<k){
                ans=max({ans,helper(s,k,l,i)});
                l=i+1;
                found=1;
            }
        }
        if(found) ans=max(ans,helper(s,k,l,end));
        else ans=end-start;
        return ans;
    }
```