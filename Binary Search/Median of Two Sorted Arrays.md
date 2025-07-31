[Problem Link](https://leetcode.com/problems/median-of-two-sorted-arrays/description/)
### Problem Statement : 

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.
## Examples

**Example 1:**

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

### Approach 1 :

- Naive approach
- Merge both arrays into one sorted array
- Find the middle element(s)

> `Time Complexity` : O(m+n)
> 
> `Space Complexity` : O(m+n)

---

### Approach 2:

- We don't need the full merger array.
- Just track the elements until the median position.

#### Code :

``` cpp
double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
	
	int n = nums1.size();
	int m = nums2.size();

	int total = (m+n)/2;
	int count = 0;

	int median1 = (m+n-1)/2;
	int median2 = (m+n)/2;
	// In case of odd : median1 == median2

	int p1 = 0, p2 = 0;
	int v1 = 0, v2 = 0; 

	while(count <= total){
		int val = 0;
		if(p1==n){
			val = nums2[p2++];
		}else if(p2==m){
			val = nums1[p1++];
		}else if(nums1[p1] <= nums2[p2]){
			val = nums1[p1++];
		}else{
			val = nums2[p2++];
		}

		if(count == median1)
		v1 = val;
		if(count == median2)
		v2 = val;

		count++;
	}

	return double(v1+v2)/2.0;

}

```



> `Time Complexity` : O(n+m)
> 	
> `Space Complexity` : O(1)

---

### Approach 3:

- Use **binary search** on the smaller array.
- Partition both arrays so that:
    - All elements on the left partition ≤ all elements on the right partition.
    - Left side contains exactly half of total elements.
- Median comes from **max(left) & min(right)**.
- Partion :

```
cut1 = (low+high) / 2
cut2 = (m+n+1)/2 - cut1
```
- `l1 = nums1[cut1-1]`, `l2 = nums2[cut2-1]` , `r1 = nums1[cut1]` , `r2 = nums2[cut2]`
- If `l1 <= r2 && l2 <= r1` → valid partition.
    - If odd total → median = max(l1, l2)
    - Else median = (max(l1, l2) + min(r1, r2)) / 2.0
- Else adjust `low` or `high`.

#### Code :

``` cpp
double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    if (nums1.size() > nums2.size()) return findMedianSortedArrays(nums2, nums1);

    int m = nums1.size(), n = nums2.size();
    int low = 0, high = m;

    while (low <= high) {
        int cut1 = (low + high) / 2;
        int cut2 = (m + n + 1) / 2 - cut1;

        int l1 = (cut1 == 0) ? INT_MIN : nums1[cut1 - 1];
        int l2 = (cut2 == 0) ? INT_MIN : nums2[cut2 - 1];
        int r1 = (cut1 == m) ? INT_MAX : nums1[cut1];
        int r2 = (cut2 == n) ? INT_MAX : nums2[cut2];

        if (l1 <= r2 && l2 <= r1) {
            if ((m + n) % 2 == 0)
                return (max(l1, l2) + min(r1, r2)) / 2.0;
            else
                return max(l1, l2);
        }
        else if (l1 > r2) high = cut1 - 1;
        else low = cut1 + 1;
    }
    return 0.0;
}
```

> `Time Complexity` : O(log(min(m,n)))
> 	
> `Space Complexity` : O(1)

---
