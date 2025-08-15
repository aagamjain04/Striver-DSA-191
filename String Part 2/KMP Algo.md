[Problem Link](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)
### Problem Statement : 

Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

**Example 1:**

```
Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.
```

### Approach 1 :
- Naive Search (Brute Force)
- Check every starting position `i` in `haystack` where a substring of length `needle.size()` can fit.
- Compare characters one by one.


> `Time Complexity` : O(n * m)
> 
> `Space Complexity` : O(1) 


---

###  Approach 2 :

**Idea**: Avoid re-checking characters when a mismatch occurs by preprocessing the pattern (`needle`) into an **LPS (Longest Prefix Suffix)** array.
    
**Steps**:
    1. Precompute LPS array for `needle`.
    2. Use it to skip unnecessary comparisons in `haystack`.

#### Example :

```
haystack = "onionionspl"
needle   = "onions"
n = 12, m = 6
```

We compute **Longest Proper Prefix** which is also a suffix for each prefix.

|i|needle[i]|LPS[i]|Explanation|
|---|---|---|---|
|0|o|0|Single char — no prefix/suffix match|
|1|n|0|"on" — no match|
|2|i|0|"oni" — no match|
|3|o|1|"onio" — prefix `"o"` matches suffix `"o"`|
|4|n|2|"onion" — prefix `"on"` matches suffix `"on"`|
|5|s|0|"onions" — no match|

`LPS = [0, 0, 0, 1, 2, 0]`

#### Code :

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        

        int h = haystack.size();
        int n = needle.size();

        vector<int> lps(n);

        int len = 0,i = 1;
        while(i<n){

            if(needle[i]==needle[len]){
                lps[i] = len+1;
                len++;
                i++;
            }else{
                if(len>0)
                len = lps[len-1];
                else
                i++;
            }
        }

        int ans = -1;

        int j = 0;
        i = 0;
        while(i<h){

            if(haystack[i] == needle[j]){
                i++;
                j++;
            }else{
                if(j>0)
                j = lps[j-1];
                else
                i++;
            }

            if(j==n){
                return ans = (i-n);
            }
        }

        return ans;


    }
};
```


> `Time Complexity` : O(n+m) 
> 
> `Space Complexity` : O(m) 

---
