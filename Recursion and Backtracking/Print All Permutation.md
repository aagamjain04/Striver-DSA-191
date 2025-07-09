[Problem Link](https://leetcode.com/problems/permutations/description/)
### Problem Statement : 

Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in **any order**.
## Examples

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

### Approach 1 :

- Use **backtracking** to generate all possible permutations.
- If we pass `s` be reference then resultant array will not be lexicographically sorted
- Advantage will be array is not copied at every recursive call so more faster.
- For getting lexicographically sorted result, do not pass by reference.
#### Code :

``` cpp
void recur(int i,int n,vector<vector<int>> &res,vector<int> &nums){

	if(i==n){
		res.push_back(nums);
		return;
	}

	for(int j=i;j<n;j++){

		swap(nums[i],nums[j]);
		recur(i+1,n,res,nums);
		swap(nums[i],nums[j]);
	}
}

vector<vector<int>> permute(vector<int>& nums) {
	
	vector<vector<int>> res;

	int n = nums.size();

	recur(0,n,res,nums);

	return res;
}
```


> `Time Complexity` : O(n × n!) -> n! is number of recursive calls and n is copying `nums` into `res`
> 
> `Space Complexity` : `O(1)` (not considering stacks and result space)

---

