[Problem Link](https://leetcode.com/problems/is-graph-bipartite/)
### Problem Statement : 

Given an adjacency list of a graph adj of V no. of vertices having 0 based index. Check whether the graph is bipartite or not.

If we are able to colour a graph with two colours such that no adjacent nodes have the same colour, it is called a bipartite graph.

```
A bipartite graph is a graph which can be coloured using 2 colours such that no adjacent nodes have the same colour. Any linear graph with no cycle is always a bipartite graph. With a cycle, any graph with an even cycle length can also be a bipartite graph. So, any graph with an odd cycle length can never be a bipartite graph.
```

---


### Approach 1 :

- DFS Approach
- The intuition is the brute force of filling colours using any traversal technique, just make sure no two adjacent nodes have the same colour. If at any moment of traversal, we find the adjacent nodes to have the same colour, it means that there is an odd cycle, or it cannot be a bipartite graph.
- Maintain an array `mark` → color assignment of nodes.
    - `0 = uncolored`, `1 = red`, `2 = blue`.
- For each unvisited node:
    - Assign a color (`1`).
    - Run DFS to color all neighbors with alternate color (`3 - color`).
    - If a neighbor already has the same color → **not bipartite**.
- If all nodes can be colored without conflict → graph is bipartite.


#### Code :

``` cpp
class Solution {
public:

    // 0 : Uncolored
    // 1 : Red colour
    // 2 : Blue colour
    bool check(int u,vector<vector<int>>& graph,vector<int> &mark,int colour){
        mark[u] = colour;

        for(auto &v:graph[u]){

            if(mark[v]==0){
                if(!check(v,graph,mark,3-colour))
                return false;
            }else if(mark[v]==colour){
                return false;
            }
        }  

        return true;
    }

    bool isBipartite(vector<vector<int>>& graph) {
        
        int n = graph.size();
        
        vector<int> mark(n);

        for(int i=0;i<n;i++){
            if(mark[i]==0 && !check(i,graph,mark,1))
            return false;
        }
        return true;
         
    }
};
```


> `Time Complexity` : O(V + E) (Each cell processed once)
> 
> `Space Complexity` : O(V) (recursion stack + mark array) 

---

### Approach 2 :

- BFS
- Use an array `mark` → colors (`0 = uncolored`, `1 = red`, `2 = blue`).
- For each unvisited node:
    - Assign `1` as starting color.
    - Push node into queue.
    - While queue not empty:
        - Pop `(u)`.
        - For each neighbor `v`:
            - If uncolored → assign opposite color (`3 - mark[u]`) and push.
            - If already same color as `u` → not bipartite.
                
- If all components can be colored without conflict → bipartite.

#### Code :

```cpp
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> mark(n, 0); // 0 = uncolored, 1 = red, 2 = blue

        for (int i = 0; i < n; i++) {
            if (mark[i] == 0) {
                queue<int> q;
                q.push(i);
                mark[i] = 1; // start with red

                while (!q.empty()) {
                    int u = q.front();
                    q.pop();

                    for (auto v : graph[u]) {
                        if (mark[v] == 0) {
                            mark[v] = 3 - mark[u]; // alternate color
                            q.push(v);
                        } else if (mark[v] == mark[u]) {
                            return false; // same color conflict
                        }
                    }
                }
            }
        }
        return true;
    }
};
```

> `Time Complexity` : O(V + E) (Each cell processed once)
> 
> `Space Complexity` : O(V) (coloring + queue) 

---
