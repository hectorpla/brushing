# Divide two numbers

some fact about two's complement and how shiftings are done in computer:

* `-INT_MIN = INT_MIN` : negate all bits and then +1, overflow
* `-1 >> 1 = -1`  : arithmetic right shift, sign bit are used to replace the most significant bit
* &gt;&gt; operator always truncate to -inf \(test cases then you know\)

{% hint style="info" %}
1. use negative numbers because less possible to overflow
2. probe the divisor: guard overflow, line 23
3. find quotient: find set bit for position \[0, k\], k is the highest shift, don't forget position 0
{% endhint %}

```python
def divide(self, dividend, divisor):
    # functionality: truncated towards zero
    # some edges: 1. divide by zero 2.over-/under- flow
    
    # idea: the quotient can be reprensted in binary number, use << to subsitute *
    # before doing that, convert both the dividend and divisor to non-negative
    
    INT_MIN, INT_MAX = -2 ** 31, 2 ** 31 - 1
    # corners: INT_MIN will overflow when negated
    isNegative = (dividend > 0) ^ (divisor > 0)
    dividend = -dividend if dividend > 0 else dividend
    divisor = -divisor if divisor > 0 else divisor
    assert dividend <= 0 and divisor <= 0
    
    # 1. left shift the divisor to the largest possible 2^m
    # (dividend, divisor)   probe       # shifts
    # (-10, -3)              -6         1
    # (-16, -5)              -10        1
    # (-2147483648, -1)     -2147483648 31
    # (-2147483648, -2)     -2147483648 30
    HALF_INT_MIN = INT_MIN >> 1
    shift = 0
    while divisor >= HALF_INT_MIN and dividend <= (divisor << 1):
        divisor <<= 1
        shift += 1
    
    # 2. reverse the process, check every shift, 
    # find all set bits in range [0, shift]
    quotient = 0
    while shift >= 0:  # mind shift == 0
        if dividend <= divisor:
            # caveat: -2147483648 - (-2147483648) = 0
            # supposed process: 1. negate -2147483648 -> -2147483648
            #                   2. -2147483648 + (-2147483648) = -2147483648
            dividend -= divisor
            quotient += (-1 << shift)
        divisor >>= 1
        shift -= 1
    
    # 3. return combination of quotient and the sign
    if isNegative:
        return quotient
    return -quotient if quotient > INT_MIN else INT_MAX

# tests
# -- extreme
# (-2147483648, -1)
# (-2147483648, 1)
# (-2147483648, 2)
# (2147483647, 1)
# (2147483647, 2)
# (-2147483648, -2)
# -- normal
# (10, 3)
# (-10, 3)
# (10, -3)
# (3, 3)
# (-3, -3)
# (1024, 8)
# (1024, -8)
# (1024, 9)
```

