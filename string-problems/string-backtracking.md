# String backtracking

### **842. Split into Fibonacci Sequence**

one take-away: when mixing Type number and None, should use "x is None" to check because "not None" and "not 0" both return True



### 678. Valid Parenthesis String

DFS + memo, by pruning, O\(n^2\) time

core: at '\*', we have up to three branches

```python
def checkValidString(self, s):
    n = len(s)
    memo = [{} for _ in range(n)] # [{int: bool}]
    
    # state: (opened, start)
    def search(opened, start): # return bool -> whether the starting at `start` with openned give us a valid string
        if start >= len(s):
            return opened == 0
        if opened in memo[start]:
            return memo[start][opened]
        if s[start] == '*':
            # up to three branches
            isValid = search(opened + 1, start + 1) or search(opened, start + 1)\
                or (opened > 0 and search(opened - 1, start + 1))
        elif s[start] == '(':
            isValid = search(opened + 1, start + 1)
        else:
            isValid = opened > 0 and search(opened - 1, start + 1) # mind type: used opened would cause to return int
        memo[start][opened] = isValid
        return isValid
    
    return search(0, 0)
# tests
# ")"
# "("
# "()"
# "(*)"
# "((*)"
# "(*))"
# ")**"
# "(**"
```

connection to the greedy solution:  
  instead of branching out at '\*', keep track of the possible the possible values of \# opened 

