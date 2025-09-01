[Problem Link](https://www.interviewbit.com/problems/distinct-numbers-in-window/)
### Problem Statement : 

You are given an array of `N` integers `A1, A2, ..., AN` and an integer `B`.  
Return an array of size `N - B + 1` where the `i`-th element contains the number of **distinct elements** in the subarray:

**Example 1:**

```
Input : A = [1, 2, 1, 3, 4, 2, 3]
B = 4

Output : [3, 4, 4, 3]

```

`

---


###  Approach 1 :

### Naive Approach (O(N * B))
- For each window, insert elements into a set to count distinct numbers.
- Time complexity: **O(N * B)**


---

### Approach 2 :

1. Use a **sliding window** with a frequency map (`unordered_map<int,int>`).
2. Process the **first window**:
    - Count frequency of elements and get distinct count.
3. Slide the window across the array:
    - **Remove** contribution of outgoing element (decrease its frequency, erase if freq = 0).
    - **Add** contribution of incoming element (increase its frequency).
    - Record distinct count at each step.
        
This ensures each element is processed only twice (enter + exit window).

#### Code :

```cpp
vector<int> distinctNumbersInWindow(vector<int>& A, int B) {
    int N = A.size();
    vector<int> result;
    
    if (B > N) return result; // empty if window size > array size

    unordered_map<int, int> freq;
    int distinctCount = 0;

    // First window
    for (int i = 0; i < B; i++) {
        if (freq[A[i]] == 0) distinctCount++;
        freq[A[i]]++;
    }
    result.push_back(distinctCount);

    // Remaining windows
    for (int i = B; i < N; i++) {
        // Remove outgoing element
        freq[A[i - B]]--;
        if (freq[A[i - B]] == 0) distinctCount--;

        // Add incoming element
        if (freq[A[i]] == 0) distinctCount++;
        freq[A[i]]++;

        result.push_back(distinctCount);
    }

    return result;
}


```


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(B) for the frequency map

---

### Follow-Up: Return Distinct Elements (Not Just Count)

#### Using `unordered_map + ordered set`
- Maintain:
    - A frequency map (`unordered_map<int, int>`)
    - A balanced BST / `set<int>` to track which numbers are currently present.
        
- For each window:
    - Remove outgoing element (if frequency becomes 0, erase from set).
    - Add incoming element (insert into set).
        
- Now, the set directly stores **distinct elements in sorted order**.

This gives:

- Count of distinct → `set.size()`
- Distinct elements themselves → iterate over `set`.

#### Complexity Analysis

- **Time Complexity**:
    - O(N log B) → log B from `set` insertion/deletion.
- **Space Complexity**:
    - O(B) for `freq` + `set`.
---

