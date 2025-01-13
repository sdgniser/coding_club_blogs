---
layout: post
title: Transcribing DNA into RNA
problem: 2
permalink: /problems/transcribing-dna-into-rna/
date: 2025-01-12
---

An RNA string is a string formed from the alphabet containing 'A', 'C', 'G', and 'U'.
# Problem

Given a DNA string `t` corresponding to a coding strand, its transcribed RNA string `u` is formed by replacing all occurrences of 'T' in `t` with 'U' in `u`.

**Given** A DNA string `t` having length at most 1000 nt.

**Return** The transcribed RNA string of `t`.

### Sample Dataset
```
GATGGAACTTGACTACGTAAATT
```

### Sample Output
```
GAUGGAACUUGACUACGUAAAUU
```

# Solution
```python
"""
We can iterate over the DNA sequence and replace each 'T' with 'U' to get the RNA sequence.
You can also use the inbuit replace function to replace all the occurences of a character in a string.
"""

# Read the input data
with open('rosalind_rna.txt', 'r') as f:
    dna_sequence = f.read().strip()

# Iterating over the DNA sequence and replacing 'T' with 'U'
def dna_to_rna(dna_sequence):
    rna_sequence = ""
    for base in dna_sequence:
        if base == 'T':
            rna_sequence += 'U'
        else:
            rna_sequence += base
    return rna_sequence


# Inbuilt replace function to replace 'T' with 'U'
def dna_to_rna(dna_sequence):
    return dna_sequence.replace('T', 'U')


# dna_sequence = "GATGGAACTTGACTACGTAAATT"
rna_sequence = dna_to_rna(dna_sequence)
print(rna_sequence)
```

