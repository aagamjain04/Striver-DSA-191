[Problem Link](https://leetcode.com/problems/rotate-image/description/)

### Problem Statement : 

You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).   
\
You have to rotate the image in-place which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

```
Original Matrix:
                          
1 2 3                                           7 4 1
4 5 6              ----->                       8 5 2
7 8 9                                           9 6 3

             After 90° Clockwise Rotation:

```

#### Pattern Observed

For a 90° clockwise rotation:

- `newMatrix[0][0] = matrix[2][0]`
- `newMatrix[0][1] = matrix[1][0]`
- `newMatrix[i][j] = matrix[n-1-j][i]`

### Approach 1 : Brute force

- Create a dummy matrix and copy contents
- `newMatrix[i][j] = matrix[n-1-j][i]`

> `Space Complexity` : O(n²)

---

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

> **Space Complexity**: O(1) - We rotate the matrix in-place.

---