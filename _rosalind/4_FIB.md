---
layout: post
title: "Rabbits and Recurrence Relations"
problem: 4  
permalink: /problems/rabbits-and-recurrence-relations/
date: 2025-01-12
---

# Problem

A sequence is an ordered collection of objects (usually numbers), which are allowed to repeat. Sequences can be finite or infinite. Two examples are the finite sequence $(\pi, \sqrt{-2}, 0, \pi)$ and the infinite sequence of odd numbers $(1, 3, 5, 7, 9, \ldots)$. We use the notation $a_n$ to represent the $n$-th term of a sequence.

A recurrence relation is a way of defining the terms of a sequence with respect to the values of previous terms. In the case of Fibonacci's rabbits from the introduction, any given month will contain the rabbits that were alive the previous month, plus any new offspring. A key observation is that the number of offspring in any month is equal to the number of rabbits that were alive two months prior. As a result, if $F_n$ represents the number of rabbit pairs alive after the $n$-th month, then we obtain the Fibonacci sequence having terms $F_n$ that are defined by the recurrence relation $F_n = F_{n-1} + F_{n-2}$ (with $F_1 = F_2 = 1$ to initiate the sequence). Although the sequence bears Fibonacci's name, it was known to Indian mathematicians over two millennia ago.

When finding the $n$-th term of a sequence defined by a recurrence relation, we can simply use the recurrence relation to generate terms for progressively larger values of $n$. This problem introduces us to the computational technique of dynamic programming, which successively builds up solutions by using the answers to smaller cases.

## Given

Positive integers $n \leq 40$ and $k \leq 5$.

## Return

The total number of rabbit pairs that will be present after $n$ months, if we begin with 1 pair and in each generation, every pair of reproduction-age rabbits produces a litter of $k$ rabbit pairs (instead of only 1 pair).


## Sample Dataset
```
5 3
```

## Sample Output
```
19
```

# Solution
```python
"""
We can solve this problem using dynamic programming.
We can use a list to store the number of rabbit pairs at each month.
We can initialize the list with 1 pair of rabbits for the first month.
For each month, we can calculate the number of rabbit pairs by adding the number of rabbit pairs from the previous month and the number of rabbit pairs from two months ago multiplied by the number of offspring pairs.

This would be our recurrence relation:
F(n) = F(n-1) + k * F(n-2)
"""

def rabbit_pairs(n, k):
    # Initialize the list with 1 pair of rabbits for the first month
    rabbit_pairs = [1, 1]
    for i in range(2, n):
        # Calculate the number of rabbit pairs by adding the number of rabbit pairs from the previous month and the number of rabbit pairs from two months ago multiplied by the number of offspring pairs
        rabbit_pairs.append(rabbit_pairs[i - 1] + rabbit_pairs[i - 2] * k)
    return rabbit_pairs[-1]


def rabbit_pairs_recursive(n, k, dp_table=None):
    if dp_table is None:
        dp_table = {}
    if n in dp_table:
        return dp_table[n]
    if n == 1 or n == 2:
        result = 1
    else:
        result = rabbit_pairs_recursive(n - 1, k, dp_table) + rabbit_pairs_recursive(n - 2, k, dp_table) * k
    dp_table[n] = result
    return result

months = 5
litter_size = 3
print(rabbit_pairs(months, litter_size))

# Reading data from file
with open('rosalind_fib.txt', 'r') as f:
    months, litter_size = map(int, f.readline().split())

# Writing data to file
with open('output.txt', 'w') as f:
    f.write(str(rabbit_pairs(months, litter_size)))

```

