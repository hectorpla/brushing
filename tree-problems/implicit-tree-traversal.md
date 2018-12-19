# Implicit tree traversal

### 386 Lexicographical Numbers

see top sol, very neat, tree traversal by arithmetic computation



### 856. Score of Parentheses

{% hint style="info" %}
`(`: go down one level

`)`: go up on level

only add to result when terminal nodes met
{% endhint %}

very tricky base case



```python
def scoreOfParentheses(self, S):
        # very tricky base case
        result = 0
        
        weight = 1
        # like DFS traverse
        for i, char in enumerate(S):
            if char == '(':
                weight <<= 1
            else:
                if S[i-1] == '(': # base case: leaf, only add to the result in this situation
                    result += weight >> 1
                weight >>= 1 # bug: should apply to leaves or intermediate nodes
        return result
    
    # tests
    # "()"
    # "()()"
    # "(())"
    # "(())()"
    # "((())())"
```

