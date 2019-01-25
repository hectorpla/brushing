# Two-root

### manipulation on two trees: def\(root1, root2\)

* 617 merge tree check **both** exists first

```python
def mergeTrees(t1, t2):
    """
    :type t1: TreeNode
    :type t2: TreeNode
    :rtype: TreeNode
    """
    
    # sub-problems: merge two trees
    
    # tests
    # [], [1] -> [1]
    # [1], [2] -> [3]
    # [1], [2,5] -> [3,5]
    
    # 	Tree 1                     Tree 2                  Merged Tree 
  #     1                         2                             3
  #    / \                       / \                           / \
  #   3   2                     1   3                         4   5
  #  /                           \   \                      /  \   \
  # 5                             4   7                    5    4   7
    def merge(t1, t2):
        if t1 and t2:
            t1.val += t2.val
            t1.left = merge(t1.left, t2.left)
            t1.right = merge(t1.right, t2.right)
            return t1
        return t1 if t1 else t2
    return merge(t1, t2)
```

* 100 same tree check both present and both absent



* 101 symmetric tree, pretty much the same as 100, iterative solution \(same logic as recursive\) same as 100

