[Problem Link](https://leetcode.com/problems/balanced-binary-tree/description/)
### Problem Statement : 

Given a binary tree, determine if it is **height-balanced**.

A **height-balanced** binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

---


###  Approach 1 :

- For each node, compute left height & right height → check balance.
#### Code :

```cpp
```cpp
int go(TreeNode* root,bool& ans){
	if(root==NULL)
	return 0;

	
	int lh = go(root->left,ans);
	int rh = go(root->right,ans);

	if(abs(lh-rh)>1) {
		ans = false;
	}

	return max(lh,rh)+1;


}

bool isBalanced(TreeNode* root) {
	bool ans = true;
	go(root,ans);
	return ans;
	
}
```



> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(H) recursion stack 

---

