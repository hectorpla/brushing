# Sliding window

#### 3. Longest Substring Without Repeating Characters

```python
def lengthOfLongestSubstring(s):
    """
    :type s: str
    :rtype: int
    """
    result = 0
    start = 0
    nextStart = {}
    
    for i, char in enumerate(s):
        # tricky here, should make sure the pointer will not go backwards
        start = max(start, nextStart.get(char, start))
        result = max(result, i - start + 1)
        nextStart[char] = i + 1
    return result
```

