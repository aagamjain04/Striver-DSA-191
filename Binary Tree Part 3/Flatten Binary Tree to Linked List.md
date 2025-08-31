[Problem Link](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/description/)
### Problem Statement : 

Given the `root` of a binary tree, flatten the tree into a "linked list":

- The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
- The "linked list" should be in the same order as a [**pre-order** **traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR) of the binary tree.

**Example 1:**

```
Input : 
        1
       / \
      2   5
     / \    \
    3   4    6

    
Output : 1 -> 2 -> 3 -> 4 -> 5 -> 6

```
`

---

###  Approach 1 :

- Recursive
- Traverse `right → left → root`.
- At each node, point its `right` to the previously processed node (`prev`).
- Set `left = NULL`.
- Update `prev` to current node.


#### Code :

```cpp
class Solution {
public:
    TreeNode* prev;

    void go(TreeNode* root) {
        if (!root) return;

        // Reverse preorder: right -> left -> root
        go(root->right);
        go(root->left);

        root->right = prev;
        root->left = NULL;
        prev = root;
    }

    void flatten(TreeNode* root) {
        prev = NULL;
        go(root);
    }
};
```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(h) recursion stack

---

###  Approach 2 :

- Iterative morris like solution
- If `curr->left` exists:
    - Find the **rightmost node** of `curr->left`.
    - Attach `curr->right` as this node’s `right`.
    - Move the left subtree to `curr->right`, and set `curr->left = NULL`.
- Move to next `curr->right`.


#### Code :

```cpp
class Solution {
public:
    void flatten(TreeNode* root) {
        TreeNode* curr = root;

        while (curr) {
            if (curr->left) {
                // Find rightmost node of left subtree
                TreeNode* temp = curr->left;
                while (temp->right) {
                    temp = temp->right;
                }

                // Rewire connections
                temp->right = curr->right;
                curr->right = curr->left;
                curr->left = NULL;
            }

            curr = curr->right;
        }
    }
};
```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(1)

---
