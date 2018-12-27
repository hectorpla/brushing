# Parsing negative number

the check for number is originally `tok.isdigit()` , big wrong

```python
def evalRPN(tokens):
    # division should be cut towrads zero
    
    stack = []  # to store intermediate result
    for tok in tokens:  # bug: corner case negative number
        if tok[0].isdigit() or tok[0] == '-' and len(tok) > 1:
            stack.append(int(tok))
        else:
            assert len(stack) >= 2
            rhs, lhs = stack.pop(), stack.pop()
            if tok == '+':
                result = lhs + rhs
            elif tok == '-':
                result = lhs - rhs
            elif tok == '*':
                result = lhs * rhs
            else:
                result = int(lhs / rhs)
            stack.append(result)
    assert len(stack) == 1
    return stack[0]
```

