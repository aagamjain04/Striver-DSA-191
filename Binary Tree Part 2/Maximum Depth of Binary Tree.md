[Problem Link](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)
### Problem Statement : 

Given the `root` of a binary tree, return _its maximum depth_.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

```
        3
       / \
      9   20
         /  \
        15   7
        
output : 3

```
`

---
###  Approach 1 :

1. Recursive approach
- If tree is empty -> depth = 0.
-  Otherwise:
    `maxDepth(root) = 1 + max(maxDepth(root->left), maxDepth(root->right))`
- Simple and intuitive.
    

2. Iterative (BFS / Level Order) Approach
- Use a queue for **level-order traversal**.
- Each level processed → increment depth counter.

#### Code :

```cpp
// DFS
int maxDepth(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(maxDepth(root->left), maxDepth(root->right));
}

// BFS
int maxDepth(TreeNode* root) {
    if (!root) return 0;

    queue<TreeNode*> q;
    q.push(root);
    int depth = 0;

    while (!q.empty()) {
        int size = q.size();
        depth++;
        for (int i = 0; i < size; i++) {
            TreeNode* node = q.front();
            q.pop();
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
    }
    return depth;
}
```


> `Time Complexity` : `O(n)`
> 
> `Space Complexity` : Recursive DFS → `O(H)` (stack space, where H = height), BFS → `O(N)` (queue storage).

---

