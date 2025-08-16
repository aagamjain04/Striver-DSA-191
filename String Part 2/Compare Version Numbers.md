[Problem Link](https://leetcode.com/problems/compare-version-numbers/description/)
### Problem Statement : 

Given two **version strings**, `version1` and `version2`, compare them. A version string consists of **revisions** separated by dots `'.'`. The **value of the revision** is its **integer conversion** ignoring leading zeros.

To compare version strings, compare their revision values in **left-to-right order**. If one of the version strings has fewer revisions, treat the missing revision values as `0`.

Return the following:

- If `version1 < version2`, return -1.
- If `version1 > version2`, return 1.
- Otherwise, return 0.

**Example 1:**

```
version1 = "1.01", version2 = "1.001" → 0
version1 = "1.0", version2 = "1.0.0" → 0
version1 = "0.1", version2 = "1.1"   → -1
version1 = "1.2", version2 = "1.10"  → -1

```

---

###  Approach 1 :

- **Split both version strings** using `.`.
- Convert each revision to an **integer** (ignoring leading zeros automatically).
- . Compare revisions index by index:
    - If one version runs out of parts, assume missing part = `0`.    
    - If a revision differs → return result immediately.
 - If all revisions are equal → return `0`.

#### Code :

```cpp
int compareVersion(string version1, string version2) {

	int n1 = version1.size();
	int n2 = version2.size();

	int i = 0 , j = 0;

	while(i<n1 || j<n2){
		long base1 = 0 , base2 = 0;

		while(i<n1 && version1[i]!='.'){
			base1 = base1*10 + (version1[i]-'0');
			i++;
		}

		 while(j<n2 && version2[j]!='.'){
			base2 = base2*10 + (version2[j]-'0');
			j++;
		}

		if(base1<base2)
		return -1;
		if(base1>base2)
		return 1;

		i++;
		j++;

	}

	return 0;
	
}
```


> `Time Complexity` : `O(max(n1 n2))`
> 
> `Space Complexity` : O(1) 

---

