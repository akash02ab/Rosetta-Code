# 9 Billion Names of God

## Problem Statement


This task is a variation of the [short story](https://en.wikipedia.org/wiki/The_Nine_Billion_Names_of_God#Plot_summary) by Arthur C. Clarke.

(Solvers should be aware of the consequences of completing this task.)

In detail, to specify what is meant by a "name":

* The integer 1 has 1 name "1".
* The integer 2 has 2 names "1+1" and "2".
* The integer 3 has 3 names "1+1+1", "2+1", and "3".
* The integer 4 has 5 names "1+1+1+1", "2+1+1", "2+2", "3+1", "4".
* The integer 5 has 7 names "1+1+1+1+1", "2+1+1+1", "2+2+1", "3+1+1", "3+2", "4+1", "5".

<br>

## Task 1

* Get all names of gods from 1 to n.

| Input | Output |
| --- | --- |
| `n`: natural number | `arr[][]`: every name of gods from `1` to `n` |

<br>

## Example

```
input:
5

output:
['1']
['1 + 1', '2']
['1 + 1 + 1', '2 + 1', '3']
['1 + 1 + 1 + 1', '2 + 1 + 1', '2 + 2', '3 + 1', '4']
['1 + 1 + 1 + 1 + 1', '2 + 1 + 1 + 1', '2 + 2 + 1', '3 + 1 + 1', '3 + 2', '4 + 1', '5']
```

<br>

## Solution

1. Reach the base condition which is ['1'] name of 1<sup>st</sup> god then recursively build the other god names one by one.

2. To get n<sup>th</sup> god name use the subarray that is already generated in previous recursive call.
    * to get all the name start with 1 goto previous subarray and concatinate 1 with all the name that start with number less than equal to 1.
    * to get all the name start with 2 goto 2<sup>nd</sup> last subarray and concatinate 2 with all the name that start with number less than equal to 2.
    * repeat this to get all the names upto n - 1.

3. Add `n` to the previously generated subarray.

<br>

## Python Implementation

```python
def getAllNames(n):
    if n == 1:
        return [['1']]

    result = getAllNames(n - 1)
    
    temp = []
    k = len(result) - 1
    
    for i in range(1, n):
        j = 0
        l = len(result[k])
        
        while j < l and result[k][j][0] <= str(i):
            temp.append(str(i) + ' + ' + result[k][j])
            j += 1
        
        k -= 1
    
    temp.append(str(n))
    
    result.append(temp)
    
    return result 

n = int(input())

result = getAllNames(n)

for name in result:
    print(name)
```

<br>

## Time and Space Complexity

T(n) = **O**(n<sup>3</sup>)
<br> *[Recurrence Relation :- T(n) = T(n - 1) + O(n<sup>2</sup>)]*
<br>S(n) = **O**(n<sup>2</sup>)
<br> *(stack has to hold all the generated result)*