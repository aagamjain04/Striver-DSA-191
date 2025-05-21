
[Problem Link](https://leetcode.com/problems/search-a-2d-matrix/)

### Problem Statement : 
You have been given a 2-D array **'mat'** of size **'N x M'** where **'N'** and **'M'** denote the number of rows and columns, respectively. The elements of each row are sorted in non-decreasing order. Moreover, the first element of a row is greater than the last element of the previous row (if it exists). 
\
You are given an integer **‘target’**, and your task is to find if it exists in the given 'mat' or not.

## Example

**Example 1:**

```
Input Format: N = 3, M = 4, target = 8,
mat[] = 
1 2 3 4
5 6 7 8 
9 10 11 12
Result: true
Explanation: The ‘target’  = 8 exists in the 'mat' at index (1, 3).
```

**Example 2:**

```
Input Format: N = 3, M = 3, target = 78,
mat[] = 
1 2 4
6 7 8 
9 10 34
Result: false
Explanation:The ‘target' = 78 does not exist in the 'mat'. Therefore in the output, we see 'false'.
```


### Approach 1 :

- Go over all rows and columns and see if element is present

> `Time Complexity` : O(m * n)
> 
> `Space Complexity` : O(1)

---

###  Approach 2 (Observation) :

Assume 2D matrix as flat array.

Example matrix:

```
    0   1   2   3
  ┌───┬───┬───┬───┐
0 │ 0 │ 1 │ 2 │ 3 │  n = 3
  ├───┼───┼───┼───┤  m = 4
1 │ 4 │ 5 │ 6 │ 7 │
  ├───┼───┼───┼───┤
2 │ 8 │ 9 │ 10│ 11│
  └───┴───┴───┴───┘
```

Where:
```
 l = 0, r = 11
 m = (l+r)/2 = 5

To access index 5 on 2D:

 i = 5/m = 1
 j = 5%m = 1

To access index 7:

 i = 7/m = 1
 j = 7%m = 3
```


=> Do a simple linear search assuming flat array using above formula to access index.

> `Time Complexity` : O(log (m * n))
> 
> `Space Complexity` : O(1)

---

###  Approach 3 :

Example array:

```
┌───┬───┬───┬───┐
│ 1 │ 3 │ 5 │ 7 │
├───┼───┼───┼───┤
│ 10│ 11│ 16│ 20│
├───┼───┼───┼───┤
│ 23│ 30│ 34│ 60│
└───┴───┴───┴───┘
```

Steps:

1. Do an upper bound on first column
2. The row below it is where binary search is to be done
3. Do binary search on row

### Algorithm Implementations

| Upper Bound                                                                                                                                                                            | Lower Bound                                                                                                                                                                           | Binary Search                                                                                                                                                                                                                                          |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `int l = 0;`<br>`int r = n;`<br><br>`while (l < r) {`<br>  `int m = (l+r)/2;`<br><br>  `if (arr[m] <= target)`<br>    `l = m+1;`<br>  `else`<br>    `r = m;`<br>`}`<br><br>`return r;` | `int l = 0;`<br>`int r = n;`<br><br>`while (l < r) {`<br>  `int m = (l+r)/2;`<br><br>  `if (arr[m] < target)`<br>    `l = m+1;`<br>  `else`<br>    `r = m;`<br>`}`<br><br>`return r;` | `int l = 0;`<br>`int r = n-1;`<br><br>`while (l <= r) {`<br>  `int m = (l+r)/2;`<br><br>  `if (arr[m] == target)`<br>    `return true;`<br>  `else if (arr[m] < target)`<br>    `l = m+1;`<br>  `else`<br>    `r = m-1;`<br>`}`<br><br>`return false;` |

### Code :

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        
        int n = matrix.size();
        int m = matrix[0].size();

        // do upper bound search on first column
        int l = 0, r = n;

        while(l<r){
            int m = (l+r)/2;
            if(matrix[m][0]<=target)
            l = m+1;
            else r = m;
        }

        int k = r-1;
        if(k==-1)
        return false;

        // do binary search on kth row

        l = 0 , r = m-1;

        while(l<=r){
            int m = (l+r)/2;
            if(matrix[k][m]==target)
            return true;
            else if(matrix[k][m]<target)
            l = m+1;
            else r = m-1;
        }
        return false;

    }
};```


**Note**: If lower/upper bound does not exist then r = n.

> `Time Complexity` : O(logm + logn)
> 
> `Space Complexity` : O(1)

---
