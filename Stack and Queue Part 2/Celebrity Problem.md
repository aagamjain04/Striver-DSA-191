[Problem Link](https://www.geeksforgeeks.org/problems/the-celebrity-problem/1)
### Problem Statement : 

We have an `n x n` matrix `mat[][]`:
- `mat[i][j] = 1` → Person `i` knows person `j`
- `mat[i][j] = 0` → Person `i` does **not** know person `j`
- Celebrity → **Known by everyone** but **knows no one**.

Return **index of celebrity** or **-1** if no celebrity exists.

**Example 1:**

```
Input:
nums = [1,3,-1,-3,5,3,6,7]
k = 3

Sliding windows & max:
[1, 3, -1] → 3
[3, -1, -3] → 3
[-1, -3, 5] → 5
[-3, 5, 3] → 5
[5, 3, 6] → 6
[3, 6, 7] → 7

Output: [3, 3, 5, 5, 6, 7]
```

### Approach 1 :
- Brute force
- Create two arrays `indegree` and `outdegree`, to store the `indegree` and `outdegree`
- Run a nested loop, the outer loop from 0 to n and inner loop from `0 to n`
- For every pair **i, j** check if **i** knows **j** then increase the outdegree of **i** and indegree of **j**
- For every pair **i, j** check if **j** knows **i** then increase the outdegree of **j** and indegree of **i**
- Run a loop from **0 to n** and find the id where the indegree is **n-1** and outdegree is **0**

#### Code :


``` cpp
// Function to find the celebrity
int celebrity(vector<vector<int>> & mat) {
    int n = mat.size();

    // indegree and outdegree array
    vector<int> indegree(n, 0), outdegree(n, 0);

    // query for all edges
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            int x = mat[i][j];

            // set the degrees
            outdegree[i] += x;
            indegree[j] += x;
        }
    }

    // find a person with indegree n-1
    // and out degree 0
    for (int i = 0; i < n; i++)
        if (indegree[i] == n - 1 && outdegree[i] == 0)
            return i;

    return -1;
}

```

> `Time Complexity` : O(n2)
> 
> `Space Complexity` : O(n) 


---

###  Approach 2 :

- Stack Elimination
- Push all people into a stack.
- Repeatedly pop two persons `a` and `b`:
    - If `a` knows `b`, `a` **cannot** be celebrity → push `b`.
    - Else `b` cannot be celebrity → push `a`.
- One candidate left → verify with definition.

#### Code :

```cpp
int celebrity(vector<vector<int> >& mat) {
	// code here
	int n = mat.size();
	stack<int> st;
	
	for(int i=0;i<n;i++){
		st.push(i);
	}
	
	while(st.size()>1){
		
		int a = st.top();
		st.pop();
		int b = st.top();
		st.pop();
		
	   // if a knows b
		if (mat[a][b]) {
			st.push(b);
		}
		else {
			st.push(a);
		}
	}
	
	// Potential candidate of celebrity
	int c = st.top();
	st.pop();
	for (int i = 0; i < n; i++) {
		if(i == c) continue;

		if (mat[c][i]|| !mat[i][c])
			return -1;
	}

	return c;
}

```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(n)

---

### Approach 3 :

- Two pointer
- Start with two pointers `i = 0` and `j = n-1`.
- If `i` knows `j` → `i` cannot be celebrity → `i++`.
- Else `j` cannot be celebrity → `j--`.
- At end, `i == candidate`.
- Verify.

#### Code :


```cpp
int celebrity(vector<vector<int> >& mat) {
	// code here
	int n = mat.size();
	int i = 0, j = n - 1;
	while (i < j) {
		
		// j knows i, thus j can't be celebrity
		if (mat[j][i] == 1) 
			j--;

		// else i can't be celebrity
		else
			i++;
	}
	
	// Potential candidate of celebrity
	int c = i;
  

	// Check if c is actually
	// a celebrity or not
	for (int i = 0; i < n; i++) {
		if(i == c) continue;

		// If any person doesn't
		// know 'c' or 'c' doesn't
		// know any person, return -1
		if (mat[c][i]|| !mat[i][c])
			return -1;
	}

	return c;
}
```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(1)

---
