### **Heap Overview**
A **heap** is a complete binary tree that satisfies the **heap property**:
- **Min Heap** → The value of each node is **less than or equal to** its children.
    - **Root** contains the **minimum** element.
- **Max Heap** → The value of each node is **greater than or equal to** its children.
    - **Root** contains the **maximum** element.


### Why heaps are used?

- Implementing **priority queues**
- Efficient **min / max element retrieval** (O(1))
- Sorting (Heap Sort: O(n log n))

### **Heap Operations Complexity**

| Operation   | Time Complexity |
| ----------- | --------------- |
| Insertion   | O(log n)        |
| Deletion    | O(log n)        |
| Get Min/Max | O(1)            |
| Heapify     | O(log n)        |
| Build Heap  | O(n)            |
### Heap Representation
Usually stored as an **array**.

For node at index `i`:
- **Parent:** `(i - 1) / 2`
- **Left child:** `2 * i + 1`
- **Right child:** `2 * i + 2`

### Heapify
**Definition:** Process of rearranging a node and its subtree so that it satisfies the heap property.


###  Build Heap in O(n)

#### **Naive Approach**
Insert elements one-by-one → `n × O(log n)` = **O(n log n)**.


#### **Optimal Approach (Bottom-Up Heapify)**
1. Interpret array as a complete binary tree.
2. Start from **last non-leaf node** → index `(n/2 - 1)`.
3. Heapify down each node moving towards root.
#### Why O(n), Not O(n log n)?

- **Most nodes** are near the leaves → height = small → cheap to heapify.
- **Few nodes** are at the top → costly to heapify, but very few in number.

```
**Work per level:**
Height h Nodes Max swaps per node Total work
0 (leaves) ~n/2 O(0) ~0
1 ~n/4 O(1) ~n/4
2 ~n/8 O(2) ~n/4
...
log n 1 O(log n) ~log n


**Summation:**
T(n) = (n/2 × 0) + (n/4 × 1) + (n/8 × 2) + ... + (1 × log n)
T(n) < 2n ⇒ O(n)
```


####  STL provides:

- `priority_queue<int>` → Max Heap
    
- `priority_queue<int, vector<int>, greater<int>>` → Min Heap
#### Code :

``` cpp
#include <bits/stdc++.h>
using namespace std;

int left(int i) {return (2*i+1);}
int right(int i) {return (2*i+2);}
int parent(int i) {return (i-1)/2;}

class MinHeap{

    vector<int> heap;

    void heapifyUp(int i){
        while(i>0 && heap[i]<heap[parent(i)]){
            swap(heap[i],heap[parent(i)]);
            i = parent(i);
        }
    }

    void heapifyDown(int i){
        if(heap.size()==0)
        return;

        int l = left(i);
        int r = right(i);
        int smallest = i;

        if(l<heap.size() && heap[l] < heap[smallest])
        smallest = l;

        if(r<heap.size() && heap[r] < heap[smallest])
        smallest = r;

        if(smallest!=i){
            swap(heap[smallest],heap[i]);
            heapifyDown(smallest);
        }
    }

    public: void push(int val){
        heap.push_back(val);
        heapifyUp(heap.size()-1);
    }

    public: void pop(){
        if(heap.empty())
        return;

        swap(heap[0],heap[heap.size()-1]);
        heap.pop_back();
        heapifyDown(0);
    }

    public: int top(){
        if(heap.size()==0){
            return 0;
        }
        return heap[0];
    }
  

};


int main()
{
    
    MinHeap minHeap;
    minHeap.push(3);
    minHeap.push(1);
    minHeap.push(4);
    minHeap.push(2);
    cout << "Min Heap top: " << minHeap.top() << "\n"; // 1
    minHeap.pop();
    cout << "Min Heap top after pop: " << minHeap.top() << "\n"; // 2


    return 0;
}


```