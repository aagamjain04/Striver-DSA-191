[Problem Link](https://leetcode.com/problems/word-break/description/)
### Problem Statement : 

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

Example :

```
Input: s = "leetcode", wordDict = ["leet","code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```


---

### Approach 1 :

- DP with memoization
- Try every possible prefix of `s`.
- If the prefix is in `wordDict`, recursively check the remaining suffix.
- Memoize to avoid repeated computation
    
#### Code :

```cpp
class Solution {
public:

    unordered_map<string,int> hash;
    int dp[301];

    bool go(int i,string &s){

        if(i == s.size()){
            return true; // able to segment
        }
        if(dp[i]!=-1)
        return dp[i];

        bool ans = false;
        for(int k=i;k<s.size();k++){
            string x = s.substr(i,k-i+1);
            if(hash.find(x)!=hash.end()){
                ans = ans | go(k+1,s);
            }
        }

        return dp[i] = ans;
    }

    bool wordBreak(string s, vector<string>& wordDict) {
        
        memset(dp,-1,sizeof(dp));

        for(auto &s:wordDict){
            hash[s] = 1;
        }

        return go(0,s);

    }
};
```

> `Time Complexity` : O(n^2)
> 
> `Space Complexity` : O(n)

---

### Approach 2 :

- Tabulation - Bottom UP
- `dp[i]` = true if `s[0..i-1]` can be segmented into dictionary words.
- Initialization: `dp[0] = true` (empty string is always valid).
- For each `i`, check all possible partitions `s[j..i-1]` where `dp[j]` is true and `s[j..i-1]` is in `wordDict`.

#### Code :

``` cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        

        unordered_map<string,int> hash;
        for(auto &s:wordDict){
            hash[s] = 1;
        }   
        int n = s.size();
        vector<int> dp(n+1,false);
        dp[0] = true; 


        for(int i=1;i<=n;i++){
            for(int j=0;j<i;j++){

                string x = s.substr(j,i-j);
                if(hash[x] && dp[j]==true){
                    dp[i] = true;
                    break;
                }
               
            }
        }

        return dp[n];

    }
};
```

> `Time Complexity` : O(n^2)
> 
> `Space Complexity` : O(n)


---
