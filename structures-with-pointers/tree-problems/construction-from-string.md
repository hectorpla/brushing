---
description: 'Nov, Jan'
---

# Construction from string or sequences

## Conclusion

think about these problems: do post-/pre- order traversal and in-order traversal uniquely determine a tree? 

think about duplicates: yes, if the consistency is maintained the traversal is generated and the protocol is given

## Problems

### 536. Construct Binary Tree from String

did it recursively; **iterative** solution also simple, try again

```python
def str2tree(self, s):
    cur = 0 # current index used by the dfs
    n = len(s)
    def construct():
        nonlocal cur
        # token: number
        sym = 1
        if s[cur] == '-':
            sym = -1
            cur += 1
        val = 0
        while cur < n and s[cur].isdigit(): 
            val = val * 10 + int(s[cur])
            cur += 1
        root = TreeNode(val * sym)
        
        # token: expect (
        if cur < n and s[cur] == '(':
            cur += 1
            root.left = construct()
            assert s[cur] == ')'
            cur += 1

        # token: expect (    
        if cur < n and s[cur] == '(':
            cur += 1
            root.right = construct()
            assert s[cur] == ')'
            cur += 1
        return root
        
    return construct()
# tests
    # base: "3"
    # "1(2)"
    # "1(2)(3)"
    # "1(2(6)(7))(3()(8))", 
    #   3()(8) should be [3,null,8] instead of [3,0,8], wrong test case
```



### **Construct Binary Tree from Preorder and Inorder Traversal**

```python
def buildTree(self, preorder, inorder):
    # assume no duplicates
    def buildHelper(pstart, pend, istart, iend):
        if pstart > pend: # mind the compar operator
            return None
        val = preorder[pstart]
        root = TreeNode(val)
        for i in range(istart, iend + 1):
            leftLength = i - istart
            if inorder[i] == val:
                root.left = buildHelper(pstart + 1, pstart + leftLength, istart, i - 1)
                root.right = buildHelper(pstart + leftLength + 1, pend, i + 1, iend)
                return root
        raise 'should not reach here'
    # worst: O(n^2)
    n = len(preorder)
return buildHelper(0, n-1, 0, n-1)
```

there is a much faster solution: 

1. Like DFS, post order
2. take advantage of the given that numbers are unique

Optimization:  
recursive complexity: T\(n\) = T\(n-1\) + **n** =&gt; O\(n^2\), we need to improve the **bold** term, **hash map** helps!

```python
# O(n) solution: much tricky, with two pointers
def buildTree(self, preorder, inorder):
    assert len(preorder) == len(inorder)
    n = len(preorder)
    
    i, j = 0, 0  # points at pre-order and post-order
    def build(parentVal):
        nonlocal i, j
        if j == n:  # for right child of the last elem in pre-order
            assert i == n
            return None
        if i == n or inorder[j] == parentVal:  # for finishing left sub-tree parsing
            return None
        rootVal = preorder[i]
        root = TreeNode(rootVal)
        i += 1 # consume an index at pre-
        
        # in-order traversal
        root.left = build(rootVal)
        assert inorder[j] == rootVal  # roots converge
        j += 1  # consume index at in-  # parity of advance
        # print(i, j, 'root', rootVal, parentVal)
        # assert i == j or parentVal == inorder[j] # finished parsing the left part, prevent bug!!!!!, wrong assumption
        root.right = build(parentVal)  # should pass down the ancestor val down
        return root
    
    return build(None)
# tests
# [], []
# [1], [1]
# [1,2], [1,2]
# [1,2], [2,1]
# [1,2,3], [1,3,2]
# [1,2,3], [2,3,1]  # didn't cover
```

### 889. Construct Binary Tree from Preorder and Postorder Traversal

top sol: iterative, brilliant   


### Reconstruct BST from pre-/post- order traversal \(probably not on LC\)

{% hint style="info" %}
from the properties of BST, we can implicitly know the in-order traversal in the pre-/post- order. We can reduce the problem to pre + in
{% endhint %}

> caveat: how about duplicates? as long as the convention is preserved \(either all duplicate keys go to the left sub-tree or go to the right sub-tree\), we can still precisely recover the tree from a pre-/post- order



