[Problem Link](https://www.interviewbit.com/problems/allocate-books/)
### Problem Statement : 

You are given:
- An array `arr[]` of size `n` where `arr[i]` represents the number of pages in the `i-th` book.
- An integer `m` → number of students.

You must allocate books such that:
1. Each student gets **at least one** book.
2. Each book is assigned to **exactly one** student.
3. Allocation is **contiguous** (books cannot be split or rearranged).

**Goal:**  
Minimize the **maximum number of pages** assigned to any student.

If allocation is **not possible**, return `-1`.
## Examples

**Example 1:**

```
Input: arr = [12,34,67,90], m = 2
Output: 113
Explanation: Student 1 : [12,34,67] = 113
Student 2 : [90] = 90
Result = max(113,90) = 113, which is minimum as possible

```

### Approach 1 :

- Brute force approach
- Try **all possible allocations** of books among students.
- Find allocation with **minimum max pages**.
- **Exponential** → not feasible for large `n`.


---

### Approach 2:

- Binary Search
- If we fix a maximum pages limit `X`, we can **check** if allocation is possible:
	  - Assign books to students without exceeding `X` pages per student.
	  - If more than `m` students are needed → allocation not possible.
- Search for **minimum X** using **binary search**.

#### Code :

``` cpp
bool isPossible(vector<int>& arr, int m, int limit) {
    int students = 1, pages = 0;
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] > limit) return false; // can't allocate this book
        if (pages + arr[i] > limit) {
            students++;
            pages = arr[i];
            if (students > m) return false;
        } else {
            pages += arr[i];
        }
    }
    return true;
}

int allocateBooks(vector<int>& arr, int m) {
    int n = arr.size();
    if (m > n) return -1; // not enough books for each student

    int low = *max_element(arr.begin(), arr.end());
    int high = accumulate(arr.begin(), arr.end(), 0);
    int ans = -1;

    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (isPossible(arr, m, mid)) {
            ans = mid;
            high = mid - 1; // try smaller max
        } else {
            low = mid + 1; // increase limit
        }
    }
    return ans;
}
```


> `Time Complexity` : O(n log(sum(arr)))
> 	
> `Space Complexity` : O(1)

---
