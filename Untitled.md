# Matrix Rotation 90 Degrees Clockwise

## Problem Statement

Given a matrix, your task is to rotate the matrix 90 degrees clockwise.

## Example

Original Matrix:

```
1 2 3
4 5 6
7 8 9
```

After 90° Clockwise Rotation:

```
7 4 1
8 5 2
9 6 3
```

## Pattern Observed

For a 90° clockwise rotation:

- `newMatrix[0][0] = matrix[2][0]`
- `newMatrix[0][1] = matrix[1][0]`
- `newMatrix[i][j] = matrix[n-1-j][i]`

## Approach 1: Brute Force

Create a new matrix and copy elements according to the rotation pattern.

```cpp
void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    vector<vector<int>> rotated(n, vector<int>(n));
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            rotated[i][j] = matrix[n-1-j][i];
        }
    }
    
    matrix = rotated;
}
```

**Space Complexity**: O(n×n) - We create a new matrix of the same size.

### Approach 2: Optimal (In-place)

Rotate the matrix in-place by:

1. Transpose the matrix (swap rows with columns)
2. Reverse each row

Original Matrix:

```
1 2 3
4 5 6
7 8 9
```

After Transpose:

```
1 4 7
2 5 8
3 6 9
```

After Reversing Rows:

```
7 4 1
8 5 2
9 6 3
```

```cpp
void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    
    // Transpose
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            swap(matrix[i][j], matrix[j][i]);
        }
    }
    
    // Reverse each row
    for (int i = 0; i < n; i++) {
        reverse(matrix[i].begin(), matrix[i].end());
    }
}
```

**Space Complexity**: O(1) - We rotate the matrix in-place.

## Time Complexity

Both approaches have a time complexity of O(n²) where n is the dimension of the matrix.