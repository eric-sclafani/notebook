
# Stack
A `stack` is an abstract data type and a linear data structure that follow the last in, first out principle (LIFO). This means that the **last element** added to the stack is the **first one** to be removed (popped) from it.

Stacks are a self-explanatory data structure in that data is "stacked" on top of each other. When data is ==pushed== onto the stack, it goes to the top. When data is ==popped==, the top-most data is removed from it.

==Key Operations:==
- **Push:** Adds an element to the top of the stack.
- **Pop:** Removes and returns the element from the top of the stack.
- **Peek/Top:** Returns the element at the top of the stack without removing it.
- **IsEmpty:** Checks if the stack is empty.
- **IsFull:** Checks if the stack is full (relevant for fixed-size implementations).

![[stack2.jpg]]

Stacks play a crucial role in how function calls work in [[RAM|memory]] by keeping track of the order of function calls. They're also used in applications that have an undo/redo functionality.

# Queue

A `queue` is like a stack, except that they follow the first in, first out principle (FIFO). Aka, the **first element** added to the queue is the first to be popped.

==Key operations:==
- **Enqueue (or Push):** Adds an element to the rear of the queue.
- **Dequeue (or Pop):** Removes and returns the element at the front of the queue. 
- **Peek (or Front):** Returns the element at the front of the queue without removing it. 
- **IsEmpty:** Checks if the queue is empty.
- **IsFull (for array-based implementations):** Checks if the queue has reached its maximum capacity.

![[queue.jpg]]

