# Greedy vs. frugal

### 13. Roman to Integer

greedy on line 27, subtract later

on the hand, with frugality in mind, we look ahead and 

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
        # bug: didn't check pred in successor
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

