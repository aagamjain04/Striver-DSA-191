
[Problem Link](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)

### Problem Statement : 

Given an integer array `nums` and an integer `k`, return _the_ `kth` _largest element in the array_.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

## Examples

**Example 1:**

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

### Approach 1 :

- Sort the array in **descending order**.
- Return the element at index `k-1`.
- **Time Complexity:** `O(n log n)` (due to sorting).
- **Space Complexity:** `O(1)` (in-place sorting).


---

### Approach 2:

- Use a **min-heap** of size `k`.
- Traverse the array:
    - Push each element into the heap.
    - If heap size exceeds `k`, pop the smallest element.
- After processing all elements, the **top of the heap** is the `kth largest`.

#### Code :

``` cpp

int findKthLargest(vector<int>& nums, int k) {
	int n = nums.size();
	priority_queue<int,vector<int>,greater<int>> minHeap;

	for(auto &i:nums){

		minHeap.push(i);
		if(minHeap.size()>k){
			minHeap.pop();
		}
	}

	return minHeap.top();
}
```



> `Time Complexity` : O(n log k)
> 	
> `Space Complexity` : O(1)

---

### Approach 3:

- Partition array similar to quick sort.
- Recursively search in one partition until pivot is at index `n - k`.
- Quickselect is a selection algorithm based on **Quicksort partitioning**.
- Instead of fully sorting, it partitions until the pivot is at position `n - k`.

#### 1. Partitioning
- Choose last element as pivot.
- Move all elements **≤ pivot** to left, rest to right.
- This produces **ascending order** relative to pivot.

#### 2. Pivot Position
- After partition, pivot is in its **correct sorted position**.
- Compare this position (`p`) with target index `k`:
  - If `p == k` → found answer.
  - If `p < k` → search right side.
  - If `p > k` → search left side.

#### Code :

``` cpp
int quickSelect(int l,int r,vector<int> &nums,int k){

	int pivot = nums[r];
	int p = l;

	for(int i=l;i<r;i++){
		if(nums[i]<=pivot){
			swap(nums[i],nums[p]);
			p++;
		}
	}
	swap(nums[p],nums[r]);

	if(p==k)
	return nums[p];
	else if(k>p)
	return quickSelect(p+1,r,nums,k);
	else return quickSelect(l,p-1,nums,k);
}

int findKthLargest(vector<int>& nums, int k) {
	int n = nums.size();

	return quickSelect(0,n-1,nums,n-k);
}

```

> `Time Complexity Average` : O(n)
> 
>  Time Complexity Worst : O(n^2) (rare)
> 	
> `Space Complexity` : O(1)


`In leetcode this approch this gives TLE as one of the test case as almost all values similar`

---

## **Choosing the Approach**

- Small input size → Sorting is fine.
- Large input + small k → Min-Heap is better.
- Randomized + large n → Quickselect can be best.

---
