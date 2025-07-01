[Problem Link](https://leetcode.com/problems/trapping-rain-water/description/)
### Problem Statement : 

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

## Examples

**Example 1:**

```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

### Approach 1 :

- The key insight is that the amount of water that can be trapped at any position depends on the minimum of:
	- the maximum height to the left of that position
	- the maximum height to the right of that position
- We calculate the maximum height to the left and right of each bar and use that to determine how much water can be trapped at each index.
- `leftMax[n]`: To store the max height to the **left** of each index.
- `rightMax[n]`: To store the max height to the **right** of each index.


#### Code :

``` cpp
int trap(vector<int>& height) {
	int n = height.size();

	vector<int> leftMax(n);
	vector<int> rightMax(n);
	int ans = 0;

	// scan from left to right
	for(int i=1;i<n;i++){
		leftMax[i] = max(leftMax[i-1],height[i-1]);
	}

	// scan from right to left
	for(int i=n-2;i>=0;i--){
		rightMax[i] = max(rightMax[i+1],height[i+1]);
	}

	for(int i=0;i<n;i++){
		cout << leftMax[i] << " ";
	}
	cout << "\n";
	  for(int i=0;i<n;i++){
		cout << rightMax[i] << " ";
	}
	cout << "\n";


	//calulate for each
	for(int i=1;i<n-1;i++){
		if(height[i]< (min(leftMax[i],rightMax[i])))
		ans+= (min(leftMax[i],rightMax[i]) - height[i]);
	}

	return ans;

}

```


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(n)


---


### Approach 2 :

- Two pointer approach
- The **Two Pointer Approach** works for the **Trapping Rain Water** problem due to the **key observation**:
- The amount of water trapped at any index depends on the **minimum** of the **maximum height to its left and right**.
- `l` and `r`: Two pointers starting from left and right.
- `lmax` and `rmax`: Track the **maximum height seen so far** from the left and right.
- `ans`: Accumulates the total trapped water.

#### Logic : 

At each iteration, we compare `lmax` and `rmax`:
- If `lmax <= rmax`, then we are **confident** that:
    - `lmax` is the **limiting factor** for the height at index `l`.
    - So water trapped at `l` = `lmax - height[l]` (if positive).
    - Why? Because the right side has enough height (`rmax ≥ lmax`) to trap water.
        
- If `rmax < lmax`, then:
    - `rmax` is the **limiting factor** for the height at index `r`.
    - So water trapped at `r` = `rmax - height[r]`.

#### Code :

``` cpp
int trap(vector<int>& height) {
        
	int n = height.size();
	int lmax = height[0];
	int rmax = height[n-1];

	int l = 1;
	int r = n-1;
	int ans = 0;
	while(l <= r) {
		if(lmax <= rmax){
			lmax = max(lmax,height[l]);
			ans+= (lmax-height[l]);
			l++;
		}else {
			rmax = max(rmax,height[r]);
			ans+= (rmax-height[r]);
			r--;
		}
	}

	return ans;
}

```


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)

---
