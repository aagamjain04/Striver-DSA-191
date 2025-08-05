[Problem Link](https://leetcode.com/problems/valid-parentheses/description/)
### Problem Statement : 

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.## Examples

**Example 1:**

```
Input: s = "()[]{}"
Output: true

Input: s = "(]"
Output: false

Input: s = "([{}])"
Output: true

Input: s = "((())"
Output: false

```

### Approach 1 :

- Using stack
- **Create a stack** to keep track of opening brackets.
- **Iterate through characters** in string:
    - If it's an **opening bracket** (`(`, `{`, `[`):
        - Push onto stack.
    - If it's a **closing bracket**:
        - Check if stack is **empty** → return false (closing before opening).
        - Pop from stack and check if it matches:
            - `')'` must match `'('`
            - `']'` must match `'['`
            - `'}'` must match `'{'`
        - If not matching → return false.    
- After iteration:
    - If stack is **empty** → return true (all matched).
    - Else → return false (some unmatched opening brackets remain).

#### Code :

``` cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        for(auto i:s){
            if(i=='(' || i =='{' || i=='[')
            st.push(i);
            else{
                if(st.empty())
                return false;
                else{
                    if(i==')' && st.top() == '(')
                    st.pop();
                    else if(i=='}' && st.top() == '{')
                    st.pop();
                    else if(i==']' && st.top() == '[')
                    st.pop();
                    else return false;
                }
            }
        }

        return st.empty();
    }
};

```


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(n)


---

