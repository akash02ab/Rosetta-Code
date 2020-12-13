# 100 Doors

## Problem Statement

There are 100 doors in a row that are all initially closed. You make 100 passes by the doors. The first time through, visit every door and 'toggle' the door (if the door is closed, open it; if it is open, close it). The second time, only visit every 2nd door (i.e., door #2, #4, #6, ...) and toggle it. The third time, visit every 3rd door (i.e., door #3, #6, #9, ...), etc., until you only visit the 100th door.

Implement a function to determine the state of the doors after the last pass. Return the final result in an array, with only the door number included in the array if it is open.


## Solution

```
let's take a number between 1 and 100 as n.

let n = 50

all the factors of 50 are [1, 2, 5, 10, 25, 50]

also,
1 x 50 = 50
2 x 25 = 50
5 x 25 = 50

note that 50 has 6 factors (even).

let n = 36

all the factors of 36 are [1, 2, 3, 4, 6, 9, 12, 18, 36]

note that 36 has 9 factors (odd).

also,
1 x 36 = 36
2 x 18 = 36
3 x 12 = 36
4 x 9 = 36
6 x 6 = 36 (since all the other factors are paired up)

So, we can conclude that door will left open for numbers having odd number of factors and the numbers that have odd number of factors are the one having perfect squares.
```

<br>

### Python Implementation

```python
def findOpenDoors(n):
    d = []
    
    for i in range(1, 101):
        sqrt = i ** 0.5
        
        if sqrt == int(sqrt):
            d.append(i)

    return d

n = 100

print(findOpenDoors(n))
```

<br>

### Javascript Implementation

```javascript
var findOpenDoors = (n) => {
    d = [];

    for(var i=1; i<=100; i++){
        var sqrt = Math.sqrt(i);

        if(sqrt === (sqrt | 0))
            d.push(i);
    }

    return d;
}

var n = 100;

console.log(findOpenDoors(n));
```
