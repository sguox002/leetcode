# Chapter 6 Math
### part 1-easy
2. Add Two Numbers *
linked list
```cpp
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int cf=0;
        ListNode* root=0,*prev;
        while(l1 || l2 || cf)
        {
            int val=(l1?l1->val:0)+(l2?l2->val:0)+cf;
            cf=val/10;
            ListNode* p=new ListNode(val%10);
            if(l1) l1=l1->next;
            if(l2) l2=l2->next;
            if(!root) root=p;else prev->next=p;
            prev=p;
        }
        
        return root;
    }
```	

7. Reverse Integer *
using string, note there is a sign
```cpp
    int reverse(int x) {
        if(!x) return 0;
        long long ans=0;
        while(x)
        {
            ans*=10;
            int d=x%10;
            ans+=d;
            x/=10;
        }
        if(ans>INT_MAX || ans<INT_MIN) return 0;
        return ans;
    }
```

8. String to Integer (atoi) *
```cpp
    int myAtoi(string str) {
      long long res=0; //shall have a large container
	  int sign=1;
      bool found_sign=0;
      bool found_digit=0;
	  for(int i=0;i<str.size();i++)
	  {
		  char c=str[i];
		  if(c=='-') {if(!found_sign) {sign=-1;found_sign=1;}else break;}
		  else if(c=='+' ) {if(!found_sign) {sign=1;found_sign=1;}else break;}
		  else if(c>='0' && c<='9') {found_digit=1;res=res*10+c-'0';if(res>INT_MAX) break;}
		  else if(c==' ' || c=='\t') if(!found_digit && !found_sign) continue;else break;
		  else break;
	  }
	  //overflow error dealing
	  //if(sign*res>INT_MAX || sign*res<INT_MIN) return 0;
	  if(res<0) if(sign>0) return INT_MAX;else return INT_MIN;
	  if(sign*res>INT_MAX) return INT_MAX;
	  if(sign*res<INT_MIN) return INT_MIN;
	  return sign*res;
    }
```
or use stringstream
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

9. Palindrome Number *
convert to string and reverse to see if equal

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

29. Divide Two Integers ***
no division
use subtraction. to speed up the subtraction we can double the divisor
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
	
43. Multiply Strings **
naive approach using hand multiplication
```cpp
    string multiply(string num1, string num2) {
        //single s[i]*s[j] will contribute to res[i+j] and res[i+j+1]
        //and then accumulate
        reverse(num1.begin(),num1.end());
        reverse(num2.begin(),num2.end());
        vector<int> res(num1.size()+num2.size());
        for(int i=0;i<num1.size();i++)
        {
            for(int j=0;j<num2.size();j++)
            {
                res[i+j]+=(num1[i]-'0')*(num2[j]-'0');
            }
        }
        int cf=0;
        string ans;
        for(int i=0;i<res.size();i++)
        {
            int d=res[i]+cf;
            ans+=(d%10)+'0';
            cf=d/10;
        }
        while(ans.size()>1 && ans.back()=='0') ans.pop_back();
        reverse(ans.begin(),ans.end());
        return ans;
    }
```

50. Pow(x, n) **
divide and conque, x^n=x^2*x^(n/2)
```cpp
    double myPow(double x, int n) {
        if(n==0) return 1;
        long long nn=n;
        if(n<0) {x=1/x;nn=-nn;}
        return nn%2?x*myPow(x*x,nn/2):myPow(x*x,nn/2);
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

67. Add Binary *
reverse and add

69. Sqrt(x) **
searching for the upper bound for m*m>x
see binary search

149. Max Points on a Line
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

166. Fraction to Recurring Decimal
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

168. Excel Sheet Column Title ***
pay attention to starting index 0
1-A
26-Z
27-AA 
the base is 26.  (27-1)%26-->10
0+1->A
1-1->0->A

consider the letter 'A' to have a value of 1, 'B'->2 ..... 'Z'->26
note that in the above notation, values are 1-based

here our Radix (R) == 26

the final value of a number X Y Z = X * R^2 + Y * R + Z

this looks similar to base-10 decimal number but the biggest difference is that the numbers on every digit starts with 1, instead of 0., and the max on each digit goes up to R (Radix) instead of R-1

for example
Z== Radix
then next number is AA = R + 1 = Z+1
ZZ = R * R + R
next number is AAA = 1*R^2 + 1 * R + 1 = ZZ +1

so from the AAA notation to their sequence number (decimal) it's easy, but the other way is a bit tricky due to the way % and / operates

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

172. Factorial Trailing Zeroes **
n/2 2s n/5 5s so we need count number of 5s

```cpp
    int trailingZeroes(int n) {
       //counter number of 5s 
        int n5s=0;
        for(int i=1;i<=n/5;i++)
        {
            n5s++;
            int t=i;
            while(t%5==0) {t/=5;n5s++;}
        }
        return n5s;
    }
```

202. Happy Number
A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.
detect a cycle using hashset

```cpp
    bool isHappy(int n) {
        unordered_set<int> ms;
        while(n!=1)
        {
            if(ms.count(n)) return 0;
            ms.insert(n);
            string s=to_string(n);
            n=0;
            for(char c: s) n+=(c-'0')*(c-'0');
            
        }
        return 1;
    }
```

204. Count Primes ***
cross out all even numbers
cross out all multiples
this is a typical algorithm
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

223. Rectangle Area
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
224. Basic Calculator
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


231. Power of Two
n&(n-1)

326. Power of Three
3^k is factor of 3^max 

233. Number of Digit One
Given an integer n, count the total number of digit 1 appearing in all non-negative integers less than or equal to n.
log(n) algorithm
divide and conquer

For every digit in n (Suppose n = 240315, the digits are 2, 4, 0, 3, 1, 5)，I respectively count the number of digit 1 assuming the position of current digit is 1 and other digits of n is arbitrary.

For example, I select 3 in n as the current digit, and I suppose the position of 3 is 1.

The highn is the number composed with the digits before the current digit. In the example, highn = 240;

The lown is the number composed with the digits after the current digit. In the example, lown = 15.

The lowc = 10 ^ (the number of lower digits). In the example, lowc = 100;

As curn = 3 and curn > 1, (highn * 10 + 1) must be less than (highn * 10 + curn). Then the higher part can be 0 ~ highn, the lower part can be 0 ~ (lowc-1), and the current result = (highn + 1) * lowc.

```cpp
int countDigitOne(int n) {
        long long int res(0);
        int highn(n), lowc(1), lown(0);
        while(highn > 0){
            int curn = highn % 10;
            highn = highn / 10;
            if(1 == curn){
                //higher: 0~(highn-1);  lower:  0 ~ (lowc-1)
                res += highn * lowc;
                //higher: highn ~ highn;     lower:0~lown
                res += lown + 1;
            }else if(0 == curn){  
                //curn < 1
               //higher: 0~(highn-1);  lower:  0 ~ (lowc-1)
                res += highn * lowc;
            }else{              
                //curn > 1
                res += (highn + 1) * lowc;
            }
            //update lown and lowc
            lown = curn * lowc + lown;
            lowc = lowc * 10;
        }
        return res;
    }
```
	
258. Add Digits
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
	

263. Ugly Number
only has 2,3,5 factor
recursive or iterative dividing 2,3,5 until is 1

```cpp
    bool isUgly(int num) {
        if(!num) return 0;
       while(num%2==0)  num/=2;
        while(num%3==0) num/=3;
        while(num%5==0) num/=5;
        return num==1;
    }
```
	
264. Ugly Number II
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

313. Super Ugly Number
just replace k pointers using dp


268. Missing Number
0 to n, one is missing.
xor 1 to n and identical one goes to 0
or add together
```cpp
    int missingNumber(vector<int>& nums) {
        int tsum=accumulate(nums.begin(),nums.end(),0);
        int n=nums.size();
        return n*(n+1)/2-tsum;
    }	
```	

273. Integer to English words
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

279. Perfect Squares
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

319. Bulb Switcher
n bulbs from 1 to n, from 2 to n toggle all its multiples. Number of lights left.
A bulb ends up on if it is switched an odd number of times.

Call them bulb 1 to bulb n. Bulb i is switched in round d if and only if d divides i. So bulb i ends up on if and only if it has an odd number of divisors.

Divisors come in pairs, like i=12 has divisors 1 and 12, 2 and 6, and 3 and 4. Except when i is a square, like 36 has divisors 1 and 36, 2 and 18, 3 and 12, 4 and 9, and double divisor 6. So bulb i ends up on if and only if i is a square.

So just count the square numbers.

335. Self Crossing
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

343. Integer Break
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
	
357. Count Numbers with Unique Digits
from 0 to 10^n get the number of unique digits
9*9*8*7*6...


	
942. DI String Match
return any permutation matching the DI string
one solution: We track high (h = N - 1) and low (l = 0) numbers within [0 ... N - 1]. When we encounter 'I', we insert the current low number and increase it. With 'D', we insert the current high number and decrease it. In the end, h == l, so we insert that last number to complete the premutation.
this is a greedy solution or two pointers

728. Self Dividing Numbers
A self-dividing number is a number that is divisible by every digit it contains.
just using brutal force

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

908. Smallest Range I
Given an array A of integers, for each integer A[i] we may choose any x with -K <= x <= K, and add x to A[i].

After this process, we have some array B.

Return the smallest possible difference between the maximum value of B and the minimum value of B.
math: max and min, we can max pull down by 2K. if the difference>0, then the min difference is max-min-2K

868. Binary Gap
find the largest distance of 1 in its binary representation
straightforward

892. Surface Area of 3D Shapes
surface area of 3d shape
calculate the stacked cube surface area
minus its neighboring (i-1,j) and (i,j-1)
```cpp
    int surfaceArea(vector<vector<int>> grid) {
        int res = 0, n = grid.size();
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j]) res += grid[i][j] * 4 + 2;
                if (i) res -= min(grid[i][j], grid[i - 1][j]) * 2;
                if (j) res -= min(grid[i][j], grid[i][j - 1]) * 2;
            }
        }
        return res;
    }
```

812. Largest Triangle Area
the area of a triangle using 3 points: the area formula also works for other shape (polygon)
```cpp
    double largestTriangleArea(vector<vector<int>>& points) {
        int area=0,max_area=0;
        int m=points.size();
        for(int i=0;i<m;i++)
        {
            for(int j=i+1;j<m;j++)
            {
                for(int k=j+1;k<m;k++)
                {
                    area=points[i][0]*points[j][1]-points[j][0]*points[i][1];
                    area+=points[j][0]*points[k][1]-points[k][0]*points[j][1];
                    area+=points[k][0]*points[i][1]-points[i][0]*points[k][1];
                    if(max_area<abs(area)) max_area=abs(area);
                }    
            }
        }
        return 0.5*max_area;
    }
```    




453. Minimum Moves to Equal Array Elements
Given a non-empty integer array of size n, find the minimum number of moves required to make all array elements equal, where a move is incrementing n - 1 elements by 1.

sum + m * (n - 1) = x * n
x = minNum + m

598. Range Addition II
        //seems to find the most overlapped region
        //since it always covers the (0,0), we only need track the right bottom point
        //it is the min of all the x and min of all the y
        

628. Maximum Product of Three Numbers
sort the array. It may contains negatives.
sort the whole array which is O(nlogn)
just find the min 2 and max 3 which is O(n)
```java
    public int maximumProduct(int[] nums) {
        int max1 = Integer.MIN_VALUE, max2 = Integer.MIN_VALUE, max3 = Integer.MIN_VALUE, min1 = Integer.MAX_VALUE, min2 = Integer.MAX_VALUE;
        for (int n : nums) {
            if (n > max1) {
                max3 = max2;
                max2 = max1;
                max1 = n;
            } else if (n > max2) {
                max3 = max2;
                max2 = n;
            } else if (n > max3) {
                max3 = n;
            }

            if (n < min1) {
                min2 = min1;
                min1 = n;
            } else if (n < min2) {
                min2 = n;
            }
        }
        return Math.max(max1*max2*max3, max1*min1*min2);
    }
```

836. Rectangle Overlap
        //x1-x2 interval and x3-x4 interval shall overlap
        //y1-y2 interval and y3-y4 interval shall overlap
rec1[0]<rec2[2] && rec2[0]<rec1[2] && rec1[1]<rec2[3] && rec2[1]<rec1[3];



415. Add Strings
reverse and add



66. Plus One
MSB is at the front, so we add from the end

645. Set Mismatch
1 to n, one number is changed to another number. find the missing number
The idea is based on:
(1 ^ 2 ^ 3 ^ .. ^ n) ^ (1 ^ 2 ^ 3 ^ .. ^ n) = 0
Suppose we change 'a' to 'b', then all but 'a' and 'b' are XORed exactly 2 times. The result is then
0 ^ a ^ b ^ b ^ b = a ^ b
Let c = a ^ b, if we can find 'b' which appears 2 times in the original array, 'a' can be easily calculated by a = c ^ b.

367. Valid Perfect Square
1. n=k*k
2. binary search
3. a square number=1+3+5+...

441. Arranging Coins
n coins to arrange it as 1,2,3,4....
m(m+1)/2<=n





914. X of a Kind in a Deck of Cards
count card number for each number and find the gcd>1
gcd for a list of number is the gcd of the gcd with the numnber

507. Perfect Number
We define the Perfect Number is a positive integer that is equal to the sum of all its positive divisors except itself.
brutal force: up to sqrt(n)
be sure if i==num/i and it counts twice

633. Sum of Square Numbers
check if a number is sum of two squares
a^2+b^2=c, we just search one from 1 to sqrt(c/2) for a and once a is fixed we can find b

949. Largest Time for Given Digits
need to consider the time as a whole. just use all the permutation and get the largest valid time

754. Reach a Number
You are standing at position 0 on an infinite number line. There is a goal at position target.

On each move, you can either go left or right. During the n-th move (starting from 1), you take n steps.

Return the minimum number of steps required to reach the destination.
very similar to the dp problem of racing car

Step 0: Get positive target value (step to get negative target is the same as to get positive value due to symmetry).
Step 1: Find the smallest step that the summation from 1 to step just exceeds or equalstarget.
Step 2: Find the difference between sum and target. The goal is to get rid of the difference to reach target. For ith move, if we switch the right move to the left, the change in summation will be 2*i less. Now the difference between sum and target has to be an even number in order for the math to check out.
Step 2.1: If the difference value is even, we can return the current step.
Step 2.2: If the difference value is odd, we need to increase the step until the difference is even (at most 2 more steps needed).
Eg:
target = 5
Step 0: target = 5.
Step 1: sum = 1 + 2 + 3 = 6 > 5, step = 3.
Step 2: Difference = 6 - 5 = 1. Since the difference is an odd value, we will not reach the target by switching any right move to the left. So we increase our step.
Step 2.2: We need to increase step by 2 to get an even difference (i.e. 1 + 2 + 3 + 4 + 5 = 15, now step = 5, difference = 15 - 5 = 10). Now that we have an even difference, we can simply switch any move to the left (i.e. change + to -) as long as the summation of the changed value equals to half of the difference. We can switch 1 and 4 or 2 and 3 or 5.
```java
    public int reachNumber(int target) {
        target = Math.abs(target);
        int step = 0;
        int sum = 0;
        while (sum < target) {
            step++;
            sum += step;
        }
        while ((sum - target) % 2 != 0) {
            step++;
            sum += step;
        }
        return step;
    }
```



400. Nth Digit
find the nth digit for 1,2,3,....
one digit: 1-9: 9
two digit: 10-99: 90
three digits: 100-999: 900
accumulate these numbers and using binary search to find the number with nth digit




	
### medium


535. Encode and Decode TinyURL
using 0-9 a-z A-Z to form a tinyURL
tinyURL includes 6 chars from above

using hashmap: 
tiny url vs real url
id vs tinyURL
everytime we increase the id when a new url comes

537. Complex Number Multiplication
read the real and imag and perform multiplication 
read using stringstream

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

877. Stone Game
even number of piles, total sum is odd. taking from either end.
who will win
can use dp
sum(piles(even)) < sum(piles[odd]), pick all odd
sum(piles(even)) > sum(piles[odd]), pick all even
always win

553. Optimal Division
perform float division. by adding parenthesis to reach max result
X1/X2/X3/../Xn will always be equal to (X1/X2) * Y, no matter how you place parentheses. i.e no matter how you place parentheses, X1 always goes to the numerator and X2 always goes to the denominator. Hence you just need to maximize Y. And Y is maximized when it is equal to X3 *..*Xn. So the answer is always X1/(X2/X3/../Xn) = (X1 *X3 *..*Xn)/X2

413. Arithmetic Slices
see dp

789. Escape The Ghosts
you and ghost can move 4 directions and move simultaneously. see if you can escape and reach the target position
Let's say you always take the shortest route to reach the target because if you go a longer or a more tortuous route, I believe the ghost has a better chance of getting you.
Denote your starting point A, ghost's starting point B, the target point C.
For a ghost to intercept you, there has to be some point D on AC such that AD = DB. Fix D. By triangle inequality, AC = AD + DC = DB + DC >= BC. What that means is if the ghost can intercept you in the middle, it can actually reach the target at least as early as you do. So wherever the ghost starts at (and wherever the interception point is), its best chance of getting you is going directly to the target and waiting there rather than intercepting you in the middle.

462. Minimum Moves to Equal Array Elements II
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

858. Mirror Reflection
reflection by expanding the rect in 2 directions and problem becomes a simple gcd problem

781. Rabbits in Forest
In a forest, each rabbit has some color. Some subset of rabbits (possibly all of them) tell you how many other rabbits have the same color as them. Those answers are placed in an array.

Return the minimum number of rabbits that could be in the forest.
For rabits with same colors, the answers must be the same. However, when the total amount of that same answer exceeds  'that answer + 1', there must be a new color. (say [3,3,3,3,3], the first four 3s indicates 4 rabits with the same color. The 5th 3 must represent a new color that contains 4 other rabits). We only calculate the amount of rabits with the same color once. Hashmap is used to record the frequency of the same answers. Once it exceeds the range, we clear the frequency and calculate again.

If x+1 rabbits have same color, then we get x+1 rabbits who all answer x.
now n rabbits answer x.
If n % (x + 1) == 0, we need n / (x + 1) groups of x + 1 rabbits.
If n % (x + 1) != 0, we need n / (x + 1) + 1 groups of x + 1 rabbits.

the number of groups is math.ceil(n / (x + 1)) and it equals to (n + x) / (x + 1) , which is more elegant.

672. Bulb Switcher II
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


869. Reordered Power of 2
store the digits in sorted order string
and then we iterate all 2^n to get same string
check if the two string equal




592. Fraction Addition and Subtraction
need to find the lcm for all demom
final results need find the gcd

423. Reconstruct Original Digits from English
first count those unique letters in each number
after they decided, minus it from other



593. Valid Square
4 points if form a square
C(4,2) to calculate the side length, 4 same and 2 same

using hashset to count only two



640. Solve the Equation
find the = and move all x items to left and non-x items to right
ax=b

670. Maximum Swap
a greedy choice
find the right max and swap with the first smaller.

963. Minimum Area Rectangle II
any four points
using the side as diagnonal and use the center point and the side length as the key to form a hashmap
these points are all on a circle.

775. Global and Local Inversions
if global==local
only local inversion exists. so the i-A[i] difference cannot be more than 1



372. Super Pow
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


396. Rotate Function
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

368. Largest Divisible Subset
subset numbers all satisfy Si%Sj=0
dp with backtrace



397. Integer Replacement
if n is even n/=2
if n is odd n++ or n--
repeat until n=1, return the min step

binary 01: we subtract 1->00
binary 11: we add 1->00
so we can reduce by half sooner





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

365. Water and Jug Problem.
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



523. Continuous Subarray Sum
target sum multiple of k
prefix sum and put the %k the same value together

910. Smallest Range II
add +k or -k
sort it first and then greedy

166. Fraction to Recurring Decimal
need to find the repeat pattern

866. Prime Palindrome
find the smallest prime palindrome number >=N
All palindrome with even digits is multiple of 11.
So among them, 11 is the only one prime
if (8 <= N <= 11) return 11
For other, we consider only palindrome with odd dights.

reduce the number digits by half.






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

753. Cracking the Safe
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

810. Chalkboard XOR Game
