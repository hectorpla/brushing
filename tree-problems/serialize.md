---
description: 'Nov, Dec'
---

# Serialize & Deserialize

### 297 Serialize and Deserialize Binary Tree

Take away: 1. good use of dummy node

```python
from collections import deque
class Codec:
    # traverse by level
    # tests
    # []
    # [1], for bug 4
    # [1,2]
    # [1,null,3]
    def serialize(self, root):
        result = []
        q = deque()
        if root:
            q.append(root)
        while q:
            node = q.popleft()
            result.append(str(node.val) if node else '#') # bug1: didn't convert to str
            if node:
                q.append(node.left)
                q.append(node.right)
        return ','.join(result)
        
    def deserialize(self, data):
        if len(data) == 0: return None
        dummy = TreeNode(-1)
        current = dummy
        
        q = deque()
        pointingPosition = 1 # 0: left, 1: right
        for token in data.split(','): # ''.split(',') -> ['']
            child = None if token == '#' else TreeNode(int(token)) # bug3: didn't convert to TreeNode
            if child: # bug4: should do this before pop
                q.append(child) 
            if pointingPosition == 0:
                current.left = child
            elif pointingPosition == 1:
                current.right = child
                if q: # bug2: pop from empty, not good code
                    current = q.popleft()
            
            pointingPosition ^= 1
        return dummy.right
```

concise solution: **preorder**  
[https://leetcode.com/problems/serialize-and-deserialize-binary-tree/discuss/74253/Easy-to-understand-Java-Solution](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/discuss/74253/Easy-to-understand-Java-Solution)

### 428. Serialize and Deserialize N-ary Tree

```python
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val, children):
        self.val = val
        self.children = children
"""
from collections import deque
class Codec:
    # idea: serialize by level traversal, each node is represented as (val, numChildren)
    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: Node
        :rtype: str
        """
        result = []
        q = deque([root])
        while q:
            node = q.popleft()
            if not node:
                result.append('#|0') # don't need to check null
                continue
            result.append(str(node.val) + '|' + str(len(node.children)))
            q.extend(node.children)
        return ','.join(result)

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: Node
        """
        dummy = Node(-1, [None])
        cur = None
        link = 0
        q = deque([dummy])
        
        for val, numChild in map(lambda x: x.split('|'), data.split(',')):
            newNode = Node(int(val), [None] * int(numChild)) if val != '#' else None
            if newNode and newNode.children: # !bug: didn't check the number of children
                q.append(newNode)
            if link == 0:
                cur = q.popleft()
            cur.children[link] = newNode
            link = (link + 1) % len(cur.children)
        return dummy.children[0]
```



### 449 Serialize and Deserialize BST

in-order traversal of a BST is deterministic. Encode the tree into a pre-order sequence and then combine. **Reduce** to problem \(105 Construct Binary Tree from Preorder and Inorder Traversal\)

spent a lot of time in finding a O\(n\) method to decode, failed

```python
class Codec:    
    # corner: duplicates
    # [2,2,2] -> "2,2,2" -> [2,2,2] or [2,2,null,2] or [2,null,2,null,2]
    # the solution seems not to cover those cases
    def serialize(self, root):
        # pre-order
        result = []
        def traverse(root):
            if not root:
                return
            result.append(root.val)
            traverse(root.left)
            traverse(root.right)
        traverse(root)
        return ','.join(map(str, result))

    def deserialize(self, data):
        # corner
        if not data: return None # "".split(',') yields [""]
        # build from the pre-order
        data = list(reversed(data.split(','))) # always pop from tail
        def build(lowerBound, upperBound):
            if not data:
                return None
            top = int(data[-1])
            if not (lowerBound < top < upperBound):
                return None
            data.pop()
            root = TreeNode(top)
            root.left = build(lowerBound, top)
            root.right = build(top, upperBound)
            return root
        return build(float('-inf'), float('inf'))
        
        # tests
        # [1]
        # [2,1]
        # [1,null,2]
        # [2,1,3]
```

### 431. Encode N-ary Tree to Binary Tree

structurally make things simple:

convention: n-ary -&gt; binary: only the **left** child has valid information

```java
class Codec {
    private static int LABEL_VALUE = Integer.MIN_VALUE;
    // Encodes an n-ary tree to a binary tree.
    public TreeNode encode(Node root) {
        if (root == null) { return null; }
        TreeNode cur = new TreeNode(root.val);
        TreeNode res = cur;
        // the meat part is always only the left child, structually achieve the goal
        for (Node ch : root.children) {
            cur.left = encode(ch);
            cur.right = new TreeNode(LABEL_VALUE);
            cur = cur.right;
        }
        return res;
    }

    // Decodes your binary tree to an n-ary tree.
    public Node decode(TreeNode root) {
        if (root == null) { return null; }
        List<Node> decodedChildren = new ArrayList<>();
        Node res = new Node(root.val, decodedChildren);
        TreeNode cur = root;
        while (cur.left != null) {
            decodedChildren.add(decode(cur.left));
            cur = cur.right;
        }
        return res;
    }
    
}
```

