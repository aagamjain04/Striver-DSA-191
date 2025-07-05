[Problem Link](https://leetcode.com/problems/subsets-ii/)
### Problem Statement : 

Given an integer array `nums` that may contain duplicates, return all possible subsets (the power set).

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.
## Examples

**Example 1:**

```
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]
```

### Approach 1 :

- **Sort the array first** - This groups duplicates together
- **Skip duplicates at the same recursion level** - Prevents duplicate subsets
- **Use backtracking** - Explore all possibilities systematically

#### Code :

``` cpp
void recur(int i,vector<int> &nums, vector<int> &temp, vector<vector<int>> &res) {
    res.push_back(temp);  // Add current subset to result
    
    for(int j = i; j < nums.size(); j++) {
        // Skip duplicates at same level
        if(j > i && nums[j] == nums[j-1]) continue;
        
        temp.push_back(nums[j]);     // Include current element
        recur(j+1, nums, temp, res); // Recurse with next index
        temp.pop_back();             // Backtrack
    }
}

```


> `Time Complexity` : O(2^n) - Each element can be included or excluded
> 
> `Space Complexity` : O(n) - Recursion depth and temporary array

---
