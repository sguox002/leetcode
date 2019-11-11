some subjects on algorithm

## base problem (number position system)
### balance base-3 problem
using -1,1,0 to represent a number
n=sum(Ai*3^i), then Ai would be 0, 1, 2.
we have to change 2 to -1 using 2=3-1 (3-1)*3^i=3^(i+1)-3^i. that is we need add a carry flag to the next when the digit is 2.

```cpp
  while(n){
    int d=n%3;
    n/=3;
    int cf=0;
    if(d==2)
      cf=1;
     n+=cf;
  }
```
cases: n=0 and n<0, for negative cases: we just reverse every digits.

### negative base problem
using similar approach:
n=sum(Ai*p^i), Ai is 0 to |p|-1
when p<0, n%p will get the result from 0 to p+1 (negatives) and we need convert the digit to positive:
Ai=-p+Ai+p
Ai*p^i=(Ai-p+p)*p^i=(Ai-p)*p^i+p^(i+1), that means when we get the remainder <0, we add a carrier to next level
```cpp
  while(n){
    int d=n%d;
    n/=d;
    int cf=0;
    if(d<0){
      cf=1;
    }
    n+=cf;
 }
 ```
 see LC1017. Convert to Base -2
 
 ### gray code
 number to gray code: i^(i>>1)
 see LC89. Gray Code
 gray code to number: 
 We will move from the most significant bits to the least significant ones (the least significant bit has index 1 and the most significant bit has index k). 
 g3  g2  g1  g0
 |  /|  /|  /|
 V/  V / V/  V
 b3  b2  b1  b0
 
 or equiv:
 b3=g3
 b2=g3^g2
 b1=g3^g2^g1
 b0=g3^g2^g1^g0
 
```cpp
int rev_g (int g) {
  int n = 0;
  for (; g; g >>= 1)
    n ^= g;
  return n;
}
```
- gray code can form a hamiltonian cycle in a hypercube with each bit as a dimension in space.
hamiltonian cycle  is a graph cycle (i.e., closed loop) through a graph that visits each node exactly once.
- gray code is used in DAC to minimize conversion errors
- gray code is also used in genetic algorithm theory
- can be used to solve tower of hanoi
tower of hanoi: three rods with a stack of disks in ascending order, move it one by one to another rod, following the ascending order all the time.
represent the status using binary: the MSB represents the largest disk and the LSB represents the smallest disk
each time we only change one bit, which is equiv to a move.

Practice:
It is necessary to arrange numbers from 0 to 2^(N+M)-1 in the matrix with 2^N rows and 2^M columns. 
Moreover, numbers occupying two adjacent cells must differ only in single bit in binary notation. 
Cells are adjacent if they have common side. 
Matrix is cyclic, i.e. for each row the leftmost and rightmost matrix cells are considered to be adjacent 
(the topmost and the bottommost matrix cells are also adjacent).

Input
The first line of input contains two integers N and M (0<N,M; N+M<=20).

Output
Output file must contain the required matrix in a form of 2^N lines of 2^M integers each.

Sample test(s)

Input
1 1

Output
0 2
1 3

the number shall have n+m bits (from 0 to 2^(n+m)-1) and each row is a cyclic gray code with m bits, each col is a cyclic gray code with n bits
but the starting is different. using different start value (xor the start value).
see LC1238. Circular Permutation in Binary Representation (1d case of this problem)


## algebra
### binary exponential
using divide and conquer and convert x^n to a O(logn) algorithm.
```cpp
long long binpow(long long a, long long b) {
    long long res = 1;
    while (b > 0) {
        if (b & 1)
            res = res * a;
        a = a * a;
        b >>= 1;
    }
    return res;
}
```

example 1: effective computation x^n%m
```cpp
long long binpow(long long a, long long b, long long m) {
    a %= m;
    long long res = 1;
    while (b > 0) {
        if (b & 1)
            res = res * a % m;
        a = a * a % m;
        b >>= 1;
    }
    return res;
}
```

example 2: effective computation of fib series
F(n)=F(n-1)+F(n-2)
change to a matrix format and it reduces to a M^n problem

example 3: geometric transformation, translation, scale, rotation et al
represent the transformation using matrix and it reduces to matrix multiplication and power
similar to opengl, we need to add 4th dimension to support translation.

example 4: two number product modulo problem (ab)%m->(2*a/2*b)%m

example 5: number of paths of length k in a graph.
represent the graph in a matrix G (undirected, unweighted), edge can be visited multiple times
G[i,j] represents number of paths between i and j.
G is the case for k=1
starting from k: C(k+1)=C(k)*G
C(k+1)=G^k
matrix multiplication is O(N^3)

example 6: shortest paths of a fixed length k
We are given a directed weighted graph G with n vertices and an integer k. For each pair of vertices (i,j) we have to find the length of the shortest path between i and j that consists of exactly k edges.

represent the graph using adjacency matrix, where G[i,j] represents the weight from i to j. if there is no edge, it shall be inf.
and G is the case for k=1.

again assume L(k) is the matrix for k, then we advance one step to get k+1:
L(k+1,i,j)=min(L(k,i,p)+G(p,j)) for all p=0 to n-1
if we define the min operation as @, L(k+1)=L(k)@G, then we get:
L(K)=G@G....@G (since the min operation is associative ie min(a,min(b,c))=min(min(a,b),c)
and then we can apply the binary exponential on this operator.

example 7: generalization: shortest paths up to length k.
for each vertex v, we duplicate a vertex v' and create edge [v,v'] and a self cycle in [v',v'] with weight 0. then we can convert the problem [u,v] with at most k to a problem from [u,v'] with a fixed length k+1.

practice questions:
Many well-known cryptographic operations require modular exponentiation. That is, given integers x, y and n, compute x^y mod n. In this question, you are tasked to program an efficient way to execute this calculation.
Input
The input consists of a line containing the number c of datasets, followed by c datasets, followed by a line containing the number ‘0’.
Each dataset consists of a single line containing three positive integers, x, y, and n, separated by blanks. You can assume that 1 < x, n < 2^15 = 32768, and 0 < y < 2^31 = 2147483648.
Output
The output consists of one line for each dataset. The i-th line contains a single positive integer z such that z = x^y mod n
for the numbers x, y, z given in the i-th input dataset.
Sample Input
2
2 3 5
2 2147483647 13
0
Sample Output
3
11

question 2:
get n^k the first 3 digits and last 3 digits
we can get all the digits using n^k%10

question 3:
parking lot with 2n-2 spaces, 4 types of cars, total number > parking spaces, and each type of cars > parking spaces.
parking the car with n ajacent cars the same make (exactly), return the total number of parking combinations
2n-2 spaces, 4 types of cars, exactly n the same.
analysis: n treated as one, then we have 2n-2-n=n-2 selections. When n cars fixed in position, we cannot choose same car in its adjacent position, left and right two cases: any combinations

question 4:
find the last digits of a^b
a^b reduce the last digit d^b



### gcd and euclid algorithm
### gcd
```cpp
int gcd (int a, int b) {
    if (b == 0)
        return a;
    else
        return gcd (b, a % b);
}

int gcd (int a, int b) {
    while (b) {
        a %= b;
        swap(a, b);
    }
    return a;
}

int lcm (int a, int b) {
    return a / gcd(a, b) * b;
}
```
O(log(min(a,b))
suppose d=gcd(a,b), then both a and b are divisible by d:
a%b=a-[a/b]*b
divide each side by d: then we see a%b is divisible by d also

gcd function is associative which is often used in some problems:
gcd(gcd(a,b),c)=gcd(a,gcd(b,c))
this can applies to >2 numbers, such as if a group of number exist coprime combinations

### extended euclid algorithm
ax+by=gcd(a,b)
this finds a way to represent gcd(a,b) using a and b (find its coefficient)
- it guarantee the existence of solution (water jug problem)
- get the solution (x,y)


