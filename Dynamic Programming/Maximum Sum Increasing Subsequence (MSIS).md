[Problem Link](https://www.geeksforgeeks.org/problems/maximum-sum-increasing-subsequence4749/1)
### Problem Statement : 

Given an array of positive integers `arr`, find the maximum sum subsequence such that the subsequence is **strictly increasing**.
### Example :

```
Input: arr = [1, 101, 2, 3, 100, 4, 5]
Output: 106  
Explanation: [1, 2, 3, 100] has maximum sum = 106
```


---

### Approach 1 :

- Similar to **Longest Increasing Subsequence (LIS)**.
- Instead of maximizing **length**, we maximize the **sum** of the subsequence.
- For each element `arr[i]`, we find the **maximum sum subsequence ending at i**.
    

#### Code :

```cpp
class Solution {
public:
    int maxSumIS(vector<int>& arr) {
        int n = arr.size();
        vector<int> dp(n);
        int maxSum = 0;

        for(int i = 0; i < n; i++) {
            dp[i] = arr[i]; // at least the element itself
            for(int j = 0; j < i; j++) {
                if(arr[i] > arr[j]) {
                    dp[i] = max(dp[i], dp[j] + arr[i]);
                }
            }
            maxSum = max(maxSum, dp[i]);
        }
        return maxSum;
    }
};
```

> `Time Complexity` : O(n^2)
> 
> `Space Complexity` : O(n)

---
