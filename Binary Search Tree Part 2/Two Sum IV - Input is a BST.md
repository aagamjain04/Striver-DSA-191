[Problem Link](https://www.geeksforgeeks.org/problems/implementing-ceil-in-bst/1)
### Problem Statement : 

Given the `root` of a binary search tree and an integer `k`, return `true` _if there exist two elements in the BST such that their sum is equal to_ `k`, _or_ `false` _otherwise_.

**Example 1:**


```
Input : 
        5
       / \
      3   7
     / \  / \
    2  4 6  8



Output : 
k = 9 → (5 + 4)
k = 12 → (4 + 8)
k = 20 → false

```

---

###  Approach 1 :

1. Traverse BST.
2. At each node, check if `(k - node->val)` is already in a hashset.
3. If yes → return true.
4. Otherwise, insert current node value into hashset and continue.
    
#### Code :

```cpp
class Solution {
public:
    bool dfs(Node* root, int k, unordered_set<int>& seen) {
        if (!root) return false;
        if (seen.count(k - root->val)) return true;
        seen.insert(root->val);
        return dfs(root->left, k, seen) || dfs(root->right, k, seen);
    }

    bool findTarget(Node* root, int k) {
        unordered_set<int> seen;
        return dfs(root, k, seen);
    }
};
```


> `Time Complexity` : - O(n)
> 
> `Space Complexity` : O(n)

---

