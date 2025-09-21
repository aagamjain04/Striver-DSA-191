[Problem Link](https://leetcode.com/problems/implement-trie-prefix-tree/description/)
### Problem Statement : 

Implement a Trie data structure that supports the following methods:

1. Insert (word): To insert a string `word` in the Trie.
2. Count Words Equal To (word): Return the count of occurrences of the string word in the Trie.
3. Count Words Starting With (prefix): Return the count of words in the Trie that have the string “prefix” as a prefix.
4. Erase (word): Delete one occurrence of the string word from the Trie.

**Example 1:**

```
Input : ["Trie", "insert", "countWordsEqualTo", "insert", "countWordsStartingWith", "erase", "countWordsStartingWith"]
[ "apple", "apple", "app", "app", "apple", "app" ]

Output : [null, null, 1, null, 2, null, 1]

Explanation :
Trie trie = new Trie()
trie.insert("apple")
trie.countWordsEqualTo("apple")  // return 1
trie.insert("app") 
trie.countWordsStartingWith("app") // return 2
trie.erase("apple")
trie.countWordsStartingWith("app")   // return 1

```

---

###  Approach 1 :

- **`Trie()`** – Initialize trie root.
- **`insert(word)`** – Insert word into trie.
    - For each char: move/create child node.
    - Increase `countPrefix`.
    - At the end, increase `countEndWith`.
        
- **`countWordsEqualTo(word)`** – Return how many times the exact word is inserted.
    - Traverse word. If path missing → return 0.
    - Return `countEndWith` at last node.
        
- **`countWordsStartingWith(prefix)`** – Return number of words with given prefix.
    - Traverse prefix. If path missing → return 0.
    - Return `countPrefix` of last prefix node.
        
- **`erase(word)`** – Delete one occurrence of word.
    - Traverse word, decrement `countPrefix`.
    - At the end, decrement `countEndWith`.
    - (We don’t physically delete nodes → keeps it simple.)`

#### Code :

```cpp
#include <bits/stdc++.h> 



class Trie{

    struct Node{
        Node* alpha[26] = {NULL};
        bool isEnd = false;
        int endCount = 0;
        int prefixCount = 0;
    };

    public:

    Node* root;

    Trie(){
        root = new Node();
    }

    void insert(string &word){
        
        Node* temp = root;
        for(auto &i:word){
            if(temp->alpha[i-'a']==NULL){
                temp->alpha[i-'a'] = new Node();
            }
            temp = temp->alpha[i-'a'];
            temp->prefixCount++;
        }
        temp->isEnd = true;
        temp->endCount++;
    }

    int countWordsEqualTo(string &word){
        
        Node* temp = root;
        for(auto &i:word){
            if(temp->alpha[i-'a']!=NULL){
                temp = temp->alpha[i-'a'];
            }else{
                return 0;
            }
        }
        if(temp->isEnd)
        return temp->endCount;
        else 
        return 0;
    }

    int countWordsStartingWith(string &word){
        Node* temp = root;
        for(auto &i:word){
            if(temp->alpha[i-'a']!=NULL){
                temp = temp->alpha[i-'a'];
            }else{
                return 0;
            }
        }
    
        return temp->prefixCount;
       
    }

    void erase(string &word){
        Node* temp = root;
        for(auto &i:word){
            temp = temp->alpha[i-'a'];
            temp->prefixCount--;
        }
        temp->endCount--;
        if(temp->endCount == 0){
            temp->isEnd = false;
        }
    }
};

```


> `Time Complexity` : O(L) -> for Insert/Search/Prefix (L = word/prefix length)
> 
> `Space Complexity` : O(N * L) 

---