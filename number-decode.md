# Number decode

### 60. Permutation Sequence

unlike binary number, the weights are 1, 2, 4, 8...,  
permutation number, they are 1, 2!, 3!, 4!

```python
def getPermutation(self, n, k):
    def decode(n, k):
        """ mapping number from 0 ~ n!-1 to according unique num sequence
            2, 1 -> [0,0]
            2, 2 -> [1,0]
        """
        k -= 1
        result = []
        for i in range (1, n + 1):
            result.append(k % i)
            k //= i
        return reversed(result)

    # n = 1, k = 1 -> 1
    # n = 2, k = 1 -> 12
    # n = 2, k = 2 -> 21
    # n = 3, k = 2 -> 132
    # n = 3, k = 3 -> 213
    pool = [i for i in range(1, n + 1)]
    result = []
    for index in decode(n, k):
        result.append(pool[index])
        del pool[index]
    return ''.join(map(str, result))
```



### Gray Code



### Decimal number to Binary Number

```python
def decode(num):
    vector = []
    for _ in range(31): # for signed int in java
        vector.append(num & 1)
        num >>= 1
        if num == 0:
            break
    return reversed(vector)
```

