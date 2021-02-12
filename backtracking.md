## backtracking

backtracking is recursive based. So think it take a step and then solve the subproblem.
what defines the subproblem and what is the expected output and what are the exit condition.

backtracking is used to get the required combinations or path. 
similar to dp: take the first step and solve the subsequent subproblem. Many times it is also the base of top down dp approach.

backtracking is also similar to dfs, except we need collect information so we need do the backtracking by ourselves similar to dfs (done by recursive stack push and pop)

### general backtracking
dfs to get all required combinations or paths.
- it generally needs an array to store the results
- it generally includes add/removal elements from the array
- use reference passing of the array to save time.

- 22	Generate Parentheses    		64.2%	Medium	
- 784	Letter Case Permutation    		65.7%	Medium	
- 17	Letter Combinations of a Phone Number    		48.0%	Medium	
- 967	Numbers With Same Consecutive Differences    		44.1%	Medium	
- 797	All Paths From Source to Target    		78.2%	Medium	
- 1215	Stepping Numbers    		42.7%	Medium	
- 1291	Sequential Digits    		57.4%	Medium	
- 248	Strobogrammatic Number III    		39.9%	Hard	
- 247	Strobogrammatic Number II    		48.1%	Medium	
- 320	Generalized Abbreviation    		52.9%	Medium	
- 756	Pyramid Transition Matrix    		55.3%	Medium	
- 440	K-th Smallest in Lexicographical Order    		29.3%	Hard	
- 386	Lexicographical Numbers    		52.8%	Medium	
- 465	Optimal Account Balancing    		47.6%	Hard	
- 1601	Maximum Number of Achievable Transfer Requests    		47.2%	Hard	
- 1655	Distribute Repeating Integers    		40.0%	Hard	
- 1681. Minimum Incompatibility

example code:
- 22	Generate Parentheses    		64.2%	Medium	
generate all possible valid parentheses string with length n.

```
    vector<string> generateParenthesis(int n) {
        vector<string> ans;
        backtrack(n,n,"",ans);
        return ans;
    }
    void backtrack(int m,int n,string t,vector<string>& ans){
        if(!m && !n){
            ans.push_back(t);
            return;
        }
        if(m) backtrack(m-1,n,t+'(',ans);
        if(n>m) backtrack(m,n-1,t+')',ans);
    }
```	

756. Pyramid Transition Matrix

```
    unordered_map<string,vector<char>> mp;
    bool pyramidTransition(string bottom, vector<string>& allowed) {
        for(auto s: allowed) mp[s.substr(0,2)].push_back(s[2]);
        return backtrack(bottom,0,"");
    }
    bool backtrack(string& bott,int start,string next){
        if(bott.size()==1) return 1;
        if(start==bott.size()-1) return backtrack(next,0,"");
        
        for(char c: mp[bott.substr(start,2)]){
            if(backtrack(bott,start+1,next+c)) return 1;
        }
        
        return 0;
    }
```	

### permutations

permutation: loop to use each element as the first position and then a subproblem
- duplicate: using sort and skip, or using hashmap
- swap.
- permutation will not just go right, it shall loop the same whole range.


- 47	Permutations II    		48.4%	Medium	
- 46	Permutations    		65.2%	Medium	
- 267	Palindrome Permutation II    		36.9%	Medium	
- 266	Palindrome Permutation    		62.3%	Easy	
- 1079	Letter Tile Possibilities    		75.6%	Medium	
- 996	Number of Squareful Arrays    		47.5%	Hard	
- 332	Reconstruct Itinerary    		37.3%	Medium	
- 1467	Probability of a Two Boxes Having The Same Number of Distinct Balls    		61.3%	Hard	

### combinations
combination generally loops over each element and only goes to right.
so the range is keeping smaller.

- 90	Subsets II    		48.0%	Medium	
- 78	Subsets    		63.7%	Medium	
- 77	Combinations    		56.2%	Medium	
- 377	Combination Sum IV    		45.7%	Medium	
- 216	Combination Sum III    		59.4%	Medium	
- 40	Combination Sum II    		49.4%	Medium	
- 39	Combination Sum    		58.1%	Medium	
- 254	Factor Combinations    		47.0%	Medium	
- 416	Partition Equal Subset Sum    		44.3%	Medium	
- 698	Partition to K Equal Sum Subsets    		45.3%	Medium	
- 473	Matchsticks to Square    		37.9%	Medium	
- 1286	Iterator for Combination    		70.7%	Medium	
- 638	Shopping Offers    		52.1%	Medium	
- 491	Increasing Subsequences    		46.9%	Medium	
- 1258	Synonymous Sentences    		67.2%	Medium	
- 1735. Count Ways to Make Array With Product

### dp similar: try all prefix and solve subproblem
- 282	Expression Add Operators    		36.2%	Hard	
- 290	Word Pattern    		38.1%	Easy	
- 291	Word Pattern II    		43.8%	Hard	
- 816	Ambiguous Coordinates    		47.6%	Medium	
- 842	Split Array into Fibonacci Sequence    		36.4%	Medium	
- 131. Palindrome Partitioning
- 1593	Split a String Into the Max Number of Unique Substrings    		46.7%	Medium	

### board game
- 52	N-Queens II    		59.1%	Hard	
- 51	N-Queens    		48.2%	Hard	
- 1307	Verbal Arithmetic Puzzle    		37.6%	Hard	
- 425	Word Squares    		49.4%	Hard	
- 488	Zuma Game    		39.2%	Hard	
- 36 valid sudoko 
- 37 soduko solver 

