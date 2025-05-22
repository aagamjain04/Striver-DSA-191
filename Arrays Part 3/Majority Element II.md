
[Problem Link](https://leetcode.com/problems/majority-element-ii/description/)

### Problem Statement : 
Given an integer array of size `n`, find all elements that appear more than `⌊ n/3 ⌋` times.

## Example

**Example 1:**

```
Input : nums = [3,2,3]
Output : [3]
```

**Example 2:**

```
Input : nums = [1]
Output : [1]
```

**Example 3:**

```
Input : nums = [1,2]
Output : [1,2]
```
### Approach 1 : 

- Brute force
- Take one element and check its count in the array. Whichever has `count > N/3` return that element.

> **Time Complexity**: O(N²)
> 
> **Space Complexity**: O(1)

---

### Approach 2 :

- Use hash-map to store the count and whenever `frequency > N/3`, return that element.

> **Time Complexity**: O(N)
> 
> **Space Complexity**: O(N)

---

### Approach 3 :

#### Moore's Voting Element

- Extend Moore's voting algorithm and initialize four variables :
- `2 count`
- `2 majorityEle`


### Implementation

```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        vector<int> res;
        int n = nums.size();
        int count1 = 0, count2 = 0;
        int majorityEle1 = 1e9+1, majorityEle2 = 1e9+1;

        for(int i=0;i<n;i++){
            if(nums[i]==majorityEle1)
            count1++;
            else if(nums[i]==majorityEle2)
            count2++;
            else if(count1==0){
                majorityEle1 = nums[i];
                count1++;
            }
            else if(count2==0){
                majorityEle2 = nums[i];
                count2++;
            }
            else {
                count1--;
                count2--;
            }

        }

        count1 = 0;
        count2 = 0;

        for(int i=0;i<n;i++){
            if(nums[i]==majorityEle1)
            count1++;
            if(nums[i]==majorityEle2)
            count2++;
        }

        if(count1>n/3)
        res.push_back(majorityEle1);
        if(count2>n/3)
        res.push_back(majorityEle2);

        return res;
    }
};
```



>**Time Complexity**: O(N) 
>
> **Space Complexity**: O(1)

---
