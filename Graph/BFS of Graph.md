[Problem Link](https://www.geeksforgeeks.org/problems/bfs-traversal-of-graph/1)
### Problem Statement : 

You are given a connected **undirected graph** with **V vertices**, represented by a **2D adjacency list** `adj[][]`, where each `adj[i]` contains the list of vertices connected to vertex `i`.

Perform a **Breadth First Search (BFS)** traversal starting from **vertex 0**, visiting vertices from **left to right** as per the given adjacency list, and return the traversal result.

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

BFS starting from 0 → [0, 1, 2, 3, 4]


```


---

### Approach 1 :

- Simple BFS

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


> `Time Complexity` : O(V+E) (Each vertex visited once, each edge explored once.)
> 
> `Space Complexity` : O(V) queue + visited arrray

---


