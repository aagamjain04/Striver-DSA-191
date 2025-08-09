[Problem Link](https://leetcode.com/problems/lfu-cache/description/)
### Problem Statement : 

Design a data structure LFUCache with:

- `LFUCache(int capacity)` — Initializes cache.
- `int get(int key)` — Returns value if key exists, else -1.
- `void put(int key, int value)` — Inserts or updates key-value.

**Eviction Policy:**
- Least Frequently Used key removed when full.
- If tie, Least Recently Used is evicted.

## Requirements

- O(1) average time complexity for both `get` and `put`.

**Example 1:**

```
LFUCache cache(2);
cache.put(1, 1);
cache.put(2, 2);
cache.get(1);    // returns 1
cache.put(3, 3); // evicts key 2
cache.get(2);    // returns -1 (not found)
cache.get(3);    // returns 3
```

### Approach:

### Core Data Structures

| Data Structure | Purpose |
|----------------|---------|
| `unordered_map<int, pair<int, int>> keyToValFreq` | Maps key to {value, freq} |
| `unordered_map<int, list<int>> freqToKeys` | Maps frequency to keys in LRU order |
| `unordered_map<int, list<int>::iterator> keyToIter` | Stores iterator of key in freq list |
| `int minFreq` | Tracks current minimum frequency |

---

## Operations

### get(key)
1. If key not found, return -1.
2. Else:
   - Remove key from old freq list.
   - Update frequency.
   - Add key to new freq list at the end.
   - Update `keyToIter`.
   - If old list empty and was `minFreq`, increment `minFreq`.

### put(key, value)
1. If capacity is 0, do nothing.
2. If key exists:
   - Update value.
   - Call `get(key)` to increase frequency.
3. If key doesn't exist:
   - If at capacity:
     - Evict LRU key from `freqToKeys[minFreq]`.
   - Insert key with freq 1.
   - Set `minFreq = 1`.

---

## Time and Space Complexity

- **Time**: O(1) for both `get()` and `put()` (amortized).
- **Space**: O(N) for storing keys and frequencies.



#### Code :

```cpp
#include <bits/stdc++.h>
using namespace std;

class LFUCache {
private:
    int capacity, minFreq;
    unordered_map<int, pair<int, int>> keyToValFreq; // key -> {value, freq}
    unordered_map<int, list<int>> freqToKeys;        // freq -> keys in LRU order
    unordered_map<int, list<int>::iterator> keyToIter; // key -> iterator in freq list

public:
    LFUCache(int capacity) {
        this->capacity = capacity;
        this->minFreq = 0;
    }

    int get(int key) {
        if (keyToValFreq.find(key) == keyToValFreq.end())
            return -1;

        int val = keyToValFreq[key].first;
        int freq = keyToValFreq[key].second;

        // Remove key from current freq list
        freqToKeys[freq].erase(keyToIter[key]);

        // If this was the last key with minFreq, update minFreq
        if (freqToKeys[freq].empty()) {
            freqToKeys.erase(freq);
            if (minFreq == freq) minFreq++;
        }

        // Add to new freq list
        freq++;
        freqToKeys[freq].push_back(key);
        keyToIter[key] = --freqToKeys[freq].end();
        keyToValFreq[key].second = freq;

        return val;
    }

    void put(int key, int value) {
        if (capacity == 0) return;

        // If key exists, update value and freq
        if (keyToValFreq.find(key) != keyToValFreq.end()) {
            keyToValFreq[key].first = value;
            get(key); // To update frequency
            return;
        }

        // Evict if at capacity
        if (keyToValFreq.size() >= capacity) {
            int keyToEvict = freqToKeys[minFreq].front();
            freqToKeys[minFreq].pop_front();
            if (freqToKeys[minFreq].empty())
                freqToKeys.erase(minFreq);
            keyToValFreq.erase(keyToEvict);
            keyToIter.erase(keyToEvict);
        }

        // Insert new key
        keyToValFreq[key] = {value, 1};
        freqToKeys[1].push_back(key);
        keyToIter[key] = --freqToKeys[1].end();
        minFreq = 1;
    }
};

```


