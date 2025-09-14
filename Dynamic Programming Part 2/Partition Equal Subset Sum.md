[Problem Link](https://leetcode.com/problems/partition-equal-subset-sum/description/)
### Problem Statement : 

Given an integer array `nums`, return `true` if you can partition the array into two subsets such that the sum of the elements in both subsets is equal, otherwise return `false`.

Example :

```
Input: nums = [1,5,11,5]
Output: true
Explanation: Array can be partitioned as [1, 5, 5] and [11]
```


---

### Approach 1 :

- If we can partition array into two equal subsets, then `sum(subset1) = sum(subset2)`
- Since `sum(subset1) + sum(subset2) = total_sum`, we get `2 × sum(subset1) = total_sum`
- Therefore: `sum(subset1) = total_sum / 2`
- The problem becomes: "Can we find a subset with sum = total_sum/2?"
- 0/1 Knapsack Problem

#### Code :

```cpp
class Solution {
public:

    
   
    bool canPartition(vector<int>& nums) {
        
        int n = nums.size();
        int sum = 0;
        for(auto& i : nums){
            sum+=i;
        }

        if(sum&1)
        return false;

        int target = sum/2;

        bool dp[n+1][target+1];

        // Initialize all values to false
        for(int i=0; i<=n; i++){
            for(int j=0; j<=target; j++){
                dp[i][j] = false;
            }
        }

        // sum 0 is always true

        for(int i=0;i<=n;i++)
        dp[i][0] = true;

        for(int i=1;i<=n;i++){
            for(int t=1;t<=target;t++){
	            // Don't include current element
                bool notTake = dp[i-1][t];
                bool take = false;
                if(t>=nums[i-1]){
		            // include current element
                    take = dp[i-1][t-nums[i-1]];
                }
                dp[i][t] = take | notTake;
            }
        }

        return dp[n][target];

       
    }
};
```

> `Time Complexity` : O(n * sum)
> 
> `Space Complexity` : O(n * sum)

---

### Approach 2 :

- Tabulation Space Optimized
- **Idea**
	In the regular DP solution, we use a **2D table `dp[n+1][sum+1]`**.  
	But notice:

- To compute `dp[i][*]`, we only need the **previous row `dp[i-1][*]`**.
    
- Hence, instead of storing the whole table, we can store only **two rows** (`prev` and `curr`).

#### Code :

``` cpp
class Solution {
public:

    bool canPartition(vector<int>& nums) {  
        //space optimzed
        int n = nums.size();
        int sum = 0;
        for(auto& i : nums){
            sum+=i;
        }

        if(sum&1)
        return false;

        int target = sum/2;

        vector<bool> prev(target+1,false),curr(target+1,false);

        prev[0] = curr[0] = true;
        

        for(int i=1;i<=n;i++){
            for(int t=1;t<=target;t++){
                bool notTake = prev[t];
                bool take = false;
                if(t>=nums[i-1]){
                    take = prev[t-nums[i-1]];
                }
                curr[t] = take | notTake;
            }
            prev = curr;
        }
        return prev[target];
       
    }
};
```

> `Time Complexity` : O(n * target)
> 
> `Space Complexity` : O(target)


---
