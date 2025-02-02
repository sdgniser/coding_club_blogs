---
layout: post
title: "Consensus and Profile"
problem: 10
permalink: /problems/concensus-and-profile/
date: 2025-01-17
---

## Consensus and Profile

# Problem

A matrix is a rectangular table of values divided into rows and columns. An $m \times n$ matrix has $m$ rows and $n$ columns. Given a matrix $A$, we write $A_{i,j}$ to indicate the value found at the intersection of row $i$ and column $j$.

Say that we have a collection of DNA strings, all having the same length $n$. Their profile matrix is a $4 \times n$ matrix $P$ in which $P_{1,j}$ represents the number of times that 'A' occurs in the $j$th position of one of the strings, $P_{2,j}$ represents the number of times that 'C' occurs in the $j$th position, and so on (see below).

A consensus string $c$ is a string of length $n$ formed from our collection by taking the most common symbol at each position; the $j$th symbol of $c$ therefore corresponds to the symbol having the maximum value in the $j$th column of the profile matrix. Of course, there may be more than one most common symbol, leading to multiple possible consensus strings.

```
DNA Strings
A T C C A G C T
G G G C A A C T
A T G G A T C T
A A G C A A C C
T T G G A A C T
A T G C C A T T
A T G G C A C T

Profile
A   5 1 0 0 5 5 0 0
C   0 0 1 4 2 0 6 1
G   1 1 6 3 0 1 0 0
T   1 5 0 0 0 1 1 6

Consensus
A T G C A A C T
```

Given: A collection of at most 10 DNA strings of equal length (at most 1 kbp) in FASTA format.

Return: A consensus string and profile matrix for the collection. (If several possible consensus strings exist, then you may return any one of them.)

## Sample Dataset

```
>Rosalind_1
ATCCAGCT
>Rosalind_2
GGGCAACT
>Rosalind_3
ATGGATCT
>Rosalind_4
AAGCAACC
>Rosalind_5
TTGGAACT
>Rosalind_6
ATGCCATT
>Rosalind_7
ATGGCACT
```

## Sample Output

```
ATGCAACT
A: 5 1 0 0 5 5 0 0
C: 0 0 1 4 2 0 6 1
G: 1 1 6 3 0 1 0 0
T: 1 5 0 0 0 1 1 6
```


# Solution

```python
"""
Consensus and Profile
"""

# read_fasta from GC
def read_fasta(file):
    sequences = {}
    with open(file, 'r') as f:
        for line in f:
            if line.startswith('>'):
                seq_id = line[1:].strip()
                sequences[seq_id] = ''
            else:
                sequences[seq_id] += line.strip()
    return sequences


# Getting profile matrix and storing in a dictionary
def get_profile_matrix(sequences):
    n = len(sequences[list(sequences.keys())[0]])
    profile = {'A': [0] * n, 'C': [0] * n, 'G': [0] * n, 'T': [0] * n}
    for seq in sequences.values():
        for i in range(len(seq)):
            profile[seq[i]][i] += 1
    return profile


def get_consensus(profile):
    consensus = ''
    for i in range(len(profile['A'])):
        max_count = 0
        max_nucleotide = ''
        for nucleotide, counts in profile.items():
            if counts[i] > max_count:
                max_count = counts[i]
                max_nucleotide = nucleotide
        consensus += max_nucleotide
    return consensus

def output_profile_matrix(profile):
    for nucleotide, counts in profile.items():
        print(f'{nucleotide}:', *counts)

file = 'rosalind_cons.txt'
sequences = read_fasta(file)
profile = get_profile_matrix(sequences)
consensus = get_consensus(profile)

# print(consensus)
# output_profile_matrix(profile)

# Writing output to file
with open('output.txt', 'w') as f:
    f.write(f'{consensus}\n')
    for nucleotide, counts in profile.items():
        f.write(f'{nucleotide}: {" ".join(map(str, counts))}\n')
```
