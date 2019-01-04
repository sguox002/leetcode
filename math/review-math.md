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
