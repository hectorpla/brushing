# Greedy vs. frugal

## Parsing

### 13. Roman to Integer

{% hint style="info" %}
greedy on line 27, subtract later
{% endhint %}

on the hand, with frugality in mind, we look ahead and add to result only when necessary, the final case is tricky

```python
def romanToInt(self, s):
    """
    :type s: str
    :rtype: int
    """
    if len(s) < 1:
        return 0    
    # look-head or delay evaluation
    value = {'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000}
    successor = {'I': ['V','X'], 'X': ['L', 'C'], 'C': ['D','M']}
    def transformDouble(pred, suc):
        if pred in successor and suc in successor[pred]:
            return value[suc] - value[pred]
        return 0    
    def transformSingle(token):
        return value[token]
    
    it = iter(s)
    result = transformSingle(next(it))
    for prev, tok in zip(s, it):
        match = transformDouble(prev, tok)
        result += match - transformSingle(prev) if match else transformSingle(tok)
    return result

# tests
# "II"
# "XXXI"
# "IV"
# "CXL"
# "CXLII"
# "XCIX"
```

### 227. Basic Calculator II

{% hint style="info" %}
Greedy:

* cache the intermediate result, **refresh** when '+' or '-' met; **update** when '\*' or '/'
* always add temp result to top level, subtract when '\*' or '/' encountered

In high level, process on the basis of every pair of **\(operation, rhs\)**, except the first number \(initially paired with '+'\), huge simplification when coding
{% endhint %}

```python
def calculate(self, s):
    # always not more than 2 layers
    n = len(s)
    i = 0
    def parseInt():
        nonlocal i, n
        start = i
        while i < n and s[i].isdigit():
            i += 1
        return int(s[start:i])
    
    result = 0
    temp = 0 # for intermediate result, cache
    op = '+'
    
    # greedy approach: subtract later
    while i < n:
        char = s[i]
        if char in ['+','-','*','/']:
            op = char
        elif char.isdigit():
            rhs = parseInt()
            if op in ['+', '-']:
                temp = rhs if op == '+' else -rhs
            else: # '*', '/'
                result -= temp
                # py convention: -3//2 = -2, always floor, use int(3/2) to cut closer to zero
                temp = temp * rhs if op == '*' else int(temp / rhs)
            result += temp
            continue # don't advance
        i += 1
    return result

# tests
# "1 + 2"
# "1 - 2"
# "1 + 2 + 3 - 5"
# "3 * 4"
# "6 / 3"
# "2*6/4*5"
# "3 / 2 + 12"
# "3 - 2 * 12", test '-'
# "5 + 2 * 3 / 4 - 3 * 5"
```

