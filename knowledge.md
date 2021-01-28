## common small tricks:

### modular inverse
Fermat little thereom:
for prime p, a^p-a is a multiple of p. 
a*(a^(p-1)-1) is a multipe of p.
thus a^(p-1)=1 (mod m)
or a^(p-2)=a^(-1) (mod m)
using 1/a=a^(p-2) (binary power in log(p) time) to get the modular.

```cpp
    long modinv(int a,int n,int p){ //calculate a^-1%p -->a^(p-2)%p
        if(n==0) return 1;
        if(n%2) return a*modinv((long)a*a%p,n/2,p)%p;
        return modinv((long)a*a%p,n/2,p)%p;
    }
```	

### iterate all sub bitmask for a bitmask
start from the largest
```
for(int s=m;s;s=(s-1)&m){
}
```

### next bigger number with same number of set bits
```
    unsigned next_bigger(unsigned a) {
      /* works for any word length */
      unsigned c = (a & -a);
      unsigned r = a+c;
      return (((r ^ a) >> 2) / c) | r;
    }    
```

### gcd
```
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


### dijkstra and shortest path problem
dijkstra find the shortest path between two nodes, trying the shortest edge first. non-negative weight.
- one source to all other nodes, shortest path tree.
dijkstra's algorithm:

s1: Mark all nodes unvisited. Create a set of all the unvisited nodes called the unvisited set.
s2: Assign to every node a tentative distance value: set it to zero for our initial node and to infinity for all other nodes. Set the initial node as current.[16]
s3: For the current node, consider all of its unvisited neighbours and calculate their tentative distances through the current node. Compare the newly calculated tentative distance to the current assigned value and assign the smaller one. For example, if the current node A is marked with a distance of 6, and the edge connecting it with a neighbour B has length 2, then the distance to B through A will be 6 + 2 = 8. If B was previously marked with a distance greater than 8 then change it to 8. Otherwise, the current value will be kept.
s4: When we are done considering all of the unvisited neighbours of the current node, mark the current node as visited and remove it from the unvisited set. A visited node will never be checked again.
s5: If the destination node has been marked visited (when planning a route between two specific nodes) or if the smallest tentative distance among the nodes in the unvisited set is infinity (when planning a complete traversal; occurs when there is no connection between the initial node and remaining unvisited nodes), then stop. The algorithm has finished.
s6: Otherwise, select the unvisited node that is marked with the smallest tentative distance, set it as the new "current node", and go back to step 3.

see the Maze II.


### skip list:

The worst case search time for a sorted linked list is O(n) as we can only linearly traverse the list and cannot skip nodes while searching. For a Balanced Binary Search Tree, we skip almost half of the nodes after one comparison with root. For a sorted array, we have random access and we can apply Binary Search on arrays.

Can we augment sorted linked lists to make the search faster? The answer is Skip List. The idea is simple, we create multiple layers so that we can skip some nodes. See the following example list with 16 nodes and two layers. The upper layer works as an “express lane” which connects only main outer stations, and the lower layer works as a “normal lane” which connects every station. Suppose we want to search for 50, we start from first node of “express lane” and keep moving on “express lane” till we find a node whose next is greater than 50. Once we find such a node (30 is the node in following example) on “express lane”, we move to “normal lane” using pointer from this node, and linearly search for 50 on “normal lane”. In following example, we start from 30 on “normal lane” and with linear search, we find 50.



What is the time complexity with two layers? The worst case time complexity is number of nodes on “express lane” plus number of nodes in a segment (A segment is number of “normal lane” nodes between two “express lane” nodes) of “normal lane”. So if we have n nodes on “normal lane”, √n (square root of n) nodes on “express lane” and we equally divide the “normal lane”, then there will be √n nodes in every segment of “normal lane” . √n is actually optimal division with two layers. With this arrangement, the number of nodes traversed for a search will be O(√n). Therefore, with O(√n) extra space, we are able to reduce the time complexity to O(√n).

if we keep top layer is half of bottom layer, then we get log(n) complexity.

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

ax+by=gcd(a,b)=gcd(b,a%b)
bx+(a%b)y=g
bx+a-[a/b]*by=g

base case: a=0, x=1, gcd=b
```cpp
int gcd(int a, int b, int & x, int & y) {
    if (a == 0) {
        x = 0;
        y = 1;
        return b;
    }
    int x1, y1;
    int d = gcd(b % a, a, x1, y1);
    x = y1 - (b / a) * x1;
    y = x1;
    return d;
}
```
note:
- extended euclid algorithm also works for more than two numbers.

### Linear Diophantine Equation
ax+by=c
- a=b=c=0, infinite solutions
- a=b=0 c!=0, no solutions
- find any solution if c is divisible by gcd(a,b)
ax+by=c
ax'*c/g+by'*c/g=c
x=x'*c/g
y=y'*c/g

```cpp
int gcd(int a, int b, int &x, int &y) {
    if (a == 0) {
        x = 0; y = 1;
        return b;
    }
    int x1, y1;
    int d = gcd(b%a, a, x1, y1);
    x = y1 - (b / a) * x1;
    y = x1;
    return d;
}

bool find_any_solution(int a, int b, int c, int &x0, int &y0, int &g) {
    g = gcd(abs(a), abs(b), x0, y0);
    if (c % g) {
        return false;
    }

    x0 *= c / g;
    y0 *= c / g;
    if (a < 0) x0 = -x0;
    if (b < 0) y0 = -y0;
    return true;
}
```
- get all solutions
a(x+b/g)+b(y-a/g)=c
so x=x'+b/g and y=y'-a/g is also a solution we can just add a k integer factor

- find solutions in range [xmin,xmax] and [ymin,ymax]
the idea is to shift the solution so that x is in the range and get y range, and then shift y into [ymin, ymax] and get x range
and then get the two intersection.
```cpp
void shift_solution(int & x, int & y, int a, int b, int cnt) {
    x += cnt * b;
    y -= cnt * a;
}

int find_all_solutions(int a, int b, int c, int minx, int maxx, int miny, int maxy) {
    int x, y, g;
    if (!find_any_solution(a, b, c, x, y, g))
        return 0;
    a /= g;
    b /= g;

    int sign_a = a > 0 ? +1 : -1;
    int sign_b = b > 0 ? +1 : -1;

    shift_solution(x, y, a, b, (minx - x) / b);
    if (x < minx)
        shift_solution(x, y, a, b, sign_b);
    if (x > maxx)
        return 0;
    int lx1 = x;

    shift_solution(x, y, a, b, (maxx - x) / b);
    if (x > maxx)
        shift_solution(x, y, a, b, -sign_b);
    int rx1 = x;

    shift_solution(x, y, a, b, -(miny - y) / a);
    if (y < miny)
        shift_solution(x, y, a, b, -sign_a);
    if (y > maxy)
        return 0;
    int lx2 = x;

    shift_solution(x, y, a, b, -(maxy - y) / a);
    if (y > maxy)
        shift_solution(x, y, a, b, sign_a);
    int rx2 = x;

    if (lx2 > rx2)
        swap(lx2, rx2);
    int lx = max(lx1, lx2);
    int rx = min(rx1, rx2);

    if (lx > rx)
        return 0;
    return (rx - lx) / abs(b) + 1;
}
```
- Find the solution with minimum value of x+y
x=x'+b/g
y=y'-a/g
x+y=x'+y'+k*(b-a)/g
b>a choose smallest k
b<a: choose max k.

### fibocci series
- matrix form:
(Fn Fn+1)=(F0 F1)⋅P^n
p=[0,1;1,1]

- fast doubling method:
F(2k)=F(k)*(2F(k+1)-F(k))
F(2k+1)=F(k+1)^2+F(k)^2
```cpp
pair<int, int> fib (int n) {
    if (n == 0)
        return {0, 1};

    auto p = fib(n >> 1);
    int c = p.first * (2 * p.second - p.first);
    int d = p.first * p.first + p.second * p.second;
    if (n & 1)
        return {d, c + d};
    else
        return {c, d};
}
```
- periodicity modulo p

## prime numbers
Sieve of Eratosthenes 筛法
```cpp
int n;
vector<bool> is_prime(n+1, 1);
is_prime[0] = is_prime[1] = 0;
for (int i = 2; i <= n; i++) {
    if (is_prime[i] && (long long)i * i <= n) {
        for (int j = i * i; j <= n; j += i)
            is_prime[j] = 0;
    }
}
```
complexity analysis:
storage: O(n), time: O(nloglog(n))

optimization: 
- sifting to only the root
```cpp
int n;
vector<char> is_prime(n+1, true);
is_prime[0] = is_prime[1] = false;
for (int i = 2; i * i <= n; i++) {
    if (is_prime[i]) {
        for (int j = i * i; j <= n; j += i)
            is_prime[j] = false;
    }
}
```
- only process odd numbers
- block sieving
It follows from the optimization "sieving till root" that there is no need to keep the whole array is_prime[1...n] at all time. For performing of sieving it's enough to keep just prime numbers until root of n, i.e. prime[1... sqrt(n)], split the complete range into blocks, and sieve each block separately. In doing so, we never have to keep multiple blocks in memory at the same time, and the CPU handles caching a lot better.

Let s be a constant which determines the size of the block, then we have ⌈ns⌉ blocks altogether, and the block k (k=0...⌊ns⌋) contains the numbers in a segment [ks;ks+s−1]. We can work on blocks by turns, i.e. for every block k we will go through all the prime numbers (from 1 to n−−√) and perform sieving using them. It is worth noting, that we have to modify the strategy a little bit when handling the first numbers: first, all the prime numbers from [1;n−−√] shouldn't remove themselves; and second, the numbers 0 and 1 should be marked as non-prime numbers. While working on the last block it should not be forgotten that the last needed number n is not necessary located in the end of the block.

Here we have an implementation that counts the number of primes smaller than or equal to n using block sieving.

```cpp
int count_primes(int n) {
    const int S = 10000;

    vector<int> primes;
    int nsqrt = sqrt(n);
    vector<char> is_prime(nsqrt + 1, true);
    for (int i = 2; i <= nsqrt; i++) {
        if (is_prime[i]) {
            primes.push_back(i);
            for (int j = i * i; j <= nsqrt; j += i)
                is_prime[j] = false;
        }
    }

    int result = 0;
    vector<char> block(S);
    for (int k = 0; k * S <= n; k++) {
        fill(block.begin(), block.end(), true);
        int start = k * S;
        for (int p : primes) {
            int start_idx = (start + p - 1) / p;
            int j = max(start_idx, p) * p - start;
            for (; j < S; j += p)
                block[j] = false;
        }
        if (k == 0)
            block[0] = block[1] = false;
        for (int i = 0; i < S && start + i <= n; i++) {
            if (block[i])
                result++;
        }
    }
    return result;
}
```

this is good for followup to save memory requirement and improve cache performance.

### integer prime factorization:
```cpp
vector<long long> trial_division1(long long n) {
    vector<long long> factorization;
    for (long long d = 2; d * d <= n; d++) {
        while (n % d == 0) {
            factorization.push_back(d);
            n /= d;
        }
    }
    if (n > 1)
        factorization.push_back(n);
    return factorization;
}
```
number of divisior
in a map: prime vs count, number of divisor is product of each cnt+1

sum of divisor:
1+p+p^2+...+p^k for p =(p^(k+1)-1)/(p-1) and then multiply all these sum

## data structure
### min stack /min queue
- find the min in a stack i O(1)
save the element and current min as a pair in a single stack

- find the min (same problem) using queue
add to the back and remove from the front
using deque.
keep the deque in nondecreasing order, so the front is always the min.
not all the elements are stored
when removing an element, needs check if the front is the element

- queue modification using two stacks
all elements are stored
do not need to check the element

- Finding the minimum for all subarrays of fixed length M.
above three methods are all able to solve it.

### sparse table
sparse table is a data structure for range query of immutable array.
the idea is to precompute the range queries for 2^j intervals (similar to binary reprsentation of any number)
store the precomputed queries in a 2d matrix:
for length=n, there are at least log2(n) items.
st[i,j] represents the range for [i,i+2^j)
st[i,j] fits nicely [i,i+2^(j-1）） and [i+2^(j-1),i+2^j) (both length 2^(j-1))
recurrence relation:
st[i,j]=f(st[i,j-1]+st[i+2^(j-1),j-1])
f is the function of query.

- range sum query:
```cpp

long long st[MAXN][K];

for (int i = 0; i < N; i++)
    st[i][0] = array[i];

for (int j = 1; j <= K; j++)
    for (int i = 0; i + (1 << j) <= N; i++)
        st[i][j] = st[i][j-1] + st[i + (1 << (j - 1))][j - 1];
		
query:

long long sum = 0;
for (int j = K; j >= 0; j--) {
    if ((1 << j) <= R - L + 1) {
        sum += st[L][j];
        L += 1 << j;
    }
}		
```

- range min query
compute the min of the range [L，R].
min(st[L,j],st[R-2^j+1,j]) j=log2(R-L+1)
this splits into two section.
first range L+[0,2^j-1], this mostly does not cover R.
second range: R-2^j+1+[0,2^j-1]
this two ranges overlaps.

```cpp
//fast calculate log2
int log[MAXN+1];
log[1] = 0;
for (int i = 2; i <= MAXN; i++)
    log[i] = log[i/2] + 1;

//calculate spare table st.
int st[MAXN][K];

for (int i = 0; i < N; i++)
    st[i][0] = array[i];

for (int j = 1; j <= K; j++)
    for (int i = 0; i + (1 << j) <= N; i++)
        st[i][j] = min(st[i][j-1], st[i + (1 << (j - 1))][j - 1]);	
//query in O(1)
int j = log[R - L + 1];
int minimum = min(st[L][j], st[R - (1 << j) + 1][j]);
```
above approach only works for idempotent function (which can applies multiple times without changing the results)
for example the min function, but sum function cannot work.

### disjoint set
- naive implementation - lead to chain in worst cases
- path compression 
- union by rank or size. (rank: the depth, size: number of vertices)
will has constant access time.

- link by index (index is a random number)
- coin flip linking (using random number odd/even to decide the linking)

applications:
- Connected components in a graph
add vertices and edges into a graph
This application is quite important, because nearly the same problem appears in Kruskal's algorithm for finding a minimum spanning tree. 
Using DSU we can improve the O(mlogn+n^2) complexity to O(mlogn).
using DSU:
we sort the edges from small to long
using the edges to union two vertices. if both vertices are in the same set, discard it.
stop when we get N-1 edges

- Search for connected components in an image
One of the applications of DSU is the following task: there is an image of n×m pixels. Originally all are white, but then a few black pixels are drawn. You want to determine the size of each white connected component in the final image.

For the solution we simply iterate over all white pixels in the image, for each cell iterate over its four neighbors, and if the neighbor is white call union_sets. Thus we will have a DSU with nm nodes corresponding to image pixels. The resulting trees in the DSU are the desired connected components.

The problem can also be solved by DFS or BFS, but the method described here has an advantage: it can process the matrix row by row (i.e. to process a row we only need the previous and the current row, and only need a DSU built for the elements of one row) in O(min(n,m)) memory.

- Store additional information for each set
you can store any information in the set, such as size, depth,...

- Compress jumps along a segment / Painting subarrays offline
example: array of length L, and each query [l,r,c] will paint the cells in range [l,r] to the color c.
return the final color of the array.
paint in reverse order, we only need to take care of unpainted cells.
thus we can union the same color.
```cpp
for (int i = 0; i <= L; i++) {
    make_set(i);
}

for (int i = m-1; i >= 0; i--) {
    int l = query[i].l;
    int r = query[i].r;
    int c = query[i].c;
    for (int v = find_set(l); v <= r; v = find_set(v)) {
        answer[v] = c;
        parent[v] = v + 1;
    }
}
```

For the solution we can make a DSU, which for each cell stores a link to the next unpainted cell. Thus initially each cell points to itself. 
After painting one requested repaint of a segment, all cells from that segment will point to the cell after the segment.
when painted, point to its right. if it is painted, then it points to it next unpainted right.
for example:
last paint is [8,9,0], then 8->9->10
paint [5,11,1], then we get 5->6->7->10->11->12
good problem.

- Support distances up to representative
add distance to representative (the root of the set)
```cpp
void make_set(int v) {
    parent[v] = make_pair(v, 0);
    rank[v] = 0;
}

pair<int, int> find_set(int v) {
    if (v != parent[v].first) {
        int len = parent[v].second;
        parent[v] = find_set(parent[v].first);
        parent[v].second += len;
    }
    return parent[v];
}

void union_sets(int a, int b) {
    a = find_set(a).first;
    b = find_set(b).first;
    if (a != b) {
        if (rank[a] < rank[b])
            swap(a, b);
        parent[b] = make_pair(a, 1);
        if (rank[a] == rank[b])
            rank[a]++;
    }
}
```

- Support the parity of the path length / Checking bipartiteness online

### Fenwick tree
used for updating a range and query a range using a reversible function.
most often used function is sum
both operation complexity is O(logn)

For array A[0..N] we precompute the T[0..N]
for every T[i] it is the function on the range [g[i],i]

def sum(int r): #calculate the sum for A[0,r] using intervals seamlessly.
    res = 0
    while (r >= 0):
        res += t[r]
        r = g(r) - 1
    return res

def increase(int i, int delta): #increase A[i], we need change all T involving i.
    for all j with g(j) <= i <= j:
        t[j] += delta
	
note: for sum, we are reducing the r, for increasing, we increase the j
0-based index:
g[i]=i&(i+1)
h[j]=j|(j+1)

finding sum:
```cpp
struct FenwickTree {
    vector<int> bit;  // binary indexed tree
    int n;

    FenwickTree(int n) {
        this->n = n;
        bit.assign(n, 0);
    }

    FenwickTree(vector<int> a) : FenwickTree(a.size()) {
        for (size_t i = 0; i < a.size(); i++)
            add(i, a[i]);
    }

    int sum(int r) {
        int ret = 0;
        for (; r >= 0; r = (r & (r + 1)) - 1)
            ret += bit[r];
        return ret;
    }

    int sum(int l, int r) {
        return sum(r) - sum(l - 1);
    }

    void add(int idx, int delta) {
        for (; idx < n; idx = idx | (idx + 1))
            bit[idx] += delta;
    }
};
```

2d BIT:
```cpp
struct FenwickTree2D {
    vector<vector<int>> bit;
    int n, m;

    // init(...) { ... }

    int sum(int x, int y) {
        int ret = 0;
        for (int i = x; i >= 0; i = (i & (i + 1)) - 1)
            for (int j = y; j >= 0; j = (j & (j + 1)) - 1)
                ret += bit[i][j];
        return ret;
    }

    void add(int x, int y, int delta) {
        for (int i = x; i < n; i = i | (i + 1))
            for (int j = y; j < m; j = j | (j + 1))
                bit[i][j] += delta;
    }
};
```

1-based index:
g[i]=i-i&(-i)
h[i]=i+i&(-i)

```cpp
struct FenwickTreeOneBasedIndexing {
    vector<int> bit;  // binary indexed tree
    int n;

    FenwickTreeOneBasedIndexing(int n) {
        this->n = n + 1;
        bit.assign(n + 1, 0);
    }

    FenwickTreeOneBasedIndexing(vector<int> a)
        : FenwickTreeOneBasedIndexing(a.size()) {
        init(a.size());
        for (size_t i = 0; i < a.size(); i++)
            add(i, a[i]);
    }

    int sum(int idx) {
        int ret = 0;
        for (++idx; idx > 0; idx -= idx & -idx)
            ret += bit[idx];
        return ret;
    }

    int sum(int l, int r) {
        return sum(r) - sum(l - 1);
    }

    void add(int idx, int delta) {
        for (++idx; idx < n; idx += idx & -idx)
            bit[idx] += delta;
    }
};
```

common operation:
point update, range query
range update, point query
the idea is update all the bit +x after L, and update -x all the bit after R.
```cpp
void add(int idx, int val) {
    for (++idx; idx < n; idx += idx & -idx) //++idx is for 1-based
        bit[idx] += val;
}

void range_add(int l, int r, int val) {
    add(l, val);
    add(r + 1, -val);
}

int point_query(int idx) {
    int ret = 0;
    for (++idx; idx > 0; idx -= idx & -idx)
        ret += bit[idx];
    return ret;
}
```
range update, range query
```cpp
def add(b, idx, x):
    while idx <= N:
        b[idx] += x
        idx += idx & -idx

def range_add(l,r,x):
    add(B1, l, x)
    add(B1, r+1, -x)
    add(B2, l, x*(l-1))
    add(B2, r+1, -x*r)

def sum(b, idx):
    total = 0
    while idx > 0:
        total += b[idx]
        idx -= idx & -idx
    return total

def prefix_sum(idx):
    return sum(B1, idx)*idx -  sum(B2, idx)

def range_sum(l, r):
    return sum(r) - sum(l-1)
```    

### sgement tree
find the sum or min in a range in O(logn) using segment tree.
segment tree is a full binary tree with the root for the whole tree, left is the left array and right is the right array recursively. Using array to store the tree and the index of the child and parent relation is fixed.
- build the tree
build from bottom layer (post-order)
```cpp
void build(int a[], int v, int tl, int tr) {
    if (tl == tr) {
        t[v] = a[tl];
    } else {
        int tm = (tl + tr) / 2;
        build(a, v*2, tl, tm);
        build(a, v*2+1, tm+1, tr);
        t[v] = t[v*2] + t[v*2+1]; //postorder 
    }
}
```
the left side covers [tl,tm] and right side [tm+1,tr]
- query the sum
if the range [tl,tr] is the same as the node, then just return it
if not we need go to left recursively or/and right recursively

```cpp
int sum(int v, int tl, int tr, int l, int r) {
    if (l > r) 
        return 0;
    if (l == tl && r == tr) {
        return t[v];
    }
    int tm = (tl + tr) / 2;
    return sum(v*2, tl, tm, l, min(r, tm))
           + sum(v*2+1, tm+1, tr, max(l, tm+1), r);
}
```
- update a value
if a value is changed it only affects the ranges which includes the element.
also using post-order traversal
```cpp
void update(int v, int tl, int tr, int pos, int new_val) {
    if (tl == tr) {
        t[v] = new_val;
    } else {
        int tm = (tl + tr) / 2;
        if (pos <= tm)
            update(v*2, tl, tm, pos, new_val);
        else
            update(v*2+1, tm+1, tr, pos, new_val);
        t[v] = t[v*2] + t[v*2+1];
    }
}
```

- query the max or the min
we can use very similar approach, using max or min function
t[v]=max(t[2*v],t[2*v+1])

- query the max and its number of occurrence
store a pair of information: the max in the range and its occurrence
similarly using post-order.
```cpp
pair<int, int> t[4*MAXN];

pair<int, int> combine(pair<int, int> a, pair<int, int> b) {
    if (a.first > b.first) 
        return a;
    if (b.first > a.first)
        return b;
    return make_pair(a.first, a.second + b.second);
}

void build(int a[], int v, int tl, int tr) {
    if (tl == tr) {
        t[v] = make_pair(a[tl], 1);
    } else {
        int tm = (tl + tr) / 2;
        build(a, v*2, tl, tm);
        build(a, v*2+1, tm+1, tr);
        t[v] = combine(t[v*2], t[v*2+1]);
    }
}

pair<int, int> get_max(int v, int tl, int tr, int l, int r) {
    if (l > r)
        return make_pair(-INF, 0);
    if (l == tl && r == tr)
        return t[v];
    int tm = (tl + tr) / 2;
    return combine(get_max(v*2, tl, tm, l, min(r, tm)), 
                   get_max(v*2+1, tm+1, tr, max(l, tm+1), r));
}

void update(int v, int tl, int tr, int pos, int new_val) {
    if (tl == tr) {
        t[v] = make_pair(new_val, 1);
    } else {
        int tm = (tl + tr) / 2;
        if (pos <= tm)
            update(v*2, tl, tm, pos, new_val);
        else
            update(v*2+1, tm+1, tr, pos, new_val);
        t[v] = combine(t[v*2], t[v*2+1]);
    }
}
```

- compute the gcd or lcm in range
this is similar to max/min/sum, these functions are all associative.

other advance segment tree
- counting the number of zeros in range, and find the kth 0 index
segment tree stores the number of zeros in each range and find kth can use a binary tree traversal (preorder traversal or descending the segment tree):
```cpp
int find_kth(int v, int tl, int tr, int k) {
    if (k > t[v])
        return -1;
    if (tl == tr)
        return tl;
    int tm = (tl + tr) / 2;
    if (t[v*2] >= k)
        return find_kth(v*2, tl, tm, k);
    else 
        return find_kth(v*2+1, tm+1, tr, k - t[v*2]);
}
```
- searching the first prefix >= target
this can be obtained using lower_bound in O(logn)
or use binary tree search using segment tree.

- Searching for the first element greater than a given amount
give x and a range [l,r], find the first element
we build  a max segment tree and then search in the segement tree.

```cpp
int get_first(int v, int lv, int rv, int l, int r, int x) {
    if(lv > r || rv < l) return -1;
    if(l <= lv && rv <= r) {
        if(t[v] <= x) return -1;
        while(lv != rv) {
            int mid = lv + (rv-lv)/2;
            if(t[2*v] > x) {
                v = 2*v;
                rv = mid;
            }else {
                v = 2*v+1;
                lv = mid+1;
            }
        }
        return lv;
    }

    int mid = lv + (rv-lv)/2;
    int rs = get_first(2*v, lv, mid, l, r, x);
    if(rs != -1) return rs;
    return get_first(2*v+1, mid+1, rv, l ,r, x);
}
```

confused about binary index tree vs segment tree?
both can be used for dynamic cases (query and updates). BIT actually does not have a tree structure so cannot use tree traversal, but its navigation is determined by the simple relation. segment tree although uses the array but its inside logic is binary tree structure, so we can use those tree traversal and is more understandable (can use recursive).

in a lot of cases, they are exchangeable. segment tree is more versatile.

- find max subsegment sum:
given a range [l,r], find the subsegment max sum.
using segment tree, we store 4 information:
max prefix sum
max suffix sum
sum of segment
max subsegment sum in the segment

then we can determine parent's information from the left right child

```cpp
struct data {
    int sum, pref, suff, ans;
};

data combine(data l, data r) {
    data res;
    res.sum = l.sum + r.sum;
    res.pref = max(l.pref, l.sum + r.pref);
    res.suff = max(r.suff, r.sum + l.suff);
    res.ans = max(max(l.ans, r.ans), l.suff + r.pref);
    return res;
}

data make_data(int val) {
    data res;
    res.sum = val;
    res.pref = res.suff = res.ans = max(0, val);
    return res;
}

void build(int a[], int v, int tl, int tr) {
    if (tl == tr) {
        t[v] = make_data(a[tl]);
    } else {
        int tm = (tl + tr) / 2;
        build(a, v*2, tl, tm);
        build(a, v*2+1, tm+1, tr);
        t[v] = combine(t[v*2], t[v*2+1]);
    }
}

void update(int v, int tl, int tr, int pos, int new_val) {
    if (tl == tr) {
        t[v] = make_data(new_val);
    } else {
        int tm = (tl + tr) / 2;
        if (pos <= tm)
            update(v*2, tl, tm, pos, new_val);
        else
            update(v*2+1, tm+1, tr, pos, new_val);
        t[v] = combine(t[v*2], t[v*2+1]);
    }
}
data query(int v, int tl, int tr, int l, int r) {
    if (l > r) 
        return make_data(0);
    if (l == tl && r == tr) 
        return t[v];
    int tm = (tl + tr) / 2;
    return combine(query(v*2, tl, tm, l, min(r, tm)), 
                   query(v*2+1, tm+1, tr, max(l, tm+1), r));
}
```

- saving the entire array of a range
we can sort and store in some data structure, such as set, map et al.

- Find the smallest number greater or equal to a specified number. No modification queries.
We want to answer queries of the following form: for three given numbers (l,r,x) we have to find the minimal number in the segment a[l…r] which is greater than or equal to x.

store the whole array in sorted order:
```cpp
vector<int> t[4*MAXN];

void build(int a[], int v, int tl, int tr) {
    if (tl == tr) {
        t[v] = vector<int>(1, a[tl]);
    } else { 
        int tm = (tl + tr) / 2;
        build(a, v*2, tl, tm);
        build(a, v*2+1, tm+1, tr);
        merge(t[v*2].begin(), t[v*2].end(), t[v*2+1].begin(), t[v*2+1].end(),
              back_inserter(t[v]));
    }
}

int query(int v, int tl, int tr, int l, int r, int x) {
    if (l > r)
        return INF;
    if (l == tl && r == tr) {
        vector<int>::iterator pos = lower_bound(t[v].begin(), t[v].end(), x);
        if (pos != t[v].end())
            return *pos;
        return INF;
    }
    int tm = (tl + tr) / 2;
    return min(query(v*2, tl, tm, l, min(r, tm), x), 
               query(v*2+1, tm+1, tr, max(l, tm+1), r, x));
}
```

- Find the smallest number greater or equal to a specified number. With modification queries.
store the array in multiset and allows quickly update the element
leetcode practice questions on BIT and segment tree.

### treap (tree heap)
store a pair (x,y) for x it is a binary search tree, for y it is a max heap.
x can be considered as key, and y can be considered as priority.
insert O(logn)
search O(logn)
erase O(logn)
build O(n) for sorted, O(nlogn) for unsorted
union(T1,T2) O(Mlog(N/M)) removing duplicates
intersect(T1,T2) O(Mlog(N/M))

