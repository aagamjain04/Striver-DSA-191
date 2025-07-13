[Problem Link](https://www.geeksforgeeks.org/problems/m-coloring-problem-1587115620/1)
### Problem Statement : 

You are given an undirected graph consisting of `V` vertices and `E` edges represented by a list `edges[][]`, along with an integer `m`. 
Your task is to determine whether it is possible to color the graph using at most `m` different colors such that no two adjacent vertices share the `same color`. Return `true` if the graph can be colored with at most `m` colors, otherwise return `false`. 

**Note:** The graph is indexed with 0-based indexing.

### Approach 1 :

- This solution uses **backtracking** with **depth-first search (DFS)** to systematically try different color assignments.
- Trying every color from 1 to m with the help of a for a loop.
-  `canColor` function returns true if it is possible to color it with that color i.e none of the adjacent nodes have the same color.  
  

#### Code :

``` cpp
bool canColor(int u,int c,vector<int> adj[],vector<int> &colour){
	
	for(auto v:adj[u]){
		if(colour[v]==c)
		return false;
	}
	return true;
}

bool dfs(int u,vector<int> adj[],int m,int v,vector<int> &colour){
	
	if(u==v){
		return true;
	}
	
	
	for(int i=1;i<=m;i++){
		if(canColor(u,i,adj,colour)){
			colour[u] = i;
			if(dfs(u+1,adj,m,v,colour))
			return true;
			colour[u] = 0;
		}
	}
	return false;
}

bool graphColoring(int v, vector<vector<int>> &edges, int m) {
	
	vector<int> adj[v];
	
	for(auto edge:edges){
		int u = edge[0];
		int v = edge[1];
		adj[u].push_back(v);
		adj[v].push_back(u);
	}
	vector<int> colour(v);
	return dfs(0,adj,m,v,colour);
	

}
```


> `Time Complexity` : O(m^v) where each vertex can be colored in `m` ways
> `Space Complexity` : `O(v)` for recursion stack and color array

---

