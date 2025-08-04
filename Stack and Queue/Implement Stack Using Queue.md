[Problem Link](https://leetcode.com/problems/implement-stack-using-queues/description/)
### Problem Statement : 

Implement a **Stack** using a single **Queue**.
## Examples


### Approach 1 :

- When pushing a new element, we:
    1. **Enqueue** it normally.
    2. **Rotate the queue** so that the new element comes to the **front**.
- This ensures that the **front** of the queue is always the **top** of the stack.

### **Steps**

**push(x)**:
1. Enqueue `x` into the queue.
2. For all previous elements in the queue, dequeue and enqueue again (rotate).
    - This moves `x` to the front.
        
**pop()**:
- Just dequeue from the queue.
    
**top()**:
- Return the front of the queue.
    
**empty()**:
- Check if queue is empty.

#### Code :

``` cpp
class MyStack {
public:


    queue<int> q;
    MyStack() {
        
    }
    
    void push(int x) {

        q.push(x);
        int count = q.size();
        while(count > 1){
            int f = q.front();
            q.pop();
            q.push(f);
            count --;
        }
    }
    
    int pop() {
        int res = q.front(); 
       q.pop();
       return res;
    }
    
    int top() {
        return q.front();
    }
    
    bool empty() {
        return q.empty();
    }
};
```

|Operation|Complexity|
|---|---|
|Push|O(n)|
|Pop|O(1)|
|Top|O(1)|
|Empty|O(1)|

---
