[Problem Link](https://leetcode.com/problems/edit-distance/)
### Problem Statement : 

Given two strings `word1` and `word2`, return _the minimum number of operations required to convert `word1` to `word2`_.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character
### Example :

```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```


---

### Approach 1 :

- We want to transform `word1` → `word2`. At each step, we can:
- Insert a character (increase length of word1)
- Delete a character (decrease length of word1)
- Replace a character (fix mismatch)
    
If characters match, no operation is needed.  
If they don’t, we try all three operations and take the minimum.

This naturally leads to **Dynamic Programming**.

Let `dp[i][j]` = min operations to convert `word1[0..i-1]` → `word2[0..j-1]`.

1. **Base Cases**
    - If `i == 0`, need `j` insertions.
    - If `j == 0`, need `i` deletions.
        
2. **Transition**
    
    - If `word1[i-1] == word2[j-1]`:
        `dp[i][j] = dp[i-1][j-1]`
    - Else:
		```
		dp[i][j] = 1 + min(
	    dp[i-1][j],     // delete
	    dp[i][j-1],     // insert
	    dp[i-1][j-1]    // replace
	)
		```



#### Code :

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        
        int n = word1.size();
        int m = word2.size();

        vector<vector<int>> dp(n+1,vector<int> (m+1,0));

        for(int j=1;j<=m;j++){
            dp[0][j] = j;
        }
        for(int i=1;i<=n;i++){
            dp[i][0] = i;
        }


        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){

                if(word1[i-1] == word2[j-1]){
                    dp[i][j] = dp[i-1][j-1];
                }else{
                    dp[i][j] = min({dp[i-1][j-1],dp[i-1][j],dp[i][j-1]})+1;
                }
            }
        }

        return dp[n][m];
    }
};
```

> `Time Complexity` : O(m * n)
> 
> `Space Complexity` : O(m * n)

---

### Approach 2 :

- Tabulation Space Optimized
- **Idea**
	In the regular DP solution, we use a **2D table `dp[n+1][w+1]`**.  
	But notice:

- To compute `dp[i][*]`, we only need the **previous row `dp[i-1][*]`**.
    
- Hence, instead of storing the whole table, we can store only **two rows** (`prev` and `curr`).
- Let:
- `prev[j]` = `dp[i-1][j]`
- `cur[j]` = `dp[i][j]`

### Code :

``` cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        //space optimised
        int n = word1.size();
        int m = word2.size();

        vector<int> curr(m+1);
        vector<int> prev(m+1);

        for(int j=1;j<=m;j++){
            prev[j] = j;
        }
      
        for(int i=1;i<=n;i++){
            curr[0] = i;
            for(int j=1;j<=m;j++){

                if(word1[i-1] == word2[j-1]){
                    curr[j] = prev[j-1];
                }else{
                    curr[j] = min({prev[j-1],prev[j],curr[j-1]})+1;
                }
            }
            prev = curr;
        }

        return prev[m];
    }
};
```

> `Time Complexity` : O(n * m)
> 
> `Space Complexity` : O(m)

---
