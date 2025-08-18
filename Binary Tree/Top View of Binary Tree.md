[Problem Link](https://www.geeksforgeeks.org/problems/bottom-view-of-binary-tree/1)
### Problem Statement : 

You are given a binary tree, and your task is to return its **top view**. The top view of a binary tree is the set of nodes visible when the tree is viewed from the top.

**Note:** 

- Return the nodes from the leftmost node to the rightmost node.
- If two nodes are at the same position (horizontal distance) and are outside the shadow of the tree, consider the leftmost node only.

**Example 1:**

![img](../Images/bottomview.png)

`Horizontal Distances:
- hd(-2): 5
- hd(-1): 8
- hd(0): 20
- hd(1): 22
- hd(2): 25
Output - 5 8 29 22 25
`

---


###  Approach 1 :

-  Assign **Horizontal Distance (HD)** to each node:
    - Root → `0`
    - Left child → `HD - 1`
    - Right child → `HD + 1`
- Perform **Level Order Traversal (BFS)**:
    - For each HD, store the **first node** encountered.
    - Use a **map<int,int>** (HD → node value) to keep ordering.**

#### Code :

```cpp
vector<int> topView(Node *root) {
	// code here
	
	queue<pair<Node*,int>> q;
	q.push({root,0});
	map<int,Node*> mp;
	vector<int> res;
	
	while(!q.empty()){
		Node* curr = q.front().first;
		int d = q.front().second;
		q.pop();
		if(mp.find(d)==mp.end())
		mp[d] = curr;
		
		if(curr->left){
			q.push({curr->left,d-1});
		}
		if(curr->right){
			q.push({curr->right,d+1});
		}
	}
	
	for(auto i:mp){
		res.push_back(i.second->data);
	}
	return res;
}
```


> `Time Complexity` : `O(n log n)`  -> Each N node is processed once and insertion in ordered map is log N.
> 
> `Space Complexity` : O(n)-> map + queue

---
