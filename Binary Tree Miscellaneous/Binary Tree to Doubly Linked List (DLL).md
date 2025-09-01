[Problem Link](https://www.geeksforgeeks.org/problems/binary-tree-to-dll/1)
### Problem Statement : 

Given the root of a Binary Tree (BT), convert it to a **Doubly Linked List (DLL)** in-place using the same node structure.

- The **left and right pointers** of tree nodes should act as `prev` and `next` pointers in the DLL.
- The DLL should be formed using **inorder traversal (Left → Root → Right)**.
- The **leftmost node** in inorder should be the **head** of DLL.
- Return the head of the resulting DLL.

**Note:**
- Extra space is not allowed, except recursion stack (O(h), where h = height of tree).

**Example 1:**

```
Input :
       10
      /  \
     5    20
    / \
   2   8
   
Output : `2 ⇄ 5 ⇄ 8 ⇄ 10 ⇄ 20`
DLL Head = 2

```

`

---


###  Approach 1 :

- **Inorder traversal property:**
    - For BSTs, inorder gives sorted order.
    - But even for general BT, inorder defines the left-to-right sequence in DLL.
- **In-place transformation:**
    - Reuse `left` as `prev` and `right` as `next`.
    - Maintain a `prev` pointer to link the previous node in inorder.
    - The **first visited node** in inorder becomes `head`.

#### Code :

```cpp
class Solution {
  public:
    Node* prev;
    Node* head;
    
    void inorder(Node* root){
        if(root == NULL) return;
        
        // Left
        inorder(root->left);
        
        // Current
        if(prev == NULL) {
            head = root; // first node in inorder
        } else {
            prev->right = root;
            root->left = prev;
        }
        prev = root;
        
        // Right
        inorder(root->right);
    }
  
    Node* bToDLL(Node* root) {
        prev = NULL;
        head = NULL;
        inorder(root);
        return head;
    }
};


```


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(H) for recursion/stack, where H = Height of tree

---

