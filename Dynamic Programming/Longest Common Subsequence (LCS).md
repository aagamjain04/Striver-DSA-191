[Problem Link](https://leetcode.com/problems/longest-common-subsequence/)
### Problem Statement : 

Given two strings `text1` and `text2`, return the length of their **longest common subsequence (LCS)**.  
If no common subsequence exists, return `0`.

- **Subsequence**: A sequence derived from another string by deleting some characters (possibly none) without changing order.
### Example :

```
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```


---

### Approach 1 :

- Brute force
- Check all subsequence


> `Time Complexity` : O(2^(n+m))
> 
> `Space Complexity` : O(n+m) (stack space)

---

### Approach 2 :

- Recurrence
- If either string is empty:  
    `f(i, j) = 0`
- If characters match:  
    `f(i, j) = 1 + f(i-1, j-1)`
- Else:  
    `f(i, j) = max(f(i-1, j), f(i, j-1))`

- Use **memoization (`dp[i][j]`)** to avoid re-computation.

### Code :

``` cpp
class Solution {
public:
    int dp[1001][1001];
    int go(int i,int j,int n,int m,string &text1,string &text2){
        
        if(i>=n || j>=m)
        return 0;
        if(dp[i][j]!=-1)
        return dp[i][j];

        int op1 = 0, op2 = 0;

        if(text1[i]==text2[j])
        op1 = go(i+1,j+1,n,m,text1,text2) + 1;

        op2 = max(go(i+1,j,n,m,text1,text2) , go(i,j+1,n,m,text1,text2));

        return dp[i][j] = max(op1,op2);

    }

    int longestCommonSubsequence(string text1, string text2) {
        
        int n = text1.size();
        int m = text2.size();
        memset(dp,-1,sizeof(dp));
        return go(0,0,n,m,text1,text2);
    }
};
```

> `Time Complexity` : O(n * m)
> 
> `Space Complexity` : O(n * m) + recursion stack


---

### Approach 3 :

- Tabulation
- **Recurrence**:
- If `text1[i-1] == text2[j-1]`:  
    `dp[i][j] = 1 + dp[i-1][j-1]`
- Else:  
    `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`

 - **Base Case**:
- `dp[0][*] = 0` (empty string vs anything → LCS = 0)
- `dp[*][0] = 0`

### Code :

``` cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        // tabulation

        int n = text1.size();
        int m = text2.size();

        vector<vector<int>> dp(n+1,vector<int> (m+1,0));

        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){

                if(text1[i-1] == text2[j-1])
                dp[i][j] = 1 + dp[i-1][j-1];
                else
                dp[i][j] = max(dp[i-1][j],dp[i][j-1]);
            }
        }
        return dp[n][m];
    }
};
```

> `Time Complexity` : O(n * m)
> 
> `Space Complexity` : O(n * m)

---


### Approach 4 :

- Tabulation Space Optimized
- **Idea**
	In the regular DP solution, we use a **2D table `dp[n+1][m+1]`**.  
	But notice:

- To compute `dp[i][*]`, we only need the **previous row `dp[i-1][*]`**.
    
- Hence, instead of storing the whole table, we can store only **two rows** (`prev` and `curr`).

### Code :

``` cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        
        int n = text1.size();
        int m = text2.size();

        if (m > n) 
        swap(text1, text2), swap(n, m);

        vector<int> prev(m+1);
        vector<int> curr(m+1);

        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){

                if(text1[i-1] == text2[j-1])
                curr[j] = 1 + prev[j-1];
                else
                curr[j] = max(prev[j],curr[j-1]);
            }
            prev = curr;
        }
        return prev[m];
    }
};
```

> `Time Complexity` : O(n * m)
> 
> `Space Complexity` : O(min(n,m))

---
