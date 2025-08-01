[Problem Link](https://www.geeksforgeeks.org/problems/aggressive-cows/1)
### Problem Statement : 

You are given:
- An array `stalls[]` containing **unique** integers representing positions of stalls.
- An integer `k` representing the number of aggressive cows.

You must place the `k` cows in the stalls such that:
- Each cow is placed in a **different stall**.
- The **minimum distance** between any two cows is as **large as possible**.

**Goal:**  
Return the largest possible minimum distance.

## Examples

**Example 1:**

```
Input: arr = [1,2,4,8,9], k = 3
Output: 3
Explanation:
- Sorted stalls: [1, 2, 4, 8, 9]
- One optimal placement:
  - Cow 1 → stall at position 1
  - Cow 2 → stall at position 4
  - Cow 3 → stall at position 8
- Minimum distance between cows = 3  
No larger minimum distance is possible.

```

### Approach 1 :

- Brute force approach
- Try **all possible stall combinations** for placing cows.
- For each combination, calculate the minimum distance and keep the maximum.
- **Exponential** → not feasible for large `n`.


---

### Approach 2:

- Binary Search
- If we fix a minimum distance `d`, we can check whether it is possible to place all cows such that no two cows are closer than `d`.
- If placement is possible for distance `d`, it will also be possible for smaller distances.
- If placement is **not** possible for distance `d`, it will also fail for larger distances.


#### Code :

``` cpp
bool isPossible(int x,vector<int> &stalls,int k){
	
	int lastCow = stalls[0];
	int placed = 1;
	for(int i=1;i<stalls.size();i++){
		if(stalls[i] - lastCow >= x){
			
			lastCow = stalls[i];
			placed++;
		}
	}
	
	return placed>=k;
		
}


int aggressiveCows(vector<int> &stalls, int k) {
	
	int n = stalls.size();
	sort(stalls.begin(),stalls.end());
	
	int l = 1 ,r = stalls[n-1] - stalls[0];
	int ans = -1;
	while(l<=r){
		
		int mid = (r-l)/2 + l;
		
		if(isPossible(mid,stalls,k)){
			ans = mid;
			l = mid+1;
		}else{
			r = mid-1;
		}
	}
	return ans;
	
	
}
```


> `Time Complexity` : O(n log n + n log(max_dist)
> 	
> `Space Complexity` : O(1)

---
