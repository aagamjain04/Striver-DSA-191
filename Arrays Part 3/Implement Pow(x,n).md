
[Problem Link](https://leetcode.com/problems/powx-n/description/)

### Problem Statement : 
Given a double x and integer n, calculate x raised to power n. Basically Implement pow(x, n).

`-2^31 <= n <= 2^31-1`

### Approach 1 :

- Initialize `res = 1`
- Iterate over n times and multiply `res = res * x`, n times

> **Time Complexity**: O(n) - n multiplications
> 
> **Space Complexity**: O(1)

---


### Approach 2 :

### Binary Exponentiation

Example: Calculate 3^9


Breaking down the calculation:

```
3^9 = (3) × 3^8 → (3^2)^4 → (9)^4 → (9^2)^2 → 81^2 → 6561^1 → (65651)
```

```
res = 1 * 3 * 6561
```
### Algorithm Steps:

1. When power is odd:
    - Multiply result with x
2. Keep dividing n by 2 until n > 0:
3. Keep multiplying x with itself

### Implementation

```cpp
class Solution {
public:
   double myPow(double x, int n) {

        long b = n;
        if(b<0)
        b = b * -1;
        
        double a = x;
        double res = 1;
        while(b>0)
        {
            if(b&1){
                res = res * a;
            }
            a = a*a;
            b = b>>1;
        }

        if(n>0)
        return res;
        else {
            return (double(1.0)/res);
        }
        
    }
};
```



>**Time Complexity**: O(log n) - We divide the power by 2 in each step
>
> **Space Complexity**: O(1) - Only using a constant amount of extra space

---
