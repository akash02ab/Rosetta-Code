# ABC Problem

## Problem Statement

You are given a collection of ABC blocks (e.g., childhood alphabet blocks). There are 20 blocks with two letters on each block. A complete alphabet is guaranteed amongst all sides of the blocks.

Implement a function that takes a string (word) and determines whether the word can be spelled with the given collection of blocks.

Some rules to keep in mind:

* Once a letter on a block is used, that block cannot be used again.
* The function should be case-insensitive.

<br>

| Input | Output |
| --- | --- |
| `arr[20][2]`: blocks <br> `string`: word | `boolean`: `True` if word can be spelled, <br> `False` otherwise |

<br>

## Example

```
input:
B O
X K
D Q
C P
N A
G T
R E
T G
Q D
F S
J W
H U
V I
A N
O B
E R
F S
L Y
P C
Z M
Bark

output:
True
```

<br>

## Solution

1. Convert `word` to upper-case and send `word`, `blocks` to recursive function `canMakeWord`.

2. Base condition is when `word` is empty string then return empty list.

3. Initialize `result` array and split word into two string, first one is first `char` and second one is `remaining word`.

4. For each `block` in `blocks` list:
    * If `char` is found in `block`:
        * Add the `block` to `result` array.
        * Remove `block` from `blocks` list.
        * Recusively call `canMakeWord` with `remaining word` and modified `blocks` list.
        * Return the `result` array.

5. Function will return `None` if `word` can not be spelled otherwise, return list of `block` used to spell the word.

<br>

## Python Implementation

```python
def canMakeWord(word, blocks):
    if word == '':
        return []

    if len(blocks) == 0:
        return None
    
    result = []
    char, remainingWord = word[0], word[1:]
    
    for block in blocks:
        if char in block:
            result.append(block)
            blocks.remove(block)
            
            temp = canMakeWord(remainingWord, blocks)
            
            if temp: result += temp 
            else: return None
            
            return result 

blocks = []
for _ in range(20):
    block = list(map(str, input().split()))
    blocks.append(block)

word = input()

if canMakeWord(word.upper(), blocks):
    print(True)
else:
    print(False)
```

<br>

## Time and Space Complexity
T(n) = **O**(1)
<br> *[Recurrence Relation :- T(n) = T(n - 1) + O(n)]* *(here, n = 20)*
<br>S(n) = **O**(1)
