# 536. Construct Binary Tree from String

consider what are **leaves**, and take care of them; make assumption on normal case parsing

```python
def str2tree(self, s):
    # corner: "5"
    # corner: "()"
    # "5()(2)"
    
    i = 0  # scope variable
    n = len(s)
    
    def build():
        nonlocal i, n
        if i == n or s[i] == ")":
            return None
        start = i
        if s[start] == '-':
            i += 1
        assert s[i].isdigit()
        while i < n and s[i].isdigit():
            i += 1
        val = int(s[start:i])
        root = TreeNode(val)
        if i < n and s[i] == '(':
            i += 1
            root.left = build()
            assert s[i] == ')'
            i += 1
        if i < n and s[i] == '(': # duplicate logic
            i += 1
            root.right = build()
            assert s[i] == ')'
            i += 1
        return root
    return build()

# tests
# "5"
# "-5"
# "5(2)"
# "5()(2)"
# "5(1(2)(3))(9)"
```

