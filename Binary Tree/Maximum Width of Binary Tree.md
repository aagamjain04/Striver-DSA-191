[Problem Link](https://leetcode.com/problems/maximum-width-of-binary-tree/description)
### Problem Statement : 

Given the root of a binary tree, return the maximum width of the given tree.

- **Width of a level** = distance between the leftmost and rightmost non-null nodes at that level (considering nulls in between as in a complete binary tree).
    
- Maximum width = maximum of all level widths.

**Example 1:**

```
          1
        /   \
       3     2
      / \     \
     5   3     9

Level 0: width = 1  (nodes: 1)  
Level 1: width = 2  (nodes: 3,2)  
Level 2: width = 4  (nodes: 5,3,null,9)  
Answer = 4

```
`

---


###  Approach 1 :

- Treat the tree as an **implicit complete binary tree** using array indexing.
    - Root → index 0
    - Left child of index `i` → `2*i+1`
    - Right child of index `i` → `2*i+2`
- For each level:
    - Track **leftmost index** and **rightmost index**
    - Width = `right - left + 1`
- Take maximum over all levels.

#### Code :

```cpp
int widthOfBinaryTree(TreeNode* root) {
    if(!root) return 0;

    queue<pair<long long, TreeNode*>> q;
    q.push({0, root});
    int maxW = 0;

    while(!q.empty()) {
        int sz = q.size();
        long long left = q.front().first, right = left; // normalize indices
        for(int i=0;i<sz;i++) {
            auto [idx, node] = q.front(); q.pop();
            right = idx;
            if(node->left) q.push({2*(idx-left)+1, node->left});
            if(node->right) q.push({2*(idx-left)+2, node->right});
        }
        maxW = max(maxW, (int)(right - left + 1));
    }
    return maxW;
}

```


> `Time Complexity` : `O(n)`
> 
> `Space Complexity` : O(n) 

---

