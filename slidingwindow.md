# Sliding window selected questions approach

sliding window sometimes is tricky, especially when combined with other data structures, such as priority_queue, dp, hashmaps. The sliding window problems can have the following categories:

- fixed sliding window problem - generally easy, some problems (especially containing unrelated elements) can be converted to fixed window.

- two pointer: for min window, grow right until valid, then shrink left. for longest window, grow right until invalid and then shrink left.

- for counting number of valid window, get the max and min valid window.

- also there are other shrinking: for example find the valid window and then we go right to left and find the min window (without using extra data structure). for example subsequence.

## 1. fixed size sliding window

fixed window generally needs one pointer and we get rid of the left elements when the window size is exceeding the window size.
- some problems can be converted to fixed size sliding window when there exists unrelated elements.
- binary search to find min/max window satisfying some conditions.

example:
1248. Count Number of Nice Subarrays    		56.6%	Medium
find number of subarray containing exact k odd numbers..
only keep the odd number index and apply sliding window.

```cpp
    int numberOfSubarrays(vector<int>& nums, int k) {
        //sliding window?
        int ans=0;
        vector<int> odds;
        odds.push_back(-1);
        for(int i=0;i<nums.size();i++){
            if(nums[i]%2) odds.push_back(i);
        }
        odds.push_back(nums.size());
        //moving window and calculate its left and right
        if(odds.size()<k+2) return 0;
        for(int i=1;i<odds.size()-k;i++){
            ans+=(odds[i]-odds[i-1])*(odds[i+k]-odds[i+k-1]);
        }

        return ans;
    }
```

more practices on fixed size sliding window:
567	Permutation in String    		44.5%	Medium	
438	Find All Anagrams in a String    		44.3%	Medium	
134	Gas Station    		40.5%	Medium	
1343	Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold    		64.3%
1100	Find K-Length Substrings With No Repeated Characters    		73.4%	Medium	
1461	Check If a String Contains All Binary Codes of Size K    		46.3%	Medium	
1052	Grumpy Bookstore Owner    		55.5%	Medium	
1176	Diet Plan Performance    		53.9%	Easy	
1423	Maximum Points You Can Obtain from Cards    		45.1%	Medium	
1456	Maximum Number of Vowels in a Substring of Given Length    		53.5%	Medium	

### variable size sliding window using two pointers / three pointers.

generally using two pointer or three pointers to grow and shrink the window.
- for min window problem: grows right to find a valid window and then shrink to get the min window
- for max window problem: grows right to get a invalid window and then shrink to get the max window
- for min+max window problem: need 3 pointer to find the min and max window. (such as counting valid subarray problems).

validating the subarray need some other data structure, counting, hashmap, hashset et al.
the growing and shrinking depends on that the validate condition will not change.

### min window example:
76	Minimum Window Substring 
given s and t, find the min window containing all chars in t.
idea: grows right pointer until we find a valid subarray, then shrink the left pointer to get the min window.

```
    string minWindow(string s, string t) {
        unordered_map<char,int> mp;
        for(char c: t) mp[c]++;
        int i=0,j=0,mxlen=s.size()+1,mxind=-1;
        while(j<s.size()){
            mp[s[j]]--;
            while(valid(mp)) {
                if(mxlen>j-i+1){
                    mxlen=j-i+1;
                    mxind=i;
                }
                mp[s[i]]++,i++;
            }
            j++;
        }
        return mxind>=0?s.substr(mxind,mxlen):"";
    }
    bool valid(unordered_map<char,int>& mp){
        for(auto t: mp) if(t.second>0) return 0;
        return 1;
    }
```

727. Minimum Window Subsequence
similar idea: 
- grows right pointer to get a valid subarray, then try to shrink the window (but using normal growing left pointer is difficult. we can try to from j goes to left and find the matching subsequence and then get the smallest window). 
- start a new search by advance i one step right and j set to i.
- subsequence can be matched using two pointer.

```
    string minWindow(string S, string T) {
        int minind=-1,minlen=S.size()+1;
        int i=0,j=0;
        int k=0; //pointer for T.
        while(j<S.size()){
            if(S[j]==T[k]) k++;
            if(k==T.size()){ //matched, find the smallest window.
                i=j,k=T.size()-1;
                while(k>=0){
                    if(S[i]==T[k]) k--;
                    i--;
                }
                i++,k++;//restore : i the smallest window start, k=0
                int len=j-i+1;
                if(minlen>len){
                    minlen=len;
                    minind=i;
                }
                k=0,i++;//start a new search
                j=i;
            }
            j++;
        }
        if(minind<0) return "";
        return S.substr(minind,minlen);
    }
```	


more practice on min window:
- 209	Minimum Size Subarray Sum
see 662 Shortest subarray with sum at least K. (why we cannot use sliding window here with negative elements? since shrinking the window will not be able to get the min valid window).
- 1151	Minimum Swaps to Group All 1's Together   
- 727	Minimum Window Subsequence

### max window example:
340	Longest Substring with At Most K Distinct Characters
<em>Given a string s and an integer k, return the length of the longest substring of s that contains at most k distinct characters</em>
idea: growing right pointer until exceeding k unique chars, and then shrink window by moving left pointer to get valid window.
```
    int lengthOfLongestSubstringKDistinct(string s, int k) {
        unordered_map<char,int> mp;
        int i=0,j=0,ans=0;
        while(j<s.size()){
            mp[s[j]]++;
            while(mp.size()>k){
                mp[s[i]]--;
                if(mp[s[i]]==0) mp.erase(s[i]);
                i++;
            }
            if(mp.size()<=k) ans=max(ans,j-i+1);
            j++;
        }
        return ans;
    }
```

1156	Swap For Longest Repeated Character Substring
<em>Given a string text, we are allowed to swap two of the characters in the string. Find the length of the longest substring with repeated characters.
</em>

idea: try use each char as the most frequent char and find the longest window with 0 or 1 other chars (if there is one other char, we need have one more most frequent char outside the window).
```
    int maxRepOpt1(string text) {
        //sliding window with at most one other char, if there is one other char, need have the other char to swap outside the window
        if(text.empty()) return 0;
        vector<int> mp(26);
        for(char c: text) mp[c-'a']++;
        int n=text.size();
        int ans=1;
        int diff=0;
        for(char c='a';c<='z';c++){
            if(mp[c-'a']==0) continue;
            for(int i=0;i<n;i++){
                int j=i,cnt=0,diff=0;

                while(j<n && (text[j]==c||diff==0) && cnt<mp[c-'a']){
                    if(text[j]!=c) diff++;
                    cnt++,j++;
                }
                ans=max(ans,cnt);
            }
        }
        return ans;
    }
```


similar problems for more practices:
- 159	Longest Substring with At Most Two Distinct Characters
- 904	Fruit Into Baskets 
- 24	Longest Repeating Character Replacement
- 487	Max Consecutive Ones II 
- 1004	Max Consecutive Ones III
- 1493	Longest Subarray of 1's After Deleting One Element
- 1208	Get Equal Substrings Within Budget 
- 1695. Maximum Erasure value

### min-max window for counting valid subarray problem
counting number of valid subarray: get the longest subarray and shortest subarray at the same time using 3 pointers.

example:
992	Subarrays with K Different Integers 
find the number of subarrays with K different integers.
idea: using hashmap to record subarray's histogram i<j<k three pointers.
- grows k until invalid: move i to j+1 (the previous min window)
- grows j to get the min window.
- get the number of subarrays j-i+1.

```
    int subarraysWithKDistinct(vector<int>& A, int K) {
        int ans=0;
        int i=0,j=0,k=0;//use 3 pointers, i, j for the two left i<j
        unordered_map<int,int> mp;
        while(k<A.size())
        {
            mp[A[k]]++;
            if(mp.size()>K) {i=j+1;mp.erase(A[j]);}
            if(j<i) j=i;
            if(mp.size()==K) 
            {
                while(mp[A[j]]>1) {
                    mp[A[j]]--;
                    j++;
                }
                ans+=j-i+1;
            }
            k++;
        }
        return ans;
    }
```

similar problems:
- 1358	Number of Substrings Containing All Three Characters
- 713	Subarray Product Less Than K 
- 1040	Moving Stones Until Consecutive II 
