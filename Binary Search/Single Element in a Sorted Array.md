[Problem Link](https://leetcode.com/problems/single-element-in-a-sorted-array/description/)
### Problem Statement : 

You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once.

Return _the single element that appears only once_.

Your solution must run in `O(log n)` time and `O(1)` space.
## Examples

**Example 1:**

```
Input: nums = [1,1,2,3,3,4,4,8,8]
Output: 2
```

### Approach 1 :

- Brute force
- Iterate over entire array and check for the following condition :
```
If arr[i] != arr[i-1] and arr[i] != arr[i+1]
```
- If this condition is true for any element, `arr[i]`, we can conclude this is the single element.
- Edge cases : 
	-  If n == 1: This means the array size is 1. If the array contains only one element, we will return that element only.
	-  If i == 0: This means this is the very first element of the array. The only condition, we need to check is: `arr[i] != arr[i+1].`
    -  If i == n-1: This means this is the last element of the array. The only condition, we need to check is: `arr[i] != arr[i-1].`


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)

---

### Approach 2:

- Using XOR
- `a ^ a = 0`, XOR of two same numbers results in 0.
- `a ^ 0 = a`, XOR of a number with ) always results in that number.
- Now, if we XOR all the array elements, all the duplicates will result in 0 and we will be left with a single element.



> `Time Complexity` : O(n)
> 	
> `Space Complexity` : O(1)

---

### Approach 3:

-  Array is sorted; every element appears twice except one.
- **Before** single element: first in pair → even index, second → odd index.
- **After** single element: pattern breaks (shifts by 1).

```
1. Use binary search on index range `[0, n)`.
2. At mid `m`:
   - If `nums[m]` is different from both neighbors → return `nums[m]`.
   - If `(m % 2 == 0 && nums[m] == nums[m - 1])`  
     or `(m % 2 == 1 && nums[m] == nums[m + 1])` → single in left so move **left** (`r = m - 1`).
   - Else → move **right** (`l = m + 1`).
3. Return `nums[l]` when narrowed to one element.

```

#### Code :

```cpp
int singleNonDuplicate(vector<int>& nums) { 
	int n = nums.size();
	int l = 0, r = n; // Binary search boundaries

	// Binary search loop
	while (l + 1 < r) {
		int m = (l + r) / 2;

		// Case 1: Found the single element
		if (m > 0 && m < n && nums[m] != nums[m - 1] && nums[m] != nums[m + 1]) {
			return nums[m];
		}

		// Case 2: Check pattern and move search space
		if ((m % 2 == 0 && nums[m - 1] == nums[m]) || 
			(m % 2 == 1 && nums[m] == nums[m + 1])) {
			// Single element is on the left side
			r = m - 1;
		} else {
			// Single element is on the right side
			l = m + 1;
		}
	}

	// Return answer when narrowed down to one element
	return nums[l];
}
```

> `Time Complexity` : O(log n)
> 	
> `Space Complexity` : O(1)

---
