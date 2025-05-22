
[Problem Link](https://leetcode.com/problems/majority-element/)

### Problem Statement : 
Given an array of N integers, write a program to return an element that occurs more than N/2 times in the given array. You may consider that such an element always exists in the array.

## Example

**Example 1:**

```
Input : nums = [3,2,3]
Output : 3
```

**Example 2:**

```
Input : nums = [2,2,1,1,1,2,2]
Output : 2
```


### Approach 1 : 

- Brute force
- Take one element and check its count in the array. Whichever has `count > N/2` return that element.

> **Time Complexity**: O(N²)
> 
> **Space Complexity**: O(1)

---

### Approach 2 :

- Use hash-map to store the count and whenever `frequency > N/2`, return that element.

> **Time Complexity**: O(N)
> 
> **Space Complexity**: O(N)

---

### Approach 3 :

#### Moore's Voting Element

- Initialize two variables : `count` and `majorityEle`
- Traverse through array
	- If count is zero then store current element as `majorityEle`
	- If current element and and `majorityEle` is same, increment count by 1
	- If they are different decrement count by 1
- The integer present in `majorityEle` is our majority element. Also to verify just iterate over array and verify count of `majorityEle` is greater than `N/2`.

### Implementation

```cpp
int majorityElement(vector<int>& nums) {
	int n = nums.size();
	int count = 0, majorityEle = 0;
	for(int i=0; i<n; i++){
		if(count==0){
			majorityEle = nums[i];
		}

		if(nums[i]==majorityEle){
			count++;
		}else{
			count--;
		}
	   
	}
	return majorityEle;
}
```



>**Time Complexity**: O(N) 
>
> **Space Complexity**: O(1)

---
