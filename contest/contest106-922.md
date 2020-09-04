## contest 106

Sort Array By Parity II3
Minimum Add to Make Parentheses Valid4
3Sum With Multiplicity6
Minimize Malware Spread

922. Sort Array By Parity II
<em>
Given an array A of non-negative integers, half of the integers in A are odd, and half of the integers are even.

Sort the array so that whenever A[i] is odd, i is odd; and whenever A[i] is even, i is even.

You may return any answer array that satisfies this condition.
</em>

two pointer

```cpp
    vector<int> sortArrayByParityII(vector<int>& A) {
        vector<int> ans(A.size());
        int odd=1,even=0;
        for(int i=0;i<A.size();i++)
        {
            if(A[i]%2) {ans[odd]=A[i];odd+=2;}
            else {ans[even]=A[i],even+=2;}
        }
        return ans;
    }
```

923. 3Sum With Multiplicity
<em>
Given an integer array A, and an integer target, return the number of tuples i, j, k  such that i < j < k and A[i] + A[j] + A[k] == target.

As the answer can be very large, return it modulo 10^9 + 7
</em>
approach 1: sort and using 3 pointer.
```cpp
    int threeSumMulti(vector<int>& A, int target) {
        int modulus=1e9+7;
        int i,j,k,n=A.size();
        sort(A.begin(),A.end());
        int cnt_i=1,cnt_j=1,cnt_k=1,ans=0;
        int prev=0;
        for (i = 0; i < n; i++)
        {
            k = n - 1;
            j = i + 1;
            while (j < k)
            {
                if (A[i] + A[j] + A[k] > target) k--;
                else if (A[i] + A[j] + A[k] < target) j++;
                else //=target
                {
                    while (j < k-1 && A[k] == A[k - 1]) {k--;cnt_k++;}
                    while (j < k-1 && A[j] == A[j + 1]) {j++;cnt_j++;}
                    if(A[j]==A[k]) //choose 2 from number (cnk_k+cnt_j)
                        prev=((cnt_k+cnt_j)*(cnt_k+cnt_j-1)/2)%modulus;
                    else
                        prev=(cnt_j*cnt_k)%modulus;
                    ans+=prev;
                    ans%=modulus;
                    k--; j++;
                    cnt_i=1,cnt_j=1,cnt_k=1;
                }
            }
        }
        return ans%modulus;
    }
```

hashmap with 3 pointer:

```cpp
    int threeSumMulti(vector<int>& A, int target) {
        unordered_map<int, long> c;
        for (int a : A) c[a]++;
        long res = 0;
        for (auto it : c)
            for (auto it2 : c) {
                int i = it.first, j = it2.first, k = target - i - j;
                if (!c.count(k)) continue;
                if (i == j && j == k)
                    res += c[i] * (c[i] - 1) * (c[i] - 2) / 6;
                else if (i == j && j != k)
                    res += c[i] * (c[i] - 1) / 2 * c[k];
                else if (i < j && j < k)
                    res += c[i] * c[j] * c[k];
            }
        return res % int(1e9 + 7);
    }
```


924. Minimize Malware Spread

<em>
In a network of nodes, each node i is directly connected to another node j if and only if graph[i][j] = 1.

Some nodes initial are initially infected by malware.  Whenever two nodes are directly connected and at least one of those two nodes is infected by malware, both nodes will be infected by malware.  This spread of malware will continue until no more nodes can be infected in this manner.

Suppose M(initial) is the final number of nodes infected with malware in the entire network, after the spread of malware stops.

We will remove one node from the initial list.  Return the node that if removed, would minimize M(initial).  If multiple nodes could be removed to minimize M(initial), return such a node with the smallest index.

Note that if a node was removed from the initial list of infected nodes, it may still be infected later as a result of the malware spread.
</em>

union-find
idea: the node with the largest number of nodes in the set is the candidate.

```cpp
    int minMalwareSpread(vector<vector<int>>& graph, vector<int>& initial) {
        int n=graph.size();
        vector<int> parents(n),size(n,1);
        
        for(int i=0;i<n;i++) parents[i]=i;
        int num_set=n;
        for(int i=0;i<n;i++)
        {
            for(int j=i;j<n;j++)
            if(graph[i][j]) merge(parents,i,j,num_set,size);
        }
        //sort(initial.begin(),initial.end());
        //copy(size.begin(),size.end(),ostream_iterator<int>(cout," "));
        unordered_map<int,set<int>> mp;
        for(int i=0;i<initial.size();i++)
            mp[get_id(parents,initial[i])].insert(initial[i]);
        //if there are more than one, then it is 0
        int max0=0,ans=INT_MAX;
        for(auto it=mp.begin();it!=mp.end();it++)
        {
            if(it->second.size()>1) 
            {
                if(max0==0) ans=min(ans,*(it->second.begin()));
            }
            else
            {
                if(max0<size[it->first])
                {
                    max0=size[it->first];
                    ans=*(it->second.begin());
                }
                else if(max0==size[it->first])
                    ans=min(ans,*(it->second.begin()));
            }
        }
        return ans;
        
    }
    int get_id(vector<int>& parent,int i)
    {
        while(i!=parent[i]) i=parent[i];
        return i;
    }
    void merge(vector<int>& parent,int i,int j,int& num_set,vector<int>& size)
    {
        int i_id=get_id(parent,i);
        int j_id=get_id(parent,j);
        if(i_id==j_id) return;
        if(i_id<j_id) {parent[j_id]=i_id;size[i_id]+=size[j_id];size[j_id]=0;}
        else {parent[i_id]=j_id;size[j_id]+=size[i_id];size[i_id]=0;}
        num_set--;
    }    
```
	
	
