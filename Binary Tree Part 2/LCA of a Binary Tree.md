[Problem Link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)
### Problem Statement : 

Given a binary tree, find the **lowest common ancestor (LCA)** of two nodes `p` and `q`.

The LCA of two nodes `p` and `q` is defined as:

> The lowest (i.e., deepest) node in the tree that has both `p` and `q` as descendants (where a node can be a descendant of itself).

**Example 1:**

```
       3
      / \
     5   1
    / \ / \
   6  2 0  8
     / \
    7   4

Input: p = 5, q = 1    
Output: LCA = 3

Input: p = 5, q = 4
Output: LCA = 5

```
`

---


###  Approach 1 :

- Base Case:
    - If `root == NULL` → return `NULL`.
    - If `root == p` or `root == q` → return `root`.
- Recursively search LCA in left and right subtrees.
- If both left and right return non-NULL → current node is LCA.
- Otherwise, return non-NULL child.

#### Code :

```cpp
class Solution {
public:

    TreeNode* lca(TreeNode* root,TreeNode* p,TreeNode* q){

		if(root==NULL)
		return root;

        if(root==p || root==q){
            return root;
        }

        TreeNode* left = lca(root->left,p,q);
        TreeNode* right = lca(root->right,p,q);

        if(left==NULL)
        return right;
        else if(right==NULL)
        return left;
        else return root;
    }

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        
        return lca(root,p,q);
    }
};

```


> `Time Complexity` : `O(n)`
> 
> `Space Complexity` : O(H) recursion stack 

---

