899. Orderly Queue
in each move we move one of the char in the first k char to the end
return the smallest string

Remind what bubble sort eventually is: swap pairs

So, you have a buffer of at least 2 when K>1
you can put them back into the queue in different order: swap!

So, K>1 equals bubble sort

```cpp
    string orderlyQueue(string S, int K) {
        if (K>1){
            sort(S.begin(),S.end());
            return S;
        }
        string minS=S;
        for (int i=0;i<S.size();++i){
            S=S.substr(1)+S.substr(0,1);
            minS=min(S,minS);
        }
        return minS;
    }
```

753. Cracking the Safe
return the string to guarantee to open the box (keeping matching the password)
password is n digit using 0 to k-1 digits

need to cover all permutations. and try to maximize the overlap between the permutations.
we can reuse the last n-1 digits and add a new digit at the end. If we find a path reaching all nodes, that is the shortest string.
The total combination is k^n
dfs
```cpp
string crackSafe(int n, int k) {
        string ans = string(n, '0');
        unordered_set<string> visited;
        visited.insert(ans);
        
        for(int i = 0;i<pow(k,n);i++){
            string prev = ans.substr(ans.size()-n+1,n-1);
            for(int j = k-1;j>=0;j--){
                string now = prev + to_string(j);
                if(!visited.count(now)){
                    visited.insert(now);
                    ans += to_string(j);
                    break;
                }
            }
        }
        return ans;
    }
```
The solution prunes early using backward iteration on k. 
when we start from forward, there will be a circle preventing us going on.
???

810. Chalkboard XOR Game


