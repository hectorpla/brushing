# LinkedList problems

## Recipes

* advance pointer
* set next pointer null for the tail

For transformation

1. snapshot, 2. transform, 3. advance \(loop invariant\)

## Meeting

160. Intersection of Two Linked Lists  
take advantage of the fact \(lenA + lenB\) = \(lenB + lenA\)

## Transformation

Three Phases: 1. snapshot, 2. transform, 3. advance \(loop invariant\)

Think recursively: 

* maintain the invariant for each recursive call \(loop in iterative method\)
* count and identify bases; handle them separately

### 206 reverse

```python
class Solution: def reverseList(self, head): 
    """ :type head: ListNode :rtype: ListNode """    
    # [] -> []
    # [1] -> [1]
    # [1,2] -> [2,1] 

    prev = None

    while head:
        # make a snapshot
        nxt = head.next
        # tranform
        head.next = prev
        # make the assumption consistent
        prev, head = head, nxt
    return prev
```

### 156 Binary Tree Upside Down

```python
# a flip looks like
# 1            1
# | \    ->    |
# 2  3         2 - 3

def upsideDownBinaryTree(self, root): 
    """ :type root: TreeNode :rtype: TreeNode """
    # the tree is left loaded, otherwise, some nodes (on the right) will be lost
    # since the transformation is going down, we only need to traverse down, no recursion

    # [1] -> [1]
    # [1, 2] -> [2, 1]
    # [1,2,3] -> [2, 3, 1]

    if root is None: 
        return root
    old_root = root

    left, right = None, None
    while root:
        # make snapshot of the current state
        nxt = root.left
        templeft, tempright = root.right, root

        # flip
        root.left, root.right = left, right

        # advance
        left, right = templeft, tempright
        root = nxt
        
    return right
```

take-aways: when dealing with pointer manipulation, 4 steps

1. store all information that might be lost
2. do the work
3. ensure loop invariant by the assumption \(the assumption decides the initial state\)
4. check termination condition

### 24. Swap Nodes in Pairs

```python
def swapPairs(self, head):
    # [] -> []
    # [1] -> [1]
    # [1,2] -> [2,1]
    # [1,2,3] -> [2,1,3]
    # [1,2,3,4] -> [2,1,4,3]

    dummy = ListNode(-1)
    tail = dummy

    # base case, zero/one-item list
    while head and head.next:
        # snapshot
        nxt = head.next.next
        first, second = head, head.next
        
        # transform
        tail.next = second
        second.next = first
        first.next = None
        
        # advance
        tail = first
        head = nxt
    
    tail.next = head
    return dummy.next
```

### 25. Reverse Nodes in k-Group

```python
def reverseKGroup(self, head, k):
    # follow the implementation of problem 24

    if k == 0: return head

    # [1], 0 -> [1]
    # [], 2 -> []
    # [1], 2 -> [1]
    # [1,2], 1 -> [1,2]
    # [1,2], 2 -> [2,1] # bug: wrong handling for nextGroupHead
    # [1,2,3], 2 -> [2,1,3]
    # [1,2,3,4], 3 -> [3,2,1,4]

    def reverse(head):
        # assume head is not None
        prev = None
        while head:
            nxt = head.next
            head.next = prev
            prev, head = head, nxt
        return prev

    def nextGroupHead(head):
        # remember to set None
        prev = None
        for _ in range(k):
            if head:
                prev, head = head, head.next # bug, typo: head ~> prev
            else:
                raise Exception('not enough length')
        prev.next = None
        return head

    dummy = ListNode(-1)
    tail = dummy

    # k + 1 base cases: list length 0 ~ k 
    while True:
        # snapshot
        try:
            nxt = nextGroupHead(head)
        except:
            break
        
        # transform
        tail.next = reverse(head)
        
        # advance
        tail = head
        head = nxt

    tail.next = head
    return dummy.next
```

compare to the recursive approach:  [https://leetcode.com/problems/reverse-nodes-in-k-group/discuss/11423/Short-but-recursive-Java-code-with-comments](https://leetcode.com/problems/reverse-nodes-in-k-group/discuss/11423/Short-but-recursive-Java-code-with-comments)

