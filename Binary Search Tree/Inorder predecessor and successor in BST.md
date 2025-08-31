[Problem Link](https://www.geeksforgeeks.org/problems/predecessor-and-successor/1)
### Problem Statement : 

You are given root node of the **BST** and an integer **key**. You need to find the in-order **successor** and **predecessor** of the given key. If either predecessor or successor is not found, then set it to **NULL**.

**Note**:- In an inorder traversal the number just **smaller** than the target is the predecessor and the number just **greater**than the target is the successor.

**Example 1:**


```
Input : 
        50
       /  \
     30    70
    / \   / \
   20 40 60  80

Output : 
For key = 65 →
	Predecessor = 60
	Successor = 70
For key = 30 →
	Predecessor = 20
	Successor = 40
For key = 80 →
	Predecessor = 70
	Successor = NULL

```

---

###  Approach 1 :

- To find predecessor/successor efficiently:
- Traverse the tree from the root.
- If current node’s value is **greater than key**, it can be a **successor** → move **left**.
- If current node’s value is **less than key**, it can be a **predecessor** → move **right**.
- If equal → we check both sides:
    - Max of left subtree = predecessor.
    - Min of right subtree = successor.

#### Code :

```cpp
class Solution {
public:
    Node* findSuccessor(Node* root, int key) {
        Node* successor = NULL;
        while(root) {
            if(root->data > key) {
                successor = root;  // possible successor
                root = root->left;
            } else {
                root = root->right;
            }
        }
        return successor;
    }

    Node* findPredecessor(Node* root, int key) {
        Node* predecessor = NULL;
        while(root) {
            if(root->data < key) {
                predecessor = root;  // possible predecessor
                root = root->right;
            } else {
                root = root->left;
            }
        }
        return predecessor;
    }

    vector<Node*> findPreSuc(Node* root, int key) {
        Node* successor = findSuccessor(root, key);
        Node* predecessor = findPredecessor(root, key);
        return {predecessor, successor};
    }
};
```


> `Time Complexity` : O(h) (Balanced O(logn), SkewedO(n))
> 
> `Space Complexity` : O(H) recursion stack (H = tree height) 

---

