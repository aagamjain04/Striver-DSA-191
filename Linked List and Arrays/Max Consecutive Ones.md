[Problem Link](https://leetcode.com/problems/max-consecutive-ones/description/)
### Problem Statement : 

Given a binary array `nums`, return _the maximum number of consecutive_ `1`_'s in the array_.

## Examples

**Example 1:**

```
Input: nums = [1,1,0,1,1,1]
Output: 3
```

### Approach 1 :

- We need to track consecutive sequences of 1s
- Reset the counter when we encounter a 0
- Keep track of the maximum consecutive count seen so far
- Single pass solution is optimal

#### Code :

``` cpp
int findMaxConsecutiveOnes(vector<int>& nums) {
        
	int n = nums.size();
	int maxCnt = 0;
	int cnt = 0;
	for(int i=0;i<n;i++){
		if(nums[i] == 1){
			cnt++;
			maxCnt = max(maxCnt,cnt);
		}else {
			cnt = 0;
		}
	}

	return maxCnt;
}
```


> `Time Complexity` : O(1)
> 
> `Space Complexity` : O(n)


---


