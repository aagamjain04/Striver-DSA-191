[Problem Link](https://leetcode.com/problems/word-break-ii/)
### Problem Statement : 

Given a string `s` and a dictionary of strings `wordDict`, add spaces in `s` to construct a sentence where each word is a valid dictionary word. Return all such possible sentences in **any order**.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.
## Examples

**Example 1:**

```
Input: s = "pineapplepenapple", wordDict = ["apple","pen","applepen","pine","pineapple"]
Output: ["pine apple pen apple","pineapple pen apple","pine applepen apple"]
Explanation: Note that you are allowed to reuse a dictionary word.
```

### Approach 1 :

- Explore every possible way to segment the string.
- **Process:**
    1. **Start at the beginning** of the string (`index = 0`).
    2. **Find a Valid Word:** Loop through all prefixes starting from the current index. Check if a prefix exists in the dictionary.
    3. **Recurse:** If a valid word is found, make a recursive call to solve the problem for the **rest of the string**.
    4. **Build Sentence:** Pass the sentence-in-progress (with the newly found word added) to the next recursive call.
#### Code :

``` cpp
void recur(int i,int n,string &s,unordered_map<string,int> &mp,string word,vector<string> &res){

	if(i==n){
		word.pop_back();
		res.push_back(word);
		return;
	}
	
	for(int j=i;j<n;j++){
		string temp = s.substr(i,j-i+1);
		if(mp.count(temp)){
			recur(j+1,n,s,mp,word+temp+" ",res);
		}

	}
}

vector<string> wordBreak(string s, vector<string>& wordDict) {
	
	unordered_map<string,int> mp;
	for(auto &i:wordDict){
		mp[i] = 1;
	} 

	vector<string> res;
	string word = "";
	int n = s.size();
	recur(0,n,s,mp,word,res);
	return res;

}
```


> `Time Complexity` : O(2^n . k) where n is the length of the string `s` and k is the average length of the dictionary words. 
> 
> `Space Complexity` : `O(2^n)`  There are 2^n possible combinations of splits. Thus, in the worst case, each combination generates a different sentence that needs to be stored, leading to exponential space complexity.

---

### Approach 2 : 

- Same approach but now with memoization

#### Code :

```cpp
vector<string> recur(int i,int n,string &s,vector<string>& wordDict,unordered_map<string,int> &mp,unordered_map<int,vector<string>> &memo){

	if(memo.count(i))
	return memo[i];
	vector<string> res;

	if(i==n){
		res.push_back("");
		return res;
	}
   
	
	for(int j=i;j<n;j++){

		string word = s.substr(i,j-i+1);
		if(mp.count(word)){
			vector<string> suffixes = recur(j+1,n,s,wordDict,mp,memo);
			// Combine the current word with each valid suffix.
			for(auto &suffix:suffixes){
				if(suffix.empty()){
					res.push_back(word);
				}else{
					res.push_back(word + " " + suffix);
				}
			}
		   
		}    
	}
	memo[i] = res; 
	return res;
}


vector<string> wordBreak(string s, vector<string>& wordDict) {
	

	
	unordered_map<string,int> mp;
	unordered_map<int,vector<string>> memo;

	for(auto &i:wordDict){
		mp[i] = 1;
	}

	int n = s.size();

	return recur(0,n,s,wordDict,mp,memo);

}
```

> Time complexity: O(n⋅2^n) -> While memoization avoids redundant computations, it does not change the overall number of subproblems that need to be solved. In the worst case, there are still unique 2n possible substrings that need to be explored, leading to an exponential time complexity. For each subproblem, O(n) work is performed, so the overall complexity is O(n⋅2n).
>
>Space complexity: O(n⋅2^n) -> There are 2^n possible combinations of splits. Thus, in the worst case, each combination generates a different sentence that needs to be stored, leading to exponential space complexity.

---

### Approach 3 :

- Trie Optimization
- While the previous approaches focus on optimizing the search and computation process, we can also consider leveraging efficient data structures to enhance the word lookup process. This leads us to the trie-based approach, which uses a trie data structure to store the word dictionary, allowing efficient word lookup and prefix matching.

#### Code : 

``` cpp
class Solution {
public:

    struct Node{
        Node* alpha[26] = {NULL};
        bool isEnd = false; 
    };
    
    Node* root;
    vector<string> recur(int i,int n,string &s,vector<string>& wordDict,Node* root,unordered_map<int,vector<string>> &memo){

        if(memo.count(i))
        return memo[i];
        vector<string> res;

        if(i==n){
            res.push_back("");
            return res;
        }
       
        
        for(int j=i;j<n;j++){

            string word = s.substr(i,j-i+1);
            if(search(word,root)){
                vector<string> suffixes = recur(j+1,n,s,wordDict,root,memo);
                for(auto &suffix:suffixes){
                    if(suffix.empty()){
                        res.push_back(word);
                    }else{
                        res.push_back(word + " " + suffix);
                    }
                }
               
            }    
        }
        memo[i] = res; 
        return res;
    }

    bool search(string &word,Node* root){
        Node* curr = root;
        for(auto i:word){
            if(curr->alpha[i-'a']!=NULL){
                curr = curr->alpha[i-'a'];
            }else{
                return false;
            }
        }
        return curr->isEnd;
    }

    void insert(string &word,Node* root){
        Node * curr = root;
        for(auto i:word){
            if(curr->alpha[i-'a']==NULL){
                Node* next = new Node();
                curr->alpha[i-'a'] = next;
                curr = next;
            }else{
                curr = curr->alpha[i-'a'];
            }
        }
        curr->isEnd = true;
    }


    vector<string> wordBreak(string s, vector<string>& wordDict) {
        

        root = new Node();
        
        unordered_map<int,vector<string>> memo;

        for(auto &i:wordDict){
            insert(i,root);
        }

        int n = s.size();

        return recur(0,n,s,wordDict,root,memo);

    }
};

```


> Time complexity: O(n⋅2^n) -> Even though the trie-based approach uses an efficient data structure for word lookup, it still needs to explore all possible ways to break the string into words. In the worst case, there are 2n unique possible partitions, leading to an exponential time complexity. O(n) work is performed for each partition, so the overall complexity is O(n⋅2n).
>
>Space complexity: O(n⋅2^n) -> The trie data structure itself can have a maximum of 2n nodes in the worst case, where each character in the string represents a separate word. Additionally, the tabulation map used in this approach can also store up to 2n strings of size n, resulting in an overall exponential space complexity.

---
