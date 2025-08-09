[Problem Link](https://leetcode.com/problems/sliding-window-maximum/description/)
### Problem Statement : 

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return _the max sliding window_.

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
- For each window, scan all `k` elements and find the maximum.*
	1. Loop `i` from `0` to `n - k`
	2. For each window, find max by iterating `k` elements
	3. Store max in result array

> `Time Complexity` : O(n * k)
> 
> `Space Complexity` : O(1) 


---

###  Approach 2 :

- Use a **max heap** (priority queue) to store elements with their indices.  
Always remove elements that:

- Are outside the current window (`index <= i - k`)


> `Time Complexity` : O(n log k) 
> 
> `Space Complexity` : O(k)

---

### Approach 3 :

- Maintain a **deque** storing **indices** in decreasing order of their values.  
	This ensures:
	- **Front** → index of the maximum element in current window
	- Remove elements from **back** if they are **smaller than current element** (since they will never be needed again)
	- Remove elements from **front** if they are **out of the window**

#### Code :


```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        

        deque<int> dq;
        int n = nums.size();

        for(int i=0;i<k;i++){
           if(dq.empty()){
                dq.push_back(i);
           }else{
             while(!dq.empty() && nums[dq.back()] <= nums[i]){
                dq.pop_back();
             }
             dq.push_back(i);
           }
        }

        vector<int> ans;
        ans.push_back(nums[dq.front()]);

        for(int i=k;i<n;i++){

            while(!dq.empty() && dq.front() <= (i-k))
            dq.pop_front();

            while(!dq.empty() && nums[dq.back()] <= nums[i]){
                dq.pop_back();
            }
            dq.push_back(i);

            ans.push_back(nums[dq.front()]);
        

        }

        return ans;
    }
};

```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(k)

---
