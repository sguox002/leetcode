medium problems are generally not straighforward and need some thinking and convert to simpler problems.

2. Add Two Numbers
linked list, iterate on both list the same time
```cpp
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int cf=0;
        ListNode* root=0,*prev=0;
        while(l1 || l2 || cf)
        {
            int val=(l1?l1->val:0)+(l2?l2->val:0)+cf;
            cf=val/10;
            ListNode* p=new ListNode(val%10);
            if(l1) l1=l1->next;
            if(l2) l2=l2->next;
            if(!prev) root=p;else prev->next=p;
            prev=p;
        }
        return root;
    }
```    

3. Longest Substring Without Repeating Characters
there are 1 or 2, ...chars similar problems, all can be done using a two pointer window
when a char appears already in the set, we advance to the char after it.
```cpp
    int lengthOfLongestSubstring(string s) {
        int i=0,j=0;
        int ans=0;
        unordered_set<int> ms;
        while(j<s.length())
        {
            int c=s[j];
            if(ms.count(c)) 
            {
                while(s[i]!=s[j]) {ms.erase(s[i]);i++;}
                ms.erase(s[i]);i++;
            }
            ms.insert(s[j]);
            ans=max(ans,j-i+1);
            j++;
        }
        return ans;
    }
```

5. Longest Palindromic Substring
at every position check two options i,i and i,i+1
```cpp
    string longestPalindrome(string s) {
        int ans=1;
        int start_ind=0;
        if(s.empty()) return "";
        for(int i=0;i<s.size()-1;i++)
        {
            check_pal(s,i,i,ans,start_ind);
            check_pal(s,i,i+1,ans,start_ind);
        }
        return s.substr(start_ind,ans);
    }
    
    void check_pal(string& s,int i,int j,int& len,int& ind)
    {
        while(i>=0 && j<s.size() && s[i]==s[j]) i--,j++;
        if(len<j-i-1) {len=j-i-1;ind=i+1;}
    }
```

6. ZigZag Conversion
nothing special, if we notice the period is N+N-2
```cpp
    string convert(string s, int numRows) {
        if(numRows<2) return s;
       vector<string> vs(numRows);
        //0 to n-1, diag(i,i) and then repeat
        //due to the starting and ending, we can use the diag from n-2 to 1
        int i=0,j=0;
        int period=numRows+numRows-2;
        while(i<s.size())
        {
            int tind=i%period;
            if(tind<numRows) vs[tind]+=s[i];
            else
            {
                int t=tind-numRows;
                vs[numRows-2-t]+=s[i];
            }
            i++;
        }
        string zig;
        for(string& ss: vs) zig+=ss;
        return zig;
    }
```

8. String to Integer (atoi)
trivial using stringstream
```cpp
    int myAtoi(string str) {
        stringstream ss(str);
        long long ans=0;
        ss>>ans;
        if(ans>INT_MAX) return INT_MAX;
        if(ans<INT_MIN) return INT_MIN;
        return ans;
    }
```
using long long is necessary to avoid overflow

11. Container With Most Water
two pointers
shrink the region, then need increase the height
two heights are bound by the smaller one.
```cpp
    int maxArea(vector<int>& height) {
        int i=0,j=height.size()-1;
        int maxarea=0;
        while(i<j)
        {
            int minh=min(height[i],height[j]);
            maxarea=max(maxarea,minh*(j-i));
            while(i<j && height[i]<=minh) i++;
            while(i<j && height[j]<=minh) j--;
            //cout<<i<<" "<<j<<" "<<maxarea<<endl;
        }
        return maxarea;
    }
 ```
 
 
12. Integer to Roman
it is not easy to use all the rules, but if we write all thousands, hundreds, tens and ones for all combination, this is really simple
sometimes, if the cases are not much, direct approach could be much simpler
```cpp
string intToRoman(int num) {
    string table[4][10] = {{"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"},
                           {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"},
                           {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"},
                           {"", "M", "MM", "MMM"}
                          };
    string result;
    int count = 0;
    while(num > 0){
        int temp = num % 10;
        result = table[count][temp] + result;
        num /= 10;
        count++;
    }
    return result;
}
````

15. 3Sum
iterate on one and inside is a two pointer problem. This is classical extension to a 2sum problem.
```cpp
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<vector<int>> ans;
        int n=nums.size();
        int i,j,k;
        for(i=0;i<n-2;i++)
        {
            if(i && nums[i]==nums[i-1]) continue;
            j=i+1;
            k=n-1;
            //now it is a two pointer problem
            while(j<k)
            {
                int t=nums[i]+nums[j]+nums[k];
                if(t>0) k--;
                else if(t<0) j++;
                else
                {
                    ans.push_back({nums[i],nums[j],nums[k]});
                    while(j<k && nums[j+1]==nums[j]) j++;
                    while(j<k && nums[k-1]==nums[k]) k--;
                    j++,k--;   
                }
            }
        }
        return ans;
    }
``` 
16. 3Sum Closest
similar to 15
```cpp
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(),nums.end());
        int i,j,k,n=nums.size();
        int mindiff=INT_MAX;
        int ans=0;
        for(i=0;i<n-2;i++)
        {
            j=i+1,k=n-1;
            while(j<k)
            {
                int t=nums[i]+nums[j]+nums[k];
                if(t<target) j++;
                else if(t>target) k--;
                else return target;
                if(mindiff>abs(t-target))
                {
                    mindiff=abs(t-target);
                    ans=t;
                }
            }
        }
        return ans;
    }
 ```
 
 17. Letter Combinations of a Phone Number
 typical dfs+backtracking problem
 ```cpp
     vector<string> mp={"abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"} ;
    vector<string> letterCombinations(string digits) {
        vector<string> vs;
        if(digits.empty()) return vs;
        dfs(digits,vs,string(),0);
        return vs;
    }
    void dfs(string& digits,vector<string>& vs,string t,int start)
    {
        if(start==digits.size()) {vs.push_back(t);return;}
        int tind=digits[start]-'2';
        for(int i=0;i<mp[tind].size();i++)
        {
            t+=mp[tind][i];
            dfs(digits,vs,t,start+1);
            t.pop_back();
        }
    }
```

33. Search in Rotated Sorted Array
binary search
when we divide into two parts [l,mid], [mid,r] there will always one part is sorted.
if the target falls in the sorted region, it is regular binary search 
if the target falls in the non-sorted region, and reducing to a sub problem

19. Remove Nth Node From End of List

1. the node could be the head, add a dummy
2. two passes, first pass to determine number of nodes, second pass to get the n-N node
3. one pass: fast pointer and slow pointer, let the fast point advance n nodes first and then slow and fast advance.
first create a dummy node to connect to the head and then process to the node before the selected node, 
delete it using slow->next=slow->next->next
```cpp
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy=new ListNode(0);
        dummy->next=head;
        ListNode *fast=dummy,*slow=dummy;
        for(int i=1;i<=n+1;i++) fast=fast->next;
        while(fast) fast=fast->next,slow=slow->next;
        slow->next=slow->next->next;
        return dummy->next;
    }
```
One pass generally use two pointers to create a condition which makes things easier.
writing a concise code also reflects clear thought and is important

22. Generate Parentheses
n pairs
dfs+ backtracking
the only constrains is number of ( >= number of ) at any position.
add a ( when cntleft<n
add a ) when cntright+1<=cntleft
```cpp
    vector<string> generateParenthesis(int n) {
        vector<string> vs;
        string ss;
        dfs(vs,ss,0,0,n);
        return vs;
    }
    void dfs(vector<string>& vs,string ss,int i,int j,int n)
    {
        if(i==n && j==n) {vs.push_back(ss);return;}
        if(i<n) {dfs(vs,ss+'(',i+1,j,n);}
        if(i>j) {dfs(vs,ss+')',i,j+1,n);}
    }
```    
backtracking or dfs needs clear thought, otherwise will be very complicated and buggy

24. Swap Nodes in Pairs
add a dummy
```cpp
    ListNode* swapPairs(ListNode* head) {
        if(!head) return head;
        ListNode* dummy=new ListNode(0);
        ListNode *next,*prev,*curr;
        curr=head;prev=dummy;next=head->next;
        prev->next=curr;
        while(next)
        {
            curr->next=next->next;
            prev->next=next;
            next->next=curr;
            //advance one pair
            prev=curr;
            curr=curr->next;
            if(curr) next=curr->next;
            else next=0;
        }
        return dummy->next;
    }
```    

29. Divide Two Integers
no * / can be used. Basically we need use +- to implement this.
the quotient could be very large so we need improve efficiency.
first double the divisor until close to n and get the quotient
then subtract the scale * divisor and repeat
```cpp
    int divide(int dividend, int divisor) {
        int s1=dividend>=0?1:-1;
        int s2=divisor>=0?1:-1;
        long long d1=(long long)s1*dividend;
        long long d2=(long long)s2*divisor;
        if(d2>d1) return 0;
        long long quotient=0;
        while(d2<=d1)
        {
            long long scale=get_scale(d1,d2);
            quotient+=scale;
            d1-=scale*d2;
        }
        long long ans=s1*s2*quotient;
        if(ans>INT_MAX || ans<INT_MIN) ans=INT_MAX;
        return ans;
    }
    long long get_scale(long long d1,long long d2)
    {
        long long scale=1;
        while(d2*2<=d1) {d2*=2;scale*=2;}
        return scale;
    }
```
to avoid overflow use long long for all intermediate

31. Next Permutation
this is a important algorithm implemented in std
from end search the first sorted element
for example 12354, next permutation is 12435
the step: 
find the first sorted position reversely, num[i-1]<num[i]
swap num[i-1] with the first one num[j]>num[i-1] reversely
reverse i to end
```cpp
    void nextPermutation(vector<int>& nums) {
        if(nums.size()<2) return;
        int i=nums.size()-1;
        while(i)
        {
            if(nums[i]>nums[i-1]) break;
            i--;
        }
        if(!i) {reverse(nums.begin(),nums.end());return;}
        //swap i-1 with the smallest >num[i-1]
        int j=nums.size()-1;
        while(nums[j]<=nums[i-1]) j--;
        swap(nums[i-1],nums[j]);
        reverse(nums.begin()+i,nums.end());
    }
```    

33. Search in Rotated Sorted Array
divide and conquer
first cut by half, the mid will either fall into a sorted segment or not sorted segment
sorted: regular binary search
not sorted: a reduced sub problem
```cpp
    int search(vector<int>& nums, int target) {
        if(nums.size()<1) return -1;
        if(nums.size()==1) return target==nums[0]?0:-1;
        return find(nums,0,nums.size()-1,target);
    }
    int find(vector<int>& nums,int l,int r,int target)
    {
        int mid=(l+r)/2;

        if(nums[mid]==target) return mid;
        if(l>=r) return -1;
        if(nums[mid]>=nums[l]) //[l..mid] is ordered, need >= since it may cover itself
        {
            if(nums[mid]>target) //it could be inside front or after
            {
                if(binary_search(nums.begin()+l,nums.begin()+mid,target))
                    return int(lower_bound(nums.begin()+l,nums.begin()+mid,target)-nums.begin());
            }
            return find(nums,mid+1,r,target);
        }
        else //[mid,r] is in order
        {
            if(nums[mid]<target) //the number is in the front rotated
            {
                if(binary_search(nums.begin()+mid+1,nums.end(),target))
                    return int(lower_bound(nums.begin()+mid+1,nums.end(),target)-nums.begin());
            }
            return find(nums,l,mid-1,target);
        }
        return -1;
    }
```
1. edge case: empty or less than 2 elements'
2. exit condition: found or l>=r failure
3. first decide mid in which segment
- mid in first segment, target<=num[mid], then target could be in this segment [l,mid] it could also in the other segment
- mid in second segment: target>num[mid], then it could be in range [l,r] or the other range


34. Find First and Last Position of Element in Sorted Array
equal-range or lower_bound/upper_bound

79. Word Search
1. since we have multiple starting position, we can iterate on each element (if first one does not match it return immediately)
2. at each position do a dfs
dfs:
first if out of boundary or the char in the grid does not match the char in the string, return immediately
second, it should indicate success when we reach the string end
remember we need always have success or failure two exit.
if it matches, first mark it as visited by changing it to '*'
then search its 4 neigborings. Only one fail then try the other
after 4 directions dones, restore the last grid.

```cpp
    bool exist(vector<vector<char>>& board, string word) {
        int m=board.size();
        if(m==0) return 0;
        int n=board[0].size();
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(dfs(board,i,j,word,0)) return 1;
            }
        }
        return 0;
    }
    bool dfs(vector<vector<char>>& board,int i0,int j0,string word,int ind)
    {
        int m=board.size(),n=board[0].size();
        if(i0<0 || j0<0 || i0>=m || j0>=n || word[ind]!=board[i0][j0]) return 0;
        if(ind==word.size()-1) return 1;
        char c=board[i0][j0];
        board[i0][j0]='*';
        bool res=0;
        res=dfs(board,i0-1,j0,word,ind+1);
        if(!res) res=dfs(board,i0+1,j0,word,ind+1);
        if(!res) res=dfs(board,i0,j0-1,word,ind+1);
        if(!res) res=dfs(board,i0,j0+1,word,ind+1);
        board[i0][j0]=c;
        return res;
    }
```
one important thing to note: the backtracking will restore all its nodes when function returns. so next time we can still use it.

212. Word Search II
In this problem we are searching for a list of words. 
naive approach: We can just iterate above process on each word. This is correct. but it will TLE in below cases:
a a a a 
a a a a 
a a a a 
a a a a 
b c d e 
f g h i 
j k l m 
n o p q 
r s t u 
v w x y 
z z z z 
aaaaaaaaaaaaaaaa
aaaaaaaaaaaaaaab
aaaaaaaaaaaaaaac
aaaaaaaaaaaaaaad
aaaaaaaaaaaaaaae
aaaaaaaaaaaaaaaf
aaaaaaaaaaaaaaag
aaaaaaaaaaaaaaah
aaaaaaaaaaaaaaai
aaaaaaaaaaaaaaaj
aaaaaaaaaaaaaaak
aaaaaaaaaaaaaaal
aaaaaaaaaaaaaaam
aaaaaaaaaaaaaaan
aaaaaaaaaaaaaaao
aaaaaaaaaaaaaaap
aaaaaaaaaaaaaaaq
aaaaaaaaaaaaaaar
aaaaaaaaaaaaaaas
aaaaaaaaaaaaaaat
aaaaaaaaaaaaaaau
aaaaaaaaaaaaaaav
aaaaaaaaaaaaaaaw
aaaaaaaaaaaaaaax
aaaaaaaaaaaaaaay
aaaaaaaaaaaaaaaz
aaaaaaaaaaaaaaba
aaaaaaaaaaaaaabb
aaaaaaaaaaaaaabc
aaaaaaaaaaaaaabd
aaaaaaaaaaaaaabe
aaaaaaaaaaaaaabf
aaaaaaaaaaaaaabg
aaaaaaaaaaaaaabh
aaaaaaaaaaaaaabi
aaaaaaaaaaaaaabj
aaaaaaaaaaaaaabk
aaaaaaaaaaaaaabl
aaaaaaaaaaaaaabm
aaaaaaaaaaaaaabn
aaaaaaaaaaaaaabo
aaaaaaaaaaaaaabp
aaaaaaaaaaaaaabq

more efficient way: is from the string algorithm for batch pattern matching using trie data structure. to deal with a lot of patterns
trie is a very useful data structure and we shall master it.
data structure-trie node
functions to build the trie.
```cpp
    struct trie_node
    {
        bool is_end;
        vector<trie_node*> child;
        trie_node(){is_end=0;child=vector<trie_node*>(26);}
    };

    class trie
    {
        trie_node *root;
    public:
        trie_node* get_root() {return root;}
        trie(vector<string>& words)
        {
            root=new trie_node();
            for(string w: words) add_word(w);
        }
        void add_word(string w)
        {
            trie_node* p=root;
            for(char c: w)
            {
                int ind=c-'a';
                if(!p->child[ind]) p->child[ind]=new trie_node();
                p=p->child[ind];
            }
            p->is_end=1;
        }
    }; 
    
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        trie* mytrie=new trie(words);
        trie_node* root=mytrie->get_root();
        set<string> ans;
        int m=board.size(),n=board[0].size();
        for(int i=0;i<m;i++)
            for(int j=0;j<n;j++)
                dfs(board, i, j, root, "", ans);
        
        return vector<string>(ans.begin(),ans.end());
    }
    
    void dfs(vector<vector<char>>& board, int x, int y, trie_node* root, string word, set<string>& result){
        if(x<0||x>=board.size()||y<0||y>=board[0].size() || board[x][y]==' ') return;
        
        if(root->child[board[x][y]-'a'] != NULL){
            word=word+board[x][y];
            root=root->child[board[x][y]-'a']; 
            if(root->is_end) result.insert(word);
            char c=board[x][y];
            board[x][y]=' ';
            dfs(board, x+1, y, root, word, result);
            dfs(board, x-1, y, root, word, result);
            dfs(board, x, y+1, root, word, result);
            dfs(board, x, y-1, root, word, result);
            board[x][y]=c;        
        }
    }    
    
```    
backtracking now changes a bit
1. we go 4 directions (in previous if we succeed the other directions are not going
2. only visited cell is not visited, all other cells are visited.



   


