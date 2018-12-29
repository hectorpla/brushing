# Tricky String problems

### 68. Text Justification

layers of implementation

```python
import math

def spaceFrontLoaded(spaces, holes):
    """
    maybe the correct interpretation of spacing evenly with extra spaces put in front

    >>> spaceFrontLoaded(3, 2)
    [2, 1]
    >>> spaceFrontLoaded(4, 2)
    [2, 2]
    >>> spaceFrontLoaded(4, 3)
    [2, 1, 1]
    >>> spaceFrontLoaded(7, 3)
    [3, 2, 2]
    >>> spaceFrontLoaded(8, 3)
    [3, 3, 2]
    """
    assert spaces >= holes
    spaceCounts = []
    for i in range(holes):
        width = math.ceil(spaces / (holes - i))
        spaceCounts.append(width)
        spaces -= width
    assert spaces == 0
    return spaceCounts


def fullJustify(words, maxWidth):
    """
    :type words: List[str]
    :type maxWidth: int
    :rtype: List[str]

    >>> fullJustify(["ask","not","what","your","country","can","do","for","you","ask","what","you","can","do","for","your","country"], 16)
    ['ask   not   what', 'your country can', 'do  for  you ask', 'what  you can do', 'for your country']
    """
    result = []
    # assume all words can fit in a line by themselves

    def spaceBetween(words):
        """
        maxWidth = 8
        spaceBetween("abcde") -> "abcde   "
        """
        holes = len(words) - 1
        spaces = maxWidth - sum(map(len, words))
        if len(words) == 1:  # bug: corner
            return words[0] + ' ' * spaces
        # delegate to a helper
        spaceCounts = spaceFrontLoaded(spaces, holes)
        # intervene the words and spaces
        segments = [words[0]]
        for i in range(holes):
            segments.append(' ' * spaceCounts[i])
            segments.append(words[i+1])
        result = ''.join(segments)
        assert len(result) == maxWidth
        return result

    n = len(words)
    i = 0  # pointer to the word that starts a line
    while i < n:
        j = i  # the index of the ending word
        width = 0  # acc words length
        while j < n and width + len(words[j]) + j - i <= maxWidth:
            width += len(words[j])
            j += 1
        if j == n:
            # justify accordingly
            line = ' '.join(words[i:j])
            line += ' ' * (maxWidth - len(line))
        else:
            line = spaceBetween(words[i:j])
        result.append(line)
        i = j  # point to a new word
    return result

# tests
# hard to come up
# ["abcde", "a", "b"], 8

```

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



