[Problem Link](https://leetcode.com/problems/palindrome-partitioning/description/)
### Problem Statement : 

Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return all possible palindrome partitioning of `s`.
## Examples

**Example 1:**

```
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

### Approach 1 :

- Use backtracking to generate all possible partitions
- For each partition, check if the current substring is a palindrome
- If yes, add it to current partition and recursively check remaining string
- If we reach the end of string, add current partition to results`

#### Code :

``` cpp
bool isPal(string &p){
	string r = p;
	reverse(r.begin(),r.end());
	return (p.compare(r)==0);
}

void recur(int i,string &s,vector<string> &temp,vector<vector<string>> &res){

	if(i==s.size()){
		res.push_back(temp);
		return;
	}
	if(i>s.size()){
		return;
	}

	for(int j=i;j<s.size();j++){

		

		string p = s.substr(i,j-i+1);
		if(isPal(p)){
			temp.push_back(p);
			recur(j+1,s,temp,res);
			temp.pop_back();
		}
	}
}

vector<vector<string>> partition(string s) {
	
	vector<vector<string>> res;
	vector<string> temp;
	recur(0,s,temp,res);
	return res;
}
```


> `Time Complexity` : O(n * 2^n)
> 
> `Space Complexity` : O(n) - Auxiliary stack space and ignoring result space

---
