[Problem Link](https://leetcode.com/problems/clone-graph/description/)
### Problem Statement : 

- You are given a reference to a node in a connected undirected graph.
- Each node contains a value (`val`) and a list of neighbors (`neighbors`).
- Return a **deep copy** of the graph.


### Approach 1 :

1. **Check for null input** - return null if graph is empty
2. **Create clone of starting root** - clone the starting node and add to hashmap
3. **Do a DFS with node and clonedNode** - traverse using original and clone simultaneously
4. **For each neighbor in current node**:
    - **Check if neighbor's value exists in hashmap**
    - **If not found** → create new cloned node and store in hashmap, then recurse
    - **Handle the neighbor** → add the cloned neighbor to current clone's neighbors list
5. **Return the root clone**

#### Code :

``` cpp

class Solution {
public:

    unordered_map<int,Node*> mp;

    void dfs(Node* node,Node* clone){


        for(auto v:node->neighbors){
            if(mp.find(v->val)==mp.end()){
                Node* newNode = new Node(v->val);
                mp[v->val] = newNode;
                dfs(v,newNode);
            }
            clone->neighbors.push_back(mp[v->val]);
        }

    }

    Node* cloneGraph(Node* node) {
        
        if(node == NULL)
        return NULL;

        
        Node* clone = new Node(node->val);
        mp[node->val] = clone;

        dfs(node,clone);

        return clone;
    }
};
```


> `Time Complexity` : O(V+E)
> 
> `Space Complexity` : O(V) for hashmap and recursion stack

---


