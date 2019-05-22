# Lambda Calculus from the Ground Up

## Rules

What if your style guide was so limited that it only offers you **single argument functions** and nothing else? 

* No numbers
* No modules
* No objects
* No strings
* No datatypes

## Definition

> You take the thing in paranthesis and you substitute the `x`. 
> - David Beazley

```python
f = lambda x: 3*x + 1
```
```python
>>> f(4) # Replace x with 4
13
```


## Switch

Could you model a simple electrical switch?

```python
def LEFT(a):
    def f(b):
        return a
    return f

def RIGHT(a):
    def f(b):
        return b
    return f
```

```python
>>> LEFT('5v')('gnd')
'5v'

>>> RIGHT('loud')('soft')
'soft'
```

"Currying": Take multiple argument functions and turn them into single argument functions.


## Boolean Logic

```python
def TRUE(x):
    return lambda y: x

def FALSE(x):
    return lambda y: y
```

The behaviour of `TRUE` is that you're connected to the first thing, the behaviour of `FALSE` is that you're connected to the second thing.

```python
>>> TRUE('5v')('gnd')
'5v'

>>> FALSE('5v')('gnd')
'gnd'

>>> TRUE(TRUE)(FALSE)
<function TRUE at 0x102af1488>
```

```python
def NOT(x):
    return x(FALSE)(TRUE)

def AND(x):
    return lambda y: x(y)(x)

>>> AND(TRUE)(FALSE)
<function FALSE at 0x102af3048

def OR(x):
    return lambda y: x(x)(y)
 
```

## Numbers

Define a representation of natural numbers.
Again, numbers are representing a behaviour.

```python
ONE = lambda f: lambda x: f(x)
TWO = lambda f: lambda x: f(f(x))
THREE = lambda f: lambda x: f(f(f(x)))
```

How would you represent `ZERO`?

```python
ZERO = lambda f: lambda x : x
```

You will notice that the implementation of `ZERO` is exactly the same as `TRUE`.

## Math

* 0 is a number.
* If n is a number, then the successor of n is a number.
* 0 is not the successor of any number.
* succ(n1) == succ(n2) implies n1 == n2
* If S is a set that contains 0 and the successor of every number in S, then S is the set of all numbers.

#### Challenge

Implement successor. `SUCC(TWO) ---> THREE`

```python
SUCC = lambda n: (lambda f: lambda x: f(n(f)(x)))
```

#### arithmetic

```python
ADD = lambda x: lambda y: y(SUCC)(x)

MUL = lambda x: lambda y: lambda f:y(x(f))
```