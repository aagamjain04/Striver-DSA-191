[Problem Link](https://leetcode.com/problems/combination-sum/description/)
### Problem Statement : 

Given an array of **distinct** integers `candidates` and a target integer `target`, return a list of all **unique combinations** of `candidates` where the chosen numbers sum to `target`_._ You may return the combinations in **any order**.
## Examples

**Example 1:**

```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```

### Approach 1 :

- The solution uses a recursive backtracking approach to explore all possible combinations.
- Sorting the array to enable early pruning when `currSum + candidates[i] > target`.
- At each index `j`, you have **two choices**:
	1. **Pick** `candidates[j]` (and stay at `j` to allow reuse).
	2. **Skip** to `j + 1`

#### Code :

``` cpp
void recur(int i,int currSum,int target,vector<int>& candidates,vector<int> &temp,vector<vector<int>> &res){

	if(currSum == target){
		res.push_back(temp);
		return;
	}
	if(i>=candidates.size() || currSum > target)
	return;

	for(int j=i;j<candidates.size();j++){

		//Pruning: if current number exceeds remaining, stop
		if(currSum + candidates[j] > target)break;
		
		temp.push_back(candidates[j]);
		recur(j,currSum+candidates[j],target,candidates,temp,res);
		temp.pop_back();
	   
	}

}

vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
	
	sort(candidates.begin(),candidates.end());

	vector<int> temp;
	vector<vector<int>> res;

	recur(0,0,target,candidates,temp,res);

	return res;
}

```


> `Time Complexity` : O(2^target)  - Because for each element, you're branching into **pick** and **skip** paths, and combinations can go deep up to `target`.
> 
> `Space Complexity` : O(target) - Auxiliary stack space and ignoring result space

---
