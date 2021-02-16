# 24 Game

## Problem Statement

The aim of the game is to arrange four numbers in a way that when evaluated, the result is 24.

Implement a function that takes a string of four digits as its argument, with each digit from 1 to 9 (inclusive) with repetitions allowed, and returns an arithmetic expression that evaluates to the number 24. If no such solution exists, return "-1".

Only the following operators/functions are allowed: multiplication, division, addition, subtraction

<br>

| Input | Output |
| --- | --- |
| `string`: number | `string`: expression |

<br>

## Example

```
input:
2483

output:
((2+(4-3))*8)
```

<br>

## Solution

1. Find all permutation of given number.
    * *since number is always 4-digit, there will be 4! permutation possible*

2. Find all combination of operator of length 3 from given operations `['+', '-', '*', '/']`.
    * *there will be 4<sup>3</sup> combination of operators*

3. Form all possible expression from 4! permutation of numbers and 4<sup>3</sup> combination of operators.
    * *total number of expression formed will be 4! x 4<sup>3</sup>*

4. All these expression can be evaluated in different ways, form all possible ways of evaluation using parenthesization.
    *  *it turns out that the number of ways to parenthesize an expression with n+1 terms is C<sub>n</sub>, the nth Catalan number*
     <br> **C<sub>n</sub> = <sup>2n</sup>C<sub>n</sub> / (n+1)** 
    * *each expression has 4 operands and 3 operator* <br> C<sub>3</sub> = <sup>6</sup>C<sub>3</sub> / 4 = 5
    * *total number of expression are 4! x 4<sup>3</sup> x C<sub>3</sub> = 24 x 64 x 5 = 7680*

5. Filter the expression based on whether expression evaluation results to 24 or not.

<br>

## Python Implementation

```python
def precedence(operator):
    if operator == '+' or operator == '-':
        return 1 
    if operator == '*' or operator == '/':
        return 2 
    return 0

def solve(a, b, o):
    if o == '+': return a + b 
    if o == '-': return b - a 
    if o == '*': return a * b 
    if o == '/': return b / a if a else 1

def push(stack, item):
    stack.append(item)

def pop(stack):
    return stack.pop()

def top(stack):
    return stack[-1]

def isEmpty(stack):
    return len(stack) == 0 

def eval(expression):
    i, n = 0, len(expression)
    oprStack = []
    valStack = []

    while i < n:
        if expression[i] == '(':
            push(oprStack, '(')
        
        elif expression[i].isdigit():
            val = 0
            while i < n and expression[i].isdigit():
                val = (val * 10) + int(expression[i])
                i += 1 
            
            push(valStack, val)
            i -= 1

        elif expression[i] == ')':
            while not isEmpty(oprStack) and top(oprStack) != '(':
                a = pop(valStack)
                b = pop(valStack)
                o = pop(oprStack)
                push(valStack, solve(a, b, o))
            
            pop(oprStack)
        
        else:
            while not isEmpty(oprStack) and precedence(top(oprStack)) > precedence(expression[i]):
                a = pop(valStack)
                b = pop(valStack)
                o = pop(oprStack)
                push(valStack, solve(a, b, o))
            
            push(oprStack, expression[i])
        
        i += 1
    
    while not isEmpty(oprStack):
        a = pop(valStack)
        b = pop(valStack)
        o = pop(oprStack)

        push(valStack, solve(a, b, o))
    
    return top(valStack) == 24

def parenthesization(expression, size):
    result = []
    
    if size == 1:
        return [expression]

    for i in range(size):
        if expression[i] in ['+', '-', '*', '/']:
            left = parenthesization(expression[0 : i], i)
            right = parenthesization(expression[i + 1 : size], size - i - 1)
            
            for lexp in left:
                for rexp in right:
                    result.append('(' + lexp + expression[i] + rexp + ')')
    
    return result

def buildExpression(numbers, operations):
    expression = []
    
    for number in numbers:
        for operation in operations:
            exp = str(number[0])
            
            for i in range(1, len(number)):
                operator = operation[i - 1]
                variable = number[i]

                if operator == '+':
                    exp += '+' + str(variable)
                elif operator == '-':
                    exp += '-' + str(variable)
                elif operator == '*':
                    exp += '*' + str(variable)
                elif operator == '/':
                    exp += '/' + str(variable)    
        
            expression.append(exp)
    
    return expression

def combination(arr, size):
    result = []

    for i in range(size):
        for j in range(size):
            for k in range(size):
                result.append(arr[i] + arr[j] + arr[k])

    return result  

def swap(arr, i, j):
    arr[i], arr[j] = arr[j], arr[i]

def permutate(arr, i, size):
    result = []
    
    if i == size:
        result.append([int(num) for num in arr])
        return result
    
    for j in range(i, size):
        swap(arr, i, j)
        result += permutate(arr, i + 1, size)
        swap(arr, i, j)
    
    return result 

def solve24(n):
    number = list(n)
    
    numbers = permutate(number, 0, len(number))
    
    operations = combination(['+', '-', '*', '/'], 4)
    
    expressions = buildExpression(numbers, operations)
    
    result = []
    for expression in expressions:
        result += parenthesization(expression, len(expression))
    
    return list(filter(eval, result))


n = input()

sol = solve24(n)

if len(sol) == 0:
    print(-1)
else:
    print(sol[0])
```

<br>

## Time and Space Complexity

T(n) = **O**(1)
<br>S(n) = **O**(1)
<br>*(number of operation is constant i.e only 7680 operations)*