---
description: 'Jul, Nov'
---

# Binary Search

### 222. Count complete tree nodes

Take-aways:

1. reuse function \(getDepth\(root\)\)
2. adjust the program to base/small cases \(tree having 1 node\)

### 270. Closest Binary Search Tree Value \(mind test cases\)

```python
def closestValue(self, root, target):
    # test for base cases
    # [1], 3 -> 1 # the answer is always in the tree 
    # [4,2,5], 3.7 -> 4 # answer stays with the root
    # [4,2,5], 2.3-> 2 # answer changes to the left
    # [4,2,5], 4.6 -> 5 # changes to the right

    result = float('inf')
    while root:
        if abs(root.val - target) < abs(result - target):
            result = root.val
        if target < root.val:
            root = root.left
        else:
            root = root.right
    return result
```

### 285. Inorder Successor in BST

three cases: below p, above p, no  
solve it iteratively and recursively  
follow up: what if parent pointers are given \(salesforce\):  ancestor that after the first right turn under the _above_ case 

### 

### 776. Split BST

didn't figure out the recursive way, iterative is too tedious

