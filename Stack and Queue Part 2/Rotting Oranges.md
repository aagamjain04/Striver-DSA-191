[Problem Link](https://leetcode.com/problems/rotting-oranges/description/)
### Problem Statement : 

We have a grid:
- `0` → Empty cell
- `1` → Fresh orange
- `2` → Rotten orange
    
Each minute:
- Fresh oranges **adjacent (up, down, left, right)** to a rotten orange become rotten.    
We need to find:
- The **minimum minutes** required until no fresh oranges remain.
- Return `-1` if it’s **impossible**.

****


### Approach 1 :
- BFS Multisource
- All initially rotten oranges are **starting points** in the BFS.
- Perform BFS **level-by-level** to simulate the rotting process minute by minute.
- Track the time taken until the last orange rots.
- If some fresh oranges remain after BFS → return `-1`.


#### Code :


```cpp
  
int dirX[4] = {1,-1,0,0};
int dirY[4] = {0,0,1,-1};

int orangesRotting(vector<vector<int>>& grid) {
	
	queue<tuple<int,int,int>> q;

	int n = grid.size();
	int m = grid[0].size();
	int freshCount = 0;

	for(int i=0;i<n;i++){
		for(int j=0;j<m;j++){
			if(grid[i][j] == 2){
				q.push({i,j,0});
			}
			if(grid[i][j]==1){
				freshCount++;
			}
		}
	}

	int time = 0;

	while(!q.empty()){

		int currX,currY,t;

		tie(currX,currY,t) = q.front();
		q.pop();
		time = max(time,t);
	   
		for(int k=0;k<4;k++){
			int x = dirX[k] + currX;
			int y = dirY[k] + currY;
			
			if(x<n && y<m && x>=0 && y>=0 && grid[x][y] == 1){
				q.push({x,y,t+1});
				grid[x][y] = 2;
				freshCount--;
			}
		}
	   
	   
	}
	return (freshCount ? -1 : time);
}

```


> `Time Complexity` : O(n x m) -> Each cell is visited almost once
> 
> `Space Complexity` : O(n x m) -> for the BFS queue in the worst case

---
