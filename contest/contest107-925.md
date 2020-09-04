## contest 107

925 Long Pressed Name - two pointer
926 Flip String to Monotone Increasing - two end min
927 Three Equal Parts - brutal force
928 Minimize Malware Spread II - union find, reverse.

925. Long Pressed Name
<em>
Your friend is typing his name into a keyboard.  Sometimes, when typing a character c, the key might get long pressed, and the character will be typed 1 or more times.

You examine the typed characters of the keyboard.  Return True if it is possible that it was your friends name, with some characters (possibly none) being long pressed.
</em>

two pointer approach:

```cpp
    bool isLongPressedName(string name, string typed) {
        //count char one by one
        int left1=0,left2=0;
        int cnt1=0,cnt2=0;
        while(left1<name.size() && left2<typed.size())
        {
            if(name[left1]==typed[left2])
            {
                while(name[left1+cnt1]==name[left1]) cnt1++;
                while(typed[left2+cnt2]==typed[left2]) cnt2++;
                if(cnt1>cnt2) return 0;
                left1+=cnt1,left2+=cnt2;
                cnt1=cnt2=0;
            }
            else return 0;
        }
        return left1==name.size() && left2==typed.size();
    }
```

926. Flip String to Monotone Increasing
<em>
A string of '0's and '1's is monotone increasing if it consists of some number of '0's (possibly 0), followed by some number of '1's (also possibly 0.)

We are given a string S of '0's and '1's, and we may flip any '0' to a '1' or a '1' to a '0'.

Return the minimum number of flips to make S monotone increasing.
</em>

left and right two end meet:

```cpp
    int minFlipsMonoIncr(string S) {
        //need to flip to get two substring, left is all 0 right all 1
        int n=S.size();
        vector<int> left1(n+1);
        for(int i=0;i<S.size();i++) 
        {
            left1[i+1]=left1[i]+(S[i]=='1');
        }
            
        int minflip=n;
        int right0=0;
        for(int i=S.size()-1;i>=0;i--)
        {
            right0+=(S[i]=='0');
            minflip=min(minflip,right0+left1[i+1]-1);
            //cout<<i<<" "<<right0[i]<<" "<<left1[i+1]<<endl;
        }
        return minflip;
    }
```

927. Three Equal Parts
<em>
Given an array A of 0s and 1s, divide the array into 3 non-empty parts such that all of these parts represent the same binary value.

If it is possible, return any [i, j] with i+1 < j, such that:

A[0], A[1], ..., A[i] is the first part;
A[i+1], A[i+2], ..., A[j-1] is the second part, and
A[j], A[j+1], ..., A[A.length - 1] is the third part.
All three parts have equal binary value.
If it is not possible, return [-1, -1].

Note that the entire part is used when considering what binary value it represents.  For example, [1,1,0] represents 6 in decimal, not 3.  Also, leading zeros are allowed, so [0,1,1] and [1,1] represent the same value.
</em>

- first we need have 3n set bits
- second we shall have 1 with same index difference
- right zero has effect, leading zero has no effect.

```cpp
    vector<int> threeEqualParts(vector<int>& A) {
      vector<int> ones;
        for(int i=0;i<A.size();i++) if(A[i]) ones.push_back(i);
        if(ones.size()%3) return vector<int>({-1,-1});
        if(ones.size()==0) return vector<int>({1,A.size()-1});
        int len=ones.size()/3;
        //check if the three parts has the same pattern
        for(int i=0;i<len;i++)
        {
            if(ones[i]-ones[0]!=ones[i+len]-ones[len] || ones[i]-ones[0]!=ones[i+2*len]-ones[2*len])
                return vector<int>({-1,-1});
        }
        //the pattern is the same, we need check number of zeros
        int nzeros3=A.size()-1-ones.back();
        int nzeros2=ones[2*len]-ones[2*len-1]-1;
        int nzeros1=ones[len]-ones[len-1]-1;
        
        if(nzeros1>=nzeros3 && nzeros2>=nzeros3)
        {
            return vector<int>({ones[len-1]+nzeros3,ones[2*len-1]+nzeros3+1});
        }
        return vector<int>({-1,-1});
    }
```

928. Minimize Malware Spread II

<em>

(This problem is the same as Minimize Malware Spread, with the differences bolded.)

In a network of nodes, each node i is directly connected to another node j if and only if graph[i][j] = 1.

Some nodes initial are initially infected by malware.  Whenever two nodes are directly connected and at least one of those two nodes is infected by malware, both nodes will be infected by malware.  This spread of malware will continue until no more nodes can be infected in this manner.

Suppose M(initial) is the final number of nodes infected with malware in the entire network, after the spread of malware stops.

We will remove one node from the initial list, completely removing it and any connections from this node to any other node.  Return the node that if removed, would minimize M(initial).  If multiple nodes could be removed to minimize M(initial), return such a node with the smallest index.

 </em>

union-find

```cpp
    int minMalwareSpread(vector<vector<int>>& graph, vector<int>& initial) {
        //the idea: build the disjoint set by removing all infected nodes
        //add the infected nodes except the removed one one by one and find the one with most infected
        //union-find is fast to do union but not good for splitting and that is why we use the above approach
        int n=graph.size();
        vector<int> parents(n),size(n,1);
        //set<int> infected(initials.begin(),initials.end());
        
        sort(initial.begin(),initial.end());
        int minsize=INT_MAX,ans=INT_MAX;        
        for(int k=0;k<initial.size();k++)
        {
            int remove=initial[k];
            int infected_size=0;
            //build the union by disable the remove node
            for(int i=0;i<n;i++) {parents[i]=i;size[i]=1;}
            int num_set=n;
            for(int i=0;i<n;i++)
            {
                for(int j=i;j<n;j++)
                {
                    if(i==remove || j==remove) continue;
                    if(graph[i][j]) merge(parents,i,j,num_set,size);
                }
            }
            //get the number of infected nodes
            unordered_set<int> myset;
            for(int t=0;t<initial.size();t++)
            {
                if(initial[t]==remove) continue;
                myset.insert(get_id(parents,initial[t]));
            }
            
            for(auto it=myset.begin();it!=myset.end();it++)
                infected_size+=size[*it];
            if(minsize>infected_size) minsize=infected_size,ans=remove;
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
	
 


