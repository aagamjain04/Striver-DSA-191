
[Problem Link](https://leetcode.com/problems/set-matrix-zeroes/description/)

### Problem Statement : 
Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.
You must do it `in place`.

![[Screenshot 2025-04-12 at 3.54.04 PM.png]]

### Approach 1 :

- Iterate over entire matrix
- Create two hash-map `row` and `col`
- Mark the indexes accordingly
- Again iterate over whole matrix and see if any of the row or col is marked, then make that index zero
```cpp
void setZeroes(vector<vector<int>>& mat) {
        map<int,int> row,col;
        int n = mat.size();
        int m = mat[0].size();
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(mat[i][j] == 0){
                    row[i] = 1;
                    col[j] = 1;
                }
            }
        }
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(row[i]==1 || col[j]==1)
                mat[i][j] = 0;
            }
        }

    }
```

`Time Complexity` : O(m * n)
`Space Complexity` : O(m+n)

---

### Approach 2:

- Instead of using extra row ,col we can use first row and column as markers
```cpp
void setZeroes(vector<vector<int>>& mat) {
    // Get the dimensions of the matrix
    int n = mat.size(), m = mat[0].size();
    
    // Variables to track if the first row and column need to be zeroed
    bool firstRowZero = false, firstColZero = false;
    
    // Check if any element in the first column is zero
    for(int i = 0; i < n; i++)
        if(mat[i][0] == 0) firstColZero = true;
    
    // Check if any element in the first row is zero
    for(int j = 0; j < m; j++)
        if(mat[0][j] == 0) firstRowZero = true;
    
    // Mark rows and columns that need to be zeroed
    // Use the first row and column as markers
    for(int i = 1; i < n; i++) {
        for(int j = 1; j < m; j++) {
            if(mat[i][j] == 0) {
                // Mark the corresponding row and column for zeroing
                mat[0][j] = 0; // Mark this column
                mat[i][0] = 0; // Mark this row
            }
        }
    }
    
    // Zero out cells based on markers
    for(int i = 1; i < n; i++) {
        for(int j = 1; j < m; j++) {
            // If either the row or column is marked for zeroing
            if(mat[i][0] == 0 || mat[0][j] == 0)
                mat[i][j] = 0;
        }
    }
    
    // Zero out the first column if needed
    if(firstColZero)
        for(int i = 0; i < n; i++)
            mat[i][0] = 0;
    
    // Zero out the first row if needed
    if(firstRowZero)
        for(int i = 0; i < m; i++)
            mat[0][i] = 0;
}
```

Time Complexity` : O(m * n)
`Space Complexity` : O(1)

