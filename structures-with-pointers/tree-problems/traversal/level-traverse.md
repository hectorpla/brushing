# Level traverse

### 199. Binary Tree Right Side View

recipe, examine every **node** in the **current** level

take-away: run tests with _**call stack**_

```python
def rightSideView(root):
    result = []
    
    # [] -> []
    # [1] -> [1]
    # [1,2] -> [1,2]
    # [1,2,3] -> [1,3]
    # [1,2,null,6] -> [1,2,6]
    # [1,2,3,6] -> [1,3,6]
    
    def traverse(root, depth):
        """dfs pre-order traverse"""
        # call stack
        # root  depth   result
        # 1     0       [1]
        # 2     1       [1,2]
        # 6     2       [1,2,6]
        # 3     1       [1,3,6]
        if root is None:
            return
        if len(result) < depth + 1:
            result.append(root.val)
        else: # verbose
            result[depth] = root.val # bug run example: result[-1]
        traverse(root.left, depth + 1)
        traverse(root.right, depth + 1)
    traverse(root, 0)
    return result
```

### 103. Binary Tree Zigzag Level Order Traversal

two phases: 1. extract by level, 2. reverse every other level

