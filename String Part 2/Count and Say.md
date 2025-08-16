[Problem Link](https://leetcode.com/problems/count-and-say/)
### Problem Statement : 

The **count-and-say** sequence is a sequence of digit strings defined by the recursive formula:

- `countAndSay(1) = "1"`
- `countAndSay(n)` is the run-length encoding of `countAndSay(n - 1)`.

[Run-length encoding](http://en.wikipedia.org/wiki/Run-length_encoding) (RLE) is a string compression method that works by replacing consecutive identical characters (repeated 2 or more times) with the concatenation of the character and the number marking the count of the characters (length of the run). For example, to compress the string `"3322251"` we replace `"33"` with `"23"`, replace `"222"` with `"32"`, replace `"5"` with `"15"` and replace `"1"` with `"11"`. Thus the compressed string becomes `"23321511"`.

Given a positive integer `n`, return _the_ `nth` _element of the **count-and-say** sequence_.

**Example 1:**

```
Input: n = 4

Output: "1211"

Explanation:

countAndSay(1) = "1"
countAndSay(2) = RLE of "1" = "11"
countAndSay(3) = RLE of "11" = "21"
countAndSay(4) = RLE of "21" = "1211"
```

### Approach 1 :
- Recursive
- Base case: If `n == 1`, return `"1"`. 
- Otherwise:
    - Get the string for `n-1` recursively.
    - Encode it using run-length encoding.

#### Code :

```cpp
string countAndSay(int n) {

	if(n==1){
		return "1";
	}
	
	string res =  countAndSay(n-1);

	int count = 1;
	string curr = "";

	for(int i=0;i<res.size();i++){
	   
		if(i+1<res.size() && res[i]==res[i+1]){
			count++;
		}else{
			curr = curr + to_string(count) + res[i];
			count = 1;
		}
	}
	return curr;
	
}
```


> `Time Complexity` : O(1.3^n) ->The time complexity is proportional to the length of the nth string, which grows about 1.3^n, so the overall time complexity is O(1.3^n).
> It is known that the length `Ln` grows by about a factor of **~1.3–1.4** per step.  
(This was studied in Conway’s “Look-and-say” sequence → the growth constant ≈ 1.30357, called Conway’s constant.)
> 
> `Space Complexity` : O(2^n + n) 


---

###  Approach 2 :

- Iterative
- Start with `"1"`.
- For each step from `2` to `n`:
    - Read the previous string.
    - Build the next string by counting consecutive identical digits.

#### Code :

```cpp
string countAndSay(int n) {

	string res = "1";

	for(int i=2;i<=n;i++){
		string curr = "";
		int count = 1;
		for(int j=0;j<res.size();j++){

			if(j+1<res.size() && res[j]==res[j+1]){
				count++;
			}else{
				curr = curr + to_string(count) + res[j];
				count = 1;
			}
		}

		res = curr;

	}

	return res;
	
}
```


> `Time Complexity` : O(1.3^n) ->The time complexity is proportional to the length of the nth string, which grows about 1.3^n, so the overall time complexity is O(1.3^n).
> It is known that the length `Ln` grows by about a factor of **~1.3–1.4** per step.  
(This was studied in Conway’s “Look-and-say” sequence → the growth constant ≈ 1.30357, called Conway’s constant.)
> 
> `Space Complexity` : O(2^n) 


---
