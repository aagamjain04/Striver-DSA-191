[Problem Link](https://leetcode.com/problems/two-sum/description/)

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

## Examples

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```


### Approach 1 :

- Brute force
- For each element `x`, check for element `y`, such that `nums[i]=x` and `nums[j]=y` and `i!=j` and `x+y = target`

> `Time Complexity` : O(n²)
> 
> `Space Complexity` : O(1)

---

### Approach 2:

- Using hash-map
- Iterate over array and for each element `i`, check the following:
- If `target - arr[i]` exists in map, return 'YES'
- If not, store the current element in hash-map


>`Time Complexity` : O(N)  (map search will take O(1) for unordered map)
> 
> `Space Complexity` : O(N) for map


**Note:** In worst case search can take O(N) in unordered map and this happens rarely. Then T.C → O(N²)

And if we use ordered map then T.C: O(N × log N)

**Code** :

```cpp

vector<int> twoSum(vector<int>& nums, int target) {
        
	vector<int> res;
	unordered_map<int,int> mp;
	int n = nums.size();

	for(int i=0;i<n;i++){
		if(mp.count(target-nums[i])){
			res = {mp[target-nums[i]],i};
			return res;
		}else{
			mp[nums[i]] = i;
		}
	}
	return res;
}
```

---

### Approach 3 :

- Using two pointer
- Sort the array and keep one pointer at start index and other at end index
- if current sum is greater than sum , decrement end pointer
- else increment start pointer

**Code** :

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
        
	vector<pair<int,int>> vp;
	for(int i=0;i<nums.size();i++){
		vp.push_back({nums[i],i});
	}
	sort(vp.begin(),vp.end());

	int l = 0, r = nums.size()-1;

	while(l<r){
		if((vp[l].first+vp[r].first)==target){
			return {vp[l].second,vp[r].second};
		}else if((vp[l].first+vp[r].first)>target){
			r--;
		}else{
			l++;
		}
	} 

	return {};
}
```



>`Time Complexity` : O(NlogN)  
> 
>`Space Complexity` : O(1) (if only returning true/false otherwise if returning indexes it will be O(N))
> 


---

