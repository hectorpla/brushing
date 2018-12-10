# Knapsack Problem

## Different types

1. unbounded: dependent on the left cell and the upper cell
2. 0-1 knapsack: dependent on the left cell and the upper-left cell

try to do it in DFS manner

## Problems

### 956. Tallest Billboard

{% hint style="info" %}
The most natural way to tackle is a 3-d knapsack problem, `d1` is the rods, `d2` and `d3` are the length sum of the two groups ==&gt; TLE

but can compress the state, d2 and d3 can be **compressed** into only **one** dimension, take the difference! 
{% endhint %}

```python
def tallestBillboard(rods):
    k = len(rods)
    t = sum(rods) // 2
    
    dp = [[[False] * (t + 1) for _ in range(t + 1)] for _ in range(k+1)]
    dp[0][0][0] = True
    
    for k in range(1, k+1):
        rodLength = rods[k-1]
        for i in range(t+1):
            for j in range(t+1):
                dp[k][i][j] = (i >= rodLength and dp[k-1][i-rodLength][j] 
                               or j >= rodLength and dp[k-1][i][j-rodLength]
                               or dp[k-1][i][j])
    result = 0
    for i in range(1, t + 1):
        if dp[k][i][i]:
            result = i
    return result
```

### 494. Target Sum

points to consider:

* constrains: the target &lt;= 1000; what's more, T is bounded by the sum of the int vector
* O\(T\) space filling

### 518. Coin Change 2 \(unbounded\)

```python
def change(amount, coins):
    n = len(coins)
    dp = [[0] * (n + 1) for _ in range(amount + 1)]
    dp[0][:] = [1] * (n + 1)
    
    # tests
    # 1, [1] -> 1
    # 2, [1,2] -> 2
    # 4, [1,2] -> 3
    # 5, [1,2] -> 4, didn't test this case, bug
    
    # fill from top to bottom, left to right
    # instance 5
    #     (0) (2) (5) coin denomination
    # (0)  1   1   1 
    # (1)  1   1   1
    # (2)  1   2   2
    # (3)  1   2   2
    # (4)  1   2   2
    # (5)  1   3   4
    for i in range(1, n + 1): # include coins [1...i] or not
        denomination = coins[i-1]
        for money in range(1, amount + 1):
            prev = money-denomination
            # formulae including coin i or not
            dp[money][i] = dp[money][i-1] + (dp[prev][i] if prev >= 0 else 0) # bug: missing parenthesis
    return dp[amount][n]
```



### 322. Coin Change

```python
def coinChange(coins, amount):
    # dp[money][i] = min(dp[money][i-1], dp[money-coins[i]] + 1)
    n = len(coins)
    dp = [float('inf')] * (amount + 1)

    dp[0] = 0
    # matain one array
    # tests
    # 1, [1] -> 1
    # 2, [1,2] -> 1
    # 3, [1,2] -> 2
    # 4, [1,2] -> 2
    # 5, [1,2,3] -> 2
    for denomination in coins:
        for money in range(denomination, amount + 1):
            dp[money] = min(dp[money], dp[money-denomination] + 1)
    return dp[amount] if dp[amount] < float('inf') else -1 # bug: take care of the impossible cases
```



### 416. Partition Equal Subset Sum

can tackle with BFS

