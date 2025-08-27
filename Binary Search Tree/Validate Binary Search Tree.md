[Problem Link](https://leetcode.com/problems/validate-binary-search-tree/description/)
### Problem Statement : 

Given the `root` of a binary tree, _determine if it is a valid binary search tree (BST)_.

**Example 1:**


```
Input : 
        5
       / \
      1   4
         / \
        3   6


Output : false

```

---

###  Approach 1 :

- **Inorder traversal of a BST** always produces a **strictly increasing sequence**.
    - If at any point the current node’s value `<=` the previous visited value → it’s not a BST.
- Your function maintains:
    - `prev` → the last visited node’s value.
    - `res` → boolean flag to track validity.
- During inorder traversal:
    - Visit **left subtree**.
    - Check `root->val > prev`.
    - Update `prev = root->val`.
    - Visit **right subtree**.

#### Code :

```cpp
class Solution {
public:
    void go(TreeNode* root, long &prev, bool &res) {
        if (!root || !res) return; // stop early if already invalid

        go(root->left, prev, res);

        if (root->val <= prev)
            res = false;
        else
            prev = root->val;

        go(root->right, prev, res);
    }

    bool isValidBST(TreeNode* root) {
        bool res = true;
        long prev = LONG_MIN; // use very small sentinel
        go(root, prev, res);
        return res;
    }
};
```


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(H) recursion stack (H = tree height) 

---

