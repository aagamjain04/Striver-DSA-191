[Problem Link](https://www.geeksforgeeks.org/problems/egg-dropping-puzzle-1587115620/1)
### Problem Statement : 

We have **n eggs** and a building with **k floors**.  
We need the **minimum number of moves** required to determine the **critical floor** (highest floor from which an egg can be dropped without breaking).

Example :

```
Input: n = 2, k = 36
Output: 8
Explanation: In all the situations, 8 minimum moves are required to find the maximum floor. Following is the strategy to do so:
Drop from floor 8 → If breaks, check 1-7 sequentially.
Drop from floor 15 → If breaks, check 9-14.
Drop from floor 21  → If breaks, check 16-20.
Drop from floor 26 → If breaks, check 22-25.
Drop from floor 30 → If breaks, check 27-29.
Drop from floor 33 → If breaks, check 31-32.
Drop from floor 35 → If breaks, check 34.
Drop from floor 36 → Final check.
```


---

### Approach 1 :

- Naive recursive DP
- Try dropping from every floor `i = 1..k`.
- Two cases:
    - Egg breaks → check below (`n-1` eggs, `i-1` floors).
    - Egg doesn’t break → check above (`n` eggs, `k-i` floors).
        
- Take worst case (`max`) and minimize over `i`.
- Gives TLE on submission

#### Code :

```cpp
class Solution {
  public:
  
    int dp[1001][1001];
    int go(int n,int k){
        
        if(k==0 || k==1){
            return k;
        }
        if(n==1)
        return k;
        if(dp[n][k]!=-1)
        return dp[n][k];
        
        int ans = 1e9;
        for(int i=1;i<=k;i++){
            
            
            int eggBreak = go(n-1,i-1);
            int NoeggBreak = go(n,k-i);
            
            ans = min(ans,max(eggBreak,NoeggBreak));
            
        }
        
        return dp[n][k] = ans+1;
        
    }
    int eggDrop(int n, int k) {
        
        
        memset(dp,-1,sizeof(dp));
        return go(n,k);
        
    }
};
```

> `Time Complexity` : O(n·k²))
> 
> `Space Complexity` : O(n * k)

---

### Approach 2:

- Move based DP
- Instead of “minimum moves for k floors”, flip the question:
  “With `n` eggs and `m` moves, how many floors can we test?”

#### DP Definition

```typescript
dp[m][n] = maximum number of floors that can be tested 
           with m moves and n eggs

```

#### Recurrence

When dropping an egg:

- Egg breaks → we have `n-1` eggs, `m-1` moves → `dp[m-1][n-1]`
- Egg doesn’t break → we have `n` eggs, `m-1` moves → `dp[m-1][n]`
- Plus the current floor

``` typescript
dp[m][n] = dp[m-1][n-1] + dp[m-1][n] + 1
```


#### Base Cases

- `dp[0][n] = 0` → with 0 moves, can’t check any floors.
- `dp[m][0] = 0` → with 0 eggs, can’t check any floors.
    
#### Algorithm

- Start with `m = 0`.
- Increment `m` until:
    `dp[m][n] >= k`
- Answer = `m`.


#### Code :

```cpp
class Solution {
  public:

    // Function to find minimum number of attempts needed in
    // order to find the critical floor.
    int eggDrop(int n, int k) {
        
        
        int dp[1001][1001]; // dp[m][n] = maximum number of floors that can be tested with m moves and n eggs.
        memset(dp,0,sizeof(dp));
        
        
        int m = 0;
        while(dp[m][n] < k){
            
            m++;
            for(int e=1;e<=n;e++){
                dp[m][e] = dp[m-1][e] + dp[m-1][e-1] + 1;
            }
        }
        return m;
        
        
    }
};
```

> `Time Complexity` : O(n * k))
> 
> `Space Complexity` : O(n * k)

---
