[Problem Link](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

### Problem Statement : 
Given a string `s`, find the length of the **longest** **substring** without duplicate characters.
## Example

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 1:**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

### Approach 1 :

- Brute force
- Iterate over all subarrays
- Dynamically maintain a set to track characters in substring
- If a character is already in set store the max length and break out

#### Code:
```cpp
int lengthOfLongestSubstring(string str) {
        
	int n = str.size();
	int ans = 0;

	for(int i=0;i<n;i++){
		set<char> s;
		for(int j=i;j<n;j++){
			if(s.find(str[j])==s.end()){
				s.insert(str[j]);
			}else{
				ans = max(ans,(int)s.size());
				break; 
			}
		}
	}

	return ans;
}
```

> `Time Complexity` : O(n²) × O(log k) ≈ **O(n²)** since k is bounded by alphabet size
> 
> `Space Complexity` : O(k) ≈ O(1) since k is bounded by alphabet size

---

### Approach 2 :

- Sliding window and hash-map
- Use a constant size of 256 frequency array
- Iterate over the string with end pointer
	- if `freq[end]` is present and its position is within the window make `start = freq[end]+1`
	- else update `freq` array
- `ans = max(ans,end-start+1)`

**Code** :

``` cpp
 int lengthOfLongestSubstring(string s) {
	int n = s.size();
	if(n==0)
	return 0;
   
	vector<int> fre(256,-1);
	int ans = 0;
	int start = 0;
	for(int end=0;end<n;end++){
		int lastSeen = fre[s[end]];

		if(lastSeen!=-1 && lastSeen>=start){
			start = lastSeen+1;
		}
		ans = max(ans,end-start+1);

		fre[s[end]] = end;
	}


	return ans;
}
```


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1) assuming frequency array of constant size

---
