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
