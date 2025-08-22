[Problem Link](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/)
### Problem Statement : 

Given the `root` of a binary tree, return _the zigzag level order traversal of its nodes' values_. (i.e., from left to right, then right to left for the next level and alternate between).

**Example 1:**

```
       3
      / \
     9   20
        /  \
       15   7

[
  [3],
  [20, 9],
  [15, 7]
]


```
`

---


###  Approach 1 :

- BFS Traversal with Queue
- Use a **queue** to perform normal level order traversal.
- Keep a **level counter (`count`)**:
    - Odd levels → push as is.
    - Even levels → reverse before adding to result.

#### Code :

```cpp
vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    queue<TreeNode*> q;
    if(root) q.push(root);

    vector<vector<int>> res;
    int count = 0;

    while(!q.empty()){
        int sz = q.size();
        vector<int> temp;
        
        for(int i=0;i<sz;i++){
            TreeNode* curr = q.front();
            q.pop();
            temp.push_back(curr->val);

            if(curr->left) q.push(curr->left);
            if(curr->right) q.push(curr->right);
        }
        
        count++;
        if(count % 2 == 0) // even level → reverse
            reverse(temp.begin(), temp.end());

        res.push_back(temp);
    }
    return res;
}


```


> `Time Complexity` : O(n) + O(n) (due to reversal)
> 
> `Space Complexity` : O(N) for queue 

---

