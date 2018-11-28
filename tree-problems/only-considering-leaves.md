# Only considering leaves

### 872. Leaf-Similar Trees

```python
def leafSimilar(self, root1, root2):
    def getLeaves(root):
        if root is None:
            return
        if root.left:
            yield from getLeaves(root.left)
        if root.right:
            yield from getLeaves(root.right)
        if not root.left and not root.right:
            yield root.val
    for val1, val2 in zip_longest(getLeaves(root1), getLeaves(root2)):
        if val1 != val2:
            return False
    return True
```

see top sol: implementation without generator \(maintain a stack\)

### 112. Path Sum

```python
def hasPathSum(self, root, sum):
    def hasPathSum(root, sum):
        if root is None:
            return False # !!! false positive if check here
        if root.left is None and root.right is None:
            return root.val == sum
        return hasPathSum(root.left, sum - root.val) or hasPathSum(root.right, sum - root.val)
    return root is not None and hasPathSum(root, sum)
        
# tests
# [], 0
# [0], 0
# [1,2], 1 bug, false positive for case where an intermidate node has only one child
```

