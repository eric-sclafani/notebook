`RAM` or `R`andom `A`ccess `M`emory (or just `memory` for short) is the computer's short term memory. That is, where data is stored that is currently being used by your applications. RAM typically comes in the form of a **rectangular flat circuit board with memory chips** attached, also referred to as a memory module.

RAM needs to be super fast because the [[CPU]] accesses this data to perform the computations it needs to. For example, when you're playing a video game or watching Netflix, data that the CPU needs for calculations is stored in RAM.

RAM is akin to **short term memory in humans**. Our short term memory stored information that we might need for a brief moment. It is important because it ==makes sure your applications run smoothly== without hiccups. Not having enough memory means all operations will slow down everything on your PC. 

It's important to note that data stored in memory is lost when the application using it closes or computer shuts down. 

# Stack & heap

Computer memory consists of two types: the ==stack== and ==heap==. They both are needed to store different types of data as efficiently as possible.

They both work in unison to prioritize maximum speed and efficiency in computers.

See [this resource](https://courses.grainger.illinois.edu/cs225/fa2022/resources/stack-heap/) for more information

## Stack

The memory `stack` is mainly used for storing things like ==local variables inside functions==, ==function calls==, and ==memory addresses==. It follows the [[Stacks & queues|stack]] architecture (last in, first out) such that the last item to be pushed on the stack is the first one to come off the stack. This is why it handles function calls, since **functions often call other functions and need to wait for the results**.

It is **very fast to allocate and free memory automatically** and the used memory is automatically cleaned up when function calls end.

See [here](https://organicprogrammer.com/2020/08/19/stack-frame/) for more detailed information.

![[stack.jpg|350]]

## Heap

The `heap` is a large pool of memory where data can be stored _anywhere_ and doesn't follow the strict order the stack does.

It stores **objects or data that need to live beyond a single function call** and **pieces of data with unknown sizes** (like a dynamic array or user input). It is ==slower to allocate and free memory== compared to the stack, and you either have to manually deallocate memory when its no longer needed (or a language with **garbage collection** does it for you.)

When data is stored on the heap, the **memory reference** (pointer, memory address) to the object is stored on the stack.