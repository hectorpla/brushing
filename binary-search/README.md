# Binary Search

### Pow\(x, n\)

iterative vs. recursive

iterative: no tricky; maintain an array as the reversed binary sequence of n  
recursive: actually head recursion, consider the least significant digit at a level

```python
def myPow(self, x, n):
    # 2, -2 -> 0.25
    # 2, 3 -> 8
    # -2, -5 -> -0.03125

    result = 1
    acc = x if n > 0 else 1 / x
    n = abs(n)

    binaries = []
    while n:
        if n & 1:
            binaries.append(1)
        else:
            binaries.append(0)
        n >>= 1
    for b in binaries:
        if b:
            result *= acc
        acc *= acc
    return result
    
# (head recursion), not mine
def myPow(x, n):
    if n < 0:
        return myPow(1 / x, -n)
    if n == 0:
        return 1
    rest = myPow(x, n >> 1)
    return rest * rest * (x if n & 1 else 1)
```



### 29. Divide Two Integers

```python
def divide(self, dividend, divisor):
    """
    :type dividend: int
    :type divisor: int
    :rtype: int
    """
    if dividend == -2**31 and divisor == -1: # special case for overflow
        return 2**31 - 1
    if dividend < 0 or divisor < 0:
        sign = (dividend > 0) ^ (divisor > 0)
        raw = self.divide(abs(dividend), abs(divisor))
        return -raw if sign else raw
    if dividend < divisor:
        return 0
    # either left shift the dividend or right shift divisor
    rest = self.divide(dividend, divisor << 1) 

    return (rest << 1) + (dividend - divisor*(rest << 1) >= divisor)
```

