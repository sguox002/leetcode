trie

208. Implement Trie (Prefix Tree)

```cpp
struct Node
{
    Node* next[26]; //use pointer is much convenient than using array!
    bool is_leaf;
    Node(bool b=0) {fill(next,next+26,(Node*)0);is_leaf=b;}
};
class Trie {
public:
    /** Initialize your data structure here. */
    Node* root;
    Trie() {
        root=new Node();
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        Node* p=root;
        for(int i=0;i<word.size();i++)        
        {
            int ind=word[i]-'a';
            if(!p->next[ind]) p->next[ind]=new Node();
            p=p->next[ind];
        }
        p->is_leaf=1;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        Node* p=find(word);
        return p && p->is_leaf; //has to end with a leaf node
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Node* p=find(prefix);
        return p; //does not have to end with a leaf node
    }
    
    Node* find(string word)
    {
        Node* p=root;
        for(int i=0;i<word.size();i++)
        {
            int ind=word[i]-'a';
            if(p->next[ind]==0) return 0;
            p=p->next[ind];
        }
        return p;
    }
};

```
211. Add and Search Word - Data structure design
```cpp
    struct node{
        node* child[26];
        bool isleaf;
        node(){memset(child,0,26*sizeof(node*));isleaf=0;}
    };
    node* root;
    WordDictionary() {
        root=new node;
    }
    
    /** Adds a word into the data structure. */
    void addWord(string word) {
        node* p=root;
        for(int i=0;i<word.size();i++)
        {
            int t=word[i]-'a';
            if(!p->child[t]) {p->child[t]=new node;}
            p=p->child[t];
        }
        p->isleaf=1;
    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    bool search(string word) {
        return dfs(word,root);
    }
    bool dfs(string word,node* root)
    {
        for(int i=0;i<word.size() && root;i++)
        {
            if(word[i]=='.')
            {
                for(int j=0;j<26;j++)
                {
                    if(dfs(word.substr(i+1),root->child[j])) return 1;
                }
                return 0;
            }
            else
            {
                int t=word[i]-'a';
                root=root->child[t];
            }
        }
        return root && root->isleaf;
    }
```

212. Word Search II
```cpp
struct Node
{
    Node* next[26]; //use pointer is much convenient than using array!
    bool is_leaf;
    Node(bool b=0) {fill(next,next+26,(Node*)0);is_leaf=b;}
};
class Trie {
public:
    /** Initialize your data structure here. */
    Node* root;
    Trie() {
        root=new Node();
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        Node* p=root;
        for(int i=0;i<word.size();i++)        
        {
            int ind=word[i]-'a';
            if(!p->next[ind]) p->next[ind]=new Node();
            p=p->next[ind];
        }
        p->is_leaf=1;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        Node* p=find(word);
        return p && p->is_leaf; //has to end with a leaf node
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Node* p=find(prefix);
        return p; //does not have to end with a leaf node
    }
    
    Node* find(string word)
    {
        Node* p=root;
        for(int i=0;i<word.size();i++)
        {
            int ind=word[i]-'a';
            if(p->next[ind]==0) return 0;
            p=p->next[ind];
        }
        return p;
    }
    Node* get_root() {return root;}
};

class Solution {
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        Trie t;
        for(int i=0;i<words.size();i++) t.insert(words[i]);
        Node* root=t.get_root();
        //use a set to store all the possible words found from the board.
        set<string> all_words;
        for(int i=0;i<board.size();i++)
        {
            for(int j=0;j<board[0].size();j++)
                findWords(board,i,j,"",all_words,root);//to save time, some paths shall be skipped if they first do not match
        }
        vector<string> ans(all_words.size());
        copy(all_words.begin(),all_words.end(),ans.begin());
        return ans;
    }
    void findWords(vector<vector<char>>& board,int x,int y,string word,set<string>& result,Node* root)
    {
        //using backtracking
        if(x<0||x>=board.size()||y<0||y>=board[0].size() || board[x][y]==' ') return;
        char c=board[x][y];
        int ind=c-'a';
        if(root->next[ind])
        {
            word+=c;
            root=root->next[ind]; 
            if(root->is_leaf) result.insert(word);
            board[x][y]=' ';
            findWords(board, x+1, y, word, result, root);
            findWords(board, x-1, y,  word, result, root);
            findWords(board, x, y+1,  word, result, root);
            findWords(board, x, y-1,  word, result, root);
            board[x][y]=c;        
        }        
    }
};
```

336. Palindrome Pairs
```cpp

class Node
{
public:
    int id ;
    Node** child;
    vector<int> list;
    Node()
    {
        id = -1;
        child = new Node* [26];
        for(int i=0; i<26; i++) child[i] = 0;
    }
public:
    ~Node() {delete[] child;list.clear();}
};


class Solution {
public:
    vector<vector<int>> palindromePairs(vector<string>& words) {
        vector<vector<int>> res;
        vector< Node* > de;
        int sz = words.size();
       
        Node * root = new Node();
        string str ;
        for(int i=0; i<sz; i++)
        {
           str = words[i];
           addword(str,i,root,de);
        }
        for(int i=0; i<sz; i++)
        {
            str = words[i];
            searchword(res,str,i,root);
        }
        for(int i=0; i<de.size(); i++)
            delete (de[i]);
        de.clear();
        return res;
        
    }
    void addword( string& str , int pos , Node* root , vector<Node*>& de)
    {
        int sz = str.size();
        reverse(str.begin(),str.end());
        for(int i=0; i<sz; i++)
        {
            if( isValid(str,i,sz-1) )
            {
                (root->list).push_back(pos);
            }
            if(root->child[str[i]-'a']==NULL)
            {
                root->child[str[i]-'a'] = new Node();
                de.push_back(root->child[str[i]-'a']);
            }
            root = root->child[str[i]-'a'];
        }
        
        root->id = pos;
        (root->list).push_back(pos);
    }
    
    
    void searchword(vector<vector<int>> & res,string& str,int pos,Node* root)
    {
        int sz = str.size();
        for(int i=0; i<sz; i++)
        {
            if( root->id!=-1 && root->id!=pos && isValid(str,i,sz-1) )
            {
                res.push_back({pos ,root->id});
            }
            root = root->child[str[i]-'a'];
            if(root == NULL) return;
        }
        
        for(int i=0; i<(root->list).size() ; i++)
        {
            if( pos == root->list[i] )continue;
            res.push_back({pos,root->list[i]});
        }
    }
    
    
    bool isValid(string& t, int left, int right) 
    {
        while (left < right) if(t[left++] != t[right--]) return 0;
        return 1;
    }
};
```

### 648	Replace Words		Medium	
all words has the prefix is replaced with prefix
use trie
```cpp
struct Node
{
    Node* next[26]; //use pointer is much convenient than using array!
    bool is_leaf;
    Node(bool b=0) {fill(next,next+26,(Node*)0);is_leaf=b;}
};
class Trie {
public:
    /** Initialize your data structure here. */
    Node* root;
    Trie() {
        root=new Node();
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        Node* p=root;
        for(int i=0;i<word.size();i++)        
        {
            int ind=word[i]-'a';
            if(p->is_leaf) break;
            if(!p->next[ind]) p->next[ind]=new Node();
            p=p->next[ind];
            
        }
        p->is_leaf=1;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        Node* p=find(word);
        return p && p->is_leaf; //has to end with a leaf node
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Node* p=find(prefix);
        return p; //does not have to end with a leaf node
    }
    
    Node* find(string word)
    {
        Node* p=root;
        for(int i=0;i<word.size();i++)
        {
            int ind=word[i]-'a';
            if(p->next[ind]==0) return 0;
            p=p->next[ind];
        }
        return p;
    }
    Node* get_root() {return root;}
};
bool cmp(string& a,string &b) {return a.length()<b.length() || (a.length()==b.length() && a<b);}
class Solution {
public:
    string replaceWords(vector<string>& dict, string sentence) {
        Trie t;
        //sort the dict
        sort(dict.begin(),dict.end(),cmp);
        for(int i=0;i<dict.size();i++) t.insert(dict[i]);
        //split the sentence into vector strings
        string newstr;
        string word;
        Node* root=t.get_root();
        Node* p=root;
        int len=sentence.size();
        for(int i=0;i<len;)
        {
            char c=sentence[i];
            //cout<<i<<" ";
            if(c==' ') //a word is done
            {
                newstr+=word+' ';
                p=root;
                word.clear(); //reset
            }
            else
            {
                if(p->next[c-'a'])
                {
                    word+=c;
                    p=p->next[c-'a'];
                    //cout<<"n";
                }
                else //no match stop here
                {
                    if(p->is_leaf) //matched
                    {
                        //cout<<"m";
                        newstr+=word;
                        //need to jump to next word
                        while(++i<len && sentence[i]!=' ');
                    }
                    else
                    {
                        word+=c;
                        while(++i<len && sentence[i]!=' ') word+=sentence[i];
                        newstr+=word;
                        //cout<<'u';
                    }
                    newstr+=sentence[i];
                    p=root;
                    word.clear();
                }
            }
            i++;
            if(i==len) newstr+=word;
                
        }
        return newstr;
    }
};
```

