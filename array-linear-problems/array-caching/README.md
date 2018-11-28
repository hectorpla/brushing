# Array: caching

### 152. Maximum Product Subarray

Idea: cache the accumulated product, and the negative accumulated product.

The essence of the idea:

```text
-+--
at any position, should compare with the result

+-+++-++-
should only cache negative acc at position 1 because it is the biggeset 
negative number

+--0++-
should reset the cache when wen meet 0
```



### 560 \(refer to array linear\)



### 713. **Subarray Product Less Than K**

sliding window



### 678. Valid Parenthesis String

maintain the possible left paren count 

```python
def checkValidString(self, s):
    """
    :type s: str
    :rtype: bool
    """
    
    # optimal (linear): follow x-code's idea, maintain two variable
    # !backtracking: the more intuitive approach
    
    mn, mx = 0, 0 # stands for the min and max (superfluous) left parenthesis count
    for char in s:
        # loop invariant 0 <= mn <= mx, ! missed the first part
        # make sense because negative number of count means an impossible route, #right > #left
        if char == '(':
            mn += 1
            mx += 1
        elif char == ')':
            if mx == 0:
                return False
            mn = max(0, mn - 1) # bug: mn -= 1, didn't keep the loop invariant
            mx -= 1
        else:
            mn = max(0, mn - 1)
            mx += 1
        # print(mn, mx)
    return mn == 0
    
    # tests
    # trivial cases: without branches
    # '' -> True
    # '()' -> True
    # '(' -> False
    # ')' -> False
    # 
    # including branches
    # '*' => ''
    # '(*)' -> True, bug: final statement return mn == 0
    # '*()' -> True
    # '()*' -> True
    # '*' => ')'
    # '(*' => True
    # '((*' => False
    # '((*)' => True
    # '*' => '('
    # '*)' => False
```



### 238. Product of Array Except Self

take-away: use acc variable and **assign first**, **multiply second** do good to corner case \(empty\)

order of doing things matter!!!

```python
def productExceptSelf(nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        
        # two-pass: prefix product from the head and from the tail
        n = len(nums)
        result = [1] * n
        
        # tests
        # [] -> []
        # [1] -> [1], not in spec
        # [1, 0] -> [0, 1]
        # [2,3] -> [3,2]
        # [2,3,4] -> [12,8,6]
        acc = 1
        for i, num in enumerate(nums):
            result[i] = acc # assignment
            acc *= num
            
        acc = 1
        for i in range(n - 1, -1, -1):
            result[i] *= acc # bug: originally = instead of *=
            acc *= nums[i]
        return result
```

