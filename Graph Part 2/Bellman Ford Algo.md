[Problem Link](https://www.geeksforgeeks.org/problems/distance-from-the-source-bellman-ford-algorithm/1)
### Problem Statement : 

You are given:

- A weighted directed graph with **V vertices (0 to V-1)** and **E edges**.
- Each edge is represented as `edges[i] = [u, v, w]` meaning:
    - There is an edge from `u → v` with weight `w`.
- A source vertex `src`.
#### Task:

- Find shortest distances from `src` to every vertex.
- If a vertex is **unreachable**, mark distance as `1e8`.
- If the graph contains a **negative weight cycle**, return `[-1]`.

---

### Approach 1 :

- #### Bellman-Ford Algorithm
- Used for **single-source shortest path** in graphs with **negative weights**.
- Unlike Dijkstra’s, Bellman-Ford works even with negative edges.
- Detects **negative weight cycles**.

#### Steps:
1. **Initialization**:
    - Distance to all vertices = `∞` (or large value).
    - Distance to source = `0`.
2. **Relax edges V-1 times**:
    - For each edge `(u, v, w)` → update if:
        `if dist[u] + w < dist[v]:     dist[v] = dist[u] + w`
3. **Check for negative cycle**
    - Perform one more relaxation round.
    - If any distance can still be updated → **negative cycle exists** → return `[-1]`.
4. **Final step**:
    - Replace unreachable nodes (`∞`) with `108`.

#### Code :

``` cpp
class Solution {
  public:
    vector<int> bellmanFord(int V, vector<vector<int>>& edges, int src) {
        
        vector<int> dis(V,1e8);
        dis[src] = 0;
        
        
        for(int i=1;i<=V;i++){
                for(auto &e : edges){
                int u = e[0];
                int v = e[1];
                int w = e[2];
                
                if(dis[u]!=1e8 && dis[v] > dis[u]+w){
                    if(i==V){ // one more relaxation round
                        return {-1};
                    }
                    dis[v] = dis[u] + w;
                }
            }
        }
        return dis;
        
    }
};

```


> `Time Complexity` : O(V * E) -> Because we relax all edges `V-1` times. 
> 
> `Space Complexity` : O(V) -> For distance array 

---

