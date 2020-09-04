## contest 102

Sort Array By Parity2
Fruit Into Baskets5
Sum of Subarray Minimums6
Super Palindromes

905. Sort Array By Parity
<em>
Given an array A of non-negative integers, return an array consisting of all the even elements of A, followed by all the odd elements of A.

You may return any answer array that satisfies this condition.
</em>

two pointer one from left,one from right.

```cpp
    vector<int> sortArrayByParity(vector<int>& A) {
        for(int i=0,even=0;i<A.size();i++)
        {
            if(A[i]%2==0) swap(A[i],A[even++]); //j is less
        }
        return A;
    }
```

904. Fruit Into Baskets
<em>
In a row of trees, the i-th tree produces fruit with type tree[i].

You start at any tree of your choice, then repeatedly perform the following steps:

Add one piece of fruit from this tree to your baskets.  If you cannot, stop.
Move to the next tree to the right of the current tree.  If there is no tree to the right, stop.
Note that you do not have any choice after the initial choice of starting tree: you must perform step 1, then step 2, then back to step 1, then step 2, and so on until you stop.

You have two baskets, and each basket can carry any quantity of fruit, but you want each basket to only carry one type of fruit each.

What is the total amount of fruit you can collect with this procedure?
</em>

equiv. to find the window with only two types of fruits and the sum is the max.

```cpp
    int totalFruit(vector<int>& tree) {
        //this is to get the longest window which contains only two type of fruits
        unordered_map<int,int> ms;
        int i=0,j=0;
        int ans=0;
        while(j<tree.size()){
            ms[tree[j]]++;
            while(ms.size()>2) {
                ms[tree[i]]--;
                if(ms[tree[i]]==0) ms.erase(tree[i]);
                i++;
            }
            if(ms.size()<=2) ans=max(ans,j-i+1);
            j++;
        }
        return ans;        
    }
```	

907. Sum of Subarray Minimums
<em>
Given an array of integers A, find the sum of min(B), where B ranges over every (contiguous) subarray of A.

Since the answer may be large, return the answer modulo 10^9 + 7.
</em>

stack to find next greater.

```cpp
    int sumSubarrayMins(vector<int>& A) {
        stack<int> st_p,st_n;
        int n=A.size();
        vector<int> left(n),right(n);//max length
        for(int i=0;i<n;i++) {
            left[i]=i+1;
            right[i]=n-i;
        }
        //get previous less and next less
        //increasing order
        for(int i=0;i<A.size();i++){
            while(st_p.size() && A[i]<A[st_p.top()]){
                int tp=st_p.top();st_p.pop();
                right[tp]=i-tp;
            }
            if(st_p.size()) left[i]=i-st_p.top(); //else no change
            
            st_p.push(i);
        }
        int ans=0;
        int mod=1e9+7;
        for(int i=0;i<A.size();i++){
            //cout<<A[i]<<" "<<left[i]<<" "<<right[i]<<endl;
            ans+=left[i]*right[i]*A[i];
            ans%=mod;
        }
        return ans;
    }
```	

906. Super Palindromes

<em>
Let's say a positive integer is a superpalindrome if it is a palindrome, and it is also the square of a palindrome.

Now, given two positive integers L and R (represented as strings), return the number of superpalindromes in the inclusive range [L, R].

 
</em>

```cpp
    int superpalindromesInRange(string L, string R) {
     //R up to 10^5 since P<10^18  
        long l=stol(L);
        long r=stol(R);
        int limit=1e5;
        int ans=0;
        for(int i=1;i<limit;i++)
        {
            //odd and even arrange
            string s=to_string(i);
            string s_even=s+string(s.rbegin(),s.rend());
            //cout<<s_even<<" ";
            string s_odd=s+string(s.rbegin()+1,s.rend());
            
            long p=stol(s_even);
            p*=p;
            if(p>=l && p<=r)
            if(isPalindrome(p)) ans++;
            //cout<<s_odd<<endl;
            p=stol(s_odd);p*=p;
            if(p>=l && p<=r)
            if(isPalindrome(p)) ans++;
        }
        return ans;
    }
    bool isPalindrome(long x)
    {
        string s=to_string(x);
        int i=0,j=s.length()-1;
        while(i<j)
        {
            if(s[i]!=s[j]) return 0;
            i++,j--;
        }
        return 1;
    }
```