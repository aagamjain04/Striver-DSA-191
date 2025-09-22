[Problem Link](https://www.geeksforgeeks.org/problems/longest-valid-word-with-all-prefixes/1)
### Problem Statement : 

- Given an array of strings `words[]`.
- Find the **longest word** such that:
    - Every **prefix** of it also exists in the array.
- If multiple candidates exist:
    - Choose the **lexicographically smallest**.
- If no such word exists, return `""`.

**Example 1:**

```
Input: words[] = ["p", "pr", "pro", "probl", "problem", "pros", "process", "processor"]
Output: "pros" 
Explanation: "pros" is the longest word with all prefixes ("p", "pr", "pro", "pros") present.

```

---

###  Approach 1 :

- Brute force
- Store all words in a hash set.
- For each word:
    - Check all prefixes.
    - If valid, track the best answer.


> `Time Complexity` : O(N * L^2)
> 
> `Space Complexity` : O(N * L) -> Hash set

---

### Approach 2:

- Insert all words into a **Trie**.
- Mark nodes where words end.
- Perform DFS:
    - Only continue if the current node is a word-end (ensuring prefix validity).
- Track the **longest word found**.

#### Code :

``` cpp
class Solution {
    
    struct Node{
        Node* alpha[26] = {NULL};
        bool isEnd = false;
    };
    
    Node* root;
    
    void insert(string &word){
        
        Node* temp = root;
        for(auto c:word){
            
            if(temp->alpha[c-'a']==NULL){
                temp->alpha[c-'a'] = new Node;
            }
            temp = temp->alpha[c-'a'];
        }
        temp->isEnd = true;
    }
    
    bool checkPrefix(string &w){
        
        Node* temp = root;
        for(auto c:w){
            if(temp->alpha[c-'a']==NULL){
                return false;
            }
            temp = temp->alpha[c-'a'];
            if(temp->isEnd==false)
            return false;
        }
        
        return true;
    }
    
    public:
    
    string longestValidWord(vector<string>& words) {
        
        root = new Node();
        for(auto &w : words){
            insert(w);
        }
        
        string res = "";
        
        for(auto &w : words){
            if(checkPrefix(w)){
                if(w.size() > res.size()){
                    res = w;
                }else if(w.size() == res.size() && w<res){
                    res = w;
                }
            }
        }
        return res;
    }
};

```


> `Time Complexity` : O(N * L)
> 
> `Space Complexity` : O(N * L)

---


