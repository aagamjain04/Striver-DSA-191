[Problem Link](https://leetcode.com/problems/course-schedule-ii/?envType=problem-list-v2&envId=topological-sort)
### Problem Statement : 

Given `numCourses` and `prerequisites[i] = [a, b]` meaning **b → a**,  
return a valid order to complete all courses.  
If impossible (cycle exists), return an empty array.

### Example :

```
numCourses = 4
prerequisites = {{1,0},{2,0},{3,1},{3,2}}

Graph:
0 → 1 → 3
0 → 2 → 3

Possible Order: [0,1,2,3] or [0,2,1,3]

```


---


### Approach 1 :

#### Kahn's Algorithm
- Build adjacency list from prerequisites.
- Maintain **in-degree** for each course.
- Push all courses with `indegree = 0` into queue.
    
- Process queue:
    - Pop node → add to result.
    - Decrease indegree of neighbors.
    - If neighbor’s indegree = 0 → push to queue.
        
- At the end:
    - If `res.size() == numCourses` → valid order.
    - Else → cycle exists → return `{}`.


#### Code :

``` cpp
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> res;
        vector<vector<int>> adj(numCourses);
        vector<int> indeg(numCourses);
        queue<int> q;
    

        for(auto& preReq : prerequisites){
            int u = preReq[1];
            int v = preReq[0];
            indeg[v]++;
            adj[u].push_back(v);
        }

        for(int i=0;i<numCourses;i++){
            if(indeg[i]==0){
                q.push(i);
            }
        }


        while(!q.empty()){
            int u = q.front();
            q.pop();
            res.push_back(u);
            for(auto v:adj[u]){
                indeg[v]--;
                if(indeg[v]==0)
                q.push(v);
            }
        }

        if(res.size() == numCourses)
        return res;
        else return {};
    }
};
```


> `Time Complexity` : O(V+E) (Each vertex visited once, each edge explored once.)
> 
> `Space Complexity` : O(V+E) (adjacency list + indegree + queue) 

---

