[Problem Link](https://leetcode.com/problems/symmetric-tree/description/)
### Problem Statement : 

Given the `root` of a binary tree, _check whether it is a mirror of itself_ (i.e., symmetric around its center).

**Example 1:**

```
Input : 
        1
       / \
      2   2
     / \ / \
    3  4 4  3
    
Output : true

Input : 
        1
       / \
      2   2
       \    \
        3    3
    
Output : true


```
`

---

###  Approach 1 :

- **Postorder Traversal**:
    - Last element is always the **root**.
- **Inorder Traversal**:
    - Elements to the **left** of the root are in the **left subtree**.
    - Elements to the **right** of the root are in the **right subtree**.
- **Approach**:
    - Start from the last element in postorder (root).
    - Use inorder to split into left & right subtrees.
    - Recursively construct left and right subtrees.
    - Use a **hashmap** to store indices of inorder elements for O(1) lookup.



#### Code :

```cpp
class Solution {
public:
    bool isMirror(TreeNode* p, TreeNode* q) {
        if (!p && !q) return true;
        if (!p || !q) return false;
        if (p->val != q->val) return false;
        return isMirror(p->left, q->right) && isMirror(p->right, q->left);
    }

    bool isSymmetric(TreeNode* root) {
        if (!root) return true;
        return isMirror(root->left, root->right);
    }
};
```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(h) recursion stack

---

