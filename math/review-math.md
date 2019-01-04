### part 1-easy
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

13. Roman to Integer
a hashmap of the letter to the number
if its right>left, then we subtract the left
else we add the left

171. Excel Sheet Column Number
note we shall add 1

453. Minimum Moves to Equal Array Elements
Given a non-empty integer array of size n, find the minimum number of moves required to make all array elements equal, where a move is incrementing n - 1 elements by 1.

sum + m * (n - 1) = x * n
x = minNum + m

598. Range Addition II
        //seems to find the most overlapped region
        //since it always covers the (0,0), we only need track the right bottom point
        //it is the min of all the x and min of all the y
        
268. Missing Number
0 to n, one is missing.
xor 1 to n and identical one goes to 0

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

202. Happy Number
A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.
detect a cycle using hashset

415. Add Strings
reverse and add

231. Power of Two
n&(n-1)

326. Power of Three
3^k is factor of 3^max 

9. Palindrome Number
convert to string and reverse to see if equal

66. Plus One
MSB is at the front, so we add from the end

263. Ugly Number
only has 2,3,5 factor
recursive or iterative dividing 2,3,5 until is 1

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

67. Add Binary
reverse and add

172. Factorial Trailing Zeroes
n/2 2s n/5 5s so we need count number of 5s

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

69. Sqrt(x)
binary search

400. Nth Digit
find the nth digit for 1,2,3,....
one digit: 1-9: 9
two digit: 10-99: 90
three digits: 100-999: 900
accumulate these numbers and using binary search to find the number with nth digit

168. Excel Sheet Column Title
pay attention to starting index 0

204. Count Primes
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
7. Reverse Integer
using string, note there is a sign

### medium






