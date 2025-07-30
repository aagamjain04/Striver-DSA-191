[Problem Link](https://www.geeksforgeeks.org/problems/median-in-a-row-wise-sorted-matrix1527/1)
### Problem Statement : 

Given a **row-wise sorted** matrix **mat[][]** where the number of rows and columns is always **odd**. Return the **median** of the matrix.
## Examples

**Example 1:**

```
Input: mat[][] = [[1, 3, 5],   
                [2, 6, 9],   
                [3, 6, 9]]
Output: 5
Explaination: Sorting matrix elements gives us [1, 2, 3, 3, 5, 6, 6, 9, 9]. Hence, 5 is median.

```

### Approach 1 :

- Brute force
- Store all elements in a list
- Sort and return the middle element

#### 

> `Time Complexity` : O(m x n) + O(m x n log(m x n)) -> copying plus sorting
> 
> `Space Complexity` : O(m x n)

---

### Approach 2:

- Using binary search.
- Instead of sorting all elements, binary search on possible answer values and use row-wise sorting to efficiently count elements.



``` cpp
int upperBound(int l,int r,vector<int> &v,int x){
	
	while(l<r){
		int mid = (l+r)/2;
		if(v[mid]<=x){
			l = mid+1;
		}else{
			r = mid;
		}
	}
	return r;
}

bool isGood(int x,int n,int m,vector<vector<int>> &mat){
	
	int greaterThanX = 0;
	for(int i=0;i<n;i++){
		
		int k = upperBound(0,m,mat[i],x);
		greaterThanX += (m-k);
	}
	
	int lessThanEqualToX = (m*n - greaterThanX);
	
	int median = (m*n+1)/2;
	
	return (lessThanEqualToX>=median);
	
}

int median(vector<vector<int>> &mat) {

	int n = mat.size();
	int m = mat[0].size();
	
	int l = 1, r = 2000;
	
	int ans = 0;
	while(l<=r){
		int mid = (l+r)/2;
		if(isGood(mid,n,m,mat)){
			ans = mid;
			r = mid-1;
		}else{
			l = mid+1;
		}
	}
	
	return ans;
}
```

> `Time Complexity` : O(log(2000) * N * log(M))
> 	
> `Space Complexity` : O(1)

---

