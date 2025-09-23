[Problem Link](https://www.geeksforgeeks.org/problems/count-of-distinct-substrings/1)
### Problem Statement : 

- Given a string of length `N` containing only lowercase English letters, find the **total number of distinct substrings** of the string.

**Example 1:**

```
Input: s = "abc"
Output:  "a", "b", "ab", "ba", "aba" → total 5 distinct substrings

```

---

###  Approach 1 :

- Brute force
- Generate all substrings using nested loops and insert into a set.


> `Time Complexity` : O(N^2 * logn)
> 
> `Space Complexity` : O(N * L) -> set

---

### Approach 2:

- Insert all suffixes of the string into a Trie.
- Each unique path in Trie represents a distinct substring.
- Count the number of nodes created in Trie → gives total distinct substrings.

#### Code :

``` cpp
/*You are required to complete this method */
int countDistinctSubstring(string s) {
    
    struct Node{
      Node* alpha[26] = {NULL};
      
      ~Node() {
        for (int i = 0; i < 26; ++i) {
            delete alpha[i];
        }
    }
    };
    
    int n = s.size();
    
    Node* root = new Node();
    int count = 0;
    
    for(int i=0;i<n;i++){
        
        Node* temp = root;
        for(int j=i;j<n;j++){
            
            if(temp->alpha[s[j]-'a']==NULL){
                temp->alpha[s[j]-'a'] = new Node();
                count++;
            }
            temp = temp->alpha[s[j]-'a'];
        }
    }
    delete root;
    return count+1; //considering empty string
    
    
}

```


Note : Without clearing the memory it gives MLE on gfg. So its important to add destructor and delete all nodes.

> `Time Complexity` : O(N * N)
> 
> `Space Complexity` : O(N * N)

---


