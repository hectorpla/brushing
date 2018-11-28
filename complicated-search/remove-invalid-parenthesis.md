# Remove invalid parenthesis

good practice to analyze 

two facts considering which kind of parens is redundant

```python
def removeInvalidParentheses(s):
    """
    :type s: str
    :rtype: List[str]
    """
    # BFS might be the more intuitive solution (min removal), 
    # but didn't figure out how to tackle with BFS
    
    # some facts:
    # 1. if adundant, right parentheses can be removed at any place 
    #    without breaking the validility of the string
    # 2. if adundant, left parentheses can be only removed at the 
    #    places where balance would not be broken
    
    # recursive: bite size -> remove a parenthesis in the string
    
    
    def deduplicate(candidates):
        """remove continuous occurences"""
        prev = -2
        for cand in candidates:
            if cand == prev + 1:
                continue
            prev = cand
            yield cand
    
    # duplicates: "()()))"
    # to resolve this problem: hash the number removed
    
    results = []
    work = []
    visited = set()
    def searchByRemove(s, candidates):
        # case "(()(": 1. "()("; 2 "(()", both can be derived to "()"
        if s in visited: 
            return
        visited.add(s) # better than dedupliate at the end
        for index in deduplicate(candidates):
            search(s[:index] + s[index+1:])
    
    def search(s):
        count = 0 # record (# left - #right) up to now
        leftCandidates = []
        rightCandidates = []
        
        for i, char in enumerate(s):
            if char == '(':
                count += 1
                leftCandidates.append(i)
            elif char == ')':
                count -= 1
                rightCandidates.append(i)
                if count == 0: # left parentheses have been used to match the right ones
                    leftCandidates.clear()
                elif count < 0: # should take action immediately
                    searchByRemove(s, rightCandidates)
                    return
        
        if count > 0: # if left parens are redundant, remove them
            searchByRemove(s, leftCandidates)
            return
        # otherwise: count == 0
        results.append(s)
            
    search(s)
    return list(set(results))
```

