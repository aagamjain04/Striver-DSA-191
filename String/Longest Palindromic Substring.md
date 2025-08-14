[Problem Link](https://leetcode.com/problems/longest-palindromic-substring/description/)
### Problem Statement : 

Given a string `s`, return the longest palindromic substring  in `s`.

**Example 1:**

```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

### Approach 1 :
- **Expand Around Center** (Optimal for Interview)
- A palindrome mirrors around its center.
- There are `2n-1` possible centers (`n` odd-length, `n-1` between characters for even-length).
- For each center, expand left & right while characters match.
- Track the longest substring found.

#### Code :


``` cpp
class Solution {
public:

    int maxL = 1;
    int start = 0;

    string longestPalindrome(string s) {
        // two pointer expand around center
        int n = s.size();
        if(n<2)
        return s;
        for(int i=0;i<n;i++){

            expandAround(i,i,s);
            expandAround(i,i+1,s);
        }

        return s.substr(start,maxL);
    }

    void expandAround(int l,int r,string &s){

        int n = s.size();

        while(l>=0 && r<n && s[l]==s[r]){

            int len = r-l+1;
            if(len > maxL){
                maxL = len;
                start = l;
            }
            l--;
            r++;
        }
    }
};
```

> `Time Complexity` : O(n^2)
> 
> `Space Complexity` : O(1) 


---

###  Approach 2 :

- **Dynamic Programming** (Less optimal in practice)
- Use a DP table `dp[i][j]` → `true` if substring `s[i..j]` is palindrome.
- Build up from substrings of length 1 → n.
- Track longest palindrome seen.

#### Code :

```cpp
string longestPalindrome(string s) {

	int n = s.size();

	if(n<2)
	return s;
	vector<vector<bool>> dp(n,vector<bool> (n,false)) ;

	int maxL = 1;
	int start = 0;

	// iteration should be like 
	// length 1,2,3....

	// length = 1
	for (int i = 0; i < n; i++)
		dp[i][i] = true;
	
	for(int len=2;len<=n;len++){

		for(int i=0;i+len-1<n;i++){

			int l = i;
			int r = i+len -1;
			
			if(s[r]==s[l]){

				if(len==2){
					dp[l][r] = true;
				}else{
					dp[l][r] = dp[l+1][r-1];
				}

				if (dp[l][r] && len > maxL) {
					maxL = len;
					start = l;
				}
			}
			   
		}	
	}

	return s.substr(start,maxL);
}
```


> `Time Complexity` : O(n^2) 
> 
> `Space Complexity` : O(n^2) 

---

### Approach 3 :

- **Manacher's Algorithm** (Advanced)
- Achieves `O(n)` time, but much harder to implement.
- Rarely expected unless the interviewer specifically asks for it.

---
