
[Problem Link](https://leetcode.com/problems/maximum-subarray/description/)

### Problem Statement : 
Given an integer array arr, find the contiguous subarray (containing at least one number) which  
has the largest sum and returns its sum and prints the subarray.


### Approach 1 :

- Calculate the sum of all possible subarrays and keep track of the maximum sum.
	- **Time Complexity**: O(n²) where n is the length of the array **Space Complexity**: O(1)

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        int maxSum = INT_MIN;
        
        for (int i = 0; i < n; i++) {
            int currentSum = 0;
            for (int j = i; j < n; j++) {
                currentSum += nums[j];
                maxSum = max(maxSum, currentSum);
            }
        }
        
        return maxSum;
    }
};
```


---

### Approach 2:

- Kadane's algorithm is the optimal approach for solving this problem. The idea is to keep a running sum of the subarray and reset it to zero whenever it becomes negative.

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSum = nums[0];
        int currSum = 0;
        for(int i=0;i<nums.size();i++){
            currSum+=nums[i];
            maxSum = max(maxSum,currSum);
            if(currSum<=0){
                currSum = 0;
            }
        }
        return maxSum;
    }
};
```

> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)

---
