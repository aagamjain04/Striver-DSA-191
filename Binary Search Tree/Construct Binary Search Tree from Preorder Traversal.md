[Problem Link](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/description/)
### Problem Statement : 

Given an array of integers preorder, which represents the **preorder traversal** of a BST (i.e., **binary search tree**), construct the tree and return _its root_.

**Example 1:**


```
input :
preorder = [8, 5, 1, 7, 10, 12]


Output :
        8
       / \
      5   10
     / \     \
    1   7     12

```

---

###  Approach 1 :

- **Preorder Traversal Property**:
    - The first element of preorder is always the **root** of the BST.
- **BST Insertion**:
    - For each subsequent element, insert it into the BST by following BST rules:
        - If value `< curr->val` → go to the left subtree.
        - If value `> curr->val` → go to the right subtree.
- **Iterative Placement**:
    - Start from root, keep traversing down until you find the correct null spot (`left` or `right`) where the new node should be placed.
    - Insert node there.
- **Repeat for every element** → eventually constructs the full BST.

#### Code :

```cpp
TreeNode* bstFromPreorder(vector<int>& preorder) {
	TreeNode* root = new TreeNode(preorder[0]);

	for(int i=1;i<preorder.size();i++){
		int x = preorder[i];
		TreeNode* curr = root;
		while(curr){
			if(x > curr->val){
				if(curr->right==NULL){
					curr->right = new TreeNode(x);
				}
				curr = curr->right;
			}else if(x < curr->val){
				if(curr->left==NULL){
					curr->left = new TreeNode(x);
				}
				curr = curr->left;
			}else{
				curr = NULL;
			}
		}
	}

	return root;
}
```



> `Time Complexity` : Worst case `O(N^2)` (skewed tree, e.g., strictly increasing array).
> 
> `Space Complexity` : `O(1)` extra (iterative, no recursion stack).

---

###  Approach 2 :

**Recursive with bounds (Optimal)**:
- Use an index pointer while traversing preorder.
- Keep track of allowed value range (`min`, `max`).
- Place node only if value is within bounds.

#### Code :

``` cpp
class Solution {
public:

    int idx = 0;

    TreeNode* build(vector<int> &preorder,int low,int high){

        if(low > high || idx >= preorder.size())
        return NULL;

        int x = preorder[idx];
        
        if(x<low || x>high)
        return NULL;

        idx++;

        TreeNode* node = new TreeNode(x);
        node->left = build(preorder,low,x);
        node->right = build(preorder,x,high);

        return node;

    }

    TreeNode* bstFromPreorder(vector<int>& preorder) {
        
        return build(preorder,INT_MIN,INT_MAX);
    }
};
```


> `Time Complexity` : each element inserted once
> 
> `Space Complexity` : `recursion depth (`H = tree height`). Worst case `O(N)`, best case `O(log N)` for balanced tree.

---
