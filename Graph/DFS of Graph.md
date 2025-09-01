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
vector<int> bfsOfGraph(int V, vector<int> adj[]) {
	vector<int> visited(V, 0), ans;
	queue<int> q;

	q.push(0);
	visited[0] = 1;

	while (!q.empty()) {
		int u = q.front();
		q.pop();
		ans.push_back(u);

		for (int neigh : adj[u]) {
			if (!visited[neigh]) {
				q.push(neigh);
				visited[neigh] = 1; // mark visited when enqueued
			}
		}
	}
	return ans;
}

```


> `Time Complexity` : O(V+E) (Each vertex visited once, each edge explored once.)
> 
> `Space Complexity` : O(V) recursion stack + visited arrray

---


