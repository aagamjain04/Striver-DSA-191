[Problem Link](https://leetcode.com/problems/valid-anagram/description/)
### Problem Statement : 

We are given two strings `s` and `t`.  
We need to return:

- `true` if `t` is an **anagram** of `s`
- `false` otherwise
    
**Anagram** → Both strings have the same characters with the same frequency, just in a different order.

**Example 1:**

```
Input: s = "anagram", t = "nagaram"

Output: true
```

### Approach 1 :
- Sorting
- If we sort both strings and they are equal → anagram.


> `Time Complexity` : O(n log n)
> 
> `Space Complexity` : O(1) 


---

###  Approach 2 :

- Counting frequency
- Since we only have lowercase English letters (26 total), we can use a fixed-size array of length 26 to store frequency differences.
- Increment for characters in `s`, decrement for characters in `t`.
- If all counts are zero → anagram.

#### Code :

```cpp
bool isAnagram(string s, string t) {
	int n1 = s.size();
	int n2 = t.size();
	if(n1!=n2)
	return false;

	vector<int> fre(26);
	for(int i=0;i<n1;i++){
		fre[s[i]-'a']++;
		fre[t[i]-'a']--;
	}

	for(int i=0;i<26;i++){
		if(fre[i])
		return false;
	}

	return true;
}
```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(1) 

---

**Follow up:** What if the inputs contain Unicode characters? How would you adapt your solution to such a case?

- **If characters are limited and known** → frequency array is fastest.
- **If characters can be any Unicode symbol** → use a hash map.
- Always check **length first** to quickly reject impossible cases.
- Sorting works but is slower than counting.

---
