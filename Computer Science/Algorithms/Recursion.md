
A `recursive function` is one that **repeatedly calls itself until a base condition is met**. This effectively creates a loop similar to a while loop. Each function call gets placed on top of the computer call stack. If not set up correctly, it is easy to run into a stack overflow error, where too many function calls are pushed to the stack.

Why not just use a while loop? You can, and in many languages, should. But sometimes, a recursive solution can be more simple.

A great example is factorial, the product of all positive integers less than or equal to a given non-negative integer 'n'.

$$

n! = \begin{cases} 
n \times (n-1)!  \;\;\;\; if \;\; n \ge1\\ 1 \;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\; otherwise\;\;(if \;\;n=0)
\end{cases}$$

So `3!` can be enumerated as:

$$
 \begin{align}
3! = 3 \times 2!\\ 2! = 2 \times 1!\\1! = 1 \times 0! 
\end{align}
$$

Implementing factorial using recursion looks like this:

```csharp
public int factorialRecursive(int n){
	if (n == 1 || n == 0){
		return 1;
	}
	return n * factorialRecursion(n-1); 
}
```

As a while loop, this would look like:

```csharp
public int factorialLoop(int n){
	var fact = 1;
	while (n >= 1){
		fact = n * n-1;
		n--;
	}
	return fact;
}
```

Notice how with the while loop, it requires more logic to reach the same goal.

Another common recursion example is the fibonacci sequence:

```csharp
public int Fibonacci(int n)  
{  
    return n switch  
    {  
       0 => 0,  
       1 => 1,  
       _ => Fibonacci(n - 1) + Fibonacci(n - 2)  
    };
}
```


## Tail call recursion

`Tail call recursion` is a form of recursion where the recursive call is the very last operation performed within a function. This means that after the recursive call is made, there are no further computations or operations to be performed by the current function's stack frame.

The non-tail call definition of factorial looks like this. Note how the multiplication occurs **AFTER** the recursive call:

```ps
function factorial(n):
  if n == 0:
    return 1
  else:
    return n * factorial(n - 1)
```

This contrasts with the tail call definition, where the calculations are done before the recursive call, often using an accumulator:

```ps
function tail_factorial(n, accumulator):
  if n == 0:
    return accumulator
  else:
    return tail_factorial(n - 1, n * accumulator)
```


The key difference is that some programming languages implement `tail call optimization (TCO)` which allows recursive calls like this to effectively be "transformed" into iteration at the machine level, vastly improving memory efficiency. This also prevents stack overflow errors from occurring. 

Not all languages implement this patten unfortunately.