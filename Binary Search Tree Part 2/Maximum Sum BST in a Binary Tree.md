[Problem Link](https://leetcode.com/problems/maximum-sum-bst-in-binary-tree/description/)
### Problem Statement : 

Design a class `BSTIterator` that iterates over a **Binary Search Tree (BST)** in **inorder** (sorted order).

- `BSTIterator(TreeNode* root)` → initializes iterator with BST root.
- `int next()` → returns next element in inorder sequence.
- `bool hasNext()` → returns true if more elements exist.

**Example 1:**


```
Input : 
        1
       / \
      4   3
     / \   \
    2   4   5


Output : 
Subtree rooted at `4 (left)` is a BST → sum = 10
Subtree rooted at `3` is a BST → sum = 8
Whole tree is not BST.
Answer = 10

```

---
###  Approach 1 :

### Step 1: Define helper structure
For each node, return:
- `isBST`: whether subtree is BST.
- `minVal`: min value in subtree.
- `maxVal`: max value in subtree.
- `sum`: sum of subtree values (if BST).
### Step 2: Recurrence
For a node to be BST:
- Left subtree is BST.
- Right subtree is BST.
- `left.maxVal < root->val < right.minVal`.
    
If BST →

```
sum = left.sum + right.sum + root->val
minVal = min(root->val, left.minVal)
maxVal = max(root->val, right.maxVal)
update global maxSum

```

#### Code :

``` cpp
int ans = 0;
class prop {
    public:
    bool bst; // to check if tree is bsr
    int mx;   // max value in current tree
    int mi;   // min value in current tree
    int ms;   // current maximum sum

    prop(){
        bst = true;
        mx = INT_MIN;
        mi = INT_MAX;
        ms = 0;
    }

};
class Solution {
public:


    prop go(TreeNode* root){
        if(root==NULL){
            return prop();
        }

        prop p;
        prop pl = go(root->left);
        prop pr = go(root->right);

        // if substree including this is a bst
        if(pl.bst && pr.bst && pl.mx<root->val && pr.mi>root->val){
            p.bst = true;
            p.ms = pl.ms + pr.ms + root->val;
            p.mx = max(root->val,pr.mx);
            p.mi = min(root->val,pl.mi);
        }else{
            p.bst = false;
            p.ms = max(pl.ms,pr.ms);
        }
        ans = max(ans,p.ms);
        return p;

    }


    int maxSumBST(TreeNode* root) {

        ans = 0;
        go(root);
        return ans;
        
    }
};
```

> `Time Complexity` : - O(n)
> 
> `Space Complexity` : O(h)

---
