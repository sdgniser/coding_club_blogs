---
layout: post
title: "Finding a Motif in DNA"
problem: 9
permalink: /problems/finding-a-motif-in-dna/
date: 2025-01-17
---

## Combing Through the Haystack

### Problem

Given two strings `s` and `t`, `t` is a substring of `s` if `t` is contained as a contiguous collection of symbols in `s` (as a result, `t` must be no longer than `s`).

The position of a symbol in a string is the total number of symbols found to its left, including itself (e.g., the positions of all occurrences of 'U' in "AUGCUUCAGAAAGGUCUUACG" are 2, 5, 6, 15, 17, and 18). The symbol at position `i` of `s` is denoted by `s[i]`.

A substring of `s` can be represented as `s[j:k]`, where `j` and `k` represent the starting and ending positions of the substring in `s`; for example, if `s = "AUGCUUCAGAAAGGUCUUACG"`, then `s[2:5] = "UGCU"`.

The location of a substring `s[j:k]` is its beginning position `j`; note that `t` will have multiple locations in `s` if it occurs more than once as a substring of `s` (see the Sample below).

**Given:** Two DNA strings `s` and `t` (each of length at most 1 kbp).

**Return:** All locations of `t` as a substring of `s`.

### Sample Dataset

```
GATATATGCATATACTT
ATAT
```

### Sample Output

```
2 4 10
```

# Solution 

```python
"""
Combing Through The Haystack
"""

def find_motif(DNA, motif):
    positions = []
    for i in range(len(DNA) - len(motif) + 1):
        if DNA[i:i + len(motif)] == motif:
            positions.append(i + 1)
    
    return positions

print(*find_motif('GATATATGCATATACTT', 'ATAT'))

with open('rosalind_subs.txt', 'r') as f:
    DNA = f.readline().strip()
    motif = f.readline().strip()
    positions = find_motif(DNA, motif)

# Writing output to file
with open('output.txt', 'w') as f:
    f.write(' '.join(map(str, positions)))
```