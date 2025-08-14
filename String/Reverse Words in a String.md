[Problem Link](https://leetcode.com/problems/reverse-words-in-a-string/)
### Problem Statement : 

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return _a string of the words in reverse order concatenated by a single space._

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

**Example 1:**

```
Input: s = "the sky is blue"
Output: "blue is sky the"
```

**Example 2:**

```
Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```

### Approach 1 :
- Brute force
- Use a stack to push all the words in a stack
- Now, all the words of the string are present in the stack, but in reverse order
- Pop elements of the stack one by one and add them to our answer variable. Remember to add a space between the words as well.

#### Code :


``` cpp
string reverseWords(string s) {

	//using extra space
	stack<string> st;
	int n = s.size();

	string word = "";
	
	for(int i=0;i<n;i++){
		if(s[i]!= ' '){
			word+=s[i];
		}else{
			if(word.size()){
				st.push(word);
				word.clear();
			}
		}
	}
	if(word.size())
	st.push(word);

	string ans = "";
	while(!st.empty()){
		ans += st.top();
		st.pop();

		if(!st.empty())
		ans += " ";
	}
	return ans;

}

```

> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(n) 


---

###  Approach 2 :

- Optimal
- Reverse the given string using STL or two pointer (in-place)
- Ignoring spaces pick every word , reverse it and store in resultant string followed by a space.

#### Code :

```cpp
string reverseWords(string s) {
	// no extra space

	reverse(s.begin(),s.end());
	int n = s.size();


	string word = "";
	string ans = "";
	for(int i=0;i<n;i++){
		if(s[i]!=' '){
			word+=s[i];
		}else{

			if(word.size()){
				reverse(word.begin(),word.end());
				ans += word;
				ans += " ";
				word.clear();
			}
		}
	}

	reverse(word.begin(),word.end());
	ans += word;
	int k = ans.size()-1;
	while(ans[k] == ' ')
	ans.pop_back();
	return ans;
}
```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(1) -> not considering result space

---
