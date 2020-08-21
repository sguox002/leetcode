## contest 113
949. Largest Time for Given Digits 4
950. Flip Equivalent Binary Trees 5
951. Reveal Cards In Increasing Order 5
952. Largest Component Size by Common Factor 8

949. Largest Time for Given Digits.md

### Problem summary
given 4 digits from 0 to 9, build a largest time

### idea
greedy choice does not work and wasted me a lot of time, since the marked easy cheated a lot of people. This shall be at least medium.
4 digits only has 24 permutations and we build each of them, get rid of all those illegals and find the max

### code
```cpp
    string largestTimeFromDigits(vector<int>& A) {
        int maxtime=-1;
        sort(A.begin(),A.end());
        do{
            if(isvalid(A)) maxtime=max(maxtime,A[0]*1000+A[1]*100+A[2]*10+A[3]);
        }while(next_permutation(A.begin(),A.end()));
        if(maxtime<0) return "";
        string ans(5,':');
        ans[0]=maxtime/1000+'0';
        ans[1]=(maxtime%1000)/100+'0';
        ans[3]=(maxtime%100)/10+'0';
        ans[4]=(maxtime%10)+'0';
        return ans;
    }
    bool isvalid(vector<int>& A)
    {
        int hr=A[0]*10+A[1];;
        int min0=A[2]*10+A[3];
        return hr<24 && min0<60;
    }
```

### comment:
- note to use the next-permutation we need first sort the array, otherwise it will not be correct (from the smallest first) otherwise those smaller permutations are discarded.

950. Reveal Cards In Increasing Order.md

### problem summary
given a list of cards, the following operation
take the top one, move the second one to the bottom
need to form a list in increasing order
return the original order of the cards

### idea
reverse build the deck
the largest one first
when there are two or more cards, move the top one to the bottom and then add the new one
using a deque to add and pop easily

### code
```cpp
    vector<int> deckRevealedIncreasing(vector<int>& deck) {
        //smallest always on top
        sort(deck.begin(),deck.end(),greater<int>());
        deque<int> dq;
        //reverse build the array, put back the cards sequentially
        //before add the card, we need bring the back to the front
        for(int i=0;i<deck.size();i++)
        {
            if(dq.size()>=2) 
            {
                dq.push_front(dq.back());
                dq.pop_back();
            }
            dq.push_front(deck[i]);
        }
        return vector<int>(dq.begin(),dq.end());
    }

```

951. Flip Equivalent Binary Trees.md

### problem summary
choose any node and flip its left and right tree, you can perform any times
check if two trees are from these operations

### idea
this is similar to scramble string, but this is much simpler.
check recursively
left vs left && right vs right
left vs right && right vs left

### code
```cpp
    bool flipEquiv(TreeNode* root1, TreeNode* root2) {
        if(!root1 && !root2) return 1; //both null
        else if(!root1 || !root2) return 0;
        if(root1->val!=root2->val) return 0;
        if(flipEquiv(root1->left,root2->left) && flipEquiv(root1->right,root2->right)) return 1;
        if(flipEquiv(root1->left,root2->right) && flipEquiv(root1->right,root2->left)) return 1;
        return 0;
    }
```


952. Largest Component Size by Common Factor.md

### Problem summary
if two numbers share same common factor(>1) connect them
return the largest length of connected group
array length could be very large up to 2e4
element max is 1e5
### idea
1. this is a disjoint set problem
2. blindly merge two numbers the complexity is O(N^2) and could get TLE, need some improvements here

since a number can be joined in the group if there is one common factor, so we can keep a set of factors for the group. Do not need to check elements one by one.

we first prime factorize each number. When a number is added into a group, merge all its prime factors into the group also.
The hardest part is the optimization!
Another way is to merge the prime factors together.

naive algorithm using disjoint set
```cpp
    int largestComponentSize(vector<int>& A) {
        int n=A.size();
        sort(A.begin(),A.end());
        vector<int> parent(n),size(n,1);
        for(int i=0;i<n;i++) parent[i]=i;
        for(int i=0;i<n;i++)
        {
            for(int j=i+1;j<n;j++)
            {
                if(get_parent(i,parent)==get_parent(j,parent)) continue;
                if(gcd(A[j],A[i])>1) merge(i,j,parent,size);
            }
        }
        return *max_element(size.begin(),size.end());
    }
    int get_parent(int i,vector<int>&parent)
    {
        while(i!=parent[i]) i=parent[i];
        return i;
    }
    void merge(int i,int j,vector<int>& parent,vector<int>& size)
    {
        int pi=get_parent(i,parent);
        int pj=get_parent(j,parent);
        if(pi<pj) {parent[pj]=pi,size[pi]+=size[pj],size[pj]=0;}
        if(pi>pj) {parent[pi]=pj,size[pj]+=size[pi],size[pi]=0;}
    }
    int gcd(int a,int b) 
    {
      if(a==0) return b;
      return (b%a,a);
    }
```    
This get TLE.

using prime factorization, each number is represented as a hashset of prime numbers. Since we sorted the array, the larger ones tends to have more prime factors. 

Since the prime numbers are limited, we can merge the prime numbers instead of the array.
We can build a hashmap with the key as the prime factor, and the value as the set of element index.

```cpp
    int largestComponentSize(vector<int>& A) {
        int n=A.size();
        sort(A.begin(),A.end());
        vector<int> parent(n),size(n,1);
        for(int i=0;i<n;i++) parent[i]=i;
        //vector<unordered_set<int>> factors(n);
        unordered_map<int,set<int>> factors;
        unordered_set<int> tset;
        //build the prime factors for each number
        for(int i=0;i<n;i++)
        {
            tset=factorize(A[i]);
            for(auto tt:tset) factors[tt].insert(i);
        }
        for(auto t:factors)
        {
            auto t0=t.second;
            if(t0.size()>=2)
            {
                int node=*t0.begin();
                for(auto it=++t0.begin();it!=t0.end();it++) merge(node,*it,parent,size);
            }
        }
        return *max_element(size.begin(),size.end());
    }
    int get_parent(int i,vector<int>&parent)
    {
        while(i!=parent[i]) i=parent[i];
        return i;
    }
    int merge(int i,int j,vector<int>& parent,vector<int>& size)
    {
        int pi=get_parent(i,parent);
        int pj=get_parent(j,parent);
        if(pi<pj) {parent[pj]=pi,size[pi]+=size[pj],size[pj]=0;}
        if(pi>pj) {parent[pi]=pj,size[pj]+=size[pi],size[pi]=0;}
        return min(pi,pj);
    }
    unordered_set<int> factorize(int x)
    {
        unordered_set<int> prime;
        int d=2;
        while(d*d<=x)
        {
            if(x%d==0) 
            {
                while(x%d==0) x/=d; //remove all its multiples
                prime.insert(d);
            }
            d++;
        }
        if(x>1 || prime.empty()) prime.insert(x); //if there is no factor, itself is a prime number
        return prime;
    }
```

And this solution got accepted about 300ms

### comments
The key of the idea is using the prime factor to associate those elements.


