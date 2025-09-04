[Problem Link](https://leetcode.com/problems/number-of-islands/description/)
### Problem Statement : 

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return _the number of islands_.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

### Example :

```
Input :
grid = [
  ['1','1','0','0','0'],
  ['1','1','0','0','0'],
  ['0','0','1','0','0'],
  ['0','0','0','1','1']
]

Output : 3

```


---


### Approach 1 :

- DFS Approach
- Iterate over every cell in the grid.
- When encountering `grid[i][j] == '1'`:
    - Increment island count.
    - Run DFS/BFS to mark the entire island as visited (turn to `0`).
- Continue until all cells are processed.


#### Code :

``` cpp
class Solution {
public:

    int dirX[4] = {1,-1,0,0};
    int dirY[4] = {0,0,1,-1};

    void dfs(int x,int y,vector<vector<char>> &grid,int n,int m){

        grid[x][y] = '0';

        for(int k=0;k<4;k++){
            int a = dirX[k]+x;
            int b = dirY[k]+y;
            if(a>=0 && b>=0 && a<n && b<m  && grid[a][b]=='1'){
                dfs(a,b,grid,n,m);
            }
        } 
    }


    int numIslands(vector<vector<char>>& grid) {
        
    
        int n = grid.size();
        int m = grid[0].size();
        
        int count = 0;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){

                if(grid[i][j]=='1'){
                    count++;
                    dfs(i,j,grid,n,m);
                }
            }
        }
        return count;

    }
};
```


> `Time Complexity` : O(m x n) (Each cell processed once)
> 
> `Space Complexity` : O(m x n) (max stack depth) 

---

