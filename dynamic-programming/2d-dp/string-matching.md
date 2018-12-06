# String matching

### 10. Regular Expression Matching

### 44. Wildcard Matching

take-away: boundary handling

```python
def isMatch(self, s, p):
    m, n = len(s), len(p)
    # unlike 10 regex match, the byte size is whether match the head or not
    # easier than 10 because only consider a char at a time
    
    memo = {} #{(i, j): bool}
    def match(i, j):
        nonlocal m, n
        if i == m and j == n:
            return True
        if i == m:
            return p[j] == '*' and match(i, j + 1)
        if j == n:
            return False
        
        if (i, j) in memo:
            return memo[i,j]
        
        if p[j] == '*':
            # skip * or match
            res = match(i, j + 1) or match(i + 1, j)
        else:
            res = (s[i] == p[j] or p[j] == '?') and match(i + 1, j + 1)
        memo[i,j] = res
        return res
    
    return match(0, 0)

# tests
# "", ""
# "a", "" # j == n
# "", "a" # i == m
# "", "*" # i == m
# "", "**"
# "", "**a"
# "a", "a"
```

### 132. Palindrome Partitioning II

### 115. Distinct Subsequences

a good example of **boundary case** examination

{% hint style="info" %}
`count(i, j):` the counting that matches `s[i:]` with `t[j:]`  
`count(i, j) = count(i+1, j) + (s[i] == t[j] and count(i+1, j+1))`

s = 'aba', t = 'a'

count\(0, 0\) = match\('ba', 'a'\) + match\('ba', ''\) = 1 + 1 \(see boundary below\)

boundaries  
count\(3,1\) = match\(empty, empty\) = 1  
count\(3,0\) = match\(empty, 'a'\) = 0  
count\(3,1\) = count\(2, 1\) = count\(1, 1\) = count\(0, 1\) = match\(non-empty, empty\) = 1
{% endhint %}

 

### 516. Longest Palindromic Subsequence

{% hint style="info" %}
`mx(i, j): the max length of palindromic sebsequence in s[:i+1] + s[j:]  
mx(i, j) = (s[i] == s[j] and 2 + mx(i-1, j+1)) or max(mx(i-1, j), mx(i, j+1))`

`s = "bbbab"`

mx\(0, 4\) = match\("b" + "b"\) = 2

boundaries:

mx\(i, j\) = 0 for i &lt; 0 or j &gt;= n
{% endhint %}

{% hint style="info" %}
Better Definition:

`mx(i, j): the max length in s[i:j+1]`

`mx(i,j) = (s[i] == s[j] and 2 + mx(i+1, j-1)) or max(mx(i+1, j), mx(i, j-1))`
{% endhint %}



### 730. Count Different Palindromic Subsequences

to solve



### 664. Strange Printer

drew search trees

{% hint style="info" %}
minsteps\(i, j\): min steps taken to form the target substring s\[i:j+1\]

after 2hr of try-and-error ==&gt;  Wrong solution \(151/201\)  
`minsteps(i, j) =   
  minsteps(i, k) + minsteps(k+1, j) for k in [i, j] if s[i] != s[j]  
  1 + minsteps(start, end) for all [start, end] windows between which s[k] != s[i] for k in [start, end]`

`counter example: "bacbcaabb"`

The top sol not understand well
{% endhint %}

_Optimization_: !!!! 

