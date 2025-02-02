---
layout: post
title: "Computing GC Content"
problem: 5
permalink: /problems/computing-gc-content/
date: 2025-01-12
---

## Problem

The GC-content of a DNA string is given by the percentage of symbols in the string that are 'C' or 'G'. For example, the GC-content of "AGCTATAG" is 37.5%. Note that the reverse complement of any DNA string has the same GC-content.

DNA strings must be labeled when they are consolidated into a database. A commonly used method of string labeling is called FASTA format. In this format, the string is introduced by a line that begins with '>', followed by some labeling information. Subsequent lines contain the string itself; the first line to begin with '>' indicates the label of the next string.

In Rosalind's implementation, a string in FASTA format will be labeled by the ID "Rosalind_xxxx", where "xxxx" denotes a four-digit code between 0000 and 9999.

## Given

At most 10 DNA strings in FASTA format (of length at most 1 kbp each).

## Return

The ID of the string having the highest GC-content, followed by the GC-content of that string. Rosalind allows for a default error of 0.001 in all decimal answers unless otherwise stated; please see the note on absolute error below.

## Sample Dataset

```python
>Rosalind_6404
CCTGCGGAAGATCGGCACTAGAATAGCCAGAACCGTTTCTCTGAGGCTTCCGGCCTTCCC
TCCCACTAATAATTCTGAGG
>Rosalind_5959
CCATCGGTAGCGCATCCTTAGTCCAATTAAGTCCCTATCCAGGCGCTCCGCCGAAGGTCT
ATATCCATTTGTCAGCAGACACGC
>Rosalind_0808
CCACCCTCGTGGTATGGCTAGGCATTCAGGAACCGGAGAACGCTTCAGACCAGCCCGGAC
TGGGAACCTGCGGGCAGTAGGTGGAAT
```

## Sample Output

```plaintext
Rosalind_0808
60.919540
```

## Solution

```python
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

def gc_content(sequence):
    gc = 0
    for base in sequence:
        if base in 'GC':
            gc += 1
    return gc / len(sequence) * 100


def highest_gc_content(sequences):
    highest_gc = 0
    highest_gc_id = ''
    for seq_id, sequence in sequences.items():
        gc = gc_content(sequence)
        if gc > highest_gc:
            highest_gc = gc
            highest_gc_id = seq_id
    return highest_gc_id, highest_gc


file = 'rosalind_gc.txt'
sequences = read_fasta(file)
highest_gc_id, highest_gc = highest_gc_content(sequences)

with open('output.txt', 'w') as f:
    f.write(f'{highest_gc_id}\n{highest_gc}')
```
