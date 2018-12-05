# Knapsack Problem

## Different types

1. unbounded: dependent on the left cell and the upper cell
2. 0-1 knapsack: dependent on the left cell and the upper-left cell



## Problems

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

