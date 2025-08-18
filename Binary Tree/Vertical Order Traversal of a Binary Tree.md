[Problem Link](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/description/)
### Problem Statement : 

We need to return the **vertical order traversal** of a binary tree:

- Root starts at position `(0,0)` → `(col,row)`.
- Left child → `(col-1,row+1)`
- Right child → `(col+1,row+1)`
- Group nodes by **column (col)**, leftmost to rightmost.
- Within a column:
    - Sort by **row** (top to bottom).
    - If multiple nodes share same `(row,col)`, sort by **node value**.

**Example 1:**

```
        3
       / \
      9   20
         /  \
        15   7

Traversal:
(0,0) → 3
(-1,1) → 9
(1,1) → 20
(0,2) → 15
(2,2) → 7

mp = {
 -1 : { 1 : {9} },
  0 : { 0 : {3}, 2 : {15} },
  1 : { 1 : {20} },
  2 : { 2 : {7} }
}

Result = [[9], [3,15], [20], [7]]

```
`

---


###  Approach 1 :

-  **Data Structures Used**
    - `queue<tuple<int,int,TreeNode*>> q;` → BFS traversal with `(col,row,node)`.
    - `map<int, map<int, multiset<int>>> mp;` → stores nodes by `(col,row)`.
- **BFS Traversal**
    - Start with `(0,0,root)` in queue.
    - For each node:
        - Add `node->val` into `mp[col][row]`.
        - Push left child as `(col-1,row+1)`.
        - Push right child as `(col+1,row+1)`.
- **Result Construction**
    - Iterate `mp` (columns sorted automatically).
    - For each column:
        - Traverse rows in increasing order.
        - Append values from `multiset` into result.

#### Code :

```cpp
vector<vector<int>> verticalTraversal(TreeNode* root) {
	
	vector<vector<int>> res;

	queue<tuple<int,int,TreeNode*>> q;
	map<int,map<int,multiset<int>>> mp;

	if(root)
	q.push({0,0,root});

	while(!q.empty()){
		int d,lvl;
		TreeNode* curr;
		tie(d,lvl,curr) = q.front();
		q.pop();
		mp[d][lvl].insert(curr->val);

		if(curr->left){
			q.push({d-1,lvl+1,curr->left});
		}
		if(curr->right){
			q.push({d+1,lvl+1,curr->right});
		}
	
	}

	for(auto& i:mp){
		vector<int> temp;
		for(auto &j : i.second){
			temp.insert(temp.end(),j.second.begin(),j.second.end());
		}
		res.push_back(temp);
	}

	return res;
}
```


> `Time Complexity` : `O(n log n)`  -> bfs O(N) + O(log k) per element for mulitset
> 
> `Space Complexity` : O(n)-> map + queue

---
