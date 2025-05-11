

[Problem Link](https://leetcode.com/problems/merge-sorted-array/description/)

## Problem Statement

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be _stored inside the array_ `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

## Examples

**Example 1:**

```
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6].
```

**Example 2:**

```
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].
```

**Example 3:**

```
Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
```

## Approach 1: Extra Space

1. Create a new array of size `m + n`
2. Merge both arrays into this new array using two pointers
3. Copy the new array back to `nums1`


>**Time Complexity**: O(m + n)  
>
>**Space Complexity**: O(m + n)

---
## Approach 2: In-place Merge (Optimal)

Start filling `nums1` from the end:

1. Initialize three pointers:
    - `p1`: points to the last valid element in `nums1` (m-1)
    - `p2`: points to the last element in `nums2` (n-1)
    - `p`: points to the last position in the merged array (m+n-1)
2. Compare elements at `p1` and `p2`, place the larger one at position `p`
3. Decrement the pointers accordingly
4. When `p2` is exhausted, we're done
5. When `p1` is exhausted, copy remaining elements from `nums2`

```cpp
void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
    int p1 = m - 1;    // Last element in nums1
    int p2 = n - 1;    // Last element in nums2
    int p = m + n - 1; // Last position in the merged array
    
    // Fill nums1 from the end
    while (p1 >= 0 && p2 >= 0) {
        if (nums1[p1] > nums2[p2]) {
            nums1[p--] = nums1[p1--];
        } else {
            nums1[p--] = nums2[p2--];
        }
    }
    
    // If there are remaining elements in nums2, copy them
    while (p2 >= 0) {
        nums1[p--] = nums2[p2--];
    }
    
    // Note: No need to handle remaining elements in nums1 as they're already in place
}
```

> **Time Complexity**: O(m + n)  
> 
> **Space Complexity**: O(1)



## Visual Example

### Example 1: First Case

For inputs:

```
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6], n = 3
```

In-place merge process (working from the end):

```
i = 2, j = 2, k = 5
[1,2,3,0,0,0] [2,5,6]
       ^       ^
Compare nums1[i]=3 with nums2[j]=6
6 > 3, so place 6 at position k
[1,2,3,0,0,6] [2,5,6]
           k
i = 2, j = 1, k = 4

Compare nums1[i]=3 with nums2[j]=5
5 > 3, so place 5 at position k
[1,2,3,0,5,6] [2,5,6]
         k
i = 2, j = 0, k = 3

Compare nums1[i]=3 with nums2[j]=2
3 > 2, so place 3 at position k
[1,2,3,3,5,6] [2,5,6]
       k
i = 1, j = 0, k = 2

Compare nums1[i]=2 with nums2[j]=2
2 = 2, so place 2 at position k (in this case, we choose nums2[j])
[1,2,2,3,5,6] [2,5,6]
     k
i = 1, j = -1, k = 1

Since j < 0, we've placed all elements from nums2.
The remaining elements in nums1 are already in place.
Final result: [1,2,2,3,5,6]
```

### Example 2: Another Case

For inputs:

```
nums1 = [4,5,6,0,0,0], m = 3
nums2 = [1,2,3], n = 3
```

In-place merge process:

```
i = 2, j = 2, k = 5
[4,5,6,0,0,0] [1,2,3]
     ^         ^
Compare nums1[i]=6 with nums2[j]=3
6 > 3, so place 6 at position k
[4,5,6,0,0,6] [1,2,3]
           k
i = 1, j = 2, k = 4

Compare nums1[i]=5 with nums2[j]=3
5 > 3, so place 5 at position k
[4,5,6,0,5,6] [1,2,3]
         k
i = 0, j = 2, k = 3

Compare nums1[i]=4 with nums2[j]=3
4 > 3, so place 4 at position k
[4,5,6,4,5,6] [1,2,3]
       k
i = -1, j = 2, k = 2

Since i < 0, we've placed all elements from nums1.
Now we need to copy remaining elements from nums2:
Copy nums2[2]=3 to nums1[2]
[4,5,3,4,5,6] [1,2,3]
     k
j = 1, k = 1

Copy nums2[1]=2 to nums1[1]
[4,2,3,4,5,6] [1,2,3]
   k
j = 0, k = 0

Copy nums2[0]=1 to nums1[0]
[1,2,3,4,5,6] [1,2,3]
 k
j = -1, k = -1

Final result: [1,2,3,4,5,6]
```

When j becomes negative, it means all elements from nums2 have been placed and the merge is complete.