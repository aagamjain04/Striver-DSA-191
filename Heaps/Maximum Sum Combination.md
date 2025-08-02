[Problem Link](https://www.geeksforgeeks.org/problems/maximum-sum-combination/1)
### Problem Statement : 

Given two integer arrays `a[]` and `b[]` of size `n`:
- Form sum combinations: one element from `a`, one from `b`
- Each pair `(i, j)` can be used at most once
- Find the top `k` maximum sum combinations in non-increasing order
## Examples

**Example 1:**

```
Input: a[] = [1,4,2,3], b[] = [2,5,1,6], k = 2
Output: [10,9,9]
Explanation: The top 3 maximum possible sums are : 4 + 6 = 10, 3 + 6 = 9, and 4 + 5 = 9
```

### Approach 1 :

- Brute force
- Generate all `n²` sums
- Sort and take top `k`


> `Time Complexity` : O(n² log n)
> 	
> `Space Complexity` : O(n²)

---

### Approach 2:

- Generate all possible pairs
- A min-heap of size `k` is used to maintain the top `k` maximum sums. 
- For every new pair sum, we insert it into the heap if it’s among the largest `k` seen so far. 
- Finally, we extract and reverse the heap to return the top `k` sums in descending order.

#### Code :

``` cpp
vector<int> kMaxSumCombinations(vector<int> &a, vector<int> &b, int k) {
int n = a.size();
priority_queue<int, vector<int>, greater<int>> minHeap; // min-heap

for (int i = 0; i < n; i++) {
	for (int j = 0; j < n; j++) {
		int sum = a[i] + b[j];

		if (minHeap.size() < k) {
			minHeap.push(sum);
		} else if (sum > minHeap.top()) {
			minHeap.pop();
			minHeap.push(sum);
		}
	}
}

// Extract results from heap
vector<int> result;
while (!minHeap.empty()) {
	result.push_back(minHeap.top());
	minHeap.pop();
}

// Reverse to get non-increasing order
reverse(result.begin(), result.end());
return result;
}
```



> `Time Complexity` : O(n² x log k)
> 	
> `Space Complexity` : O(k)

---

### Approach 3:

- We can **avoid generating all n² sums** by:
1. Sort `a` and `b` in **descending order**.
2. Use a **max heap** to always pick the largest unvisited sum.
3. Keep track of visited `(i, j)` pairs in a `set` to avoid duplicates.
4. Initially push `(a[0] + b[0], 0, 0)` into max heap.
5. Pop the max, then push:
    - `(i+1, j)` if not visited.
    - `(i, j+1)` if not visited.
6. Repeat until we get `k` sums.

##### Why This Works

- The largest possible sum is `(a[0] + b[0])`.
- The next candidates will be:
    - `(a[1] + b[0])` (decrease `a` index)
    - `(a[0] + b[1])` (decrease `b` index)
- This ensures we **only explore promising sums**.

#### Code :

``` cpp
vector<int> kMaxSumCombinations(vector<int> &a, vector<int> &b, int k) {
    int n = a.size();
    sort(a.begin(), a.end(), greater<int>());
    sort(b.begin(), b.end(), greater<int>());

    priority_queue<pair<int, pair<int, int>>> pq; // {sum, {i, j}}
    set<pair<int, int>> visited;

    pq.push({a[0] + b[0], {0, 0}});
    visited.insert({0, 0});

    vector<int> result;
    while (k-- && !pq.empty()) {
        auto top = pq.top();
        pq.pop();

        int sum = top.first;
        int i = top.second.first;
        int j = top.second.second;

        result.push_back(sum);

        if (i + 1 < n && !visited.count({i + 1, j})) {
            pq.push({a[i + 1] + b[j], {i + 1, j}});
            visited.insert({i + 1, j});
        }

        if (j + 1 < n && !visited.count({i, j + 1})) {
            pq.push({a[i] + b[j + 1], {i, j + 1}});
            visited.insert({i, j + 1});
        }
    }

    return result;
}

```

> `Time Complexity Average` : O(n log n + k log k)
> 	
> `Space Complexity` : O(k)

---
