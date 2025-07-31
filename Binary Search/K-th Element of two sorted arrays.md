[Problem Link](https://www.geeksforgeeks.org/problems/k-th-element-of-two-sorted-array1317/1)
### Problem Statement : 

Given two sorted arrays `a[]` and `b[]` and an element `k`, the task is to find the element that would be at the `kth` position of the combined sorted array.
## Examples

**Example 1:**

```
Input: nums1 = [2,3,6,7,9], nums2 = [1,4,8,10], k = 5
Output: 6
Explanation: merged array = [1,2,3,4,6,7,8,9,10] and kth Element is 6
```

### Approach 1 :

- Naive approach
- Merge both arrays into one sorted array
- Find the `kth` element

> `Time Complexity` : O(m+n)
> 
> `Space Complexity` : O(m+n)

---

### Approach 2:

- We don't need the full merger array.
- Just track the elements until the `kth` position.


> `Time Complexity` : O(n+m)
> 	
> `Space Complexity` : O(1)

---

### Approach 3:

- Similar to [Median of Two Sorted Arrays problem](https://github.com/aagamjain04/Striver-DSA-191/blob/main/Binary%20Search/Median%20of%20Two%20Sorted%20Arrays.md).
- Partition arrays such that:
    - Left partition contains exactly **k** elements.
    - All elements in left partition ≤ all elements in right partition.
- Answer is **max of left partition**.

#### Code :

``` cpp
int kthElement(vector<int>& a, vector<int>& b, int k) {
    if (a.size() > b.size()) return kthElement(b, a, k);

    int n = a.size(), m = b.size();
    int low = max(0, k - m), high = min(k, n);

    while (low <= high) {
        int cut1 = (low + high) / 2;
        int cut2 = k - cut1;

        int l1 = (cut1 == 0) ? INT_MIN : a[cut1 - 1];
        int l2 = (cut2 == 0) ? INT_MIN : b[cut2 - 1];
        int r1 = (cut1 == n) ? INT_MAX : a[cut1];
        int r2 = (cut2 == m) ? INT_MAX : b[cut2];

        if (l1 <= r2 && l2 <= r1) {
            return max(l1, l2);
        }
        else if (l1 > r2) high = cut1 - 1;
        else low = cut1 + 1;
    }
    return -1;
}

```

- Why not `low = 0` and `high = n` directly?
- Lower Bound: `max(0, k - m)`
- Why? Because:
- `b` can give us **at most** `m` elements (its size).
- If `k` is bigger than `m`, then `a` must give at least the extra part: `k - m`.
- **Upper Bound: `min(k, n)`**
- We can take **at most**:
    - `n` elements from **a** (array size limit)
    - `k` elements total (can’t take more than we need for the `k-th` element)

> `Time Complexity` : O(log(min(m,n)))
> 	
> `Space Complexity` : O(1)

---
