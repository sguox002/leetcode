## biweek 5
### 1133. Largest Unique Number (*)
heap map..

### 1134. Armstrong number (*)
sum(digit^k)=N

### 1135. connecting cities with min cost (***)
min spanning tree combining union find and greedy
```cpp
    vector<int> parent;
    int sz;
    int minimumCost(int N, vector<vector<int>>& connections) {
        //min spanning tree greedy or union-find
        //first need make sure they are in one set.
        parent.resize(N);
        sz=N;
        for(int i=0;i<N;i++) parent[i]=i;
        sort(connections.begin(),connections.end(),[](vector<int>& a,vector<int>& b){
            return a[2]<b[2];
        });
        int ans=0;
        for(auto t: connections){
            int pi=findp(t[0]-1),pj=findp(t[1]-1);
            if(pi!=pj){
                ans+=t[2];
                parent[pi]=pj;
                sz--;
            }
        }
        return sz==1?ans:-1;
    }
    int findp(int i){
        while(i!=parent[i]){
            parent[i]=parent[parent[i]];
            i=parent[i];
        }
        return i;
    }
```

### 1136. Parallel courses (***)
bfs source nodes in a graph. (source nodes has no incoming edges).

```cpp
    int minimumSemesters(int N, vector<vector<int>>& relations) {
        vector<unordered_set<int>> adj(N);
        for(auto r: relations){
            adj[r[1]-1].insert(r[0]-1); //incoming edges
        }
        queue<int> q;
        vector<bool> v(N);
        add_source(adj,q,v);
        int step=0;
        while(q.size()){
            int sz=q.size();
            unordered_set<int> nodes;
            while(sz--){
                int t=q.front();
                q.pop();
                for(auto& vn: adj){
                    if(vn.count(t)) vn.erase(t);
                }
            }
            add_source(adj,q,v);
            step++;
        }
        int res=accumulate(v.begin(),v.end(),0);
        
        return res==N?step:-1;
    }
    void add_source(vector<unordered_set<int>>& adj,queue<int>& q,vector<bool>& v){
        for(int i=0;i<adj.size();i++){
            if(adj[i].size()==0 && !v[i]) {
                q.push(i);
                v[i]=1;
            }
        }
    }
```