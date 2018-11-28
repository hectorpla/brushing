# Constant factor matters

### 386. Lexicographical Numbers

line 10: prune ~10x leaf nodes

when n = 5000, 501~4999 -&gt; 501x ~ 4999x

```text
def lexicalOrder(self, n):
    # idea: DFS
    result = []
    # stack depth: log(n)
    def generate(base):
        if base > n:
            return
        result.append(base)
        base *= 10
        if base > n: # prune 10x: leaf nodes
            return
        for i in range(10):
            generate(base + i)

    for i in range(1, 10):
        generate(i)
    return result
```

see top sol: iterative solution, like tree traversal, beautiful solution: [https://leetcode.com/problems/lexicographical-numbers/discuss/86242/Java-O\(n\)-time-O\(1\)-space-iterative-solution-130ms](https://leetcode.com/problems/lexicographical-numbers/discuss/86242/Java-O%28n%29-time-O%281%29-space-iterative-solution-130ms)

