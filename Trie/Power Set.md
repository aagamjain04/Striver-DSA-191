[Problem Link](https://www.geeksforgeeks.org/problems/power-set4302/1)
### Problem Statement : 

- Given a string `s` of length `n`, generate all **non-empty subsequences** of `s` and return them in **lexicographically sorted order**.


Note :
```
- A subsequence is formed by deleting zero or more characters without changing the relative order of remaining characters.
- Example: "abc" → "ab", "ac", "bc", "a", "b", "c", "abc".
- Total subsequences (including empty) = 2^n.
- Non-empty subsequences = 2^n - 1.
- Need to generate all subsequences and then sort them lexicographically.
```

**Example 1:**

```
Input: s = "abc"
Output: 
a
ab
abc
ac
b
bc
c


```

---

###  Approach 1 :

- Recursive backtracking 
- Pick or skip each character.
- Store subsequences.
- Sort at the end.


```
Time Complexity:
Generating subsequences → O(2^n)
Sorting subsequences → O(m log m) where m = 2^n - 1
Total = O(2^n log(2^n))
Space Complexity: O(2^n * n) (storing subsequences).
```

---

### Approach 2:

- Bitmasking
- Each subsequence corresponds to a binary mask of length `n`.  
    Example: `"abc"` → mask `101` = `"ac"`.
- Generate all masks from `1` to `(1<<n) - 1`.
- Collect and sort subsequences.
    
**Similar time/space complexity** as recursion.


---


### Approach 3:

- Using trie
- **Generate subsequences** of the given string (recursively or via bitmask).
-  **Insert each subsequence** into a Trie.
    - Each node corresponds to a character.    
    - `isEnd` flag marks the end of a subsequence.
-  **DFS traversal** of Trie:
    - At each node, append character to current string.
    - If `isEnd` is true, print/store the current string.
    - Recurse over children `'a' → 'z'`.
        
This ensures **lexicographic ordering without explicit sorting**.

#### Code :

``` cpp
class Solution {
  public:
  
    struct Node {
      Node* alpha[26] = {NULL};
      bool isEnd = false;
      int count = 0;
    };
    
    Node* root;
    
    void insert(string &s){
        
        Node* temp = root;
        for(auto i:s){
            if(temp->alpha[i-'a']==NULL){
                temp->alpha[i-'a'] = new Node();
            }
            temp = temp->alpha[i-'a'];
        }
        temp->isEnd = true;
        temp->count++;
    }
    
    void genAllPermutation(int i,string &s,string curr){
        
        if(i==s.size()){
            insert(curr);
            return;
        }
        
        genAllPermutation(i+1,s,curr+s[i]);
        genAllPermutation(i+1,s,curr);
        
        
    }
    
    void dfs(Node* node,vector<string> &res,string &curr){
        
        if(node->isEnd){
            for(int i=0;i<node->count;i++)
            res.push_back(curr);
        }
        
        for(int i=0;i<26;i++){
            if(node->alpha[i]){
                curr.push_back(i+'a');
                dfs(node->alpha[i],res,curr);
                curr.pop_back();
                
            }
        }
    }
    
    
    vector<string> AllPossibleStrings(string s) {
       
       
       root = new Node();
       string temp = "";
       genAllPermutation(0,s,temp);
       
       vector<string> res;
       string curr = "";
       
       dfs(root,res,curr);
       return res;
        
    }
};
```


```
Complexity
Subsequence generation: O(2^n).
Trie insertion: Each subsequence of length k takes O(k) to insert.
Total = O(∑ k) ≈ O(n * 2^n).
DFS traversal: Visits each node once → O(total length of all subsequences).
Space: O(n * 2^n) (huge, but acceptable for interview when n ≤ 15–20).
```


---


