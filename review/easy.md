189. Rotate Array
approach1: double the array and return begin()+k for n elements
approach2: k to end and then followed by 0 to k
approach 3:
in-place O(1) memory approach:
brutal force: each element's position is known (i+k)%n. we need store the original value to temp
a[0] to a[n-k-1] shift to a[k]-a[n-1]
a[n-k] to a[n-1] move to a[0]-a[k-1]
approach 4:
first reverse first n-k elements
2nd reverse the last k elements
reverse the whole
most easy to implement

```cpp
    void rotate(vector<int>& nums, int k) {
        int n=nums.size();
        k%=n;
        reverse(nums.begin(),nums.begin()+n-k);
        reverse(nums.begin()+n-k,nums.end());
        reverse(nums.begin(),nums.end());
    }
```
traps: need make k<n

190. Reverse Bits
1. don't misunderstand the question. It ask reverse, not 1 to 0 and 0 to 1
2. it needs the trailing and leading 0s to fill 32 bit
```cpp    
    uint32_t reverseBits(uint32_t n) {
        uint32_t ans=0;
        for(int i=0;i<32;i++)
        {
            ans*=2;
            int t=n%2;
            n/=2;
            ans+=t;
        }
        return ans;
    }
```
be sure to double first so that the bit31 is not doubled.
