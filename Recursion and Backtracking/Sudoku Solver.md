[Problem Link](https://leetcode.com/problems/sudoku-solver/)
### Problem Statement : 

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

The `'.'` character indicates empty cells.

### Approach 1 :

Use **backtracking** to try placing digits `1` to `9` in each empty cell.  
For each empty cell:

1. Try placing a valid digit.
2. If it leads to a solution, continue recursively.
3. If not, backtrack and try the next possible digit.

##### Backtracking steps :
- Iterate through the board to find an empty cell (`'.'`).
- Try placing digits `1-9`:
    - If `isValid`, place the digit.
    - Recursively call the solver.
    - If recursion returns `true`, return `true`.
    - Otherwise, reset the cell (`'.'`) and try next digit.
- If all cells are filled correctly, return `true`.

#### Code :

``` cpp
bool canPlace(int x,int y,int k,vector<vector<char>>& board,int n){


	// check row
	for(int j=0;j<n;j++){
		if(board[x][j] == (k+'0'))
		return false;
	}

	// check col
	for(int i=0;i<n;i++){
		if(board[i][y] == (k+'0'))
		return false;
	}

	// check box
	for(int i=0;i<3;i++){
		for(int j=0;j<3;j++){
			int a = i+((x/3)*3);
			int b = j+((y/3)*3);
			if(board[a][b] == (k+'0'))
			return false;
		}
	}

	return true;

}

bool solve(vector<vector<char>>& board,int n){

	for(int i=0;i<n;i++){
		for(int j=0;j<n;j++){
			if(board[i][j] == '.'){
				for(int k=1;k<=9;k++){
					if(canPlace(i,j,k,board,n)){
						board[i][j] = k+'0';
						if(solve(board,n))
						return true;
					}
					board[i][j] = '.';
				}
				return false;
			}
		}
	}
	
	return true;
}
```


> `Time Complexity` : `O(n x 9^(nxn))` -> For every unassigned index, there are 9 possible options and for each index, we are checking other columns, rows and boxes.`
> 
> `Space Complexity` : `O(1)` (not considering stack space)

---

