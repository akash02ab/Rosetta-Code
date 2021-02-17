# Abundant, deficient and perfect number classifications

## Problem Statement

These define three classifications of positive integers based on their proper divisors.

Let  P(n)  be the sum of the proper divisors of n where proper divisors are all positive integers n other than n itself.

If P(n) < n then n is classed as deficient

If P(n) === n then n is classed as perfect

If P(n) > n then n is classed as abundant

Example: 6 has proper divisors of 1, 2, and 3. 1 + 2 + 3 = 6, so 6 is classed as a perfect number.

<br>

| Input | Output |
| --- | --- |
| `n`: positive integers | `arr[]`: count of abundant, <br> perfect, and deficient number |

<br>

## Example 

```
input:
20000

output:
[15043, 4, 4953]
```

<br>

## Solution

* For each integer from `1` to `n`:
    * Calculate `sum` of all factors of `current` integer
    * Compare with the `current` integer
        * If `sum` is equal to `current` integer, increment `perfect` 
        * If `sum` is less than `current` integer, increment `abundant`
        * If `sum` is more than `current` integer, increment `deficient`
* return `[deficient, perfect, abundant]`

<br>

## Python Implementation

```python 
def getFactorSum(n):
    sum = 0

    for i in range(1, n // 2 + 1):
        if n % i == 0:
            sum += i 

    return sum 

def getDPA(n):
    deficient, perfect, abundant = 0, 0, 0

    for i in range(1, n + 1):
        sum = getFactorSum(i)
        
        if sum == i:
            perfect += 1
        elif sum > i:
            abundant += 1
        else:
            deficient += 1
    
    return [deficient, perfect, abundant]

n = int(input())

print(getDPA(n))
```

<br>

## Time and Space Complexity
T(n) = **O**(n<sup>2</sup>)
<br>S(n) = **O**(1)