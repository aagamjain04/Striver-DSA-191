[Problem Link](https://leetcode.com/problems/longest-increasing-subsequence/description/)
### Problem Statement : 

Given an integer array `nums`, return the length of the **longest strictly increasing subsequence**.

**Subsequence**: Not necessarily contiguous, but must preserve order.
### Example :

```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

```


---

### Approach 1 :

- Dynamic Programming

#### Code :

``` cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {

        //bottomUp Tabulation

        int n = nums.size();
        vector<int> dp(n,1);
        int ans = 1;
        
        for(int i=0;i<n;i++){
            for(int j=0;j<i;j++){

                if(nums[i] > nums[j]){
                    dp[i] = max(dp[i],dp[j]+1);
                    ans = max(ans,dp[i]);
                }
            }
        }
        return ans;

    }
};
```




> `Time Complexity` : O(n^2)
> 
> `Space Complexity` : O(n)

---

### Approach 2 :

- Binary Search
- Maintain a `vector<int> lis` where:
    - For each `x` in nums:
        - If `x` is greater than last element → append.
        - Else → replace the first element in `lis` ≥ `x` using `lower_bound`.
            
- Length of `lis` = length of LIS.


Think of the problem like a **card game (patience sorting analogy):**
1. You place numbers into a **sequence of piles**.
2. Rule:
    - For each number `x` in `nums`:
        - If `x` is greater than the **last pile's top**, start a new pile (extend LIS).
            
        - Else, put `x` on the **leftmost pile whose top ≥ x** (using `lower_bound`).

Why?

- Replacing with `x` keeps the sequence "more favorable" for future numbers (smaller values give more room to grow).
    
- Pile count = length of LIS.


### Code :

``` cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        //bs
        vector<int> lis;
        for(int i=0;i<nums.size();i++){
            auto it = lower_bound(lis.begin(),lis.end(),nums[i]);
            if(it==lis.end())
            lis.push_back(nums[i]);
            else{
                int index = it - lis.begin();
                lis[index] = nums[i];
            }
        }
        return lis.size();
    }
};
```

> `Time Complexity` : O(nlogn)
> 
> `Space Complexity` : O(n)


---
