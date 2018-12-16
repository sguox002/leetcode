890. Find and Replace Pattern
permuation of pattern and match
we can transform both the pattern and the word into same abc.., the first char is a and second appeared is b...
and then check if they are equal

537. Complex Number Multiplication
just using stringstream to read real and imag correctly

791. Custom Sort String
s is sorted using some custom sort, we need sort string t
using hashmap to arrange the sorted chars first

553. Optimal Division
The second one is always under, others are above, so there is only one answer

647. Palindromic Substrings--dp
count the number of pal-substring
dp[i][j]=self+dp[i][j-1]+dp[i+1][j]-dp[i+1][j-1];

856. Score of Parentheses
using stack
or dp: dp[i] represents the score 
when (, the score is 0
