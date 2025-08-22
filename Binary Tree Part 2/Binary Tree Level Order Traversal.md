[Problem Link](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)
### Problem Statement : 

Given the `root` of a binary tree, return _the level order traversal of its nodes' values_. (i.e., from left to right, level by level).

**Example 1:**

```
        3
       / \
      9   20
         /  \
        15   7
output : 

[
  [3],
  [9,20],
  [15,7]
]

```
`

---


###  Approach 1 :

- Use a **queue** for Breadth-First Search (BFS).
- Push the root node into the queue.
- For each level:
    - Process all nodes in the queue (current level size).
    - Store their values.
    - Push their children into the queue.

#### Code :

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
	

	queue<TreeNode*> q;
	if(root)
	q.push(root);

	vector<vector<int>> res;

	while(!q.empty()){
		
		int sz = q.size();

		vector<int> temp;
		for(int i=0;i<sz;i++){
			TreeNode* curr= q.front();
			q.pop();
			temp.push_back(curr->val);

			if(curr->left)
			q.push(curr->left);

			if(curr->right)
			q.push(curr->right);  
		}
		res.push_back(temp);
	
	}

	return res;

}

```


> `Time Complexity` : `O(n)`
> 
> `Space Complexity` : O(n) 

---

