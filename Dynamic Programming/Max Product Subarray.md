[Problem Link](https://leetcode.com/problems/maximum-product-subarray/description/)
### Problem Statement : 

Given an integer array `nums`, find a subarray that has the largest product, and return _the product_.

The test cases are generated so that the answer will fit in a **32-bit** integer.

### Example :

```
Input: nums = [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.

Input: nums = [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.

```


---


### Approach 1 :

- Naive approach
- The idea is to traverse over every contiguous subarray find the product of each of these subarrays and return the maximum product among all the subarrays.



> `Time Complexity` : O(n^2)
> 
> `Space Complexity` : O(1)

---

### Approach 2 :

- Unlike the maximum subarray sum problem (Kadane’s algorithm), this problem involves multiplication, where **negative numbers** and **zeros** complicate things.
- A negative number can flip the maximum into a minimum and vice versa.
- Hence, at each step, we need to keep track of:
    - `maxProd` → the maximum product ending at the current index.
    - `minProd` → the minimum product ending at the current index (because multiplying a negative with min might give the next maximum).

- Initialize:
    - `maxProd = nums[0]`
    - `minProd = nums[0]`
    - `ans = nums[0]`
        
- For each element `nums[i]` (from index 1 onwards):
    - If `nums[i]` is negative, swap `maxProd` and `minProd`.
    - Update:
        - `maxProd = max(nums[i], maxProd * nums[i])`
        - `minProd = min(nums[i], minProd * nums[i])`
    - Update result: `ans = max(ans, maxProd)`
- Return `ans`.

Now, when we multiply by a **negative number**:
1. A **large positive product** (`maxProd`) becomes **large negative**.  
    → It might become the new **minimum**.
2. A **large negative product** (`minProd`) becomes **large positive**.  
    → It might become the new **maximum**.
    
So effectively, **`maxProd` and `minProd` flip roles** when multiplied by a negative number.  
That’s why we **swap them before updating**.


### Code :

``` cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        int maxProd = nums[0], minProd = nums[0], ans = nums[0];
        
        for (int i = 1; i < n; i++) {
            if (nums[i] < 0) swap(maxProd, minProd);  // handle negative
            maxProd = max(nums[i], maxProd * nums[i]);
            minProd = min(nums[i], minProd * nums[i]);
            ans = max(ans, maxProd);
        }
        
        return ans;
    }
};
```

> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)


---

### Approach 3 :

- Two loops
- Observation is is there are all positive, even negatives non zero numbers then our answer is whole array.
- In case of odd negatives, lets there is one negative.
- Answer will me either left products or right products.
- So effectively, run two loops forward and backward and keep track of maxProduct. In case of zero start again.


#### Code :
``` cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();

        int ans = nums[0];
        int p = 1;

        for(int i=0;i<n;i++)
        {
            p*=nums[i];
            ans = max(ans,p);
            if(p==0)
            {
                p=1;
            }
        }
        p=1;
        for(int i=n-1;i>=0;i--)
        {
            p*=nums[i];
            ans = max(ans,p);
            if(p==0)
            {
                p=1;
            }
        }

        return ans;

    }
};

```

> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)

---

