[Problem Link](https://leetcode.com/problems/maximum-profit-in-job-scheduling/description/)
### Problem Statement : 

We are given `n` jobs with:
- `startTime[i]` → when job `i` starts
- `endTime[i]` → when job `i` ends
- `profit[i]` → profit for completing job `i`
    
We need to find the **maximum profit** we can achieve by selecting a subset of jobs such that:
- No two jobs overlap
- If a job ends at time `X`, another job can start at the same time `X`.

Example :

```
start = [1, 2, 3, 4, 6]
end   = [3, 5, 10, 6, 9]
profit= [20, 20, 100, 70, 60]

Sorted by end time → jobs = 
[(1,3,20), (2,5,20), (4,6,70), (6,9,60), (3,10,100)]

dp progression:
dp[0] = 20
dp[1] = max(20,20) = 20
dp[2] = max(20,70+20) = 90
dp[3] = max(90,60+90) = 150
dp[4] = max(150,100+20) = 150
Answer = 150

```


---
### Approach 1 :

- Brute force
- Try all possible subsets of jobs
- Check if each subset has non-overlapping jobs.

> `Time Complexity` : O(2^n * n)
> 
> `Space Complexity` : O(1)


---

### Approach 2 :

- Greedy won't work as the jobs have weights and not just lengths.
- Approach is DP with binary search
- Sort jobs by end time -> Sorting by **end time** ensures that when you are at job `i`, the **optimal solution up to i-1 is already computed**.
- Use `dp[i]` = max profit achievable by considering jobs up to the `i`th job.
- For each job `i`, we have two choices:
    - **Include job `i`**:  
        Profit = `profit[i] + dp[last_non_conflicting(i)]`
    - **Exclude job `i`**:  
        Profit = `dp[i-1]`
 - Transition:
    `dp[i] = max(dp[i-1], profit[i] + dp[last_non_conflicting(i)])`
- Find `last_non_conflicting(i)` efficiently using **binary search**.
    
#### Code :

```cpp
class Solution {
public:
    int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
        
        vector<vector<int>> v;

        int n = profit.size();

        for(int i=0;i<n;i++){
            v.push_back({endTime[i],startTime[i],profit[i]});
        }

        sort(v.begin(),v.end());

        vector<int> dp(n,0);

        dp[0] = v[0][2];

        for(int i=1;i<n;i++){

            int currStart = v[i][1];
            int currEnd = v[i][0];
            int currProfit = v[i][2];

            int profit = currProfit;

            // find index whose end just less or equal currStart
            int l = 0, r = i-1;
            int idx = -1;
            while(l<=r){

                int mid = (l+r)/2;

                if(v[mid][0]<= currStart){
                    l = mid+1;
                    idx = mid;
                }else{
                    r = mid-1;
                }
            }
            if(idx!=-1){
                profit+=dp[idx];
            }

          

            dp[i] = max(dp[i-1],profit);
        }

        return dp[n-1];

    }
};
```

> `Time Complexity` : O(n log n)
> 
> `Space Complexity` : O(n)

---