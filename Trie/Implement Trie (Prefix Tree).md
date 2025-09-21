[Problem Link](https://leetcode.com/problems/implement-trie-prefix-tree/description/)
### Problem Statement : 

Design and implement a **Trie** data structure that supports:

1. **Insert** a word.   
2. **Search** if a word exists.
3. **Check prefix** if any word starts with a given prefix.

**Example 1:**

```
Input
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output
[null, null, true, false, true, null, true]

Explanation
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True

```

---

###  Approach 1 :

- A **Trie** is a **tree-like data structure** used for storing strings efficiently.
- Each **node** represents a character in a string.
- Root node is empty (`""`).
- Each node has up to **26 children** (for lowercase English letters).
- A boolean flag `isEnd` marks the **end of a word**.**

#### Operations

### 1. Insert (word)
- Traverse character by character.
- If node for character does not exist → create it.
- Mark `isEnd = true` at last character node.
- **Time Complexity**: `O(L)` (L = length of word).
    
### 2. Search (word)
- Traverse word in trie.
- If node missing → return `false`.
- At end, return `true` only if `isEnd == true`.
- **Time Complexity**: `O(L)`.
    
### 3. StartsWith (prefix)
- Traverse characters of prefix.
- If node missing → return `false`.
- Otherwise return `true`.
- **Time Complexity**: `O(L)`.

#### Code :

```cpp
class Trie {
public:

    struct Node {
        Node* alpha[26] = {NULL};
        bool isEnd = false;
    };

    Node* root;

    Trie() {
        root = new Node();
    }
    
    void insert(string word) {
        
        Node* temp = root;
        for(auto &i : word){

            if(temp->alpha[i-'a']==NULL){
                temp->alpha[i-'a'] = new Node();
            }
            temp = temp->alpha[i-'a'];
        }
        temp->isEnd = true;

    }
    
    bool search(string word) {
        
        Node* temp = root;
        for(auto &i:word){
            if(temp->alpha[i-'a']!=NULL){
                temp = temp->alpha[i-'a'];
            }else {
                return false;
            }
            
        }
        return temp->isEnd;
    }
    
    bool startsWith(string prefix) {
        Node* temp = root;
        for(auto &i:prefix){
            if(temp->alpha[i-'a']!=NULL){
                temp = temp->alpha[i-'a'];
            }else {
                return false;
            }
            
        }
        return true;
    }
};
```


> `Time Complexity` : O(L) -> for Insert/Search/Prefix (L = word/prefix length)
> 
> `Space Complexity` : O(N * L) 

---