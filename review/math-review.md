A lot of problems are greedy based on mathematics.

# part 1-easy

## rating *

### 942. DI String Match
greedy
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
	
### 728. Self Dividing Numbers
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

### 908. Smallest Range I
greedy
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

### 1025. Divisor game
greedy approach
even number 2 win, odd number 3 lose
for even number, it only has even factors + 1, we can minus 1 to give an odd number to B
for odd number, it only has odd factors, B can only pass an even number to A.


### 868. Binary Gap
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

### 268. Missing Number
0 to n, one is missing.
xor 1 to n and identical one goes to 0

### 628. Maximum Product of Three Numbers
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

### 171. Excel Sheet Column Number
note we shall add 1

### 415. Add Strings
reverse and add

### 9. Palindrome Number
convert to string and reverse to see if equal

### 66. Plus One
MSB is at the front, so we add from the end

### 263. Ugly Number
only has 2,3,5 factor
recursive or iterative dividing 2,3,5 until is 1

### 67. Add Binary
reverse and add

### 172. Factorial Trailing Zeroes
n/2 2s n/5 5s so we need count number of 5s

### 168. Excel Sheet Column Title
pay attention to starting index 0

7. Reverse Integer
using string, note there is a sign

## Rating **

### 883. Projection Area of 3D Shapes
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

### 892. Surface Area of 3D Shapes
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

### 812. Largest Triangle Area
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

### 258. Add Digits
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

### 13. Roman to Integer
a hashmap of the letter to the number
if its right>left, then we subtract the left
else we add the left

### 12. Integer to Roman
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


### 453. Minimum Moves to Equal Array Elements
Given a non-empty integer array of size n, find the minimum number of moves required to make all array elements equal, where a move is incrementing n - 1 elements by 1.

sum + m * (n - 1) = x * n
x = minNum + m

### 598. Range Addition II
        //seems to find the most overlapped region
        //since it always covers the (0,0), we only need track the right bottom point
        //it is the min of all the x and min of all the y
        


### 836. Rectangle Overlap
        //x1-x2 interval and x3-x4 interval shall overlap
        //y1-y2 interval and y3-y4 interval shall overlap
rec1[0]<rec2[2] && rec2[0]<rec1[2] && rec1[1]<rec2[3] && rec2[1]<rec1[3];

### 202. Happy Number
A happy number is a number defined by the following process: 
Starting with any positive integer, replace the number by the sum of the squares of its digits, 
and repeat the process until the number equals 1 (where it will stay), 
or it loops endlessly in a cycle which does not include 1. 
Those numbers for which this process ends in 1 are happy numbers.
detect a cycle using hashset


### 231. Power of Two
n&(n-1)

### 326. Power of Three
3^k is factor of 3^max 


### 645. Set Mismatch
1 to n, one number is changed to another number. find the missing number
The idea is based on:
(1 ^ 2 ^ 3 ^ .. ^ n) ^ (1 ^ 2 ^ 3 ^ .. ^ n) = 0
Suppose we change 'a' to 'b', then all but 'a' and 'b' are XORed exactly 2 times. The result is then
0 ^ a ^ b ^ b ^ b = a ^ b
Let c = a ^ b, if we can find 'b' which appears 2 times in the original array, 'a' can be easily calculated by a = c ^ b.

### 367. Valid Perfect Square
1. n=k*k
2. binary search
3. a square number=1+3+5+...

### 441. Arranging Coins
n coins to arrange it as 1,2,3,4....
m(m+1)/2<=n

### 914. X of a Kind in a Deck of Cards
count card number for each number and find the gcd>1
gcd for a list of number is the gcd of the gcd with the numnber

### 507. Perfect Number
We define the Perfect Number is a positive integer that is equal to the sum of all its positive divisors except itself.
brutal force: up to sqrt(n)
be sure if i==num/i and it counts twice

### 633. Sum of Square Numbers
check if a number is sum of two squares
a^2+b^2=c, we just search one from 1 to sqrt(c/2) for a and once a is fixed we can find b

### 949. Largest Time for Given Digits
need to consider the time as a whole. just use all the permutation and get the largest valid time

### 754. Reach a Number
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

### 69. Sqrt(x)
binary search

### 400. Nth Digit
find the nth digit for 1,2,3,....
one digit: 1-9: 9
two digit: 10-99: 90
three digits: 100-999: 900
accumulate these numbers and using binary search to find the number with nth digit

### 204. Count Primes
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

### medium

### 535. Encode and Decode TinyURL
using 0-9 a-z A-Z to form a tinyURL
tinyURL includes 6 chars from above

using hashmap: 
tiny url vs real url
id vs tinyURL
everytime we increase the id when a new url comes

### 537. Complex Number Multiplication
read the real and imag and perform multiplication 
read using stringstream

### 885. Spiral Matrix III
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

### 877. Stone Game
even number of piles, total sum is odd. taking from either end.
who will win
can use dp
sum(piles(even)) < sum(piles[odd]), pick all odd
sum(piles(even)) > sum(piles[odd]), pick all even
always win

### 553. Optimal Division
perform float division. by adding parenthesis to reach max result
X1/X2/X3/../Xn will always be equal to (X1/X2) * Y, no matter how you place parentheses. i.e no matter how you place parentheses, X1 always goes to the numerator and X2 always goes to the denominator. Hence you just need to maximize Y. And Y is maximized when it is equal to X3 *..*Xn. So the answer is always X1/(X2/X3/../Xn) = (X1 *X3 *..*Xn)/X2

### 413. Arithmetic Slices
see dp

### 789. Escape The Ghosts
you and ghost can move 4 directions and move simultaneously. see if you can escape and reach the target position
Let's say you always take the shortest route to reach the target because if you go a longer or a more tortuous route, I believe the ghost has a better chance of getting you.
Denote your starting point A, ghost's starting point B, the target point C.
For a ghost to intercept you, there has to be some point D on AC such that AD = DB. Fix D. By triangle inequality, AC = AD + DC = DB + DC >= BC. What that means is if the ghost can intercept you in the middle, it can actually reach the target at least as early as you do. So wherever the ghost starts at (and wherever the interception point is), its best chance of getting you is going directly to the target and waiting there rather than intercepting you in the middle.

### 462. Minimum Moves to Equal Array Elements II
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

### 858. Mirror Reflection
reflection by expanding the rect in 2 directions and problem becomes a simple gcd problem

### 781. Rabbits in Forest
In a forest, each rabbit has some color. Some subset of rabbits (possibly all of them) tell you how many other rabbits have the same color as them. Those answers are placed in an array.

Return the minimum number of rabbits that could be in the forest.
For rabits with same colors, the answers must be the same. However, when the total amount of that same answer exceeds  'that answer + 1', there must be a new color. (say [3,3,3,3,3], the first four 3s indicates 4 rabits with the same color. The 5th 3 must represent a new color that contains 4 other rabits). We only calculate the amount of rabits with the same color once. Hashmap is used to record the frequency of the same answers. Once it exceeds the range, we clear the frequency and calculate again.

If x+1 rabbits have same color, then we get x+1 rabbits who all answer x.
now n rabbits answer x.
If n % (x + 1) == 0, we need n / (x + 1) groups of x + 1 rabbits.
If n % (x + 1) != 0, we need n / (x + 1) + 1 groups of x + 1 rabbits.

the number of groups is math.ceil(n / (x + 1)) and it equals to (n + x) / (x + 1) , which is more elegant.

### 672. Bulb Switcher II
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

### 869. Reordered Power of 2
store the digits in sorted order string
and then we iterate all 2^n to get same string
check if the two string equal

### 343. Integer Break
see dp

### 357. Count Numbers with Unique Digits
from 0 to 10^n get the number of unique digits
9*9*8*7*6...

### 592. Fraction Addition and Subtraction
need to find the lcm for all denom
final results need find the gcd

### 423. Reconstruct Original Digits from English
first count those unique letters in each number
after they decided, minus it from other

### 319. Bulb Switcher
n bulbs from 1 to n, from 2 to n toggle all its multiples. Number of lights left.
A bulb ends up on iff it is switched an odd number of times.

Call them bulb 1 to bulb n. Bulb i is switched in round d if and only if d divides i. So bulb i ends up on if and only if it has an odd number of divisors.

Divisors come in pairs, like i=12 has divisors 1 and 12, 2 and 6, and 3 and 4. Except when i is a square, like 36 has divisors 1 and 36, 2 and 18, 3 and 12, 4 and 9, and double divisor 6. So bulb i ends up on if and only if i is a square.

So just count the square numbers.

### 313. Super Ugly Number
just replace k pointers using dp

### 593. Valid Square
4 points if form a square
C(4,2) to calculate the side length, 4 same and 2 same

using hashset to count only two

### 279. Perfect Squares
see dp for knapsack

### 640. Solve the Equation
find the = and move all x items to left and non-x items to right
ax=b

### 670. Maximum Swap
a greedy choice
find the right max and swap with the first smaller.

### 963. Minimum Area Rectangle II
any four points
using the side as diagnonal and use the center point and the side length as the key to form a hashmap
these points are all on a circle.

### 775. Global and Local Inversions
if global==local
only local inversion exists. so the i-A[i] difference cannot be more than 1

### 223. Rectangle Area
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

### 372. Super Pow
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

### 264. Ugly Number II
see dp

### 396. Rotate Function
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

### 368. Largest Divisible Subset
subset numbers all satisfy Si%Sj=0
dp with backtrace

### 60. Permutation Sequence
given number 1 to n, find its kth permutation
for n elements, it has n! permutations
we have n on the first, n-1 on the second, so it is reduced to n*sub(n-1)
we can locate the position for n, n-1, n-2....for each digit

### 397. Integer Replacement
if n is even n/=2
if n is odd n++ or n--
repeat until n=1, return the min step

binary 01: we subtract 1->00
binary 11: we add 1->00
so we can reduce by half sooner

### 2. Add Two Numbers
linked list

### 43. Multiply Strings
naive approach using hand multiplication

### 794. Valid Tic-Tac-Toe State
Players take turns placing characters into empty squares (" ").
The first player always places "X" characters, while the second player always places "O" characters.
"X" and "O" characters are always placed into empty squares, never filled ones.
The game ends when there are 3 of the same (non-empty) character filling any row, column, or diagonal.
The game also ends if all squares are non-empty.
No more moves can be played if the game is over.

so
When turns is 1, X moved. When turns is 0, O moved. rows stores the number of X or O in each row. cols stores the number of X or O in each column. diag stores the number of X or O in diagonal. antidiag stores the number of X or O in antidiagonal. When any of the value gets to 3, it means X wins. When any of the value gets to -3, it means O wins.

When X wins, O cannot move anymore, so turns must be 1. When O wins, X cannot move anymore, so turns must be 0. Finally, when we return, turns must be either 0 or 1, and X and O cannot win at same time.

### 365. Water and Jug Problem.
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

### 50. Pow(x, n)
divide and conque, x^n=x^2*x^(n/2)

### 523. Continuous Subarray Sum
target sum multiple of k
prefix sum and put the %k the same value together

### 910. Smallest Range II
add +k or -k
sort it first and then greedy

### 166. Fraction to Recurring Decimal
need to find the repeat pattern

### 866. Prime Palindrome
find the smallest prime palindrome number >=N
All palindrome with even digits is multiple of 11.
So among them, 11 is the only one prime
if (8 <= N <= 11) return 11
For other, we consider only palindrome with odd dights.

reduce the number digits by half.

### 29. Divide Two Integers
no division
use subtraction. to speed up the subtraction we can double the divisor

### 8. String to Integer (atoi)
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

### 899. Orderly Queue
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

### 753. Cracking the Safe
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

### 810. Chalkboard XOR Game








