[Problem Link](https://leetcode.com/problems/minimum-path-sum/description/)
### Problem Statement : 

You are given an `m x n` grid filled with non-negative numbers. Find a path from the **top-left** to the **bottom-right**, such that the **sum of all numbers along the path is minimized**.

- You can only move **down** or **right** at any step.

Example :

```
Input :
grid = [
  [1, 3, 1],
  [1, 5, 1],
  [4, 2, 1]
]

Output: 7
Final DP Table:
[
  [1, 4, 5],
  [2, 7, 6],
  [6, 8, 7]
]
```


---

### Approach 1 :

- At each cell `(i, j)`, the minimum path sum to reach it depends on:
- The path from the **top** `(i-1, j)`
- The path from the **left** `(i, j-1)`
    
So:

``` lua
dp[i][j]=grid[i][j]+min⁡(dp[i−1][j],dp[i][j−1])dp[i][j]=grid[i][j]+min(dp[i−1][j],dp[i][j−1])
```


We just need to build this DP table until `(m-1, n-1)`.

#### Code :

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {


        int m = grid.size();
        int n = grid[0].size();

        vector<vector<int>> dp(m,vector<int> (n,0));

        dp[0][0] =  grid[0][0];

        for(int i=1;i<n;i++)
        dp[0][i] = dp[0][i-1] + grid[0][i];

        for(int i=1;i<m;i++)
        dp[i][0] = dp[i-1][0] + grid[i][0];

      

        int ans = 0;

        for(int i=1;i<m;i++)
        {
            for(int  j=1;j<n;j++)
            {
                dp[i][j] = min(dp[i-1][j] , dp[i][j-1]) +  grid[i][j];
            }
        }

        return dp[m-1][n-1];
        
    }
};
```

> `Time Complexity` : O(m * n)
> 
> `Space Complexity` : O(m * n)

---

### Approach 2 :

- Space optimized
- Only the previous row and left value is needed at anytime.

#### Code :

``` cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        // without modifying array and using O(n) extra space

        int m = grid.size();
        int n = grid[0].size();

        int left = INT_MAX;

        vector<int> up(n);
        up[0] = grid[0][0];
        for(int j=1;j<n;j++){
            up[j] = grid[0][j] + up[j-1];
        }

        for(int i=1;i<m;i++){
            for(int j=0;j<n;j++){
                up[j] = min(up[j],left) + grid[i][j];
                left = up[j];
            }
            left = INT_MAX;
        }

        return up[n-1];

    }
};
```

> `Time Complexity` : O(m * n)
> 
> `Space Complexity` : O(1)


---

### Approach 3 :

- Without using any extra space and modifying given matrix.

#### Code :

``` cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        
        int n = grid.size();
        int m = grid[0].size();

        //first row ps
        for(int j=1;j<m;j++){
            grid[0][j] += grid[0][j-1];
        }

        //first col ps
        for(int i=1;i<n;i++){
            grid[i][0] +=grid[i-1][0];
        }

        for(int i=1;i<n;i++){
            for(int j=1;j<m;j++){
                grid[i][j] += min(grid[i-1][j],grid[i][j-1]);
            }
        }

        return grid[n-1][m-1];
    }
};

```

> `Time Complexity` : O(m * n)
> 
> `Space Complexity` : O(1)

---
