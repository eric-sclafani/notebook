`Algorithmic complexity` refers to the computing resources needed by an algorithm to solve a problem.

These computing resources can be the time taken for program execution (==time complexity==), or the space used in memory during its execution (==space complexity==).

The aim is to minimize these resources, so an **algorithm that takes less time and space is considered more efficient**. Complexity is usually expressed using ==Big O notation==, which describes the **upper bound of time or space needs**, and explains how they grow in relation to the input size.

It's important to analyze and understand the algorithmic complexity to choose or design the most efficient algorithm for a specific use-case.

## Big O notation

==Big O notation== is a way of describing the efficiency of a piece of code. It describes the worst case scenario for performance and helps us have a standardized way of answering the following questions:

- How fast is this code?
- How much **additional** space does this piece of code consume?

The "O" stands for **order of growth** which essentially means the magnitude of how fast something grows.

More specifically, how much complexity a function has in response to input **_n_**.

> [!note] Side Note:
> 
> Big-Theta (Θ) notation is a way to describe the typical or average-case time complexity
> 
> Big Omega notation (Ω) describes the the best-case time complexity of an algorithm.


## Time complexity

**Time complexity** is the measure of how much runtime an algorithm has in response to the input **_n_**.  As input **_n_** grows, how much time does the algorithm take to complete?

1. ==O(1)== — Constant Time: The algorithm’s running time does not depend on the size of the input; it performs a fixed number of operations.

2. ==O(log n)== — Logarithmic Time: The algorithm’s running time grows logarithmically with the size of the input.

3. ==O(n)== — Linear Time: The algorithm’s running time scales linearly with the size of the input.

4. ==O(n log n)== — Linearithmic Time: The algorithm’s running time grows in proportion to n times the logarithm of n.

5. ==O(n²)== — Quadratic Time: The algorithm’s running time is directly proportional to the square of the input size.

6. ==O(2^n)== — Exponential Time: The algorithm’s running time doubles with each increase in the input size.

7. ==O(n!)== — Factorial Time: grows proportionally to the factorial of the input size (n).

![[bigo.jpg|500]]

## Space complexity

**Space complexity** in Big O notation **measures the amount of memory** used by an algorithm with respect to the size of its input. It represents the worst-case memory consumption as the input size increases.

1. ==O(1)== — Constant Space: The algorithm uses a fixed amount of memory that does not depend on the input size.

2. ==O(n)== — Linear Space: The algorithm’s memory usage grows linearly with the input size.

3. ==O(n²)== — Quadratic Space: The algorithm’s memory usage increases proportionally to the square of the input size.