# Chapter 11 String

## contents

709	To Lower Case	 	Easy	
804	Unique Morse Code Words	 	Easy	
929	Unique Email Addresses	 	Easy	
657	Robot Return to Origin	 	Easy	
557	Reverse Words in a String III	 	Easy	
344	Reverse String	 	Easy	
893	Groups of Special-Equivalent Strings	 	Easy	
800	Similar RGB Color  	Easy	
293	Flip Game  	Easy	
937	Reorder Log Files	 	Easy	
824	Goat Latin	 	Easy	
521	Longest Uncommon Subsequence I	 	Easy	
917	Reverse Only Letters	 	Easy	
788	Rotated Digits	 	Easy	
696	Count Binary Substrings	 	Easy	
520	Detect Capital	 	Easy	
13	Roman to Integer	 	Easy	
606	Construct String from Binary Tree	 	Easy	
387	First Unique Character in a String	 	Easy	
383	Ransom Note	 	Easy	
541	Reverse String II	 	Easy	
551	Student Attendance Record I	 	Easy	
925	Long Pressed Name	 	Easy	
415	Add Strings	 	Easy	
758	Bold Words in String  	Easy	
819	Most Common Word	 	Easy	
345	Reverse Vowels of a String	 	Easy	
38	Count and Say	 	Easy	
459	Repeated Substring Pattern	 	Easy	
67	Add Binary	 	Easy	
443	String Compression	 	Easy	
434	Number of Segments in a String	 	Easy	
20	Valid Parentheses	 	Easy	
680	Valid Palindrome II	 	Easy	
14	Longest Common Prefix	 	Easy	
58	Length of Last Word	 	Easy	
28	Implement strStr()	 	Easy	
686	Repeated String Match	 	Easy	
125	Valid Palindrome	 	Easy	
408	Valid Word Abbreviation  	Easy	
157	Read N Characters Given Read4  	Easy	
859	Buddy Strings	 	Easy	

544	Output Contest Matches  	Medium	
890	Find and Replace Pattern	 	Medium	
537	Complex Number Multiplication	 	Medium	
1016 Binary String With Substrings Representing 1 To N	 	Medium	
791	Custom Sort String	 	Medium	
1023 Camelcase Matching	 	Medium	
647	Palindromic Substrings	 	Medium	
856	Score of Parentheses	 	Medium	
553	Optimal Division	 	Medium	
609	Find Duplicate File in System	 	Medium	
22	Generate Parentheses	 	Medium	
635	Design Log Storage System  	Medium	
1003	Check If Word Is Valid After Substitutions	 	Medium	
12	Integer to Roman	 	Medium	
249	Group Shifted Strings  	Medium	
539	Minimum Time Difference	 	Medium	
49	Group Anagrams	 	Medium	
833	Find And Replace in String	 	Medium	
916	Word Subsets	 	Medium	
536	Construct Binary Tree from String  	Medium	
583	Delete Operation for Two Strings	 	Medium	
816	Ambiguous Coordinates	 	Medium	
809	Expressive Words	 	Medium	
681	Next Closest Time  	Medium	
831	Masking Personal Information	 	Medium	
767	Reorganize String	 	Medium	
17	Letter Combinations of a Phone Number	 	Medium	
966	Vowel Spellchecker	 	Medium	
848	Shifting Letters	 	Medium	
555	Split Concatenated Strings  	Medium	
616	Add Bold Tag in String  	Medium	
186	Reverse Words in a String II  	Medium	
842	Split Array into Fibonacci Sequence	 	Medium	
227	Basic Calculator II	 	Medium	
522	Longest Uncommon Subsequence II	 	Medium	
678	Valid Parenthesis String	 	Medium	
385	Mini Parser	 	Medium	
6 ZigZag Conversion	 	Medium	
161 One Edit Distance  	Medium	
93	Restore IP Addresses	 	Medium	
722	Remove Comments	 	Medium	
43	Multiply Strings	 	Medium	
556	Next Greater Element III	 	Medium	
71	Simplify Path	 	Medium	
3 Longest Substring Without Repeating Characters	 	Medium	
5 Longest Palindromic Substring	 	Medium	
271	Encode and Decode Strings  	Medium	
165	Compare Version Numbers	 	Medium	
91	Decode Ways	 	Medium	
468	Validate IP Address	 	Medium	
151	Reverse Words in a String	 	Medium	
8 String to Integer (atoi)	 	Medium	


761	Special Binary String	 	Hard	
527	Word Abbreviation  	Hard	
632	Smallest Range	 	Hard	
899	Orderly Queue	 	Hard	
159	Longest Substring with At Most Two Distinct Characters  	Hard	
770	Basic Calculator IV	 	Hard	
736	Parse Lisp Expression	 	Hard	
772	Basic Calculator III  	Hard	
340	Longest Substring with At Most K Distinct Characters  	Hard	
730	Count Different Palindromic Subsequences	 	Hard	
72	Edit Distance	 	Hard	
936	Stamping The Sequence	 	Hard	
115	Distinct Subsequences	 	Hard	
591	Tag Validator	 	Hard	
87	Scramble String	 	Hard	
336	Palindrome Pairs	 	Hard	
76	Minimum Window Substring	 	Hard	
97	Interleaving String	 	Hard	
214	Shortest Palindrome	 	Hard	
158	Read N Characters Given Read4 II - Call multiple times  	Hard	
32	Longest Valid Parentheses	 	Hard	
10	Regular Expression Matching	 	Hard	
273	Integer to English Words	 	Hard	
30	Substring with Concatenation of All Words	 	Hard	
68	Text Justification	 	Hard	
44	Wildcard Matching	 	Hard	
564	Find the Closest Palindrome	 	Hard	
126	Word Ladder II	 	Hard	
65	Valid Number	 	Hard

## easy part
### 709	To Lower Case	 	Easy	
trivial, tolower or use transform or foreach

### 804	Unique Morse Code Words	 	Easy	
hashset to remove duplicates

### 929	Unique Email Addresses	 	Easy	
user name and domain and then put into hashset

### 657	Robot Return to Origin	 	Easy	
### 4 directions, udlr. trivial. always move one step

### 557	Reverse Words in a String III	 	Easy	
use space or stringstream to read each word.

### 344	Reverse String	 	Easy	
trivial, two pointer or reverse or rbegin, rend

### 893	Groups of Special-Equivalent Strings	 	Easy	
the problem is really not clear.
odd index can be swapped, even index elements can be swapped.
basically we can sort individually to form a third string and group them together.
```cpp
    int numSpecialEquivGroups(vector<string>& A) {
        //odd index and even index shall form same string
        unordered_map<string,vector<string>> mp;
        for(int i=0;i<A.size();i++)
        {
            mp[getkey(A[i])].push_back(A[i]);
        }
        return mp.size();
    }
    string getkey(string s)
    {
        string odd,even;
        for(int i=0;i<s.size();i++)
        {
            if(i%2) odd+=s[i];
            else even+=s[i];
        }
        sort(odd.begin(),odd.end());
        sort(even.begin(),even.end());
        return even+odd;
    }
```	

### 800	Similar RGB Color  	Easy	
locked
### 293	Flip Game  	Easy	
locked
### 937	Reorder Log Files	 	Easy	
log format: string+numbers or string+strings
reorder: first string type, followed by digits type. string type in order, but digit in original order (ignoring the id)
store digit type in vector, store string type in map
```cpp
    vector<string> reorderLogFiles(vector<string>& logs) {
        map<string, string> mp;
        vector<string>  vs_digit;
        for(int i=0;i<logs.size();i++)
        {
            int ind=logs[i].find_first_of(' ');
            string id=logs[i].substr(0,ind);
            int ind0=ind;
            while(logs[i][ind]==' ') ind++;
            if(isdigit(logs[i][ind])) vs_digit.push_back(logs[i]);
            else
            {
                string content=logs[i].substr(ind0);
                mp[content]=id;
            }
        }
        vector<string> ans;
        for(auto it=mp.begin();it!=mp.end();it++)
            ans.push_back(it->second+it->first);
        for(int i=0;i<vs_digit.size();i++) ans.push_back(vs_digit[i]);
        return ans;
    }
```
	
### 824	Goat Latin	 	Easy	
straightforward

### 521	Longest Uncommon Subsequence I	 	Easy	
if two string the same
if two string not the same: return the longest one

### 917	Reverse Only Letters	 	Easy	
simple using two pointer

### 788	Rotated Digits	 	Easy	
```cpp
    int rotatedDigits(int N) {
        //each digit has to be rotated. so it can only contain 0,1,8,2,5,6,9
        //0,1,8 has to combine with 2,5,6,9
        //we can have three set: 018,2569,and other
        int total=0;
        for(int i=1;i<=N;i++)
        {
            int cnt018=0,cnt2569=0,cnt_other=0;
            string s=to_string(i);
            for(int j=0;j<s.size();j++)
            {
                switch(s[j])
                {
                    case '0':case '1':case '8':cnt018++;break;
                    case '2':case '5':case '6':case '9':cnt2569++;break;
                    default:cnt_other++;break;
                }
                if(cnt_other) break;
            }
            total+=(!cnt_other && cnt2569);
        }
        return total;
    }
```
	
### 696	Count Binary Substrings	 	Easy	
count substring with same number of consecuative 0 and 1s
First, I count the number of 1 or 0 grouped consecutively.
For example "0110001111" will be [1, 2, 3, 4].

Second, for any possible substrings with 1 and 0 grouped consecutively, the number of valid substring will be the minimum number of 0 and 1.
For example "0001111", will be min(3, 4) = 3, ("01", "0011", "000111")

```cpp
    int countBinarySubstrings(string s) {
        int cur = 1, pre = 0, res = 0;
        for (int i = 1; i < s.size(); i++) {
            if (s[i] == s[i - 1]) cur++;
            else {
                res += min(cur, pre);
                pre = cur;
                cur = 1;
            }
        }
        return res + min(cur, pre);
    }
```
	
### 520	Detect Capital	 	Easy
```cpp
    bool detectCapitalUse(string word) {
        string upp(word.size(),0),low(word.size(),0);
        transform(word.begin(),word.end(),upp.begin(),::toupper);
        transform(word.begin(),word.end(),low.begin(),::tolower);
        if(word==upp || word==low) return 1;
        low[0]=::toupper(low[0]);
        return word==low;
    }
```
	
### 13	Roman to Integer	 	Easy	
see math

### 606	Construct String from Binary Tree	 	Easy	
see tree


### 387	First Unique Character in a String	 	Easy	
store the first index, and count into a hashmap or vector

### 383	Ransom Note	 	Easy	
hashmap counting each char

### 541	Reverse String II	 	Easy	
Given a string and an integer k, you need to reverse the first k characters for every 2k characters counting from the start of the string. If there are less than k characters left, reverse all of them. If there are less than 2k but greater than or equal to k characters, then reverse the first k characters and left the other as original.
```cpp
    string reverseStr(string s, int k) {
        int start=0;
        //inverse k characters and jump by 2k
        if(k>=s.size())
        {
            reverse(s.begin(),s.end());
            return s;
        }
        //if less than k reverse all of it
        for(int i=0;i<s.size()-k;i+=2*k)
        {
            reverse(s.begin()+start,s.begin()+start+k);
            start+=2*k;
        }
        reverse(s.begin()+start,s.end());
        return s;
    }
```
or recursive

### 551	Student Attendance Record I	 	Easy	
just count A and L

### 925	Long Pressed Name	 	Easy
two pointer and compare each char's length
	
### 415	Add Strings	 	Easy	
straightforward

### 758	Bold Words in String  	Easy	
locked

### 819	Most Common Word	 	Easy	
with banned list.
remove non-letter chars
```cpp
    string mostCommonWord(string paragraph, vector<string>& banned) {
        transform(paragraph.begin(),paragraph.end(),paragraph.begin(),::tolower);
        stringstream ss(paragraph);
        unordered_map<string,int> mp;
        unordered_set<string> dict(banned.begin(),banned.end());
        string w,freqw;
        int maxcnt=0;
        while(ss>>w) 
        {
            if(!isalpha(w.back())) w.pop_back();
            if(!dict.count(w)) 
            {
                mp[w]++;
                if(maxcnt<mp[w])
                {
                    maxcnt=mp[w];
                    freqw=w;
                }
            }
        }
        return freqw;
    }
```	

### 345	Reverse Vowels of a String	 	Easy	
simple

### 38	Count and Say	 	Easy	
simple

### 459	Repeated Substring Pattern	 	Easy	
Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.
double it and remove first and last char to see if the string is inside.
```cpp
    bool repeatedSubstringPattern(string s) {
        string ss=s+s;
        int n=ss.size();
        //check ss.substr(1) contains s
        return ss.substr(1,n-2).find(s)!=string::npos;
    }
```
	
### 67	Add Binary	 	Easy	

### 443	String Compression	 	Easy	

### 434	Number of Segments in a String	 	Easy	
stringstream

### 20	Valid Parentheses	 	Easy	
()[]{}
use three counters anytime cnt>=0
left ++, right --

### 680	Valid Palindrome II	 	Easy	
reverse and delete conflict from s or rs
```cpp
    bool validPalindrome(string s) {
       string rs=s;
        reverse(rs.begin(),rs.end());
        if(s==rs) return 1;
        int n=s.size();
        for(int i=0;i<s.size();i++)
        {
            if(s[i]!=rs[i]) //either delete the char in s or rs
            {
                return s.substr(0,i)+s.substr(i+1)==rs.substr(0,n-i-1)+rs.substr(n-i) ||
                    rs.substr(0,i)+rs.substr(i+1)==s.substr(0,n-i-1)+s.substr(n-i);
            }
        }
        return 1;
    }
```
	

### 14	Longest Common Prefix	 	Easy	
simple

### 58	Length of Last Word	 	Easy	
simple using stringstream or just brutal force from right.

### 28	Implement strStr()	 	Easy	
find the string position

### 686	Repeated String Match	 	Easy	
Given two strings A and B, find the minimum number of times A has to be repeated such that B is a substring of it. If no such solution, return -1.

For example, with A = "abcd" and B = "cdabcdab".

Return 3, because by repeating A three times (“abcdabcdabcd”), B is a substring of it; and B is not a substring of A repeated two times ("abcdabcd").
```cpp
    int repeatedStringMatch(string A, string B) {
        string t=A;
        int n=B.length()/A.length()+2;
        for(int i=1;i<=n;i++,t+=A)
            if(t.find(B)!=string::npos) return i;
        return -1;
        
    }
```
	
### 125	Valid Palindrome	 	Easy	
ignore non-letter and non-digit chars

### 408	Valid Word Abbreviation  	Easy	
locked
### 157	Read N Characters Given Read4  	Easy	
locked

### 859	Buddy Strings	 	Easy	
swap one time in A and then A==B
```cpp
    bool buddyStrings(string A, string B) {
        if(A.size()!=B.size()) return 0;
        //same string, if we can swap and no change
        if(A==B)
        {
            vector<int> v(26);
            for(char c: A) {v[c-'a']++;if(v[c-'a']>1) return 1;}
            return 0;
        }
        //find the first mismatch from both end
        int i=0,j=A.size()-1;
        while(A[i]==B[i]) i++;
        while(A[j]==B[j]) j--;
        swap(A[i],A[j]);
        return A==B;
    }
```	

## medium

### 544	Output Contest Matches  	Medium	
locked

### 890	Find and Replace Pattern	 	Medium	
all mapped to fixed pattern mee to abb
```cpp
    vector<string> findAndReplacePattern(vector<string>& words, string pattern) {
        pattern=strmap(pattern);
        vector<string> ans;
        for(int i=0;i<words.size();i++)
        {
            //cout<<words[i]<<"->"<<strmap(words[i])<<endl;
            if(strmap(words[i])==pattern) ans.push_back(words[i]);
        }
        return ans;
    }
    string strmap(string str)
    {
        int j=0;
        unordered_map<char,char> ms;
        for(int i=0;i<str.size();i++)
        {
            if(ms.count(str[i])) str[i]=ms[str[i]];
            else {ms[str[i]]=j+'a';str[i]=j+'a';j++;}
        }
        return str;
    }
```	

### 537	Complex Number Multiplication	 	Medium	
simple
### 1016 Binary String With Substrings Representing 1 To N	 	Medium	
direct approach
convert to 32 bit binary string and trim the leading zeros and to see if it is a substr of the input string

### 791	Custom Sort String	 	Medium	
s is sorted using some rule, sort B using the same rule. not appeared in A does not care.
approach 1: using two hashmaps a-b and b-a
approach 2: cnt each chars in T into a hashmap and then check S one char by one char and append 

```cpp
    string customSortString(string S, string T) {
        unordered_map<char,int> mp1;//char, count
        for(int i=0;i<T.size();i++) mp1[T[i]]++;
        string s;
        for(int i=0;i<S.size();i++)        
        {
            char c=S[i];
            if(mp1.count(c)) 
            {
                s.append(mp1[c],c);
                mp1.erase(c);
            }
        }
        //remaining characters, what ever sequence
        for(auto it=mp1.begin();it!=mp1.end();it++)
            s.append(it->second,it->first);
        return s;
    }
```
	
### 1023 Camelcase Matching	 	Medium	
capital letters are treated as camelcase
```cpp
    vector<bool> camelMatch(vector<string>& queries, string pattern) {
        //one by one search: greedy, all left shall be lowercase
        vector<bool> ans(queries.size());
        for(int i=0;i<queries.size();i++)
        {
            ans[i]=match(queries[i],pattern);
        }
        return ans;
    }
    bool match(string& w,string& p)
    {
        int i=0,j=0;
        while(j<p.size() && i<w.size())
        {
            if(w[i]==p[j]) j++;
            else if(isupper(w[i])) return 0;
            i++;
        }
        while(i<w.size()) if(isupper(w[i++])) return 0;
        return j==p.size();
    }
```
	
### 647	Palindromic Substrings	 	Medium	
count number of pal-substrings
dp
```cpp
    int countSubstrings(string s) {
        //dp solution: using 2d approach
        //dp[i,j]=self+dp[i,j-1]+dp[i+1,j]-dp[i+1,j-1]
        //dp[i,j] is the number of p-string for S(i...j)
        //i: reverse, j: normal
        int n=s.size();
        vector<vector<int>> dp(n,vector<int>(n));
        for(int i=0;i<n;i++) dp[i][i]=1; //itself is a p-string
        
        for(int i=n-2;i>=0;i--)
        {
            for(int j=i+1;j<n;j++) //j>i must be met
            {
                int self=isPalindrome(s,i,j);
                dp[i][j]=self+dp[i][j-1]+dp[i+1][j]-dp[i+1][j-1];
            }
        }
        return dp[0][n-1];
    }
    int isPalindrome(string& s,int i,int j)
    {
        //two pointer early exit
        while(i<j) 
        {
            if(s[i]==s[j]) {i++,j--;} else return 0;
        }
        return 1;
    }
```

### 856	Score of Parentheses	 	Medium	
Given a balanced parentheses string S, compute the score of the string based on the following rule:

() has score 1
AB has score A + B, where A and B are balanced parentheses strings.
(A) has score 2 * A, where A is a balanced parentheses string.

```cpp
    int scoreOfParentheses(string S) {
        stack<int> st;
        st.push(0);
        for(int i=0;i<S.size();i++)
        {
            if(S[i]=='(') st.push(0);
            else
            {
                int score=st.top();
                st.pop();
                if(score==0) score=1;
                else score*=2;
                st.top()+=score;
            }
        }
        return st.top();
    }
```	
### 553	Optimal Division	 	Medium	
simple
### 609	Find Duplicate File in System	 	Medium	
file: path, contents
if two files has same contents, they are considered duplicate
output the duplicate files path and name
hashmap: content vs path
pretty straightforward
```cpp
    vector<vector<string>> findDuplicate(vector<string>& paths) {
        //file structure is in tree structure
        //read a part of its file and use a hashvalue for the file. (generally use a hash for the whole file)
        //make a map for the file and its hash value. this is exactly what the git does
        unordered_map<string,vector<string>> mymap;
        for(int i=0;i<paths.size();i++)
        {
            int ind=paths[i].find_first_of(" \t");
            string dir=paths[i].substr(0,ind+1);//include the space
            dir[ind]='/'; //replace it with /
            //cout<<dir<<endl;
            //retrieve all files under the directory and using its contents for the key!
            int start=ind+1;
            while(1)
            {
                //get filename and file content
                //cout<<start<<":"<<ind<<":";
                string fname,fcontent;
                int j=paths[i].find_first_of("(",start);
                int k=paths[i].find_first_of(")",start);
                fname=paths[i].substr(start,j-start);
                fcontent=paths[i].substr(j+1,k-j-1);
                //cout<<dir<<":"<<fname<<":"<<fcontent<<endl;
                mymap[fcontent].push_back(dir+fname);
                ind=paths[i].find_first_of(" \t",start); //to see if we have more
                if(ind!=string::npos) start=ind+1;
                else break;
            }
            //the last one 
        }
        vector<vector<string>> res;
        for(auto it=mymap.begin();it!=mymap.end();it++)
        {
            if(it->second.size()>1)
            {
                res.push_back(it->second);
            }
        }
        return res;
    }
```
	
### 22	Generate Parentheses	 	Medium	
recursive or dfs
only constraints left>=right
```cpp
    vector<string> generateParenthesis(int n) {
        vector<string> vs;
        string s;
        int lcount=0,rcount=0;
        build(vs,n,lcount,rcount,s);
        return vs;
    }
    void build(vector<string>& vs,int n,int lcount,int rcount,string s)
    {
        if(s.size()==2*n) {vs.push_back(s);return;}
        if(lcount<n) build(vs,n,lcount+1,rcount,s+"(");
        if(lcount>rcount) build(vs,n,lcount,rcount+1,s+")");
    }
```
	
### 635	Design Log Storage System  	Medium	
locked

### 1003	Check If Word Is Valid After Substitutions	 	Medium	
We are given that the string "abc" is valid.

From any valid string V, we may split V into two pieces X and Y such that X + Y (X concatenated with Y) is equal to V.  (X or Y may be empty.)  Then, X + "abc" + Y is also valid.

If for example S = "abc", then examples of valid strings are: "abc", "aabcbc", "abcabc", "abcabcababcc".  Examples of invalid strings are: "abccba", "ab", "cababc", "bac".

Return true if and only if the given string S is valid.

approach: reverse the process by removing valid string from S
```cpp
    bool isValid(string S) {
       //remove string when valid is found
        deque<char> dq;
        int i=0;
        while(i<S.length())
        {
            if(dq.size()>1 && S[i]=='c')
            {
                if(dq.back()=='b' && dq[dq.size()-2]=='a')
                {
                    dq.pop_back();
                    dq.pop_back();
                }
                else dq.push_back(S[i]);
            }
            else dq.push_back(S[i]);
            i++;
        }
        return dq.empty();
    }
```
	

### 12	Integer to Roman	 	Medium	
see math
### 249	Group Shifted Strings  	Medium	
locked
### 539	Minimum Time Difference	 	Medium	

time is in the format of 24 hours. this is unwrap
we just need to convert to integer and sort. The last one and first one diff add a 24 hour


### 49	Group Anagrams	 	Medium	
sort the string as the key

### 833	Find And Replace in String	 	Medium	
give a list of index and substrings to be replaced with given target strings
string to expand or shrink, so we need do it from the right side.
sort the index but keep the original index information.
```cpp
    string findReplaceString(string S, vector<int>& indexes, vector<string>& sources, vector<string>& targets) {
       int start=0;
        string ans;
        vector<vector<int>> mp;
        for(int i=0;i<indexes.size();i++) mp.push_back({indexes[i],i});
        sort(mp.begin(),mp.end());
        
        for(auto t: mp)
        {
            //int ind=indexes[i],len=sources[i].size();
            int ind=t[0],len=sources[t[1]].size();
            if(S.substr(ind,len)==sources[t[1]])
            {
                ans+=S.substr(start,ind-start)+targets[t[1]];
                start=ind+len;
            }
        }
        if(start<S.size()) ans+=S.substr(start);
        return ans;
    }
```	

### 916	Word Subsets	 	Medium	
We are given two arrays A and B of words.  Each word is a string of lowercase letters.

Now, say that word b is a subset of word a if every letter in b occurs in a, including multiplicity.  For example, "wrr" is a subset of "warrior", but is not a subset of "world".

Now say a word a from A is universal if for every b in B, b is a subset of a. 

Return a list of all universal words in A.  You can return the words in any order.

ie. word include every letter in B including duplicates
so we can just count the hashmap of letters in B and to see if word in A covers the map
pretty straightforward
```cpp
    vector<string> wordSubsets(vector<string>& A, vector<string>& B) {
        vector<int> bmp(26);
        //reduce B into one word
        for(int i=0;i<B.size();i++)
        {
            vector<int> tmp(26);
            for(int j=0;j<B[i].size();j++) tmp[B[i][j]-'a']++;
            for(int j=0;j<26;j++)
            {
                bmp[j]=max(bmp[j],tmp[j]);
            }
        }
        
        vector<string> ans;
        for(int i=0;i<A.size();i++)
        {
            vector<int> tmp(26);
            for(int j=0;j<A[i].size();j++) tmp[A[i][j]-'a']++;
            bool found=1;
            for(int j=0;j<26;j++)
            {
                if(bmp[j]>tmp[j]) {found=0;break;}
            }
            if(found) ans.push_back(A[i]);
        }
        return ans;
    }
```


### 536	Construct Binary Tree from String  	Medium	
see tree, locked

### 583	Delete Operation for Two Strings	 	Medium	
min number of deletion to make two string the same
see dp for edit distance

### 816	Ambiguous Coordinates	 	Medium	
coordinates are removed space, comma, points.
restore the coordinates return all valid coordinates
intuition:
1. we need have one comma and one space exactly
2. we can have 0/1/2 decimal points total
3. 0.0, 00, 00.01 are not allowed

this is not easy at all
approach:
1. first separate the string into two parts with , and space
2. the only requirement, we cannot have all zeros in any part
3. process each part and add into answer. To add 0 or 1 decimal point is much simpler.


```cpp
    vector<string> ambiguousCoordinates(string S) {
        //add a , in the string, for the separated half, we can add a . to each of them
        //first we separate it using ,
        //only rule: it cannot be all zero, except it has one zero
        vector<string> vs;
        string s=S.substr(1,S.length()-2); //remove ()
        //cout<<s<<endl;
        for(int i=1;i<=s.length()-1;i++)
        {
            string a=s.substr(0,i);
            string b=s.substr(i);
            //cout<<"check "<<a<<" "<<b<<endl;
            helper(a,b,vs);
        }
        return vs;
    }
    
    void helper(string a,string b,vector<string>& vs)
    {
        if(!is_legal(a) || !is_legal(b) ) return;
        //both are legal, then we can add decimal to each of the string
        //zero or one decimal point to the string
        vector<string> va=add_decimal(a);
        vector<string> vb=add_decimal(b);
        //cout<<"add decimal:"<<b<<":"<<vb.size()<<endl;
        for(int i=0;i<va.size();i++)
        {
            for(int j=0;j<vb.size();j++) vs.push_back("("+va[i]+", "+vb[j]+")");
        }
    }
    bool is_legal(string& a)
    {
        if(stoi(a)==0) return a.size()==1;
        //if(a.size()>1 && a[0]=='0') return 0;
        return 1;
    }
    
    bool is_validInt(string& a) //the decimal 
    {
        if(a.size()>1 && a[0]=='0') return 0;
        return 1;
    }
    bool is_validDecimal(string& s) //after the ., cannot be zero at the end
    {
        return s.back()!='0';
    }
    vector<string> add_decimal(string& s)
    {
        vector<string> ans;
        if(s.size()==1 || s[0]!='0') ans.push_back(s); //if no decimal is legal?
        //note the last digit after decimal cannot be 0
        for(int i=1;i<=s.size()-1;i++)
        {
            string a=s.substr(0,i),b=s.substr(i);
            if(b.back()=='0') continue;
            if(is_validInt(a) && is_validDecimal(b)) 
                ans.push_back(a+"."+b);
        }
        return ans;
    }
```	

### 809	Expressive Words	 	Medium	
two pointer
```cpp
    int expressiveWords(string S, vector<string>& words) {
        int ans=0;
        for(string w: words)
        {
            if(isStretchy(S,w)) ans++;
        }
        return ans;
    }
    bool isStretchy(string& s,string& w)
    {
        int m=s.size(),n=w.size();
        //cout<<s<<" "<<w<<endl;
        if(m<=n) return 0;
        int i=0,j=0,cnta=1,cntb=1;
        while(i<m && j<n)
        {
            while(i<m-1 && s[i]==s[i+1]) {i++;cnta++;}
            while(j<n-1 && w[j]==w[j+1]) {j++,cntb++;}
            //cout<<s[i]<<" "<<cnta<<", "<<w[j]<<" "<<cntb<<endl;
            if(cntb!=cnta) 
            {
                if(cnta<cntb || cnta<3 || s[i]!=w[j]) return 0;
            }
            cntb=cnta=1;
            i++,j++;
        }
        return i==m && j==n;
    }
```
	
### 681	Next Closest Time  	Medium	
locked

### 831	Masking Personal Information	 	Medium	
email: convert to first char+5*+last char in the username part
phone: local number ***-***-last 4 digits, with country code: +***-***-***-last 4 digit
() and other extra char shall be removed.
if country code has only two chars we need convert to +**

straightforward
```cpp
    string maskPII(string S) {
        //email contains a @, otherwise it is phone number
        transform(S.begin(),S.end(),S.begin(),::tolower);
        size_t pos=S.find('@');
        string ss;
        if(pos==string::npos) //it is phone number
        {
            for(int i=0;i<S.size();i++) 
            {
                //cout<<S[i]<<endl;
                if(isdigit(S[i])) {ss+=S[i];}
            }
            //cout<<ss;
            string t=ss;
            if(t.size()<=10) ss="***-***-"+t.substr(t.size()-4);
            else {ss="+";ss.append(t.size()-10,'*');ss+="-***-***-"+t.substr(t.size()-4);}
            
        }
        else
        {
            ss+=S[0];
            ss+="*****";
            ss+=S[pos-1]+S.substr(pos);
        }
        return ss;
    }
```	
### 767	Reorganize String	 	Medium	
reorganize so that neighboring chars are different
see greedy using pq, always choose the top 2

### 17	Letter Combinations of a Phone Number	 	Medium	
see dfs

### 966	Vowel Spellchecker	 	Medium	
spell checker:
first rule: capitalization, if word match, return the case same word
2nd rule: if replaced one vowel and match, return the dict word
approach:
we make two hashmap:
one is nocase, one is novowel to map the original string
another hashmap for the dictionary

```cpp
    vector<string> spellchecker(vector<string>& wordlist, vector<string>& queries) {
        unordered_map<string,string> mp_novow,mp_nocase; //word vs no vowels
        unordered_set<string> dict(wordlist.begin(),wordlist.end());
        char vow[]={'a','e','i','o','u'};
        unordered_set<char> vowels(vow,vow+6);
        for(int i=0;i<wordlist.size();i++)
        {
            //remove the vowels
            string s1,s2;
            for(int j=0;j<wordlist[i].size();j++)
            {
                char c=tolower(wordlist[i][j]);
                if(!vowels.count(c)) s1+=c;else s1+='*';
                s2+=c;
            }
            if(!mp_novow.count(s1))mp_novow[s1]=wordlist[i];//only need the first
            if(!mp_nocase.count(s2))mp_nocase[s2]=wordlist[i]; //only need the first
        }
        vector<string> ans(queries.size());
        for(int i=0;i<queries.size();i++)
        {
            if(dict.count(queries[i])) ans[i]=queries[i]; //exact match
            else //check if case match
            {
                string s1,s2;
                for(int j=0;j<queries[i].size();j++)
                {
                    char c=tolower(queries[i][j]);
                    s1+=c;
                    if(!vowels.count(c)) s2+=c;else s2+='*';
                }
                if(mp_nocase.count(s1)) ans[i]=mp_nocase[s1];
                else //case not match, check if no vowel match
                {
                    if(mp_novow.count(s2)) ans[i]=mp_novow[s2];//no vowels must also match position
                    else ans[i]="";
                }
            }
        }
        return ans;
    }
```
	
### 848	Shifting Letters	 	Medium	
We have a string S of lowercase letters, and an integer array shifts.

Call the shift of a letter, the next letter in the alphabet, (wrapping around so that 'z' becomes 'a'). 

For example, shift('a') = 'b', shift('t') = 'u', and shift('z') = 'a'.

Now for each shifts[i] = x, we want to shift the first i+1 letters of S, x times.

Return the final string after all such shifts to S are applied.

Input: S = "abc", shifts = [3,5,9]
Output: "rpl"
Explanation: 
We start with "abc".
After shifting the first 1 letters of S by 3, we have "dbc".
After shifting the first 2 letters of S by 5, we have "igc".
After shifting the first 3 letters of S by 9, we have "rpl", 

The ith character is shifted shifts[i] + shifts[i+1] + ... + shifts[shifts.length - 1] times. That's because only operations at the ith operation and after, affect the ith character.

Let X be the number of times the current ith character is shifted. Then the next character i+1 is shifted X - shifts[i] times.

For example, if S.length = 4 and S[0] is shifted X = shifts[0] + shifts[1] + shifts[2] + shifts[3] times, then S[1] is shifted shifts[1] + shifts[2] + shifts[3] times, S[2] is shifted shifts[2] + shifts[3] times, and so on.

In general, we need to do X -= shifts[i] to maintain the correct value of X as we increment i.


```cpp
    string shiftingLetters(string S, vector<int>& shifts) {
        int n=shifts.size();
        for(int i=n-2;i>=0;i--) shifts[i]+=shifts[i+1]%26;
        for(int i=0;i<S.size();i++) 
        {
            int t=(S[i]-'a'+shifts[i])%26+'a';
            S[i]=t;
        }
        return S;
    }
```	
### 555	Split Concatenated Strings  	Medium	
locked
### 616	Add Bold Tag in String  	Medium	
locked
### 186	Reverse Words in a String II  	Medium	
locked
### 842	Split Array into Fibonacci Sequence	 	Medium	
see greedy

### 227	Basic Calculator II	 	Medium	
non negative numbers only with +-*/

```cpp
int calculate(string s) {
    istringstream in('+' + s + '+');
    long long total = 0, term = 0, n;
    char op;
    while (in >> op) 
    {
        if (op == '+' || op == '-') //treat +/- as *1/-1
        {
            total += term; //previous results
            in >> term;
            term *= 44 - op;//+ 43, - 45 equiv to op=='+'?1:-1
        } 
        else //*/operator
        {
            in >> n;//read a integer
            if (op == '*')  term *= n;
            else term /= n;
        }
    }
    return total;
}
```
add a + in the front and at the end for convenience
use stringstream to read char and int.

### 522	Longest Uncommon Subsequence II	 	Medium	
now there are a list of words
Sort the strings in the reverse order. If there is not duplicates in the array, then the longest string is the answer.

But if there are duplicates, and if the longest string is not the answer, then we need to check other strings. But the smaller strings can be subsequence of the bigger strings.
For this reason, we need to check if the string is a subsequence of all the strings bigger than itself. If it's not, that is the answer.
```
    public int findLUSlength(String[] strs) {
        Arrays.sort(strs, new Comparator<String>() {
            public int compare(String o1, String o2) {
                return o2.length() - o1.length();
            }
        });
        
        Set<String> duplicates = getDuplicates(strs);
        for(int i = 0; i < strs.length; i++) {
            if(!duplicates.contains(strs[i])) {
                if(i == 0) return strs[0].length();
                for(int j = 0; j < i; j++) {
                    if(isSubsequence(strs[j], strs[i])) break;
                    if(j == i-1) return strs[i].length();
                }
            }
        }
        return -1;
    }
    
    public boolean isSubsequence(String a, String b) {
        int i = 0, j = 0;
        while(i < a.length() && j < b.length()) {
            if(a.charAt(i) == b.charAt(j)) j++;
            i++;
        }
        return j == b.length();
    }
    
    private Set<String> getDuplicates(String[] strs) {
        Set<String> set = new HashSet<String>();
        Set<String> duplicates = new HashSet<String>();
        for(String s : strs) {
            if(set.contains(s)) duplicates.add(s);
            set.add(s);
        }
        return duplicates;
    }
```	
### 678	Valid Parenthesis String	 	Medium	
now include * which can act as left or right or nothing
think in dp or recursive
```cpp
    bool checkValidString(string s) {
        //using stack, 
        return is_validstring(s,0,0,0);
    }
    bool is_validstring(const string& s,int i,int nleft,int nright)
    {
        if(nleft<nright) return 0;
        if(i==s.length())
        {
            return nleft==nright;
        }
        if(s[i]=='(') return is_validstring(s,i+1,nleft+1,nright);
        if(s[i]==')') return is_validstring(s,i+1,nleft,nright+1);
        else //use it as left, right or nothing
        {
            return is_validstring(s,i+1,nleft,nright) || is_validstring(s,i+1,nleft+1,nright) || is_validstring(s,i+1,nleft,nright+1);
        }
    }
```
	
### 385	Mini Parser	 	Medium	
Given a nested list of integers represented as a string, implement a parser to deserialize it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Note: You may assume that the string is well-formed:

String is non-empty.
String does not contain white spaces.
String contains only digits 0-9, [, - ,, ].
serialization and deserialization is important to store/transfer/restore the object.

```cpp
    NestedInteger deserialize(string s) {
        istringstream in(s);
        return deserialize(in);
    }
private:
    NestedInteger deserialize(istringstream &in) {
        int number;
        if (in >> number)  return NestedInteger(number);
        in.clear();//clear error flag (not a number)
        in.get();//get a char
        NestedInteger list;
        while (in.peek() != ']') 
        {
            list.add(deserialize(in));
            if (in.peek() == ',')
                in.get();
        }
        in.get();
        return list;
    }
```


### 6 ZigZag Conversion	 	Medium	
```cpp
    string convert(string s, int numRows) {
        vector<string> vs(numRows);
		int cnt=0;
		int len=s.size();
		//periodically one zig and one zag forms one period
		while(cnt<len)
		{
		for(int i=0;i<numRows && cnt<len;i++,cnt++) vs[i].push_back(s[cnt]);
		for(int i=0;i<numRows-2 && cnt<len;i++,cnt++) vs[numRows-2-i].push_back(s[cnt]);
		}
		string res;
		return accumulate(vs.begin(),vs.end(),res);//accumulate is in numeric
    }
```	
### 161 One Edit Distance  	Medium	
locked

### 93	Restore IP Addresses	 	Medium	
direct approach sometimes is more direct and faster
```cpp
    vector<string> restoreIpAddresses(string s) {
        vector<string> ret;
        string ans;
        
        for (int a=1; a<=3; a++)
        for (int b=1; b<=3; b++)
        for (int c=1; c<=3; c++)
        for (int d=1; d<=3; d++)
            if (a+b+c+d == s.length()) {
                int A = stoi(s.substr(0, a));
                int B = stoi(s.substr(a, b));
                int C = stoi(s.substr(a+b, c));
                int D = stoi(s.substr(a+b+c, d));
                if (A<=255 && B<=255 && C<=255 && D<=255)
                    if ( (ans=to_string(A)+"."+to_string(B)+"."+to_string(C)+"."+to_string(D)).length() == s.length()+3)
                        ret.push_back(ans);
            }    
        
        return ret;
    }
```

### 722	Remove Comments	 	Medium	
regex
```cpp
    vector<string> removeComments(vector<string>& source) {
        stringstream ss;
        const char* delim="\n";
        copy(source.begin(),source.end(),ostream_iterator<string>(ss,delim));
        string s=ss.str();
        //cout<<s;
        regex e("//.*|/\\*(.|\n)*?\\*/");
        string ns=regex_replace(s,e,"");
        //cout<<ns;
        //split the string into vector using tokened string
        ss.str(ns);
        vector<string> ans;
        while(ss.good())
        {
            string token;
            getline(ss,token,'\n');
            if(!token.empty())
            ans.push_back(token);
        }
        //copy(ans.begin(),ans.end(),ostream_iterator<string>(cout,"\n"));
        return ans;
    }
```
first convert the vector of strings into one string add \n
second using regular expression to remove /*...*/ or \\to the end

other approach: brutal force
```cpp
    vector<string> removeComments(vector<string>& s) {
        vector<string> ans;
        bool inBlock = false;
        string sf;
        for (auto &t:s) {
            for (int i = 0; i < t.size();) {
                if (!inBlock) {
                    if (i + 1 == t.size()) sf += t[i++];
                    else {
                        string m = t.substr(i,2);
                        if (m == "/*") inBlock = 1, i+=2;
                        else if (m == "//") break;
                        else sf += t[i++];
                    }
                }
                else {
                    if (i + 1 == t.size()) i++;
                    else {
                        string m = t.substr(i,2);
                        if (m == "*/") inBlock = 0, i+=2;
                        else i++;
                    }
                }
            }
            if (sf.size() && !inBlock) ans.push_back(sf), sf = "";
        }
        return ans;
    }
```
### 43	Multiply Strings	 	Medium	
see math

### 556	Next Greater Element III	 	Medium	
convert to string and get next_permutation.

```cpp
    int nextGreaterElement(int n) {
        string s=to_string(n);
        next_permutation(s.begin(),s.end());
        int res=stoul(s);
        return (res>n)?res:-1;
    }
```	
### 71	Simplify Path	 	Medium	
using stack
```cpp
    string simplifyPath(string path) {
     //. stay .. go back. other go down, get the final path
        //this is a stack problem
        //slash is the separater, multiple slash is the same
        //possibly the last one is not /
        path+="/";
        vector<string> st;
        int start=0,end=0;
        while(1)
        {
            end=path.find_first_of('/',start+1);
            if(end==string::npos) break;
            string s=path.substr(start+1,end-start-1);
            if(s.empty() || s==".");
            else if(s=="..") {if(!st.empty()) st.pop_back();}
            else st.push_back(s);
            start=end;
        }
        
        string res;
        
        for(int i=0;i<st.size();i++)
           res+="/"+st[i];
        if(st.empty()) res="/";
        return res;
    }
```

### 3 Longest Substring Without Repeating Characters	 	Medium	
this could have multiple approaches
first if we use ith char as the ending, we can get the length of the segment
using a hashmap to record previous position
```cpp
int lengthOfLongestSubstring(string s) {
    // for ASCII char sequence, use this as a hashmap
    vector<int> charIndex(256, -1);
    int longest = 0, m = 0;

    for (int i = 0; i < s.length(); i++) {
        m = max(charIndex[s[i]] + 1, m);    // automatically takes care of -1 case
        charIndex[s[i]] = i;
        longest = max(longest, i - m + 1);
    }
    return longest;
}
```
m is the max index with no repeated. similar to two pointer approach.

### 5 Longest Palindromic Substring	 	Medium	
dp
```cpp
    string longestPalindrome(string s) {
       //dp solution using i j
        int n=s.length();
        vector<vector<int>> dp(n,vector<int>(n)); //initialize to 0
        //dp(i,j)=length of p-string
        int ans=0;//the max length
        int start=0;
        for(int i=0;i<n;i++) {dp[i][i]=1;ans=1;}
        for(int i=0;i<n-1;i++) 
        {
            if(s[i]==s[i+1]) {dp[i][i+1]=2,ans=2,start=i;}
        }
        for(int i=n-3;i>=0;i--) //since it needs i+1
        {
            for(int j=i+2;j<n;j++) //since it needs j-1
            {
                if(s[i]==s[j] && dp[i+1][j-1]) dp[i][j]=dp[i+1][j-1]+2;
                if(ans<dp[i][j]) ans=dp[i][j],start=i;
            }
        }
        return s.substr(start,ans);
    }
```	
### 271	Encode and Decode Strings  	Medium	
locked
### 165	Compare Version Numbers	 	Medium	
Compare two version numbers version1 and version2.
If version1 > version2 return 1; if version1 < version2 return -1;otherwise return 0.

using . to separate parts and compare parts by parts. if one is missing parts, use 0 to compare
```cpp
	int compareVersion(string version1, string version2) {
		//replace . to space and use stringstream to read
		for(char& c: version1) if(c=='.') c=' ';
		for(char& c: version2) if(c=='.') c=' ';
		stringstream s1(version1),s2(version2);
		int v1=0,v2=0;
		while(s1.good() || s2.good())
		{
            s1>>v1,s2>>v2;
			if(v1>v2) return 1;
			else if(v1<v2) return -1;
			v1=v2=0;
		}
		return 0;
	}
```	
### 91	Decode Ways	 	Medium	
see dp
A to 1 and Z to 26

### 468	Validate IP Address	 	Medium	
ipv4 and ipv6
ipv4: 4 numbers from 0 to 255, separated by ., no leading zero.
ipv6: 8 numbers, each number is 4 digit hex, separated by :
we can use sscanf to get the hex

```cpp
    string validIPAddress(string IP) {
        //using : or . for ipv4 or ipv6
        //stringstream to read
        bool b;
        if(IP.find('.')!=string::npos)
        {
            if(isIPv4(IP)) return "IPv4";
            return "Neither";
        }
        if(isIPv6(IP)) return "IPv6";
        return "Neither";
    }
    
    bool isIPv4(string ip)
    {
        int start=0;
        
        for(int i=0;i<3;i++)
        {
            int pos=ip.find('.',start);
            if(pos!=string::npos)
            {
                //check the substring
                int len=pos-start;
                if(len>3 || len<1) return 0;
                string s=ip.substr(start,pos-start);
                if(s[0]=='0' && s.size()>1) return 0;
                //int res=stoi(s);
                int res;
                if(s.find_first_not_of("0123456789")!=string::npos) return 0;
                if(sscanf(s.c_str(),"%d",&res)!=1) return 0;//cannot contain char other than 0-9
                if(res<0 || res>255) return 0;
            }
            start=pos+1;
        }
        string s=ip.substr(start);
        int len=s.size();
        if(len>3) return 0;
        if(s[0]=='0' && s.size()>1) return 0;
        int res;//=stoi(s);//if it is not a number this will fail
        if(sscanf(s.c_str(),"%d",&res)!=1) return 0;
        if(s.find_first_not_of("0123456789")!=string::npos) return 0;
        if(res<0 || res>255) return 0;
        return 1;
    }   
    bool isIPv6(string ip)
    {
        int start=0;
        int t=0,ind;
        char c;
        const char* p=ip.c_str();
        if(ip.find_first_not_of("0123456789:abcdefABCDEF")!=string::npos) return 0;
        for(int i=0;i<7;i++)
        {
            ind=ip.find_first_of(':',start);
            if(ind-start>4 || ind-start<1) return 0;
            
            int num=sscanf(p+start,"%x",&t);
            if(num!=1) return 0;
            //sscanf(p+ind,"%c",&c);
            //if(c!=':') return 0;
            start=ind+1;
            //
        }
        string s=ip.substr(start);
        if(s.size()>4) return 0;
        if(sscanf(s.c_str(),"%x",&t)!=1) return 0;
        
        
        return 1;
    }
```	

### 151	Reverse Words in a String	 	Medium	
read words into a vector and then reverse the vector
simple
```cpp
    void reverseWords(string &s) {
        if(s.empty()) return;
        istringstream ss(s);
        string str;
        vector<string> vs;
        while(ss>>str) {vs.push_back(str);str.clear();}
        if(vs.size())
        {
			reverse(vs.begin(),vs.end());
			//s=accumulate(vs.begin(),vs.end(),string());
			s.clear();
			for(int i=0;i<vs.size()-1;i++) s+=vs[i]+" ";
			s+=vs[vs.size()-1];
        }
        else s.clear();
    }
```	
### 8 String to Integer (atoi)	 	Medium	
using stringstream is trivial.

## hard
### 761	Special Binary String	 	Hard	
Special binary strings are binary strings with the following two properties:

The number of 0's is equal to the number of 1's.
Every prefix of the binary string has at least as many 1's as 0's. (>=)
Given a special string S, a move consists of choosing two consecutive, non-empty, special substrings of S, and swapping them. (Two strings are consecutive if the last character of the first string is exactly one index before the first character of the second string.)

At the end of any number of moves, what is the lexicographically largest resulting string possible?

Example 1:
Input: S = "11011000"
Output: "11100100"
Explanation:
The strings "10" [occuring at S[1]] and "1100" [at S[3]] are swapped.
This is the lexicographically largest string possible after some number of swaps.

Just 4 steps:

Split S into several special strings (as many as possible).
Special string starts with 1 and ends with 0. Recursion on the middle part.
Sort all special strings in lexicographically largest order.
Join and output all strings.

```cpp
string makeLargestSpecial(string S) {
    int count = 0, i = 0;
    vector<string> res;
    for (int j = 0; j < S.size(); ++j) {
      if (S[j] == '1') count++;
      else count--;
      if (count == 0) {
        res.push_back('1' + makeLargestSpecial(S.substr(i + 1, j - i - 1)) + '0');
        i = j + 1;
      }
    }
    sort(res.begin(), res.end(), greater<string>());
    string res2 = "";
    for (int i = 0; i < res.size(); ++i) res2 += res[i];
    return res2;
  }
```

### 527	Word Abbreviation  	Hard	
locked

### 632	Smallest Range	 	Hard	
You have k lists of sorted integers in ascending order. Find the smallest range that includes at least one number from each of the k lists.

We define the range [a,b] is smaller than range [c,d] if b-a < d-c or a < c if b-a == d-c.

using pq for merge sort of k lists. the range is the max and min difference.
we need to keep the min and max.
```cpp
	vector<int> smallestRange(vector<vector<int>>& nums) {
		int n=nums.size();
		vector<int> ind(n); //the index for each list
		priority_queue<vector<int>,vector<vector<int>>,greater<vector<int>>> pq; //stores value and index i, j
		int mn=INT_MAX,mx=INT_MIN; //the range
		vector<int> ans;
		int minrange=INT_MAX;
        for(int i=0;i<n;i++)
        {
            pq.push({nums[i][ind[i]],i});
            mx=max(mx,nums[i][ind[i]]);
            mn=min(mn,nums[i][ind[i]]);
        }
        
		while(1)
		{
            if(minrange>mx-mn)
            {
                minrange=mx-mn;
                ans={mn,mx};
            }
            auto t=pq.top();
            pq.pop();
            ind[t[1]]++;
            if(ind[t[1]]<nums[t[1]].size()) 
            {
                pq.push({nums[t[1]][ind[t[1]]],t[1]});
                mn=pq.top()[0];
                mx=max(mx,nums[t[1]][ind[t[1]]]);
                //cout<<mn<<" "<<mx<<" "<<nums[t[1]][ind[t[1]]]<<endl;
            }
            else break; //used one list
		}
		return ans;
	}
```	

### 899	Orderly Queue	 	Hard	
A string S of lowercase letters is given.  Then, we may make any number of moves.

In each move, we choose one of the first K letters (starting from the left), remove it, and place it at the end of the string.

Return the lexicographically smallest string we could have after any number of moves.

k=1, we can rotate the whole string, n states
k>1: we can rotate the whole string (0), start from 1,.... start from k-1.
bubble sort eventually is swapping pairs, So, you have a buffer of at least 2 when K>1
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
	
### 159	Longest Substring with At Most Two Distinct Characters  	Hard	
locked
### 770	Basic Calculator IV	 	Hard	
### 736	Parse Lisp Expression	 	Hard
You are given a string expression representing a Lisp-like expression to return the integer value of.

The syntax for these expressions is given as follows.

An expression is either an integer, a let-expression, an add-expression, a mult-expression, or an assigned variable. Expressions always evaluate to a single integer.
(An integer could be positive or negative.)
A let-expression takes the form (let v1 e1 v2 e2 ... vn en expr), where let is always the string "let", then there are 1 or more pairs of alternating variables and expressions, meaning that the first variable v1 is assigned the value of the expression e1, the second variable v2 is assigned the value of the expression e2, and so on sequentially; and then the value of this let-expression is the value of the expression expr.
An add-expression takes the form (add e1 e2) where add is always the string "add", there are always two expressions e1, e2, and this expression evaluates to the addition of the evaluation of e1 and the evaluation of e2.
A mult-expression takes the form (mult e1 e2) where mult is always the string "mult", there are always two expressions e1, e2, and this expression evaluates to the multiplication of the evaluation of e1 and the evaluation of e2.
For the purposes of this question, we will use a smaller subset of variable names. A variable starts with a lowercase letter, then zero or more lowercase letters or digits. Additionally for your convenience, the names "add", "let", or "mult" are protected and will never be used as variable names.
Finally, there is the concept of scope. When an expression of a variable name is evaluated, within the context of that evaluation, the innermost scope (in terms of parentheses) is checked first for the value of that variable, and then outer scopes are checked sequentially. It is guaranteed that every expression is legal. Please see the examples for more details on scope.
Evaluation Examples:
Input: (add 1 2)
Output: 3

Input: (mult 3 (add 2 3))
Output: 15

Input: (let x 2 (mult x 5))
Output: 10

Input: (let x 2 (mult x (let x 3 y 4 (add x y))))
Output: 14
Explanation: In the expression (add x y), when checking for the value of the variable x,
we check from the innermost scope to the outermost in the context of the variable we are trying to evaluate.
Since x = 3 is found first, the value of x is 3.

Input: (let x 3 x 2 x)
Output: 2
Explanation: Assignment in let statements is processed sequentially.

Input: (let x 1 y 2 x (add x y) (add x y))
Output: 5
Explanation: The first (add x y) evaluates as 3, and is assigned to x.
The second (add x y) evaluates as 3+2 = 5.

Input: (let x 2 (add (let x 3 (let x 4 x)) x))
Output: 6
Explanation: Even though (let x 4 x) has a deeper scope, it is outside the context
of the final x in the add-expression.  That final x will equal 2.

Input: (let a1 3 b2 (add a1 1) b2) 
Output 4
Explanation: Variable names can contain digits after the first character.

```cpp
    int evaluate(string expression) {
        unordered_map<string,int> myMap;
        return help(expression,myMap);
    }
    
    int help(string expression,unordered_map<string,int> myMap) {
        if ((expression[0] == '-') || (expression[0] >= '0' && expression[0] <= '9'))
            return stoi(expression);
        else if (expression[0] != '(')
            return myMap[expression];
        //to get rid of the first '(' and the last ')'
        string s = expression.substr(1,expression.size()-2);
        int start = 0;
        string word = parse(s,start);
        if (word == "let") {
            while (true) {
                string variable = parse(s,start);
                //if there is no more expression, simply evaluate the variable
                if (start > s.size())
                    return help(variable,myMap);
                string temp = parse(s,start);
                myMap[variable] = help(temp,myMap);                    
            }
        }
        else if (word == "add") 
            return help(parse(s,start),myMap) + help(parse(s,start),myMap);
        else if (word == "mult") 
            return help(parse(s,start),myMap) * help(parse(s,start),myMap);
    }
    
    //function to seperate each expression
    string parse(string &s,int &start) {
        int end = start+1, temp = start, count = 1;
        if (s[start] == '(') {
            while (count != 0) {
                if (s[end] == '(')
                    count++;
                else if (s[end] == ')')
                    count--;
                end++;
            }
        }
        else {
            while (end < s.size() && s[end] != ' ')
                end++;
        }
        start = end+1;
        return s.substr(temp,end-temp);
    }
```	
### 772	Basic Calculator III  	Hard	
locked
### 340	Longest Substring with At Most K Distinct Characters  	Hard	
locked
### 730	Count Different Palindromic Subsequences	 	Hard	
Given a string S, find the number of different non-empty palindromic subsequences in S, and return that number modulo 10^9 + 7.

A subsequence of a string S is obtained by deleting 0 or more characters from S.

A sequence is palindromic if it is equal to the sequence reversed.

Two sequences A_1, A_2, ... and B_1, B_2, ... are different if there is some i for which A_i != B_i.

dp:


### 72	Edit Distance	 	Hard	
see dp

### 936	Stamping The Sequence	 	Hard	
You want to form a target string of lowercase letters.

At the beginning, your sequence is target.length '?' marks.  You also have a stamp of lowercase letters.

On each turn, you may place the stamp over the sequence, and replace every letter in the sequence with the corresponding letter from the stamp.  You can make up to 10 * target.length turns.

For example, if the initial sequence is "?????", and your stamp is "abc",  then you may make "abc??", "?abc?", "??abc" in the first turn.  (Note that the stamp must be fully contained in the boundaries of the sequence in order to stamp.)

If the sequence is possible to stamp, then return an array of the index of the left-most letter being stamped at each turn.  If the sequence is not possible to stamp, return an empty array.

For example, if the sequence is "ababc", and the stamp is "abc", then we could return the answer [0, 2], corresponding to the moves "?????" -> "abc??" -> "ababc".

Also, if the sequence is possible to stamp, it is guaranteed it is possible to stamp within 10 * target.length moves.  Any answers specifying more than this number of moves will not be accepted.

```cpp
    vector<int> movesToStamp(string stamp, string target) {
        //reverse operation: matched then change it to ***
        //until we change the target string into *****
        //note we can only match one end if it is covered
        int n=target.length();
        string final(n,'*');
        vector<int> ans;
        while(target!=final)
        {
            int ind=match_change(target,stamp);
            if(ind==-1) return vector<int>();
            ans.push_back(ind);
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
    int match_change(string& target,string stamp)
    {
        //find the first matching and return
        //at least has one non * char inside, * matches any char
        bool matched=0;
        for(int i=0;i<target.size();i++)
        {
            int cnt_match=0;
            int j=0;
            for(j=0;j<stamp.size();j++)
            {
                if(target[i+j]=='*') continue;
                if(target[i+j]==stamp[j]) cnt_match++;
                else break;
            }
            if(j==stamp.size()&& cnt_match) 
            {
                for(j=0;j<stamp.size();j++) target[i+j]='*';
                return i;
            }
        }
        return -1; //no matching
    }
```
	
### 115	Distinct Subsequences	 	Hard	
dp

### 591	Tag Validator	 	Hard	
<TAG>Tag Content</TAG> tag is uppercase and length is from 1 to 9.
tag content: can contain valid closed tags, cdata and any characters except unmatched <
cdata format: <![CDATA[cdata content]]>
```cpp    bool isValid(string code) {
        int i = 0;
        return validTag(code, i) && i == code.size();
    }

private:
    bool validTag(string s, int& i) {
        int j = i;
        string tag = parseTagName(s, j);
        if (tag.empty()) return false;
        if (!validContent(s, j)) return false;
        int k = j + tag.size() + 2; // expecting j = pos of "</" , k = pos of '>'
        if (k >= s.size() || s.substr(j, k + 1 - j) != "</" + tag + ">") return false;
        i = k + 1;
        return true;
    }

    string parseTagName(string s, int& i) {
        if (s[i] != '<') return "";
        int j = s.find('>', i);
        if (j == string::npos || j - 1 - i < 1 || 9 < j - 1 - i) return "";
        string tag = s.substr(i + 1, j - 1 - i);
        for (char ch : tag) {
            if (ch < 'A' || 'Z' < ch) return "";
        }
        i = j + 1;
        return tag;
    }

    bool validContent(string s, int& i) {
        int j = i;
        while (j < s.size()) {
            if (!validText(s, j) && !validCData(s, j) && !validTag(s, j)) break;
        }
        i = j;
        return true;
    }

    bool validText(string s, int& i) {
        int j = i;
        while (i < s.size() && s[i] != '<') { i++; }
        return i != j;
    }

    bool validCData(string s, int& i) {
        if (s.find("<![CDATA[", i) != i) return false;
        int j = s.find("]]>", i);
        if (j == string::npos) return false;
        i = j + 3;
        return true;
    }
```	
### 87	Scramble String	 	Hard	
tree or recursive:
```cpp
    bool isScramble(string s1, string s2) {
        //always use recursively approach.
        //the left and right branch shall have the same character map
        int len=s1.size();
        int len=s1.size();
        if(len==0) return 1;
        if(s1==s2) return 1;
        //string are not the same, just sort them and compare
        string ts1=s1,ts2=s2;
        sort(ts1.begin(),ts1.end());
        sort(ts2.begin(),ts2.end());
        if(ts1!=ts2) return 0;
        //since the string can be swapped or not swapped, we need check it left and right, left-left, 4 combination
        //note we cannot start from 0, and this will cause an infinite loop!
        for(int i=1;i<len;i++) //be careful of the index, starting position cannot exceed the length
        {
            if(isScramble(s1.substr(0,i),s2.substr(0,i)) && isScramble(s1.substr(i),s2.substr(i))) //left and right is not swapped
                return 1;
            if(isScramble(s1.substr(0,i),s2.substr(len-i)) && isScramble(s1.substr(i),s2.substr(0,len-i)))
               return 1;
        }
        return 0;
    }
```
dp
```cpp
    bool isScramble(string s1, string s2) {
        int n = s1.size(), len, i, j, k;
        if(0==n) return true;
        if(1==n) return s1==s2;
        bool dp[n+1][n][n];

        for(i=0; i<n; ++i)
            for(j=0; j<n; ++j)
                dp[1][i][j] = s1[i] == s2[j];
                
        for(len=2; len <=n; ++len)
            for(i=0; i<=n-len; ++i)
                for(j=0; j<=n-len; ++j)
                {
                    dp[len][i][j] = 0;
                    for(k=1; k<len && !dp[len][i][j]; ++k)
                    {
                        dp[len][i][j] = dp[len][i][j]||(dp[k][i][j] && dp[len-k][i+k][j+k]);
                        dp[len][i][j] = dp[len][i][j]||(dp[k][i+len-k][j] && dp[len-k][i][j+k]);
                    }
                }
        return dp[n][0][0];            

    }
```	
### 336	Palindrome Pairs	 	Hard	
Given a list of unique words, find all pairs of distinct indices (i, j) in the given list, so that the concatenation of the two words, i.e. words[i] + words[j] is a palindrome.
brutal force is fine
```cpp
    vector<vector<int>> palindromePairs(vector<string>& words) {
        //brutal force approach: 
        vector<vector<int>> res;
        vector<int> item(2);
        for(int i=0;i<words.size();i++)
        {
            for(int j=0;j<words.size();j++)
            {
                if(i==j) continue;
                if(is_palindrome(words[i],words[j]))
                {
                    item[0]=i;item[1]=j;
                    res.push_back(item);
                }
            }
        }
        return res;
    }
    bool is_palindrome(const string& a, const string& b)
    {
        //concat a and b, however avoid make a string
        int la=a.size(),lb=b.size();
        int len=la+lb;
        int mid=len/2;
        for(int i=0;i<mid;i++)
        {
            //compare i and len-1-i
            char c,d;
            int j=len-1-i;
            c=(i<la)?a[i]:b[i-la];
            d=(j<la)?a[j]:b[j-la];
            if(c!=d) return 0;
        }
        return 1;
    }
```
	
### 76	Minimum Window Substring	 	Hard	
Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).
For most substring problem, we are given a string and need to find a substring of it which satisfy some restrictions. A general way is to use a hashmap assisted with two pointers.

first create the hashmap of the string T
use two pointer for s: eveytime we found a char in both, we decrease the counter
until we found no char left, we try to shrink the window. 

this is a pretty solution. 
```cpp
string minWindow(string s, string t) {
	unordered_map<char, int> mp;	// Statistic for count of char in t
	for (auto c : t) mp[c]++;
	// counter represents the number of chars of t to be found in s.
	int start = 0, end = 0, minStart = 0, minLen = INT_MAX;
	int slen = s.size(),tlen = t.size();
		
	while (end<slen) // Move end to find a valid window.
    {
		if (mp[s[end]]) tlen--;// If char in s exists in t, decrease counter
		mp[s[end]]--;// Decrease m[s[end]]. If char does not exist in t, m[s[end]] will be negative.
		end++;
		
		while (tlen == 0) // When we found a valid window, move start to find smaller window.
        {
			if(end-start<minLen) 
            {
				minStart=start;
				minLen=end-start;
			}
			mp[s[start]]++;
			
			if (mp[s[start]]) tlen++;// When char exists in t, increase counter.
			start++;
		}
	}
	if (minLen != INT_MAX)return s.substr(minStart, minLen);
	return "";
}
```	
### 97	Interleaving String	 	Hard	
Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.
if put s1 in the row and s2 in the column, s3 is the path in the matrix (only right and down is allowed) from top left to bottom right
dp:
```cpp
    bool isInterleave(string s1, string s2, string s3) {
        int n1=s1.size(),n2=s2.size();
        if(s3.length() != n1+n2) return 0;
        if(n1==0) return s3==s2;
        if(n2==0) return s3==s1;
        vector<vector<bool>> dp(n1+1,vector<bool>(n2+1));
        dp[0][0]=1; //empty vs empty
        //boundary condition
        for(int i=1;i<n1+1;i++) dp[i][0]=dp[i-1][0] && (s1[i-1]==s3[i-1]); //j=0, s1 compare with s3
        for(int j=1;j<n2+1;j++) dp[0][j]=dp[0][j-1] && (s2[j-1]==s3[j-1]); //i=0: s2 compare with s3
        for(int i=1; i<n1+1; i++)
        {
            for(int j=1; j< n2+1; j++)
            {
                dp[i][j] = (dp[i-1][j] && s1[i-1] == s3[i+j-1] ) || (dp[i][j-1] && s2[j-1] == s3[i+j-1] );
            }
        }   
        return dp[n1][n2];
    }
```	

### 214	Shortest Palindrome	 	Hard	
Given a string s, you are allowed to convert it to a palindrome by adding characters in front of it. Find and return the shortest palindrome you can find by performing this transformation.

KMP
```cpp
    string shortestPalindrome(string s) {
        //using KMP method to find the largest prefix!
        string sr(s.rbegin(),s.rend());
        int len=sr.size();
        s+='$'+sr;
        vector<int> sfun=computePrefixFunction(s);
        //the longest common prefix is the last one
        //append the remaining in the reverse string
        int largest_prefix=sfun.back();
        return sr.substr(0,len-largest_prefix)+s.substr(0,len);
    }
    vector<int> computePrefixFunction(const string& p)
    {
        int len=p.size();
        vector<int> s(len,0);
        int border=0;
        for(int i=1;i<len;i++)
        {
            while(border && p[i]!=p[border]) border=s[border-1];
            if(p[i]==p[border]) border++;
            else border=0;
            s[i]=border;
        }
        return s;
    }
```
assume we add xxx to make it palindrome
xyz__________
then the original string must be like this ______zyx and the inner side must be a pal string
it is equivalent to find the longest prefix palindrome string

The idea is to use two anchors j and i to compare the String from beginning and end.
If j can reach the end, the String itself is Palindrome. 
Otherwise, we divide the String by j, and get mid = s.substring(0, j) and suffix.

We reverse suffix as beginning of result and recursively call shortestPalindrome to get result of mid 
then append suffix to get result.
```cpp
    string shortestPalindrome(string s) {
        //find the longest palindrome in s and then use it as the base to add
        //the longest must start with the first char
        int n=s.size();
        int j = 0;
        for (int i = s.length() - 1; i >= 0; i--) 
        {
            if (s[i] == s[j]) { j += 1; }
        }
        if (j == s.length()) { return s; }
        string suffix=s.substr(j);
        string rss=suffix;
        reverse(rss.begin(),rss.end());
        return rss+shortestPalindrome(s.substr(0,j))+suffix;
    }

```
this two pointer scheme	is pretty interesting. it switches the i and j positions
	

### 158	Read N Characters Given Read4 II - Call multiple times  	Hard	
locked
### 32	Longest Valid Parentheses	 	Hard	
Given a string containing just the characters '(' and ')', 
find the length of the longest valid (well-formed) parentheses substring.

### 10	Regular Expression Matching	 	Hard	
see dp
### 273	Integer to English Words	 	Hard	
### 30	Substring with Concatenation of All Words	 	Hard	
### 68	Text Justification	 	Hard	
### 44	Wildcard Matching	 	Hard	
see dp
### 564	Find the Closest Palindrome	 	Hard	
### 126	Word Ladder II	 	Hard	
### 65	Valid Number	 	Hard
use stringstream
```cpp
    bool isNumber(string s) {
        string whitespace=" \t";
        int ind=s.find_last_not_of(whitespace);
        s=s.substr(0,ind+1);
        stringstream ss(s);
        double a;
        //we need remove trailing space since it is not good for processing
 
        ss>>a;
        
        if(ss.fail()) return 0;
        if(ss.eof()) return 1;
        return 0;
    }
```	



### 890. Find and Replace Pattern
permuation of pattern and match
we can transform both the pattern and the word into same abc.., the first char is a and second appeared is b...
and then check if they are equal

### 537. Complex Number Multiplication
just using stringstream to read real and imag correctly

### 791. Custom Sort String
s is sorted using some custom sort, we need sort string t
using hashmap to arrange the sorted chars first

### 553. Optimal Division
The second one is always under, others are above, so there is only one answer

### 647. Palindromic Substrings--dp
count the number of pal-substring
dp[i][j]=self+dp[i][j-1]+dp[i+1][j]-dp[i+1][j-1];

### 856. Score of Parentheses
using stack
or dp: dp[i] represents the score 
when (, the score is 0

### 22. Generate Parentheses
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses
recursive build: has n left ( and n right ). The principle is any time left>=right
```cpp
    void build(vector<string>& vs,int n,int lcount,int rcount,string s)
    {
        if(s.size()==2*n) {vs.push_back(s);return;}
        if(lcount<n) build(vs,n,lcount+1,rcount,s+"(");
        if(lcount>rcount) build(vs,n,lcount,rcount+1,s+")");
    }
```
### 12. Integer to Roman
<4
### 4
### 6-8
need to know the rules

### 539. Minimum Time Difference
that is a wrap. sort it for integer, and the last one and the first one (make it a circle) negative +24*60

### 583. Delete Operation for Two Strings-dp
### 916. Word Subsets
the words need include all the chars in the dictionary including number of occurance
hash

### 833. Find And Replace in String
record the operations
and build the string from end to head (where the index does not change)

### 49. Group Anagrams
using hashmap

### 816. Ambiguous Coordinates
all spaces,'.' and ',' are removed. return all possible combinations
this is recursive, we iteratively split the string into half, and then add decimal at different positions for each of them
and then combine the left and right

### 831. Masking Personal Information
masking the email and phone numnber

### 809. Expressive Words
letters are repeated more than needed (at least 3 same char)
use two pointer 


### 767. Reorganize String
arrange the most frequent first. and then less frequent

### 17. Letter Combinations of a Phone Number
return all combinations of letters

### 848. Shifting Letters
shift the first number of chars by a[i] times.
easy it is accum sum for each letter

### 842. Split Array into Fibonacci Sequence
observation: the first two number uniquely determines the whole array. number has no leading zero
the idea is:
first try to choose the first number, 1 digit, 2 digits...,.....
then choose the second number: 1 digits, 2 digits..... and then we check if we can exhaust the string
when there is 0 number, find num1
when there is 1 number, find num2
when there is 2 or more number, find the last two sum

need a start index, and current index

### 522. Longest Uncommon Subsequence II
defined: a subsequence of one string and is not subsequence of any other string. need return the length
intuition: 
sort in size. long subsequence cannot be subsequence of short strings
check if a str is subsequence of another string: using two pointer
the most important: the subsequence shall be one of the words. since if subsequence cannot be common, adding some more char is also not common.

### 227. Basic Calculator II
supports + - * /
simple syntax processing: 
if * and / we need to read the followed number to complete calculation
if + and - we need get the left and right results before do the calculation
better using stack to store the results

### 385. Mini Parser
[] is an object, [[]] nested. the basic is the object. recursive for nested

### 678. Valid Parenthesis String
* can be left, right, or empty.
rule: at any position, left ( is always >= right ).
recursive: count left and right, and index of current char, treat * as left right or empty
```cpp
    bool is_validstring(const string& s,int i,int nleft,int nright)
    {
        if(nleft<nright) return 0;
        if(i==s.length())
        {
            return nleft==nright;
        }
        if(s[i]=='(') return is_validstring(s,i+1,nleft+1,nright);
        if(s[i]==')') return is_validstring(s,i+1,nleft,nright+1);
        else //use it as left, right or nothing
        {
            return is_validstring(s,i+1,nleft,nright) || is_validstring(s,i+1,nleft+1,nright) || is_validstring(s,i+1,nleft,nright+1);
        }
    }
```
### 93. Restore IP Addresses
ip is unsigned int8, no leading zero
each number has 1 to 3 digits
all 4 numbers adds to total length of string
(leading zero can be eliminated by converting back to string)

### 6. ZigZag Conversion
accumulate vector string can connect the strings

### 722. Remove Comments
remove c++ comments
regular expression: 
vector of a string to a string using copy to stringstream.

### 556. Next Greater Element III
using the same digits
next permutation or greedy to swap the first smaller with first larger (reverse direction)

### 43. Multiply Strings
simple
### 71. Simplify Path
. current dir
.. upper dir
use a stack or vector to record the actual path

### 5. Longest Palindromic Substring--dp
### 3. Longest Substring Without Repeating Characters
for all substring ending at i. the substring without duplicate. two pointer.

### 165. Compare Version Numbers
note string compare is not sufficient since it allows leading zeros. we need convert to int and then convert to string.
read from a string using some delim and copy back to another output stringstream. getline can accept delim

    string sentence = "And I feel fine...";
    istringstream iss(sentence);
    copy(istream_iterator<string>(iss),
         istream_iterator<string>(),
         ostream_iterator<string>(cout, "\n"));
       
### 91. Decode Ways--dp
### 468. Validate IP Address
validate ipv4 or ipv6

### 151. Reverse Words in a String
common trick using stringstream to convert to vector

### 8. String to Integer (atoi)

### 214. Shortest Palindrome.md
### problem summary
by adding min number of chars in the front to make the string palindrome

### approach
The problem is to find the longest palindrome string starting with 0.
However brutal force TLE for all aaaaa since we have to repeat each char until the longest

The smartest approach using recursive
we use two pointers i and j from both end. if j goes to end, the whole string is a palindrome string.
that is a reverse compare. Otherwise, we divide the String by j, and get mid = s.substring(0, j) and suffix.

```cpp
    string shortestPalindrome(string s) {
        //find the longest palindrome in s and then use it as the base to add
        //the longest must start with the first char
        int n=s.size();
        int j = 0;
        for (int i = s.length() - 1; i >= 0; i--) 
        {
            if (s[i] == s[j]) { j += 1; }
        }
        if (j == s.length()) { return s; }
        string suffix=s.substr(j);
        string rss=suffix;
        reverse(rss.begin(),rss.end());
        return rss+shortestPalindrome(s.substr(0,j))+suffix;
    }
```    
