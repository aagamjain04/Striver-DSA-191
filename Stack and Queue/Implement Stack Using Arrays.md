

### Operations :

| Operation   | Time Complexity | Description                                      |
| ----------- | --------------- | ------------------------------------------------ |
| **Push**    | O(1) amortized  | Add element at `++top`, resize if full.          |
| **Pop**     | O(1)            | Reduce `top` pointer.                            |
| **Peek**    | O(1)            | Return `arr[top]` without removing.              |
| **isEmpty** | O(1)            | Check if `top == -1`.                            |
| isFull      | O(1)            | Check if `top == capacity-1`                     |
| **Resize**  | O(n)            | Copy elements to a new array of double capacity. |
`Amortized time complexity is a way to analyze the average cost of operations over a sequence of operations, rather than looking at the worst-case cost of a single operation.`

### Code : 

``` cpp
#include <bits/stdc++.h>
using namespace std;

class Stack {

private:
    int *arr;
    int top;
    int capacity;

    void resize(){

        int newCapacity = capacity * 2;
        int *newArr = new int[newCapacity];
        for(int i=0;i<capacity;i++){
            newArr[i] = arr[i];
        }
        delete[] arr;
        arr = newArr;
        capacity = newCapacity;
        cout << "Resized to capacity: " << capacity << "\n";

    }

public:


    Stack(){
        arr = new int[2]; // Small initial size, will get bigger with resizing
        capacity = 2;
        top = -1;
    }

    void push(int x){
        if(isFull()){
            resize();
        }
        arr[++top] = x;
    }

    void pop(){
        if(isEmpty()){
            cout << "Stack is empty\n";
            return;
        }
        top--;
    }

    int peek(){
        if(isEmpty()){
            cout << "Stack is empty\n";
            return -1;
        }
        return arr[top];
    }

    bool isEmpty(){
        return top==-1;
    }

    bool isFull(){
        return top==capacity-1;
    }

    ~Stack(){
        delete[] arr;
    }




};

int main(){
   
    Stack st;
    st.push(10);
    st.push(20);
    st.push(30);
    st.push(40);

    cout << "Top element " << st.peek() << "\n";

    st.pop();

    cout << "Top element after pop " << st.peek() << "\n";


    return 0;
}


```