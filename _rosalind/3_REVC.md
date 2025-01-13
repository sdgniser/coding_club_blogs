---
layout: post
title: Complementing a Strand of DNA
problem: 3
permalink: /problems/complementing-a-strand-of-dna/
date: 2025-01-12
---

In DNA strings, symbols 'A' and 'T' are complements of each other, as are 'C' and 'G'.
## Problem

The reverse complement of a DNA string `s` is the string s<sup>c</sup> formed by reversing the symbols of `s`, then taking the complement of each symbol (e.g., the reverse complement of "GTCA" is "TGAC").

**Given**  
A DNA string `s` of length at most 1000 bp.

**Return**  
The reverse complement s<sup>c</sup> of `s`.

### Sample Dataset
```
AAAACCCGGT
```

### Sample Output
```
ACCGGGTTTT
```

# Solution

```python
"""
Our task is to find the reverse complement of a DNA string.
We first read the DNA string from the file and then reverse the string.
Then we replace each nucleotide with its complement.
"""

# Replace each nucleotide with its complement
complement = {'A': 'T', 'T': 'A', 'C': 'G', 'G': 'C'}

def reverse_complement(dna):
    reverse_dna = dna[::-1] # Reverse the DNA string
    reverse_complement = "".join(complement[nucleotide] for nucleotide in reverse_dna)
    return reverse_complement


# Or you could use if-else statements to replace each nucleotide with its complement
def reverse_complement2(dna):
    reverse_complement = ""
    for n in dna:
        if n == 'A':
            reverse_complement += 'T'
        elif n == 'T':
            reverse_complement += 'A'
        elif n == 'C':
            reverse_complement += 'G'
        elif n == 'G':
            reverse_complement += 'C'
    reverse_complement = reverse_complement[::-1]
    return reverse_complement


# Read the input data
with open('rosalind_revc.txt', 'r') as file:
    input_dna = file.read().strip()

reverse_complement_dna = reverse_complement(input_dna)

# Write the output to a file
with open('output.txt', 'w') as file:
    file.write(reverse_complement_dna)

```