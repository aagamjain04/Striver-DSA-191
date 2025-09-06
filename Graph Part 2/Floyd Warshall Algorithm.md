[Problem Link](https://www.geeksforgeeks.org/problems/implementing-floyd-warshall2042/1)
### Problem Statement : 

- We are given a weighted **directed graph** represented by an adjacency matrix `dist[][]`:
- `dist[i][j]` → weight of edge `i → j`.
- If no edge exists, `dist[i][j] = 1e8` (infinity).
- Graph may contain **negative weights** but **no negative weight cycles**.
- Task: Compute the **shortest distance between all pairs of nodes (i, j)**.

---
### Approach 1 :

- Use **Dynamic Programming** with the **Floyd-Warshall algorithm**:
- Consider each node `k` as an **intermediate node**.
- Try to relax/update all paths `i → j` via `k`.
    
    ```
    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    ```    
    
- We do this for every intermediate node `k` (from 0 to n-1).

#### Code :

``` cpp
class Solution {
  public:
    void floydWarshall(vector<vector<int>> &dist) {
        
        int n = dist.size();
        for(int k=0;k<n;k++){
            for(int i=0;i<n;i++){
                for(int j=0;j<n;j++){
                    
                    if(i==k || j==k || dist[i][k]==1e8 || dist[k][j]==1e8)
                    continue;
                    dist[i][j] = min(dist[i][j],dist[i][k] + dist[k][j]);
                }
            }
        }
        
        
    }
};
```


> `Time Complexity` : O(n³) -> Triple nested loop.
> 
> `Space Complexity` : O(1)

---

