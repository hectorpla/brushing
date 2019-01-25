# Sequence generation

### Grey code

### 338. Counting Bits

many solutions: 1. most significant bit, 2. least significant bit, 3. last set bit \(P\(x\)=P\(x&\(x−1\)\)+1\)

### 932. Beautiful Array

odd-even

{% hint style="info" %}
`BA(n) = BA((n+1) // 2) .* 2 .- 1  @  BA(n // 2) .* 2`,    '.\*' means element-wise multiplication like in Matlab
{% endhint %}



### 247. Strobogrammatic Number II

```python
def findStrobogrammatic(self, n):
    # dp[n] <== dp[n-1], dp[n-2]
    
    # n is odd: (A1B, A8B for A, B in dp[n-1]) -redudant + (1A1, 8A8, 6A9, 9A6 in dp[n-2])
    # n is even: (1A1, 8A8, 6A9, 9A6 in dp[n-2])
    
    # missing cases: n = 4, "1001", "8008", "6009", "9006"
    
    pprev = [""]
    if n == 0:
        return pprev
    prev = ["0", "1", "8"]
    
    for i in range(2, n+1):
        # for num starting and ending with 0, filter later
        temp = [x + A + y for A in pprev for (x, y) in [("1", "1"), ("8", "8"), ("6", "9"), ("9", "6")]]
        if i != n: # better logic
            temp.extend("0" + A + "0" for A in pprev)
        pprev, prev = prev, temp
    return prev
```

