[Problem Link](https://leetcode.com/problems/coin-change/description/)
### Problem Statement : 

You are given:

- An integer array `coins[]` of different denominations.    
- An integer `amount`.
    
**Task**: Return the _fewest number of coins_ needed to make up `amount`.  
If not possible, return `-1`.

- Infinite supply of each coin is available.

Example :

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```


---

## Why **Greedy** Won’t Work

Greedy picks the largest possible coin at each step.  
This **fails** in some cases because the _locally optimal choice may not lead to a globally optimal solution_.

### Example:
`coins = [1, 3, 4], amount = 6`
- Greedy: Pick `4` → remainder `2` → then `1+1` → total **3 coins**.
- Optimal: Pick `3 + 3` → total **2 coins**. 
    
Hence, **greedy is not guaranteed to work**.  
We need **Dynamic Programming**.

### Approach 1 :

- We want the minimum coins to make amount = `x`.  

```
For each coin `c`, if we take it, we need:

dp[x]=1+dp[x−c]dp[x]=1+dp[x−c]

So:

- Base case: `dp[0] = 0` (0 coins to make amount 0).
- If `x - c < 0`, skip that coin.    
- If no valid coins → `dp[x] = INF`.


```


Final Answer = `dp[amount]` (or `-1` if INF).

#### Code :

```cpp
class Solution {
public:

    int go(vector<int> &coins,int amount,vector<int> &dp){

        if(amount==0)
        return 0;
        else if(amount<0)
        return 1e9;
        else if(dp[amount]!=-1)
        return dp[amount];

        int ans = 1e9;

        for(auto c: coins){

            ans = min(ans,go(coins,amount-c,dp)+1);
        }

        return dp[amount] = ans;
    }

    int coinChange(vector<int>& coins, int amount) {
        // memoization, space O(amount)
        vector<int> dp(amount+1,-1);
        int res = go(coins,amount,dp);

        return (res==1e9) ? -1 : res;
    }
};
```

> `Time Complexity` : O(n * amount)
> 
> `Space Complexity` : O(amount) + recursion stack

---

### Approach 2 :

- Bottom up tabulation

#### Code :

``` cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        //tabulation

        vector<int> dp(amount+1,1e9);
        dp[0] = 0;
        for(int i=1;i<=amount;i++){

            for(auto c:coins){

                if(c<=i){
                    dp[i] = min(dp[i],dp[i-c]+1);
                }
            }
        }
        return dp[amount]==1e9 ? -1 : dp[amount];
    }
};
```

> `Time Complexity` : O(n * amount)
> 
> `Space Complexity` : O(amount)


---
