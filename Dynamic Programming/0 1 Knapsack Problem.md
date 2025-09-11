[Problem Link](https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1)
### Problem Statement : 

Given **n** items, each with a specific **weight** and **value**, and a knapsack with a capacity of **W**, the task is to put the items in the knapsack such that the **sum of weights of the items <= W** and the **sum of values** associated with them is **maximized**. 

**Note:** You can either place an item entirely in the bag or leave it out entirely. Also, each item is available in **single** quantity.
### Example :

```
Input: W = 4, `val[]` = [1, 2, 3], `wt[]` = [4, 5, 1]   
Output: 3  
Explanation: Choose the last item, which weighs 1 unit and has a value of 3.
```


---

### Approach 1 :

- Recursive (Exponential
- At each item, we have 2 choices:
    - **Include** the item (if weight permits).
    - **Exclude** the item.
- Recurrence:
``` lua
f(i, W) = max(
    value[i] + f(i-1, W - weight[i]),   // take item
    f(i-1, W)                           // skip item
) 
```
- Base Case: If `i == 0`, return `value[0]` if `weight[0] <= W`, else `0`.


> `Time Complexity` : O(2^(n))
> 
> `Space Complexity` : O() (stack space)

---

### Approach 2 :

- Apply memoization in approach 1
- Avoids re-computation

### Code :

``` cpp
class Solution {
  public:
    
    int dp[1001][1001];
    int go(int i,int W,vector<int> &val,vector<int> &wt){
        
        if(i>=wt.size() || W<=0)
        return 0;
        if(dp[i][W]!=-1)
        return dp[i][W];
        
        
        int pick = 0, notPick = 0;
        
        //pick
        if(W>=wt[i])
        pick = go(i+1,W-wt[i],val,wt) + val[i];
        
        //notpick
        notPick = go(i+1,W,val,wt);
        
        return dp[i][W] = max(pick,notPick);
    }
  
    int knapsack(int W, vector<int> &val, vector<int> &wt) {
        // code here
        memset(dp,-1,sizeof(dp));
        return go(0,W,val,wt);
    }
};
```

> `Time Complexity` : O(n * W)
> 
> `Space Complexity` : O(n * W)


---

### Approach 3 :

- Tabulation  - Bottom Up DP

``` lua
if (weight[i] <= w)
    dp[i][w] = max(value[i] + dp[i-1][w - weight[i]], dp[i-1][w]);
else
    dp[i][w] = dp[i-1][w];

```

### Code :

``` cpp
class Solution {
  public:
    int knapsack(int W, vector<int> &val, vector<int> &wt) {
        // code here
        int n = val.size();
        
        vector<vector<int>> dp(n+1,vector<int> (W+1,0) );
        
        for(int i=1;i<=n;i++){
            for(int j=1;j<=W;j++){
                
                if(wt[i-1]<=j){
                    dp[i][j] = max(val[i-1]+dp[i-1][j-wt[i-1]],dp[i-1][j]); 
                }else{
                    dp[i][j] = dp[i-1][j];
                }
                
            }
        }
        return dp[n][W];
    }
};
```

> `Time Complexity` : O(n * W)
> 
> `Space Complexity` : O(n * W)

---


### Approach 4 :

- Tabulation Space Optimized
- **Idea**
	In the regular DP solution, we use a **2D table `dp[n+1][W+1]`**.  
	But notice:

- To compute `dp[i][*]`, we only need the **previous row `dp[i-1][*]`**.
    
- Hence, instead of storing the whole table, we can store only **two rows** (`prev` and `curr`).

### Code :

``` cpp
class Solution {
  public:
    int knapsack(int W, vector<int> &val, vector<int> &wt) {
        // code here
        
        int n = val.size();
        
        vector<int> curr(W+1);
        vector<int> prev(W+1);
        
        for(int i=1;i<=n;i++){
            for(int j=1;j<=W;j++){
                
                if(wt[i-1]<=j){
                    curr[j] = max(val[i-1]+prev[j-wt[i-1]],prev[j]); 
                }else{
                    curr[j] = prev[j];
                }
                
            }
            prev = curr;
        }
        
        return prev[W];
        
    }
};
```

> `Time Complexity` : O(n * W)
> 
> `Space Complexity` : O(W)

---
