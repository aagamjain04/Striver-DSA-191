[Problem Link](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)
### Problem Statement : 

Given the `root` of a binary search tree, and an integer `k`, return _the_ `kth` _smallest value (**1-indexed**) of all the values of the nodes in the tree_.

**Example 1:**


```
Input : 
        5
       / \
      3   7
     / \  / \
    2  4 6  8



Output : 
Inorder: [2, 3, 4, 5, 6, 7, 8]
k=3 → 4
k=5 → 6

```

---

###  Approach 1 :

- Perform an inorder traversal
- Maintain a counter while traversing.
- Stop when the counter reaches `k`.

#### Code :

```cpp
class Solution {
public:
    int count = 0;
    int ans = -1;

    void inorder(Node* root, int k) {
        if (!root || ans != -1) return;

        inorder(root->left, k);
        count++;
        if (count == k) {
            ans = root->val;
            return;
        }
        inorder(root->right, k);
    }

    int kthSmallest(Node* root, int k) {
        inorder(root, k);
        return ans;
    }
};
```


> `Time Complexity` : - O(k)
> 
> `Space Complexity` : O(h) recursion stack

---
## Follow-up: BST Modified Frequently

If the BST is **updated often** (insert/delete) and we need `kth smallest` frequently, the above solution is inefficient.

### Optimized Idea: Augmented BST with Subtree Size

- Maintain an **extra field** in each node: `size` = number of nodes in its subtree.
- During insertion/deletion, update `size` accordingly.
- To find `kth` smallest:
    1. Let `leftSize = size(root->left)`.
    2. If `k == leftSize + 1` → return `root->val`.
    3. If `k ≤ leftSize` → go left.
    4. Else → go right with `k = k - leftSize - 1`.
        
- **Time:** `O(h)` per query (balanced tree → `O(log n)`).
    
- **Space:** `O(1)` extra per node (just one `size` field).
    

This is essentially an **Order Statistics Tree** (used in `std::set` with policy-based data structures in C++).

---

