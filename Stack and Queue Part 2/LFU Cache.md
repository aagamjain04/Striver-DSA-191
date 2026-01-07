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

## Data Structures

```cpp
unordered_map<int, list<Node>::iterator> keyToNode;  // key -> node iterator
unordered_map<int, list<Node>> freqToList;           // frequency -> list of nodes
int minFreq;  // Track minimum frequency for O(1) eviction
```

### Why These Structures?

- **keyToNode**: O(1) lookup of any key's node
- **freqToList**: Groups nodes by frequency, uses `list` for O(1) insert/delete with iterator
- **minFreq**: Avoids searching for minimum frequency during eviction

---

## Algorithm

### Get Operation

1. Check if key exists (return -1 if not)
2. **Save the value first** (iterator will be invalidated!)
3. Update frequency (move node from freq list to freq+1 list)
4. Return saved value

### Put Operation

**Case 1: Key exists**

- Update value
- Increment frequency

**Case 2: Key doesn't exist**

- If at capacity: evict last node in `freqToList[minFreq]`
- Insert new node with frequency = 1
- Set `minFreq = 1`

### Update Frequency Helper

1. Remove node from current frequency list
2. If that list is now empty AND it was minFreq: `minFreq++`
3. Increment node's frequency
4. Add node to new frequency list (at front for LRU order)
5. Update keyToNode mapping


---

## Time and Space Complexity

- **Time**: O(1) for both `get()` and `put()` (amortized).
- **Space**: O(N) for storing keys and frequencies.



#### Code :

```cpp
// Node represents a cache entry
struct Node {
    int key;    // key of the cache entry
    int value;  // value stored for the key
    int freq;   // how many times this key has been accessed

    // Constructor to initialize node fields
    Node(int k,int v,int f){
        this->key = k;
        this->value = v;
        this->freq = f;
    }
};

class LFUCache {

    int capacity = 0;   // Maximum number of items cache can hold
    int minFreq = 0;    // Minimum frequency among all keys currently in cache

    // Maps frequency -> list of Nodes having that frequency
    // Nodes are stored in a list to maintain LRU order within same frequency
    unordered_map<int,list<Node>> freqToList;

    // Maps key -> iterator pointing to the node inside freqToList
    // Allows O(1) access to any node
    unordered_map<int,list<Node>::iterator> keyToNode;

    // Helper function to update frequency of a node
    void updateFreq(list<Node>::iterator it){
        int key = it->key;     // Extract key from node
        int value = it->value; // Extract value from node
        int freq = it->freq;  // Current frequency of node

        // Remove the node from its current frequency list
        freqToList[freq].erase(it);

        // If this frequency list becomes empty and it was the minimum frequency,
        // then increment minFreq
        if(freqToList[freq].empty() && minFreq==freq){
            minFreq++;
        }

        // Increase frequency of the node
        freq++;

        // Insert the node into the front of the new frequency list
        // Front represents most recently used within this frequency
        freqToList[freq].push_front(Node(key,value,freq));

        // Update the key-to-node iterator mapping
        keyToNode[key] = freqToList[freq].begin();
    }

public:

    // Constructor initializes cache capacity and minimum frequency
    LFUCache(int capacity) {
        this->capacity = capacity;
        this->minFreq = 0;
    }
        
    // Returns the value of the key if present, else -1
    int get(int key) {
        // If key does not exist in cache
        if(keyToNode.find(key)==keyToNode.end())
            return -1;

        // Get iterator pointing to the node
        auto it = keyToNode[key];

        int value = it->value;  // Store value to return

        // Update frequency because key was accessed
        updateFreq(it);

        return value;
    }
    
    // Inserts or updates a key-value pair in the cache
    void put(int key, int value) {
        // If capacity is zero, cache cannot store anything
        if(capacity==0)
            return;

        // If key does not already exist
        if(keyToNode.find(key)==keyToNode.end()){

            // If cache is full, evict least frequently used item
            if(keyToNode.size()>=capacity){

                // Get the least recently used node from minimum frequency list
                int evictKey = freqToList[minFreq].back().key;

                // Remove it from the frequency list
                freqToList[minFreq].pop_back();

                // Remove it from key-to-node map
                keyToNode.erase(evictKey);
            }

            // New node always starts with frequency = 1
            minFreq = 1;

            // Insert new node at front of frequency 1 list
            freqToList[1].push_front(Node(key,value,1));

            // Store iterator to this node
            keyToNode[key] = freqToList[1].begin();

        }else{
            // If key already exists

            // Get iterator to existing node
            auto it = keyToNode[key];

            // Update value
            it->value = value;

            // Update frequency because key was accessed/updated
            updateFreq(it);
        }

    }
};




```


