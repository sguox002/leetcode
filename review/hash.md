# hashtable

## contents

771	Jewels and Stones		Easy	
760	Find Anagram Mappings 	Easy	
961	N-Repeated Element in Size 2N Array		Easy	
1002 Find Common Characters		Easy	
811	Subdomain Visit Count		Easy	
359	Logger Rate Limiter 	Easy	
500	Keyboard Row		Easy	
463	Island Perimeter		Easy	
884	Uncommon Words from Two Sentences		Easy	
266	Palindrome Permutation 	Easy	
136	Single Number		Easy	
575	Distribute Candies		Easy	
706	Design HashMap		Easy	
953	Verifying an Alien Dictionary		Easy	
349	Intersection of Two Arrays		Easy	
690	Employee Importance		Easy	
748	Shortest Completing Word		Easy	
705	Design HashSet		Easy	
389	Find the Difference		Easy	
242	Valid Anagram		Easy	
217	Contains Duplicate		Easy	
387	First Unique Character in a String		Easy	
447	Number of Boomerangs		Easy	
409	Longest Palindrome		Easy	
599	Minimum Index Sum of Two Lists		Easy	
350	Intersection of Two Arrays II		Easy	
202	Happy Number		Easy	
720	Longest Word in Dictionary		Easy	
1	Two Sum		Easy	
594	Longest Harmonious Subsequence		Easy	
246	Strobogrammatic Number 	Easy	
645	Set Mismatch		Easy	
734	Sentence Similarity 	Easy	
970	Powerful Integers		Easy	
438	Find All Anagrams in a String		Easy	
205	Isomorphic Strings		Easy	
624	Maximum Distance in Arrays 	Easy	
219	Contains Duplicate II		Easy	
290	Word Pattern		Easy	
170	Two Sum III - Data structure design 	Easy	
204	Count Primes		Easy	

535	Encode and Decode TinyURL		Medium	
739	Daily Temperatures		Medium	
94	Binary Tree Inorder Traversal		Medium	
311	Sparse Matrix Multiplication 	Medium	
451	Sort Characters By Frequency		Medium	
609	Find Duplicate File in System		Medium	
347	Top K Frequent Elements		Medium	
508	Most Frequent Subtree Sum		Medium	
648	Replace Words		Medium	
676	Implement Magic Dictionary		Medium	
781	Rabbits in Forest		Medium	
694	Number of Distinct Islands 	Medium	
981	Time Based Key-Value Store		Medium	
454	4Sum II		Medium	
939	Minimum Area Rectangle		Medium	
249	Group Shifted Strings 	Medium	
554	Brick Wall		Medium	
244	Shortest Word Distance II 	Medium	
49	Group Anagrams		Medium	
718	Maximum Length of Repeated Subarray		Medium	
692	Top K Frequent Words		Medium	
325	Maximum Size Subarray Sum Equals k 	Medium	
974	Subarray Sums Divisible by K		Medium	
36	Valid Sudoku		Medium	
380	Insert Delete GetRandom O(1)		Medium	
525	Contiguous Array		Medium	
560	Subarray Sum Equals K		Medium	
966	Vowel Spellchecker		Medium	
314	Binary Tree Vertical Order Traversal 	Medium	
299	Bulls and Cows		Medium	
957	Prison Cells After N Days		Medium	
930	Binary Subarrays With Sum		Medium	
187	Repeated DNA Sequences		Medium	
274	H-Index		Medium	
954	Array of Doubled Pairs		Medium	
987	Vertical Order Traversal of a Binary Tree		Medium	
356	Line Reflection 	Medium	
18	4Sum		Medium	
3	Longest Substring Without Repeating Characters		Medium	
355	Design Twitter		Medium	
138	Copy List with Random Pointer		Medium	
288	Unique Word Abbreviation 	Medium	
166	Fraction to Recurring Decimal		Medium	

895	Maximum Frequency Stack		Hard	
632	Smallest Range		Hard	
159	Longest Substring with At Most Two Distinct Characters 	Hard	
711	Number of Distinct Islands II 	Hard	
770	Basic Calculator IV		Hard	
992	Subarrays with K Different Integers		Hard	
726	Number of Atoms		Hard	
340	Longest Substring with At Most K Distinct Characters 	Hard	
37	Sudoku Solver		Hard	
1001	Grid Illumination		Hard	
85	Maximal Rectangle		Hard	
358	Rearrange String k Distance Apart 	Hard	
381	Insert Delete GetRandom O(1) - Duplicates allowed		Hard	
710	Random Pick with Blacklist		Hard	
336	Palindrome Pairs		Hard	
76	Minimum Window Substring		Hard	
30	Substring with Concatenation of All Words		Hard	
1044 Longest Duplicate Substring		Hard	
149	Max Points on a Line		Hard	

## easy
### 771	Jewels and Stones		Easy	
simple. a list of jewels and a list of stones

### 760	Find Anagram Mappings 	Easy	
locked

### 961	N-Repeated Element in Size 2N Array		Easy	
simple, shown more than once is the answer

### 1002 Find Common Characters		Easy	
just count chars in each word and then choose the min of all

### 811	Subdomain Visit Count		Easy
discuss.leetcode.	com, count each domain

### 359	Logger Rate Limiter 	Easy	
locked

### 500	Keyboard Row		Easy	
simple

### 463	Island Perimeter		Easy	
this does not need hashtable just check top and left and subtract


### 884	Uncommon Words from Two Sentences		Easy	
two maps and remove those common

### 266	Palindrome Permutation 	Easy	
locked

### 136	Single Number		Easy	
every appear twice except one, using xor

### 575	Distribute Candies		Easy	
candies: number represent different type
return max number of candies can give to brothers and sisters.
apparently we can only get half of the types.
```cpp
    int distributeCandies(vector<int>& candies) {
        //build a map with its count
        //the strategy: first assign the candies with the smallest 
        unordered_set<int> table(candies.begin(),candies.end());
        return min(table.size(),candies.size()/2); 
    }
```	
### 706	Design HashMap		Easy	
hashmap using array of limited size, and each element can store a linked list for collision
```cpp    vector<list<pair<int,int>>> m_data;
    size_t m_size = 10000;
public:
    /** Initialize your data structure here. */
    MyHashMap() {
        m_data.resize(m_size);
    }
    
    /** value will always be non-negative. */
    void put(int key, int value) {
        auto &list = m_data[key % m_size];
        for (auto & val : list) {
            if (val.first == key) {
                val.second = value;
                return;
            }
        }
        list.emplace_back(key, value);
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        const auto &list = m_data[key % m_size];
        if (list.empty()) {
            return -1;
        }
        for (const auto & val : list) {
            if (val.first == key) {
                return val.second;
            }
        }
        return -1;
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        auto &list = m_data[key % m_size];
        list.remove_if([key](auto n) { return n.first == key; });
    }
```
	
### 953	Verifying an Alien Dictionary		Easy	
a-z but in different order, verify the list of words order correct or not
convert words and a-z into conventional order and then compare
```cpp
    bool isAlienSorted(vector<string>& words, string order) {
        unordered_map<char,char> mp;
        for(int i=0;i<26;i++) mp[order[i]]='a'+i;
        char c=order[0];
        for(int i=0;i<words.size();i++)
        {
            for(int j=0;j<words[i].size();j++) words[i][j]=mp[words[i][j]];
        }
        
        return is_sorted(words.begin(),words.end());
    }
```

### 349	Intersection of Two Arrays		Easy	
no duplicates allowed in result.
use set_intersection or hand write
```cpp
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        //use set_intersection
        vector<int> v(min(nums1.size(),nums2.size()));
        //sort them first
        set<int> s1,s2;
        for(int i=0;i<nums1.size();i++) s1.insert(nums1[i]);
        for(int i=0;i<nums2.size();i++) s2.insert(nums2[i]);
        vector<int>::iterator it;
        it=set_intersection(s1.begin(),s1.end(),s2.begin(),s2.end(),v.begin());//note the two arrays have to be sorted
        v.resize(it-v.begin());
        return v;
    }
```	
### 690	Employee Importance		Easy	
see dfs

### 748	Shortest Completing Word		Easy	
need to complete the license plate using words, 
sort the words according to length
if there is tie, return the first
so use stable_sort
```cpp
bool cmp(const string& a,const string& b) {return a.size()<b.size();}
class Solution {
public:
    string shortestCompletingWord(string licensePlate, vector<string>& words) {
        unordered_map<char,int> mp;
        for(int i=0;i<licensePlate.size();i++)
        {
            if(isalpha(licensePlate[i])) mp[tolower(licensePlate[i])]++;
        }
        //need sort the word list using the strlen
        stable_sort(words.begin(),words.end(),cmp);
        for(int i=0;i<words.size();i++)
        {
            if(contains(words[i],mp)) return words[i];
        }
        return string();
    }
    bool contains(string& w,unordered_map<char,int> mp)
    {
        for(int i=0;i<w.size();i++)
        {
            if(mp.count(w[i])) {mp[w[i]]--;if(mp[w[i]]==0) mp.erase(w[i]);}
            if(mp.empty()) return 1;
        }
        return 0;
    }
};
```

### 705	Design HashSet		Easy	
similar approach for hashmap

### 389	Find the Difference		Easy	
two strings, only one char is extra, just xor

### 242	Valid Anagram		Easy	
simple

### 217	Contains Duplicate		Easy	
convert to hashset and check if size is the same

### 387	First Unique Character in a String		Easy	
count in hashmap and find the first char with counter=1 (two pass)
or store the index into hash, and loop the hashtable which is more efficient.


### 447	Number of Boomerangs		Easy	
Given n points in the plane that are all pairwise distinct, a "boomerang" is a tuple of points (i, j, k) such that the distance between i and j equals the distance between i and k (the order of the tuple matters).

Find the number of boomerangs. You may assume that n will be at most 500 and coordinates of points are all in the range [-10000, 10000] (inclusive).
approach: just O(n^2) to get all distance and save into hashmap. 
```cpp
    int numberOfBoomerangs(vector<pair<int, int>>& points) {
        //brutal force: for each point get the distance to all other points, store the distance in a map
        int res=0;
        for(int i=0;i<points.size();i++) //point by point
        {
            unordered_map<long long,int> distmap;
            for(int j=0;j<points.size();j++)
            {
                if(j==i) continue;
                int dx=points[i].first-points[j].first;
                int dy=points[i].second-points[j].second;
                long long dist=(long long)dx*dx+(long long)dy*dy;
                distmap[dist]++;
            }
        
            //now calculate the number of boomerang
            //if we have n equal distance, the number of boomerang is n*(n-1)=P(n,2)

            for(auto it=distmap.begin();it!=distmap.end();it++)
            {
                if(it->second>1) res+=(it->second)*(it->second-1);
            }
        }
        return res;
    }
```
	
### 409	Longest Palindrome		Easy	
Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

This is case sensitive, for example "Aa" is not considered a palindrome here.
only one is allowed to be odd times

```cpp
    int longestPalindrome(string s) {
        //palindrome is the same from forward or backward
        //understand the problem is most important
        //pick only those letters with even plus at least one with odd times
        //char with odd numbers can be used even numbers and leave one!
        unordered_map<char,int> mp;
        for(int i=0;i<s.size();i++) mp[s[i]]++;
        //count number of odd numbers
        int count=0;
        for(auto it=mp.begin();it!=mp.end();it++) if(it->second&1) count++;
        if(count) return s.size()-count+1;
        return s.size();
        
    }
```	
### 599	Minimum Index Sum of Two Lists		Easy	
common interest with min index sum
one map is fine, and check against another array.
```cpp
    vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
        vector<string>res;
        unordered_map<string,int>m;
        int min = INT_MAX;
        for(int i = 0; i < list1.size(); i++) m[list1[i]] = i;
        for(int i = 0; i < list2.size(); i++)
            if(m.count(list2[i]) != 0)
                if(m[list2[i]] + i < min) min = m[list2[i]] + i, res.clear(), res.push_back(list2[i]);
                else if(m[list2[i]] + i == min) res.push_back(list2[i]);
        return res;
    }
```	
### 350	Intersection of Two Arrays II		Easy	
need keep the duplicates, using hashmap to keep the counters
or use multiset
```cpp
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        vector<int> v(min(nums1.size(),nums2.size()));
        multiset<int> s1,s2;
        for(int i=0;i<nums1.size();i++) s1.insert(nums1[i]);
        for(int i=0;i<nums2.size();i++) s2.insert(nums2[i]);
        vector<int>::iterator it;
        it=set_intersection(s1.begin(),s1.end(),s2.begin(),s2.end(),v.begin());
        v.resize(it-v.begin());
        return v;
    }
```	
### 202	Happy Number		Easy	
sum of digit square which equals to 1
either there is a cycle or go to 1

### 720	Longest Word in Dictionary		Easy	
the longest word can be built one char by one char from words in the dictionary
approach 1: each time remove one char at the end. and check if in the dictionary
approach 2: trie, first insert the longest one, later one we will judge start with.
```cpp
    Trie t;    
    string longestWord(vector<string>& words) {
        //trie method
        sort(words.begin(),words.end(),cmp);
        //copy(words.begin(),words.end(),ostream_iterator<string>(cout," "));
        unordered_set<string> myset(words.begin(),words.end());
        for(int i=0;i<words.size();i++)
        {
            t.insert(words[i]);
            bool found=1;
            for(int j=1;j<words[i].size();j++)
            {
                string ts=words[i].substr(0,j);
                if(!myset.count(ts) || !t.startsWith(ts)) {found=0;break;}
            }
            if(found) return words[i];
        }
        return string();
    }
```
	
### 1	Two Sum		Easy	
trivial

### 594	Longest Harmonious Subsequence		Easy	
in the array max-min=1 
find the longest harmoniuos subsequence.
hashmap to record each number's counters
then for each number we look for its neighboring

```cpp
    int findLHS(vector<int>& nums) {
        map<int,int> mp;
        if(nums.size()<2) return 0;
        for(int i=0;i<nums.size();i++) mp[nums[i]]++;
        //look for neighboring keys with difference to be 1
        int maxlen=0;
        auto it1=mp.begin(); //this needs at least two elements
        auto it=it1;//this changes the it1 to the same element
        it++;
        for(;it!=mp.end();it++,it1++)
        {
            if(it->first==it1->first+1)
            {
                int len=it->second+it1->second;
                if(len>maxlen) maxlen=len;
            }
        }
        return maxlen;
    }
```	
### 246	Strobogrammatic Number 	Easy	
locked

### 645	Set Mismatch		Easy	
1 to n, one number converts to other number, so one is missing and one is duplicated.
find the two numbers
xor will find a^b
we need another pass to find the duplicate or the missing. (using seen, change to negative)

### 734	Sentence Similarity 	Easy	
locked

### 970	Powerful Integers		Easy	
z=x^i+y^j
just use brutal force

### 438	Find All Anagrams in a String		Easy	
sliding window for hashmap

### 205	Isomorphic Strings		Easy	
s and t can be replaced. the two string has the same pattern
convert a third pattern
```cpp
    bool isIsomorphic(string s, string t) {
        //need preserve the order
        unordered_map<char,char> mp;
        string ss,tt;
        char c='a';
        for(char tc: s) {
            if(!mp.count(tc)) mp[tc]=c++; 
            ss+=mp[tc];
        }
        mp.clear();
        c='a';
        for(char tc: t) {
            if(!mp.count(tc)) mp[tc]=c++;
            tt+=mp[tc];
        }
        return ss==tt;
    }
```	


### 624	Maximum Distance in Arrays 	Easy	
locked
### 219	Contains Duplicate II		Easy	
Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the absolute difference between i and j is at most k.
```cpp
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        //using hashmap to store the elements and its index
        unordered_map<int,int> mymap;
        for(int i=0;i<nums.size();i++)
        {
            if(mymap[nums[i]] && i-mymap[nums[i]]<k) return 1;
            mymap[nums[i]]=i+1;
        }
        return 0;
    }
```
	
### 290	Word Pattern		Easy	
very similar to 205, check if the string has the same pattern
convert to the same pattern for both a and b.
```cpp
bool wordPattern(string pattern, string str) {
    map<char, int> p2i;
    map<string, int> w2i;
    istringstream in(str);
    int i = 0, n = pattern.size();
    for (string word; in >> word; ++i) {
        if (i == n || p2i[pattern[i]] != w2i[word])
            return false;
        p2i[pattern[i]] = w2i[word] = i + 1;
    }
    return i == n;
}
```

### 170	Two Sum III - Data structure design 	Easy	
locked

### 204	Count Primes		Easy	
this is a typical problem
```cpp
    int countPrimes(int n) {
        //matlab implementation on primes
        if(n<3) return 0;
        vector<bool> primes(n/2,1); //only store odd numbers, the number=2*k+1
        //note primes[0]=2 instead of 1
        for(int i=3;i<=sqrt(n);i+=2) //only odd factors
        {
            if(primes[(i-1)/2]) 
                for(int j=i*i;j<n;j+=2*i) {primes[(j-1)/2]=0;}
        }
        return accumulate(primes.begin(),primes.end(),0);
    }
```	
