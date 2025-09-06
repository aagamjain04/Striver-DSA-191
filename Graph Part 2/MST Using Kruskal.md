[Problem Link](https://www.geeksforgeeks.org/problems/implementing-floyd-warshall2042/1)
### Problem Statement : 

- Given a **weighted, undirected, connected graph** with `V` vertices and `E` edges.    
- Each edge is `[u, v, w]` where:
    - `u, v` = vertices
    - `w` = weight of edge
        
- Task: **Find the sum of weights of edges in the Minimum Spanning Tree (MST).**

```
A spanning tree of a graph consists of all nodes of the graph and some of the edges of the graph so that there is a path between any two nodes.

A minimum spanning tree is a spanning tree whose weight is as small as possible.
```

---
### Approach 1 :

- **Kruskal’s Algorithm** (Greedy, edge-based)
- Sort edges by weight.
- Pick edges one by one, ensuring no cycle (use DSU/Union-Find).

#### Code :

``` cpp
class Solution {
  public:
    vector<int> par;
    
    // Find with path compression
    int findPar(int x){
        if(par[x] < 0) return x;
        return par[x] = findPar(par[x]);
    }
    
    // Union by size
    void merge(int a, int b){
        if(par[a] < par[b]) { // a's set is larger
            par[a] += par[b];
            par[b] = a;
        } else {
            par[b] += par[a];
            par[a] = b;
        }
    }
    
    int spanningTree(int V, vector<vector<int>>& edges) {
        par.assign(V, -1);
        
        // Min-heap using negative weights
        priority_queue<tuple<int,int,int>> pq;
        for(auto &e : edges){
            pq.push({-e[2], e[0], e[1]});
        }
        
        int res = 0;
        while(!pq.empty()){
            int w, u, v;
            tie(w, u, v) = pq.top();
            pq.pop();
            w *= -1;
            
            int pu = findPar(u);
            int pv = findPar(v);
            if(pu != pv){
                res += w;
                merge(pu, pv);
            }
        }
        return res;
    }
};
```


> `Time Complexity` : O(E log E + E α(V))` ≈ `O(E log E)
> 
> `Space Complexity` : O(1)

---

