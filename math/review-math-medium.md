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

12. Integer to Roman
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

869. Reordered Power of 2
store the digits in sorted order string
and then we iterate all 2^n to get same string
check if the two string equal

343. Integer Break
see dp

357. Count Numbers with Unique Digits
from 0 to 10^n get the number of unique digits
9*9*8*7*6...

592. Fraction Addition and Subtraction
need to find the lcm for all demom
final results need find the gcd

423. Reconstruct Original Digits from English
first count those unique letters in each number
after they decided, minus it from other

319. Bulb Switcher
n bulbs from 1 to n, from 2 to n toggle all its multiples. Number of lights left.
A bulb ends up on iff it is switched an odd number of times.

Call them bulb 1 to bulb n. Bulb i is switched in round d if and only if d divides i. So bulb i ends up on if and only if it has an odd number of divisors.

Divisors come in pairs, like i=12 has divisors 1 and 12, 2 and 6, and 3 and 4. Except when i is a square, like 36 has divisors 1 and 36, 2 and 18, 3 and 12, 4 and 9, and double divisor 6. So bulb i ends up on if and only if i is a square.

So just count the square numbers.

313. Super Ugly Number
just replace k pointers using dp

593. Valid Square
4 points if form a square
C(4,2) to calculate the side length, 4 same and 2 same

using hashset to count only two

279. Perfect Squares
see dp for knapsack

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

264. Ugly Number II
see dp

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

60. Permutation Sequence
given number 1 to n, find its kth permutation
for n elements, it has n! permutations
we have n on the first, n-1 on the second, so it is reduced to n*sub(n-1)
we can locate the position for n, n-1, n-2....for each digit

397. Integer Replacement
if n is even n/=2
if n is odd n++ or n--
repeat until n=1, return the min step

binary 01: we subtract 1->00
binary 11: we add 1->00
so we can reduce by half sooner

2. Add Two Numbers
linked list

43. Multiply Strings
naive approach using hand multiplication

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



