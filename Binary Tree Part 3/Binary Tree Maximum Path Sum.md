[Problem Link](https://leetcode.com/problems/binary-tree-maximum-path-sum/description/)
### Problem Statement : 

A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return _the maximum **path sum** of any **non-empty** path_.

**Example 1:**

```
Input :
        -10
        /  \
       9   20
          /  \
         15   7


Output :
Paths:
  - Single nodes: 9, -10, 20, 15, 7
  - Paths: 15 → 20 → 7 = 42
Answer = 42


```
`

---


###  Approach 1 :

- At every node, we consider **three roles this node can play** in the maximum path:
1. **Standalone node**
    - Path consists of this node only (`root->val`).
    - Important if both children give negative contributions.
2. **Path extender (upwards contribution)**
    - Path = `node + max(left, right)`.
    - This case is returned to the parent, because when passing a contribution upward, we can only choose **one side**(a path can’t split upward).
3. **Path bridge (local maximum at this node)**
    - Path = `node + left + right`.
    - This represents the case where the path **passes through the current node**, connecting both subtrees.
    - This candidate **updates the global maximum**, but cannot be returned to parent.

#### Code :

```cpp
class Solution {
public:
    int maxSum; // Global variable to track the maximum path sum
    
    // Recursive helper function
    int computePath(TreeNode* root) {
        if (!root) return 0; // Base case: null node contributes 0
        
        // Recursively calculate max path sum from left and right subtrees
        int leftPath = computePath(root->left);
        int rightPath = computePath(root->right);
        
        // Option 1: Just take current node
        int onlyNode = root->val;
        
        // Option 2: Node + left + right (path passes through current node)
        int nodeAsBridge = root->val + leftPath + rightPath;
        
        // Option 3: Node + either left OR right (extend path upwards)
        int nodeWithOneChild = root->val + max(leftPath, rightPath);
        
        // Best possible path including current node
        int bestAtNode = max({onlyNode, nodeAsBridge, nodeWithOneChild});
        
        // Update global maximum
        maxSum = max(maxSum, bestAtNode);
        
        // Return contribution to parent → node alone OR node + one child
        return max(onlyNode, nodeWithOneChild);
    }

    int maxPathSum(TreeNode* root) {
        maxSum = INT_MIN;   // Initialize with smallest integer
        computePath(root);
        return maxSum;
    }
};


```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(H) recursion stack

---

