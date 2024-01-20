---
tags:
  - design-and-analysis-of-algorithms
---

Wow anshie talks fast

Review Lectures

Big question:
## What makes a good algorithm?
A good algorithm (in no specific order):
- **is correct**
- **is fast**
- has low memory usage
- is consistent (in accuracy and speed)
- is suited for the context (output-sensitive)
- is suited for the general case (output-insensitive)
- is easy to understand/implement/use
- is easy to check for correctness
Anshie's additions
- is easy to parallelize
- is good for checkpointing (easy to save the state of the computation)

## Worst-case Asymptotic Runtime
Worst-Case: we care that in the worst possible case we still have a good speed
It's hard to identify the "average" use, so doing worst-case optimizes that all inputs will run fast
Asymptotic: We care that for big inputs it runs fast
Sometimes the constant matters, but its easier to optimize on a constant level versus breaking a new asymptotic bound
$$O(\log n) < O(n) < O(n\log n) < O(n^2) < O(n^3) < O(n^k) < O(2^n) < O(k^n) < O(n!)$$

# Prime Checking Algorithms
## Algorithm 1
```
Given: integer n > 2

for k = 2 to n-1
	Test if n is divisible by k
	If yes, return "Not Prime"
return "Prime"
```

Is this $O(n)$?
Is this linear-time?

First, what is $n$? 
$n$ is the size of the input. More accurately, it is the number of bits to write down the input
If $n$ were the integer $n$ from the pseudocode, then yes! This is $O(n)$
But if $n$ is the input size, then no, this isn't $O(n)$

When we say linear-time, we mean linear with respect to the input, so this algorithm is not linear-time, but it is $O(n)$!
To reduce confusion, we might change the pseudocode to not use $n$ but instead $x$, we could then plainly say it is $O(x)$

The size of the input is $\log_2 x = n$, so $O(x) = O(2^n)$
