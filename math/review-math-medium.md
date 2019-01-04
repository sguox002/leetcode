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

