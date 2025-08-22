[Problem Link](https://leetcode.com/problems/diameter-of-binary-tree/description/)
### Problem Statement : 

Given the `root` of a binary tree, return _the length of the **diameter** of the tree_.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

**Example 1:**

```
        1
       / \
      2   3
     / \
    4   5

output : 3 [4 → 2 → 1 → 3]

```
`

---


###  Approach 1 :

- The **diameter at a given node** =
    `height(left subtree) + height(right subtree)`
- At every node, calculate:
    - Left height
    - Right height
    - Update max diameter = `max(diameter, lh + rh)`
- Return the **maximum diameter found** during traversal.

- Note : This approach is different from the one in find diameter of tree as in binary tree we have only two child so we can add that, but in tree we can have many child so 2 dfs approach is used.

#### Code :

```cpp
class Solution {
    int diameter;
    
    int depth(TreeNode* root) {
        if (!root) return 0;

        int lh = depth(root->left);
        int rh = depth(root->right);

        // Update diameter at this node
        diameter = max(diameter, lh + rh);

        return 1 + max(lh, rh);
    }

public:
    int diameterOfBinaryTree(TreeNode* root) {
        diameter = 0;
        depth(root);
        return diameter;
    }
};

```


> `Time Complexity` : `O(n)`
> 
> `Space Complexity` : O(H) recursion stack 

---

