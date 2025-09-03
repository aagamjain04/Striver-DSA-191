[Problem Link](https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1)
### Problem Statement : 

Given a directed graph with `V` vertices and `E` edges, check if the graph contains any cycle.  
The graph is represented as a 2D vector `edges[][]`, where each entry `edges[i] = [u, v]` denotes an edge from vertex `u → v`.

---


### Approach 1 :

- DFS
- Traverse the graph using DFS.
- A cycle exists if there is a path starting from a vertex that leads back to itself.
- Use **DFS with 3 states** in `vis[]`:
    - `0` → Not visited
    - `1` → Visiting (currently in recursion stack)
    - `2` → Fully processed
        
- During DFS:
    - If we encounter a node with `vis[v] == 1` → **Cycle detected** (back edge).
    - After exploring all neighbors, mark node as `2` (processed).
- Run DFS for all nodes (to handle disconnected components).

#### Code :

``` cpp
class Solution {
  public:
  
    bool dfs(int u,vector<vector<int>> &adj,vector<int> &vis){
        
        vis[u]=1;
        
        for(auto& v:adj[u]){
            if(vis[v]==0){
                if(dfs(v,adj,vis))
                return true;
            }else if(vis[v]==1){
                return true;
            }
        }
        vis[u] = 2;
        return false;
    }
  
  
    bool isCyclic(int V, vector<vector<int>> &edges) {
        // code here
        // dfs
        
        vector<int> vis(V,0);
        vector<vector<int>> adj(V);
        for(auto e:edges){
            int u = e[0];
            int v = e[1];
            adj[u].push_back(v);
        }
        
        for(int i=0;i<V;i++){
            if(vis[i]==0){
                if(dfs(i,adj,vis))
                return true;
            }
        }
        return false;
        
    }
};
```


> `Time Complexity` : O(V+E) (Each vertex visited once, each edge explored once.)
> 
> `Space Complexity` : O(V) visited array + recursion stack

---


### Approach 2 :

- Based on **Topological Sorting**.
- Steps:
    1. Compute **in-degree** of all vertices.
    2. Push nodes with `in-degree = 0` into a queue.
    3. Repeatedly remove nodes from queue and reduce in-degree of their neighbors.
    4. Count processed nodes.
        
- If `processed != V` → cycle exists (because some nodes were part of a cycle and never reached `in-degree = 0`).


#### Code :

``` cpp
class Solution {
  public:
  
    bool isCyclic(int V, vector<vector<int>> &edges) {
        // code here
        // bfs
        
        vector<int> ind(V,0);
        vector<vector<int>> adj(V);
        for(auto e:edges){
            int u = e[0];
            int v = e[1];
            adj[u].push_back(v);
            ind[v]++;
        }
        int processed = 0;
        queue<int> q;
        for(int i=0;i<V;i++){
            if(ind[i]==0)
            q.push(i);
        }
        
        while(!q.empty()){
            int u = q.front();
            q.pop();
            processed++;
            for(auto& v:adj[u])
            {
                ind[v]--;
                if(ind[v]==0){
                    q.push(v);
                }
            }
        }
        
        return processed!=V;
                   
    }
};
```

> `Time Complexity` : O(V+E) (each edge and node processed once).
> 
> `Space Complexity` : O(V) (queue+ in-degree array)


---
