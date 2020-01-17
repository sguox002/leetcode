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


 
 









