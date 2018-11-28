# Permutation

### 46. Permutations

based on swap

```python
def permute(nums):
    # great assumption: distinct values
    # approach: backtracking
    # [a,b,c], swap a and b, next level; a and c, next level...

    # [] -> []
    # [1] -> [[1]]
    # [1,2] -> [[1,2], [2,1]]
    # [1,2,3] -> ...

    # instance 1: [1]
    # instance 2: actual
    results = []
    n = len(nums)
    def swap_and_search(start):
        nonlocal n
        if start >= n: # a. 0; c. 1; e. 2; h. 1; j. 2
            results.append(nums.copy()) # f. [1,2]
        for i in range(start, n):
            nums[start], nums[i] = nums[i], nums[start] # b. (0,0); d. (1,1); g. (0, 1); i. (1,1)
            swap_and_search(start + 1)
            nums[start], nums[i] = nums[i], nums[start] # restore
    swap_and_search(0)
    return results
```

