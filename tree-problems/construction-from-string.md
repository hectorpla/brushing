# Construction from string or sequences

## conclusion

think about these problems: do post-/pre- order traversal and in-order traversal uniquely determine a tree? No, think about duplicates

## problems

### 536. Construct Binary Tree from String

did it recursively; **iterative** solution also simple, try again

```python
def str2tree(self, s):
    cur = 0 # current index used by the dfs
    n = len(s)
    
    # tests
    # base: "3"
    # "1(2)"
    # "1(2)(3)"
    # "1(2(6)(7))(3()(8))", 3()(8) should be [3,null,8] instead of [3,0,8], wrong test case
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

recursive complexity

T\(n\) = T\(n-1\) + **n** =&gt; O\(n^2\), we need to improve the **bold** term

hashmap helps!

### 889. Construct Binary Tree from Preorder and Postorder Traversal

top sol: iterative, brilliant   


