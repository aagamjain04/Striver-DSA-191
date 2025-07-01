[Problem Link](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)
### Problem Statement : 

Given an integer array sorted in non-decreasing order, remove the duplicates in place such that each unique element appears only once. The relative order of the elements should be kept the same.

If there are k elements after removing the duplicates, then the first k elements of the array should hold the final result. It does not matter what you leave beyond the first k elements.

**Note:** Return k after placing the final result in the first k slots of the array.


## Examples

**Example 1:**

```
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
```

### Approach 1 :

- Declare a HashSet.
- Run a for loop from starting to the end.
- Put every element of the array in the set.
- Store size of the set in a variable K.
- Now put all elements of the set in the array from the starting of the array.
- Return K.


#### Code :

``` cpp
int removeDuplicates(vector<int>& nums) {
        
	int n = nums.size();
	set<int> s(nums.begin(),nums.end());
	int k = 0;
	for(auto i:s){
		nums[k] = i;
		k++;
	}
	return k;
}
```


> `Time Complexity` : O(nlogn)
> 
> `Space Complexity` : O(n)


---


### Approach 2 :

- Two pointer approach
- **Slow pointer (k)**: Points to the position where the next unique element should be placed
- **Fast pointer (i)**: Scans through the array to find unique elements



#### Code :

``` cpp
int trap(vector<int>& height) {

	int n = nums.size();
	int k = 1;

	for(int i=1;i<n;i++){
		if(nums[i]!=nums[k-1]){
			nums[k] = nums[i];
			k++;
		}
	}
	return k;
}

```


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)

---
