[Problem Link](https://www.naukri.com/code360/problems/path-in-a-tree_3843990?leftPanelTabValue=PROBLEM)
### Problem Statement : 

You are given a binary tree with ‘N’ number of nodes and a node ‘X’. Your task is to print the path from the root node to the given node ‘X’.

A binary tree is a hierarchical data structure in which each node has at most two children.

**Example 1:**

```
Tree:
       1
      / \
     2   3
    / \
   4   5

Target = 5

Path found:
5 → 2 → 1
After reverse → [1,2,5]

```
`

---


###  Approach 1 :

-  **Base Case**
    - If node is `NULL` → return `false`.
    - If node’s value = target → add to path and return `true`.
- **Recursive Step**
    - Search left and right subtrees.
    - If either returns `true` → current node is also part of the path → add to path.
- **Final Step**
    - Reverse the collected path (because we collect from leaf up to root).**

#### Code :

```cpp
bool go(TreeNode<int>* root, int x, vector<int> &res) {
    if (root == NULL) return false;

    if (root->data == x) {
        res.push_back(root->data);
        return true;
    }

    if (go(root->left, x, res) || go(root->right, x, res)) {
        res.push_back(root->data);
        return true;
    }

    return false;
}

vector<int> pathInATree(TreeNode<int> *root, int x) {
    vector<int> res;
    go(root, x, res);
    reverse(res.begin(), res.end());
    return res;
}

```


> `Time Complexity` : `O(n)`
> 
> `Space Complexity` : O(h) , recursion stack

---

### Approach 2 :
- Iterative
- Perform **BFS/DFS** to find the target node.
- While traversing, maintain a **map of child → parent**.
- Once target is found:
    - Start from target node.
    - Move upwards using the parent map until reaching the root.
    - Reverse to get root → target path.

#### Code :

```cpp
vector<int> pathInATree(TreeNode<int> *root, int x) {
    if (!root) return {};

    unordered_map<TreeNode<int>*, TreeNode<int>*> parent;
    queue<TreeNode<int>*> q;
    q.push(root);
    parent[root] = nullptr;

    TreeNode<int>* target = nullptr;

    // BFS to find target and record parents
    while (!q.empty()) {
        TreeNode<int>* curr = q.front();
        q.pop();

        if (curr->data == x) {
            target = curr;
            break;
        }

        if (curr->left) {
            parent[curr->left] = curr;
            q.push(curr->left);
        }
        if (curr->right) {
            parent[curr->right] = curr;
            q.push(curr->right);
        }
    }

    // Backtrack from target to root
    vector<int> res;
    while (target) {
        res.push_back(target->data);
        target = parent[target];
    }
    reverse(res.begin(), res.end());

    return res;
}

```

> `Time Complexity` : `O(n)`, BFS
> 
> `Space Complexity` : O(N) , map+queue

---
