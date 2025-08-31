[Problem Link](https://www.geeksforgeeks.org/problems/floor-in-bst/1)
### Problem Statement : 

You are given a BST(Binary Search Tree) with **n** number of nodes and value **x**. your task is to find the greatest value node of the BST which is smaller than or equal to x.  
**Note:** when x is smaller than the smallest node of BST then returns -1.

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

- This is the **Floor of x in BST** problem.
- **BST Property**: Left child < Parent < Right child.
- While traversing the BST:
    - If `root->val == x`, that’s the exact floor → return it.
    - If `root->val < x`, then this node **could be the floor** → move to **right** to check for a closer (but still ≤ x) value.
    - If `root->val > x`, this node is **too large** → move to **left**.

#### Code :

```cpp
int floorInBST(Node* root, int x) {
    int res = -1;
    while (root) {
        if (root->val == x) {
            return root->val; // exact match
        }
        else if (root->val < x) {
            res = root->val;  // possible floor
            root = root->right;
        }
        else {
            root = root->left;
        }
    }
    return res;
}
```


> `Time Complexity` : - O(h) where `h` = height of BST (`O(log n)` for balanced tree, `O(n)` worst case).
> 
> `Space Complexity` : O(1) iterative

---

