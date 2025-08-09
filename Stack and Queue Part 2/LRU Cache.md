[Problem Link](https://leetcode.com/problems/lru-cache/description/)
### Problem Statement : 

Design a data structure that follows the constraints of a Least Recently Used (LRU) cache
Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
- `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1:**

```
**Input**
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
**Output**
[null, null, null, 1, null, -1, null, -1, 3, 4]

**Explanation**
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

### Approach:

The goal is to design a **cache** that:

- **Stores up to `capacity`** key-value pairs
- **Evicts the Least Recently Used (LRU)** entry when full
- Both `get()` and `put()` must work in **O(1)** time
    

---

## **Key Idea**

We use:
1. **Doubly Linked List (DLL)** – to maintain **usage order** (most recent at the head, least recent at the tail).    
2. **Hash Map** – to provide **O(1) access** to nodes in the list.
    
---
## **How it Works**

### **1. Doubly Linked List**

- Each node stores:
    - `key`
    - `value`
    - `prev` pointer
    - `next` pointer
- **Most recently used** → at the **head**
- **Least recently used** → at the **tail**
- Dummy **head** and **tail** nodes are used for easy insert/delete.
    
---

### **2. Hash Map**

- Maps `key` → pointer to corresponding **DLL node**.
- Allows **O(1)** look-up and direct node access.
    

---

### **3. Operations**

#### **get(key)**

1. If key **not found** → return `-1`
2. If found:
    - Move node to **head** (mark as most recently used)
    - Return its value

#### **put(key, value)**

1. If key **already exists**:
    - Update the value
    - Move node to **head**
        
2. If key **does not exist**:
    
    - Create a new node
    - Insert it at **head**
    - Add to hash map
    - If capacity exceeded:
        - Remove node at **tail** (LRU)
        - Remove from hash map

---

### **Why O(1)?**

- **Map lookup** → O(1)
- **DLL insertion/deletion** → O(1)
- No shifting or searching needed.
    
---

## **Complexity**

|Operation|Time Complexity|Space Complexity|
|---|---|---|
|`get()`|O(1)|O(capacity)|
|`put()`|O(1)|O(capacity)|


---


#### Code :

```cpp
struct Node {
    int key,value;
    Node *prev,*next;
    Node(int key,int value){
        this->key = key;
        this->value = value;
    }
};

class LRUCache {
public:

    int capacity = 0;
    unordered_map<int,Node*> cache;
    Node *head,*tail; 

    LRUCache(int capacity) {
        this->capacity = capacity;
        head = new Node(-1,-1);
        tail = new Node(-1,-1);
        head->next = tail;
        tail->prev = head; 
    }
    
    int get(int key) {
        if(cache.find(key) == cache.end())
        return -1;

        Node* node = cache[key];
        moveToHead(node);
        return node->value;
    }
    
    void put(int key, int value) {
        if(cache.find(key)!=cache.end()){
            Node* node = cache[key];
            node->value = value;
            moveToHead(node); 
        }else{
            Node* newNode = new Node(key,value);
            cache[key] = newNode;
            addNode(newNode);

            if(cache.size() > capacity){
                Node* tailNode = popTail();
                cache.erase(tailNode->key);
                delete tailNode;
            }
        }
    }

    void moveToHead(Node* node){
        removeNode(node);
        addNode(node);
    }

    // helper remove
    void removeNode(Node* node){
        Node* nextNode = node->next;
        Node* prevNode = node->prev;

        prevNode->next = nextNode;
        nextNode->prev = prevNode;
    }

    Node* popTail(){
        Node* res = tail->prev;
        removeNode(res);
        return res;
    }

    // add node right after dummy head
    void addNode(Node* node){
        Node* nextNode = head->next;
        node->prev = head;
        node->next = nextNode;
        nextNode->prev = node;
        head->next = node;
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```


