[Problem Link](https://www.geeksforgeeks.org/problems/matrix-chain-multiplication0303/1)
### Problem Statement : 

We are given an array `arr[]` of size `n` where the dimensions of the `i`-th matrix are:

`Mi=arr[i−1]×arr[i]Mi​=arr[i−1]×arr[i]`

We need to find the minimum number of scalar multiplications required to multiply the chain of matrices.

---

### Approach 1 :

- Matrix multiplication is **associative**, meaning:
    (A×B)×C=A×(B×C)(A×B)×C=A×(B×C)
    but the **cost** in terms of multiplications differs depending on the parenthesization.
    
- Example:  
    For dimensions `10 x 20, 20 x 30, 30 x 40`:
    
    - If we do `(A × B) × C` → cost = `(10×20×30) + (10×30×40) = 6000 + 12000 = 18000`
        
    - If we do `A × (B × C)` → cost = `(20×30×40) + (10×20×40) = 24000 + 8000 = 32000`  
        → Optimal is the **first way**.

### Recurrence (Recursive DP)

- Define `dp[i][j]` = minimum cost to multiply matrices from `i` to `j`.
- If there is only one matrix (`i == j`) → cost = 0.
- Otherwise, try all possible partitions `k`:
    
    ``` lua
    dp[i][j]=min​(dp[i][k]+dp[k+1][j]+arr[i−1]×arr[k]×arr[j])
    
    Given, i≤k<j
    
    ```

### Algorithm

1. Initialize `dp[i][i] = 0` (single matrix needs no multiplication).
2. Fill DP table for chains of length 2 up to `n-1`.
3. Return `dp[1][n-1]`.
    

#### Code :

```cpp
class Solution {
public:
    int matrixMultiplication(int N, int arr[]) {
        // dp[i][j] -> min cost to multiply matrices i...j
        vector<vector<int>> dp(N, vector<int>(N, 0));
        
        // chain length (L)
        for(int len = 2; len < N; len++) {
            for(int i = 1; i + len - 1 < N; i++) {
                int j = i + len - 1;
                dp[i][j] = INT_MAX;
                for(int k = i; k < j; k++) {
                    int cost = dp[i][k] + dp[k+1][j] + arr[i-1] * arr[k] * arr[j];
                    dp[i][j] = min(dp[i][j], cost);
                }
            }
        }
        return dp[1][N-1];
    }
};
```

> `Time Complexity` : O(n^3) -> n^2 states and each state tries up to n partitions
> 
> `Space Complexity` : O(n^2)

---
