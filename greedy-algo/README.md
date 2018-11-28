# Greedy Algo

132. gas station



### 392. Is Subsequence

follow-up: preprocess t into a char2indices: {str: int\[\]}, except the initial time, average time: O\(M\) -&gt; O\(k logM\), where M is the size of t and k is the size of 



### 406. Queue Reconstruction by Height

make the process deterministic

thinking process:

* when the heights have no duplicates, sort by height so that when inserting the person, we know exactly the position
* when there are duplicates, we prioritize those with less count in front, so that in order, the insertion position is correct



### 316. Remove Duplicate Letters

O\(n^2\) thinking process

* pick a char every iteration
* the index of char should be followed by all remaining chars
* caveat: should take the leftmost char if there are duplicates

```python
# the first working solution
def removeDuplicateLetters(self, s):
    # contrived a O(n^2) method
    # idea: determine the head at a time, once determine, remove all the ocurrences of that head, repeat the process untill no chars left
    # how to determine? scan from right to left, find the lexicographally smallest char that is followed by all remaining chars
    n = len(s)
    remaining = set(s)
    result = []
    prev = -1 # the index of the previous candidate
    while remaining:
        candidate = prev + 1 # index of the candidate char, bug: the pointer might point to a previously added char
        size = len(remaining)
        visited = set()
        for i in range(n - 1, prev, -1):
            char = s[i]
            if char not in remaining:
                continue
            visited.add(char)
            if len(visited) == size and (char <= s[candidate] or s[candidate] not in remaining): # tricky, < instead of <=
                candidate = i
        result.append(s[candidate])
        remaining.remove(s[candidate])
        prev = candidate
    return ''.join(result)
```

my optimal sol: a different way to interpret, **visited set   vs.  rightmost indices**

```python
def removeDuplicateLetters(self, s):    
    rightmost = {char: i for i, char in enumerate(s)} # record rightmost index 
    
    remaining = set(s)
    stack = [] 
    
    # monotonic stack with complicated condition: line 19
    for i, char in enumerate(s):
        if char not in remaining:
            continue
        # if the one to pop is not the last one
        while stack and char < stack[-1] and i < rightmost[stack[-1]]: # !!! the core
            remaining.add(stack.pop()) # add char back to todo pool
        stack.append(char)
        remaining.remove(char) # char solved
    return ''.join(stack)
```



### 484. Find Permutation

```python
def findPermutation(self, s):
    # problem statement: lexi smallest implies some kind of greedy
    # somehow take advantage of heap
    # struggling with translatig into program
    # try-and-error!!!
    
    n = len(s) + 1
    hq = list(range(1, n + 1))
    
    heapify(hq)
    
    stack = [heappop(hq)] # increasing stack
    result = []
    # complexity: O(nlong) time, O(n) space
    for tok in s:
        if tok == 'D':
            stack.append(heappop(hq))
        else:
            while stack:
                result.append(stack.pop())
            stack.append(heappop(hq))
    result.extend(reversed(stack))
    return result
```

