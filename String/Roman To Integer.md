[Problem Link](https://leetcode.com/problems/roman-to-integer/description/)
### Problem Statement : 

Given a string `s`, return the longest palindromic substring  in `s`.

---
## Roman Numeral Rules

**Symbols and Values:**

| Symbol | Value |
| ------ | ----- |
| I      | 1     |
| V      | 5     |
| X      | 10    |
| L      | 50    |
| C      | 100   |
| D      | 500   |
| M      | 1000  |

---

**Subtractive Notation Rules:**

- `I` before `V` → 4 (`IV`), `I` before `X` → 9 (`IX`)
- `X` before `L` → 40 (`XL`), `X` before `C` → 90 (`XC`)
- `C` before `D` → 400 (`CD`), `C` before `M` → 900 (`CM`)

If a smaller value comes **before** a larger one → **subtract** it, else **add** it.

---

### Approach :
### 1. Map Roman symbols to values
- Store values in an `unordered_map<char, int>` for O(1) lookup.
### 2. Scan the string

- For each character:
    - If current value `<` next value → subtract.
    - Else → add.
### 3. Return sum

---
#### Code :


``` cpp
int romanToInt(string s) {
	
	 unordered_map<char, int> value = {
        {'I', 1}, {'V', 5}, {'X', 10}, {'L', 50},
        {'C', 100}, {'D', 500}, {'M', 1000}
    };


	int n = s.size();
	int res = 0;
	int i = 0;
	while(i<n){

		int x = mp[s[i]];
		int y = 0;
		if(y<n)
		y = mp[s[i+1]];

		if(x>=y){
			res = res + x;
			i++;
		}else{
			res = res - x + y;
			i+=2; 
		}
	}
	return res;
}
```

> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1) 

---