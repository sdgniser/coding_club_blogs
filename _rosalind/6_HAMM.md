---
layout: post
title: "Counting Point Mutations"
problem: 6
permalink: /problems/counting-point-mutations/
date: 2025-01-17
---

## Counting Point Mutations

Given two strings `s` and `t` of equal length, the Hamming distance between `s` and `t`, denoted  $$d_H(s,t)$$, is the number of corresponding symbols that differ in `s` and `t`. See Figure 2.

**Given:** Two DNA strings `s` and `t` of equal length (not exceeding 1 kbp).

**Return:** The Hamming distance $$d_H(s,t)$$.

**Sample Dataset:**

```plaintext
GAGCCTACTAACGGGAT
CATCGTAATGACGGCCT
```

**Sample Output:**

```plaintext
7
```

# Solution

```python
""""
We iterate through both the strings simulatenously and compare what all characters are different
"""

def hamming_distance(s, t):
    count = 0    
    for i in range(len(s)):
        if s[i] != t[i]:
            count += 1
    return count

def hamming(s, t):  # one liner because why not
    return sum([1 for i in range(len(s)) if s[i] != t[i]])

# Read the input data
with open('rosalind_hamm.txt', 'r') as file:
    s = file.readline().strip()
    t = file.readline().strip()

print(hamming_distance(s, t))
```
