---
description: FB
---

# Prefix suffix filter

mind the termination condition to insert

 analyze time/space complexity

```python
class WordFilter: 
    class TrieNode:
        def init(self): 
            self.children = {} # {str: TrieNode}
            self.word = None # int
            
    def __init__(self, words):
        """
        :type words: List[str]
        """
        self.root = self.TrieNode()
        self.words = words
        for i in range(len(words)):
            self._insert(i)
    
    def _insert(self, word_index):
        word = self.words[word_index]
    
        # improvement constrain of depth
        def insert(cur, start, end, prePostBoth): # prePostBoth = 0 | 1 | 2 for pre | post | both
            # print(word, start, end, "type", prePostBoth)
            cur.word = word_index
            if start > len(word) - 1 or end < 0: # bug: originally start > end
                return
            if prePostBoth in [0, 2]: # prefix
                key = word[start] + ' '
                cur.children.setdefault(key, self.TrieNode())
                insert(cur.children[key], start + 1, end, 0)
            if prePostBoth in [1, 2]: # postfix
                key = ' ' + word[end]
                cur.children.setdefault(key, self.TrieNode())
                insert(cur.children[key], start, end - 1, 1)
            if prePostBoth == 2: # both
                key = word[start] + word[end]
                cur.children.setdefault(key, self.TrieNode())
                insert(cur.children[key], start + 1, end - 1, 2)
    
        insert(self.root, 0, len(word) - 1, 2)
    
    # tests
    # ["WordFilter","f","f","f","f","f"]
    # [[["apple", "ake"]], ["a","e"],["ap","e"],["a","le"],["ak", ""],["b",""]]
    def f(self, prefix, suffix):
        """
        :type prefix: str
        :type suffix: str
        :rtype: int
        """
        def fixes():
            pit = iter(prefix)
            sit = iter(reversed(suffix)) # bug: didn't reverse
            for i in range(max(len(prefix), len(suffix))):
                yield next(pit, ' ') + next(sit, ' ')
    
        cur = self.root
        for key in fixes():
            if key not in cur.children:
                return -1
            cur = cur.children[key]
        return cur.word
```

