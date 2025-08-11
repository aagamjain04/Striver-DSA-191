[Problem Link](https://www.geeksforgeeks.org/problems/maximum-of-minimum-for-every-window-size3453/1)
### Problem Statement : 

Given an array `arr[]`, for every possible window size `k` (1 ≤ k ≤ n),

1. Consider **all subarrays** of size `k`.
2. Find the **minimum** element in each subarray.
3. Take the **maximum** among these minimum values.

We need to output these maximums for all `k`.

**Example 1:**

```
Input
arr = [10, 20, 30, 50, 10, 70, 30]

Window size 1 → Min in each = [10,20,30,50,10,70,30] → Max = 70  
Window size 2 → Min in each = [10,20,30,10,10,30]    → Max = 30  
Window size 3 → Min in each = [10,20,10,10,10]       → Max = 20  
Window size 4 → Min in each = [10,10,10,10]          → Max = 10  
...and so on.

```

### Approach 1 :
- Brute force
- For each `k` from 1 to n:
	- Slide a window of size `k`.
	- Find min in each window.
	- Track max of these mins.



> `Time Complexity` :O(N³) with naive min search, or O(N²) with deque-based min in O(1) amortized.
> 
> `Space Complexity` : O(1) with naive, O(N) with deque based 


---

###  Approach 2 :

- For each element `arr[i]`, determine the largest window size where `arr[i]` is the **minimum**
- We can do this by finding:
    - **Previous Smaller Element (PSE)** → first smaller element to the left.
    - **Next Smaller Element (NSE)** → first smaller element to the right.
- Window length where `arr[i]` is minimum = `right[i] - left[i] - 1`.

#### Example :

```
arr = [10, 20, 30, 50, 10, 70, 30]
n = 7
```

| i   | arr[i] | PSE[i] | NSE[i] | len | ans[len] update           |
| --- | ------ | ------ | ------ | --- | ------------------------- |
| 0   | 10     | -1     | 7      | 7   | ans[7] = max(-∞, 10) → 10 |
| 1   | 20     | 0      | 4      | 3   | ans[3] = max(-∞, 20) → 20 |
| 2   | 30     | 1      | 4      | 2   | ans[2] = max(-∞, 30) → 30 |
| 3   | 50     | 2      | 4      | 1   | ans[1] = max(-∞, 50) → 50 |
| 4   | 10     | -1     | 7      | 7   | ans[7] = max(10, 10) → 10 |
| 5   | 70     | 4      | 6      | 1   | ans[1] = max(50, 70) → 70 |
| 6   | 30     | 4      | 7      | 2   | ans[2] = max(30, 30) → 30 |

#### Code :

```cpp
vector<int> maxOfMins(vector<int>& arr) {
	
	int n = arr.size();
	vector<int> left(n,n),right(n,-1);
	stack<int> st;
	vector<int> ans(n+1);
	
	// next smaller element to left
	for(int i=0;i<n;i++){
		
		while(!st.empty() && arr[st.top()]>arr[i]){
			left[st.top()] = i;
			st.pop();
		}
		st.push(i);
	}
	
	while(!st.empty())
	st.pop();
	
	// next smaller element to right
	for(int i=n-1;i>=0;i--){
		
		while(!st.empty() && arr[st.top()]>arr[i]){
			right[st.top()] = i;
			st.pop();
		}
		st.push(i);
	}
	
	
   // Fill answers for lengths
	for (int i = 0; i < n; i++) {
		int len = left[i] - right[i] - 1;
		ans[len] = max(ans[len], arr[i]);
	}
	
	// Propagate answers backward
	for (int i = n - 1; i >= 1; i--)
		ans[i] = max(ans[i], ans[i + 1]);
	
	ans.erase(ans.begin()); // Remove ans[0] (unused)
	return ans;
	
	
	
}
```


> `Time Complexity` : O(N) (each element pushed/popped from stack once).
> 
> `Space Complexity` : O(N) for stack and result array

---

