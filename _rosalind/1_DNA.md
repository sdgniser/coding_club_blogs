---
layout: post
title: Counting DNA Nucleotides
problem: 1
permalink: /problems/counting-dna-nucleotides/
date: 2025-01-12
---

# Problem

A string is simply an ordered collection of symbols selected from some alphabet and formed into a word; the length of a string is the number of symbols that it contains.

An example of a length 21 DNA string (whose alphabet contains the symbols 'A', 'C', 'G', and 'T') is "ATGCTTCAGAAAGGTCTTACG."

**Given:** A DNA string `s` of length at most 1000 nt.

**Return:** Four integers (separated by spaces) counting the respective number of times that the symbols 'A', 'C', 'G', and 'T' occur in `s`.

## Sample Dataset

```
AGCTTTTCATTCTGACTGCAACGGGCAATATGTCTCTGTGTGGATTAAAAAAAGAGTGTCTGATAGCAGC
```

## Sample Output

```
20 12 17 21
```

# Solution

```python
with open('rosalind_dna.txt', 'r') as f:
    dna = f.read().strip()

# Or you could directly copy the input string and paste it as a variable 

# Method 1 using dictionary
nucleotide_count = {'A': 0, 'C': 0, 'G': 0, 'T': 0}
for n in dna:
    nucleotide_count[n] += 1

print(nucleotide_count['A'], nucleotide_count['C'], nucleotide_count['G'], nucleotide_count['T'])


# If you're unfamiliar with dictionaries, you can just use 4 variables to store the count of each nucleotide

A, C, G, T = 0, 0, 0, 0
for n in dna:
    if n == 'A':
        A += 1
    elif n == 'C':
        C += 1
    elif n == 'G':
        G += 1
    elif n == 'T':
        T += 1


# Method 2 using count() method
print(dna.count('A'), dna.count('C'), dna.count('G'), dna.count('T')) 
```
