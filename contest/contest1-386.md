## warmup contest
### 386. Lexicographical Numbers (***)
backtracking: starting with 1 to 9 and then append digits after it.
```cpp
    vector<int> lexicalOrder(int n) {
        vector<int> ans;
        string ns=to_string(n);
        int ndigit=ns.size();
        for(int i=1;i<=9;i++)
          backtrack(n,i,ans);
        return ans;
    }
    void backtrack(int n,int t,vector<int>& ans){
        int last_digit=t%10;
        if(t>n) return;
        if(t<=n) ans.push_back(t);
        for(int i=0;i<=9;i++){
            t=t*10+i;
            backtrack(n,t,ans);
            t/=10;
        }
    }
```

### 387. first unique character in a string (*)
using hashmap

### 388. longest absolute file path (****)
tree structure using stack to store the directory
also stringstream and string proces.
```cpp
    int lengthLongestPath(string input) {
        //file tree can use hashmap, but we can directly use the input for parsing
        //\n represent a line, \t represent the level
        stringstream ss(input);
        int level=0;
        vector<int> vlen;
        string name;
        int ans=0;
        char sbuf[256];
        while(ss.getline(sbuf,256,'\n')){ //it will skip \n and \t
            name=string(sbuf);
            level=0;
            while(name[level]=='\t') level++;
            if(name.find(".")!=string::npos){ //this is a file
                while(vlen.size()>level) vlen.pop_back();
                int tlen=0;
                for(int t: vlen) tlen+=t;
                ans=max(ans,tlen+(int)name.size());
            }
            else {//a directory name
                while(vlen.size()>level) vlen.pop_back();
                vlen.push_back(name.size()-level);
            }
        }
        return ans;
    }
```	
note: getline need use char* with delimiter.