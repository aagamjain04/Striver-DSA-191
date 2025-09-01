[Problem Link](https://leetcode.com/problems/flood-fill/description/)
### Problem Statement : 

Given an `m x n` grid representing an image:

- Each cell is a pixel with a color (integer).
- Starting from pixel `(sr, sc)`, change its color to a new color.
- Recursively/BFS/DFS fill all **connected pixels** (sharing an edge) that have the same initial color.
- Return the modified image.

**Example 1:**

```
Input : Image:
1 1 1
1 1 0
1 0 1

sr = 1, sc = 1, color = 2

Output :
2 2 2
2 2 0
2 0 1


```

`

---


### Approach 1 :

1. BFS
2. We need to **explore all connected pixels** of the same initial color.

#### Code :

```cpp

class Solution {
public:

    int dirX[4] = {1,-1,0,0};
    int dirY[4] = {0,0,1,-1};

    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        
        queue<pair<int,int>> q;
        q.push({sr,sc});
        int n = image.size();
        int m = image[0].size();
        int initColor = image[sr][sc];

        while(!q.empty()){

            int currX = q.front().first;
            int currY = q.front().second;

            image[currX][currY] = color;

            q.pop();

            for(int k=0;k<4;k++){

                int X = currX + dirX[k];
                int Y = currY + dirY[k];
                if(X>=0 && Y>=0 && X<n && Y<m && image[X][Y]==initColor && image[X][Y]!=color){
                    q.push({X,Y});
                }

            }
        }

        return image;


    }
};
```


> `Time Complexity` : O(m x n)
> 
> `Space Complexity` : O(m x n)

---

