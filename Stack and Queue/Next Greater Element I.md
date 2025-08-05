[Problem Link](https://leetcode.com/problems/next-greater-element-i/description/)
### Problem Statement : 

We are given:
- `nums1`: a subset of `nums2`
- For each element in `nums1`, find its **Next Greater Element** in `nums2`.
- If no greater element exists, return `-1`.

**Example 1:**

```
Input: nums1 = [4,1,2], nums2 = [1,3,4,2]
Output: [-1,3,-1]
Explanation: The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.

```

### Approach 1 :

- Brute force
- For each element in `nums1`:
	1. Find its position `j` in `nums2`	    
	2. From position `j+1` in `nums2`, move right until you find a greater element.
	3. If found → store it; else → store `-1`.

> `Time Complexity` : O(1) -> ignoring answer array
> 
> `Space Complexity` : O(n * m)


---

### Approach 2 :

### Step 1: Precompute Next Greater for `nums2` using a Stack
- Use a **monotonic decreasing stack**.
- Traverse `nums2` **from right to left**:
  - While the stack is not empty and `stack.top() <= current`, pop from stack.
  - If stack is empty → no greater element → store `-1`.
  - Else → `stack.top()` is the next greater element.
  - Push current element onto stack.

### Step 2: Build Result for `nums1`
- Store precomputed results in a hash map `{element → next greater}`.
- For each element in `nums1`, look up in the map.

#### Code :

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        
        int n = nums1.size();
        vector<int> ans(n,-1);

        unordered_map<int,int> mp;
        for(int i=0;i<n;i++){
            mp[nums1[i]] = i;
        }
        stack<int> st;

        for(int i=0;i<nums2.size();i++){
            int curr = nums2[i];

            while(!st.empty() && st.top()<=curr){
                int num = st.top();
                st.pop();
                if(mp.find(num)!=mp.end()){
                    int idx = mp[num];
                    ans[idx] = curr;
                }
            }
            st.push(curr);
        }
        return ans;
    }
};
```


> `Time Complexity` : O(n) -> stack and map
> 
> `Space Complexity` : O(n + m)

---
