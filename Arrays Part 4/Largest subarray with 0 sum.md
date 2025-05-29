[Problem Link](https://www.geeksforgeeks.org/problems/largest-subarray-with-0-sum/1)

### Problem Statement : 
Given an array `arr` containing both positive and negative integers, the task is to compute the length of the largest subarray that has a sum of `0`.

## Example

**Example 1:**

```
Input: arr[] = [1, 0, -4, 3, 1, 0]
Output: 5
Explanation: The subarray is `[0, -4, 3, 1, 0]`.
```


**Example 2:**

```
Input: arr[] = [2, 10, 4]
Output: 0
Explanation: There is no subarray with a sum of 0.
```

### Approach 1 :

- Brute force
- Consider all possible subarrays
- Store the max length whose sum is zero

#### Code :

``` cpp
int maxSubArray(vector<int> &arr){
	int n = arr.size();
	int ans = 0;

	for(int i = 0; i < n; i++){

		int sum = 0;
		for(int j = i; j < n; j++){
			sum+=arr[i];
			if(sum == 0){
				ans = max(ans,j-i+1);
			}
		}

	}

	return ans;
}

```

> `Time Complexity` : O(n²)
> 
> `Space Complexity` : O(1)

---

### Approach 2:

- Use prefix sum to store sum
```
prefix sum array

PS = [x,x,x,x,i,x,x,x,j]

sum (i,j] =  PS[j]-PS[i]
```

- Store index of each PS in map
- if element already exist in map, no need to update max just take the diff of indexes
- Return the max difference

#### Code :

``` cpp
int maxLen(vector<int>& arr) {

	int n = arr.size();
	
	vector<int> ps(n);
	unordered_map<int,int> mp;
	ps[0] = arr[0];
	mp[ps[0]] = 0;
	mp[0] = -1;
	
	int ans = 0;
	if(ps[0]==0)
	ans = 1;
	
	for(int i=1;i<n;i++){
		ps[i] = ps[i-1]+arr[i];
		if(mp.find(ps[i])!=mp.end()){
			ans = max(ans,i-mp[ps[i]]);
		}else{
			 mp[ps[i]] = i;
		}
	   
	}
	
	return ans;
        
        
}
``` 

> `Time Complexity` : O(n) , considering O(1) for unordered map
> 
> `Space Complexity` : O(n) , for hash-map

---
