[Problem Link](https://leetcode.com/problems/same-tree/description/)
### Problem Statement : 

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

---


###  Approach 1 :

- Base Case:
    - If both nodes are `NULL` → return true.
    - If one is `NULL` and the other is not → return false.
        
- Recursive Step:
    - Check `p->val == q->val`.
    - Recursively check left subtrees and right subtrees.

#### Code :

```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(!p && !q) return true;          // both null
        if(!p || !q) return false;         // one null, other not
        if(p->val != q->val) return false; // values differ
        
        return isSameTree(p->left, q->left) &&
               isSameTree(p->right, q->right);
    }
};

```


> `Time Complexity` : `O(n)`
> 
> `Space Complexity` : O(H) recursion stack 

---

