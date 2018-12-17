890. Find and Replace Pattern
permuation of pattern and match
we can transform both the pattern and the word into same abc.., the first char is a and second appeared is b...
and then check if they are equal

537. Complex Number Multiplication
just using stringstream to read real and imag correctly

791. Custom Sort String
s is sorted using some custom sort, we need sort string t
using hashmap to arrange the sorted chars first

553. Optimal Division
The second one is always under, others are above, so there is only one answer

647. Palindromic Substrings--dp
count the number of pal-substring
dp[i][j]=self+dp[i][j-1]+dp[i+1][j]-dp[i+1][j-1];

856. Score of Parentheses
using stack
or dp: dp[i] represents the score 
when (, the score is 0

22. Generate Parentheses
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
12. Integer to Roman
<4
4
6-8
need to know the rules

539. Minimum Time Difference
that is a wrap. sort it for integer, and the last one and the first one (make it a circle) negative +24*60

583. Delete Operation for Two Strings-dp
916. Word Subsets
the words need include all the chars in the dictionary including number of occurance
hash

833. Find And Replace in String
record the operations
and build the string from end to head (where the index does not change)

49. Group Anagrams
using hashmap

816. Ambiguous Coordinates
all spaces,'.' and ',' are removed. return all possible combinations
this is recursive, we iteratively split the string into half, and then add decimal at different positions for each of them
and then combine the left and right

831. Masking Personal Information
masking the email and phone numnber

809. Expressive Words
letters are repeated more than needed (at least 3 same char)
use two pointer 


767. Reorganize String
arrange the most frequent first. and then less frequent

17. Letter Combinations of a Phone Number
return all combinations of letters

848. Shifting Letters
shift the first number of chars by a[i] times.
easy it is accum sum for each letter

842. Split Array into Fibonacci Sequence
observation: the first two number uniquely determines the whole array. number has no leading zero
the idea is:
first try to choose the first number, 1 digit, 2 digits...,.....
then choose the second number: 1 digits, 2 digits..... and then we check if we can exhaust the string
when there is 0 number, find num1
when there is 1 number, find num2
when there is 2 or more number, find the last two sum

need a start index, and current index

522. Longest Uncommon Subsequence II
defined: a subsequence of one string and is not subsequence of any other string. need return the length
intuition: 
sort in size. long subsequence cannot be subsequence of short strings
check if a str is subsequence of another string: using two pointer
the most important: the subsequence shall be one of the words. since if subsequence cannot be common, adding some more char is also not common.

227. Basic Calculator II
supports + - * /
simple syntax processing: 
if * and / we need to read the followed number to complete calculation
if + and - we need get the left and right results before do the calculation
better using stack to store the results

385. Mini Parser
[] is an object, [[]] nested. the basic is the object. recursive for nested

678. Valid Parenthesis String
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
93. Restore IP Addresses
ip is unsigned int8, no leading zero
each number has 1 to 3 digits
all 4 numbers adds to total length of string
(leading zero can be eliminated by converting back to string)

6. ZigZag Conversion
accumulate vector string can connect the strings

722. Remove Comments
remove c++ comments
regular expression: 
vector of a string to a string using copy to stringstream.

556. Next Greater Element III
using the same digits
next permutation or greedy to swap the first smaller with first larger (reverse direction)

43. Multiply Strings
simple
71. Simplify Path
. current dir
.. upper dir
use a stack or vector to record the actual path

5. Longest Palindromic Substring--dp
3. Longest Substring Without Repeating Characters
for all substring ending at i. the substring without duplicate. two pointer.

165. Compare Version Numbers
note string compare is not sufficient since it allows leading zeros. we need convert to int and then convert to string.
read from a string using some delim and copy back to another output stringstream. getline can accept delim

    string sentence = "And I feel fine...";
    istringstream iss(sentence);
    copy(istream_iterator<string>(iss),
         istream_iterator<string>(),
         ostream_iterator<string>(cout, "\n"));
       
91. Decode Ways--dp
468. Validate IP Address
validate ipv4 or ipv6

151. Reverse Words in a String
common trick using stringstream to convert to vector

8. String to Integer (atoi)





