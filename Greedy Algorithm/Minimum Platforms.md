[Problem Link](https://www.geeksforgeeks.org/problems/minimum-platforms-1587115620/1)
### Problem Statement : 

You are given the arrival times `arr[]` and departure times `dep[]` of all trains that arrive at a railway station on the same day. Your task is to determine the minimum number of platforms required at the station to ensure that no train is kept waiting.

At any given time, the same platform cannot be used for both the arrival of one train and the departure of another. Therefore, when two trains arrive at the same time, or when one arrives before another departs, additional platforms are required to accommodate both trains.

## Examples

**Example 1:**

```
Input: arr[] = [900, 940, 950, 1100, 1500, 1800], dep[] = [910, 1200, 1120, 1130, 1900, 2000]
Output: 3
Explanation: There are three trains during the time 9:40 to 12:00. So we need a minimum of 3 platforms.
```

### Approach 1 :

- Every train arrival is a request for a platform.
- A train can depart and free a platform before another arrives.
- Sorting allows us to handle arrival/departure like a **sweep line algorithm** (chronological event tracking).


#### Logic : 
1. **Sort both arrays**:
    - `arr[]`: Arrival times.
    - `dep[]`: Departure times.
    - Why? To process events in chronological order.
        
2. **Initialize**:
    - `platforms = 1` → At least one platform needed.
    - `maxPlatforms = 1` → Track the peak requirement.
    - Pointers: `i = 1` (next arrival), `j = 0` (next departure)
        
3. **Iterate until all trains are processed**:
    - **If** next train arrives before or at the same time as the earliest departure (`arr[i] <= dep[j]`):
        - Need **another platform** → `platforms++`
        - Move to next arrival → `i++`
    - **Else**:
        - A platform is **freed** → `platforms--`
        - Move to next departure → `j++`
    - **Update** `maxPlatforms` if current exceeds previous max.


#### Code :

``` cpp
int findPlatform(vector<int>& arr, vector<int>& dep) {
        // Your code here
        
	sort(arr.begin(),arr.end());
	sort(dep.begin(),dep.end());
	
	int n = arr.size();
	
	int platform = 1;
	int maxPlatforms = 1;
	
	int i = 1;
	int j = 0;
	
	while(i<n){
		
		if(arr[i]>dep[j]){
			platform--;
			j++;
		}else {
			platform++;
			i++;
		}
		
		maxPlatforms = max(maxPlatforms,platform);
		
	}
	
	return maxPlatforms;
}

```


> `Time Complexity` : O(nlogn)
> 
> `Space Complexity` : O(1)


---


