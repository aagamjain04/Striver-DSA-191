### Operations :

| Operation    | Time Complexity | Description                                        |
| ------------ | --------------- | -------------------------------------------------- |
| **Enqueue**  | O(1) amortized  | Insert at `(rear + 1) % capacity`, resize if full. |
| **Dequeue**  | O(1)            | Move `front` forward.                              |
| **GetFront** | O(1)            | Return `arr[front]`.                               |
| **isEmpty**  | O(1)            | Check `count == 0`.                                |
| isFull       | O(1)            | Check count == capacity                            |
| **Resize**   | O(n)            | Copy elements to new array of double capacity.     |
`Amortized time complexity is a way to analyze the average cost of operations over a sequence of operations, rather than looking at the worst-case cost of a single operation.`


### Code :

``` cpp
#include <bits/stdc++.h>
using namespace std;

class Queue {

private:
    int count;
    int front;
    int rear;
    int *arr;
    int capacity;

    void resize(){

        int newCapacity = capacity * 2;
        int *newArr= new int[newCapacity];

        for(int i=0;i<count;i++){
            newArr[i] = arr[(front + i)%capacity];
        }

        delete[] arr;
        arr = newArr;
        capacity = newCapacity;

        front = 0;
        rear = count-1;

        cout << "Resized to capacity: " << capacity << "\n";
    }

public:

    Queue(){
        count = 0;
        front = 0;
        rear = -1;
        capacity = 2;
        arr = new int[2];
    }

    void enqueue(int x){
        if(isFull()){
            resize();
        }
        rear = (rear + 1)%capacity;
    
        arr[rear] = x;
        count++;

    }

    void dequeue(){
        if(isEmpty()){
            cout << "Queue is empty\n";
            return;
        }
        front = (front + 1) % capacity;
        count--;
    }

    int getFront(){
        if(isEmpty()){
            cout << "Queue is empty\n";
            return -1;
        }
       
        return arr[front];
    };

    bool isEmpty(){
        return (count == 0);
    }

    bool isFull(){
        return (count == capacity);
    }

    ~Queue(){
        delete[] arr;
    }

};


int main(){
    
    Queue q;

    q.enqueue(10);
    q.enqueue(20);
    q.enqueue(30);
    q.enqueue(40);

    cout << "Front: " << q.getFront() << "\n";


    q.dequeue();
    cout << "Front after dequeue: " << q.getFront() << "\n";


    return 0;
}


```