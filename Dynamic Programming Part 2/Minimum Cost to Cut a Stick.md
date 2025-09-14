[Problem Link](https://leetcode.com/problems/partition-equal-subset-sum/description/)
### Problem Statement : 

You are given a stick of length `n` labeled from `0` to `n`.  
You are also given an integer array `cuts` where `cuts[i]` denotes a position where you need to cut the stick.

- You can perform cuts in **any order**.
- The cost of a cut is equal to the **length of the stick** being cut at that time.
- After cutting, the stick is split into two smaller sticks.
- You need to minimize the **total cost** of making all cuts.
    
**Return:** the minimum total cost of performing all cuts.

Example :

```
Input: n = 7, cuts = [1, 3, 4, 5]
Output: 16
Explanation: 
Optimal Sequence:

- First cut at `3` → cost = 7  
- Then cut at `1` → cost = 3
- Then cut at `5` → cost = 4
- Then cut at `4` → cost = 2
    
Total = 16
```


---

### Approach 1 :

- **Greedy doesn’t work**  
    Always cutting the smallest or largest piece first is not guaranteed minimal cost.
- **Overlapping subproblems**  
    Same segments of the stick are repeatedly considered → DP is needed.

- **Preprocess cuts** 
	- Sort cuts
	- This way, every cut happens strictly between valid boundaries.
- **DP State**
    - Let `dp[i][j]` = minimum cost to cut stick between `cuts[i]` and `cuts[j]`.
    - We only consider cuts between `i` and `j`.
- **Recurrence**
    `dp[i][j] = min(dp[i][k] + dp[k][j]) + (j - i) for all possible cuts k between them.`
- **Base case**  
    If no cuts possible (`i>j`), cost = `0`.    
- **Answer**  
    Final answer = `dp[0][m-1]` where `m = cuts.size()` after adding boundaries.

#### Code :

```cpp
class Solution {
public:

    int dp[101][101];
    int go(int left,int right,vector<int> &cuts,int i,int j){

        if(i>j)
        return 0;
        if(dp[i][j]!=-1)
        return dp[i][j];
        int ans = 1e9;
        for(int k=i;k<=j;k++){

            int leftPart = go(left,cuts[k],cuts,i,k-1);
            int rightPart = go(cuts[k],right,cuts,k+1,j);
            ans = min(ans, leftPart+rightPart+ (right-left) );
        }
        return dp[i][j] = ans;

    }

    int minCost(int n, vector<int>& cuts) {
        
        sort(cuts.begin(),cuts.end());
        memset(dp,-1,sizeof(dp));

        return go(0,n,cuts,0,cuts.size()-1);
    }

};
```

> `Time Complexity` : O(m^3)
> 
> `Space Complexity` : O(m^2)

---

