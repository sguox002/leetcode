## contest 104

X of a Kind in a Deck of Cards3
Partition Array into Disjoint Intervals5
Word Subsets6
Cat and Mouse8

914. X of a Kind in a Deck of Cards
<em>
In a deck of cards, each card has an integer written on it.

Return true if and only if you can choose X >= 2 such that it is possible to split the entire deck into 1 or more groups of cards, where:

Each group has exactly X cards.
All the cards in each group have the same integer.
</em>

- find the gcd of all card's number.

```cpp
    bool hasGroupsSizeX(vector<int>& deck) {
        unordered_map<int, int> count;
        int res = 0;
        for (int i : deck) count[i]++;
        for (auto i : count) res = __gcd(i.second, res);
        return res > 1;
    }
```

915. Partition Array into Disjoint Intervals
<em>
Given an array A, partition it into two (contiguous) subarrays left and right so that:

Every element in left is less than or equal to every element in right.
left and right are non-empty.
left has the smallest possible size.
Return the length of left after such a partitioning.  It is guaranteed that such a partitioning exists.
</em>
lmax<rmin

```cpp
    int partitionDisjoint(vector<int>& A) {
        int n = A.size(), pmax = 0;
        vector<int> B(n);
        B[n - 1] = A[n - 1];
        for (int i = n - 2; i > 0; --i)
            B[i] = min(A[i], B[i + 1]);
        for (int i = 1; i < n; ++i) {
            pmax = max(pmax, A[i - 1]);
            if (pmax <= B[i]) return i;
        }
    }
```


916. Word Subsets
<em>
We are given two arrays A and B of words.  Each word is a string of lowercase letters.

Now, say that word b is a subset of word a if every letter in b occurs in a, including multiplicity.  For example, "wrr" is a subset of "warrior", but is not a subset of "world".

Now say a word a from A is universal if for every b in B, b is a subset of a. 

Return a list of all universal words in A.  You can return the words in any order.
</em>

combine b: but only takes the max frequency in b.
```cpp
    vector<string> wordSubsets(vector<string>& A, vector<string>& B) {
        vector<int> bmp(26);
        //reduce B into one word
        for(int i=0;i<B.size();i++)
        {
            vector<int> tmp(26);
            for(int j=0;j<B[i].size();j++) tmp[B[i][j]-'a']++;
            for(int j=0;j<26;j++)
            {
                bmp[j]=max(bmp[j],tmp[j]);
            }
        }
        
        vector<string> ans;
        for(int i=0;i<A.size();i++)
        {
            vector<int> tmp(26);
            for(int j=0;j<A[i].size();j++) tmp[A[i][j]-'a']++;
            bool found=1;
            for(int j=0;j<26;j++)
            {
                if(bmp[j]>tmp[j]) {found=0;break;}
            }
            if(found) ans.push_back(A[i]);
        }
        return ans;
    }
```

913. Cat and Mouse

<em>
A game on an undirected graph is played by two players, Mouse and Cat, who alternate turns.

The graph is given as follows: graph[a] is a list of all nodes b such that ab is an edge of the graph.

Mouse starts at node 1 and goes first, Cat starts at node 2 and goes second, and there is a Hole at node 0.

During each player's turn, they must travel along one edge of the graph that meets where they are.  For example, if the Mouse is at node 1, it must travel to any node in graph[1].

Additionally, it is not allowed for the Cat to travel to the Hole (node 0.)

Then, the game can end in 3 ways:

If ever the Cat occupies the same node as the Mouse, the Cat wins.
If ever the Mouse reaches the Hole, the Mouse wins.
If ever a position is repeated (ie. the players are in the same position as a previous turn, and it is the same player's turn to move), the game is a draw.
Given a graph, and assuming both players play optimally, return 1 if the game is won by Mouse, 2 if the game is won by Cat, and 0 if the game is a draw.
</em>

too hard!!	
