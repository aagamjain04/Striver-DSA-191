
## Overview

- A **regular BST** can degrade to a **linked list** in the worst case (O(N) height) when data is skewed (sorted input).
- To guarantee efficient operations, we need a **balanced tree** where height is O(log N).
- **Self-balancing trees** automatically adjust their shape after insertions/deletions.

---

## Properties

- A BST is **balanced** if the height of left and right subtrees of any node differ by at most a constant factor.
    
- Ensures:
    
    - **Search:** O(log N)
    - **Insert:** O(log N)
    - **Delete:** O(log N)

---

### 1. **AVL Tree**

- **Balance Factor** = `height(left) - height(right)` ∈ { -1, 0, 1 }.
- After insert/delete → rotations fix imbalance.
- Guarantees **strict balance** → search operations fast.
- Cost: Slightly more rotations on updates than Red-Black Trees.
    

 **Use case:** Databases, memory indexing where **search-heavy workloads** dominate.

---

### 2. **Red-Black Tree**

- Weaker balancing compared to AVL but fewer rotations.
- Rules:
    
    - Each node is either red or black.
    - Root is black.
    - No two red nodes adjacent.
    - Every path from root to leaf has same number of black nodes.
        
- Ensures height ≤ 2*log(N).
    
 **Use case:** Widely used in libraries:

- `std::map`, `std::set` in C++
- `TreeMap` in Java

---

# AVL vs Red-Black Trees (Rotations & Balancing)

Self-balancing BSTs differ mainly in **how strictly they balance height**.  
This affects **number of rotations** and **performance trade-offs**.

---

## AVL Tree

- **Balance Factor** = height(left) − height(right) ∈ {−1, 0, 1}.
- **Strictly balanced** → guarantees height = O(log N).
- **Rotations** happen more often.
- Typical imbalance cases:
    - **LL (Left-Left)** → Right rotation
    - **RR (Right-Right)** → Left rotation
    - **LR (Left-Right)** → Left + Right rotation
    - **RL (Right-Left)** → Right + Left rotation
        

Rotations can happen **after almost every insert or delete**.

---

## Red-Black Tree

- Balancing rules based on **colors (red/black)** instead of strict height.
- Guarantees height ≤ 2 × log(N).
- **Less strict balance** than AVL.
- Fewer rotations needed → often just **recoloring** is enough.
    

Insertions/deletions usually cost fewer rotations than AVL.

---

## Rotation Frequency

|Operation|AVL Tree|Red-Black Tree|
|---|---|---|
|**Insert**|Up to **O(log N)** rotations (in worst case)|At most **2 rotations** (often just recoloring)|
|**Delete**|Up to **O(log N)** rotations|At most **3 rotations**|
|**Search**|Faster (height closer to log N)|Slightly slower (height can be up to 2·log N)|

---

## Visual Example

### Case: Insert causing Right-Right imbalance in AVL

`Insert 30, 40, 50`

Unbalanced AVL:

```
    30
      \
       40
         \
          50

```

AVL Fix (Left Rotation on 30):

```
     40
    /  \
   30   50

```

---

### Same case in Red-Black Tree

- Insertions recolor nodes and rotate if needed.
- Often only **1 rotation + recoloring** is required.
- Sometimes **no rotation at all** if properties hold.
    

---

## Interview Takeaways

- **AVL vs Red-Black**
    - AVL: Better for **search-heavy** workloads (faster lookups).
    - RB: Better for **insert/delete-heavy** workloads (fewer rotations).
        
- **Rotation Cost**
    
    - AVL: May rotate many times per update.
    - RB: Max 2–3 rotations → faster in practice.
        
- **Usage**
    
    - AVL: Databases, memory-intensive search systems.
    - RB: Library implementations (`std::map`, `std::set`, Java `TreeMap`), OS kernels.
        

---

If asked _“Why do libraries use Red-Black Trees instead of AVL?”_ → Answer:

> Because Red-Black trees require fewer rotations on inserts/deletes, giving better amortized performance in dynamic datasets. AVL trees are stricter and better for search-heavy workloads, but not as update-friendly.

---


