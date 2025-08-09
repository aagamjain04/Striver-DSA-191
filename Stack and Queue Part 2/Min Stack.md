[Problem Link](https://leetcode.com/problems/min-stack/description/)
### Problem Statement : 

We need to design a stack that supports:

1. `push(val)` — add element to top
2. `pop()` — remove top element
3. `top()` — get top element
4. `getMin()` — get **minimum element** in the stack
    

**Constraints:**

- All operations must be **O(1)**
- Must work for **negative and duplicate values**

**Example 1:**

```
MinStack s;
s.push(-2);
s.push(0);
s.push(-3);
s.getMin();   // returns -3
s.pop();
s.top();      // returns 0
s.getMin();   // returns -2
```

### Approach 1 :

- Two Stacks (O(1) Time, O(n) Space
- ### **Idea**
- Use:
    1. **Main stack** → store all values
    2. **Min stack** → store current minimum at each point
- On `push`:
    - Push `val` into main stack
    - Push `min(val, minStack.top())` into min stack
- On `pop`:
    - Pop from both stacks
- `getMin()`:
    - Return `minStack.top()` in O(1)


#### Code :

```cpp
class MinStack {
public:

    // O(1) time and O(N) space
    
    stack<int> st;
    stack<int> minSt;

    MinStack() {
        
    }
    
    void push(int val) {
        if(st.empty()){
            st.push(val);
            minSt.push(val);
        }else{
            st.push(val);
            minSt.push(min(val,minSt.top()));
        }
    }
    
    void pop() {
        st.pop();
        minSt.pop();
    }
    
    int top() {
        return st.top();
    }
    
    int getMin() {
        return minSt.top();
    }
};

```



> `Time Complexity` : O(1)
> 
> `Space Complexity` : O(n) 


---

### Approach 2 :

- Single Stack with Min Tracking (O(1) Time, O(1) Extra Space)
- ### **Idea**
- Store pairs `(value, current_min)` in the same stack  
    OR
- Store only value but encode previous min in the stack (space-optimized trick)
    
**Encoding Trick:**

1. Keep `minEle` variable for current min
2. If `val` < `minEle`, push **encoded value**: `2*val - minEle`, and update `minEle = val`
3. On pop, if top is encoded, decode previous `minEle`:
    `prevMin = 2*minEle - encodedValue`
4. `top()` needs decoding if encoded

#### Code :


```cpp
class MinStack {
public:

    // O(1) time and O(1) space
    
    stack<long> st;
    long min;

    MinStack() {
        min = 1e9;
    }
    
    void push(int val) {
       
        if(st.empty()){
            st.push(val);
            min = val;
        }else if(val >= min){
            st.push(val);
        }else{
            st.push(2LL*val - min);
            min = val;
        }

    }
    
    void pop() {
       if(st.top() >= min){
        st.pop();
       }else{
            int x = st.top();
            min = 2*min - x;
            st.pop();
       }
    }
    
    int top() {
        if(st.top() >= min){
        return st.top();
       }else{
            int x = st.top();
            return min;
       }
    }
    
    int getMin() {
        return min;
    }
};


```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(1)

---
