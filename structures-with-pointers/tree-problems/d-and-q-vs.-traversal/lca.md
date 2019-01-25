# D & Q

### LCA

Special cases: cases where one node is the ancestor of the other node

```python
def lowestCommonAncestor(self, root, p, q):
        # [1,2], 1,2 -> 1
        # [1,2,3], 2,3 -> 1
        # [1,null,2,3,4], 3,4 -> 2
        if root is None or root is p or root is q:
            return root
        left, right = self.lowestCommonAncestor(root.left, p, q), self.lowestCommonAncestor(root.right, p, q)
        if left and right:
            return root
        return left if left else right
```

work top-down: 

```text
     1      find 5, 6: 1 delegates to 3
    / \
   2   3
      / \
     5   6
```

work bottom-up: 

```text
     1      find 2, 3: 2 and 3 floating up to 1
    / \
   2   5
        \
         3
```

