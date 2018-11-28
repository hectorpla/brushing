# Word Ladder

recipe: check goal\(**node**\) in current **level**

```python
mport string from collections
def ladderLength(beginWord, endWord, wordList):
    # problem statement: 
    # given a graph info, find the vertex-length of the shortest path from start to end

    word_dict = set(wordList)
    word_dict.add(beginWord)
    if endWord not in word_dict:
        return 0

    def getNeighbors(word):
        for i in range(len(word)):
            for char in string.ascii_lowercase:
                if char == word[i]: continue
                suc = word[:i] + char + word[i+1:]
                if suc in word_dict:
                    word_dict.remove(suc) # remove the 
                    yield suc

    q = deque([beginWord])
    length = 0
    # recipe: check whether the current node is the goal
    while q:
        length += 1
        for _ in range(len(q)):
            node = q.popleft()
            if node == endWord: # op
                return length
            q.extend(getNeighbors(node)) # delegate the iteration!
    return 0
```



recipe: check if the current node is in the other set  
Caveat: when delegating the child iteration to a generator, it implies the operation is done in **next** level

```python
def ladderLength(self, beginWord, endWord, wordList): 
    wordList.append(beginWord) 
    wordDict = set(wordList) 
    if endWord not in wordDict: return 0
    
    s1 = set()
    s2 = set()
    def getNeighbors(word, s): 
        for i in range(len(word)):
            for char in string.ascii_lowercase:
                if char == word[i]: continue
                suc = word[:i] + char + word[i+1:]
                if suc in wordDict:
                    if suc in s: 
                        continue
                    # s.add(suc) # bug: added children to the visited
                    yield suc

    # recipe: check if the current node is in the other set
    length = 0
    q1 = deque([beginWord])
    q2 = deque([endWord])
    q, other_q = q1, q2
    s, other_s = s1, s2
    while q:
        length += 1
        for _ in range(len(q)):
            node = q.popleft()
            if node in s: # check visited in the current level
                continue
            s.add(node) # should add to set in current level
            if node in other_s:
                return length - 1 
            q.extend(getNeighbors(node, s))
        q, other_q = other_q, q
        s, other_s = other_s, s
    return 0
```

