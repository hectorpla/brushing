# Structural error

### 32. Longest Valid Parentheses

want to fix "\(\)\(\)", concat, top level; but "\(\(\)\(\)" is not fixed

```python
def longestValidParentheses(self, s):
    # this solution fails at "(()()"
    result = 0
    lastIndex, lastCount = 0, -1, 0
    stack = [] # keep track of the open paren index
    for i, char in enumerate(s):
        if char == '(':
            stack.append(i)
        elif stack:
            leftIndex = stack.pop()
            count = i - leftIndex + 1
            result = max(result, count)
            if not stack: # top level accummulation
                if lastIndex + 1 == leftIndex:
                    result = max(result, count + lastCount)
                lastIndex, lastCount = i, lastCount + count # count should be accumulated instead of reassigned 
    return result
# tests
# "("
# ")"
# "()"
# "(())"
# "()()", bug discovered, top level
# "()(())())", bug still, top level
# "(()()" -> 4, bug, didn't generalize the top level bug fix
# "(()())"
# "((())())"
# "(()))()" -> 4, unmatched
```



A working but not good solution

```python
def longestValidParentheses(self, s):
    result = 0
    lastIndex, lastCount = -1, 0
    stack = [] # keep track of the open paren index
    for i, char in enumerate(s):
        if char == '(':
            stack.append(i)
        elif stack:
            leftIndex = stack.pop()
            count = i - leftIndex + 1
            result = max(result, count)
            if not stack: # top level accummulation
                if lastIndex + 1 == leftIndex:
                    # print('top level concat', leftIndex, 'counts:', count, lastCount)
                    result = max(result, count + lastCount)
                    lastCount += count # bug: count should be accumulated
                else:
                    lastCount = count
                lastIndex = i
            else: # for non-top-level concat 
                result = max(result, i - stack[-1])
    return result
```

a much more easy-to-understand solution

```python
def longestValidParentheses(self, s):
        ranges = [] # take extra space though
        stack = []
        for i, char in enumerate(s):
            if char == '(':
                stack.append(i)
            elif stack:
                # bug: didn't pop the nested strings
                left = stack.pop()
                while ranges and ranges[-1][0] > left:
                    ranges.pop()
                ranges.append((left, i))
        mx = 0
        left, prev = 0, -1 # bug: initial value of prev, case first range starts at 1
        for start, end in ranges:
            if prev + 1 != start:
                left = start
            mx = max(mx, end - left + 1)
            prev = end
        return mx
```

### 225. Implement Stack using Queues

set a isReady variable \(see submission\), over simplify the problem, only solve the problem in shallow level

