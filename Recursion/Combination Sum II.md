[Problem Link](https://leetcode.com/problems/combination-sum-ii/description/)
### Problem Statement : 

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.
## Examples

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: [
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

### Approach 1 :

- `The key difference from Combination Sum I is handling duplicates:
- Also same element cannot be picked again, so once picked up move to `j+1`

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
		// Skip duplicates at same recursion level
		if(j>i && candidates[j]==candidates[j-1])continue; 

		//Pruning: if current number exceeds remaining, stop
		if(currSum + candidates[j] > target)break; 

		temp.push_back(candidates[j]);
		recur(j+1,currSum+candidates[j],target,candidates,temp,res);
		temp.pop_back();
	   
	}

}

vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
	
	sort(candidates.begin(),candidates.end());
	vector<int> temp;
	vector<vector<int>> res;

	recur(0,0,target,candidates,temp,res);

	return res;

}

```


> `Time Complexity` : O(2^n) - ignoring sorting as it can be ignored here
> 
> `Space Complexity` : O(n) - Auxiliary stack space and ignoring result space

---
