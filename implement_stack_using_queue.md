# Implement Stack using Queue

Implement the following operations of a stack using queues.

```push(x)``` -- Push element x onto stack.

```pop()``` -- Removes the element on top of the stack.

```top()``` -- Get the top element.

```empty()``` -- Return whether the stack is empty.

Notes:
You must use only standard operations of a queue -- which means only push to back, peek/pop from front, size, and is empty operations are valid.




---


###Two Approaches

We can simulate s stack using queue by either changing the behavior of queue's ```pop()```, or ```push()```, which yields two different solutions.

###Changing pop()



```
class Stack {
public:
    // Push element x onto stack.
    void push(int x) {
        myQueue.push(x);
    }

    // Removes the element on top of the stack.
    void pop() {
        int size = myQueue.size();
        queue<int> temp;
        for(int i = 0; i < size - 1; i++){
            int front = myQueue.front();
            temp.push(front);
            myQueue.pop();
        }
        myQueue = temp;
        
    }

    // Get the top element.
    int top() {
        return myQueue.back();
    }

    // Return whether the stack is empty.
    bool empty() {
        return myQueue.empty();
    }
private:
    queue<int> myQueue;
};
```


###Changing push()

```
class Stack {
public:
    // Push element x onto stack.
    void push(int x) {
        myQueue.push(x);
        int size = myQueue.size();
        for(int i = 0; i < size - 1; i++){
            myQueue.push(myQueue.front());
            myQueue.pop();
        }
    }

    // Removes the element on top of the stack.
    void pop() {
        myQueue.pop();
    }

    // Get the top element.
    int top() {
        return myQueue.front();
    }

    // Return whether the stack is empty.
    bool empty() {
        return myQueue.empty();
    }
private:
    queue<int> myQueue;
};
```