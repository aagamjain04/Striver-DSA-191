[Problem Link](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)
### Problem Statement : 

We are given:
- A string `s` with only lowercase English letters.
- We can **only add characters at the front** of the string.
- We need to find the **minimum number of characters** to add to make `s` a palindrome.

**Example 1:**

```
Input: s = "abc"  
Output: 2  
Explanation: We can make above string palindrome as "cbabc", by adding 'b' and 'c' at front.

Input: s = "aacecaaaa"  
Output: 2  
Explanation: We can make above string palindrome as "aaaacecaaaa" by adding two a's at front of string.
```

### Approach 1 :
- Brute force
- Check if the whole string is a palindrome.
- If not, remove the last character and check again until you find the longest palindromic prefix.
- The number of characters removed = number of characters to add.


> `Time Complexity` : O(n²)
> 
> `Space Complexity` : O(1) 


---

###  Approach 2 :

- We can solve this using **KMP’s LPS (Longest Prefix Suffix)** array:
1. Construct a new string:  
    `temp = s + "#" + reverse(s)`  
    (`#` is a delimiter to avoid overlap confusion)
2. Compute the **LPS array** for `temp`:
    - `lps[i]` gives the length of the longest prefix of `temp` which is also a suffix.
3. `lps[lastIndex]` = length of **longest palindromic prefix** of `s`.
4. Answer = `n - lps[lastIndex]`.

#### **Why KMP Works Here**

- The LPS table for `s + "#" + reverse(s)` effectively finds the **longest prefix of `s` that matches a suffix of `reverse(s)`**, which corresponds to the longest palindromic prefix in `s`.
#### Example :

```
s = "aacecaaa"
reverse(s) = "aaacecaa"
temp = "aacecaaa#aaacecaa"
LPS of temp = [0, 1, 0, 0, 0, 1, 2, 2, 0, 1, 2, 2, 3, 4, 5, 6, 7]
lps[lastIndex] = 7
n = 8
Answer = 8 - 7 = 1

```


#### Code :

```cpp
int minChar(string &s) {
	// code here
	
	int N = s.size();
	string rev = s;
	reverse(rev.begin(),rev.end());
	
	s = s + "$" + rev;
	
	int n = s.size();
	vector<int> lps(n,0);
	
	int i = 1, len = 0;
	
	while(i<n){
		
		if(s[i]==s[len]){
			lps[i] = len+1;
			i++;
			len++;
		}else{
			if(len>0)
			len = lps[len-1];
			else i++;
			
		}
	}

	return N-lps.back();

	
}
```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(n) 

---
