[Problem Link](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)
### Problem Statement : 

You are given the `root` of a binary search tree (BST) and an integer `val`.

Find the node in the BST that the node's value equals `val` and return the subtree rooted with that node. If such a node does not exist, return `null`.

---


###  Approach 1 :

- If `root == NULL` → return `NULL`.
- If `root->val == val` → found, return node.
- If `val < root->val` → search in `root->left`.
- Else → search in `root->right`.

#### Code :

```cpp

# Recursive
TreeNode* searchBST(TreeNode* root, int val) {
	if (!root) return NULL;
	if (root->val == val) return root;
	if (val < root->val) return searchBST(root->left, val);
	else return searchBST(root->right, val);
}

# Iterative
TreeNode* searchBST(TreeNode* root, int val) {
	while (root) {
		if (root->val == val) return root;
		else if (val < root->val) root = root->left;
		else root = root->right;
	}
	return NULL;
}

```


> `Time Complexity` : O(h)
> 
> `Space Complexity` : O(h) for recursive and O(1) for iterative

---

