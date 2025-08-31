[Problem Link](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)
### Problem Statement : 

Serialization is the process of converting a data structure or object into a string of bits so that it can be stored/transferred and reconstructed later.  
We need to design an algorithm to **serialize** a binary tree into a string and **deserialize** it back into the original tree structure.

There is no restriction on format—just ensure correctness.

**Example 1:**


```
Input : 
       1
      / \
     2   3
        / \
       4   5



Serialized BFS : 
"1,2,3,#,#,4,5,#,#,#,#,"


```

---

###  Approach 1 :

- Use **Level Order Traversal (BFS)**.
- While serializing:
    - Append node values separated by commas.
    - Append `"#"` for `NULL` children.
- While deserializing:
    - Use a queue to rebuild the tree level by level.
    - For each node, read its left and right child from the serialized stream.
- 

#### Code :

```cpp
class Codec {
public:

    // Serialize: BFS traversal with '#' for NULL
    string serialize(TreeNode* root) {
        string s = "";
        queue<TreeNode*> q;
        if (root) q.push(root);

        while (!q.empty()) {
            TreeNode* curr = q.front();
            q.pop();

            if (curr == NULL) {
                s += "#,";
            } else {
                s += to_string(curr->val) + ",";
                q.push(curr->left);
                q.push(curr->right);
            }
        }
        return s;
    }

    // Deserialize: BFS reconstruction
    TreeNode* deserialize(string data) {
        if (data == "") return NULL;

        stringstream ss(data);
        string str;
        getline(ss, str, ',');
        TreeNode* root = new TreeNode(stoi(str));

        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            TreeNode* curr = q.front();
            q.pop();

            // Left child
            if (getline(ss, str, ',')) {
                if (str != "#") {
                    curr->left = new TreeNode(stoi(str));
                    q.push(curr->left);
                }
            }

            // Right child
            if (getline(ss, str, ',')) {
                if (str != "#") {
                    curr->right = new TreeNode(stoi(str));
                    q.push(curr->right);
                }
            }
        }
        return root;
    }
};

// Usage Example:
// Codec ser, deser;
// TreeNode* ans = deser.deserialize(ser.serialize(root));
```


> `Time Complexity` : - O(N) 
> 
> `Space Complexity` : O(N)

---

