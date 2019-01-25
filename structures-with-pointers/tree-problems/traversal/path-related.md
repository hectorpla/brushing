# Path related \(Backtrack\)

### 437. Path Sum III

prefix-sum

```python
def pathSum(self, root, sum):
    # from perspective of each node, top-down approach, maintain the a count table
    result = 0
    sumCount = {0:1} # {int： int}, might the 0-count-prefix
    
    # back-track
    def traverse(root, acc):
        nonlocal result
        if root is None:
            return
        
        acc += root.val
        # like two-sum or prefix-sum
        result += sumCount.get(acc - sum, 0)
        
        sumCount[acc] = sumCount.get(acc, 0) + 1
        traverse(root.left, acc)
        traverse(root.right, acc)
        sumCount[acc] -= 1
    traverse(root, 0)
    return result
```

