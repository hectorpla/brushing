# Tricky String problems

### 68. Text Justification

### \*\*\*\*

### **616. Add bold in String**

Has a dict of words to select from, two dimensions to consider: M \(word dict\), N \(length of the string to  check\) 

turns out that N is small, can go that path

second trial: 

* used Trie, seems to be overkill, complexity: O\(n \* **m**\), n = len\(target string\), m = longest length of word in the dictionary
* instead, find each word in the dictionary, complexity: O\(**k** \* n\), k = len\(word dict\), this case





### 936. Stamping The Sequence

core: reverse the process

```python
def movesToStamp(self, stamp, target):
    target = list(target)
    n = len(target)
    k = len(stamp)
    
    def match(startIndex):
        """* in the target matches with any char"""
        nonlocal k
        for i in range(k):
            if target[startIndex + i] != '*' and target[startIndex + i] != stamp[i]:
                return False
        return True
    
    def mark(startIndex): # return boolean -> if the window is finished
        """ """
        nonlocal k
        count = 0
        for i in range(startIndex, startIndex + k):
            count += target[i] == '*'
            target[i] = '*'
        return count != k
        
    result = []
    todo = list(range(0, n - k + 1))
    
    prevSize = len(todo)
    # O((n - k)^2 * k)
    while len(todo):
        for start in todo:
            if match(start):
                if mark(start): # only if the window has unmatched
                    result.append(start)
                todo.remove(start)
        if prevSize == len(todo):
            break
        prevSize = len(todo)
    return list(reversed(result)) if len(todo) == 0 else []
```



