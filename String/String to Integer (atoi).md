[Problem Link](https://leetcode.com/problems/string-to-integer-atoi/description/)
### Problem Statement : 

Implement the `myAtoi(string s)` function, which converts a string to a 32-bit signed integer.

The algorithm for `myAtoi(string s)` is as follows:

1. **Whitespace**: Ignore any leading whitespace (`" "`).
2. **Signedness**: Determine the sign by checking if the next character is `'-'` or `'+'`, assuming positivity if neither present.
3. **Conversion**: Read the integer by skipping leading zeros until a non-digit character is encountered or the end of the string is reached. If no digits were read, then the result is 0.
4. **Rounding**: If the integer is out of the 32-bit signed integer range `[-231, 231 - 1]`, then round the integer to remain in the range. Specifically, integers less than `-231` should be rounded to `-231`, and integers greater than `231 - 1` should be rounded to `231 - 1`.

Return the integer as the final result.

**Example 1:**

```
Input: s = "42"
Output: 42
```

**Example 2:**

```
Input: s = " -042"
Output: -42
```

Example 1:**

```
Input: s = "1337c0d3"
Output: 1337
```

### Approach 1 :

#### Rules

1. **Ignore Leading Whitespace**
    - Skip all `' '` characters until a non-space is found.
2. **Determine Sign**
    - If next char is `'-'`, result is negative.
    - If `'+'`, result is positive.
    - If neither, assume positive.
3. **Read Digits**
    - Skip leading zeros (optional optimization).
    - Read consecutive digits until:
        - A non-digit is encountered, or
        - End of string.
4. **Stop at First Non-Digit After Digits**
    - Ignore the rest.
5. **Clamp to 32-bit Integer Range**
    - Min: `-2^31` = **-2147483648**
    - Max: `2^31 - 1` = **2147483647**

- Initialize `i = 0` and `n = s.size()`.
- **Skip spaces**.
- **Check sign** and store in `sign` variable.
- Initialize `result = 0`.
- For each digit:
    - Check for overflow **before** multiplying by 10:
        - If `result > INT_MAX / 10` or  
            `result == INT_MAX / 10 && digit > 7` (for positive) → return `INT_MAX`.
        - If `result > INT_MAX / 10` or  
            `result == INT_MAX / 10 && digit > 8` (for negative) → return `INT_MIN`.    
    - Update `result = result * 10 + digit`.
- Return `sign * result`.

#### Code :


``` cpp
int myAtoi(string s) {

	int n = s.size();
	int sign = 1, base = 0, digit = 0;

	int i = 0;

	while(i<n && s[i]==' ')
	i++;

	if(i<n && s[i]=='-' || s[i]=='+'){
		sign = (s[i]=='-' ? -1 : 1);
		i++;
	}
  

	while(s[i]>='0' && s[i]<='9'){

		digit = s[i] - '0';

		if(base > INT_MAX/10 || (base==INT_MAX/10 && digit>7)){

			return (sign==1 ? INT_MAX : INT_MIN);
		}
		base = base * 10 + digit;
		i++;
	}

	return (base*sign);
	
}
```

> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1) 

---
