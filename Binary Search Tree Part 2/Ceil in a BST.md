[Problem Link](https://www.geeksforgeeks.org/problems/implementing-ceil-in-bst/1)
### Problem Statement : 

You are given a **root** binary search tree and an integer **x .** Your task is to find the Ceil of **x** in the tree.  
**Note:** Ceil(x) is a number that is either equal to x or is immediately greater than x.  
If Ceil could not be found, return -1.

**Example 1:**


```
Input : 
        8
       / \
      4   12
     / \  / \
    2  6 10 14


Output : 
x = 11 -> Floor = 10
x = 5 -> Floor = 4
X = 1 -> Floor = -1

```

---

###  Approach 1 :

- This is the **Ceil of x in BST** problem.
- **BST Property:** Left child < Parent < Right child.
- While traversing the BST:
    - If `root->val == x` → return `x` (exact match).
    - If `root->val < x` → current node **too small**, move to **right**.
    - If `root->val > x` → this node **could be ceil**, store it and move to **left** for a closer value.

#### Code :

```cpp
int ceilInBST(Node* root, int x) {
    int res = -1;
    while (root) {
        if (root->val == x) {
            return root->val; // exact match
        }
        else if (root->val > x) {
            res = root->val;  // possible ceil
            root = root->left;
        }
        else {
            root = root->right;
        }
    }
    return res;
}
```


> `Time Complexity` : - O(h) where `h` = height of BST (`O(log n)` for balanced tree, `O(n)` worst case).
> 
> `Space Complexity` : O(1) iterative

---

