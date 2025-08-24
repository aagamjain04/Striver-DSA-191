[Problem Link](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)
### Problem Statement : 

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**

```
Input : 
inorder   = [9, 3, 15, 20, 7]  
postorder = [9, 15, 7, 20, 3] 

Output :
        3
       / \
      9   20
         /  \
        15   7


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
    unordered_map<int,int> mp;

    TreeNode* builder(int inleft, int inright, vector<int>& inorder,
                      int postleft, int postright, vector<int>& postorder) {
        if (inleft > inright || postleft > postright)
            return NULL;

        // Root is last element in postorder
        TreeNode* root = new TreeNode(postorder[postright]);

        // Index of root in inorder
        int k = mp[postorder[postright]];
        int leftSize = k - inleft;

        // Build left and right subtrees
        root->left = builder(inleft, k - 1, inorder, postleft, postleft + leftSize - 1, postorder);
        root->right = builder(k + 1, inright, inorder, postleft + leftSize, postright - 1, postorder);

        return root;
    }

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        int n = inorder.size();
        for (int i = 0; i < n; i++) {
            mp[inorder[i]] = i; // store inorder indices
        }
        return builder(0, n - 1, inorder, 0, n - 1, postorder);
    }
};

```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(n) hashmap + recursion stack

---

