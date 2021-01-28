
backtracking:
46. permutation
with all unique.
similar but simpler as 47.

47. permutation II
- using hashmap (with duplicates you always can try this)
- more robust, not easy to make mistakes.
```
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        unordered_map<int,int> mp;
        for(int i: nums) mp[i]++;
        vector<vector<int>> ans;
        int n=nums.size();
        backtrack(mp,n,{},ans);
        return ans;
    }
    void backtrack(unordered_map<int,int>& mp,int n,vector<int> v,vector<vector<int>>& ans){
        if(v.size()==n) {
            ans.push_back(v);
            return;
        }
        for(auto& t: mp){
            if(t.second){
                t.second--;
                v.push_back(t.first);
                backtrack(mp,n,v,ans);
                v.pop_back();
                t.second++;
            }
        }
    }
```

skip duplicates
```
    vector<vector<int> > permuteUnique(vector<int> &num) {
        sort(num.begin(), num.end());
        vector<vector<int> >res;
        backtrack(num, 0, res);
        return res;
    }
    void backtrack(vector<int> nums, int start, vector<vector<int> > &res) {
        if (start == nums.size()-1) {
            res.push_back(nums);
            return;
        }
        for (int i = start; i < nums.size(); i++) {
            if (i != start && nums[start] == nums[i]) continue;
            swap(nums[i], nums[start]);
            backtrack(nums, start+1, res);
        }
    }    
```
- it uses value passing, so no need to swap back.	
- it uses the same input vector as output (once swapped, the right part is not sorted, and it is OK to include duplicates on the right.)
- when done one, the input array is retored back and we move to next.
- if we change to reference passing, and restore the array, we get much more sets.

using visited array and each time try the whole array
```
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(begin(nums),end(nums));
        vector<bool> v(nums.size());
        vector<vector<int>> ans;
        backtrack(nums,{},v,ans);
        return ans;
    }
    void backtrack(vector<int>& nums,vector<int> t,vector<bool>& v,vector<vector<int>>& ans){
        if(t.size()==nums.size()){
            ans.push_back(t);
            return;
        }
        for(int i=0;i<nums.size();i++){
            if(v[i]) continue;
            if(i && nums[i]==nums[i-1] && !v[i-1]) continue;
            v[i]=1;
            t.push_back(nums[i]);
            backtrack(nums,t,v,ans);
            v[i]=0;
            t.pop_back();
        }
    }
```
to skip duplicates:-
when a number has the same value with its previous, we can use this number only if his previous is used.	
(which is tricky).

62. subsets

```
	vector<vector<int>> subsets(vector<int>& nums) {	
		vector<vector<int>> ans;
		backtrack(nums,0,{},ans);
		return ans;
	}
	void backtrack(vector<int>& nums,int ind,vector<int> vt,vector<vector<int>>& ans)
	{
		ans.push_back(vt);
		for(int i=ind;i<nums.size();i++)
		{
			vt.push_back(nums[i]);
			backtrack(nums,i+1,vt,ans);
			vt.pop_back();
		}
	}
```	
for example [1,2,3]
it starts with []
then add 1, [1]
then add 2, [1,2]
then add 3, [1,2,3]
pop 3
pop 2
pop 1
add 2 [2]
add 3 [2,3]
pop 3
pop 2
add 3 [3]
