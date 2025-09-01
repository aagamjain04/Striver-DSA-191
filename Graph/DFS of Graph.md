[Problem Link](https://www.geeksforgeeks.org/problems/depth-first-traversal-for-a-graph/1)
### Problem Statement : 

You are given a connected undirected graph with **V vertices** represented by a 2D adjacency list `adj[][]`, where each `adj[i]` contains the list of vertices connected to vertex `i`.

Perform a **Depth First Search (DFS)** traversal starting from **vertex 0**, visiting vertices from **left to right** as per the adjacency list, and return a list containing the DFS traversal.

## Examples

**Example 1: 
```
V = 5
adj = [
  [1, 2, 3],
  [0],
  [0, 4],
  [0],
  [2]
]

DFS starting from 0 → [0, 1, 2, 4, 3]

```


---

### Approach 1 :

- Simple DFS

#### Code :

``` cpp
void dfs(int u,vector<int> &res,vector<vector<int>>& adj,vector<int> &vis){
	
	if(vis[u])
	return;
	vis[u] = 1;
	res.push_back(u);
	for(auto &v:adj[u]){
		dfs(v,res,adj,vis);
	}
}

vector<int> dfs(vector<vector<int>>& adj) {
	
	int n =adj.size();
	vector<int> res;
	vector<int> vis(n);
	dfs(0,res,adj,vis);
	return res;
	
}
```


> `Time Complexity` : O(V+E)
> 
> `Space Complexity` : O(V) recursion stack + visited arrray

---


