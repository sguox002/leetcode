# Chapter 6 Math

Part 2: still easy but need math thinking

168. Excel Sheet Column Title ***

math:  N=T(XY)=(X-'A'+1)*26+(Y-'A'+1)
N%26=Y-'A'+1, Y=N%26-1+'A', note N%26 will get 0 to 25, if we get 0, then we will get a char in front of A which is incorrect.
but we can change:
N-1=Y-'A'


```cpp
    string convertToTitle(int n) {
        //
        string s;
        while(n)
        {
            int d=(n-1)%26;
            s+='A'+d;
            n=(n-1)/26;
        }
        reverse(s.begin(),s.end());
        return s;
    }
```

171. Excel Sheet Column Number ***
note we shall add 1
A-1
Z-26

again 26 base
AA: c-'A'+1 is the number and then *26^n
math: (c-'A'+1)*26^n

```cpp
    int titleToNumber(string s) {
        //base 26
        int ans=0;
        for(char c: s)
        {
            ans*=26;
            ans+=c-'A'+1;
        }
        return ans;
    }
```	
	
166. Fraction to Recurring Decimal ***
hand division
a/b: if a<b, then a*10, if the remaining is the same, then it is repeating
2/3
1: 2<3, we get 0.
2: 20/3: we get 6, remaining 2

```cpp
    string fractionToDecimal(int num, int denom) {
		string ans;
        if(num==0) return "0";
		int sig1=num>=0?1:-1;
		int sig2=denom>=0?1:-1;
		long n=num,d=denom;
		n=abs(n),d=abs(d);
		long a=n/d,rem=n%d;

        vector<long> res;
		res.push_back(a);
		unordered_map<int,int> mp;
		mp[rem]=1;
		int i=1;
		while(rem)
		{
			rem*=10;
			res.push_back(rem/d);
			rem%=d;
			if(!mp.count(rem)) mp[rem]=res.size();
			else break;
		}
		if(!rem) //not recurring
		{
            if(sig1*sig2<0) ans="-";
			ans+=to_string(res[0]);
			if(res.size()>1) ans+=".";
			for(int i=1;i<res.size();i++) ans+=to_string(res[i]);
		}
		else
		{
			int j=mp[rem];
            if(sig1*sig2<0) ans="-";
			ans+=to_string(res[0]);
			if(res.size()>1) ans+=".";
			for(int i=1;i<res.size();i++) 
			{
				if(i!=j) ans+=to_string(res[i]);
				else ans+="("+to_string(res[i]);
			}
            ans+=")";
		}
		return ans;
    }
```	

some corner case:
1. 0/b: the sign is not useful
2. sign must be attached explicitly, if the integer part is 0, the sign is lost.


## Part 3: need more rigid math or thinking


13. Roman to Integer ***
a hashmap of the letter to the number
if its right>left, then
 we subtract the left
else we add the left
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
```

12. Integer to Roman ***
```cpp
    string intToRoman(int num) {
        string s;
        char digit[4]={0}; //store the digits in decimal
        char roman[]={'I','V','X','L','C','D','M'};
        int val[]={1,5,10,50,100,500,1000};

        //start from the msb digit
        int start_indx=6; //1000
        for(int i=1000;i>=1;i/=10,start_indx-=2)
        {
            int digit=num/i;
            if(!digit) continue;
            if(digit<4) s.append(digit,roman[start_indx]);
            else if(digit<5) s=s+roman[start_indx]+roman[start_indx+1];
            else if(digit<9) {s+=roman[start_indx+1];s.append(digit-5,roman[start_indx]);}
            else s=s+roman[start_indx]+roman[start_indx+2];
            num-=digit*i;
        }
        return s;
    }
```



60. Permutation Sequence ****
given number 1 to n, find its kth permutation
for n elements, it has n! permutations
we have n on the first, n-1 on the second, so it is reduced to n*sub(n-1)
we can locate the position for n, n-1, n-2....for each digit
```cpp
    string getPermutation(int n, int k) {
        string ans;
        vector<int> ms(n);
        for(int i=0;i<n;i++) ms[i]=i+1;
        k=k-1;
        while(ms.size())
        {
            int f=factorial(n-1);
            int d=k/f;
            ans+=ms[d]+'0';
            ms.erase(ms.begin()+d);
            k%=f;n--;
        }
        return ans;
    }
    int factorial(int n)
    {
        int ans=1;
        for(int i=2;i<=n;i++) ans*=i;
        return ans;
    }
```	

### 65. Valid Number
Validate if a given string can be interpreted as a decimal number.
note using stringstream cannot over the double range

The idea is pretty straightforward. A valid number is composed of the significand and the exponent (which is optional). As we go through the string, do the following things one by one:

skip the leading whitespaces;
check if the significand is valid. To do so, simply skip the leading sign and count the number of digits and the number of points. A valid significand has no more than one point and at least one digit.
check if the exponent part is valid. We do this if the significand is followed by 'e'. Simply skip the leading sign and count the number of digits. A valid exponent contain at least one digit.
skip the trailing whitespaces. We must reach the ending 0 if the string is a valid number.

```cpp
bool isNumber(const char *s) 
{
    int i = 0;
    
    // skip the whilespaces
    for(; s[i] == ' '; i++) {}
    
    // check the significand
    if(s[i] == '+' || s[i] == '-') i++; // skip the sign if exist
    
    int n_nm, n_pt;
    for(n_nm=0, n_pt=0; (s[i]<='9' && s[i]>='0') || s[i]=='.'; i++)
        s[i] == '.' ? n_pt++:n_nm++;       
    if(n_pt>1 || n_nm<1) // no more than one point, at least one digit
        return false;
    
    // check the exponent if exist
    if(s[i] == 'e') {
        i++;
        if(s[i] == '+' || s[i] == '-') i++; // skip the sign
        
        int n_nm = 0;
        for(; s[i]>='0' && s[i]<='9'; i++, n_nm++) {}
        if(n_nm<1)
            return false;
    }
    
    // skip the trailing whitespaces
    for(; s[i] == ' '; i++) {}
    
    return s[i]==0;  // must reach the ending 0 of the string
}
```



149. Max Points on a Line ****
Given n points on a 2D plane, find the maximum number of points that lie on the same straight line.
approach:
points on a line: kx+b=y, a line is determined by the k and a point on the line.
using hashmap to count the lines
1. use dy/dx as double slope
2. use {dy,dx} string as slope (which need to be simplified, divide by the gcd)
O(N^2) try all combination of all these points
note: 
if a b c are on the same line: a-b, a-c are counted, b-c is counted one extra. so the final is n*(n+1)/2
contain duplicates: duplicates are considered two points.
hash table shall not be a global but a local. 

```cpp
    int maxPoints(vector<vector<int>>& points) {
        int n=points.size();
		if(n<3) return n;
		int maxlen=0;
		for(int i=0;i<n;i++)
		{
			unordered_map<string,int> mp;
			int dup=0,vertical=0,localmax=0;
			for(int j=i+1;j<n;j++)
			{
				int dx=points[i][0]-points[j][0];
				int dy=points[i][1]-points[j][1];
				int f=gcd(dx,dy);
				double b=0;
				if(!dx && !dy) dup++;
                else if(!dx) vertical++;
				else //dx or dy==0 will not fall in
				{
					dx/=f,dy/=f;
					b=points[i][1]-dy*1.0/dx*points[i][0];
                    string s=to_string(dy)+"/"+to_string(dx)+","+to_string(b);
                    mp[s]++;
                    localmax=max(localmax,mp[s]);
				}
				localmax=max(localmax,vertical);
				
			}
            maxlen=max(maxlen,localmax+dup+1);
		}
        return maxlen;
    }
	int gcd(int a,int b)
	{
		if(!a) return b;
		return gcd(b%a,a);
	}
```
fixing one point:
1. duplicate point
2. vertical line
3. other lines



204. Count Primes ***
cross out all even numbers
cross out all multiples
this is a typical algorithm
simple one:

```cpp
    int countPrimes(int n) {
        vector<bool> mark(n+1,1);
        mark[0]=0; mark[1]=0;
        int ans=0;
        for(int i=2;i<n;i++)
        {
            if(mark[i])
            {
                ans++;
                for(long j=(long)i*i;j<n;j+=i) mark[j]=0;
            }
        }
        return ans;
    }
```
more efficient one:	(all are counted except those crossed
we only need go to sqrt(n) since it is the max factor.

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

223. Rectangle Area **
two rect overlap
```cpp
     int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        int left=max(A,E);
        int right=min(C,G);
        int top=max(B,F);
        int bott=min(D,H);
        int w=right>left?right-left:0; //extreme case right-left will be wrong!
        int h=bott>top?bott-top:0;
        int area=w*h;
        if(area<0) area=0;
        int tarea=(C-A)*(D-B)+(G-E)*(H-F);
        return tarea-area;
    }
```
224. Basic Calculator ****
supports + - for non-negative numbers and ()

Simple iterative solution by identifying characters one by one. One important thing is that the input is valid, which means the parentheses are always paired and in order.
Only 5 possible input we need to pay attention:

digit: it should be one digit from the current number
'+': number is over, we can add the previous number and start a new number
'-': same as above
'(': push the previous result and the sign into the stack, set result to 0, just calculate the new result within the parenthesis.
')': pop out the top two numbers from stack, first one is the sign before this pair of parenthesis, second is the temporary result before this pair of parenthesis. We add them together.
Finally if there is only one number, from the above solution, we haven't add the number to the result, so we do a check see if the number is zero.

```cpp
	int calculate(string s)
	{
		stack<int> st;
		int res=0,num=0,sign=1;
		for(char c: s)
		{
			if(isdigit(c)) num=10*num+(c-'0');//to avoid overflow add ()
			else if(c=='+')
			{	
				res+=sign*num;
				num=0,sign=1;
			}
			else if(c=='-')
			{
				res+=sign*num;
				num=0,sign=-1;
			}
			else if(c=='(')
			{
				st.push(res);
				st.push(sign);
				sign=1,res=0;
			}
			else if(c==')')
			{
				res+=sign*num;
				num=0;
				res*=st.top(),st.pop();
				res+=st.top(),st.pop();
			}
		}
		if(num) res+=sign*num;
		return res;
	}
```



	
233. Number of Digit One ***
for the ith digit, makes it 1, and divide to left and right
if the ith digit >1, then (left+1)*10^i (since the right can be any digit 0 to 9)
==1: then right can be from 0 to right (right+1) and left can be 0 to left (left+1)*10^i
<1, then we need borrow 1 from left, left*10^i
```cpp
	int countDigitOne(int n){
		int ans=0;
		if(!n) return 0;
		int i=0;
		int nn=n;
        int c=0;
		while(n)
		{
			int b=n%10;
			int a=n/10;
			if(b>1) ans+=(a+1)*pow(10,i);
			else if(b==1) ans+=a*pow(10,i)+(c+1);
			else ans+=a*pow(10,i);
			n/=10;
            c+=b*pow(10,i);
            i++;
		}
		return ans;
	}
```	

	
258. Add Digits ***
module property!
Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.
do it in O(1)
The problem, widely known as digit root problem, has a congruence formula:

https://en.wikipedia.org/wiki/Digital_root#Congruence_formula
For base b (decimal case b = 10), the digit root of an integer is:

dr(n) = 0 if n == 0
dr(n) = (b-1) if n != 0 and n % (b-1) == 0
dr(n) = n mod (b-1) if n % (b-1) != 0
or

dr(n) = 1 + (n - 1) % 9
```cpp
    int addDigits(int num) {
        return 1+(num-1)%9;
    }
```
about %:
c++ use a/b round to abs floor for example -2/3=0, 2/3=0
so -2%3=-1 2%3=2, so if one of a or b is negative, a%b is negative.
since 9%9=0 so we use (9-1)%9+1 to get 9.


263. Ugly Number **
only has 2,3,5 factor
recursive or iterative dividing 2,3,5 until is 1

```cpp
    bool isUgly(int num) {
        if(num<=0) return 0;
       while(num%2==0)  num/=2;
        while(num%3==0) num/=3;
        while(num%5==0) num/=5;
        return num==1;
    }
```
	
264. Ugly Number II ***
see dp
```cpp
    int nthUglyNumber(int n) {
		if(n==0) return 0;
		if(n==1) return 1;
		int i=0,j=0,k=0;
		vector<int> dp(n);
		dp[0]=1;
		for(int t=1;t<n;t++)
		{
			int curr=min(dp[i]*2,min(dp[j]*3,dp[k]*5));
			if(curr==dp[i]*2) i++;
			if(curr==dp[j]*3) j++;
			if(curr==dp[k]*5) k++;
			dp[t]=curr;
		}
		return dp[n-1];
    }
```

313. Super Ugly Number ***
approach 1: just use k pointers as in 264
approach 2: dp solution
```cpp
    int nthSuperUglyNumber(int n, vector<int>& primes) {
        vector<int> dp(n,INT_MAX); //the n super ugly number
        int m=primes.size(); //factors
        vector<int> ind(m);
        dp[0]=1; //1 is super ugly number
        for(int i=2;i<=n;i++)
        {
            for(int j=0;j<m;j++) dp[i-1]=min(dp[i-1],dp[ind[j]]*primes[j]);
            for(int j=0;j<m;j++) if(dp[i-1]==primes[j]*dp[ind[j]]) ind[j]++;
        }
        return dp[n-1];
    }
```	


268. Missing Number **
0 to n, one is missing.
xor 1 to n and identical one goes to 0
or add together
```cpp
    int missingNumber(vector<int>& nums) {
        int tsum=accumulate(nums.begin(),nums.end(),0);
        int n=nums.size();
        return n*(n+1)/2-tsum;
    }	
	
    int missingNumber(vector<int>& nums) {
        int ans=0,i=1;
        for(int t: nums) ans^=i^t,i++;
        return ans;
    }	
```	

273. Integer to English words ***
1000 is the subproblem.

```cpp
string LESS_THAN_20[] = {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
string TENS[] = {"", "Ten", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
string THOUSANDS[] = {"", "Thousand", "Million", "Billion"};


class Solution {
public:
    string numberToWords(int num) {
		if(num==0) return "Zero";
		string ans;
		int i=0;
		while(num)
		{
			if(num%1000)
				ans=helper(num%1000)+THOUSANDS[i]+" "+ans;
			num/=1000;
			i++;
		}
        while(ans.back()==' ') ans.pop_back();//remove trailing space
        
		return ans;
    }
	
	string helper(int num)
	{
        //cout<<num<<endl;
		if(num==0) return "";
		if(num<20) return LESS_THAN_20[num]+" ";
		if(num<100) return TENS[num/10]+" "+helper(num%10);
		return LESS_THAN_20[num/100]+" Hundred "+helper(num%100);
	}
```	

279. Perfect Squares ***
Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.
see dp for knapsack
```cpp
    int numSquares(int n) {
		vector<int> dp(n+1,INT_MAX);
        dp[0]=0;
		dp[1]=1; //1=1*1
		for(int i=2;i<=n;i++)
		{
			for(int j=sqrt(i);j>=1;j--)
				dp[i]=min(dp[i],dp[i-j*j]+1);
		}
		return dp[n];
    }
```	

319. Bulb Switcher ***
n bulbs from 1 to n, from 2 to n toggle all its multiples. Number of lights left.
A bulb ends up on if it is switched an odd number of times.

Call them bulb 1 to bulb n. Bulb i is switched in round d if and only if d divides i. So bulb i ends up on if and only if it has an odd number of divisors.

Divisors come in pairs, like i=12 has divisors 1 and 12, 2 and 6, and 3 and 4. Except when i is a square, like 36 has divisors 1 and 36, 2 and 18, 3 and 12, 4 and 9, and double divisor 6. So bulb i ends up on if and only if i is a square.

So just count the square numbers.

cards problem is very similar.


335. Self Crossing ***
You are given an array x of n positive numbers. You start at point (0,0) and moves x[0] metres to the north, then x[1] metres to the west, x[2] metres to the south, x[3] metres to the east and so on. In other words, after each move your direction changes counter-clockwise.

Write a one-pass algorithm with O(1) extra space to determine, if your path crosses itself, or not.

There are only two cases which not cross
1. keeps shrinking
2. keeps growing

if cross, it must first cross the recent loop. so we do not have to consider all previous
the idea is fixed the line 1
4th line cross line 1
5th line cross line 1
6th line cross line 1
7th line cross line 1 (same as 7th cross line 2)

```cpp
boolean isSelfCrossing(int[] x) {
        int l = x.length;
        if(l <= 3) return false;
        
        for(int i = 3; i < l; i++){
            if(x[i] >= x[i-2] && x[i-1] <= x[i-3]) return true;  //Fourth line crosses first line and onward
            if(i >=4)
            {
                if(x[i-1] == x[i-3] && x[i] + x[i-4] >= x[i-2]) return true; // Fifth line meets first line and onward
            }
            if(i >=5)
            {
                if(x[i-2] - x[i-4] >= 0 && x[i] >= x[i-2] - x[i-4] && x[i-1] >= x[i-3] - x[i-5] && x[i-1] <= x[i-3]) return true;  // Sixth line crosses first line and onward
            }
        }
        return false;
    }
```

343. Integer Break ****
Given a positive integer n, break it into the sum of at least two positive integers and maximize the product of those integers. 
Return the maximum product you can get
see dp
dp[i] is the max product get for i
dp[i]=max(dp[i],dp[j]*dp[i-j]) for all j

math solution:
assuming p=p1*p2*....*pk
for pi
if we break it into 2 and pi-2 2*(pi-2)>pi-->pi>4 
if we break it into 3 and pi-3 3*(pi-3)>pi-->pi>4.5
n=a*3+b*2 and the product is 3^a*2^b
to max the product, we need more 3 than 2
cauchy inequality: same or n n+1 will have max product.

```cpp
    public int integerBreak(int n) {
        if(n == 2)
            return 1;
        else if(n == 3)
            return 2;
        else if(n%3 == 0)
            return (int)Math.pow(3, n/3);
        else if(n%3 == 1)
            return 2 * 2 * (int) Math.pow(3, (n - 4) / 3);
        else 
            return 2 * (int) Math.pow(3, n/3);
    }
```
use 3 until we reach 4. that is the essence of this problem.


357. Count Numbers with Unique Digits ****
from 0 to 10^n get the number of unique digits
9*9*8*7*6...
Following the hint. Let f(n) = count of number with unique digits of length n.

f(1) = 10. (0, 1, 2, 3, ...., 9)

f(2) = 9 * 9. Because for each number i from 1, ..., 9, we can pick j to form a 2-digit number ij and there are 9 numbers that are different from i for j to choose from.

f(3) = f(2) * 8 = 9 * 9 * 8. Because for each number with unique digits of length 2, say ij, we can pick k to form a 3 digit number ijk and there are 8 numbers that are different from i and j for k to choose from.

Similarly f(4) = f(3) * 7 = 9 * 9 * 8 * 7....

...

f(10) = 9 * 9 * 8 * 7 * 6 * ... * 1

f(11) = 0 = f(12) = f(13)....
```cpp
    int countNumbersWithUniqueDigits(int n) {
      int num=0;
      if(!n) return 1;
      for(int i=1;i<=n;i++) num+=uniqueNumbers(i);
      return num;
    }
    int uniqueNumbers(int n)
    {
        if(n==1) return 10;
        int num=9;
        for(int i=1;i<n;i++) num*=(9-i+1);
        return num;
    }
```	


365. Water and Jug Problem ****
jug x and y, to measure z. we can only use x and y and x+y or x-y

```cpp
    bool canMeasureWater(int x, int y, int z) {
        //number theory: coprime: can get any number
        //has a gcd, then it can only get any number of N*gcd
        //for example 5,3: 5-3=2, 5-(3-2)=1
        if(z==x+y || z==x || z==y) return 1;
        if(!x ||!y) return 0;
        int g=gcd(x,y);
        return z%g==0 && z<=x+y;
    }
    int gcd(int a,int b)
    {
        //using euclid's theory
        if(!b) return a;
        return gcd(b,a%b);
    }
```

367. Valid Perfect Square  **
do not use sqrt()
1. n=k*k
2. binary search
3. a square number=1+3+5+...
```cpp
	bool isPerfectSquare(int num) {
		int l=1,r=num;
		while(l<=r)
		{
			int m=l+(r-l)/2;
			long t=(long)m*m;
			if(t==num) return 1;
			if(t<num) l=m+1;
			else r=m-1;
		}
		return 0;
	}
```
	
368. Largest Divisible Subset ****
subset numbers all satisfy Si%Sj=0
dp with backtrace
first sort it.
```cpp
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        
        vector<int> T(nums.size(), 0);
        vector<int> parent(nums.size(), 0);
        
        int m = 0;
        int mi = 0;
        
        // for(int i = 0; i < nums.size(); ++i) // if extending by larger elements
        for(int i = nums.size() - 1; i >= 0; --i) // iterate from end to start since it's easier to track the answer index
        {
            // for(int j = i; j >=0; --j) // if extending by larger elements
            for(int j = i; j < nums.size(); ++j)
            {
                // if(nums[i] % nums[j] == 0 && T[i] < 1 + T[j]) // if extending by larger elements
                // check every a[j] that is larger than a[i]
                if(nums[j] % nums[i] == 0 && T[i] < 1 + T[j])
                {
                    // if a[j] mod a[i] == 0, it means T[j] can form a larger subset by putting a[i] into T[j]
                    T[i] = 1 + T[j];
                    parent[i] = j;
                    
                    if(T[i] > m)
                    {
                        m = T[i];
                        mi = i;
                    }
                }
            }
        }
        
        vector<int> ret;
        
        for(int i = 0; i < m; ++i)
        {
            ret.push_back(nums[mi]);
            mi = parent[mi];
        }

        // sort(ret.begin(), ret.end()); // if we go by extending larger ends, the largest "answer" element will come first since the candidate element we observe will become larger and larger as i increases in the outermost "for" loop above.
       // alternatively, we can sort nums in decreasing order obviously. 
        
        return ret;
    }
```	

372. Super Pow ****
Your task is to calculate a^b mod 1337 where a is a positive integer and b is an extremely large positive integer given in the form of an array.
```cpp
/*One knowledge: ab % k = (a%k)(b%k)%k
Since the power here is an array, we'd better handle it digit by digit.
One observation:
a^1234567 % k = (a^1234560 % k) * (a^7 % k) % k = (a^123456 % k)^10 % k * (a^7 % k) % k
Looks complicated? Let me put it other way:
Suppose f(a, b) calculates a^b % k; Then translate above formula to using f :
f(a,1234567) = f(a, 1234560) * f(a, 7) % k = f(f(a, 123456),10) * f(a,7)%k;
Implementation of this idea: divide and conque
*/
class Solution {
    const int base = 1337;
    int powmod(int a, int k) //a^k mod 1337 where 0 <= k <= 10
    {
        a %= base;
        int result = 1;
        for (int i = 0; i < k; ++i)
            result = (result * a) % base;
        return result;
    }
public:
    int superPow(int a, vector<int>& b) {
        if (b.empty()) return 1;
        int last_digit = b.back();
        b.pop_back();
        return powmod(superPow(a, b), 10) * powmod(a, last_digit) % base;
    }
};
```

396. Rotate Function ****
Given an array of integers A and let n to be its length.

Assume Bk to be an array obtained by rotating the array A k positions clock-wise, we define a "rotation function" F on A as follow:

F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1].

Calculate the maximum value of F(0), F(1), ..., F(n-1).

approach
F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1]
F(k-1) = 0 * Bk-1[0] + 1 * Bk-1[1] + ... + (n-1) * Bk-1[n-1]
       = 0 * Bk[1] + 1 * Bk[2] + ... + (n-2) * Bk[n-1] + (n-1) * Bk[0]
Then,

F(k) - F(k-1) = Bk[1] + Bk[2] + ... + Bk[n-1] + (1-n)Bk[0]
              = (Bk[0] + ... + Bk[n-1]) - nBk[0]
              = sum - nBk[0]
Thus,

F(k) = F(k-1) + sum - nBk[0]

```cpp
    int maxRotateFunction(vector<int>& A) {
        //this is the inner product of two vectors
        //derived from the recursive relation F(k)-F(k-1)=Sum(A)-nA(n-k)
        int sum=0,allsum=0;
        int n=A.size();
        for(int i=0;i<n;i++) {sum+=A[i];allsum+=i*A[i];}//F(0) and sum(A)
        int maxsum=allsum;
        for(int i=1;i<n;i++)
        {
            allsum+=sum-n*A[n-i];
            if(allsum>maxsum) maxsum=allsum;
        }
        return maxsum;
    }
```

397. Integer Replacement ***
if n is even n/=2
if n is odd n++ or n--
repeat until n=1, return the min step

binary 01: we subtract 1->00
binary 11: we add 1->00
so we can reduce by half sooner
greedy based on math.
```cpp
    int integerReplacement(int n) {
        //using dynamic programming will not meet memory requirement
        int cnt=0;
        unsigned int nn=n;
        while(nn>1)
        {
            if(nn%2==0)
            {
                nn/=2;
            }
            else
            {
                if(((long long)nn+1)%4==0 && nn>3) nn++;
                else nn--;
            }
            cnt++;
        }
        return cnt;
     }
```	 

400. Nth Digit ****
find the nth digit for 1,2,3,....
one digit: 1-9: 9
two digit: 10-99: 90
three digits: 100-999: 900
accumulate these numbers and using binary search to find the number with nth digit

```cpp
long long f(int k) //give the number of digits, up to 31
{
    long long res=0;
	if(!k) return 1;
    for(int i=1;i<=k;i++) res+=i*(int)pow(10.0,i-1);
    return res*9;
}
long long ndigits[]={f(0),f(1),f(2),f(3),f(4),f(5),f(6),f(7),f(8),f(9)};//how many digits

class Solution {
public:
    int findNthDigit(int n) {
        if(n<10) return n;
        int ind=static_cast<int>(upper_bound(ndigits,ndigits+10,n)-ndigits);
        int n1=n-ndigits[ind-1];
        int num=n1/ind+(int)pow(10.0,ind-1)-1; //this is the number, starting from 999..9
        int num1=n1%ind; //this is the ind starting from the LSB
        if(n1%ind) num++;else num1=ind;
        
        return (num/(int)pow(10.0,ind-num1))%10;//(int)pow(10.0,num1-1);
    }
};
```
	
413. Arithmetic Slices ***
see dp
```cpp
    int numberOfArithmeticSlices(vector<int>& A) {
        int n = A.size();
        if (n < 3) return 0;
        vector<int> dp(n, 0); // dp[i] means the number of arithmetic slices ending with A[i]
        if (A[2]-A[1] == A[1]-A[0]) dp[2] = 1; // if the first three numbers are arithmetic or not
        int result = dp[2];
        for (int i = 3; i < n; ++i) {
            // if A[i-2], A[i-1], A[i] are arithmetic, then the number of arithmetic slices ending with A[i] (dp[i])
            // equals to:
            //      the number of arithmetic slices ending with A[i-1] (dp[i-1], all these arithmetic slices appending A[i] are also arithmetic)
            //      +
            //      A[i-2], A[i-1], A[i] (a brand new arithmetic slice)
            // it is how dp[i] = dp[i-1] + 1 comes
            if (A[i]-A[i-1] == A[i-1]-A[i-2]) 
                dp[i] = dp[i-1] + 1;
            result += dp[i]; // accumulate all valid slices
        }
        return result;
    }
```
	
423. Reconstruct Original Digits from English ****

first count those unique letters in each number
after they decided, minus it from other
```cpp
    string originalDigits(string s) {
        string digit[]={"zero","one","two","three","four","five","six","seven","eight","nine"};
        //letter's occurance if it only occurs once, then that number is definitely in
        //0 2 4 6 8 has distinct char inside. we can safely pick them out
        vector<int> cnt(26,0);
        for(int i=0;i<s.size();i++) cnt[s[i]-'a']++;
        vector<int> a(10,0); //to store the number of words
        a[0]=cnt['z'-'a'];
        a[2]=cnt['w'-'a'];
        a[4]=cnt['u'-'a'];
        a[6]=cnt['x'-'a'];
        a[8]=cnt['g'-'a'];
        a[3]=cnt['h'-'a']-a[8];
        a[5] = cnt['f'-'a'] - a[4];
        a[7] = cnt['v'-'a'] - a[5];
        a[1] = cnt['o'-'a'] - a[0] - a[2] - a[4];
        a[9] = cnt['i'-'a'] - a[5] - a[6] - a[8];   
        string res;
        for(int i=0;i<10;i++)
            res.append(a[i],i+'0');
        return res;
    }
```
	

	
458. poor pigs

There are 1000 buckets, one and only one of them is poisonous, while the rest are filled with water. They all look identical. If a pig drinks the poison it will die within 15 minutes. What is the minimum amount of pigs you need to figure out which bucket is poisonous within one hour?

Answer this question, and write an algorithm for the general case.

General case:

If there are n buckets and a pig drinking poison will die within m minutes, how many pigs (x) you need to figure out the poisonous bucket within p minutes? There is exactly one bucket with poison.

462. Minimum Moves to Equal Array Elements II ****
a move is to add +/-1 to an element
min moves to make the elements equal.
choose the median and make every number equal to median
Suppose you have two endpoints A and B, when you want to find a point C that has minimum sum of distance between AC and BC, the point C will always between A and B. Draw a graph and you will understand it. Lets keep moving forward. After we locating the point C between A and B, we can define that
dis(AC) = c - a; dis(CB) = b - c;
sum = dis(AC) + dis(CB) = b - a.
b - a will be a constant value, given specific b and a. Thus there will be no difference between points among A and B.

In this problem, we set two boundaries, saying i and j, and we move the i and j to do the computation.

```java
    public int minMoves2(int[] nums) {
        Arrays.sort(nums);
        int i = 0, j = nums.length-1;
        int count = 0;
        while(i < j){
            count += nums[j]-nums[i];
            i++;
            j--;
        }
        return count;
    }
```

478. Generate Random Point in a Circle ***
radius from 0 to r
angle from 0 to 359
x=r*cos(a)
y=r*sin(a)

however, if we use rand for radius, we will get much denser close to circle center. 
area is pi*r^2 we shall let it to be uniform in area so r^2 shall be uniform.

```cpp
    const double PI = 3.14159265358979732384626433832795;
    double m_radius, m_x_center, m_y_center;
		
    double uniform() {
        return (double)rand() / RAND_MAX;
    }
    
    Solution(double radius, double x_center, double y_center) {
        m_radius = radius; m_x_center = x_center; m_y_center = y_center;
    }
    
    vector<double> randPoint() {
        double theta = 2 * 3.14159265358979323846264 * uniform();
        double r = sqrt(uniform());
        return vector<double>{
            m_x_center + r * m_radius * cos(theta),
            m_y_center + r * m_radius * sin(theta)
        };
    }
```
1. how to generate random
2. how to make area uniform.	


483. Smallest Good Base ***
binary search or
math
```cpp
    string smallestGoodBase(string n) {
       //approach n=sum(k^i) =(k^(m+1)-1)/(k-1), k<n^(1/m)<k+1
       //so only need to consider n^(1/m) m from 2 to log2(n)
        long long nn=stoll(n);
        int max_m=log2(nn);
        int prek=0;
        //cout<<nn<<":"<<max_m;
        for(int m=max_m;m>1;m--)
        {
            int k=pow(nn,1.0/m);
            if(k==prek) continue;
            else prek=k;
            //long long t=(mypow(k,m+1)-1)/(k-1); //will overflow!
            if((nn-1)%k) continue;
            else
            {
                long long t=(mypow(k,m)-1)/(k-1); //will overflow!
                cout<<m<<" "<<k<<" "<<t<<endl;
                if((nn-1)/k==t) return to_string(k);//note power cannot be used since it does not have the precision
            }
        }
        return to_string(nn-1);
    }
    
    long long mypow(int k,int m)
    {
        long long res=1;
        for(int i=0;i<m;i++) res*=k;
        return res;
    }
```
507. Perfect Number ***
We define the Perfect Number is a positive integer that is equal to the sum of all its positive divisors except itself.
brutal force: up to sqrt(n)
be sure if i==num/i and it counts twice

```cpp
    bool checkPerfectNumber(int num) {
        if(num==1) return 0;
        int ans=1;
        for(int i=2;i<=sqrt(num);i++)
        {
            if(num%i==0)
            {
                if(i*i!=num) ans+=i+num/i;
                else ans+=i;
            }
        }
        return ans==num;
    }
```
above solution add the factor twice for n^2
note n=1 is a corner case.
	
517. Super Washing Machines ****
```cpp
    int findMinMoves(vector<int>& machines) {
        //approach: total number is not changed, final number is the mean, through distribution
        //less than target: can have incoming or outcoming, +1 or -1
        //larger than target: can only have outcoming, -1
        int sum=0;
        sum=accumulate(machines.begin(),machines.end(),0);
        if(sum%machines.size()) return -1;
        int target=sum/machines.size();
        //transform(machines.begin(),machines.end(),machines.begin(),)
        for(int i=0;i<machines.size();i++) machines[i]-=target;
        //all needs to be zero! the minimal total number of moves is max accumlated gain loss and individual gain/loss
        int cnt=0,min0=0;
        for(int i=0;i<machines.size();i++)
        {
            cnt+=machines[i];
            min0=max(min0,max(abs(cnt),machines[i]));//note loss does not account
        }
        return min0;
    }
```
	
523. Continuous Subarray Sum ***
target sum multiple of k
prefix sum and put the %k the same value together

O(N^2)
```cpp
     bool checkSubarraySum(vector<int>& nums, int k) {
        //accumulate first and then check (a-b)%k==0
        vector<int> accum(nums.size()+1,0);
        for(int i=0;i<nums.size();i++) accum[i+1]=accum[i]+nums[i];
        if(k)
        for(int i=0;i<accum.size();i++)
        {
            for(int j=i+2;j<accum.size();j++)
                if((accum[j]-accum[i])%k==0) return 1;
        }
        else
        for(int i=0;i<accum.size();i++)
        {
            for(int j=i+2;j<accum.size();j++)
                if(accum[j]==accum[i]) return 1;
        }
            
        return 0;
        
    }
```
use %k for O(N), note k=0 case
```cpp
    bool checkSubarraySum(vector<int>& nums, int k) {
        int n = nums.size(), sum = 0, pre = 0;
        unordered_set<int> modk;
        for (int i = 0; i < n; ++i) {
            sum += nums[i];
            int mod = k == 0 ? sum : sum % k;
            if (modk.count(mod)) return true;
            modk.insert(pre);
            pre = mod;
        }
        return false;
    }
```	
		 

535. Encode and Decode TinyURL ****
using 0-9 a-z A-Z to form a tinyURL
tinyURL includes 6 chars from above

using hashmap: 
tiny url vs real url
id vs tinyURL
everytime we increase the id when a new url comes
hash map implementation.

```cpp
//using numbers and digits total 6 char to form a dict from the url to tiny
//need come up an algorithm to hash the string into another string    
    // Encodes a URL to a shortened URL.
    //0-9a-zA-Z total 62 characters
public:
    string dict = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    int id = 0;
    unordered_map<string,string> m;  //key is longURL, value is shortURL
    unordered_map<int, string> idm;  //key is id in DB, value is longURL
    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        if(m.find(longUrl) != m.end())return m[longUrl];
        string res = "";
        id++;
        int count = id;
        while(count > 0)
        {
            res = dict[count%62] + res;
            count /= 62;
        }
        while(res.size() < 6)
        {
            res = "0" + res;
        }
        m[longUrl] = res;
        idm[id] = longUrl;
        return res;
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
        int id = 0;
        for(int i = 0; i < shortUrl.size(); i++)
        {
            id = 62*id + (int)(dict.find(shortUrl[i]));
        }
        if(idm.find(id) != idm.end())return idm[id];
        return "";
    }
```
	
592. Fraction Addition and Subtraction ***
need to find the lcm for all demom
final results need find the gcd
just the hand calculation on the fraction addition

```cpp
    string fractionAddition(string expression) {
        vector<int> num,denom;
        string st;
        for(int i=0;i<expression.size();i++)
        {
            char c=expression[i];
            if(c=='+' || c=='-')
            {
                if(!st.empty()) //process it
                {
                    int ind=st.find_first_of('/');
                    int a=stoi(st.substr(0,ind));
                    int b=stoi(st.substr(ind+1));
                    num.push_back(a);
                    denom.push_back(b);
                    st.clear();
                }
            }
            st+=c;
        }
        if(!st.empty()) //process it
        {
            int ind=st.find_first_of('/');
            int a=stoi(st.substr(0,ind));
            int b=stoi(st.substr(ind+1));
            num.push_back(a);
            denom.push_back(b);
            st.clear();
        }
        //find the lcm of the denominator
        int lcm0=denom[0];
        for(int i=1;i<denom.size();i++)
        {
            lcm0=lcm(lcm0,denom[i]);
        }
        for(int i=0;i<num.size();i++) num[i]*=lcm0/denom[i];
        int numsum=accumulate(num.begin(),num.end(),0);
        //int sign=numsum>=0?1:-1;
        int gcd0=gcd(abs(numsum),lcm0);
        numsum/=gcd0;
        lcm0/=gcd0;
        return to_string(numsum)+'/'+to_string(lcm0);
        
    }
    int gcd(int a,int b)
    {
        if(!b) return a;
        return gcd(b,a%b);
    }
    int lcm(int a,int b)
    {
        int div=gcd(a,b);
        return a*b/div;
    }
```
	





	
640. Solve the Equation
find the = and move all x items to left and non-x items to right
ax=b
hand solving the equations
```cpp
    string solveEquation(string equation) {
        //approach: lhs and rhs, move all x items to left and other to right
        int ind=equation.find_first_of('=');
        string lhs=equation.substr(0,ind);
        string rhs=equation.substr(ind+1);
        int a0=0,a1=0,b0=0,b1=0;
        process(lhs,a0,a1);
        process(rhs,b0,b1);
        //cout<<a0<<" "<<a1<<"="<<b0<<" "<<b1<<endl;
        //now the equation is ready
        int a=a0-b0,b=b1-a1;
        string ans;
        if(!a && !b) return "Infinite solutions";
        else if(!a && b) return "No solution";
        else {ans="x="+to_string((long long)b/a);return ans;} 
           
    }
    void process(string s,int& a,int& b) //a is the constant, b is the coefficient
    {
        //[+/-[0-9]x or +/-numbers]
        string st; //stack to store chars
        for(int i=0;i<s.size();i++)
        {
            char c=s[i];
            if(c=='+' || c=='-' ) //stop and look!
            {
                if(!st.empty()) 
                {                    
                    if(st.back()=='x') 
                    {
                        st.pop_back();
                        if(st.size()<1) st+='1';
						if(st.size()==1 && (st[0]=='+' || st[0]=='-')) st+='1';
                        a+=stoi(st);
                        st.clear();
                    }
                    else {b+=stoi(st);st.clear();}
                }
            }
            st+=c;
        }
        if(!st.empty()) //not processed yet
        {
            if(st.back()=='x') 
            {
                st.pop_back();
                if(st.size()<1) st+='1';
				if(st.size()==1 && (st[0]=='+' || st[0]=='-')) st+='1';
                a+=stoi(st);
                st.clear();
            }
            else {b+=stoi(st);st.clear();}
        }
    }
```

645. Set Mismatch ***
1 to n, one number is changed to another number. find the missing number
The idea is based on:
(1 ^ 2 ^ 3 ^ .. ^ n) ^ (1 ^ 2 ^ 3 ^ .. ^ n) = 0
Suppose we change 'a' to 'b', then all but 'a' and 'b' are XORed exactly 2 times. The result is then
0 ^ a ^ b ^ b ^ b = a ^ b
Let c = a ^ b, if we can find 'b' which appears 2 times in the original array, 'a' can be easily calculated by a = c ^ b.
```cpp
    public int[] findErrorNums(int[] nums) {
        int n = nums.length;
        int[] count = new int[n];
        int[] ans = {0,0};
        for(int i = 0; i < n; i++) {
            ans[1] ^= (i+1) ^ nums[i];
            if (++count[nums[i]-1] == 2)
                ans[0] = nums[i];
        }
        ans[1] ^= ans[0];
        return ans;
    }
```
find the duplicate using counting or marking.

```cpp
    vector<int> findErrorNums(vector<int>& nums) {
       //assume i is changed to j, then 1 to n xor nums will get i^j
        //j appeared twice, or marking the seens
        int dup,miss;
        for(int i=0;i<nums.size();i++)
        {
            int t=abs(nums[i]);
            if(nums[t-1]<0) dup=t;
            else nums[t-1]*=-1;
        }
        for(int i=0;i<nums.size();i++)
            if(nums[i]>0) miss=i+1;
        return {dup,miss};
    }
```	
	
670. Maximum Swap ***
Given a non-negative integer, you could swap two digits at most once to get the maximum valued number. Return the maximum valued number you could get.
a greedy choice
find the right max and swap with the first smaller.
```cpp
    int maximumSwap(int num) {
        //this is a selection sort
        string s=to_string(num);
        int len=s.size();
        //char maxc=*max_element(s.begin(),s.end());
        int pos;
        int firstpos=INT_MAX;
        for(int i=0;i<s.size();i++)
        {
            int ind=(int)(max_element(s.rbegin(),s.rbegin()+len-1-i)-s.rbegin());//this gives the first max
            ind=len-1-ind;
            if(s[i]<s[ind])
            {
                swap(s[i],s[ind]);
                break;
            }
        }
        //swap the last max with the first non-max
        return stoll(s);
    }
```
	


672. Bulb Switcher II ***
flips all lights
flips all odd lights
flip all even lights
flip all 3k+1 lights
here are three important observations:

For any operation, only odd or even matters, i.e. 0 or 1. Two same operations equal no operation.
The first 3 operations can be reduced to 1 or 0 operation. For example, flip all + flip even = flip odd. So the result of the first 3 operations is the same as either 1 operation or original.
The solution for n > 3 is the same as n = 3.
For example, 1 0 0 ....., I use 0 and 1 to represent off and on.
The state of 2nd digit indicates even flip; The state of 3rd digit indicates odd flip; And the state difference of 1st and 3rd digits indicates 3k+1 flip.
In summary, the question can be simplified as m <= 3, n <= 3. I am sure you can figure out the rest easily.
```cpp
    int flipLights(int n, int m) {
        if (m == 0 || n == 0) return 1;
        if (n == 1) return 2;
        if (n == 2) return m == 1? 3:4;
        if (m == 1) return 4;
        return m == 2? 7:8;
    }
```	



753. Cracking the Safe ****
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

754. Reach a Number ***
You are standing at position 0 on an infinite number line. There is a goal at position target.

On each move, you can either go left or right. During the n-th move (starting from 1), you take n steps.

Return the minimum number of steps required to reach the destination.
very similar to the dp problem of racing car

Step 0: Get positive target value (step to get negative target is the same as to get positive value due to symmetry).
Step 1: Find the smallest step that the summation from 1 to step just exceeds or equals target.
Step 2: Find the difference between sum and target. The goal is to get rid of the difference to reach target. For ith move, if we switch the right move to the left, the change in summation will be 2*i less. Now the difference between sum and target has to be an even number in order for the math to check out.
Step 2.1: If the difference value is even, we can return the current step.
Step 2.2: If the difference value is odd, we need to increase the step until the difference is even (at most 2 more steps needed).
Eg:
target = 5
Step 0: target = 5.
Step 1: sum = 1 + 2 + 3 = 6 > 5, step = 3.
Step 2: Difference = 6 - 5 = 1. Since the difference is an odd value, we will not reach the target by switching any right move to the left. So we increase our step.
Step 2.2: We need to increase step by 2 to get an even difference (i.e. 1 + 2 + 3 + 4 + 5 = 15, now step = 5, difference = 15 - 5 = 10). Now that we have an even difference, we can simply switch any move to the left (i.e. change + to -) as long as the summation of the changed value equals to half of the difference. We can switch 1 and 4 or 2 and 3 or 5.
```cpp
    int reachNumber(int target) {
        target=abs(target);
        int ans=0;
        int sum=0,i=0;
        while(sum<target) sum+=++i;
        while((sum-target)%2) //diff is odd
        {
            sum+=++i;
        }
        return i;
    }
```

775. Global and Local Inversions
if global==local
only local inversion exists. so the i-A[i] difference cannot be more than 1
```cpp
    bool isIdealPermutation(vector<int>& A) {
	for (int i = 0; i < A.size(); ++i) {
            if (abs(A[i] - i) > 1) return false;
        }
	return true;
    }
```
	
780. Reaching Points
A move consists of taking a point (x, y) and transforming it to either (x, x+y) or (x+y, y).

Given a starting point (sx, sy) and a target point (tx, ty), return True if and only if a sequence of moves exists to transform the point (sx, sy) to (tx, ty). Otherwise, return False.

If sx,sy occurs in the path of Euclidean method to get GCD (by subtracting lesser value from greater value) of tx,ty, then return true.

To see why this is true, consider how the tx, ty could have been formed if tx > ty. Let ax, ay be the pair in previous step. It cannot be ax, ax+ay because both ax and ay are greater than 0. So the only other possibility is ax+ay, ay. This means ay = ty and ax = tx-ty. Now we can optimize this subtraction a bit by doing ax = tx % ty since we will keep subtracting ty from tx until tx > ty.

One special case we need to handle during this optimization is when tx=9,ty=3,sx=6, sy=3 which can be covered using the condition if(sy == ty) return (tx - sx) % ty == 0;
Similar argument applies for tx <= ty.
```cpp
    bool reachingPoints(int sx, int sy, int tx, int ty) {
        while(tx >= sx && ty >= sy){
            if(tx > ty){
                if(sy == ty) return (tx - sx) % ty == 0;
                tx %= ty;
            }else{
                if(sx == tx) return (ty - sy) % tx == 0;
                ty %= tx;
            }
        }   
        
        return false;
    }
```
	
781. Rabbits in Forest
In a forest, each rabbit has some color. Some subset of rabbits (possibly all of them) tell you how many other rabbits have the same color as them. Those answers are placed in an array.

Return the minimum number of rabbits that could be in the forest.
For rabits with same colors, the answers must be the same. However, when the total amount of that same answer exceeds  'that answer + 1', there must be a new color. (say [3,3,3,3,3], the first four 3s indicates 4 rabits with the same color. The 5th 3 must represent a new color that contains 4 other rabits). We only calculate the amount of rabits with the same color once. Hashmap is used to record the frequency of the same answers. Once it exceeds the range, we clear the frequency and calculate again.

If x+1 rabbits have same color, then we get x+1 rabbits who all answer x.
now n rabbits answer x.
If n % (x + 1) == 0, we need n / (x + 1) groups of x + 1 rabbits.
If n % (x + 1) != 0, we need n / (x + 1) + 1 groups of x + 1 rabbits.

the number of groups is math.ceil(n / (x + 1)) and it equals to (n + x) / (x + 1) , which is more elegant.

```cpp
    int numRabbits(vector<int>& answers) {
        unordered_map<int,int> mp; //num-color vs number of rabits
        for(int i=0;i<answers.size();i++) mp[answers[i]+1]++;
        int num_rab=0;
        for(auto it=mp.begin();it!=mp.end();it++)
        {
                if(it->second%it->first) num_rab+=(it->second/it->first+1)*it->first;
                else num_rab+=it->second/it->first*it->first;
        }
        return num_rab;
    }
```

782. Transform to Chessboard
```cpp
    int movesToChessboard(vector<vector<int>>& board) {
        //check all rows against row 0, check all columns against col 0
        //save them in a vector in 1 0 format, 1: the same, 0: reverse, other invalid
        int n=board.size();
        vector<int> row(n,1),col(n,1);
        for(int i=1;i<n;i++)
        {
            int t=0;
            for(int j=0;j<n;j++) t+=board[i][j]^board[0][j];
            if(t!=n && t!=0) return -1;
            row[i]=t==0;
        }
        //copy(row.begin(),row.end(),ostream_iterator<int>(cout," "));cout<<endl;
        for(int j=1;j<n;j++)
        {
            int t=0;
            for(int i=0;i<n;i++) t+=board[i][j]^board[i][0];
            if(t!=n && t!=0) return -1;
            col[j]=t==0;
        }
        //copy(col.begin(),col.end(),ostream_iterator<int>(cout," "));cout<<endl;
        //check minimum swap for row and col to make it in chess order
        int m1=num_swap(row),m2=num_swap(col);
        return m1>=0 && m2>=0?m1+m2:-1;
    }
    int num_swap(vector<int>& row)
    {
        int n=row.size();
        int num1s=accumulate(row.begin(),row.end(),0); //1 and 0 shall same amount or differ by 1
        //n: even, must be the same, odd: differ by 1
        bool keep_1st=1;
        int ns=0;
        if(n%2==0) //for even, 1 or 0 can be both first
        {
            if(num1s*2!=n) return -1;
            int ns1=0;
            for(int i=0;i<n;i+=2) 
            {
                ns+=row[i]; //if the element is not 1, then need switch, they coul
                ns1+=!row[i];
            }
            ns=min(ns,ns1);
        }
        else
        {
            if(abs(num1s*2-n)!=1) return -1;
            if(num1s*2<n) keep_1st=0;//first row must be switched

            for(int i=0;i<n;i+=2) 
            {
                ns+=row[i]!=keep_1st; //if the element is not 1, then need switch, they coul
            }
          
        }
        return ns;
    }
```
Intuition:
Two conditions to help solve this problem:

In a valid chess board, there are 2 and only 2 kinds of rows and one is inverse to the other.
For example if there is a row 01010011 in the board, any other row must be either 01010011 or 10101100.
The same for columns
A corollary is that, any rectangle inside the board with corners top left, top right, bottom left, bottom right must be 4 zeros or 2 ones 2 zeros or 4 zeros.

Another important property is that every row and column has half ones. Assume the board is N * N:
If N = 2*K, every row and every column has K ones and K zeros.
If N = 2*K + 1, every row and every column has K ones and K + 1 zeros or K + 1 ones and K zeros.


Explanation:
Since the swap process does not break this property, for a given board to be valid, this property must hold.
These two conditions are necessary and sufficient condition for a valid chessboard.

Once we know it is a valid cheese board, we start to count swaps.
Basic on the property above, when we arange the first row, we are actually moving all columns.

I try to arrange one row into 01010 and 10101 and I count the number of swaps.

In case of N even, I take the minimum swaps, because both are possible.
In case of N odd, I take the even swaps.
Because when we make a swap, we move 2 columns or 2 rows at the same time.
So col swaps and row swaps should be same here.

Time Complexity:
O(N^2) to check the whole board.


C++:

    int movesToChessboard(vector<vector<int>>& b) {
        int N = b.size(), rowSum = 0, colSum = 0, rowSwap = 0, colSwap = 0;
        for (int i = 0; i < N; ++i) for (int j = 0; j < N; ++j)
                if (b[0][0]^b[i][0]^b[0][j]^b[i][j]) return -1;
        for (int i = 0; i < N; ++i) {
            rowSum += b[0][i];
            colSum += b[i][0];
            rowSwap += b[i][0] == i % 2;
            colSwap += b[0][i] == i % 2;
        }
        if (rowSum != N / 2 && rowSum != (N + 1) / 2) return -1;
        if (colSum != N / 2 && colSum != (N + 1) / 2) return -1;
        if (N % 2) {
            if (colSwap % 2) colSwap = N - colSwap;
            if (rowSwap % 2) rowSwap = N - rowSwap;
        }
        else {
            colSwap = min(N - colSwap, colSwap);
            rowSwap = min(N - rowSwap, rowSwap);
        }
        return (colSwap + rowSwap) / 2;
    }	

789. Escape The Ghosts
you and ghost can move 4 directions and move simultaneously. see if you can escape and reach the target position
Let's say you always take the shortest route to reach the target because if you go a longer or a more tortuous route, I believe the ghost has a better chance of getting you.
Denote your starting point A, ghost's starting point B, the target point C.
For a ghost to intercept you, there has to be some point D on AC such that AD = DB. Fix D. By triangle inequality, AC = AD + DC = DB + DC >= BC. What that means is if the ghost can intercept you in the middle, it can actually reach the target at least as early as you do. So wherever the ghost starts at (and wherever the interception point is), its best chance of getting you is going directly to the target and waiting there rather than intercepting you in the middle.

```cpp
    bool escapeGhosts(vector<vector<int>>& ghosts, vector<int>& target) {
        //to see who is closer to the target
        int min_dist=INT_MAX;
        int dx,dy;
        int dist=abs(target[0])+abs(target[1]);
        for(int i=0;i<ghosts.size();i++)
        {
            dx=abs(ghosts[i][0]-target[0]);
            dy=abs(ghosts[i][1]-target[1]);
            if(dx+dy<dist) return 0;
            min_dist=min(min_dist,dx+dy);
            //cout<<min_dist<<":"<<dx+dy<<endl;
        }
        
        return dist<min_dist;
    }
```
	
794. Valid Tic-Tac-Toe State
Players take turns placing characters into empty squares (" ").
The first player always places "X" characters, while the second player always places "O" characters.
"X" and "O" characters are always placed into empty squares, never filled ones.
The game ends when there are 3 of the same (non-empty) character filling any row, column, or diagonal.
The game also ends if all squares are non-empty.
No more moves can be played if the game is over.

so
When turns is 1, X moved. When turns is 0, O moved. rows stores the number of X or O in each row. cols stores the number of X or O in each column. diag stores the number of X or O in diagonal. antidiag stores the number of X or O in antidiagonal. When any of the value gets to 3, it means X wins. When any of the value gets to -3, it means O wins.

When X wins, O cannot move anymore, so turns must be 1. When O wins, X cannot move anymore, so turns must be 0. Finally, when we return, turns must be either 0 or 1, and X and O cannot win at same time.
```cpp
bool validTicTacToe(vector<string>& board) {
    bool wony = false, wonx = false;
    int rows = board.size();
    int cols = rows ? board[0].size() : 0;
    int diax = 0, diay = 0, xdiax = 0, xdiay = 0, xx = 0, yy = 0;
    for(int i = 0; i < rows; i++) 
    {
        int rowsx = 0, rowsy = 0;
        int colsx = 0, colsy = 0;
        for(int j = 0; j < cols; j++) 
        {
            // see current row
            if(board[i][j] == 'X') {rowsx++,xx++;}//xx:total x
            if(board[i][j] == 'O') {rowsy++, yy++;}//yy:total O
            // see ith column
            if(board[j][i] == 'X') colsx++;
            if(board[j][i] == 'O') colsy++;
        }
        
        // see both the diagonals
        if(board[i][i] == 'X') diax++;
        if(board[i][i] == 'O') diay++;
        if(board[i][cols - 1 - i] == 'X') xdiax++;
        if(board[i][cols - 1 - i] == 'O') xdiay++;
        
        if(rowsx == 3 || colsx == 3 || diax == 3 || xdiax == 3)
            wonx = true;
        
         if(rowsy == 3 || colsy == 3 || diay == 3 || xdiay == 3)
            wony = true;
    }
    
    if(xx < yy || !(xx == yy || xx == yy + 1))
        return false;
    
    if((wonx && wony) || (wonx && xx == yy) || (wony && xx != yy))
        return false;
    
    return true;
}
```

805. Split Array With Same Average
In a given integer array A, we must move every element of A to either list B or list C. (B and C initially start empty.)

Return true if and only if after such a move, it is possible that the average value of B is equal to the average value of C, and B and C are both non-empty.

```cpp
    bool splitArraySameAverage(vector<int>& A) {
        int n = A.size(), m = n/2, totalSum = accumulate(A.begin(), A.end(), 0);
        sort(A.rbegin(), A.rend()); // Optimization
        for (int i = 1; i <= m; ++i) 
            if (totalSum*i%n == 0 && combinationSum(A, 0, i, totalSum*i/n)) return true;
        return false;
    }
    bool combinationSum(vector<int>& nums, int idx, int k, int tar) {
        if (tar > k * nums[idx]) return false; // Optimization, A is sorted from large to small
        if (k == 0) return tar == 0;
        for (int i = idx; i <= nums.size()-k; ++i) 
            if (nums[i] <= tar && combinationSum(nums, i+1, k-1, tar-nums[i])) return true;
        return false;
    }
```
	
810. Chalkboard XOR Game
We are given non-negative integers nums[i] which are written on a chalkboard.  Alice and Bob take turns erasing exactly one number from the chalkboard, with Alice starting first.  If erasing a number causes the bitwise XOR of all the elements of the chalkboard to become 0, then that player loses.  (Also, we'll say the bitwise XOR of one element is that element itself, and the bitwise XOR of no elements is 0.)

Also, if any player starts their turn with the bitwise XOR of all the elements of the chalkboard equal to 0, then that player wins.

Return True if and only if Alice wins the game, assuming both players play optimally.

```cpp
    bool xorGame(vector<int>& nums) {
        int xo = 0;
        for (int i: nums) xo ^= i;
        return xo == 0 || nums.size() % 2 == 0;
    }
```



829. Consecutive Numbers Sum
number of ways to write a number to be a sum of consecuative numbers
```cpp
    int consecutiveNumbersSum(int N) {
        //it can have 1,2,3,4,5,...
        //M consecuative sum: m*(m+1)/2+m*x=N
        int cnt=1;
        for(int i=2;i<sqrt(2*N);i++)
        {
            if((N-i*(i-1)/2)%i==0) cnt++;
        }
        return cnt;
        
    }
```
	

	
858. Mirror Reflection
reflection by expanding the rect in 2 directions and problem becomes a simple gcd problem

```cpp
    int mirrorReflection(int p, int q) {
        int t=p*q/gcd(p,q);
        //cout<<t;
        int m=t/p,n=t/q;
        //n: the number of extension along x, m: the number of extension along y
        //m,n four combinations: 00,01,10,11
        if(n%2 && m%2==0)  return 0;
        if(n%2 && m%2) return 1;
        if(n%2==0 && m%2) return 2;
    }
    int gcd(int a, int b)
    {
        if(b==0) return a;
        else return gcd(b,a%b);
    }
```
	
866. Prime Palindrome
find the smallest prime palindrome number >=N
All palindrome with even digits is multiple of 11.
So among them, 11 is the only one prime
if (8 <= N <= 11) return 11
For other, we consider only palindrome with odd dights.

reduce the number digits by half.
```cpp
    int primePalindrome(int N) {
        if (8 <= N && N <= 11) return 11;
        for (int x = 1; x < 100000; ++x) {
            string s = to_string(x), r(s.rbegin(), s.rend());
            int y = stoi(s + r.substr(1));
            if (y >= N && isPrime(y)) return y;
        }
        return -1;
    }
    bool isPrime(int num) {
        if (num < 2 || num % 2 == 0) return num == 2;
        for (int i = 3; i * i <= num; i += 2)
            if (num % i == 0) return false;
        return true;
    }
```	


869. Reordered Power of 2
Starting with a positive integer N, we reorder the digits in any order (including the original order) such that the leading digit is not zero.

Return true if and only if we can do this in a way such that the resulting number is a power of 2.
store the digits in sorted order string
and then we iterate all 2^n to get same string
check if the two string equal
```cpp
    bool reorderedPowerOf2(int N) {
        if(N==1) return 1;
        vector<int> p2(32);
        vector<string> ps2(32);
        p2[0]=1;
        ps2[0]="1";
        string temp=to_string(N);
        sort(temp.begin(),temp.end());
        for(int i=1;i<32;i++) 
        {
            p2[i]=p2[i-1]*2;
            //cout<<p2[i]<<endl;
            ps2[i]=to_string(p2[i]);
            sort(ps2[i].begin(),ps2[i].end());
            cout<<ps2[i]<<endl;
            if(temp==ps2[i]) return 1;
        }
        return 0;
        
    }
```	

877. Stone Game
even number of piles, total sum is odd. taking from either end.
who will win
can use dp
If Alex wants to pick even indexed piles piles[0], piles[2], ....., piles[n-2],
he picks first piles[0], then Lee can pick either piles[1] or piles[n - 1].
Every turn, Alex can always pick even indexed piles and Lee can only pick odd indexed piles.

In the description, we know that sum(piles) is odd.
If sum(piles[even]) > sum(piles[odd]), Alex just picks all evens and wins.
If sum(piles[even]) < sum(piles[odd]), Alex just picks all odds and wins.

So, Alex always defeats Lee in this game.
always win
```cpp
    bool stoneGame(vector<int>& piles) {
       int n=piles.size();
       int l=0,r=n-1;
        //three variables: l, r and id, use -1 as flag is not good
       vector<vector<int>> dp(n,vector<int>(n,INT_MAX));
        return helper(piles,dp,0,n-1)>0;
    }
    
    int helper(vector<int>& piles,vector<vector<int>>& dp,int l,int r)
    {
        if(l>r) return 0;
        if(dp[l][r]!=INT_MAX) return dp[l][r];
        dp[l][r]=max(piles[l]-helper(piles,dp,l+1,r),piles[r]-helper(piles,dp,l,r-1));
        return dp[l][r];
    }
```
	
878. Nth Magical Number
approach  1: use gcd lcm to define l and r, and use binary search
note the binary search looks for the first true f,f,f,f...t,t...t,f,f,f,f

approach 2: the pattern repeats at the lcm
```cpp
    int nthMagicalNumber(int N, int A, int B) {
        int ans=0;
        int lcm=A*B/gcd(A,B);
        int mod=1e9+7;
        long l=min(A,B), r=N*l;
        while(l<r)
        {
            long m=l+(r-l)/2;
            int n=m/A+m/B-m/lcm;
            if(n<N) l=m+1;
            else r=m;
        }
        return l%mod;
    }
    int gcd(int a,int b)
    {
        if(!b) return a;
        return gcd(b,a%b);
    }
```

gcd only judge b==0 is fine since only b acts as the divisor
	

883. Projection Area of 3D Shapes
cube projection to xy, yz, xz plane and return the total area
```cpp
    int projectionArea(vector<vector<int>>& grid) {
        int xy=0,xz=0,yz=0;
        //xy we only care if it's z is 0 or not
        //xz we only care each y direction's max z: along row
        //yz we only care each x direction's max z: along column
        int n=grid.size();
        int total=0;
        vector<int> maxx(n,INT_MIN),maxy(n,INT_MIN);
        for(int i=0;i<n;i++) //row
        {
            for(int j=0;j<n;j++) //column
            {
                total+=(grid[i][j]>0); //xy plane
                maxx[i]=max(maxx[i],grid[i][j]); //xz: projection y
                maxy[j]=max(maxy[j],grid[i][j]); //yz: projection x
            }
            total+=maxx[i];
        }
        total+=accumulate(maxy.begin(),maxy.end(),0);
        return total;
    }
```    

885. Spiral Matrix III
starting from (r0,c0), walking clockwise
change direction and change step
```cpp
    vector<vector<int>> spiralMatrixIII(int R, int C, int r, int c) {
        vector<vector<int>> res = {{r, c}};
        int x = 0, y = 1, tmp;
        for (int n = 0; res.size() < R * C; n++) {
            for (int i = 0; i < n / 2 + 1; i++) {
                r += x, c += y;
                if (0 <= r && r < R && 0 <= c && c < C)
                    res.push_back({r, c});
            }
            tmp = x, x = y, y = -tmp;
        }
        return res;
    }
```

887. Super Egg Drop
binary search
or dp
```cpp
    int superEggDrop(int K, int N) {
        vector<int> dp(K + 1, 0);
        int s = 0;
        while (dp[K]<N)
        {
            for (int i = K; i > 0; i--)
            {
                dp[i] += dp[i - 1] + 1;
            }

            s++;
        }
        return s; 
    }
```
891. Sum of Subsequence Widths
Given an array of integers A, consider all non-empty subsequences of A.

For any sequence S, let the width of S be the difference between the maximum and minimum element of S.

Return the sum of the widths of all subsequences of A. 

As the answer may be very large, return the answer modulo 10^9 + 7.

```cpp
    int sumSubseqWidths(vector<int> A) {
        sort(A.begin(), A.end());
        long c = 1, res = 0, mod = 1e9 + 7;
        for (int i = 0; i < A.size(); ++i, c = (c << 1) % mod)
            res = (res + A[i] * c - A[A.size() - i - 1] * c) % mod;
        return (res + mod) % mod;
    }
```
	

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

902. Numbers At Most N Given Digit Set
```cpp
    int atMostNGivenDigitSet(vector<string>& D, int N) {
        string NS = to_string(N);
        int digit = NS.size(), dsize = D.size(), rtn = 0;
        
        for (int i = 1 ; i < digit ; ++i)
            rtn += pow(dsize, i);
        
        for (int i = 0 ; i < digit ; ++i) {
            bool hasSameNum = false;
            for (string &d : D) {
                if (d[0] < NS[i]) 
                    rtn += pow(dsize, digit - i - 1);
                else if (d[0] == NS[i]) 
                    hasSameNum = true;
            }
            if (!hasSameNum) return rtn;
        }               
        return rtn+1;
    }
```
906. Super Palindromes
itself and also a square of another palindrome
```cpp
    int superpalindromesInRange(string L, string R) {
     //R up to 10^5 since P<10^18  
        long l=stol(L);
        long r=stol(R);
        int limit=1e5;
        int ans=0;
        for(int i=1;i<limit;i++)
        {
            //odd and even arrange
            string s=to_string(i);
            string s_even=s+string(s.rbegin(),s.rend());
            //cout<<s_even<<" ";
            string s_odd=s+string(s.rbegin()+1,s.rend());
            
            long p=stol(s_even);
            p*=p;
            if(p>=l && p<=r)
            if(isPalindrome(p)) ans++;
            //cout<<s_odd<<endl;
            p=stol(s_odd);p*=p;
            if(p>=l && p<=r)
            if(isPalindrome(p)) ans++;
        }
        return ans;
    }
    bool isPalindrome(long x)
    {
        string s=to_string(x);
        int i=0,j=s.length()-1;
        while(i<j)
        {
            if(s[i]!=s[j]) return 0;
            i++,j--;
        }
        return 1;
    }
```
	
908. Smallest Range I
Given an array A of integers, for each integer A[i] we may choose any x with -K <= x <= K, and add x to A[i].

After this process, we have some array B.

Return the smallest possible difference between the maximum value of B and the minimum value of B.
math: max and min, we can max pull down by 2K. if the difference>0, then the min difference is max-min-2K
```cpp
    int smallestRangeI(vector<int>& A, int K) {
        int min0=*min_element(A.begin(),A.end());
        int max0=*max_element(A.begin(),A.end());
        int d=max0-min0-2*K;
        return d>0?d:0;
    }
```
	
910. Smallest Range II
add +k or -k
sort it first and then greedy
```cpp
    int smallestRangeII(vector<int>& A, int K) {
        sort(A.begin(), A.end());
        int res = A[A.size() - 1] - A[0];
        int left = A[0] + K, right = A[A.size() - 1] - K;
        for (int i = 0; i < A.size() - 1; i++) {
            int maxi = max(A[i] + K, right), mini = min(left, A[i + 1] - K);
            res = min(res, maxi - mini);
        }
        return res;
    }
```	


927. Three Equal Parts
```cpp
    vector<int> threeEqualParts(vector<int>& A) {
      vector<int> ones;
        for(int i=0;i<A.size();i++) if(A[i]) ones.push_back(i);
        if(ones.size()%3) return vector<int>({-1,-1});
        if(ones.size()==0) return vector<int>({1,A.size()-1});
        int len=ones.size()/3;
        //check if the three parts has the same pattern
        for(int i=0;i<len;i++)
        {
            if(ones[i]-ones[0]!=ones[i+len]-ones[len] || ones[i]-ones[0]!=ones[i+2*len]-ones[2*len])
                return vector<int>({-1,-1});
        }
        //the pattern is the same, we need check number of zeros
        int nzeros3=A.size()-1-ones.back();
        int nzeros2=ones[2*len]-ones[2*len-1]-1;
        int nzeros1=ones[len]-ones[len-1]-1;
        //cout<<nzeros1<<" "<<nzeros2<<" "<<nzeros3;
        if(nzeros1>=nzeros3 && nzeros2>=nzeros3)
        {
            return vector<int>({ones[len-1]+nzeros3,ones[2*len-1]+nzeros3+1});
        }
        return vector<int>({-1,-1});
    }
```	
952. Largest Component Size by Common Factor
union find
```cpp
    int largestComponentSize(vector<int>& A) {
        int n=A.size();
        sort(A.begin(),A.end());
        vector<int> parent(n),size(n,1);
        for(int i=0;i<n;i++) parent[i]=i;
        //vector<unordered_set<int>> factors(n);
        unordered_map<int,set<int>> factors;
        unordered_set<int> tset;
        //build the prime factors for each number
        for(int i=0;i<n;i++)
        {
            tset=factorize(A[i]);
            for(auto tt:tset) factors[tt].insert(i);
        }
        for(auto t:factors)
        {
            auto t0=t.second;
            if(t0.size()>=2)
            {
                int node=*t0.begin();
                for(auto it=++t0.begin();it!=t0.end();it++) merge(node,*it,parent,size);
            }
        }
        return *max_element(size.begin(),size.end());
    }
    int get_parent(int i,vector<int>&parent)
    {
        while(i!=parent[i]) i=parent[i];
        return i;
    }
    int merge(int i,int j,vector<int>& parent,vector<int>& size)
    {
        int pi=get_parent(i,parent);
        int pj=get_parent(j,parent);
        if(pi<pj) {parent[pj]=pi,size[pi]+=size[pj],size[pj]=0;}
        if(pi>pj) {parent[pi]=pj,size[pj]+=size[pi],size[pi]=0;}
        return min(pi,pj);
    }
    unordered_set<int> factorize(int x)
    {
        unordered_set<int> prime;
        int d=2;
        while(d*d<=x)
        {
            if(x%d==0) 
            {
                while(x%d==0) x/=d; //remove all its multiples
                prime.insert(d);
            }
            d++;
        }
        if(x>1 || prime.empty()) prime.insert(x); //if there is no factor, itself is a prime number
        return prime;
    }
```
	

963. Minimum Area Rectangle II
any four points
using the side as diagnonal and use the center point and the side length as the key to form a hashmap
these points are all on a circle.
```cpp
    double minAreaFreeRect(vector<vector<int>>& points) {
        unordered_map<string,vector<vector<int>>> mp; //key vs diagonal axis (two points' index)
        int n=points.size();
        for(int i=0;i<n;i++)
        {
            for(int j=i+1;j<n;j++)
            {
                int dx=points[i][0]-points[j][0];
                int dy=points[i][1]-points[j][1];
                long dist2=dx*dx+dy*dy;
                double cx=(points[i][0]+points[j][0])*0.5;
                double cy=(points[i][1]+points[j][1])*0.5;
                string s=to_string(dist2)+": "+to_string(cx)+", "+to_string(cy);
                //cout<<s<<endl;
                mp[s].push_back({i,j});
            }
        }
        double min_area=1e15;
        for(auto it=mp.begin();it!=mp.end();it++)
        {
            if(it->second.size()<2) continue;
            //there could be multiple rectangle inside
            //now we have 4 points in diagonal, p0-p2 diagonal, p1-p3 diagonal
            vector<vector<int>>& vrect=it->second;
            for(int i=0;i<vrect.size();i++)
            {
                for(int j=i+1;j<vrect.size();j++)
                {
                    int p0=vrect[i][0],p2=vrect[i][1];
                    int p1=vrect[j][0],p3=vrect[j][1];
                    int dx=points[p0][0]-points[p1][0];
                    int dy=points[p0][1]-points[p1][1];
                    long long dist01=dx*dx+dy*dy;
                    dx=points[p0][0]-points[p3][0];
                    dy=points[p0][1]-points[p3][1];
                    long long dist03=dx*dx+dy*dy;
                    double area=sqrt(double(dist01)*dist03);
                    min_area=min(min_area,area);
                }
            }
        }
        return min_area==1e15?0:min_area;
    }
```

964. Least Operators to Express Number
Given a single positive integer x, we will write an expression of the form x (op1) x (op2) x (op3) x ... where each operator op1, op2, etc. is either addition, subtraction, multiplication, or division (+, -, *, or /).  For example, with x = 3, we might write 3 * 3 / 3 + 3 - 3 which is a value of 3.

When writing such an expression, we adhere to the following conventions:

The division operator (/) returns rational numbers.
There are no parentheses placed anywhere.
We use the usual order of operations: multiplication and division happens before addition and subtraction.
It's not allowed to use the unary negation operator (-).  For example, "x - x" is a valid expression as it only uses subtraction, but "-x + x" is not because it uses negation.
```cpp
    int leastOpsExpressTarget(int x, int target) {
        //dp: x-ary number to use least operations
        //n=sum(Ai*X^i) to use less operator need to compare x^(i+1) and x^i
        vector<int> digits;
        while(target) {digits.push_back(target%x);target/=x;}
        
        int n=digits.size();
        digits.push_back(0);//could add a carrier flag before the MSB
        vector<int> pos(n+1),neg(n+1);
        pos[0]=digits[0]*2;
        neg[0]=(x-digits[0])*2; //x^0=x/x use more operations
        for(int i=1;i<=n;i++)
        {
            //if use neg, we have a carrier flag to i
            //Aix^i: add Ai times, each with i multplication
            pos[i]=digits[i]*i+min(pos[i-1],neg[i-1]+i); 
            //AiX^i=x^(i+1)-(x-Ai)x^i
            neg[i]=min((x-digits[i])*i+pos[i-1],(x-digits[i]-1)*i+neg[i-1]);
        }
        return min(pos[n],neg[n])-1;
    }
```



972. Equal Rational Numbers
expand to exceed double precision

```cpp
    bool isRationalEqual(string S, string T) {
        return f(S) == f(T);
    }

    double f(string S) {
        auto i = S.find("(");
        if (i != string::npos) {
            string base = S.substr(0, i);
            string rep = S.substr(i + 1, S.length() - i - 2);
            for (int j = 0; j < 20; ++j) base += rep;
            return stod(base);
        }
        return stod(S);
    }
```	



991. Broken Calculator
On a broken calculator that has a number showing on its display, we can perform two operations:

Double: Multiply the number on the display by 2, or;
Decrement: Subtract 1 from the number on the display.
Initially, the calculator is displaying the number X.

Return the minimum number of operations needed to display the number Y.
```cpp
    int brokenCalc(int X, int Y) {
        //every step we can choose -1 or *2
        //greedy solution: 
        //if Y is even, divide by 2, if Y is odd add 1
        int res = 0;
        while (Y > X) {
            Y = Y % 2 > 0 ? Y + 1 : Y / 2;
            res++;
        }
        return res + X - Y;        
    }
```

1006. Clumsy Factorial
```cpp
    int clumsy(int N) {
       int ans=N;
        char op[]={'*','/','+','-'};
        stack<int> st;
        int j=0;
        for(int i=N-1;i>=1;i--)
        {
            switch(op[j%4])
            {
                case '*': ans*=i;break;
                case '/': ans/=i;break;
                case '+': if(st.size()) {ans=st.top()-ans;st.pop();}ans+=i;break;
                case '-': st.push(ans);ans=i;break;
            }
            //cout<<ans<<endl;
            j++;
        }
        if(st.size()) {ans=st.top()-ans;st.pop();}
        return ans;
    }
```
mathematic: 4 as a group, and do not need extra data structure.
(n+1)*n/(n-1)=n+1 for n>=5



1012. Numbers With Repeated Digit
Given a positive integer N, return the number of positive integers less than or equal to N that have at least 1 repeated digit.

Count res the Number Without Repeated Digit
Then the number with repeated digits = N - res

Similar as
788. Rotated Digits
902. Numbers At Most N Given Digit Set

Explanation:
Transform N + 1 to arrayList
Count the number with digits < n
Count the number with same prefix
For example,
if N = 8765, L = [8,7,6,6],
the number without repeated digit can the the following format:
XXX
XX
X
1XXX ~ 7XXX
80XX ~ 86XX
870X ~ 875X
8760 ~ 8765

Time Complexity:
the number of permutations A(m,n) is O(1)
We count digit by digit, so it's O(logN)

```cpp
    int permutation(int m, int n) {
        return n == 0 ? 1 : permutation(m, n - 1) * (m - n + 1);
    }
public:
    int numDupDigitsAtMostN(int N) {
        vector<int> digit;
        for (int i = N + 1; i > 0; i /= 10) {
            digit.push_back(i % 10);
        }
        reverse(digit.begin(), digit.end());
        
        int result = 0;
        int n = digit.size();
        for (int i = 1; i < n; i++) {
            result += 9 * permutation(9, i - 1);
        }
        
        // Count the number with same prefix
        unordered_set<int> seen;
        for (int i = 0; i < n; i++) {
            for (int j = i > 0 ? 0 : 1; j < digit[i]; j++) {
                if (seen.find(j) == seen.end()) {
                    result += permutation(9 - i, n - i - 1);
                }
            }
            if (seen.find(digit[i]) != seen.end()) {
                break;
            }
            seen.insert(digit[i]);
        }
        return N - result;
    }
```

1015. Smallest Integer Divisible by K
Given a positive integer K, you need find the smallest positive integer N such that N is divisible by K, and N only contains the digit 1.

Return the length of N.  If there is no such N, return -1.

Intuition:
Let's say the final result has N digits of 1.
If N exist, N <= K, just do a brute force check.
Also if K % 2 == 0, return -1, because 111....11 can't be even.
Also if K % 5 == 0, return -1, because 111....11 can't end with 0 or 5.


Explanation
For different N, we calculate the remainder of mod K.
It has to use the remainder for these two reason:

Integer overflow
The division operation for big integers, is NOT O(1), it's actually depends on the number of digits..
Prove
Why 5 is a corner case? It has a reason and we can find it.
Assume that N = 1 to N = K, if there isn't 111...11 % K == 0
There are at most K - 1 different remainders: 1, 2, .... K - 1.

So this is a pigeon holes problem:
There must be at least 2 same remainders.

Assume that,
f(N) ≡ f(M), N > M
f(N - M) * 10 ^ M ≡ 0
10 ^ M ≡ 0, mod K
so that K has factor 2 or factor 5.

Proof by contradiction，
If (K % 2 == 0 || K % 5 == 0) return -1;
otherwise, there must be a solution N <= K.

Time Complexity:
Time O(K), Space O(1)
```cpp
    int smallestRepunitDivByK(int K) {
        for (int r = 0, N = 1; N <= K; ++N)
            if ((r = (r * 10 + 1) % K) == 0)
                return N;
        return -1;
    }
```

1017. Convert to Base -2
similar as 2 ,but we shall use N>>1 for negatives


```cpp
    string baseNeg2(int N) {
        string res = "";
        while (N) {
            res = to_string(N & 1) + res;
            N = -(N >> 1);
        }
        return res == "" ? "0" : res;
    }
```

