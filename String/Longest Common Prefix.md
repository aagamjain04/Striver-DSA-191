[Problem Link](https://leetcode.com/problems/longest-common-prefix/description/)
### Problem Statement : 

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

### Approach 1 :
- Horizontal Scanning
- Take the first string as the initial prefix.
- Compare it with each subsequent string:
    - Shrink the prefix until it matches the start of the string.
- If prefix becomes empty → return `""`.

#### Code :


``` cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        
        int n = strs.size();
        string prefix = strs[0];

        for(int i=1;i<n;i++){
           
            int k = 0;
            for(int j=0;j<strs[i].size();j++){
                if(k<prefix.size() && strs[i][j] == prefix[k]){
                    k++;
                }else break;
            }
            prefix.resize(k);
        }

        return prefix;

    }
};
```

> `Time Complexity` : O(S) where S = sum of all characters in all strings
> 
> `Space Complexity` : O(1) 


---

###  Approach 2 :

- **Trie-based approach** for **O(S)** complexity, which is often a follow-up question **interviews.**
- Why Trie?
- If there are many strings and you want efficient prefix operations (not just LCP once), a Trie is useful.
- Especially good when:
    - You need to query prefixes multiple times.
    - Strings share many overlapping parts.


## Idea
1. **Build a Trie** from all strings.    
2. Starting from root:
    - Traverse down while:
        - There is **exactly one child**.
        - The current node is **not** the end of a word.
    - Append that character to the result.
3. Stop when:
    - More than one child → multiple possible continuations.
    - Or the node marks the end of a string.

#### Code :

```cpp
struct TrieNode{
    TrieNode* alpha[26] = {NULL};
    bool isEnd = false;
    int childCount = 0;
};

TrieNode* root;


class Solution {
public:

    void insert(TrieNode* root,string &word){
        TrieNode* temp = root;
        for(auto i:word){
            if(temp->alpha[i-'a']==NULL){
                temp->alpha[i-'a'] = new TrieNode();
                temp->childCount++;
            }
            temp = temp->alpha[i-'a'];
        }
        temp->isEnd = true;
    }

    string longestCommonPrefix(vector<string>& strs) {

        //using trie
        int n = strs.size();
        root = new TrieNode();

        for(int i=0;i<strs.size();i++){
            if(strs[i].size()==0)
            return "";
            insert(root,strs[i]);
        }

        string prefix = "";

        TrieNode* temp = root;

        while(temp && temp->childCount==1 && temp->isEnd==false){
            for(int i=0;i<26;i++){
                if(temp->alpha[i]!=NULL){
                    prefix += i+'a';
                    temp = temp->alpha[i];
                    break;
                }
            }
        }

        return prefix;


        
    }
};
```


> `Time Complexity` : O(S) 
> 
> `Space Complexity` : O(S) 

---