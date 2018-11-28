# 1-based to 0-based

### **Nth Digit**

**9 + 90 \* 2 + 900 \* 3 + ...**  


```python
def findNthDigit(n):
    # caveat: n is 1-based
    
    # 3 -> 3
    # 11 -> 0
    # 39 -> 4(24)
    # 200 -> 
    
    digit = 1
    acc = 9
    while True:
        if n > acc * digit:
            n -= acc * digit
            digit += 1
            acc *= 10
        else:
            break
    number = 10 ** (digit - 1) + (n - 1) // digit # number is 0-based
    shift = digit - 1 - (n - 1) % digit # digit is 0-based: n - 1
    return (number // (10 ** shift)) % 10
```

