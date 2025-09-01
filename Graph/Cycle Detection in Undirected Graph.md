[Problem Link](https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1)
### Problem Statement : 

You are given an **undirected graph** with `V` vertices and `E` edges represented by a 2D vector `edges[][]`, where each entry `edges[i] = [u, v]` denotes an edge between vertices `u` and `v`.  
Determine whether the graph contains a **cycle** or not.  
The graph may consist of multiple components.

## Examples

**Example 1: 
```
Input: V = 5, edges = [[0,1], [1,2], [2,0], [3,4]]
Graph:
  0 - 1
   \  |
     2     3 - 4

Cycle exists: YES (0-1-2-0)

```


---

### Approach 1 :

- DFS
- Traverse the graph using DFS.
- For each visited node, check all its neighbors.
- If a neighbor is already visited **and is not the parent**, then a cycle exists.
- Run DFS for **all components**.

#### Code :

``` cpp
class Solution {
  public:
  
    bool dfs(int u,int p,vector<int> &vis,vector<vector<int>> &adj){
        
        vis[u] = 1;
        for(auto &v : adj[u]){
            
            if(vis[v]==0){
                if(dfs(v,u,vis,adj)) return true;
            }else if(v!=p){
                return true;
            }
            
        }
        return false;
    }
  
  
    bool isCycle(int V, vector<vector<int>>& edges) {
        
        vector<vector<int>> adj(V);
        
        for(auto& i:edges){
            int u = i[0];
            int v = i[1];
            adj[u].push_back(v);
            adj[v].push_back(u);
        }
        
        vector<int> vis(V);
        
        for(int i=0;i<V;i++){
            if(vis[i]==0){
                 if(dfs(i,-1,vis,adj))
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


- DSU
- Initially, each vertex is its own parent.
- For each edge `(u, v)`:
    - Find the parent of `u` and `v`.
    - If they are already in the same set → cycle detected.
    - Otherwise, union the two sets.
        
- Efficient with path compression + union by rank.


#### Code :

``` cpp
class Solution {
  public:
  
    vector<int> par;

    int findPar(int x){
        
        if(par[x]<0){
            return x;
        }
        return par[x] = findPar(par[x]);
        
    }
    
    void merge(int a,int b){
        
        if(par[a]<par[b]){
            par[a]+=par[b];
            par[b] = a;
        }else{
            par[a]+=par[b];
            par[a] = b;
        }
        
    }
  
    bool isCycle(int V, vector<vector<int>>& edges) {
              
        par.assign(V,-1);
        
        for(auto &e:edges){
            
            int u = e[0];
            int v = e[1];
            
            int pu = findPar(u);
            int pv = findPar(v);
            
            if(pu==pv)
            return true;
            
            merge(pu,pv);
          
        }
        
        return false;
        
    }
};
```

> `Time Complexity` : `O(E * α(V))` (almost linear, inverse Ackermann function).
> 
> `Space Complexity` : O(V)


---

```
α(V) is the inverse Ackermann function, which grows so slowly that for any realistic graph, it’s ≤ 4–5. Hence, Union-Find is practically linear time in edges.
```

## 1. What is **α(V)?**

- `α(V)` is the **inverse Ackermann function**.
- It comes from the amortized time complexity of **Union-Find** (Disjoint Set Union) when optimized with:
    - **Path compression** (flattening tree during `find`), and
    - **Union by rank/size** (attach smaller tree under larger one).

- The Ackermann function grows _very_ fast. Its inverse grows _extremely slowly_.    
- For all practical input sizes (even up to the size of the universe),  
    **α(V) ≤ 4 or 5.**  
    So, in practice, we treat it as **almost constant time**.
    
---

## 2. Why does DSU give this complexity?

- For each edge `(u, v)`:
    1. We perform **two `find` operations** (to get parents of `u` and `v`).
    2. We may perform one **`union` operation**.
- Each `find` and `union` takes `O(α(V))` with path compression + union by rank.
    
So, total = `E` edges × `O(α(V))` = **`O(E * α(V))`.**

---

## 3. Intuition

- Without optimization: `find` could take `O(V)` in the worst case (linked-list-like tree).
- With **path compression + union by rank**, trees flatten so much that future operations are _almost constant_.
- That’s why it’s called **amortized complexity**.


---
