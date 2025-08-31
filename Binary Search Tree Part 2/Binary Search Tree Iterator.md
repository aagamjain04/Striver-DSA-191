[Problem Link](https://leetcode.com/problems/binary-search-tree-iterator/description/)
### Problem Statement : 

Design a class `BSTIterator` that iterates over a **Binary Search Tree (BST)** in **inorder** (sorted order).

- `BSTIterator(TreeNode* root)` → initializes iterator with BST root.
- `int next()` → returns next element in inorder sequence.
- `bool hasNext()` → returns true if more elements exist.

**Example 1:**


```
Input : 
        7
       / \
      3   15
          / \
         9  20




Output : 
Inorder: [3, 7, 9, 15, 20]
Calls:
	next() → 3
	next() → 7
	hasNext() → true
	next() → 9

```

---
###  Approach 1 :

- Use a stack to simulate recursion.
- Push **all left children** first.
- `next()` pops from stack, then pushes its right subtree’s left path.
- Ensures `O(h)` space, `O(1)` amortized per operation.

#### Code :

``` cpp
class BSTIterator {
public:

    stack<TreeNode*> st;

    void pushLeft(TreeNode* root){

        while(root){
            st.push(root);
            root = root->left;
        }
    }

    BSTIterator(TreeNode* root) {
        pushLeft(root);
    }
    
    int next() {
        
        TreeNode* curr = st.top();
        st.pop();
        int res = curr->val;
        if(curr->right){
            pushLeft(curr->right);
        }
        return res;
    }
    
    bool hasNext() {
        return !st.empty();
    }
};
```

> `Time Complexity` : - O(1)
> 
> `Space Complexity` : O(h)

---

###  Approach 2 :

- Morris Traversal
- Uses **threaded binary tree** (temporarily modifies right pointers).
- Achieves `O(1)` space, but modifies tree structure (needs restoration).
    
#### Code :

```cpp

class BSTIterator {
public:

    TreeNode* curr;

    BSTIterator(TreeNode* root) {
        curr = root;
    }
    
    int next() {
        int res = -1;
        while(res==-1){
            
            if(curr->left==NULL){
                res = curr->val;
                curr = curr->right;
            }else{

                TreeNode* leftPart = curr->left;
                while(leftPart->right && leftPart->right!=curr){
                    leftPart = leftPart->right;
                }

                if(leftPart->right==curr){
                    leftPart->right = NULL;
                    res = curr->val;
                    curr = curr->right;
                }else{
                    leftPart->right = curr;
                    curr = curr->left;
                }

            }
        }
        return res;
    }
    
    bool hasNext() {
        return (curr!=NULL);
    }
};

```


> `Time Complexity` : - O(1)
> 
> `Space Complexity` : O(1)

---

