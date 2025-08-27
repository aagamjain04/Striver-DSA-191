[Problem Link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)
### Problem Statement : 

Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

**Example 1:**


```
Input : 
        6
       / \
      2   8
     / \ / \
    0  4 7  9
      / \
     3   5

Output : 
p = 2, q = 8 -> LCA=6
p = 2, q = 4 -> LCA=2

```

---

###  Approach 1 :

- In a BST:
    - All nodes in the **left subtree** are smaller than the root.
    - All nodes in the **right subtree** are larger than the root.
        
- So:
    - If both `p` and `q` are **smaller** than `root` → LCA lies in the **left subtree**.  
    - If both `p` and `q` are **greater** than `root` → LCA lies in the **right subtree**.
    - Otherwise → the current `root` is the **split point** → **LCA found**.

#### Code :

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == NULL) return NULL;

        // If both p and q are smaller than root → move left
        if(root->val > p->val && root->val > q->val)
            return lowestCommonAncestor(root->left, p, q);

        // If both p and q are greater than root → move right
        else if(root->val < p->val && root->val < q->val)
            return lowestCommonAncestor(root->right, p, q);

        // Otherwise, root is the split point → LCA found
        else
            return root;
    }
};
```


> `Time Complexity` : - O(h) where `h` = height of BST
> - Best case (balanced BST): `O(log n)`
> - Worst case (skewed BST): `O(n)`
> 
> `Space Complexity` : O(H) recursion stack (H = tree height) 

---

### Approach 2 :

- Iterative

#### Code :

```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        while(root) {
            // If both nodes are smaller → move left
            if(root->val > p->val && root->val > q->val) {
                root = root->left;
            }
            // If both nodes are greater → move right
            else if(root->val < p->val && root->val < q->val) {
                root = root->right;
            }
            // Split point found
            else {
                return root;
            }
        }
        return NULL;
    }
```

> `Time Complexity` : - O(h) where `h` = height of BST
> - Best case (balanced BST): `O(log n)`
> - Worst case (skewed BST): `O(n)`
> 
> `Space Complexity` : O(1)

---
