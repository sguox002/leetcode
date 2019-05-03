# Chapter 6 Math

some easy questions to gain some experience and refresh some knowledge

## Part 1: straightforward with min knowledge

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
```cpp
    bool isPalindrome(int x) {
        string s=to_string(x);
        string rs=s;
        reverse(s.begin(),s.end());
        return rs==s;
    }
```

29. Divide Two Integers **
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

67. Add Binary *
reverse and add
```cpp
    string addBinary(string a, string b) {
        string ans;
        int i=a.size()-1,j=b.size()-1;
        int cf=0;
        while(i>=0 || j>=0 || cf)
        {
            int d=(i>=0?a[i]-'0':0)+(j>=0?b[j]-'0':0)+cf;
            cf=d/2;
            ans+='0'+d%2;
            i--,j--;
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
```
	
69. Sqrt(x) **
searching for the upper bound for m*m>x
see binary search
```cpp
    int mySqrt(int x) {
		int l=0,r=x;
		while(l<r)
		{
			int m=l+((long)r-l+1)/2;
			long t=(long)m*m;
			if(t<=x) l=m;
			else r=m-1;
		}
		return l;
	}
```

172. Factorial Trailing Zeroes **
n/2 2s n/5 5s so we need count number of 5s
1-'A'

```cpp
    int trailingZeroes(int n) {
        //depends how many 5s inside
        int ans=0;
        while(n)
        {
            ans+=n/5;
            n/=5;
        }
        return ans;
    }
```

202. Happy Number **
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

231. Power of Two *
n&(n-1): corner case: n<0 and & is low priority.
```cpp
    bool isPowerOfTwo(int n) {
        if(n<=0) return false;
        return !(n&(n-1));
    }
```	
326. Power of Three *
3^k is factor of 3^max 
```cpp
    bool isPowerOfThree(int n) {
        if(!n) return 0;
        while(n%3==0) n/=3;
        return n==1;
    }
```
note brutal force: n=0 case will not come out of the loop.

441. Arranging Coins **
n coins to arrange it as 1,2,3,4....
m(m+1)/2<=n
can use binary search
math approach
```
    int arrangeCoins(int n) {
        //the m(m+1)<=2*n, ceil(sqrt(2*n)-1)
        int m=sqrt((long long)2*n);
        if(m*(m+1)>2*n) return m-1;
        return m;
    }
```
	
453. Minimum Moves to Equal Array Elements
Given a non-empty integer array of size n, find the minimum number of moves required to make all array elements equal, where a move is incrementing n - 1 elements by 1.

sum + m * (n - 1) = x * n
x = minNum + m (the min adds m times)
m=sum-n*minNum;
greedy + math
 pay attention to int overflow

```cpp
    int minMoves(vector<int>& nums) {
      //assume final move to a number t: sum+m(n-1)=t*n, t=min+m
        int n=nums.size();
        long sum=accumulate(nums.begin(),nums.end(),0l);
        long mn=*min_element(nums.begin(),nums.end());
        return sum-n*mn;
    }
```

537. Complex Number Multiplication *
read the real and imag and perform multiplication 
read using stringstream
```cpp
    string complexNumberMultiply(string a, string b) {
        stringstream sa(a),sb(b);
        int r1,i1,r2,i2;
        char op;
        sa>>r1>>op>>i1;
        sb>>r2>>op>>i2;
        int r=r1*r2-i1*i2,i=r1*i2+r2*i1;
        return to_string(r)+'+'+to_string(i)+"i";
    }
```
	
553. Optimal Division **
perform float division. by adding parenthesis to reach max result
X1/X2/X3/../Xn will always be equal to (X1/X2) * Y, no matter how you place parentheses. 
i.e no matter how you place parentheses, X1 always goes to the numerator and X2 always goes to the denominator. 
Hence you just need to maximize Y. 
And Y is maximized when it is equal to X3 *..*Xn. So the answer is always X1/(X2/X3/../Xn) = (X1 *X3 *..*Xn)/X2

```cpp
    string optimalDivision(vector<int>& nums) {
        if(nums.size()<1) return string();
        if(nums.size()==1) return to_string(nums[0]);
        string res=to_string(nums[0])+"/";
        if(nums.size()>2) res+='(';
        
        for(int i=1;i<nums.size();i++)
            res+=to_string(nums[i])+'/';
        res.pop_back();
        if(nums.size()>2) res+=')';
        return res;
        
    }
```	

593. Valid Square ***
4 points if form a square
C(4,2) to calculate the side length, 4 same and 2 same

using hashset to count only two
```cpp
    bool validSquare(vector<int>& p1, vector<int>& p2, vector<int>& p3, vector<int>& p4) {
        unordered_map<int,int> side2;
        vector<vector<int>> pts;
        pts.push_back(p1);pts.push_back(p2);pts.push_back(p3);pts.push_back(p4);
        for(int i=0;i<pts.size();i++)
        {
            for(int j=i+1;j<pts.size();j++)
            {
                int dx=pts[i][0]-pts[j][0];
                int dy=pts[i][1]-pts[j][1];
                side2[dx*dx+dy*dy]++;
            }
        }
        //4 sides the same, 2 sides the same
        if(side2.size()==2)
        {
            if(side2.begin()->second==2 || side2.begin()->second==4) return 1;
        }
        return 0;
    }
```

598. Range Addition II *
easy. just get the min x and y1-y2
```cpp
    int maxCount(int m, int n, vector<vector<int>>& ops) {
        int mx=m,my=n;
        for(auto t: ops)
        {
            mx=min(mx,t[0]);
            my=min(my,t[1]);
        }
        return mx*my;
    }
```

628. Maximum Product of Three Numbers
```cpp
    int maximumProduct(vector<int>& nums) {
        //sort it first, the max product must be chosen from the min 2 *max and the max 3
        vector<int> a;
        sort(nums.begin(),nums.end());

        int n=nums.size();
        int t1=nums[n-1]*nums[n-2]*nums[n-3];
        int t2=nums[0]*nums[1]*nums[n-1];
        return max(t1,t2);
    }
```

not using sorting to find the 6 numbers
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

633. Sum of Square Numbers ***
check if a number is sum of two squares
a^2+b^2=c, we just search one from 1 to sqrt(c/2) for a and once a is fixed we can find b
```cpp
    bool judgeSquareSum(int c) {
        //approach: search from 1 to sqrt(c/2)
        for(int i=0;i<=sqrt(c/2);i++)
        {
            int b=sqrt(c-i*i);
            if(i*i+b*b==c) return 1;
        }
        return 0;
    }
```

728. Self Dividing Numbers *
A self-dividing number is a number that is divisible by every digit it contains.
just using brutal force	
```cpp
    vector<int> selfDividingNumbers(int left, int right) {
        vector<int> res;
        for (int i = left, n = 0; i <= right; i++) {
            for (n = i; n > 0; n /= 10)
                if (!(n % 10) || i % (n % 10)) break;
            if (!n) res.push_back(i);
        }
        return res;
    }
```	

812. Largest Triangle Area
the area of a triangle using 3 points: the area formula also works for other shape (polygon)
for convex or non-convex, choose a point and form several triangles (clockwise will get positive, counter-clockwise will get negative)
note: we shall use the abs since negative is valid.
the area of triangle: 
assume ABC are in clockwise, then area:
=0.5*(Ax(By-Cy)+Bx*(Cy-Ay)+Cx(Ay-By))

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

836. Rectangle Overlap
        //x1-x2 interval and x3-x4 interval shall overlap
        //y1-y2 interval and y3-y4 interval shall overlap
rec1[0]<rec2[2] && rec2[0]<rec1[2] && rec1[1]<rec2[3] && rec2[1]<rec1[3];
```cpp
    bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2) {
        int x1=rec1[0],x2=rec1[2],x3=rec2[0],x4=rec2[2];
        int y1=rec1[1],y2=rec1[3],y3=rec2[1],y4=rec2[3];
        return max(x1,x3)<min(x2,x4) && max(y1,y3)<min(y2,y4);
    }
```
two 1d interval overlap problem (2d)



868. Binary Gap
find the largest distance of 1 in its binary representation
straightforward
```cpp
    int binaryGap(int N) {
       int ans=0;
        bitset<32> bits(N);
        int prev=-1;
        for(int i=0;i<32;i++) 
        {
            if(bits[i])
            {
                if(prev!=-1) ans=max(ans,i-prev);
                prev=i;
            }
        }
        return ans;
    }
```
	
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

914. X of a Kind in a Deck of Cards
count card number for each number and find the gcd>1
gcd for a list of number is the gcd of the gcd with the numnber
```cpp
    bool hasGroupsSizeX(vector<int>& deck) {
        unordered_map<int, int> count;
        int res = 0;
        for (int i : deck) count[i]++;
        for (auto i : count) res = __gcd(i.second, res);
        return res > 1;
    }
```



942. DI String Match
return any permutation matching the DI string
one solution: We track high (h = N - 1) and low (l = 0) numbers within [0 ... N - 1]. When we encounter 'I', we insert the current low number and increase it. With 'D', we insert the current high number and decrease it. In the end, h == l, so we insert that last number to complete the premutation.
this is a greedy solution or two pointers
```cpp
    vector<int> diStringMatch(string S) {
        //DI sequence, backtrace of dp 
        //we fill I: the min the D the max
        int n=S.length();
        vector<int> ans(n+1);
        int min0=0,max0=n;
        for(int i=0;i<S.length();i++)
        {
            if(S[i]=='I') ans[i]=min0++;
            else ans[i]=max0--;
        }
        if(S.back()=='I') ans[n]=min0;
        else ans[n]=max0;
        return ans;
    }
```	

949. Largest Time for Given Digits
need to consider the time as a whole. just use all the permutation and get the largest valid time
```cpp
string largestTimeFromDigits(vector<int>& A) {
        int maxtime=-1;
        sort(A.begin(),A.end());
        do{
            if(isvalid(A)) maxtime=max(maxtime,A[0]*1000+A[1]*100+A[2]*10+A[3]);
        }while(next_permutation(A.begin(),A.end()));
        if(maxtime<0) return "";
        string ans(5,':');
        ans[0]=maxtime/1000+'0';
        ans[1]=(maxtime%1000)/100+'0';
        ans[3]=(maxtime%100)/10+'0';
        ans[4]=(maxtime%10)+'0';
        return ans;
    }
    bool isvalid(vector<int>& A)
    {
        int hr=A[0]*10+A[1];;
        int min0=A[2]*10+A[3];
        return hr<24 && min0<60;
    }
```
or use string compare
```cpp
    string largestTimeFromDigits(vector<int>& A) {
        sort(A.begin(),A.end());
        //try all combinations
        string ans;
        do{
            if(isValid(A))
                ans=max(ans,to_string(A[0])+to_string(A[1])+':'+to_string(A[2])+to_string(A[3]));
        }while(next_permutation(A.begin(),A.end()));
        return ans;
    }
    bool isValid(vector<int>& A)
    {
        int hr=A[0]*10+A[1];
        int min=A[2]*10+A[3];
        return hr<24 && min<60;
    }
```	

970. Powerful Integers
Given two positive integers x and y, an integer is powerful if it is equal to x^i + y^j for some integers i >= 0 and j >= 0.

Return a list of all powerful integers that have value less than or equal to bound.

You may return the answer in any order.  In your answer, each value should occur at most once.

```cpp
    vector<int> powerfulIntegers(int x, int y, int bound) {
        if(bound<2) return vector<int>();
        if(x==1 && y==1) return vector<int>({2});
        if(x==1 || y==1) //bound-1=y^i
        {
            vector<int> ans;
            int xx=1;
            x=max(x,y);
            for(int i=0;i<=log(bound-1)/log(y);i++)
            {
                if(i) xx*=x;
                ans.push_back(xx+1);   
            }
            return ans;
        }
        int m=log(bound-1)/log(x),n=log(bound-1)/log(y);
        unordered_set<int> ans;
        int xx=1,yy=1;
        for(int i=0;i<=m;i++)
        {
            if(i) xx*=x;
            yy=1;
            for(int j=0;j<=n;j++)
            {
                if(j) yy*=y;
                //cout<<xx+yy<<" ";
                if(xx+yy<=bound) ans.insert(xx+yy);
            }
        }
        return vector<int>(ans.begin(),ans.end());
    }
```
more concise:
```cpp
    vector<int> powerfulIntegers(int x, int y, int bound) {
        vector<int> ans;
        for(int i=2;i<=bound;i++)
        {
            bool found=0;
            for(int a=1;a<bound;a*=x)
            {
                for(int b=1;a+b<=bound;b*=y)
                {
                    if(a+b==i) {ans.push_back(i);found=1;break;}
                    if(y==1) break;
                }
                if(found || x==1) break;
            }
        }
        return ans;
    }
```
note: for x or y==1 the loop will not come out
one number may have more than one solution so when we find we need break the loop	

976. Largest Perimeter Triangle
```cpp
    int largestPerimeter(vector<int>& A) {
       //a triangle: a+b>c a-b<c
        sort(A.begin(),A.end(),greater<int>());
        for(int i=0;i<A.size()-2;i++)
        {
            if(A[i]<A[i+1]+A[i+2]) return A[i]+A[i+1]+A[i+2];
        }
        return 0;
    }
```

1009. Complement of Base 10 Integer
```
    int bitwiseComplement(int N) {
        int ans=0;
        int i=0;
        if(N==0) return 1;
        while(N)
        {
            if(N%2==0) {ans+=(1<<i);}
            N/=2;i++;
        }
        return ans;
    }	
```

1025. Divisor Game
Alice and Bob take turns playing a game, with Alice starting first.

Initially, there is a number N on the chalkboard.  On each player's turn, that player makes a move consisting of:

Choosing any x with 0 < x < N and N % x == 0.
Replacing the number N on the chalkboard with N - x.
Also, if a player cannot make a move, they lose the game.

Return True if and only if Alice wins the game, assuming both players play optimally.

math fact: odd number only has odd factors, even number must have an even factor.

	
	
