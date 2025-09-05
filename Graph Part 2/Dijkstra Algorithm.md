[Problem Link](https://www.geeksforgeeks.org/problems/implementing-dijkstra-set-1-adjacency-matrix/1)
### Problem Statement : 

You are given:

- Given an **undirected, weighted graph** with `V` vertices (0 to V-1) and `E` edges.
- Each edge is represented as `edges[i] = [u, v, w]`, meaning:
    - There is an edge between `u` and `v` with weight `w`.
- Graph is **connected** and has **no negative weight edges**.
#### Task:

- Find the **shortest distance** from a given source `src` to every vertex.
- Return a vector of distances where `dist[i] = shortest path from src → i`.

---
### Approach 1 :

- #### Dijkstra’s Algorithm
- Efficient for graphs with **non-negative weights**.
- Uses a **min-heap (priority queue)** to greedily select the closest unvisited node.
- Works for **undirected graphs** too (just add both directions in adjacency list).

#### Steps:
1. Build adjacency list from `edges` (since input is in edge list form).
    - Because the graph is undirected: add both `(u → v, w)` and `(v → u, w)`.
- Initialize `dist[]` with `∞` (or large value).
    - Set `dist[src] = 0`.
- Use a **priority queue** `(distance, node)` starting with `(0, src)`.
- While PQ is not empty:
    - Extract the node with minimum distance.
    - Relax all its neighbors:
    
    ```
    if dist[u] + w < dist[v]:
    dist[v] = dist[u] + w
    push (dist[v], v) into PQ
    ```    
- Return `dist[]`.

#### Code :

``` cpp
class Solution {
  public:
    vector<int> dijkstra(int V, vector<vector<int>> &edges, int src) {
       
       vector<int> dis(V,1e8);
       
       vector<vector<pair<int,int>>> adj(V);
       
       for(auto& e:edges){
           int u = e[0];
           int v = e[1];
           int w = e[2];
           adj[u].push_back({v,w});
           adj[v].push_back({u,w});
       }
       
       priority_queue<pair<int,int>> pq;
       pq.push({0,src});
       dis[src] = 0;
       
       while(!pq.empty()){
           
           int curr = pq.top().second;
           int wt = -pq.top().first;
           pq.pop();
           if(dis[curr]<wt)continue; // stale entry
           
           for(auto &e : adj[curr]){
               
               int w = e.second;
               int v = e.first;
               if(dis[v]>(w+dis[curr])){
                   dis[v] = w+dis[curr];
                   pq.push({-dis[v],v});
               }
           }
           
       }
       
       return dis;
        
    }
};
```


> `Time Complexity` : O((V + E) log V) ->Each edge is processed once, PQ operations cost log V
> 
> `Space Complexity` : O(V+E) -> For adj list + dist array

---

