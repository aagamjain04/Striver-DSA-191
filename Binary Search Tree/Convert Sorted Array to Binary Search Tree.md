[Problem Link](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)
### Problem Statement : 

Given an integer array `nums` where the elements are sorted in **ascending order**, convert _it to a_ **_height-balanced_** _binary search tree_.

A **height-balanced** binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

**Example 1:**


```
Input : nums = [-10, -3, 0, 5, 9]


Output :
        0
       / \
     -3   5
     /      \
   -10       9

```

---

###  Approach 1 :

- Pick **middle element** of array as root → ensures equal split of elements.
- Left half → left subtree.
- Right half → right subtree.

**Recursive approach**
- Base case: if `l > r`, return `NULL`.
- Recursive step:
    - Mid = `(l + r) / 2`.
    - Create node with `nums[mid]`.
    - Build left subtree from `[l, mid-1]`.
    - Build right subtree from `[mid+1, r]`.

#### Code :

```cpp
TreeNode* helper(int low,int high,vector<int> &nums){

	if(high<low){
		return NULL;
	}

	int mid = (low+high)/2;

	TreeNode* node = new TreeNode(nums[mid]);

	node->left = helper(low,mid-1,nums);
	node->right = helper(mid+1,high,nums);

	return node;
}

TreeNode* sortedArrayToBST(vector<int>& nums) {
	
	int n = nums.size();
	return helper(0,n-1,nums);
}
```

```
How both odd and even sized arrays still produce balanced BSTs : 
“For odd-sized arrays, the middle element is unique and creates a perfectly balanced tree. For even-sized arrays, either of the two middle elements can be chosen as root. Both choices still produce a height-balanced BST because the subtree size difference is at most 1.”
```

> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(log N) -> recursion depth(balanced tree height) 

---

