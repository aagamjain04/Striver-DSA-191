[Problem Link](https://leetcode.com/problems/implement-queue-using-stacks/description/)
### Problem Statement : 
//TODO
Implement a **Stack** using a single **Queue**.
## Examples


### Approach 1 :

- Two Stacks — Push O(1), Pop O(n)

### **Idea**

1. **Push(x)**: Push `x` directly into `stack1`. → **O(1)**    
2. **Pop()**:
    - Move all elements from `stack1` to `stack2`.
    - Pop the top element from `stack2`.
    - Move elements back to `stack1`. → **O(n)**
3. **Peek()**: Same as `Pop()`, but just return element without removing.

#### Code :

``` cpp
class MyQueue {
public:


    stack<int> st1,st2;
    MyQueue() {
        
    }
    
    void push(int x) {
        while(!st2.empty()){
            int num = st2.top();
            st2.pop();
            st1.push(num);
        }
        st1.push(x);
        while(!st1.empty()){
            int num = st1.top();
            st1.pop();
            st2.push(num);
        }
    }
    
    int pop() {
        int num = st2.top();
        st2.pop();
        return num;
    }
    
    int peek() {
        return st2.top();
    }
    
    bool empty() {
        return (st2.empty());
    }
};
```

---

### Approach 2 :

- Two Stacks — Amortized O(1)

### **Idea**

- Use two stacks:
    - **input**: For push operations.
    - **output**: For pop operations.
- **Push(x)**: Push into `input`. → **O(1)**
- **Pop()**:
    - If `output` is empty, move all elements from `input` to `output`.
    - Pop from `output`. → **Amortized O(1)**
- **Peek()**:
    - Same as `Pop()` but return without removing.

### Code : 

``` cpp
class MyQueue {
public:


    stack<int> st1,st2;
    MyQueue() {
        
    }
    
    void push(int x) {
        st1.push(x);
    }
    
    int pop() {
        if(!st2.empty()){
            int num = st2.top();
            st2.pop();
            return num;
        }
        
        while(!st1.empty()){
            int num = st1.top();
            st1.pop();
            st2.push(num);
        }
        int num = st2.top();
        st2.pop();
        return num;

    }
    
    int peek() {

        if(!st2.empty()){
            int num = st2.top();
            return num;
        }
        
        while(!st1.empty()){
            int num = st1.top();
            st1.pop();
            st2.push(num);
        }
        int num = st2.top();
    
        return num;

        
    }
    
    bool empty() {
        return (st1.empty() && st2.empty());
    }
};


```

---
