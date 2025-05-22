
[Problem Link](https://leetcode.com/problems/unique-paths/description/)

### Problem Statement : 
There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return _the number of possible unique paths that the robot can take to reach the bottom-right corner_.

![img](../Images/robot_maze.png)


### Approach 1 : 

- Brute force recursion
- At any point we have 2 option : move right or go down
- `(i+1,0) OR (j+1,0)`

``` cpp
int uniquePath(int i,int j){
	if(i==m-1 && j==n-1)
	return 1;
	else if(i>=m || j>=n)
	return 0;

	return (uniquePath(i+1,0) + uniquePath(0,j+1));
}

```

> **Time Complexity**: O(2^(m+n))
> 
> **Space Complexity**: O(m+n) -> stack space

---

### Approach 2 :

- Using DP
- Initialize first row and column as 1 as there is only one way to reach there.
- Now at any point `(i,j)` we can reach from that point directly above or left of it.
- `no of ways to reach (i-1,0) + no ways to reach (i,j-1)`
- `mat[i][j] = mat[i-1][j] + mat[i][j-1]`


> **Time Complexity**: O(MN)
> 
> **Space Complexity**: O(MN)

---

### Approach 3 :

#### Combinatorics

- To reach end, we'll take a total steps of (m-1) right and (n-1) down. So total of `m+n-2` steps.
- Reduces to find no of ways to arrange (m-1) right moves or (m-1) down moves

**Formula:**

- `(m+n-2) C (m-1)` OR `(m+n-2) C (n-1)` [Take min(m,n)]

**Where:**

- `N = m+n-2`
- `r = min(m,n) - 1`

**Mathematical Formula:**

```
C(N,r) = N! / (r! × (N-r)!)
```

**Implementation**

cpp

```cpp
double ans = 1;
for (int i = 1; i <= r; i++) {
    ans = ans * (N - r + i);
    ans = ans / i;
}
return (int) ans;
```


>**Time Complexity**: O(min(M,N)) 
>
> **Space Complexity**: O(1)

---
