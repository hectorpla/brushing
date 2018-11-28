# Implicit Tree Traversal

## Conclusion

This kind of problem can mostly identified by **input type of array or other serialized input**

**naive natural stack corresponds to pre-order traversal**

## **Problems**

### 255. Verify Preorder Sequence in Binary Search Tree

convert the pre-order sequence into **in-order** traversal, **semantics** of stack operations:

1. push: store a node for later use
2. pop: visit node \(as in in-order traversal\)

! after analysis, **monotonic structure** makes we visit node in value-ascending order

```python
def verifyPreorder(self, preorder):
    # idea: DFS (in-order) traversal of a tree, 
    # maintain a decreasing stack (ad-hoc, inspiration at writing time, bad)
    # after analysis, monotonic structure makes
    
    stack = []
    prev = float('-inf')
    for val in preorder:
        if val < prev:
            return False
        while stack and stack[-1] < val:
            prev = stack.pop()
        stack.append(val)
    return True
    # follow-up: use O(1) space
```

follow-up: **take advantage of the given input**

### 331. Verify Preorder Serialization of a Binary Tree

straight translation from pre-order to stack simulation

bonus: can be solved by counting **degree**!

```python
def isValidSerialization(self, preorder):
    # idea: stack simulation -> push 2 when a number met, else reduce recursively
    
    # bad: took long time to think about the details
    stack = [0] # padding, a trick
    for token in preorder.split(','):
        if token == '#':
            stack.append(0)
            while stack and stack[-1] == 0:
                stack.pop()
                stack[-1] -= 1
        else:
            stack.append(2)
    return len(stack) == 1 and stack[0] == -1 # only decrease 1
# tests
# [1,#,#] -> T
# [#] -> T
# [1,#] -> F, an imcomplete tree
# [1,#,#,1,#,#] -> F, a forest
# [1,#,#,#] -> F
```

### 388. Longest Absolute File Path

mind the order of: 1. retract to the right level 2. count length or append pwd

```python
def lengthLongestPath(self, input):
    # iterative: construct path info while maintaining the longest path
    def countDepth(line):
        count = 0
        for char in line:
            if char == '\t':
                count += 1
            else:
                break
        return count
    
    result = 0
    stack = [0] # to keep track of the current depth
    for line in input.split('\n'):
        depth = countDepth(line)
        while len(stack) > depth + 1: # retract to the parent
            stack.pop()

        if '.' in line: # in case of file
            result = max(result, stack[-1] + len(line))
        else:
            stack.append(stack[-1] + len(line) - depth)
        
    return result

# tests
# "dir\n\tsubdir1\n\tsubdir2" -> no file
# "dir\n\tsubdir1\n\tsubdir2\n\t\tfile.ext" 
# "dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext"
```



### 71. Simplify Path

exhaustively consider the components between '/'

```python
def simplifyPath(self, path):
    stack = []
    
    for comp in path.split('/'):
        # cases where we should remain in the current level
        if len(comp) == 0 or comp == '.' or not stack and comp == '..':
            continue
        if comp == '..':
            stack.pop()
        else:
            stack.append(comp)
    return '/' + '/'.join(stack)

# tests
# '///a' -> '/a'
# '//..' -> '/'
# '/a/..' -> '/'
# '/a/.' -> '/a'
# '/a/kd/.//../ba/./a'
```



