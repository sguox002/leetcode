hackerrank.com

# c++:
## trivial practice:
- say hello word
- input and output
- basic data type
- conditional statements
- for loop
- function
- pointers
- array introduction
- variabled sized arrays, 2d with each row variable sized
- stringstream, cin>> will not eat the , so we need use cin.get to eat it.
- string.
- struct
- class
- classes and objects.
- box it.- box compare using operator<.
- vector erase
- lower bound
- setstate
- maps
- inheritance introduction
- hotel price: shall use virtual for the base.
- cpp exception handling- try catch
- rectangle area
- multilevel inheritance - simple
- overloading ostream iterator
- message ordering- add a static timestamp and a compare operator <.
- access inherited function-- using scope name, A::function
- c++ class template.
- preprocessor solution
```cpp
#define INF (unsigned)!((int)0)
#define FUNCTION(name,operator) inline void name(int &current, int candidate) {!(current operator candidate) ? current = candidate : false;}
#define io(v) cin>>v
#define toStr(str) #str
#define foreach(v, i) for (int i = 0; i < v.size(); ++i)
```

- matrix operator overloading
- overload operators

### print pretty
cout support:
left, right: left/right align
showpos, noshowpos: show +
fixed/scientific: floating point output format.
setw: set width
lowercase/uppercase
scientific
setprecision
showbase: show the base
hex, dec, show hex or dec

### attribute parse
with <tag...></tag> format
using stack or array as stack.
- line by line read using stringstream.
- parse each line and form stack.
- form hashmap.

### inherited code
write a exception class.
```cpp
class BadLengthException {
    int n;
public:
    BadLengthException(int errornumber) {
        n = errornumber;
    }

    int what() {return n;}
};
```

### exception server
```cpp
        try{
            cout << Server::compute(A,B) << endl;
        } 
        catch (bad_alloc& error) { //bad allocation
            cout << "Not enough memory" << endl;
        }
        catch (exception& error) { //the exception
            cout << "Exception: " << error.what() << endl;
        }
        catch (...) { //unknown exception
            cout << "Other Exception" << endl;
        }
```		

### virtual functions
- note the static and member variable.
- each object has its own id, same class object's id are increasing.
- inside class, static member can be used as member variables.

### abstract classes-polymophism
- it implements a LRU using doubly linked list. However without stl list, it is harder.
- using hashmap to record each node's iterator.

### deque
implement a sliding window max finder.

deque to store the index, the max at the front of the deque.

### magic spell
- use dynamic_cast to check what the pointer is.
- find two array's longest common subsequence using dp.

```cpp
    Fireball* fireSpell=dynamic_cast<Fireball*>(spell);
    Frostbite* frostSpell=dynamic_cast<Frostbite*>(spell);
    Thunderstorm* thunderSpell=dynamic_cast<Thunderstorm*>(spell);
    Waterbolt* waterSpell=dynamic_cast<Waterbolt*>(spell);
    if(fireSpell) fireSpell->revealFirepower();
    else if(frostSpell) frostSpell->revealFrostpower();
    else if(thunderSpell) thunderSpell->revealThunderpower();
    else if(waterSpell) waterSpell->revealWaterpower();
    else {
        //lcs of two string
        string s=spell->revealScrollName(),t=SpellJournal::journal;
        int m=s.size(),n=t.size();
        //int lcs=0;
        vector<vector<int>> dp(m+1,vector<int>(n+1));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(s[i]==t[j]) dp[i+1][j+1]=dp[i][j]+1;
                else dp[i+1][j+1]=max(dp[i+1][j],dp[i][j+1]);
            }
        }
        cout<<dp[m][n]<<endl;
        }
```

### attending workshops
- construct the workshop class and operator<
- find the longest non-overlapped chain using greedy.
- dp will TLE

```cpp
struct Workshop{
    int start,end,dur;
    Workshop(){}
    Workshop(int s,int d):start(s),dur(d){end=start+d;}
    bool operator<(Workshop& w){
        return start<w.start || (start==w.start && end<w.end);
    }
};
struct Available_Workshops {
    int n;
    vector<Workshop> wlist;
    Available_Workshops(){}
};
Available_Workshops* initialize(int* s,int* d,int n){
    Available_Workshops* wlist=new Available_Workshops;
    for(int i=0;i<n;i++){
        wlist->wlist.push_back(Workshop(s[i],d[i]));
    }
    return wlist;
}
int CalculateMaxWorkshops(Available_Workshops* wlist){
    sort(wlist->wlist.begin(),wlist->wlist.end());
    int n=wlist->wlist.size();
        int count  = 0;
        int cur_end = INT_MIN;
        for(int i=0;i<n;i++)
        {
            if(wlist->wlist[i].start >= cur_end)
            {
                count ++;
                cur_end = wlist->wlist[i].end;
            }
               
            else if(wlist->wlist[i].end < cur_end)
                cur_end = wlist->wlist[i].end;
        }
        return count;
    //return ans;
}
```
### template specialization
```cpp
enum class Fruit { apple, orange, pear };
enum class Color { red, green, orange };

template <typename T> struct Traits;

// Define specializations for the Traits class template here.
template <> 
struct Traits<Fruit>{
    static string name(int index){
        switch(index){
                case 0:return "apple";
                case 1: return "orange" ;
                case 2: return "pear";
        }  
        return "unknown";
    } 
};
template <> 
struct Traits<Color>{
    static string name(int index){
        switch(index){
                case 0:return "red";
                case 1: return "green" ;
                case 2: return "orange";           
        }
        return "unknown";  
    } 
};
```

### variadics
reverse variable length of binary digits
```cpp
// Enter your code for reversed_binary_value<bool...>()
template <bool a> int reversed_binary_value() { return a; }

template <bool a, bool b, bool... d> int reversed_binary_value() {
  return (reversed_binary_value<b, d...>() << 1) + a;
}

```

### bit array
- find cycle and count number of unique elements

- using rabbit-tortoise (similar to find cycle entrance and length of cycle and entrance length.

- use unordered set will TLE.

```cpp
int main() {
 
    // set up variables and constants and read input
 
    unsigned int N, S, P, Q, mu, nu;
 
    const unsigned int m = 1 << 31;
 
    cin >> N >> S >> P >> Q;
 
    // set up sequence
 
    unsigned int* a = new unsigned int[N];
 
    a[0] = S % m;
 
    for (int i = 1; i < N; i++) {
        a[i] = (a[i-1] * P + Q) % m;
    }
 
    // begin cycle detection (-> floyd's algorithm)
 
    for (int i = 0; i < N; i++) {
 
        if ((2*i)+1 > N-1) {
 
            // no cycle found -> N distinct values, output N, clean up and terminate execution
 
            cout << N;
            delete[] a;
            return 0;
 
        } else if(a[i] == a[(2*i)+1]) {
 
            // cycle detected, break loop to proceed with algorithm
 
            nu = i+1;
            break;
        }
    }
 
    // find first element of cycle
 
    for (int i = 0; i < N; i++) {
 
        if(a[i] == a[i+nu]) {
            mu = i;
            break;
        }
    }
 
    // find first reoccurence of first cycle element
 
    for (int i = mu+1; i < N; i++) {
        if(a[mu] == a[i]) {
 
            // number of elements up until first repeated cycle element = number of distinct values in sequence
 
            cout << i;
            break;
        }
    }
 
    // clean up, terminate execution
 
    delete[] a;
    return 0;
}
```
		

# mathematics

## trivial
- find the point (point symmetric)
- max draws - for get same color pair socks (n+1)
- handshake - n*(n-1)/2
- min height triangle - integer height, ceil function.
- army game- each point can cover at most 4 cells, we expand m and n to 2 multiples
(n+n%2)*(m+m%2)/4
- connecting towns - modular arithmetics



### leonardo's prime factors
max number of unique prime factors <=n.
```cpp
int primeCount(long n) {
    /*
     * Write your code here.
     */
    unsigned int count;
    unsigned long long int prod;
    unsigned long long int prim;

    if (n < 2) return 0;

    prod = 2;
    count = 1;
    for (prim = 3; prod * prim <= n; prim += 2) {
        if (__gcd(prod, prim) == 1) {
            prod *= prim;
            count++;
        }
    }
    return count;
}    
```

### cutting paper square.
for a mxn paper, we can first cut it to a single row or column, which needs m-1 cuts
then for m rows, we need n-1 cuts for each of them: 
total m-1+m*(n-1)=mn-1

### summing the N series
if we add T1+...+Tn directly we get: n*n.

### best divisor
It needs to calculate n's all divisors and add the digit sum. and find the largest one

if tie, choose the smaller  factor.
```cpp
    int ans=1,maxsum=1;
    if(ans<digitsum(n)){
        maxsum=digitsum(n);
        ans=n;
    }
    for(int i=2;i<=sqrt(n);i++){
        if(n%i==0){
            int sum=digitsum(i);
            if(maxsum<sum){
                maxsum=sum;
                ans=i;
            }
            sum=digitsum(n/i); //n/i is larger we need use <= here.
            if(maxsum<=sum){
                maxsum=sum;
                ans=n/i;
            }
            //cout<<i<<" "<<n/i<<" "<<maxsum<<endl;
        }
    }
```	

### restaurant
cut a mxn bread into max possible square
m/gcd*n/gcd

### reverse game.
if we do the reverse we find the result would be
n-1,0,n-2,1,n-3,2.....
and we need to derive the relation
```cpp
        if(2*k<n-1) cout<<2*k+1<<endl;
        else cout<<2*(n-1-k)<<endl;
```
### sherlock and moving tiles
the code has some problem since it passes the vector of long using it.
the math: 
```cpp
    //assume s1<s2, at time t, s1 right at s1*sqrt(2)*t+L, s2 left at s2*sqrt(2)*t
    //dx=L+(s1-s2)/sqrt(2)*t=sqrt(q) 
    vector<double> ans;
    for(int q: queries) {
        double t=(l-sqrt(q*1.0))*sqrt(2.0)/abs(s1-s2);
        if(t<0) t=0;
        ans.push_back(t);
    }
```

### strange grid again
each row 5 numbers:
..............
..............
20 22 24 26 28
11 13 15 17 19
10 12 14 16 18
 1  3  5  7  9
 0  2  4  6  8
find the number at [r,c], bottom row is the first row. 
two rows repeat and add 0,10,20,30,40....to each of them.
```cpp
int strangeGrid(int r, int c) {
    /*
     * Write your code here.
     */
     r--,c--;
     int nr=r/2,oddeven=r%2;
     return (long)nr*10+2*c+oddeven;
}
```
fail with 1000000011 5, correct answer is 5000000058
need fix the function signature and the calling statement to long.

### diwali lights
N lights in a string, at least one light is on, return the number of patterns
apparently 2^n-1
%1e5

### strange numbers
strange number: it can be divided by its number of digit, and recursively.
find the number of strange numbers in a range.
for a number with n digit, it shall be divisble by n!
l and r can be equivalent to find from 1 to l or r, number of strange numbers.

approach: generate all the numbers and then find.

### matrix trace
you cannot use dp for this since m,n is too large.
m, n is up to 1e6 and mxn would be 1e12 and that's 1000G memory.
It is a combination problem with D and R, m-1 D and n-1 R. and this is a combination.
C(m+n-2,m-1)
However taking the mod is a problem.
m<-m-1, n<-n-1
C(m+n,m)=(m+1)/1*(m+2)/2*(m+3)/3....*(m+n)/m.
There is a dependence that the calculation is done in series to guarantee its divisibility
if mod is done for each of them, the guarantee is broken.
but if we keeps the multiply continue, it will overflow.

1
1315 1172
answer shall be 311823447

- approach 1: prime factorization of each number and convert division to subtraction
- approach 2: find all primes in range. and then calculate the number is easier.
- approach 3: euclid extended algorithm

A bit harder than expected. Not because of the code, but because of the math.

In total, you need to take (n-1)+(m-1) steps in directions (down)+(right) how many ways are there to choose (n-1) places to move down out of (n+m-2) moves?

C(n+m-2,n-1)=C(n+m-2,m-1)
The formula for combinations involves factorials and grows two quickly for that reason, and the problem statement, you want to use modular arithmetic

S mod p=(10**9 + 7)
in modular arithmetic, multiplication behaves the same as the residues mod n, that is:

(a*b)%n=(a%n*b%n)%n
The formula also requires division (multiplicative inverse in modular arithmetic)

(a*(a^-1)) = 1 mod p      which is math for       (a*(a^-1))%p = 1     
a^-1 in modular arithmetic can be computed using Extended Euclidean Algorithm however in your case n=1000000007 is a prime number therefore we can use

The extended Euclidean algorithm is the essential tool for computing multiplicative inverses in modular structures, typically the modular integers and the algebraic field extensions. A notable instance of the latter case are the finite fields of non-prime order.

(a^(-1))=a^(p-2)%p
finally, a^(n-2) can be computed more efficiently using your preferred method . I stole fastEXP from this guy
```python
p = 10**9+7

def factorial(n):
    result = 1                      # starting from 1 
    while n >= 2:                   # multiply n, n-1, n-2, ..., 2 
        result = ((result)*(n%p))%p # and don't forget to mod by p
        n -= 1
    return result

def solve(x,y):    # combinations formula aCb=a!/b!(a-b)!  mod p
    num = factorial(x+y-2)
    denom = factorial(x-1)*factorial(y-1)
    return ( (num%p) * ( fastEXP(denom,p-2,p) ) ) %p # p is prime so we can do this
    
def fastEXP(base, exp, modulus):
    base %= modulus
    result = 1
    while exp > 0:
        if exp & 1:
            result = (result * base) % modulus
        base = (base*base)%modulus
        exp >>= 1
    return result
```
	
## Problem Solving

### trivial
- simple array sum, accumulate
- compare the triplets, elementwise compare
- a very big sum: using long for intermediate results
- diagnonal difference: diag1 i==j, diag2 i+j==n-1
- plus minus: count >0 ==0 <0 percentage.
- staircase: start with n spaces and fill with #
- minmax sum: sum without min/max
- birthday cake candles: one loop find the max
- time conversion: from AM/PM format to 24 hour format, note process 12
- grading student: grade shall only go up.
- apple and oranges: count apples and oranges on the house
- kangaroo: solve for non-negative integer solution
- breaking the record, keep the min and max.
- birthday chocolate: sliding window sum
- divisible sum pairs: unordered_map to keep the mod, pay attention to 0
- migratory birds: using hashmap to count, and pq to sort
- sock merchant: match the same color, using hashmap to count.
- counting valleys: valleys shall has a down to below 0 and up 0. using prefix sum
- repeated string: division and modulation.
- Bon Appétit: 
- cats and mouse: meaningless
- hurdle race: initial max height, and each does add 1.
- pdf viewer: maxh*length
- angry professor: count person arriving before time.
- equalize the array: simple.

### between two sets
numbers in array 1 shall be factors of the number
numbers in array 2 shall be multiples of the number
- get the lcm of the array 1, note lcm for an array is lcm=lcm(lcm,i)
- get the gcd of the array 2, note gcd=gcd(gcd,i)
- and then all multiples which gcd%i==0

### forming a magic square
note we have 8 instead of 4 permutations.
start from one: rotate 90 degrees
transpose the matrix and do the rotation again.

### picking numbers
find the longest numbers with diff<=1. count and store in different bins

### jump on the clouds
avoid the thunderheads which is 1, other safe clouds are 0. min number of jump to the final clouds
typical bfs. each step is 1 or 2.

### bigger is greater
next greater word given a word.
```cpp
string biggerIsGreater(string w) {
    if(next_permutation(w.begin(),w.end()))
        return w;
    return "no answer";
}
```

### new year chaos
each person can only switch with its front at most 2.
return the min number of such switches.
idea: number > its index, a switch happens.
```cpp
void minimumBribes(vector<int> q) {
    int ans = 0;
    for (int i = q.size() - 1; i >= 0; i--) {
        if (q[i] - (i + 1) > 2) {
            cout << "Too chaotic" << endl;
            return;
        }
        for (int j = max(0, q[i] - 2); j < i; j++)
            if (q[j] > q[i]) ans++;
    }
    cout << ans << endl;
}
```

### day of the programmer
it is a bit tricky.
- the regular way to determine leap year
- the russian way to determine leap year <1918
- special for 1918 feb
- stringstream to output formated string. Do not know why the format has to be specified again
```cpp
string dayOfProgrammer(int year) {
    //256th day
    bool leap=(year%4==0 && (year%100!=0 || year%400==0));
    int days[]={31,28,31,30,31,30,31,31,30,31,30,31};
    int sum=0;
    int mon,day;
    if(year==1918) days[1]=15;
    if(year<1918) leap=year%4==0;
    for(int i=0;i<12;i++){
        int t=(i==1 && leap)+days[i];
        if(sum+t<=256){
            sum+=t;
        }
        else {mon=i+1,day=256-sum;break;}
    }
    //cout<<mon<<" "<<day<<endl;
    //string s;
    stringstream ss;//(s);
    ss<<setfill('0')<<setw(2)<<day<<"."; //does not work if connected with the next one.
    ss<<setfill('0')<<setw(2)<<mon<<"."<<year;
    return ss.str();
}
```

### drawing book
two page book: page 1 is always on the right, last page is awlays on the front page.
that means from the beginning or from the end is the same.
from left: 1, turn one and see 2,3, page k: k/2
from right, n: turn one and see n-1 and n-2, page k: (n-k+1)/2
misunderstand: the last page is printed at the end, with blank page inserted if necessary
for example 6 pages, it shall be like this:
B1,23,45,B6
return min(p/2,n/2-p/2);

### electronic shop
buy two items with sum<=n the largest
m,n<1000, so O(N^2) combination could be acceptable.

### climbing the leaderboard
- need eliminate duplicates
- need find each number's position
- array could be 1e5 and O(N^2) could be TLE
```cpp
vector<int> climbingLeaderboard(vector<int> scores, vector<int> alice) {
    set<int> sc(scores.begin(),scores.end());
    vector<int> ans;
    for(int i: alice){
        auto it=sc.lower_bound(i);
        int ind=distance(it,sc.end());
        if(it==sc.end() || *it>i)  ind++;
        ans.push_back(ind);
    }
    return ans;
}
```
distance for set is O(N), use array.
```cpp
vector<int> climbingLeaderboard(vector<int> scores, vector<int> alice) {
    sort(scores.begin(),scores.end());
    auto it=unique(scores.begin(),scores.end());
    scores.resize(distance(scores.begin(),it));
    vector<int> ans;
    for(int i: alice){
        auto it=lower_bound(scores.begin(),scores.end(),i);
        int ind=distance(it,scores.end());
        if(it==scores.end() || *it>i)  ind++;
        ans.push_back(ind);
    }
    return ans;
}
```

### utopian tree
each year two cycles: spring double and summer +1
n<60 we can use brutal force, trivial

### non-divisible subsets
given an array of numbers, create the longest array where any two number's sum is not divibible by k.
- n up to 1e5, so cannot use brutal force
- if we mod each number, we are not allowing a pair which adds to k or 0.
- intuition is using dp: 
- actually it is not a dp, it is a biparty problem
```cpp
int nonDivisibleSubset(int k, vector<int> s) {
    //biparition 
    vector<int> dp(k);
    for(int i: s) dp[i%k]++;
    int ans=0;
    int i=1,j=k-1;
    while(i<=j){
        if(i!=j) ans+=max(dp[i],dp[k-i]);
        else ans++; //can only keep one
        i++,j--;
    }
    if(dp[0]) ans++;
    return ans;
}
```
- two special case: when i==j=k/2 and i=0

### queen attack
cannot use brutal force since nxn with n up to 1e5.
get the nearest obstacle in the 8 directions and calculate the distance.
if there is no obstacle along the direction, we can add out of the boundary.

but hard to get it right.

### the grid search
search a matrix pattern in a large matrix
intuition: brutal force sliding window search.
```cpp
string gridSearch(vector<string> G, vector<string> P) {
    for(int i=0;i<G.size()-P.size();i++){
        for(int j=0;j<G[0].size()-P[0].size();j++){
            bool term=0;
            for(int k=0;k<P.size() && !term;k++){
                for(int l=0;l<P[0].size();l++){
                    if(G[i+k][j+l]!=P[k][l]) {term=1;break;}
                }
            }
            if(!term) return "YES";
        }
    }
    return "NO";
}
```
however, this code fails 5 test cases.
it is very easy to make mistake not check the last element
we shall use <= for i loop and j loop instead of <.

### absolute permutation
permutation of 1..n, with |A[i]-i|=k for all i, 1 based index.
return the lexi smallest permutation.
so if the position-value difference is k.
for the first: |a[1]-1|=k, a[1]>=1, so a[1]=k+1
a[i]>i: a[i]=k+i. (i<=k), make sure i+k will not >n.
a[i]<i: a[i]=i-k. (i>k)
need to compare k+i,i-k which one is smaller.
i-k may be coincident with previous used.
```cpp
vector<int> absolutePermutation(int n, int k) {
    //i<=k i+k, i>k: i-k
    vector<int> ans;
    vector<bool> used(n+1);
    for(int i=1;i<=n;i++){
        if(i<=k) {
            if(i+k>n) return {-1};
            ans.push_back(i+k);
            used[i+k]=1;
        }
        else {
            if(!used[i-k]){
                ans.push_back(i-k);
                used[i-k]=1;
            }
            else{
                if(i+k>n) return {-1};
                ans.push_back(i+k);
                used[i+k]=1;
            }
        }
    }
    return ans;
}
```

### the bomberman game
problem:
0s: initial setup the bomber positions
1s: does nothing
2s: fill all other places with bombs
3s: 3 seconds ago will detonate
idea:
initialize the matrix with 3.
the fill other cells with 4 (or 5?)
the using the time and clear its surrounding cells.
seconds to simulate would be very large
once we find no state changes, we stop.
pay attention to simultaneously bomb, so if one bomb make its neighboring bomb disappear, it is illegal
we shall let them all bomb!!!

misunderstand: it will pant bombs after it bombs

### almost sorted
using only one operation of the two:
- swap two elements
- reverse one subsegment.

n would be very large 1e5.
intuition: sort the array and compare the first and last different.
count the difference and check if reverse is sorted.

### matrix layer rotation
rotate a matrix layer by layer anti-clock for r steps.
intuition: if we rebuild the matrix into several layers, then it is left rotation
the layer number is the min(i,j).
traverse shall follow the requirement.
use l,r,t,b for the traverse (similar to spiral matrix in leetcode)
```cpp
void matrixRotation(vector<vector<int>> matrix, int s) {
    vector<vector<int>> mat;
    int m=matrix.size(),n=matrix[0].size();
    int l=0,r=n-1,t=0,b=m-1;
    int cnt=m*n,layer=0;
    while(cnt){
        mat.push_back({});
        for(int i=l;i<=r;i++) mat[layer].push_back(matrix[t][i]),cnt--;
        if(++t>b) break;
        for(int i=t;i<=b;i++) mat[layer].push_back(matrix[i][r]),cnt--;
        if(--r<l) break;
        for(int i=r;i>=l;i--) mat[layer].push_back(matrix[b][i]),cnt--;
        if(--b<t) break;
        for(int i=b;i>=t;i--) mat[layer].push_back(matrix[i][l]),cnt--;
        if(++l>r) break;
        layer++;
    }

    for(int l=0;l<mat.size();l++){
        auto& v=mat[l];
        rotate(v.begin(),v.begin()+s%v.size(),v.end());
    }
    //restore back to original matrix
    layer=0;
    l=0,r=n-1,t=0,b=m-1;
    for(int lay=0;lay<mat.size();lay++){
        cnt=0;
        auto v=mat[lay];
        for(int i=l;i<=r;i++) matrix[lay][i]=v[cnt++];
        if(++t>b) break;
        for(int i=t;i<=b;i++) matrix[i][n-1-lay]=v[cnt++];
        if(--r<l) break;
        for(int i=r;i>=l;i--) matrix[m-1-lay][i]=v[cnt++];
        if(--b<t) break;
        for(int i=b;i>=t;i--) matrix[i][lay]=v[cnt++];
        if(++l>r) break;
        layer++;
    }

    for(auto v: matrix){
        copy(v.begin(),v.end(),ostream_iterator<int>(cout," "));
        cout<<endl;
    }
}
```

### count speical sub-cubes
this is to find number of sub-cubes which has a max ==k, and the subcube size is kxkxk
intuition: an extension of 2d matrix, sliding window to find the max.
brutal force: (n-k)^3*k^3*n (since we need to solve all k from 1 to n.)
```cpp
int numsubs(vector<int>& cubes,int n,int s){
    int ans=0;
    for(int i=0;i<=n-s;i++){
        for(int j=0;j<=n-s;j++){
            for(int k=0;k<=n-s;k++){
                int valid=1;
                for(int a=0;a<s && valid;a++){
                    for(int b=0;b<s && valid;b++){
                        for(int c=0;c<s && valid;c++){
                            int ind=(i+a)*n*n+(j+b)*n+(k+c);
                            if(cubes[ind]>s) {valid=0;break;}
                            if(cubes[ind]==s) {valid=2;}
                        }
                    }
                }
                ans+=valid==2;
            }
        }
    }
    return ans;
}
vector<int> specialSubCubes(vector<int> cube) {
    vector<int> ans;
    int n3=cube.size(); //limit 50: 
    int n=cbrt(n3);
    for(int i=1;i<=n;i++){
        ans.push_back(numsubs(cube,n,i));
    }
    return ans;
}

```
- cbrt is stl for calculate cube root of a number.
- pay attention to using s as side length
- for many cases it TLE. and we need optimizations.

the key for optimization is:
Any cube of side=k is composed of six smaller cubes of side=k-1.
to understand this, we first use 2d matrix as an example
if we want to calculate the max for the 3x3 matrix.
1 2 3
4 5 6
7 8 9 
we can get the 4 submatrix max (yes, they are overlapped):
1 2
4 5
and
2 3
5 6
and
4 5
7 8
and
5 6
8 9
So this approach saves the previous max and saves a lot of calculations.
this is a dp approach.
It is not so easy to get the 8 cubes correct for 3d cases.
suppose we have a 2x2 cube: if we cut it into 1x1, it has 8 vertex, each vertex shall extends out one 
```cpp
int numsubs(int n,int s,vector<vector<vector<int>>>& dp){
    int ans=0;
    vector<vector<vector<int>>> prev=dp;
    for(int i=0;i<=n-s;i++){
        for(int j=0;j<=n-s;j++){
            for(int k=0;k<=n-s;k++){
                //8 s-1 smaller cubes located at the 8 vertexs.
                dp[i][j][k]=max({prev[i][j][k],prev[i][j+1][k],
                    prev[i][j][k+1],prev[i+1][j][k],
                    prev[i][j+1][k+1],prev[i+1][j][k+1],prev[i+1][j+1][k],
                    prev[i+1][j+1][k+1]
                });
                ans+=dp[i][j][k]==s;
            }
        }
    }
    return ans;
}
vector<int> specialSubCubes(vector<int> cube) {
    vector<int> ans;
    int n3=cube.size(); //limit 50: 
    int n=cbrt(n3);
    vector<vector<vector<int>>> dp(n,vector<vector<int>>(n,vector<int>(n)));
    int cnt=0;
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            for(int k=0;k<n;k++){
                int ind=i*n*n+j*n+k;
                dp[i][j][k]=cube[ind];
                if(cube[ind]==1) cnt++;
            }
        }
    }
    ans.push_back(cnt);
    for(int i=2;i<=n;i++){
        ans.push_back(numsubs(n,i,dp));
    }
    return ans;
}
```

### interval selection (5 star)
similar to leetcode double booking 
find the length of the longest chain.
intuition: dp approach. only allows up to 2 overlapping intervals.
if we sort by the ending time, dp subproblem will be the largest subset ending at Ti.
this can be solved in O(nlogn) with greedy algo. Sort all the end points, then iterate through all the end points using a simple array of size 2 to keep track of the intervals that are still open(haven't met their closing point yet). Whenever the opening intervals reached 3, just remove the interval that spans the furtherst.
similar problem: leetcode 1326. Minimum Number of Taps to Open to Water a Garden

```cpp
int intervalSelection(vector<vector<int>> intervals) {
    int ans=0;
    sort(intervals.begin(),intervals.end(),[](vector<int>& a,vector<int>& b){
        return a[1]<b[1];
    });
    int right0=0,right1=0;
    for(int i=0;i<intervals.size();i++){
        int start=intervals[i][0],end=intervals[i][1];
        if(start>right1){ //current interval on the right of t1 (disjoint)
            ans++;
            right1=end;
        }
        else if(start>right0){ //current interval in the range (right0,right1)
            ans++;
            right0=end; //discard right0
            if(right0>right1) swap(right0,right1); //keep right1>right0
        }
	else {} //invalid case, we have to discard it.
    }
    return ans;
}
```
- actually we only need to keep two rights. right0<right1, which is more understandable.
- valid case 1: current start>right1
- valid case 2: current start in the range [right0,right1]. In this case, we need discard right0
- invalid case: current start <=right0, and causes triple overlap. and it is the longest among the three.

First, sort by the end time. That part is easy, and is intuitive from the 1 resource case. Since we are dealing with the 2 resource interval scheduling problem, our greedy method is more complicated than the simple "just keep grabbing the next compatible interval with the smallest end time" that we would use in the 1-resource problem. Instead, for every interval, we need to decide which resource to give it to. Here's the rule: we keep track the most recent interval placed in each of the two resources, and for a new interval, we replace the interval of the resource with the latest compatible end time (if any). Doing this allows the highest chance of some interval with a later end time being able to fit in the other resource, since that other resource has an earlier end time, so is strictly more likely to make accomodations for some interval we will encounter later. Hopefully this helps people, as I found most of the comments on this thread about the greedy solution utterly incomprehensible.

The sorting is O(N lg N) and the rest is O(N).

### connected cells in a grid
simple dfs.

### the longest common subsequence
dp with traceback
```cpp
vector<int> longestCommonSubsequence(vector<int> a, vector<int> b) {
    int m=a.size(),n=b.size();
    //dp: approach dp[i,j] the max length of the common subsequence
    //also need to record the previous value
    vector<vector<int>> dp(m+1,vector<int>(n+1));
    for(int i=0;i<m;i++){
        for(int j=0;j<n;j++){
            if(a[i]==b[j]) dp[i+1][j+1]=dp[i][j]+1;
            else dp[i+1][j+1]=max(dp[i][j+1],dp[i+1][j]);
        }
    }
    //cout<<"maxlen="<<dp[m][n]<<endl;
    int maxlen=dp[m][n];
    vector<int> ans;
    int i=m,j=n;
    while(maxlen){
        if(a[i-1]==b[j-1]){
            ans.push_back(a[i-1]);
            i--,j--;
            maxlen--;
        }
        else{
            if(dp[i-1][j]<dp[i][j-1]){ //we are choose advance i in the previous loop
                j--;
            }
            else i--;
        }
    }
    reverse(ans.begin(),ans.end());
    return ans;
}
```
A bit tricky when traceback, we need back j if we advance i in the previous loop
### larry's array
choose any consecuative subarray and do number of rotations and see if we can sort the array.
idea: find 1 and rotate it to top, find 2 and rotate it to second....
TBD




