[Problem Link](https://www.geeksforgeeks.org/problems/rat-in-a-maze-problem/1&selectedLang=python3)
### Problem Statement : 

Consider a rat placed at **(0, 0)** in a square matrix of order **N * N**. It has to reach the destination at **(N - 1, N - 1)**. Find all possible paths that the rat can take to reach from source to destination. The directions in which the rat can move are **'U'(up)**, **'D'(down)**, **'L' (left)**, **'R' (right)**. Value 0 at a cell in the matrix represents that it is blocked and the rat cannot move to it while value 1 at a cell in the matrix represents that rat can travel through it.

**Note**: In a path, no cell can be visited more than one time.

Print the answer in lexicographical(sorted) order
## Examples

**Example 1:**

```
Input:
N = 4
m[][] = {{1, 0, 0, 0},
        {1, 1, 0, 1}, 
        {1, 1, 0, 0},
        {0, 1, 1, 1}}
Output: DDRDRR DRDDRR


```

### Approach 1 :

- Use recursion and backtracking
- Explore all possible paths systematically
- Build path string while traversing
- Mark/unmark cells to avoid revisiting
#### Code :

``` cpp
class Solution {
  public:
  
    int x[4] = {1, -1, 0, 0};
    int y[4] = {0, 0, 1, -1};
    string dirString = "DURL";
    
    void start(int i, int j, int n, string &dir, vector<string> &res, vector<vector<int>>& maze) {
        if (i == n - 1 && j == n - 1) {
            res.push_back(dir);
            return;
        }
        
        for (int k = 0; k < 4; k++) {
            int a = i + x[k];
            int b = j + y[k];
            if (a >= 0 && b >= 0 && a < n && b < n && maze[a][b] == 1) {
                maze[a][b] = 0;           // Mark as visited
                dir.push_back(dirString[k]);
                start(a, b, n, dir, res, maze);
                dir.pop_back();           // Backtrack direction
                maze[a][b] = 1;           // Backtrack cell
            }
        }
    }
  
    vector<string> ratInMaze(vector<vector<int>>& maze) {
        int n = maze.size();
        vector<string> res;
        if (maze[0][0] == 0) return res; // If start is blocked

        string dir = "";
        maze[0][0] = 0;  // Mark start as visited
        start(0, 0, n, dir, res, maze);
        sort(res.begin(), res.end());
        return res;
    }
};

```


> `Time Complexity` : `O(4^(N^2))` -> each cell can lead to 4 directions
> 
> `Space Complexity` : `O(N^2)` in the recursion stack (max depth)

---

