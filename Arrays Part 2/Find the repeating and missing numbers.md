[Problem Link](https://www.geeksforgeeks.org/find-a-repeating-and-a-missing-number/)

### Problem Statement : 
You are given a read-only array of N integers with values in the range [1, N] both inclusive. Each integer appears exactly once except:

- Number A which appears twice (repeating number)
- Number B which is missing

The task is to find both the repeating number A and the missing number B.

## Example

**Example 1:**

```
Input: nums = [4,3,6,2,1,1]
Output: 1,5
Explanation: 5 is missing and 1 is repeating
```


### Approach 1 :

- Brute force
- Maintain a frequency array from 1 to N
- The index with frequency 2 is our repeating number
- The index with frequency 0 is our missing number


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(n)

---

### Approach 2:

- Mathematical
```
let x = repeating
let y = missing

S = [x+x+....N]
Sn = [1+2+x+y...N]

S - Sn = x - y   --- (1)
```

```

S2 = [x² + x² + ....N²]
S2n = [1² + 2² + 3² ... + x² + y² ...N²]

S2 - S2n = x² - y²  --- (2)

```


We know that :
```
Sn = n(n+1)/2

S2n = n(n+1)(2n+1)/6
```


```
From (1) and (2)

S2 - S2n = (x-y)(x+y)

x+y = x-y / s2-s2n

x+y = s-sn / s2-s2n --- (3)

Substitute values in (1) and (3) and get x and y

```

> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)

---