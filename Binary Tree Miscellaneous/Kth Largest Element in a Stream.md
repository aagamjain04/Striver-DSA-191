[Problem Link](https://leetcode.com/problems/kth-largest-element-in-a-stream/description/)
### Problem Statement : 

We want to track the **kth highest test score** among applicants in real-time:
- When initialized with `k` and an initial list of scores, we must prepare the data structure.
- When a new score is added, return the **kth largest score** so far.
    
This helps dynamically maintain the admissions cut-off.

**Example 1:**

```
k = 3, nums = [4, 5, 8, 2]
Start: Heap stores [4,5,8] → top = 4 (3rd largest).
Add 3 → Heap [4,5,8] (3 < 4, ignored), kth largest = 4.
Add 10 → Heap [5,8,10], kth largest = 5.
Add 9 → Heap [8,10,9], kth largest = 8.
Add 4 → Heap [8,10,9], kth largest = 8.
```

`

---


###  Approach 1 :

- Maintain a **min-heap of size k**.    
    - The heap always contains the **top k largest elements** seen so far.
    - The **root of the heap (top element)** = kth largest.
    - Insertion complexity: **O(log k)**.
    - Min-heap lets us discard elements smaller than the current top k.
    - kth largest is always at the root.

#### Code :

```cpp
class KthLargest {
public:

    priority_queue<int,vector<int>,greater<int>> pq;
    int K;
    KthLargest(int k, vector<int>& nums) {
        K = k;
        for(auto &i:nums){

            add(i);
        }
    }
    
    int add(int val) {
        if(pq.size()<K){
            pq.push(val);
        }else{
            if(pq.top()<val){
                pq.pop();
                pq.push(val);
            }   
        }
        return pq.top();
    }
};

```


> `Time Complexity` : O(log k) per insertion
> 
> `Space Complexity` : O(k) (heap stores only for k elements)

---

### Approach 2 :

- Balanced BST
- Use a multiset (self-balancing BST, ordered container).
- Always maintain the **k largest elements** inside the multiset.
- The smallest element in the set = kth largest.
    
#### Operations
- Insert new element into set.
- If size exceeds `k`, remove the smallest element.
- The smallest element in set = kth largest.

#### Code :

```cpp
class KthLargest {
public:
    multiset<int> s; // maintains k largest elements
    int K;

    KthLargest(int k, vector<int>& nums) {
        K = k;
        for (int num : nums) {
            add(num);
        }
    }
    
    int add(int val) {
        s.insert(val);
        if (s.size() > K) {
            s.erase(s.begin()); // remove smallest element
        }
        return *s.begin(); // kth largest
    }
};

```

> `Time Complexity` : O(log k) per insertion
> 
> `Space Complexity` : O(k)

---
