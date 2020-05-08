## contest 184

### 1408. String Matching in an Array
<em>
Given an array of string words. Return all strings in words which is substring of another word in any order. 

String words[i] is substring of words[j], if can be obtained removing some characters to left and/or right side of words[j].

 

Example 1:

Input: words = ["mass","as","hero","superhero"]
Output: ["as","hero"]
Explanation: "as" is substring of "mass" and "hero" is substring of "superhero".
["hero","as"] is also a valid answer.
</em>

Intuition:
- apparently only shorter ones can be substring of another string
O(N^2) brutal force

```cpp
    vector<string> stringMatching(vector<string>& words) {
        vector<string> ans;
        sort(words.begin(),words.end(),[](string& a,string &b){
            return a.size()<b.size();
        });
        for(int i=0;i<words.size();i++){
            for(int j=i+1;j<words.size();j++){
                if(words[j].find(words[i])!=string::npos){
                    ans.push_back(words[i]);
                    break;
                }
            }
        }
        return ans;
    }
```	

can also use suffix tree to store all suffix in the trie.
overkill for an easy problem

### 1409. Queries on a Permutation With Key
<em>
Given the array queries of positive integers between 1 and m, you have to process all queries[i] (from i=0 to i=queries.length-1) according to the following rules:

In the beginning, you have the permutation P=[1,2,3,...,m].
For the current i, find the position of queries[i] in the permutation P (indexing from 0) and then move this at the beginning of the permutation P. Notice that the position of queries[i] in P is the result for queries[i].
Return an array containing the result for the given queries.

 

Example 1:

Input: queries = [3,1,2,1], m = 5
Output: [2,1,2,1] 
Explanation: The queries are processed as follow: 
For i=0: queries[i]=3, P=[1,2,3,4,5], position of 3 in P is 2, then we move 3 to the beginning of P resulting in P=[3,1,2,4,5]. 
For i=1: queries[i]=1, P=[3,1,2,4,5], position of 1 in P is 1, then we move 1 to the beginning of P resulting in P=[1,3,2,4,5]. 
For i=2: queries[i]=2, P=[1,3,2,4,5], position of 2 in P is 2, then we move 2 to the beginning of P resulting in P=[2,1,3,4,5]. 
For i=3: queries[i]=1, P=[2,1,3,4,5], position of 1 in P is 1, then we move 1 to the beginning of P resulting in P=[1,2,3,4,5]. 
Therefore, the array containing the result is [2,1,2,1].  
</em>

Approach: simulation of the query. using hashmap to record the value vs index.

```cpp
    vector<int> processQueries(vector<int>& queries, int m) {
        unordered_map<int,int> v2i,i2v;
        for(int i=0;i<m;i++) {
            v2i[i+1]=i;//val vs index
            i2v[i]=i+1;
        }
        vector<int> ans;
        for(int q:queries){
            int ind=v2i[q];
            int val=i2v[0];
            //not swapping but move to the head
            for(int i=ind;i>0;i--){
                swap(v2i[i2v[i]],v2i[i2v[i-1]]);
                swap(i2v[i],i2v[i-1]);
            }
            ans.push_back(ind);
        }
        return ans;
    }
```

This is brutal force and has a complexity of O(N^2)
to have a better efficiency for removing a node and insert a node, we can use linked_list
better approach using binary index tree or Fenwick tree,

### 1410. HTML Entity Parser
<em>
HTML entity parser is the parser that takes HTML code as input and replace all the entities of the special characters by the characters itself.

The special characters and their entities for HTML are:

Quotation Mark: the entity is &quot; and symbol character is ".
Single Quote Mark: the entity is &apos; and symbol character is '.
Ampersand: the entity is &amp; and symbol character is &.
Greater Than Sign: the entity is &gt; and symbol character is >.
Less Than Sign: the entity is &lt; and symbol character is <.
Slash: the entity is &frasl; and symbol character is /.
Given the input text string to the HTML parser, you have to implement the entity parser.

Return the text after replacing the entities by the special characters.
</em>

straightforward: find and mark all position and replace from right to left.
cannot do inplace find and replace.

```cpp
    string entityParser(string text) {
        unordered_map<string,string> dict;
        dict.insert({"&quot;","\""});
        dict.insert({"&apos;","\'"});
        dict.insert({"&amp;","&"});
        dict.insert({"&gt;",">"});
        dict.insert({"&lt;","<"});
        dict.insert({"&frasl;","/"});
        string ans;
        for(auto t: dict){
            string patt=t.first,rep=t.second;
            int start=0;
            vector<int> pos;
            while(start<text.size()){
                start=text.find(patt,start);
                if(start==string::npos) break;
                pos.push_back(start);
                start+=patt.size();
            }
            for(int i=pos.size()-1;i>=0;i--){
                text.replace(pos[i],patt.size(),rep);
            }
        }
        return text;
    }
```	
	
### 1411. Number of Ways to Paint N × 3 Grid
<em>
You have a grid of size n x 3 and you want to paint each cell of the grid with exactly one of the three colours: Red, Yellow or Green while making sure that no two adjacent cells have the same colour (i.e no two cells that share vertical or horizontal sides have the same colour).

You are given n the number of rows of the grid.

Return the number of ways you can paint this grid. As the answer may grow large, the answer must be computed modulo 10^9 + 7.
</em>

Intuition: dp approach
- each layer can only have two choices: aba, abc
- aba: a has 3 options, b has 2 options total 6. abc has 3x2x1=6 options
- if previous one is aba, current layer can be aba and abc.
for example previous is RGR, current layer can be:
first can be G and B, second one can be B and R. third one can be G and B

G-B-G
  R-G
    B
B-R-G
    B
that is 3 type of aba and 2 type of abc

if previous is abc, for example RGB
then first one can be GB, secnd one can be RB, third one can be RG.
GRG
GBG
GBR
BRG

that is 2 types of aba and 2 types of abc

aba=6,abc=6 for n=1;
next layer: aba=4*aba+3*abc, abc=aba+2*abc
```cpp
    int numOfWays(int n) {
		int mod=1e9+7;
		vector<long> aba(n+1),abc(n+1);
		aba[1]=abc[1]=6;
		for(int i=2;i<=n;i++){
			aba[i]=3*aba[i-1]+2*abc[i-1];
			abc[i]=2*aba[i-1]+2*abc[i-1];
			aba[i]%=mod;
			abc[i]%=mod;
		}
		return (aba[n]+abc[n])%mod;
    }
```	
and this can reduce to O(1) space.
