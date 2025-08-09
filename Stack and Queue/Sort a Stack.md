[Problem Link](https://www.geeksforgeeks.org/problems/sort-a-stack/1)
### Problem Statement : 

Given a stack, sort it so that the **top** has the greatest element.

**Example 1:**

```
Input stack (top first):   3 1 4 2
Output stack (top first):  4 3 2 1

```

### Approach 1 — Auxiliary Stack (Iterative)
- Use a second stack to hold elements in sorted order.
- For each element popped from the original:
  - Move smaller elements from auxiliary back to original.
  - Insert current element into the right position in auxiliary.
- Copy sorted stack back to original.

> `Time Complexity` : O(n²)
> 
> `Space Complexity` : O(n) stack


---

###  Approach 2 — Recursive (No Extra Stack)
- Recursively pop all elements until the stack is empty.
- Insert each popped element back into the stack in sorted order using a helper function.


#### Code :

```cpp
/*The structure of the class is
class SortedStack{
public:
    stack<int> s;
    void sort();
};
*/

/* The below method sorts the stack s
you are required to complete the below method */


void insert(stack<int> &s,int num){
    if(s.empty() || s.top()<=num)
    {
        s.push(num);
        return;
    }
    
    int x = s.top();
    s.pop();
    insert(s,num);
    s.push(x);   
}

void SortedStack ::sort() {
    
    if(s.empty()){
        return;
    }
    
    int num = s.top();
    s.pop();
    sort();
    
    insert(s,num);  
}
```


> `Time Complexity` : O(n²) 
> 
> `Space Complexity` : O(n) recursion

---
